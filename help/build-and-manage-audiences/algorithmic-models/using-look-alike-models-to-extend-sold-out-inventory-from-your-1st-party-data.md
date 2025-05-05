---
title: Utiliser des modèles semblables pour étendre les stocks épuisés à partir de données propriétaires
description: Dans ce tutoriel, nous examinons les étapes à suivre pour configurer et utiliser des modèles semblables, de sorte que vous puissiez créer de nouvelles audiences semblables et les vendre comme extension de votre segment de conversion.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Utiliser des modèles semblables pour étendre les stocks épuisés à partir de données propriétaires {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

Dans ce tutoriel, nous allons passer en revue les étapes à suivre pour configurer et utiliser l’apparence [!UICONTROL Models] afin que vous puissiez créer de nouvelles audiences semblables et les vendre comme extension de votre segment de conversion.

## Détails du cas d’utilisation {#use-case-details}

Vous êtes un éditeur de contenu. Si vous avez déjà épuisé l’inventaire des convertisseurs de votre site, vous pouvez penser que votre opportunité s’arrête là. Saisissez l’apparence d’AAM [!UICONTROL Models]. Grâce à cette fonctionnalité, vous pouvez étendre l’inventaire épuisé et également vendre des audiences de personnes qui n’ont peut-être pas encore effectué de conversions, mais qui ressemblent/agissent comme des personnes qui ont effectué des conversions. Ce segment d’audience se vendrait généralement pour des convertisseurs inférieurs aux convertisseurs réels, mais vous permet néanmoins d’ajouter à votre chiffre d’affaires en fournissant une option d’audience supplémentaire pour les annonceurs qui souhaitent placer des publicités sur votre site. L’avantage supplémentaire de ce cas d’utilisation est qu’il ne vous coûte rien d’exécuter ce modèle sur vos données propriétaires.

Les étapes de ce tutoriel sont les suivantes :

1. Identification/création d’une caractéristique ou d’un segment utilisateur (conversion) idéal
1. Créez un modèle en utilisant cette caractéristique/ce segment de conversion comme élément de base.
1. Sélectionnez la ou les sources de données [!UICONTROL First party] dans le modèle et exécutez le modèle.
1. Créez un [!UICONTROL Algorithmic Trait] à partir des résultats du modèle et ajoutez la caractéristique à un segment.
1. Offrir le segment aux annonceurs intéressés pour étendre les ventes de segments de conversion

## Identification ou création d’une caractéristique ou d’un segment utilisateur (conversion) idéal {#identify-create-an-ideal-user-conversion-trait-or-segment}

Qu’est-ce que vous essayez d’amener les gens à FAIRE sur votre site ? Quel est votre événement de conversion ? Bien sûr, il existe de nombreuses réponses à cette question, selon le type/la verticale de votre site et vos objectifs organisationnels. Dans tous les cas, il est courant dans AAM de créer une caractéristique pour les visiteurs qui répondent à ces critères.

Dans ce cas pratique, cela est déjà supposé, car vous avez épuisé l’inventaire pour les personnes qui sont des convertisseurs. Toutefois, pour les besoins de ce tutoriel, il est bon d’en parler comme référence pour le reste du cas d’utilisation.

En outre, lors de l’utilisation d’événements pour créer des caractéristiques, vous devez garder à l’esprit un piège majeur afin de ne pas collecter plus d’utilisateurs que vous ne devriez dans la caractéristique. Regardez la vidéo suivante pour la grande révélation. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**REMARQUE :** Dans la vidéo ci-dessus, l’exemple que je vous montre suppose que vous disposez d’Adobe Analytics. Ce n&#39;est évidemment pas le cas. Si vous disposez de Google Analytics (GA), nous disposons d’un module que vous pouvez utiliser pour envoyer des données dans AAM (voir la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=fr)). Si votre activité de conversion sur votre site est envoyée à AAM par GA, vous pouvez créer votre caractéristique de conversion à partir de celui-ci. Si vous disposez d’une solution d’analyse différente (ou d’aucune solution d’analyse), vous pouvez toujours envoyer des données à AAM par l’intermédiaire de notre code de DIL et de la fonction `submit`, etc. (voir la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=fr)). Ensuite, créez à nouveau la caractéristique de conversion en fonction des données envoyées lorsque l’activité de conversion est exécutée sur le site.

## Création d’un modèle analogue à partir de données propriétaires {#creating-a-look-alike-model-from-first-party-data}

Au cours de cette étape, nous allons créer un modèle analogue [!UICONTROL First Party]. Cela signifie que non seulement nous allons utiliser une caractéristique/un segment de conversion propriétaire pour notre caractéristique/segment de base (ce serait normal pour la plupart des modèles de toute façon), mais nous allons également uniquement nous intéresser au pool de données propriétaires pour plus de personnes qui ressemblent aux convertisseurs. Nous n’examinerons pas les données de deuxième ou de troisième niveau.

Dans ce cas d’utilisation, c’est important, car nous tentons de créer sur notre site un segment d’utilisateurs qui ressemblent à des convertisseurs mais qui n’ont tout simplement pas encore été convertis, de sorte que nous puissions vendre ce segment analogue aux annonceurs intéressés.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Création d’une caractéristique algorithmique {#creating-an-algorithmic-trait}

Ensuite, nous devrons créer un [!UICONTROL Algorithmic Trait], afin que les résultats du modèle puissent être utilisés. Sans créer de caractéristique, le modèle est inutile. Ainsi, une fois le modèle exécuté, veillez à accéder à la boîte de dialogue de caractéristique et à créer un [!UICONTROL Algorithmic Trait]. La vidéo suivante vous guide et vous montre quelques conseils.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Offrir le [!UICONTROL Algorithmic Segment] aux annonceurs {#offering-the-algorithmic-segment-to-advertisers}

Une fois que vous avez créé un [!UICONTROL Algorithmic Trait], vous pouvez créer un segment dans lequel le placer, de sorte que vous puissiez activer les données (vous ne pouvez pas activer une caractéristique, mais plutôt créer un segment à une caractéristique avec le [!UICONTROL Algorithmic Trait] à l’intérieur, de sorte que vous puissiez activer (utiliser) le segment.

Une fois que vous avez créé un segment de visiteurs propriétaires ayant obtenu de bons résultats dans le modèle analogue (c’est-à-dire qui ressemblent à des convertisseurs mais n’ont pas encore été convertis), vous pouvez proposer ce segment aux annonceurs sur votre site, même après avoir épuisé l’ensemble de votre inventaire des convertisseurs réels sur votre site. Il s’agit d’un excellent moyen d’étendre cette audience et de continuer à afficher des recettes supplémentaires en utilisant la fonctionnalité look-Alike [!UICONTROL Models] en Audience Manager.
