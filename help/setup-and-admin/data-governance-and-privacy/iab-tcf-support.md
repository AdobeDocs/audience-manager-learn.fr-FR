---
title: Prise en charge du TCF 2.2 de l’IAB
description: Découvrez le module d’Audience Manager du TCF de l’IAB et comment il fonctionne avec l’objet d’accord préalable d’Adobe et votre fournisseur de gestion du consentement (CMP).
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

# Prise en charge du TCF 2.2 de l’IAB en Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe vous fournit les moyens de gérer et de communiquer les choix de confidentialité de vos utilisateurs par le biais de la fonctionnalité d’opt-in et de la prise en charge du plug-in d’Audience Manager dans Transparency and Consent Framework 2.2 de l’IAB (TCF 2.2). Cet article fonctionne avec la documentation pour vous aider à comprendre le module d’Audience Manager du TCF de l’IAB et son fonctionnement avec l’objet Opt-in d’Adobe et votre fournisseur de gestion du consentement (CMP). Pour en savoir plus sur l’IAB, consultez leur site Web à l’adresse [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## Première étape : comprendre l’inclusion de l’ID d’Experience Cloud {#first-step-understand-ecid-s-opt-in}

Pour comprendre comment utiliser le TCF de l’IAB, vous devez d’abord comprendre la fonctionnalité [!DNL Opt-in], qui fait partie de la bibliothèque du service d’ID Experience Cloud (ECID). Si vous ne savez pas comment fonctionne l’opt-in, consultez [cet article utile](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html?lang=fr) en premier. Vous devez également consulter la [documentation](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=fr) de l’Opt-in. Une fois que vous avez parcouru ces ressources, revenez à cette page et continuez.

## Module d’Audience Manager pour IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

Maintenant que vous avez une compréhension de base du fonctionnement du service Opt-in, l’Audience Manager peut y associer la prise en charge de [!DNL IAB Transparency and Consent Framework (TCF)], qui est effectuée par le biais d’un plug-in dans l’objet Opt-in.

Le module d’Audience Manager du TCF de l’IAB étend les fonctionnalités de l’Opt-in et permet à AAM clients d’évaluer, d’honorer et de transférer les choix de confidentialité des utilisateurs aux partenaires en aval conformément au TCF de l’IAB. Il fournit une norme que les contrôleurs de données (c’est-à-dire vous en tant que client Adobe) et les fournisseurs (DMP, DSP, SSP, serveurs de publicités, etc.) peut utiliser pour comprendre le consentement dans le paysage du consentement.

## Activation du TCF de l’IAB {#enabling-iab-tcf}

L’activation du module d’Audience Manager pour IAB TCF est facile si vous utilisez Adobe Experience Platform Launch, car il s’agit d’une simple case à cocher, comme illustré dans la courte vidéo ci-dessous :

