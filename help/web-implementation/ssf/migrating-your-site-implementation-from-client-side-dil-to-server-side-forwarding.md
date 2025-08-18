---
title: Effectuez la migration de l’implémentation Audience Manager de votre site de DIL côté client vers le transfert côté serveur
description: Découvrez comment migrer l’implémentation d’Audience Manager (AAM) de votre site de DIL côté client vers le transfert côté serveur. Ce tutoriel s’applique si vous disposez à la fois d’AAM et d’Adobe Analytics et que vous envoyez des accès de la page à AAM à l’aide du code DIL (Data Integration Library), ainsi que des accès de la page à Adobe Analytics.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# Effectuez la migration de l’implémentation Audience Manager de votre site de DIL côté client vers le transfert côté serveur {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Ce tutoriel s’applique à vous si vous disposez à la fois de Adobe Audience Manager (AAM) et d’Adobe Analytics et que vous envoyez actuellement un accès de la page à AAM à l’aide du code DIL ([!DNL Data Integration Library]), ainsi qu’un accès de la page à Adobe Analytics. Puisque vous disposez de ces deux solutions et qu’elles font toutes deux partie de Adobe Experience Cloud, vous avez la possibilité de suivre la bonne pratique qui consiste à activer le transfert côté serveur, qui permet aux serveurs de collecte de données [!DNL Analytics] de transférer les données d’analyse de site en temps réel vers Audience Manager, au lieu de laisser le code côté client envoyer un accès supplémentaire de la page à AAM. Ce tutoriel vous guide tout au long des étapes nécessaires pour passer de l’ancienne implémentation de DIL côté client à la nouvelle méthode de transfert côté serveur.

## Côté client (DIL) ou côté serveur {#client-side-dil-vs-server-side}

Lorsque vous comparez ces deux méthodes de transfert de données Adobe Analytics dans AAM, il peut s’avérer utile de visualiser d’abord les différences dans l’image suivante :

![côté client à côté serveur](assets/client-side_vs_server-side_aam_implementation.png)

### Implémentation de DIL côté client {#client-side-dil-implementation}

Si vous utilisez cette méthode pour importer des données Adobe Analytics dans AAM, deux accès proviennent de vos pages web : l’un accède à [!DNL Analytics], et l’autre à AAM (après avoir copié les données [!DNL Analytics] sur la page web). [!UICONTROL Segments] sont renvoyées d’AAM vers la page, où elles peuvent être utilisées pour la personnalisation, etc. Il s’agit d’une implémentation héritée qui n’est plus recommandée.

Au-delà du fait que cela ne suit pas les bonnes pratiques, l&#39;utilisation de cette méthode présente les inconvénients suivants :

* Deux accès provenant de la page au lieu d’un seul
* le transfert côté serveur est nécessaire pour le partage en temps réel des audiences AAM vers [!DNL Analytics]. par conséquent, les implémentations côté client n’autorisent pas cette fonctionnalité (et potentiellement d’autres à l’avenir)

Il est recommandé de passer à une méthode de transfert côté serveur pour l’implémentation d’AAM.

### Implémentation du transfert côté serveur {#server-side-forwarding-implementation}

Comme illustré dans l’image ci-dessus, un accès provient de la page web vers Adobe Analytics. [!DNL Analytics] transfère ensuite ces données à AAM en temps réel et les visiteurs sont évalués en termes de caractéristiques et de [!UICONTROL segments] AAM, comme si l’accès provenait directement de la page.

[!UICONTROL Segments] sont renvoyées sur le même accès en temps réel à [!DNL Analytics], qui transfère la réponse sur la page web à des fins de personnalisation, etc.

Le passage au transfert côté serveur ne présente aucun inconvénient temporel. Adobe recommande vivement à toute personne disposant à la fois d’Audience Manager et de [!DNL Analytics] d’utiliser cette méthode d’implémentation.

## Vous avez deux tâches principales {#you-have-two-main-tasks}

Il y a pas mal d&#39;informations sur cette page, et tout est important, bien sûr. Cependant, cela **tout se résume à deux choses principales que vous devez faire** :

1. Remplacez votre code DIL côté client par le code de transfert côté serveur
1. Appuyez sur l’interrupteur dans le [!DNL Analytics] de [!DNL Admin Console] pour démarrer le transfert réel des données (par [!UICONTROL report suite])

Si vous ignorez l’une de ces tâches, le transfert côté serveur ne fonctionnera pas correctement. Des étapes et des données supplémentaires ont été ajoutées à ce document pour vous aider à effectuer correctement ces deux étapes pour votre configuration.

