---
title: Prise en charge de l’IAB TCF 2.2
description: Découvrez le module externe Audience Manager pour IAB TCF et son fonctionnement avec l’objet d’accord préalable Adobe et votre fournisseur de gestion du consentement (CMP).
feature: Data Governance & Privacy
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: f9708e705d95b43084ff11e342dc54ff11d6326c
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 0%

---

# Prise en charge de l’IAB TCF 2.2 dans Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe vous offre les moyens de gérer et de communiquer les choix de confidentialité de vos utilisateurs par le biais de la fonctionnalité d’accord préalable et par le biais du module externe Audience Manager pour la prise en charge de IAB Transparency and Consent Framework 2.2 (TCF 2.2). Cet article fonctionne avec la documentation pour vous aider à comprendre le module externe Audience Manager pour IAB TCF et comment il fonctionne avec l’objet Opt-in d’Adobe et votre fournisseur de gestion du consentement (CMP). Pour en savoir plus sur l&#39;IAB, veuillez consulter son site Web à l&#39;adresse [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## Première étape : comprendre le processus d’opt-in à l’Experience Cloud ID {#first-step-understand-ecid-s-opt-in}

Pour comprendre comment utiliser le IAB TCF, vous devez d’abord connaître [!DNL Opt-in] fonctionnalité, qui fait partie de la bibliothèque du service Experience Cloud ID (ECID). Si vous ne connaissez pas le fonctionnement de l’opt-in, consultez d’abord [cet article utile](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html?lang=fr). Vous devez également consulter la [documentation](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=fr) de souscription. Une fois que vous avez parcouru ces ressources, revenez sur cette page et continuez.

## Module externe Audience Manager pour IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

Maintenant que vous avez au moins une compréhension de base du fonctionnement du service Opt-in, Audience Manager peut y ajouter un calque [!DNL IAB Transparency and Consent Framework (TCF)] la prise en charge, qui est effectuée via un plug-in dans l’objet Opt-in.

Le module externe Audience Manager pour IAB TCF étend les fonctionnalités d’Opt-in et permet aux clients AAM d’évaluer, d’honorer et de transférer les choix de confidentialité des utilisateurs aux partenaires en aval conformément au module IAB TCF. Il fournit une norme que les contrôleurs de données (c’est-à-dire vous en tant que client Adobe) et les fournisseurs (DMP, DSP, SSP, serveurs de publicités, etc.) peuvent utiliser pour comprendre le consentement dans le paysage du consentement.

## Activer IAB TCF {#enabling-iab-tcf}

L’activation du module externe Audience Manager pour IAB TCF est facile si vous utilisez Adobe Experience Platform Launch, car il s’agit d’une simple case à cocher, comme illustré dans la courte vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

Si vous n’utilisez pas Launch, vous pouvez également utiliser `isIabContext=true` pour l’activer lorsque vous instanciez le visiteur Experience Cloud. Cela lance le flux IAB TCF, c’est-à-dire qu’il ajoute une autre étape à la collecte de consentement, en utilisant l’IAB TCF pour interroger la chaîne IAB TC et la renvoie à Opt-in, qui à son tour communique avec les solutions Experience Cloud.

## Chaîne IAB TC {#iab-tcf-consent-string}

L’une des normes fournies par IAB est une « chaîne de consentement » (également appelée « DaisyBit »), c’est-à-dire deux listes réunies :

1. Objectifs : **Que fait** le consentement donné ?
1. Fournisseurs : **À qui** donne-t-on son consentement ?

### Objectifs {#purposes}

Avec IAB TCF 2.2, il existe dix « fins » pour lesquelles collecter le consentement (ce que les fournisseurs peuvent faire avec les données du visiteur). Adobe Audience Manager ne nécessite pas le consentement des dix, mais uniquement aux fins suivantes, en plus du consentement du fournisseur :

* **Objectif 1 :** Stocker et/ou accéder à des informations sur un appareil ;
* **Objectif 10 :** développer et améliorer les produits ;
* **Objectif spécial 1 :** assurer la sécurité, prévenir la fraude et déboguer.

