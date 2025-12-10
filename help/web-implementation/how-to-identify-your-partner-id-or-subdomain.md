---
title: Comment identifier votre identifiant ou sous-domaine partenaire
description: Découvrez comment identifier votre ID de partenaire ou votre sous-domaine lors de l’implémentation de certaines fonctionnalités d’Experience Cloud et deux emplacements pour obtenir cet ID dans l’interface utilisateur d’Audience Manager.
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Comment identifier votre sous-domaine Audience Manager {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

Lors de l’implémentation de certaines fonctionnalités d’Experience Cloud, vous devez connaître votre `Subdomain` Audience Manager (également parfois appelée votre `client ID` ou votre `Partner ID`). Dans cette vidéo, nous allons vous montrer deux endroits où vous pouvez obtenir ces informations dans l’interface utilisateur d’Audience Manager.

## Donner la fin... {#giving-away-the-ending}

Si vous préférez simplement vous lancer et le trouver sans regarder cette courte vidéo, vous pouvez trouver votre `Partner Subdomain` à deux endroits dans l’interface utilisateur :

1. Si vous avez déjà créé une caractéristique [!UICONTROL rule-based], cliquez sur **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] est en regard de la caractéristique dans la liste des caractéristiques de ce dossier, et l’URL inclura votre sous-domaine dans l’URL.
1. Si vous accédez à l’interface **[!UICONTROL Tools]** > **[!UICONTROL Tags]** et cliquez sur **[!UICONTROL Get code]** pour votre conteneur, le sous-domaine se trouve vers la fin de la ligne Akamai

Si vous ne parvenez pas à le trouver rapidement avec ces références rapides, la vidéo est un engagement de courte durée. :)

>[!VIDEO](https://video.tv.adobe.com/v/40887/?captions=fre_fr&quality=12)

>[!IMPORTANT]
>
>Un ID numérique est attribué à chaque client du Adobe Experience Cloud. Il est souvent appelé « PID » (Partner ID). Ce n’est pas l’identifiant dont nous parlons dans cet article et cette vidéo. Au lieu de cela, le « sous-domaine partenaire », parfois appelé identifiant du partenaire, est généralement une version du nom du client et est le sous-domaine du serveur auquel les données sont envoyées. Par exemple, si votre société s’appelle « Bob’s Knobs » (toutes les choses étant des poignées de porte, bien sûr, haha), il est probable que votre sous-domaine partenaire soit « bobsknobs », tandis que le « PID » serait quelque chose comme « 12345 ». En règle générale, vous n’avez pas besoin de connaître votre PID, mais il est important de connaître votre sous-domaine afin de pouvoir configurer votre implémentation Audience Manager.
>
