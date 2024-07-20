---
title: Mise à jour vers Adobe Audience Manager DIL version 8.0 (ou ultérieure)
description: Cet article présente les étapes et recommandations relatives à la mise à jour du code du Data Integration Library (DIL) Adobe Audience Manager (AAM) vers la version 8.0 ou ultérieure. Cela fait référence à la mise en oeuvre du DIL "côté client", et non au transfert côté serveur des données Adobe Analytics. Cela concerne la gestion dynamique des balises, le Launch by Adobe et les mises en oeuvre sans solution de gestionnaire de balises d’Adobe.
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 0%

---

# Mise à jour vers la version 8.0 (ou ultérieure) du DIL de Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Cet article présente les étapes et recommandations pour la mise à jour du code Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) vers la version 8.0 ou ultérieure. Cela fait référence à la mise en oeuvre du DIL &quot;côté client&quot;, et non au transfert côté serveur des données Adobe Analytics. Cela concerne la gestion dynamique des balises, le Launch by Adobe et les mises en oeuvre sans solution de gestionnaire de balises d’Adobe.

## Présentation {#overview}

Le code [!DNL Data Integration Library] (DIL) de l’Audience Manager vous permet de mettre en oeuvre AAM sur votre site Web*. Lors de l’implémentation des versions précédentes de DIL, il n’était pas nécessaire de mettre également en oeuvre le service d’ID Experience Cloud (ECID) d’Adobe (bien que ce fût une très bonne idée). À partir de la version 8.0 de DIL, il existe une dépendance stricte à l’égard de la version 3.3 ou ultérieure de l’ECID. Si vous implémentez DIL 8.0 ou version ultérieure sans ECID 3.3 ou avec une version antérieure, vous obtenez une erreur et elle ne fonctionnera pas. Comme il existe plusieurs façons de mettre en oeuvre AAM, nous avons créé cette page pour vous donner quelques étapes à suivre, ainsi que quelques recommandations. Vous trouverez ci-dessous ces étapes et recommandations ventilées par plateforme/méthode de mise en oeuvre. Pour plus d’informations sur DIL, consultez la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* Comme indiqué dans la description de cette page, cela ne portera que sur les implémentations de DIL &quot;côté client&quot;, utilisées par AAM clients qui ne disposent pas d’Adobe Analytics. Si vous disposez d’Adobe Analytics, vous devez utiliser la méthode de transfert côté serveur pour implémenter AAM. Cette méthode est décrite dans la [documentation](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).

## Duplication et obsolescence des éléments et méthodes {#duplicate-and-deprecated-elements-and-methods}

Dans les versions antérieures de DIL et d’ECID, il existait des méthodes dupliquées (méthodes qui effectuent la même fonction dans DIL et dans ECID), ce qui entraînait une confusion quant à la méthode à utiliser. En règle générale, vous deviez les utiliser et les faire correspondre, et ce message n’était pas bien communiqué à nos clients. À compter de la version 8.0 de DIL, ces méthodes et éléments en double ont été abandonnés dans DIL. Il est recommandé d’utiliser la version ECID.

Par exemple :

* Lors de l’utilisation de [!DNL DIL.create], quelques éléments ont été abandonnés et vous devez utiliser les éléments ECID à la place. Ces éléments sont appelés dans la [[!DNL DIL.create] documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* La méthode de niveau instance [!DNL idSync] a également été abandonnée, comme indiqué dans la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html) de la méthode.

## Synchronisation des identifiants avec un ID de client {#id-syncing-with-a-customer-id}

Dans AAM, vous pouvez synchroniser votre UUID (identifiant utilisateur unique anonyme) sur l’ordinateur avec un ID de client, de sorte que vous puissiez charger des données hors ligne sur ce client et les associer à son comportement en ligne, afin de mieux comprendre vos clients. Dans le passé, cela s&#39;est fait de deux façons :

* Méthode de niveau instance [!DNL idSync]
* L’élément [!DNL declaredId] dans [!DNL DIL.create]

Si vous avez utilisé l’une de ces anciennes méthodes pour vous synchroniser avec un ID de client, il est vivement recommandé de procéder à la mise à jour vers à l’aide de la méthode [!DNL setCustomerIDs], qui fait partie du service ECID. Pour plus d’informations sur [!DNL setCustomerIDs], consultez la [documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html) de la méthode.

