---
title: Augmentation du retour sur dépenses publicitaires à l’aide de modèles algorithmiques (semblables)
description: La véritable puissance de la modélisation look-alike d’Audience Manager réside dans le fait que vous cherchez à étendre votre audience de base par rapport à un tout nouvel ensemble d’utilisateurs de qualité provenant de sources de données tierces et secondaires. Dans ce tutoriel, découvrez les étapes de création d’un modèle à partir de ces données.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 0%

---

# Augmentation du retour sur dépenses publicitaires à l’aide de modèles algorithmiques (semblables) dans Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

La véritable puissance des [!UICONTROL Modeling] semblables d’Audience Manager réside dans le fait que vous cherchez à étendre votre audience de base par rapport à un tout nouvel ensemble d’utilisateurs de qualité provenant de sources de données tierces et secondaires. Dans ce tutoriel, découvrez les étapes nécessaires à la création d’un modèle à partir de ces données.

## Activer les flux de données secondaires ou tiers à partir d’Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Pour utiliser des données tierces et secondaires dans un modèle similaire, nous devons d’abord activer ces données dans votre interface Audience Manager. Adobe dispose d’un grand nombre de fournisseurs de données secondaires et tiers parmi lesquels vous pouvez choisir. Ils sont disponibles pour vous dans une interface en libre-service dans AAM, via l’Audience Marketplace. Accédez à l’Audience Marketplace et parcourez les possibilités. La vidéo suivante vous explique comment procéder, notamment en activant les flux gratuits « essayer avant d’acheter », afin que vous puissiez vous verrouiller sur les données qui seront les plus utiles à votre organisation avant de vous engager à respecter les tarifs du fournisseur de données.

En outre, pour vous aider à rechercher et à décider quel fournisseur de données utiliser, une excellente ressource est la [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identifier ou créer une caractéristique ou un segment d’utilisateur idéal (conversion) {#identify-create-an-ideal-user-conversion-trait-or-segment}

Qu&#39;est-ce que vous essayez de faire faire aux gens sur votre site ? Quel est votre événement de conversion ? Bien sûr, il existe de nombreuses réponses différentes à cette question, selon le type de site/le secteur vertical et les objectifs organisationnels. Dans tous les cas, il est courant dans AAM de créer une caractéristique pour les visiteurs qui répondent à ces critères.

Dans la vidéo ci-dessous, je vous montrerai comment créer une caractéristique de conversion, que vous souhaiterez avoir en place au fur et à mesure que vous poursuivrez ce tutoriel et créerez un modèle similaire.

En outre, lorsque vous utilisez des événements Adobe Analytics pour créer des caractéristiques, il y a un piège important à garder à l’esprit, afin de ne pas collecter plus d’utilisateurs que vous ne le devriez dans la caractéristique. Regardez la vidéo suivante pour le grand dévoilement. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**REMARQUE :** dans la vidéo ci-dessus, l’exemple que je montre suppose que vous disposez d’Adobe Analytics. Évidemment, ce n&#39;est peut-être pas le cas. Si vous disposez de Google Analytics (GA), nous disposons d’un module que vous pouvez utiliser pour envoyer des données dans AAM (voir la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)). Si votre activité de conversion sur votre site est envoyée à AAM par GA, vous pouvez alors créer votre caractéristique de conversion à partir de ce module. Si vous disposez d’une autre solution d’analyse (ou d’aucune solution d’analyse), vous pouvez tout de même envoyer des données à AAM via notre code DIL et la fonction `submit`, etc. (voir la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)). Créez ensuite la caractéristique de conversion en fonction des données envoyées lorsque l’activité de conversion est effectuée sur le site.

## Créer un modèle analogue à partir de données tierces ou secondaires {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Après avoir suivi les étapes ci-dessus, nous sommes maintenant prêts à créer un modèle algorithmique (similaire). Pendant la configuration du modèle, nous utiliserons la caractéristique de conversion comme caractéristique de base (les visiteurs clés que nous voulons dupliquer) et nous utiliserons le flux de données tiers activé comme pool de personnes à extraire.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Une bonne pratique importante {#an-important-best-practice}

Lors de la création du modèle algorithmique dans Audience Manager, nous souhaitons évidemment que le modèle soit le plus efficace possible. Comme le modèle prend en compte toutes les caractéristiques dont font partie les membres de votre caractéristique/segment de base, il n’aide pas le modèle si TOUTES les personnes se trouvent dans une caractéristique/segment. Par conséquent, si vous disposez de caractéristiques super génériques (comme toutes les personnes qui se sont rendues sur votre site ou toutes celles qui ont reçu une annonce de votre part, etc.), assurez-vous que la source de données à laquelle elles appartiennent n’est PAS incluse dans les sources de données de votre modèle. Dans le cas d’utilisation de cet article, il est peu probable que vous le fassiez, car nous nous concentrons sur l’examen des données tierces pour nos nouveaux semblables, mais il est utile de le mentionner quand même et il s’applique à TOUS vos modèles algorithmiques.

## Création d’un [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

Ensuite, nous devrons créer un [!UICONTROL Algorithmic Trait], afin que les résultats du modèle puissent être utilisés. Sans créer de caractéristique, le modèle est inutile. Ainsi, une fois le modèle exécuté, veillez à accéder à la boîte de dialogue caractéristique et à créer une [!UICONTROL Algorithmic Trait]. La vidéo suivante le parcourt et présente quelques conseils.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Créer un segment à partir des données de modèle et l’envoyer aux DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Une fois que vous avez créé un [!UICONTROL Algorithmic Trait], vous pouvez créer un segment pour le placer, afin de pouvoir activer les données (vous ne pouvez pas activer une caractéristique, mais plutôt créer un segment à une caractéristique avec le [!UICONTROL Algorithmic Trait] dedans, afin de pouvoir activer (utiliser) le segment).

Une fois que vous avez créé un segment à partir de cette caractéristique algorithmique, vous avez une audience de clients potentiels qui ressemblent à des personnes qui ont déjà effectué une conversion sur votre site. Vous pouvez désormais mapper ce segment à l’une de vos destinations DSP dans Audience Manager. Vous pourrez cibler votre marketing sur ces semblables, qui sont plus susceptibles de convertir des publicités sur votre site que le public normal, ce qui augmentera votre retour sur dépenses publicitaires.
