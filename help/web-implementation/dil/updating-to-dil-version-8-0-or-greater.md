---
title: Mise à jour vers Adobe Audience Manager DIL version 8.0 (ou ultérieure)
description: 'Cet article vous fournit des étapes et des recommandations pour mettre à jour le code Adobe Audience Manager (AAM) Data Integration Library (DIL) vers la version 8.0 ou une version ultérieure. Il s’agit de l’implémentation DIL « côté client », et non du transfert côté serveur des données Adobe Analytics. Les trois éléments suivants sont concernés : la gestion dynamique des balises, Launch by Adobe et les implémentations sans solution de gestionnaire de balises Adobe.'
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 0%

---

# Mise à jour vers la version 8.0 (ou ultérieure) de DIL de Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Cet article vous fournit des étapes et des recommandations pour mettre à jour le code Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) vers la version 8.0 ou une version ultérieure. Il s’agit de l’implémentation DIL « côté client », et non du transfert côté serveur des données Adobe Analytics. Les trois éléments suivants sont concernés : la gestion dynamique des balises, Launch by Adobe et les implémentations sans solution de gestionnaire de balises Adobe.

## Présentation {#overview}

Le code [!DNL Data Integration Library] (DIL) d’Audience Manager permet d’implémenter AAM sur votre site web*. Lors de l’implémentation de versions précédentes de DIL, il n’était pas nécessaire que le service Experience Cloud ID (ECID) d’Adobe soit également implémenté (bien que ce fût une très bonne idée). À partir de la version 8.0 de DIL, il existe une dépendance stricte à la version 3.3 ou ultérieure d’ECID. Si vous implémentez DIL 8.0 ou une version ultérieure sans ECID 3.3, ou avec une version antérieure, vous obtiendrez une erreur et il ne fonctionnera pas. Comme vous pouvez implémenter AAM de plusieurs façons, nous avons créé cette page pour vous donner quelques étapes à suivre, ainsi que quelques recommandations. Vous trouverez ci-dessous ces étapes et recommandations réparties par plateforme/méthode d’implémentation. Pour plus d’informations sur DIL, consultez la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* Comme indiqué dans la description de cette page, cela ne couvre que les implémentations DIL « côté client », utilisées par les clients AAM qui ne disposent pas d’Adobe Analytics. Si vous disposez d’Adobe Analytics, vous devez utiliser la méthode de transfert côté serveur pour implémenter AAM. Cette méthode est décrite dans la [documentation](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).

## Éléments et méthodes en double et obsolètes {#duplicate-and-deprecated-elements-and-methods}

Dans les versions précédentes de DIL et d’ECID, il existait des méthodes en double (des méthodes qui remplissent la même fonction dans DIL et l’ECID), ce qui entraînait une certaine confusion quant à la méthode à utiliser. En règle générale, vous deviez utiliser les deux et les apparier, et ce message n’a pas été bien communiqué à nos clients. Depuis DIL 8.0, ces méthodes et éléments en double ont été abandonnés dans DIL, et il est recommandé d’utiliser la version ECID.

Par exemple :

* Lors de l’utilisation de [!DNL DIL.create], quelques éléments ont été rendus obsolètes. Vous devez utiliser les éléments ECID à la place. Ces éléments sont décrits dans la [[!DNL DIL.create] documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* La méthode au niveau de l’instance [!DNL idSync] a également été abandonnée, comme indiqué dans la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html) de la méthode .

## Synchronisation des identifiants avec un identifiant client {#id-syncing-with-a-customer-id}

Dans AAM, vous pouvez synchroniser votre UUID (ID utilisateur unique anonyme) sur l’ordinateur avec un ID client, de sorte que vous puissiez charger des données hors ligne sur ce client et les lier à son comportement en ligne, pour mieux comprendre vos clients. Dans le passé, cela se faisait de deux façons :

* La méthode au niveau de l’instance [!DNL idSync]
* L’élément [!DNL declaredId] dans [!DNL DIL.create]

Si vous avez utilisé l’une de ces anciennes méthodes pour effectuer la synchronisation avec un ID client, il est vivement recommandé de mettre à jour vers en utilisant la méthode [!DNL setCustomerIDs], qui fait partie du service ECID. Pour plus d’informations sur [!DNL setCustomerIDs], reportez-vous à la [documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html) de la méthode .