## Options de mise en œuvre {#implementation-options}

Lorsque vous passez du transfert côté client au transfert côté serveur, l’une des tâches que vous devez effectuer consiste à remplacer le code par le nouveau code de transfert côté serveur. Pour ce faire, utilisez l’une des options suivantes :

* Balises Adobe Experience Platform : option d’implémentation recommandée d’Adobe pour les propriétés web. La tâche est facile, comme vous pouvez le constater, car les balises Platform ont réalisé tout le travail nécessaire pour vous.
* Sur la page : vous pouvez également placer le nouveau code SSF directement dans la fonction `doPlugins` de votre fichier `appMeasurement.js`, si vous n’utilisez pas (encore) Adobe Launch
* Autres gestionnaires de balises : ils peuvent être traités comme l’option précédente (Sur la page), car vous placerez toujours le code SSF en `doPlugins`, là où l’autre gestionnaire de balises stocke le code [!DNL AppMeasurement]

Nous examinerons chacun de ces éléments ci-dessous dans la section _Mise à jour du code_.

## Étapes d’implémentation {#implementation-steps}

Les étapes suivantes décrivent la mise en œuvre.

### Étape 0 : prérequis : service Experience Cloud ID (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

La principale condition préalable au transfert côté serveur est de mettre en œuvre le service Experience Cloud ID. Cette opération est plus facile à réaliser si vous utilisez Experience Platform Launch, auquel cas vous installez simplement l’extension ECID, qui fera le reste.

Si vous utilisez un TMS autre qu’Adobe, ou aucun TMS, implémentez ECID pour exécuter **avant** toute autre solution Adobe. Voir la [documentation ECID](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=fr) pour plus d’informations. La seule autre condition préalable concerne les versions de code. Par conséquent, si vous appliquez simplement les versions les plus récentes du code dans les étapes suivantes, tout ira bien.

>[!NOTE]
>
>Veuillez lire l’intégralité de ce document avant l’implémentation. La section « Minutage » ci-dessous contient des informations importantes sur *quand* vous devez implémenter chaque élément, y compris l’ECID (s’il n’est pas encore implémenté).

### Étape 1 : enregistrer les options actuellement utilisées à partir du code DIL {#step-record-currently-used-options-from-dil-code}

Alors que vous vous apprêtez à passer du code DIL côté client au transfert côté serveur, la première étape consiste à identifier tout ce que vous faites avec le code DIL, y compris les paramètres personnalisés et les données envoyés à AAM. Les éléments à noter et à prendre en compte sont les suivants :

* Variables de [!DNL Analytics] normales, à l’aide du module `siteCatalyst.init` DIL : vous n’avez pas à vous inquiéter de celle-ci, car sa tâche consiste simplement à envoyer les variables de [!DNL Analytics] normales, et cela se produit simplement en activant le transfert côté serveur.
* Sous-domaine partenaire - Dans la fonction `DIL.create`, notez le paramètre `partner` . Il s’agit de votre « sous-domaine partenaire », ou parfois de votre « identifiant partenaire », qui est nécessaire lorsque vous placez le nouveau code de transfert côté serveur.
* [!DNL Visitor Service Namespace] - Également appelé « [!DNL Org ID] » ou « [!DNL IMS Org ID] », vous en aurez également besoin lorsque vous configurerez le nouveau code de transfert côté serveur. Prenez-en note.
* containerNSID, uuidCookie et autres options avancées : notez toutes les options avancées supplémentaires que vous utilisez afin de les définir également dans le code de transfert côté serveur.
* Variables de page supplémentaires : si d’autres variables sont envoyées à AAM à partir de la page (en plus des variables de [!DNL Analytics] normales gérées par siteCatalyst.init), vous devrez en prendre note afin qu’elles puissent être envoyées via le transfert côté serveur (alerte du spoiler : via les variables de [!DNL contextData]).

### Étape 2 : mettre à jour le code {#step-updating-the-code}

