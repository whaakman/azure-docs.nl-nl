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
ms.openlocfilehash: 0812719ddee8c0ff0c2fa9256c2819611692dfe5
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Wat zijn App-wachtwoorden in Azure multi-factor Authentication?
Bepaalde niet-browsertoepassingen, zoals de systeemeigen e-mailclient van Apple die gebruikmaakt van Exchange Active Sync, ondersteunen momenteel geen multi-factor authentication-server. Multi-factor authentication is ingeschakeld per gebruiker.  Dit betekent dat een gebruiker multi-factor authentication-server niet gebruiken als:

- De gebruiker is ingeschakeld voor multi-factor authentication
- De gebruiker probeert een niet-browser-app gebruiken.

Een app-wachtwoord kan de gebruiker de app gebruiken.

Zodra u een app-wachtwoord hebt, kunt u deze gebruiken in plaats van het oorspronkelijke wachtwoord met deze niet-browsertoepassingen. Wanneer u zich voor verificatie in twee stappen registreert, bent u zorgt ervoor dat Microsoft niet wilt laten iedereen zich aanmelden met uw wachtwoord als ze ook de tweede verificatie kunnen niet uitvoeren. De systeemeigen e-mailclient van Apple op uw telefoon kan niet aanmelden als u omdat deze kan niet om verificatie in twee stappen vragen. De oplossing voor dit probleem is het een beter beveiligd appwachtwoord die u niet gebruikt dagelijkse maken. App-wachtwoorden zijn alleen voor apps die verificatie in twee stappen niet ondersteunt. Het app-wachtwoord gebruiken zodat apps kunnen multi-factor authentication overslaan en doorgaan met werken.


> [!NOTE]
> Office 2013-clients (inclusief Outlook) bieden ondersteuning voor nieuwe verificatieprotocollen en kan worden gebruikt met verificatie in twee stappen. App-wachtwoorden zijn niet vereist voor gebruik met Office 2013-clients.  Zie voor meer informatie [Office 2013 modern authentication openbare preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-to-use-app-passwords"></a>Het gebruik van app-wachtwoorden
Hier volgen enkele dingen die u moet weten over app-wachtwoorden:

* U kunt uw eigen app-wachtwoorden niet maken. Ze worden automatisch gegenereerd.
* Er is momenteel een limiet van 40 wachtwoorden per gebruiker.
* Als u probeert te maken van een app-wachtwoord nadat u hebt de limiet bereikt, hebt u een van uw bestaande app-wachtwoorden te verwijderen voordat u een nieuwe maakt.
* Gebruik één appwachtwoord per apparaat, niet per toepassing. U kunt bijvoorbeeld één app-wachtwoord voor uw laptop maken en dit app-wachtwoord gebruiken voor alle toepassingen op die laptop. Maak vervolgens een tweede app-wachtwoord gebruiken voor uw apps op het bureaublad.
* Krijgt u één app-wachtwoord de eerste keer dat u zich registreert voor verificatie in twee stappen.  Als u aanvullende bepalingen nodig hebt, kunt u ze kunt maken.



## <a name="creating-and-deleting-app-passwords"></a>Maken en het verwijderen van app-wachtwoorden
Tijdens de eerste aanmelden krijgt u een app-wachtwoord die u kunt gebruiken.  U kunt ook maken en verwijderen van app-wachtwoorden later op. Hoe u app-wachtwoorden verwijderen, is afhankelijk van hoe u multi-factor authentication-server gebruiken. Beantwoord de volgende vragen om te bepalen waar u naartoe moet app-wachtwoorden te beheren:

1. Gebruik je verificatie in twee stappen voor uw persoonlijke Microsoft-account? Zo ja, u moet verwijzen naar de [App-wachtwoorden en verificatie in twee stappen](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikel voor hulp. Zo Nee, blijven twee vraag.

2. OK, zodat u verificatie in twee stappen voor uw werk of school-account gebruiken. Gebruikt u deze aan te melden bij Office 365-apps? Zo ja, u moet verwijzen naar [maken van een app-wachtwoord voor Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) voor hulp. Zo Nee, blijven vraag drie.

3. Gebruik je verificatie in twee stappen met Microsoft Azure? Zo ja, blijven de [beheren van app-wachtwoorden in de Azure portal](#manage-app-passwords-in-the-Azure-portal) sectie van dit artikel. Zo Nee, blijven vier vraag.

4. Weet u niet waar u verificatie in twee stappen gebruiken? Blijven de [beheren van app-wachtwoorden met de portal MyApps](#manage-app-passwords-with-the-myapps-portal) sectie van dit artikel.


## <a name="manage-app-passwords-in-the-azure-portal"></a>App-wachtwoorden in de Azure portal beheren
Als u verificatie in twee stappen met Azure gebruikt, die u wilt maken van app-wachtwoorden via de Azure portal.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Appwachtwoorden te maken in de Azure portal
1. Meld u aan bij Azure Portal.
2. Aan de bovenkant op uw gebruikersnaam en selecteer **wachtwoord wijzigen**.
3. Selecteer op de pagina proofup aan de bovenkant **App-wachtwoorden**.
4. Selecteer **Maken**.
5. Voer een naam voor het app-wachtwoord in en selecteer **volgende**.
6. Het app-wachtwoord naar het Klembord kopiëren en plakken in uw app.

   ![Cloud](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


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