**Conseil rapide :** lors de l’utilisation précédente de l’une des méthodes ci-dessus, vous avez référencé l’[!UICONTROL Data Source] AAM avec l’ID de [!UICONTROL Data Source] (ou « DPID »). Lors de la mise à jour vers [!DNL setCustomerIDs], vous devez utiliser le « [!UICONTROL Data Source] » de l’[!UICONTROL Integration Code] AAM à la place. Il pointe toujours vers le même [!UICONTROL Data Source], mais il s’agit simplement d’un identifiant différent. C’est ce que montre la vidéo ci-dessous.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

Les sections suivantes répertorient les étapes et recommandations de mise à jour vers DIL 8.0 en fonction de votre méthode d’implémentation :

## Mise à jour vers DIL 8.0 dans les balises Adobe Experience Platform {#updating-to-dil-in-experience-platform-launch}

Étapes de base de la mise à jour vers DIL 8.0

1. Si vous utilisez une version de DIL antérieure à la version 8.0, avant d’effectuer la mise à niveau, accédez à la configuration de DIL dans l’extension AAM et notez toute option avancée que vous utilisez (qui sera utilisée lors d’une étape ultérieure).
1. Mettez à jour votre extension AAM vers la version 8.0 ou ultérieure.
1. Vérifiez que votre extension du service Experience Cloud ID est bien la version 3.3.0 ou ultérieure.
1. Pour toutes les méthodes/éléments obsolètes (comme `disableIDSyncs`) qui se trouvaient dans votre extension AAM antérieure à la version 8.0 ou dans le code personnalisé pour DIL, activez les méthodes ECID dans l’extension ECID.

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. Publiez les modifications.

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Mise à jour vers DIL 8.0 dans la gestion dynamique des balises Adobe {#updating-to-dil-in-adobe-dtm}

1. Mettez à jour votre outil AAM vers la version 8.0 ou ultérieure. Ce paramètre de version se trouve dans la section « Général » de l’outil AAM.
1. Pour toutes les méthodes/éléments obsolètes (comme `disableIDSyncs`) qui se trouvaient dans le code personnalisé pour DIL de votre outil AAM antérieur à la version 8.0, notez-les (afin de pouvoir les ajouter à l’outil ECID), puis supprimez-les du [!DNL DIL code] personnalisé dans l’outil AAM.
1. Mettez à jour votre extension du service Experience Cloud ID vers la version 3.3.0 ou ultérieure.
1. Ajoutez les options avancées à l’outil ECID que vous avez supprimé du code personnalisé de l’outil AAM.
1. Publier les modifications

## Mise à jour vers DIL 8.0 sans solution Adobe Tag Management {#additional-resources}

Si vous mettez à jour votre code directement sur la page, vous pouvez simplement remplacer les éléments plus anciens par des éléments plus récents, sauf lorsque vous devez déplacer des méthodes/éléments de DIL vers ECID, comme décrit ci-dessus. Dans ce cas, vous remplacerez simplement l’ancienne méthode/l’ancien élément à l’emplacement du DIL par la méthode/l’ancien élément ECID à l’emplacement de l’ECID.

Il en va de même pour les gestionnaires de balises autres qu’Adobe. Si vous disposez des anciennes versions de cette solution de gestion des balises, remplacez-les par le nouveau code, comme décrit dans les étapes suivantes.

1. Mettez à jour votre bibliothèque DIL vers la dernière version (8.0 ou ultérieure) - Vous devrez obtenir le code DIL le plus récent auprès de l’assistance clientèle d’Adobe Consulting ou d’Adobe, car il n’est actuellement pas disponible dans un emplacement public. Il vous suffit ensuite de remplacer l’ancien code de bibliothèque DIL par le nouveau code de bibliothèque DIL et de passer à l’étape suivante (ne vous arrêtez pas maintenant ou vous rencontrerez des problèmes, ha).
1. Installez [!DNL ECID Service] ou mettez à jour votre version existante vers la version 3.3.0 ou ultérieure. Vous pouvez télécharger la dernière version du service Experience Cloud ID [sur notre page GitHub](https://github.com/Adobe-Marketing-Cloud/id-service/releases). Si vous avez besoin d’aide, consultez la [documentation](https://experienceleague.adobe.com/docs/id-service/using/home.html) ou adressez-vous à un consultant Adobe.

1. Vérifiez que toutes les méthodes ou tous les éléments obsolètes qui se trouvent dans votre code personnalisé pour DIL sont déplacés vers les méthodes ECID :

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [Documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [Documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [Documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [Documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