Dans la section [Options d’implémentation](#implementation-options) (ci-dessus), plusieurs options sont fournies concernant le mode et le lieu d’implémentation du transfert côté serveur. Pour que cette section soit efficace, nous devons la diviser en ces sections (dont deux combinées). Accédez à la méthode de cette section qui décrit le mieux vos besoins.

#### Balises Adobe Experience Platform {#launch-by-adobe}

Regardez la vidéo ci-dessous pour en savoir plus sur le déplacement des options d’implémentation du code DIL côté client vers le transfert côté serveur dans Experience Platform Launch.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### Gestionnaire de balises « Sur la page » ou autre qu’Adobe {#on-the-page-or-non-adobe-tag-manager}

Regardez la vidéo ci-dessous pour en savoir plus sur le déplacement des options d’implémentation du code DIL côté client vers le transfert côté serveur dans le code [!DNL AppMeasurement], résidant dans un fichier ou dans un système de gestion des balises autre qu’Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Étape 3 : activer le transfert (par [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Jusqu’à présent dans ce tutoriel, nous avons passé tout notre temps à basculer le code du code DIL côté client vers le transfert côté serveur. C&#39;est très bien, parce que c&#39;est la partie la plus difficile. Cette section, bien que très simple à lire, est tout aussi importante que la mise à jour du code. Dans cette vidéo, vous allez découvrir comment basculer le bouton qui permet le transfert réel des données d’Analytics vers Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**REMARQUE :** comme indiqué dans la vidéo, n’oubliez pas que l’activation du transfert prendra jusqu’à 4 heures pour être entièrement implémentée sur le serveur principal d’Experience Cloud.

## Timing {#timing}

Pour rappel, il existe deux tâches principales pour passer de DIL côté client au transfert côté serveur :

1. Mise à jour du code
1. Appuyer sur l&#39;interrupteur dans le [!DNL Analytics] [!DNL Admin Console]

Mais la question est, lequel faites-vous en premier ? Cela a-t-il de l&#39;importance ? OK, désolé, c&#39;était deux questions. Mais les réponses sont... ça dépend, et oui, ça *peut* important. C&#39;est comment ça ? Faisons une ventilation. Mais d&#39;abord une question supplémentaire qui peut se poser si vous êtes une grande organisation avec de nombreux sites : Dois-je tout faire en même temps ? Celui-là est un peu plus facile. Non. Vous pouvez le faire morceau par morceau.

### Exploration plus approfondie {#a-little-deeper-dive}

La raison pour laquelle le timing et l’ordre sont importants tient au fonctionnement réel du transfert __ qui peut être résumé dans les quelques faits techniques suivants :

* Si le service Experience Cloud ID (ECID) est implémenté et que le commutateur dans le [!DNL Analytics] [!DNL Admin Console] (« commutateur ») est activé, les données SERONT transférées de [!DNL Analytics] vers AAM, même si vous n’avez pas encore mis à jour le code.
* Si l’ECID n’est pas implémenté, les données ne sont pas transférées, même si l’option est activée et que le code de transfert côté serveur est présent.
* Le code de transfert côté serveur (que ce soit dans les balises Platform ou sur la page) gère réellement la réponse et est nécessaire pour terminer la migration.
* N’oubliez pas que le commutateur de transfert côté serveur est activé par le [!UICONTROL report suite] , mais que le code est géré par la propriété dans les balises Platform, ou par le fichier [!DNL AppMeasurement] si vous n’utilisez pas les balises Platform.

### Bonnes pratiques {#best-practices}

Sur la base de ces détails techniques, voici les recommandations relatives au timing des actions à effectuer et au moment opportun :

#### Si vous n’avez PAS encore implémenté d’ECID {#if-you-do-not-have-ecid-yet-implemented}

1. Appuyez sur le bouton bascule en [!DNL Analytics] pour chaque [!UICONTROL report suite] que vous activerez pour le transfert côté serveur.

   1. Le transfert ne commence pas encore, car vous ne disposez pas d’ECID.

1. Par site, mettez à jour votre code du DIL côté client vers le transfert côté serveur (cela peut être dans les balises Platform) ou sur la page, comme expliqué dans une autre section ci-dessus).

   1. Le transfert s’effectue désormais (car vous avez ajouté l’ECID) et vous devriez également recevoir une réponse JSON appropriée à votre balise [!DNL Analytics] (voir la section Validation et dépannage ci-dessous pour plus d’informations).

#### Si l’ECID est implémenté {#if-you-do-have-ecid-implemented}

1. Préparez et planifiez afin d’être prêt à mettre à jour votre code de DIL vers le transfert côté serveur PER [!UICONTROL report suite] que vous activerez pour le transfert côté serveur :

   1. Appuyez sur le commutateur en [!DNL Analytics] pour activer le transfert côté serveur.

      1. Le transfert COMMENCERA, car l’ECID est activé.

   1. Dès que possible, mettez à jour votre code du DIL côté client vers le transfert côté unique (cela peut être dans les balises Platform ou sur la page, comme expliqué dans une autre section ci-dessus).

      1. Vous devriez recevoir une réponse JSON appropriée à votre balise [!DNL Analytics] (voir la section [ Validation et dépannage ](#validation-and-troubleshooting) ci-dessous pour plus de détails).

>[!NOTE]
>
>Il est important d’effectuer ces deux étapes aussi proches que possible l’une de l’autre, car entre les étapes 1 et 2 ci-dessus, vous aurez une duplication des données entrant dans AAM. En d’autres termes, le transfert unilatéral aura commencé à envoyer des données de [!DNL Analytics] vers AAM, et comme le code DIL est toujours sur la page, il y aura également un accès allant directement de la page vers AAM, doublant ainsi les données. Dès que vous mettez à jour le code de DIL vers le transfert côté serveur, la situation s’améliore.

>[!NOTE]
>
>Si vous préférez une petite incohérence dans les données plutôt qu’une petite duplication des données, vous pouvez changer l’ordre des étapes 1 et 2 ci-dessus. Le déplacement du code de DIL vers le transfert côté serveur arrêtait le flux de données dans AAM jusqu’à ce que vous puissiez activer le transfert côté serveur pour le [!UICONTROL report suite]. En règle générale, les clients préfèrent recevoir un petit doublement de données plutôt que d’ignorer aux visiteurs les caractéristiques et les [!UICONTROL segments].

#### Moment de la migration lorsque vous disposez de nombreux sites et [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Ce thème est brièvement abordé dans les sections précédentes, dans la mesure où la stratégie principale peut être résumée comme suit :

Migrez un site/[!UICONTROL report suite] (ou un groupe de sites/[!UICONTROL report suites]) à la fois.

Cependant, cela peut s’avérer un peu difficile en fonction de quelques scénarios possibles :

* Vous disposez d’un site qui contient plusieurs [!UICONTROL report suites] distinctes
* Vous disposez d’un [!UICONTROL report suite] qui comprend plusieurs sites (comme un [!UICONTROL report suite] global)
* Vous utilisez une propriété de balises Platform pour couvrir plusieurs sites
* Vous disposez de différentes équipes de développement pour différents sites

À cause de ces éléments, cela peut devenir un peu compliqué. Les meilleures choses que je peux suggérer sont :

* Prenez le temps d’élaborer une stratégie de migration vers le transfert côté serveur, en fonction des éléments expliqués ci-dessus
* Étant donné qu’une seule propriété dans les balises Platform (ou un seul fichier [!DNL AppMeasurement]) est généralement mappée à 1 ou 2 [!UICONTROL report suites] distincts, vous serez probablement en mesure d’établir un plan qui fonctionne sur ces groupes distincts un par un, en mettant à jour votre entreprise vers le transfert côté serveur
* Si vous travaillez avec Adobe Consulting, contactez-le au sujet de votre plan de migration, afin qu’il puisse vous aider si nécessaire

## Validation et dépannage {#validation-and-troubleshooting}

La principale façon de vérifier que le transfert côté serveur est opérationnel consiste à examiner la réponse à l’un de vos accès Adobe Analytics provenant de l’application.

Si vous n’effectuez pas de transfert côté serveur des données de [!DNL Analytics] vers Audience Manager, il n’y a vraiment aucune réponse à la balise [!DNL Analytics] (hormis un pixel 2x2). Cependant, si vous effectuez un transfert côté serveur, vous pouvez vérifier certains éléments dans la requête et la réponse [!DNL Analytics] qui vous permettront de savoir que [!DNL Analytics] communique correctement avec Audience Manager, que vous transférez l’accès et que vous obtenez une réponse.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>Méfiez-vous du faux « succès ». S’il existe une réponse et que tout semble fonctionner, assurez-vous que vous disposez de l’objet `stuff` dans la réponse. Si ce n’est pas le cas, un message indiquant `"status":"SUCCESS"` peut s’afficher. Aussi fou que cela puisse paraître, c&#39;est en fait la preuve que cela ne fonctionne PAS correctement.
>
>Si vous le voyez, cela signifie que vous avez terminé la mise à jour du code dans les balises ou [!DNL AppMeasurement] Platform, mais que le transfert dans le [!DNL Analytics] [!DNL Admin Console] n’est pas encore terminé. Dans ce cas, vous devez vérifier que vous avez activé le transfert côté serveur dans le [!DNL Analytics] [!DNL Admin Console] pour votre [!UICONTROL report suite]. Si vous l&#39;avez fait, et que cela n&#39;a pas encore duré 4 heures, soyez patient, car il peut s&#39;écouler beaucoup de temps avant que tous les changements nécessaires soient effectués sur le serveur principal.


![faux succès](assets/falsesuccess.png)

Pour plus d’informations sur le transfert côté serveur, consultez la [documentation](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=fr).