**Conseil rapide :** Lors de l’utilisation précédente de l’une des méthodes ci-dessus, vous avez référencé l’AAM [!UICONTROL Data Source] avec l’ID [!UICONTROL Data Source] (AKA le &quot;DPID&quot;). Lors de la mise à jour vers [!DNL setCustomerIDs], vous devrez utiliser &quot;[!UICONTROL Integration Code]&quot; de l&#39;AAM [!UICONTROL Data Source] à la place. Il pointe toujours vers le même [!UICONTROL Data Source], mais n’est qu’un identifiant différent. Ceci est illustré dans la vidéo ci-dessous.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

Les sections suivantes répertorient les étapes et les recommandations pour la mise à jour vers DIL 8.0 en fonction de votre méthode de mise en oeuvre :

## Mise à jour vers DIL 8.0 dans les balises Adobe Experience Platform {#updating-to-dil-in-experience-platform-launch}

Étapes de base de la mise à jour vers DIL 8.0

1. Si vous utilisez un DIL antérieur à la version 8.0, accédez à la configuration du DIL dans l’extension AAM et notez les options avancées que vous utilisez (à utiliser à l’étape suivante).
1. Mettez à jour votre extension AAM vers la version 8.0 ou ultérieure.
1. Vérifiez que l’extension de votre service d’ID Experience Cloud est version 3.3.0 ou ultérieure.
1. Pour toutes les méthodes/éléments obsolètes (comme `disableIDSyncs`) qui se trouvaient dans votre extension AAM antérieure à 8.0 ou dans le code personnalisé pour DIL, activez les méthodes ECID dans l’extension ECID.

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. Publish les modifications.

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Mise à jour vers DIL 8.0 dans Adobe DTM {#updating-to-dil-in-adobe-dtm}

1. Mettez à jour votre outil AAM vers la version 8.0 ou ultérieure. Ce paramètre de version se trouve sous la section &quot;Général&quot; de l’outil AAM.
1. Pour toutes les méthodes/éléments obsolètes (comme `disableIDSyncs`) qui se trouvaient dans le code personnalisé de votre outil d’AAM pré-8.0 pour DIL, prenez note d’eux (afin que vous puissiez les ajouter à l’outil ECID), puis supprimez-les de l’outil personnalisé [!DNL DIL code] dans l’outil AAM.
1. Mettre à jour l’extension de votre service d’ID Experience Cloud vers la version 3.3.0 ou ultérieure
1. Ajoutez les options avancées à l’outil ECID que vous avez supprimé du code personnalisé de l’outil AAM.
1. Publish des modifications

## Mise à jour vers DIL 8.0 sans solution Tag Management Adobe {#additional-resources}

Si vous mettez à jour votre code directement sur la page, vous pouvez simplement remplacer les éléments plus anciens par des éléments plus récents, sauf lorsque vous devez déplacer des méthodes/éléments de DIL à ECID, comme indiqué ci-dessus. Dans ce cas, vous remplacerez simplement l’ancienne méthode/élément de l’emplacement du DIL par la méthode/l’élément ECID de l’emplacement ECID.

Il en va de même pour les gestionnaires de balises non Adobes. Dans tous les cas où vous disposez des anciennes versions de cette solution de gestion des balises, remplacez-la par le nouveau code, comme décrit dans les étapes suivantes.

1. Mettre à jour votre bibliothèque de DIL vers la dernière version (8.0 ou ultérieure) - Vous devrez obtenir le code de DIL le plus récent auprès de l’assistance clientèle d’Adobe Consulting ou d’Adobe, car il n’est actuellement pas disponible dans un emplacement public. Ensuite, remplacez simplement l’ancien code de bibliothèque du DIL par le nouveau code de bibliothèque du DIL et passez à l’étape suivante (ne vous arrêtez pas maintenant ou vous rencontrez des problèmes, ha).
1. Installez [!DNL ECID Service] ou mettez à jour votre version existante vers la version 3.3.0 ou ultérieure. Vous pouvez télécharger la dernière version du service d’ID d’Experience Cloud [ depuis notre page GitHub](https://github.com/Adobe-Marketing-Cloud/id-service/releases). Si vous avez besoin d&#39;aide, consultez la [documentation](https://experienceleague.adobe.com/docs/id-service/using/home.html) ou contactez un consultant en Adobe.

1. Vérifiez que les méthodes ou éléments obsolètes de votre code personnalisé pour DIL sont déplacés vers les méthodes ECID :

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [Documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [Documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [Documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [Documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
