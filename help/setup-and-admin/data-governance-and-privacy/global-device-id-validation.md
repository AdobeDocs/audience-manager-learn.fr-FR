---
title: Validation de l’ID global de l’appareil
description: Découvrez la validation des identifiants d’appareil envoyés aux sources de données globales pour un formatage correct et les messages d’erreur lorsque les identifiants ne sont pas correctement formatés.
feature: Data Governance & Privacy
topics: mobile
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 0ff3f123-efb3-4124-bdf9-deac523ef8c9
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 1%

---

# Validation de l’ID global de l’appareil {#global-device-id-validation}

Les identifiants Advertising d’appareils (iDFA, GAID, Roku ID) ont des normes de formatage qui doivent être respectées pour être utilisables dans l’écosystème de la publicité numérique. Aujourd’hui, clients et partenaires peuvent charger des identifiants dans nos sources de données globales dans n’importe quel format sans être avertis si l’identifiant est correctement formaté. Cette fonctionnalité introduit la validation des identifiants d’appareil envoyés aux sources de données globales pour un formatage correct et fournit un message d’erreur lorsque les identifiants sont incorrectement formatés. Nous prendrons en charge la validation des [!DNL iDFA], des [!DNL Google Advertising] et des [!DNL Roku IDs] au lancement.

## Présentation des normes de format {#overview-of-format-standards}

Vous trouverez ci-dessous les pools d’identifiants Advertising d’appareils globaux actuellement reconnus et pris en charge par AAM. Ils sont implémentés sous la forme de [!UICONTROL Data Sources] partagés qui peuvent être utilisés par tout client ou partenaire de données qui travaille avec des données liées aux utilisateurs de ces plateformes.

<table>
  <tr>
   <td>Plateforme </td>
   <td>Identifiant AAM Data Source </td>
   <td>Format d’ID </td>
   <td>PID AAM </td>
   <td>Remarques </td>
  </tr>
  <tr>
   <td>Google Android (GAID)</td>
   <td>20914</td>
   <td>nombres hexadécimaux 32, généralement présentés comme 8-4-4-4-12<em>exemple, 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>Cet identifiant doit être collecté dans une référence de formulaire brute/non hachée/non modifiée - <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS (IDFA)</td>
   <td>20915</td>
   <td>32 nombres hexadécimaux, généralement présentés comme 8-4-4-4-12 <em>exemple, 6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>Cet identifiant doit être collecté dans une référence de formulaire brute/non hachée/non modifiée - <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku (RIDA)</td>
   <td>121963</td>
   <td>32 nombres hexadécimaux, généralement présentés comme 8-4-4-12 <em>exemple,</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>Cet identifiant doit être collecté dans une référence de formulaire brute/non hachée/non modifiée - <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft Advertising ID (MAID)</td>
   <td>389146</td>
   <td>Chaîne numérique Alpha</td>
   <td>14593</td>
   <td>Cet identifiant doit être collecté dans une référence de formulaire brute/non hachée/non modifiée - <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>Exemple de chaîne numérique Alpha, 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>Cet identifiant doit être collecté dans une référence de formulaire brute/non hachée/non modifiée - <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## Définition d’un identifiant Advertising dans l’application {#setting-an-advertising-identifier-in-the-app}

La définition de l’ID de l’annonceur dans l’application est en fait un processus en deux étapes : d’abord la récupération de l’ID de l’annonceur, puis son envoi à Experience Cloud. Vous trouverez ci-dessous des liens pour effectuer ces étapes.

1. Récupérer l’identifiant
   1. [!DNL Apple] d&#39;informations sur le [!DNL advertising ID] se trouvent [ICI](https://developer.apple.com/documentation/adsupport/asidentifiermanager).
   1. Vous trouverez quelques informations sur la définition du [!DNL advertiser ID] pour les développeurs [!DNL Android] ici [ICI](http://android.cn-mirrors.com/google/play-services/id.html).
1. Envoyez-le à Experience Cloud à l’aide de la méthode [!DNL setAdvertisingIdentifier] dans le SDK
   1. Vous trouverez des informations sur l’utilisation de `setAdvertisingIdentifier` dans la [documentation](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) pour [!DNL iOS] et [!DNL Android].

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## Message d’erreur DCS signalant des identifiants incorrects  {#dcs-error-messaging-for-incorrect-ids}

Lorsqu’un identifiant global d’appareil incorrect (IDFA, GAID, etc.) est envoyé en temps réel à Audience Manager, un code d’erreur est renvoyé sur l’accès. Voici un exemple d’erreur renvoyée, car l’ID est envoyé en tant que [!DNL Apple IDFA], qui ne doit contenir que des majuscules, alors qu’un « x » en minuscules figure dans l’ID.

![image d’erreur](assets/image_4_.png)

Consultez la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) pour obtenir la liste des codes d’erreur.

## Intégration d’identifiants globaux d’appareil {#onboarding-global-device-ids}

Outre l’envoi en temps réel des identifiants globaux d’appareil, vous pouvez également « [!DNL onboard] » (charger) des données par rapport aux identifiants. Ce processus est identique à celui qui s’applique aux données d’intégration par rapport à vos ID client (généralement via des paires clé/valeur), mais vous utiliseriez simplement les ID de Source de données appropriés, de sorte que les données soient affectées à l’ID d’appareil global. Vous trouverez de la documentation sur le processus d’intégration dans la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides). Pensez simplement à utiliser l’identifiant global de source de données, en fonction de la plateforme que vous utilisez.

Si des identifiants globaux d’appareil incorrects sont envoyés via le processus d’intégration, les erreurs s’affichent dans la [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting).

Voici un exemple d’erreur générée par ce rapport :

![image d’erreur](assets/image_5_.png)
