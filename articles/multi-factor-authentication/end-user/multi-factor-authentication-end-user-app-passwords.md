---
title: Het gebruik van App-wachtwoorden in Azure MFA? | Microsoft Docs
description: Deze pagina helpt gebruikers begrijpen wat app-wachtwoorden zijn en wat ze worden gebruikt met betrekking tot de Azure MFA.
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 55ca5ada0db30440e4599c77b7a6834ef671c7a4
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Wat zijn App-wachtwoorden in Azure multi-factor Authentication?
Bepaalde niet-browsertoepassingen, zoals de systeemeigen e-mailclient van Apple die gebruikmaakt van Exchange Active Sync, ondersteunen momenteel geen multi-factor authentication-server. Multi-factor authentication is ingeschakeld per gebruiker. Dit betekent dat als een gebruiker is ingeschakeld voor multi-factor authentication en ze zijn bij het gebruik van niet-browsertoepassingen, ze zich niet te doen. Een app-wachtwoord maakt dit mogelijk. Als u multi-factor Authentication via beleid voor voorwaardelijke toegang en niet via MFA per gebruiker afdwingen, kunt u geen app-wachtwoorden maken. Toepassingen die gebruikmaken van beleidsregels voor voorwaardelijke toegang om toegang te beheren, hoeft geen app-wachtwoorden.

Zodra u een app-wachtwoord hebt, kunt u deze gebruiken in plaats van het oorspronkelijke wachtwoord met deze niet-browsertoepassingen. Dit is omdat wanneer u zich voor verificatie in twee stappen registreert, u Microsoft niet wilt laten iedereen zich aanmelden met uw wachtwoord dat bent als ze ook de tweede verificatie kunnen niet uitvoeren. De systeemeigen e-mailclient van Apple op uw telefoon kan niet aanmelden als u omdat deze kan niet om verificatie in twee stappen vragen. De oplossing voor dit is het maken van een veiliger app-wachtwoord die u niet gebruikt dagelijkse, maar alleen voor apps die verificatie in twee stappen niet ondersteunt. Het app-wachtwoord gebruiken zodat apps kunnen multi-factor authentication overslaan en doorgaan met werken.

> [!NOTE]
> Office 2013-clients (inclusief Outlook) bieden ondersteuning voor nieuwe verificatieprotocollen en kan worden gebruikt met verificatie in twee stappen.  Dit betekent dat eenmaal is ingeschakeld, app-wachtwoorden niet vereist voor gebruik met Office 2013-clients zijn.  Zie voor meer informatie [Office 2013 modern authentication openbare preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-to-use-app-passwords"></a>Het gebruik van app-wachtwoorden
Hier volgen enkele om te onthouden over het gebruik van app-wachtwoorden.

* U kunt uw eigen app-wachtwoorden niet maken. In plaats daarvan worden ze automatisch gegenereerd. Omdat u de appwachtwoord eenmaal per app op te geven, is het veiliger om te gebruiken in een complexere, automatisch gegenereerd wachtwoord in plaats van een makkelijk te onthouden.
* Er is momenteel een limiet van 40 wachtwoorden per gebruiker. Als u probeert te maken nadat u hebt de limiet bereikt, wordt u gevraagd een van uw bestaande app-wachtwoorden te verwijderen voordat u een nieuwe maakt.
* U moet één appwachtwoord per apparaat, niet per toepassing. U kunt bijvoorbeeld één app-wachtwoord voor uw laptop maken en dit app-wachtwoord gebruiken voor alle toepassingen op die laptop. Maak vervolgens een tweede app-wachtwoord gebruiken voor uw apps op het bureaublad.
* Krijgt u één app-wachtwoord de eerste keer dat u zich registreert voor verificatie in twee stappen.  Als u aanvullende bepalingen nodig hebt, kunt u ze kunt maken.



## <a name="creating-and-deleting-app-passwords"></a>Maken en het verwijderen van app-wachtwoorden
Tijdens de eerste aanmelden krijgt u een app-wachtwoord die u kunt gebruiken.  U kunt bovendien ook maken en verwijderen van app-wachtwoorden later op.  Hoe u dit wilt doen, is afhankelijk van hoe u multi-factor authentication-server gebruiken. Beantwoord de volgende vragen om te bepalen waar u naartoe moet app-wachtwoorden te beheren:

1. Gebruik je verificatie in twee stappen voor uw persoonlijke Microsoft-account? Zo ja, u moet verwijzen naar de [App-wachtwoorden en verificatie in twee stappen](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikel voor hulp. Zo Nee, blijven twee vraag.

2. OK, zodat u verificatie in twee stappen voor uw werk of school-account gebruiken. Gebruikt u deze aan te melden bij Office 365-apps? Zo ja, u moet verwijzen naar [maken van een app-wachtwoord voor Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) voor hulp. Zo Nee, blijven vraag drie.

3. Gebruik je verificatie in twee stappen met Microsoft Azure? Zo ja, blijven de [beheren van app-wachtwoorden in de Azure portal](#manage-app-passwords-in-the-Azure-portal) sectie van dit artikel. Zo Nee, blijven vier vraag.

4. Weet u niet waar u verificatie in twee stappen gebruiken? Blijven de [beheren van app-wachtwoorden met de portal MyApps](#manage-app-passwords-with-the-myapps-portal) sectie van dit artikel.


## <a name="manage-app-passwords-in-the-azure-portal"></a>App-wachtwoorden in de Azure portal beheren
Als u verificatie in twee stappen met Azure gebruikt, die u wilt maken van app-wachtwoorden via de Azure portal.



## <a name="manage-app-passwords-with-the-myapps-portal"></a>App-wachtwoorden met de portal MyApps beheren.
Als u niet zeker weet hoe u meervoudige verificatie gebruiken, kunt klikt u altijd maken en verwijderen van app-wachtwoorden via de portal myapps.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Een appwachtwoord met behulp van de portal MyApps maken
1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Klik op uw naam in de rechterbovenhoek en kies **profiel**.
3. Selecteer **aanvullende beveiligingsverificatie**.
   ![Selecteer aanvullende beveiligingsverificatie - schermafbeelding](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Selecteer **app-wachtwoorden**.
   ![Selecteer de app-wachtwoorden - schermafbeelding](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Klik op **Create**.
6. Voer een naam voor het app-wachtwoord en klik op **volgende**.
7. Het app-wachtwoord naar het Klembord kopiëren en plakken in uw app.
   ![Een app-wachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Een app-wachtwoord met behulp van de portal MyApps verwijderen
1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Aan de bovenkant profiel te selecteren.
3. Selecteer **aanvullende beveiligingsverificatie**.

   ![Selecteer aanvullende beveiligingsverificatie - schermafbeelding](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Selecteer **app-wachtwoorden**.

   ![Selecteer de app-wachtwoorden - schermafbeelding](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Naast het app-wachtwoord dat u wilt verwijderen, klikt u op **verwijderen**.

   ![Verwijderen van een app-wachtwoord](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Bevestig dat u verwijderen van dit wachtwoord wilt door te klikken op **Ja**.
7. Als het app-wachtwoord wordt verwijderd, klikt u op **sluiten**.

## <a name="next-steps"></a>Volgende stappen

- [De instellingen van de verificatie in twee stappen beheren](multi-factor-authentication-end-user-manage-settings.md)

- Probeer de [Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md) om te controleren of uw aanmeldingen met app-meldingen in plaats van de ontvangen tekst of aanroepen.