>[!VIDEO](https://video.tv.adobe.com/v/38258/?quality=12&captions=fre_fr)

Si vous n’utilisez pas Launch, vous pouvez également utiliser `isIabContext=true` pour l’activer lorsque vous instanciez le visiteur Experience Cloud. Cela initie le flux du TCF de l’IAB, c’est-à-dire ajoute une autre étape à la collecte des consentements, en utilisant le TCF de l’IAB pour interroger la chaîne du TCF de l’IAB et la renvoie à Opt-in, qui, à son tour, communique avec les solutions Experience Cloud.

## Chaîne IAB TC {#iab-tcf-consent-string}

L’un des standards fournis par l’IAB est une &quot;chaîne de consentement&quot; (également appelée &quot;DaisyBit&quot;), qui est en fait deux listes assemblées :

1. Objectif : **Que** le consentement est-il donné pour faire ?
1. Fournisseurs : **À qui** le consentement est-il donné ?

### Objectif {#purposes}

Avec le TCF de l’IAB 2.2, il existe dix &quot;objectifs&quot; pour recueillir le consentement (ce que les fournisseurs peuvent faire avec les données du visiteur). Adobe Audience Manager ne nécessite pas les dix, mais nécessite uniquement un consentement aux fins suivantes, en plus du consentement du fournisseur :

* **Objectif 1 :** Stocker et/ou accéder aux informations sur un appareil ;
* **Objectif 10 :** Développer et améliorer les produits ;
* **Objectif spécial 1 :** Assurez la sécurité, évitez la fraude et déboguez.

Il s’agit de la première partie de la chaîne du TC de l’IAB. Elle est simplement enregistrée sous la forme 1 et 0, indiquant si cet objectif/cette activité est approuvé ou non.

>[!NOTE]
>
>Conformément aux règlements de l’IAB, l’objectif spécial 1 (assurer la sécurité, prévenir la fraude et déboguer) est toujours accepté et les utilisateurs ne peuvent pas s’y opposer.

### Fournisseurs {#vendors}

Une autre partie de la chaîne IAB TC est une longue liste de plusieurs centaines de fournisseurs, de sorte que les visiteurs puissent se voir présenter une liste des fournisseurs applicables qui ont des balises sur le site et peuvent choisir les fournisseurs à utiliser. Les vendeurs conservent leur place sur la liste. Par exemple, le numéro du fournisseur Adobe Audience Manager figurant dans cette liste est 565. Si ce nombre de la liste comporte un &quot;1&quot;, l’Audience Manager peut effectuer les opérations approuvées à l’avant de la liste. Si la zone d’AAM présente un &quot;0&quot;, elle ne peut rien faire avec les données.

**Pour qu’une Audience Manager puisse fournir une interface utilisateur aux clients afin qu’ils utilisent le TCF de l’IAB pour choisir ces objectifs et ces fournisseurs, ou pour approuver/désapprouver toute activité, vous devez utiliser un partenaire de CMP enregistré avec le TCF de l’IAB ou en créer un qui prend en charge le TCF de l’IAB et enregistré avec le TCF de l’IAB.**

## Opt-in : traduction entre IAB et applications Adobe {#opt-in-translating-between-iab-and-adobe-solutions}

L’un des avantages de l’utilisation du TCF de l’IAB est que les objectifs standard répertoriés ci-dessus donnent probablement à l’utilisateur final plus d’idée de ce qu’il approuve qu’une liste de solutions d’Adobe. Les utilisateurs finaux peuvent ne pas savoir ce que signifie &quot;approuver&quot; l’Audience Manager ou [!DNL Target], mais &quot;stocker et/ou accéder aux informations sur un appareil&quot; ou &quot;développer et améliorer des produits&quot; est probablement plus facile à comprendre et à accepter.

Pour que l&#39;Audience Manager soit validée (c.-à-d. Pour que la traduction des objectifs de l’IAB soit effectuée dans le cadre de l’Opt-in (afin de permettre AAM un &quot;oui&quot;), les objectifs 1 et 10, comme indiqué ci-dessus, doivent recevoir le consentement de l’utilisateur final. Si l’un d’eux n’est pas approuvé, ou si un fournisseur n’est pas approuvé, AAM n’exécutera pas le déclenchement des pixels ou ne définira pas de cookies. Il est également bon de savoir que de nombreux clients choisissent simplement de fournir à l’utilisateur final une interface utilisateur &quot;tout ou rien&quot;, qui permettrait ou non, bien sûr, l’utilisation de l’Audience Manager (et des autres solutions Experience Cloud).

La [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=fr) contient de superbes informations sur la manière dont le flux du plug-in d’Audience Manager pour IAB TCF s’applique aux cas d’utilisation de l’éditeur et de l’annonceur.

## IAB : envoi du consentement en aval {#iab-sending-consent-downstream}

Lorsque le module d’Audience Manager pour IAB TCF est utilisé, les choix de consentement de l’utilisateur sont également envoyés aux synchronisations d’ID au niveau de la plateforme (tierces) pour les partenaires présents sur la liste globale de fournisseurs, de sorte que le partenaire dispose des informations de consentement de l’utilisateur et puisse également agir sur celle-ci. Ces informations sont envoyées dans deux variables :

* gdpr = 1
* gdpr_consent = [chaîne de consentement codée]

Si l’utilisateur se trouve dans le contexte de l’IAB et qu’il ne donne pas son consentement (ou ne fournit pas de consentement négatif), l’Audience Manager ne collecte pas du tout la chaîne du TC de l’IAB et, de ce fait, supprime les appels. Donc, dans ce cas...aucun consentement en aval.

## Démonstration {#demo}

Dans la vidéo ci-dessous, découvrez comment les cookies et les balises d’ECID et des solutions sont affectés par les sélections de choix des utilisateurs IAB.

>[!VIDEO](https://video.tv.adobe.com/v/38241/?quality=12&captions=fre_fr)

Pour plus d’informations sur le module d’Audience Manager pour IAB TCF 2.2, y compris sur la mise en oeuvre et le test, les cas d’utilisation et le processus, consultez la [documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=fr).