Il s’agit de la première partie de la chaîne de l’IAB TC qui est simplement enregistrée comme 1 et 0, ce qui détermine si cet objectif/cette activité est approuvé ou non.

>[!NOTE]
>
>Conformément aux réglementations de l’IAB, l’objectif spécifique 1 (garantir la sécurité, prévenir les fraudes et procéder au débogage) est toujours accepté et les utilisateurs ne peuvent pas s’y opposer.

### Fournisseurs {#vendors}

Une autre partie de la chaîne IAB TC est une longue liste de plusieurs centaines de fournisseurs, de sorte que les visiteurs puissent se voir présenter une liste des fournisseurs applicables qui ont des balises sur le site et peuvent choisir les fournisseurs à utiliser. Les fournisseurs conservent leur place sur la liste. Par exemple, le numéro de fournisseur Adobe Audience Manager de cette liste est 565. Si ce nombre de la liste comporte un « 1 », Audience Manager peut effectuer les tâches approuvées en commençant au début de la liste. Si la zone d’AAM comporte un « 0 », elle ne peut rien faire avec les données.

**Pour qu’Audience Manager fournisse une interface utilisateur permettant aux clients d’utiliser le IAB TCF afin de choisir ces objectifs et fournisseurs, ou d’approuver/désapprouver toute activité, vous devez utiliser un partenaire CMP enregistré auprès de l’IAB TCF ou en créer un qui prend en charge l’IAB TCF et qui est enregistré auprès de l’IAB TCF.**

## Opt-in : traduction entre les applications IAB et Adobe {#opt-in-translating-between-iab-and-adobe-solutions}

L’un des avantages de l’utilisation du IAB TCF est que les objectifs standard répertoriés ci-dessus donnent probablement à l’utilisateur final une meilleure idée de ce qu’il approuve qu’une liste de solutions Adobe. Les utilisateurs finaux peuvent ne pas savoir ce que signifie « approuver » Audience Manager ou [!DNL Target], mais « stocker et/ou accéder aux informations sur un appareil » ou « développer et améliorer des produits » est probablement plus facile à comprendre et à accepter.

Pour qu’Audience Manager soit approuvé (par ex. Pour que la traduction des fonctions IAB pour Opt-in puisse donner un « oui » à AAM, les fonctions 1 et 10, telles qu’énumérées ci-dessus, doivent recevoir le consentement de l’utilisateur final. Si l’un de ces éléments n’est pas approuvé, ou si un revendeur n’est pas approuvé, AAM n’exécute pas de pixels activés ou ne définit pas de cookies. Il est également bon de savoir que de nombreux clients choisissent simplement de fournir à l’utilisateur final une interface utilisateur « tout ou rien », ce qui autorise ou non l’utilisation d’Audience Manager (et des autres solutions Experience Cloud).

La [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=fr) contient de précieuses informations sur la manière dont le flux du module externe Audience Manager pour IAB TCF s’applique aux cas d’utilisation de l’éditeur et de l’annonceur.

## IAB : envoi du consentement en aval {#iab-sending-consent-downstream}

Lorsque le module externe Audience Manager pour IAB TCF est utilisé, les choix de consentement de l’utilisateur sont également envoyés aux synchronisations d’ID au niveau de la plateforme (tiers) pour les partenaires qui sont présents sur la liste de fournisseurs globale, de sorte que le partenaire dispose des informations de consentement de l’utilisateur et peut également agir dessus. Ces informations sont envoyées dans deux variables :

* rgpd = 1
* gdpr_consentement = [ chaîne de consentement codée]

Si l’utilisateur se trouve dans un contexte IAB et ne donne pas son consentement (ou donne un consentement négatif), Audience Manager ne collecte pas du tout la chaîne IAB TC et abandonne donc les appels. Donc, dans ce cas... pas de transmission du consentement en aval.

## Démonstration {#demo}

Dans la vidéo ci-dessous, découvrez comment les sélections de choix des utilisateurs IAB affectent les cookies et les balises d’ECID et de solutions.

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

Pour plus d’informations sur le module externe Audience Manager pour IAB TCF 2.2, notamment sur la mise en œuvre et le test, les cas d’utilisation et le workflow, consultez la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=fr).
