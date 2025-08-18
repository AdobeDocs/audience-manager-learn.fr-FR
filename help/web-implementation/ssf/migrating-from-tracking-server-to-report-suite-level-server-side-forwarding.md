---
title: Migration du serveur de suivi vers le transfert côté serveur au niveau de la suite de rapports
description: Découvrez comment activer le transfert côté serveur des données Adobe Analytics vers Audience Manager au niveau de la suite de rapports plutôt qu’au niveau du serveur de suivi.
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

Cet article et cette vidéo vous montrent comment activer le transfert côté serveur des données [!DNL Analytics] vers Audience Manager à un niveau [!UICONTROL report suite] plutôt qu’à un niveau [!UICONTROL tracking server].

## Introduction {#introduction}

Si vous disposez de Adobe Audience Manager ET d’Adobe Analytics, vous pouvez implémenter le transfert côté serveur des données [!DNL Analytics] vers Audience Manager. Cela signifie qu’au lieu d’envoyer deux accès à votre page (un à [!DNL Analytics] et un à Audience Manager), la page peut envoyer un accès à [!DNL Analytics] et [!DNL Analytics] transmettra ces données à Audience Manager.

Si vous l’avez déjà mis en service et si vous l’avez activé/implémenté avant octobre 2017, le transfert côté serveur peut être basé sur votre [!UICONTROL Tracking Server], qui doit être activé par l’assistance clientèle d’Adobe ou Adobe Consulting. Depuis octobre 2017, vous pouvez désormais configurer vous-même le transfert côté serveur, et ce au niveau d’une suite de rapports (transfert par suite de rapports). Il y a des avantages significatifs à cela, qui est discuté ci-dessous.

## transfert de [!UICONTROL Tracking server] {#tracking-server-forwarding}

Votre [!UICONTROL tracking server] correspond à l’emplacement vers lequel vous envoyez vos données [!DNL Analytics], ainsi qu’au domaine sur lequel la demande d’image et le cookie sont écrits. Elle doit être définie dans DTM ou [!DNL Experience Platform Launch], ou dans le fichier [!DNL AppMeasurement.js]. En règle générale, elle se présente comme suit, le nom de votre site ou de votre entreprise remplaçant « mysite » :

`s.trackingServer = "mysite.sc.omtrdc.net";`

Si le transfert côté serveur est configuré pour effectuer un transfert au niveau du [!UICONTROL tracking server], tout accès envoyé à ce [!UICONTROL tracking server] (SI le service Experience Cloud ID est également activé) sera transféré à Audience Manager. Cette fonctionnalité devait être activée par l’assistance clientèle d’Adobe ou Adobe Consulting. Ce sont également eux qui peuvent le désactiver, APRÈS avoir basculé vers le transfert [!UICONTROL report suite], comme décrit ci-dessous.

Si vous ne savez pas si [!DNL tracking server forwarding] est activé pour vous, contactez l’assistance clientèle d’Adobe ou d’Adobe Consulting. Ils devraient être en mesure de vous le faire savoir.

## Transfert côté serveur au niveau [!UICONTROL Report-suite] {#report-suite-level-server-side-forwarding}

L’un des plus grands avantages du passage au transfert de [!UICONTROL report suite] à partir du transfert de [!UICONTROL tracking server] est que vous pourrez désormais utiliser « Audience Analytics », c’est-à-dire la possibilité de transférer les [!UICONTROL segments] Audience Manager vers Adobe Analytics pour une analyse de segment détaillée. Cette fonctionnalité n’est PAS prise en charge si vous effectuez toujours le transfert de [!UICONTROL tracking server] et non le transfert de [!UICONTROL report suite]. Pour plus d’informations sur Audience Analytics, consultez la [documentation](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=fr).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Conseil important {#additional-resources}

Comme indiqué dans la vidéo ci-dessus, une fois que toutes les [!UICONTROL report suites] à transférer vers Audience Manager sont configurées, vous devez contacter l’assistance clientèle d’Adobe ou Adobe Consulting et leur demander de désactiver le transfert de [!UICONTROL tracking server]. Ce n’est pas une urgence pour vous de le faire, car le transfert de [!UICONTROL tracking server] et le transfert de [!UICONTROL report suite] ne génèrent pas d’accès en double. Toutefois, il est recommandé de n’activer que le transfert de [!UICONTROL report suite].

Si vous laissez le transfert de [!UICONTROL tracking server] activé, il peut non seulement transférer les données des [!UICONTROL report suites] que vous ne souhaitez pas transférer, mais à l’avenir, après que vous (et tous les membres de votre entreprise) ayez oublié que le transfert de [!UICONTROL tracking server] est activé, vous pouvez penser que les données ne sont pas transférées pour un [!UICONTROL report suite] spécifique. En effet, elle n’est pas activée au niveau de la suite de rapports, mais les données sont toujours transférées en raison de la [!UICONTROL tracking server]. Vous perdrez alors du temps et de l’argent à comprendre pourquoi il transfère et à payer les appels au serveur AAM auxquels vous ne vous attendiez pas. Il est donc judicieux de désactiver le transfert de [!UICONTROL tracking server] dès que vous disposez de toutes les [!UICONTROL report suites] à transférer qui correspondent aux besoins de votre entreprise.
