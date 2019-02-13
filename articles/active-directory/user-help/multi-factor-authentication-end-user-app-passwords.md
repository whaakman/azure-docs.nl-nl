---
title: App-wachtwoorden in Azure Active Directory beheren | Microsoft Docs
description: Deze pagina helpt gebruikers begrijpen wat de app-wachtwoorden zijn en wat ze worden gebruikt voor met betrekking tot verificatie in twee stappen.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdcd078714d8311cf59471492187314183de28b2
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56187301"
---
# <a name="manage-app-passwords-for-two-step-verification"></a>App-wachtwoorden voor verificatie in twee stappen beheren

Bepaalde niet-browsertoepassingen, zoals Outlook 2010, biedt geen ondersteuning voor verificatie in twee stappen. Dit gebrek aan ondersteuning betekent dat als u verificatie in twee stappen, de app werkt niet. Als u lost dit probleem, kunt u een automatisch gegenereerd wachtwoord gebruiken met elke niet-browser-app, los van uw normale wachtwoord maken.

Wanneer u app-wachtwoorden, is het belangrijk om te onthouden:

- App-wachtwoorden zijn automatisch gegenereerd en slechts één keer per app worden opgegeven.

- Er is een limiet van 40 wachtwoorden per gebruiker. Als u probeert een na deze limiet te maken, wordt u gevraagd om te verwijderen van een bestaand wachtwoord naar de nieuwe maken te mogen.

- Gebruik één appwachtwoord per apparaat, niet per app. Maak bijvoorbeeld een enkel wachtwoord voor alle apps op uw laptop en vervolgens op een andere enkel wachtwoord voor alle apps op uw bureaublad.

    >[!Note]
    >Nieuwe verificatieprotocollen worden ondersteund door Office 2013 clients (inclusief Outlook) en kan worden gebruikt met verificatie in twee stappen. Deze ondersteuning betekent dat na verificatie in twee stappen is ingeschakeld, niet meer u app-wachtwoorden voor Office 2013 clients moet. Zie voor meer informatie de [hoe moderne verificatie werkt voor client-apps voor Office 2013 en Office 2016](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) artikel.

## <a name="where-to-create-and-delete-your-app-passwords"></a>Waar u wilt maken en uw app-wachtwoorden verwijderen

Tijdens het registreren van uw initiële verificatie in twee stappen controle krijgt u een app-wachtwoord. Als u meer dan één wachtwoord nodig hebt, kunt u aanvullende wachtwoorden, op basis van hoe u verificatie in twee stappen:

