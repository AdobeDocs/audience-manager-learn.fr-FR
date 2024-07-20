---
title: Migration du serveur de suivi vers le transfert côté serveur au niveau de la suite de rapports
description: Découvrez comment activer le transfert côté serveur des données Adobe Analytics pour l’Audience Manager au niveau de la suite de rapports plutôt qu’au niveau du serveur de suivi.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# Migration du serveur de suivi vers le transfert côté serveur au niveau de la suite de rapports {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Cet article et cette vidéo vous montrent comment activer le transfert côté serveur de données [!DNL Analytics] pour Audience Manager à un niveau [!UICONTROL report suite] plutôt qu’à un niveau [!UICONTROL tracking server].

## Introduction {#introduction}

Si vous disposez de Adobe Audience Manager ET Adobe Analytics, vous pouvez mettre en oeuvre le transfert côté serveur des données [!DNL Analytics] vers l’Audience Manager. Cela signifie que, au lieu que votre page envoie deux accès (un à [!DNL Analytics] et un à l’Audience Manager), elle peut envoyer un accès à [!DNL Analytics] et [!DNL Analytics] transmettra ces données à l’Audience Manager.

Si cette fonction est déjà en cours d’exécution et si elle a été activée/mise en oeuvre avant octobre 2017, le transfert côté serveur peut être basé sur votre [!UICONTROL Tracking Server], qui doit être activé par l’assistance clientèle Adobe ou Adobe Consulting. Depuis octobre 2017, vous pouvez configurer vous-même le transfert côté serveur et le faire au niveau de la suite de rapports (transfert par suite de rapports). Il y a des avantages significatifs à cela, qui sont présentés ci-dessous.

## Transfert [!UICONTROL Tracking server] {#tracking-server-forwarding}

Votre [!UICONTROL tracking server] est l’emplacement auquel vous envoyez vos données [!DNL Analytics], ainsi que le domaine sur lequel la demande d’image et le cookie sont écrits. Elle doit être définie dans la gestion dynamique des balises ou [!DNL Experience Platform Launch], ou dans le fichier [!DNL AppMeasurement.js], et ressemblera généralement à ceci : votre site ou votre nom d’entreprise remplacera &quot;mysite&quot; :

`s.trackingServer = "mysite.sc.omtrdc.net";`

Si le transfert côté serveur est configuré pour être transféré au niveau de [!UICONTROL tracking server], tout accès envoyé à cet [!UICONTROL tracking server] (SI le service d’ID Experience Cloud est également activé) sera transféré à l’Audience Manager. Cela devait être activé par le service à la clientèle d’Adobe ou Adobe Consulting. Il s’agit également de ceux qui peuvent le désactiver, APRÈS que vous avez basculé vers le transfert [!UICONTROL report suite], comme décrit ci-dessous.

Si vous ne savez pas si [!DNL tracking server forwarding] est activé pour vous, contactez l’assistance clientèle Adobe ou Adobe Consulting, et ils doivent pouvoir vous le faire savoir.

## Transfert côté serveur au niveau de [!UICONTROL Report-suite] {#report-suite-level-server-side-forwarding}

L’un des plus grands avantages du transfert de [!UICONTROL report suite] depuis le transfert [!UICONTROL tracking server] est que vous pourrez désormais utiliser &quot;Audience Analytics&quot;, qui est la capacité de transférer l’Audience Manager [!UICONTROL segments] vers Adobe Analytics pour une analyse détaillée des segments. Cette fonctionnalité exceptionnelle n’est PAS prise en charge si vous utilisez toujours le transfert [!UICONTROL tracking server] et non le transfert [!UICONTROL report suite]. Pour plus d&#39;informations sur l&#39;Audience Analytics, consultez la [documentation](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Conseil important {#additional-resources}

Comme indiqué dans la vidéo ci-dessus, une fois que tous les [!UICONTROL report suites] sont définis pour le transfert que vous souhaitez transférer vers l’Audience Manager, vous devez contacter l’assistance clientèle d’Adobe ou Adobe Consulting et leur demander de désactiver le transfert [!UICONTROL tracking server]. Il n’est pas urgent que vous le fassiez, car le transfert [!UICONTROL tracking server] et le transfert [!UICONTROL report suite] n’entraînent pas de doublons d’accès. Toutefois, il est recommandé d’activer uniquement le transfert [!UICONTROL report suite].

Si vous laissez le transfert [!UICONTROL tracking server] activé, non seulement peut-il transférer des données provenant de [!UICONTROL report suites] que vous ne souhaitez pas transférer, mais à l’avenir, une fois que vous (et toute personne de votre société) avez oublié que le transfert [!UICONTROL tracking server] est activé, vous pouvez penser que les données ne sont pas transférées pour un [!UICONTROL report suite] spécifique. En effet, elle n’est pas activée au niveau de la suite de rapports, mais les données sont toujours transférées en raison de [!UICONTROL tracking server]. Ensuite, vous perdrez du temps et de l&#39;argent à comprendre pourquoi il transfère et aussi à payer pour les appels AAM serveur auxquels vous ne vous attendiez pas. Il est donc préférable de désactiver le transfert [!UICONTROL tracking server] dès que toutes les [!UICONTROL report suites] sont définies pour transférer ce qui convient aux besoins de votre entreprise.
