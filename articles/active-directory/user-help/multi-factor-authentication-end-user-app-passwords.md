---
title: Over het gebruik van App-wachtwoorden in Azure MFA? | Microsoft Docs
description: Deze pagina helpt gebruikers begrijpen wat de app-wachtwoorden zijn en wat ze worden gebruikt voor met betrekking tot Azure MFA.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: lizross
ms.custom: end-user
ms.openlocfilehash: 290458e95aaed0cc85d83539d9d870c334df45df
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059429"
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Wat zijn App-wachtwoorden in Azure multi-factor Authentication?
Bepaalde niet-browsertoepassingen, zoals de Apple-systeemeigen e-mailclient die gebruikmaakt van Exchange Active Sync, bieden op dit moment geen ondersteuning van meervoudige verificatie. Multi-factor authentication is ingeschakeld per gebruiker. Dit betekent dat als een gebruiker is ingeschakeld voor multi-factor authentication en ze wilt gebruiken niet-browsertoepassingen, ze geen worden om dit te doen. Een app-wachtwoord maakt dit mogelijk. Als u multi-factor Authentication via beleid voor voorwaardelijke toegang en niet via MFA per gebruiker afdwingen, kunt u geen app-wachtwoorden maken. Toepassingen die gebruikmaken van beleid voor voorwaardelijke toegang om toegang te beheren, geen app-wachtwoorden nodig.

Zodra u een app-wachtwoord hebt, kunt u deze gebruiken in plaats van het wachtwoord van uw oorspronkelijke bij deze niet-browsertoepassingen. Dit komt omdat wanneer u zich voor verificatie in twee stappen registreren, u Microsoft niet aan iedereen zich aanmelden met uw wachtwoord dat bent als ze niet kunnen ook de tweede verificatie uitvoeren. De systeemeigen e-mailclient van Apple op uw telefoon aanmelden niet als u omdat het geen vragen over verificatie in twee stappen stellen. De oplossing voor dit is het maken van een veiliger app-wachtwoord die u niet gebruikt, dagelijks, maar alleen voor de apps die verificatie in twee stappen niet ondersteunt. Gebruik het app-wachtwoord zodat apps kunnen multi-factor authentication eenmalig overslaan en doorgaan met werken.

> [!NOTE]
> Nieuwe verificatieprotocollen worden ondersteund door Office 2013 clients (inclusief Outlook) en kan worden gebruikt met verificatie in twee stappen.  Dit betekent dat eenmaal is ingeschakeld, app-wachtwoorden niet vereist voor gebruik met Office 2013 clients zijn.  Zie voor meer informatie, [Office 2013 modern authentication openbare preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-to-use-app-passwords"></a>Het gebruik van app-wachtwoorden
Hier volgen enkele dingen om te weten over het gebruik van app-wachtwoorden.

* U kunt uw eigen app-wachtwoorden niet maken. In plaats daarvan worden deze automatisch gegenereerd. Omdat u alleen hebt de appwachtwoord eenmaal per app in te voeren, is het veiliger om te gebruiken een meer complexe, automatisch gegenereerd wachtwoord in plaats van een makkelijk te onthouden.
* Er is momenteel een limiet van 40 wachtwoorden per gebruiker. Als u probeert te maken nadat u de limiet hebt bereikt, wordt u gevraagd een van uw bestaande app-wachtwoorden verwijderen voordat u een nieuwe groep maken.
* U moet één appwachtwoord per apparaat, niet per toepassing. U kunt bijvoorbeeld één app-wachtwoord voor uw laptop maken en die appwachtwoord gebruiken voor al uw toepassingen op die laptop. Vervolgens maakt u een tweede app-wachtwoord moet worden gebruikt voor al uw apps op uw bureaublad.
* Krijgt u één app-wachtwoord de eerste keer dat u zich registreren voor verificatie in twee stappen.  Als u nieuwe zijn, kunt u ze kunt maken.



## <a name="creating-and-deleting-app-passwords"></a>Maken en verwijderen van app-wachtwoorden
Tijdens de eerste aanmelding krijgt u een app-wachtwoord die u kunt gebruiken.  U kunt daarnaast ook maken en verwijderen van app-wachtwoorden later op.  Hoe u dit doet, is afhankelijk van hoe u multi-factor authentication gebruiken. Beantwoord de volgende vragen om te bepalen waar u naartoe moet gaan voor het beheren van app-wachtwoorden:

1. Gebruik je verificatie in twee stappen voor uw persoonlijke Microsoft-account? Zo ja, u moet verwijzen naar de [App-wachtwoorden en verificatie in twee stappen](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikel voor meer informatie over. Als er geen, blijven twee vraag.

2. OK, zodat u verificatie in twee stappen voor uw werk of school-account gebruiken. Gebruikt u deze zich aanmeldt bij Office 365-apps? Zo ja, u moet verwijzen naar [maken van een app-wachtwoord voor Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) voor hulp. Als er geen, blijven vraag drie.

3. Gebruik je verificatie in twee stappen met Microsoft Azure? Zo ja, blijven de [app-wachtwoorden in Azure portal beheren](#manage-app-passwords-in-the-Azure-portal) sectie van dit artikel. Als er geen, blijven vier vraag.

4. Weet u niet zeker waar u verificatie in twee stappen? Blijven de [app-wachtwoorden met de MyApps-portal beheren](#manage-app-passwords-with-the-myapps-portal) sectie van dit artikel.


## <a name="manage-app-passwords-in-the-azure-portal"></a>App-wachtwoorden in Azure portal beheren
Als u verificatie met Azure, die u wilt maken van app-wachtwoorden via Azure portal.



## <a name="manage-app-passwords-with-the-myapps-portal"></a>App-wachtwoorden met de MyApps-portal beheren.
Als u niet zeker weet hoe u multi-factor authentication gebruiken, kunt klikt u vervolgens u altijd maken en verwijderen van app-wachtwoorden via de myapps-portal.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Het maken van een app-wachtwoord met behulp van de MyApps-portal
1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Klik op uw naam rechtsboven en kies **profiel**.
3. Selecteer **aanvullende beveiligingsverificatie**.
   ![Selecteer aanvullende beveiligingsverificatie - schermafbeelding](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Selecteer **app-wachtwoorden**.
   ![Selecteer de app-wachtwoorden - schermafbeelding](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Klik op **Create**.
6. Voer een naam op voor het app-wachtwoord in en klikt u op **volgende**.
7. Het app-wachtwoord kopiëren naar het Klembord en plak deze in uw app.
   ![Een app-wachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Verwijderen van een app-wachtwoord met behulp van de MyApps-portal
1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Aan de bovenkant profiel te selecteren.
3. Selecteer **aanvullende beveiligingsverificatie**.

   ![Selecteer aanvullende beveiligingsverificatie - schermafbeelding](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Selecteer **app-wachtwoorden**.

   ![Selecteer de app-wachtwoorden - schermafbeelding](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Naast het app-wachtwoord dat u wilt verwijderen, klikt u op **verwijderen**.

   ![Verwijderen van een app-wachtwoord](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Bevestig dat u verwijderen van dit wachtwoord wilt door te klikken op **Ja**.
7. Zodra de app-wachtwoord is verwijderd, klikt u op **sluiten**.

## <a name="next-steps"></a>Volgende stappen

- [De verificatie-instellingen voor verificatie in twee stappen beheren](multi-factor-authentication-end-user-manage-settings.md)

- Probeer de [Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md) om te controleren of uw aanmeldingen met app-meldingen, in plaats van tekst of aanroepen ontvangen.
