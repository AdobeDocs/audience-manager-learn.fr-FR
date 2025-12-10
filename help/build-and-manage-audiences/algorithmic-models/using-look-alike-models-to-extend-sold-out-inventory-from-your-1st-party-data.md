---
title: Utilisez des modèles semblables pour étendre les stocks épuisés à partir de données propriétaires.
description: Dans ce tutoriel, nous examinons les étapes à suivre pour configurer et utiliser des modèles semblables, afin que vous puissiez créer de nouvelles audiences semblables et les vendre en tant qu’extension de votre segment de conversion.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Utilisez des modèles semblables pour étendre les stocks épuisés à partir de données propriétaires. {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

Dans ce tutoriel, nous allons vous présenter les étapes à suivre pour configurer et utiliser des [!UICONTROL Models] semblables, afin que vous puissiez créer de nouvelles audiences semblables et les vendre en tant qu’extension de votre segment de conversion.

## Détails du cas d’utilisation {#use-case-details}

Vous êtes un éditeur de contenu. Si vous avez déjà épuisé les stocks de convertisseurs sur votre site, vous pourriez penser que votre opportunité s&#39;arrête là. Entrez dans la [!UICONTROL Models] analogue d’AAM. Grâce à cette fonctionnalité, vous pouvez augmenter davantage les stocks déjà épuisés et vendre également les audiences de personnes qui n’ont peut-être pas encore convertis, mais qui ressemblent/agissent comme des personnes qui ont converti. Ce segment ciblé se vend généralement moins cher que les convertisseurs réels, mais il vous permet néanmoins d’améliorer vos résultats en fournissant une option d’audience supplémentaire aux annonceurs qui souhaitent placer des annonces sur votre site. L’avantage supplémentaire de ce cas d’utilisation est que l’exécution de ce modèle sur vos données propriétaires ne vous coûte rien.

Les étapes de ce tutoriel sont les suivantes :

1. Identifier/créer une caractéristique ou un segment idéal pour l’utilisateur (conversion)
1. Créez un modèle en utilisant cette caractéristique/ce segment de conversion comme élément de base
1. Choisissez [!UICONTROL First party] ou les sources de données dans le modèle et exécutez le modèle
1. Créer une [!UICONTROL Algorithmic Trait] à partir des résultats du modèle et ajouter la caractéristique à un segment
1. Proposez le segment aux annonceurs intéressés pour étendre les ventes du segment de conversion

## Identifier ou créer une caractéristique ou un segment d’utilisateur idéal (conversion) {#identify-create-an-ideal-user-conversion-trait-or-segment}

Qu&#39;est-ce que vous essayez de faire faire aux gens sur votre site ? Quel est votre événement de conversion ? Bien sûr, il existe de nombreuses réponses différentes à cette question, selon le type de site/le secteur vertical et les objectifs organisationnels. Dans tous les cas, il est courant dans AAM de créer une caractéristique pour les visiteurs qui répondent à ces critères.

Dans ce cas d’utilisation, c’est déjà supposé, car vous avez vendu l’inventaire pour les personnes qui sont des convertisseurs. Toutefois, pour les besoins de ce tutoriel, il est préférable d’en discuter comme référence pour le reste du cas d’utilisation.

En outre, lorsque vous utilisez des événements pour créer des caractéristiques, il y a un piège important à garder à l’esprit, afin de ne pas collecter plus d’utilisateurs que vous ne le devriez dans la caractéristique. Regardez la vidéo suivante pour le grand dévoilement. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**REMARQUE :** dans la vidéo ci-dessus, l’exemple que je montre suppose que vous disposez d’Adobe Analytics. Évidemment, ce n&#39;est peut-être pas le cas. Si vous disposez de Google Analytics (GA), nous disposons d’un module que vous pouvez utiliser pour envoyer des données dans AAM (voir la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=fr)). Si votre activité de conversion sur votre site est envoyée à AAM par GA, vous pouvez alors créer votre caractéristique de conversion à partir de ce module. Si vous disposez d’une autre solution d’analyse (ou d’aucune solution d’analyse), vous pouvez tout de même envoyer des données à AAM via notre code DIL et la fonction `submit`, etc. (voir la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=fr)). Ensuite, créez à nouveau la caractéristique de conversion en fonction des données envoyées lorsque l’activité de conversion est effectuée sur le site.

## Créer un modèle similaire à partir de données propriétaires {#creating-a-look-alike-model-from-first-party-data}

Au cours de cette étape, nous allons créer un modèle analogue [!UICONTROL First Party]. Cela signifie que non seulement nous allons utiliser une caractéristique/un segment de conversion propriétaire pour notre caractéristique/segment de base (ce qui serait normal pour la plupart des modèles de toute façon), mais nous allons également seulement examiner le pool de données propriétaires pour plus de personnes qui ressemblent aux convertisseurs. Nous n&#39;examinerons pas les données de tierces parties.

Dans ce cas d’utilisation, c’est important, car nous essayons de créer un segment d’utilisateurs et d’utilisatrices sur notre site qui ressemblent à des convertisseurs, mais qui ne les ont pas encore convertis, afin de pouvoir vendre ce segment similaire aux annonceurs et annonceuses intéressés.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Création d’une caractéristique algorithmique {#creating-an-algorithmic-trait}

Ensuite, nous devrons créer un [!UICONTROL Algorithmic Trait], afin que les résultats du modèle puissent être utilisés. Sans créer de caractéristique, le modèle est inutile. Ainsi, une fois le modèle exécuté, veillez à accéder à la boîte de dialogue caractéristique et à créer une [!UICONTROL Algorithmic Trait]. La vidéo suivante le décrit en détail et fournit quelques conseils.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Offrir le [!UICONTROL Algorithmic Segment] aux annonceurs {#offering-the-algorithmic-segment-to-advertisers}

Une fois que vous avez créé un [!UICONTROL Algorithmic Trait], vous pouvez créer un segment pour le placer, afin de pouvoir activer les données (vous ne pouvez pas activer une caractéristique, mais plutôt créer un segment à une caractéristique avec le [!UICONTROL Algorithmic Trait] dedans, afin de pouvoir activer (utiliser) le segment.

Une fois que vous avez créé un segment de visiteurs propriétaires qui ont obtenu un score élevé dans le modèle similaire (c’est-à-dire qui ressemblent à des convertisseurs, mais qui ne se sont pas encore convertis), vous pouvez proposer ce segment aux annonceurs sur votre site, même après avoir vendu l’ensemble de votre inventaire de convertisseurs réels sur votre site. Il s’agit d’une excellente façon d’étendre cette audience et de continuer à voir des revenus supplémentaires à l’aide de [!UICONTROL Models] semblables dans Audience Manager.
