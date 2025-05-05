---
title: Augmenter le retour sur investissement en utilisant des modèles algorithmiques (analogue)
description: La puissance réelle de la modélisation look-alike d’Audience Manager réside dans le fait que vous cherchez à étendre votre audience de base par rapport à un nouvel ensemble d’utilisateurs de qualité provenant de sources de données tierces et de deuxième niveau. Dans ce tutoriel, découvrez les étapes de création d’un modèle à partir de ces données.
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

# Augmentation du retour sur investissement en utilisant des modèles algorithmiques (analogue) dans l’Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

La puissance réelle de l’ [!UICONTROL Modeling] look-alike de l’Audience Manager réside dans votre volonté d’étendre votre audience de base par rapport à un nouvel ensemble d’utilisateurs de qualité provenant de sources de données tierces et de deuxième niveau. Dans ce tutoriel, découvrez les étapes nécessaires à la création d’un modèle à partir de ces données.

## Activation des flux de données de deuxième ou de troisième niveau à partir de l’Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Pour utiliser des données de deuxième et de troisième niveau dans un modèle analogue, nous devons d’abord activer ces données dans votre interface d’Audience Manager. Adobe comporte un grand nombre de fournisseurs de données tiers et de deuxième niveau parmi lesquels vous pouvez choisir. Elles sont disponibles pour vous dans une interface en libre-service dans AAM, via l’Audience Marketplace. Accédez à l’Audience Marketplace et parcourez les possibilités. La vidéo suivante vous explique comment procéder, notamment comment activer les flux &quot;Essayer avant d’acheter&quot; gratuits, de sorte que vous puissiez verrouiller les données qui seront les plus utiles à votre organisation avant de vous engager dans la tarification du fournisseur de données.

En outre, pour vous aider à rechercher et à décider quel fournisseur de données utiliser, une ressource exceptionnelle est [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identification ou création d’une caractéristique ou d’un segment utilisateur (conversion) idéal {#identify-create-an-ideal-user-conversion-trait-or-segment}

Qu’est-ce que vous essayez d’amener les gens à FAIRE sur votre site ? Quel est votre événement de conversion ? Bien sûr, il existe de nombreuses réponses à cette question, selon le type/la verticale de votre site et vos objectifs organisationnels. Dans tous les cas, il est courant dans AAM de créer une caractéristique pour les visiteurs qui répondent à ces critères.

Dans la vidéo ci-dessous, je vous montrerai comment créer une caractéristique de conversion que vous souhaitez mettre en place au fur et à mesure que vous poursuivez ce tutoriel et créez un modèle analogue.

En outre, lorsque vous utilisez des événements Adobe Analytics pour créer des caractéristiques, vous devez garder à l’esprit un piège majeur afin de ne pas collecter plus d’utilisateurs que vous ne devriez dans la caractéristique. Regardez la vidéo suivante pour la grande révélation. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**REMARQUE :** Dans la vidéo ci-dessus, l’exemple que je vous montre suppose que vous disposez d’Adobe Analytics. Ce n&#39;est évidemment pas le cas. Si vous disposez de Google Analytics (GA), nous disposons d’un module que vous pouvez utiliser pour envoyer des données dans AAM (voir la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=fr)). Si votre activité de conversion sur votre site est envoyée à AAM par GA, vous pouvez créer votre caractéristique de conversion à partir de celui-ci. Si vous disposez d’une solution d’analyse différente (ou d’aucune solution d’analyse), vous pouvez toujours envoyer des données à AAM par l’intermédiaire de notre code de DIL et de la fonction `submit`, etc. (voir la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=fr)). Créez ensuite la caractéristique de conversion en fonction des données envoyées lorsque l’activité de conversion est exécutée sur le site.

## Création d’un modèle analogue à partir de données de deuxième ou de troisième niveau {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Après avoir suivi les étapes ci-dessus, nous sommes maintenant prêts à créer un modèle algorithmique (analogue). À mesure que nous configurons le modèle, nous utiliserons la caractéristique de conversion comme caractéristique de base (visiteurs clés que nous voulons dupliquer) et nous utiliserons le flux de données tiers activé comme groupe de personnes à partir duquel nous pouvons extraire.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Une bonne pratique importante {#an-important-best-practice}

Lors de la création du modèle algorithmique en Audience Manager, nous voulons évidemment que le modèle soit aussi efficace que possible. Comme le modèle prend en compte toutes les caractéristiques dont font partie les membres de votre caractéristique/segment de base, il n’aide pas le modèle si TOUTES les personnes se trouvent dans une caractéristique/un segment. Par conséquent, si vous disposez de caractéristiques super génériques (comme toutes les personnes qui sont allées sur votre site, ou toutes celles qui ont reçu une annonce de votre part, etc.), assurez-vous que la source de données à laquelle elles appartiennent n’est PAS incluse dans les sources de données de votre modèle. Dans le cas d’utilisation de cet article, il est peu probable que vous le fassiez, car nous nous concentrons sur les données tierces pour nos nouveaux alias d’aspect, mais cela vaut la peine d’être mentionné et cela s’applique à TOUS vos modèles algorithmiques.

## Créer un [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

Ensuite, nous devrons créer un [!UICONTROL Algorithmic Trait], afin que les résultats du modèle puissent être utilisés. Sans créer de caractéristique, le modèle est inutile. Ainsi, une fois le modèle exécuté, veillez à accéder à la boîte de dialogue de caractéristiques et à créer un [!UICONTROL Algorithmic Trait]. La vidéo suivante vous guide et vous montre quelques conseils.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Créez un segment à partir des données de modèle et envoyez-le à DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Une fois que vous avez créé un [!UICONTROL Algorithmic Trait], vous pouvez créer un segment dans lequel le placer, de sorte que vous puissiez activer les données (vous ne pouvez pas activer une caractéristique, mais plutôt créer un segment à une caractéristique avec le [!UICONTROL Algorithmic Trait] à l’intérieur, de sorte que vous puissiez activer (utiliser) le segment).

Une fois que vous avez créé un segment à partir de cette caractéristique algorithmique, vous disposez d’une audience de clients potentiels qui ressemblent à des personnes qui ont déjà converti sur votre site. Vous pouvez maintenant mapper ce segment à n’importe quelle destination DSP dans Audience Manager. Vous pourrez cibler votre marketing sur ces pseudonymes, qui sont plus susceptibles de convertir votre visite sur votre site que le public normal, ce qui augmentera votre retour sur dépenses publicitaires.