- **U verificatie in twee stappen gebruiken met uw werk of school-account en de MyApps-portal.** Maken en verwijderen van uw app-wachtwoorden met behulp van de instructies in de [maken en verwijderen app-wachtwoorden met behulp van de MyApps-portal](#create-and-delete-app-passwords-using-the-myapps-portal) sectie van dit artikel. Zie voor meer informatie over de MyApps-portal en het gebruik ervan [wat is de MyApps-portal in Azure Active Directory?](active-directory-saas-access-panel-introduction.md).

- **U verificatie in twee stappen gebruiken met uw werk of school-account en de Office 365-portal.** Maken en verwijderen van uw app-wachtwoorden met behulp van de instructies in de [maken en verwijderen app-wachtwoorden met behulp van de Office 365-portal](#create-and-delete-app-passwords-using-the-office-365-portal) sectie van dit artikel.

- **U verificatie in twee stappen gebruiken met uw persoonlijke Microsoft-account.** Maken en verwijderen van uw app-wachtwoorden met behulp van de [basisprincipes van beveiliging](https://account.microsoft.com/account/) pagina aan uw persoonlijke Microsoft-account. Zie voor meer informatie de [App-wachtwoorden en verificatie in twee stappen](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikel.

## <a name="create-and-delete-app-passwords-using-the-myapps-portal"></a>Maken en verwijderen van app-wachtwoorden met behulp van de MyApps-portal
U kunt maken en verwijderen van app-wachtwoorden via de MyApps-portal.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Het maken van een app-wachtwoord met behulp van de MyApps-portal

1. Meld u aan bij [https://myapps.microsoft.com](https://myapps.microsoft.com).

2. Selecteer uw naam rechtsboven en kies **profiel**.

3. Selecteer **aanvullende beveiligingsverificatie**.

   ![Selecteer aanvullende beveiligingsverificatie - schermafbeelding](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Selecteer **App-wachtwoorden**.

   ![Selecteer de app-wachtwoorden - schermafbeelding](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Klik op **Create**.

6. Typ een naam voor het app-wachtwoord en selecteer vervolgens **volgende**.

7. Het app-wachtwoord kopiëren naar het Klembord en plak deze in uw app.
   
    ![Een app-wachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Verwijderen van een app-wachtwoord met behulp van de MyApps-portal

1. Ga naar uw profiel, en selecteer vervolgens **aanvullende beveiligingsverificatie**.

2. Selecteer **App-wachtwoorden**, en selecteer vervolgens **verwijderen** naast het app-wachtwoord die u wilt verwijderen.

   ![Verwijderen van een app-wachtwoord](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

3. Selecteer **Ja** om te bevestigen dat u wilt verwijderen van het wachtwoord en selecteer vervolgens **sluiten**.

## <a name="create-and-delete-app-passwords-using-the-office-365-portal"></a>Maken en verwijderen van app-wachtwoorden met behulp van de Office 365-portal

Als u verificatie in twee stappen met uw werk of school-account en uw Office 365-apps gebruikt, kunt u maken en verwijderen van uw app-wachtwoorden met behulp van de Office 365-portal. U kunt maximaal 40 app-wachtwoorden hebben op elk willekeurig moment. Als u een andere app-wachtwoord na deze limiet moet, hebt u een van uw bestaande appwachtwoorden verwijderen.

### <a name="to-create-app-passwords-using-the-office-365-portal"></a>App-wachtwoorden met behulp van de Office 365-portal maken

1. Meld u aan bij uw werk- of schoolaccount.

2. Ga naar https://portal.office.com, selecteer de **instellingen** pictogram in de rechterbovenhoek van de **Office 365-portal** pagina uit en vouw vervolgens **aanvullende beveiligingsverificatie**.

    ![Office-portal met extra beveiliging verificatie gebied uitgevouwen](media/security-info/security-info-o365password.png)

3. Selecteer de tekst waarin wordt gemeld, **maken en beheren van app-wachtwoorden** te openen de **app-wachtwoorden** pagina.

4. Selecteer **maken**, typ een beschrijvende naam voor de app die de app-wachtwoord nodig, en selecteer vervolgens **volgende**.

5. Selecteer **wachtwoord naar Klembord kopiëren**, en selecteer vervolgens **sluiten**.

6. Gebruik het gekopieerde app-wachtwoord aanmelden bij uw niet-browser-app. U hoeft alleen één keer wachtwoord invoeren en deze voor de toekomst wordt onthouden.

### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Verwijderen van app-wachtwoorden met behulp van de Office 365-portal

1. Meld u aan bij uw werk- of schoolaccount.

2. Ga naar https://portal.office.com, selecteer de **instellingen** pictogram in de rechterbovenhoek van de **Office 365-portal** pagina en selecteer vervolgens **aanvullende beveiligingsverificatie**.

3. Selecteer de tekst waarin wordt gemeld, **maken en beheren van app-wachtwoorden** te openen de **app-wachtwoorden** pagina.

4. Selecteer **verwijderen** voor het app-wachtwoord wilt verwijderen, selecteert u **Ja** in het bevestigingsvenster en selecteer vervolgens **sluiten**.

    Het app-wachtwoord is verwijderd.

5. Volg de stappen voor het maken van een app-wachtwoord voor het maken van uw nieuwe app-wachtwoord.

## <a name="if-your-app-passwords-arent-working-properly"></a>Als uw app-wachtwoorden niet goed werken

Zorg ervoor dat u uw wachtwoord correct hebt ingevoerd. Als u zeker dat u uw wachtwoord juist hebt ingevoerd, kunt u proberen opnieuw aan te melden en een nieuw app-wachtwoord maken. Als geen van deze opties voor uw probleem wordt verholpen, contact op met het ondersteuningsteam van uw bedrijf, zodat ze uw bestaande app-wachtwoorden, zodat u kunt een gloednieuwe maken kunnen verwijderen. 

## <a name="next-steps"></a>Volgende stappen

- [De verificatie-instellingen voor verificatie in twee stappen beheren](multi-factor-authentication-end-user-manage-settings.md)

- Probeer de [Microsoft Authenticator-app](user-help-auth-app-download-install.md) om te controleren of uw aanmeldingen met app-meldingen, in plaats van tekst of aanroepen ontvangen.
