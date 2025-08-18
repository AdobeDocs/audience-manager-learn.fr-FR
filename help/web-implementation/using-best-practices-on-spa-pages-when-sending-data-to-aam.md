---
title: Utilisation des bonnes pratiques sur les pages SPA lors de l’envoi de données à AAM
description: Découvrez les bonnes pratiques relatives à l’envoi de données depuis des applications monopages (SPA) vers Adobe Audience Manager (AAM). Cet article porte sur l’utilisation des balises Experience Platform, la méthode d’implémentation recommandée.
feature: Implementation Basics
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: d4874d9f6d7a36bb81ac183eb8b853d893822ae0
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---

# Utilisation des bonnes pratiques sur les pages SPA lors de l’envoi de données à AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

Ce document décrit plusieurs bonnes pratiques pour envoyer des données depuis des applications monopages (SPA) vers Adobe Audience Manager (AAM). Cet article se concentre sur l’utilisation de [!UICONTROL Experience Platform tags], la méthode d’implémentation recommandée.

## Notes initiales

* Les éléments ci-dessous supposent que vous utilisez des balises Platform pour implémenter sur votre site. Les considérations persistent si vous n’utilisez pas les balises Platform, mais vous devez les adapter à votre méthode d’implémentation.
* Toutes les SPA sont différentes. Vous devrez peut-être donc ajuster certains des éléments suivants pour répondre au mieux à vos besoins, mais Adobe souhaite partager certaines bonnes pratiques auxquelles vous devez réfléchir lorsque vous envoyez des données à partir de pages SPA vers Audience Manager.

## Schéma simple de l’utilisation des SPA et d’AAM dans les balises Experience Platform (anciennement Launch){#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![spa pour aam dans les balises](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>Comme indiqué, il s’agit d’un schéma simplifié de la manière dont les pages SPA sont traitées dans une implémentation de Adobe Audience Manager (sans Adobe Analytics) à l’aide de balises Platform. Comme vous pouvez le constater, elle est assez simple, la grande décision étant la manière dont vous allez communiquer un changement d’affichage (ou une action) aux balises Platform.

## Déclenchement de balises à partir de la page SPA {#triggering-launch-from-the-spa-page}

Deux des méthodes les plus courantes pour déclencher une règle dans les balises Platform (et donc envoyer des données dans Audience Manager) sont les suivantes :

* Définition d’événements personnalisés JavaScript (voir exemple [ICI](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) avec Adobe Analytics)
* Utilisation d’un [!UICONTROL Direct Call Rule]

Dans cet exemple d’Audience Manager, vous utilisez un [!UICONTROL Direct Call rule] dans les balises Platform pour déclencher l’accès entrant dans Audience Manager. Comme vous le verrez dans les sections suivantes, cela devient utile en définissant la [!UICONTROL Data Layer] sur une nouvelle valeur, afin qu’elle puisse être récupérée par le [!UICONTROL Data Element] dans les balises Platform.

## Page de démonstration {#demo-page}

Voici une petite page qui illustre la modification d’une valeur dans la couche de données et son envoi dans Audience Manager, comme vous pouvez le faire sur une page SPA. Cette fonctionnalité peut être modélisée pour les modifications plus élaborées nécessaires. Vous pouvez trouver cette page de démonstration [ICI](https://aam.enablementadobe.com/SPA-Launch.html).

## Définition de la couche de données {#setting-the-data-layer}

Comme mentionné, lorsque du nouveau contenu est chargé sur la page ou qu’une personne effectue une action sur le site, la couche de données doit être définie dynamiquement dans l’en-tête de la page AVANT l’appel des balises Platform et l’exécution de la [!UICONTROL rules], de sorte que les balises Platform puissent sélectionner les nouvelles valeurs de la couche de données et les transmettre dans Audience Manager.

Si vous accédez au site de démonstration répertorié ci-dessus et consultez la source de la page, vous verrez :

* La couche de données se trouve dans l’en-tête de la page, avant l’appel aux balises Platform
* Le JavaScript dans le lien SPA simulé modifie la [!UICONTROL Data Layer], puis appelle les balises Platform (l’appel `_satellite.track()`). Si vous utilisiez des événements personnalisés JavaScript au lieu de cette [!UICONTROL Direct Call Rule], la leçon est la même. Modifiez d’abord le [!DNL data layer], puis appelez les balises Platform.

>[!VIDEO](https://video.tv.adobe.com/v/38107/?quality=12&captions=fre_fr)

## Ressources supplémentaires {#additional-resources}

* [Discussion sur les SPA sur les forums Adobe](https://forums.adobe.com/thread/2451022)
* [Sites d’architecture de référence pour montrer comment implémenter une SPA dans les balises Platform](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Utilisation des bonnes pratiques lors du suivi des SPA dans Adobe Analytics](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [Site de démonstration utilisé pour cet article](https://aam.enablementadobe.com/SPA-Launch.html)
