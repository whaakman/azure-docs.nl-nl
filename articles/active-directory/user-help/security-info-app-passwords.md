---
title: App-wachtwoorden instellen vanuit de pagina beveiligingsgegevens (preview) - Azure Active Directory | Microsoft Docs
description: Automatisch gegenereerde wachtwoorden instellen (app-wachtwoorden) voor gebruik met een niet-browser-app of een andere app die geen ondersteuning voor tweeledige verificatie bieden, in uw organisatie. Dit app-wachtwoord is gescheiden van een normaal wachtwoord en kan worden ingesteld van de pagina beveiligingsgegevens.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/13/2018
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 55dfab0c60e77b86157a005db34c37917a5e08d2
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57341099"
---
# <a name="manage-app-passwords-from-your-security-info-preview-page"></a>App-wachtwoorden beheren vanaf de pagina beveiligingsgegevens (preview)
Bepaalde apps, zoals Outlook 2010, bieden geen ondersteuning voor verificatie in twee stappen. Dit gebrek aan ondersteuning betekent dat als u verificatie in twee stappen in uw organisatie, de app werkt niet. Als u lost dit probleem, kunt u een automatisch gegenereerd wachtwoord gebruiken met elke niet-browser-app, los van uw normale wachtwoord maken.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Important]
>Uw beheerder mogelijk niet kunt u app-wachtwoorden gebruiken. Als er geen **App-wachtwoorden** als optie niet beschikbaar zijn in uw organisatie.

Wanneer u app-wachtwoorden, is het belangrijk om te onthouden:

- App-wachtwoorden zijn automatisch gegenereerd en slechts één keer per app worden opgegeven.

- Er is een limiet van 40 wachtwoorden per gebruiker. Als u probeert een na deze limiet te maken, wordt u gevraagd om te verwijderen van een bestaand wachtwoord naar de nieuwe maken te mogen.

- Gebruik één appwachtwoord per apparaat, niet per app. Maak bijvoorbeeld een enkel wachtwoord voor alle apps op uw laptop en vervolgens op een andere enkel wachtwoord voor alle apps op uw bureaublad.

    >[!Note]
    >Nieuwe verificatieprotocollen worden ondersteund door Office 2013 clients (inclusief Outlook) en kan worden gebruikt met verificatie in twee stappen. Deze ondersteuning betekent dat na verificatie in twee stappen is ingeschakeld, niet meer u app-wachtwoorden voor Office 2013 clients moet. Zie voor meer informatie de [hoe moderne verificatie werkt voor client-apps voor Office 2013 en Office 2016](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) artikel.

## <a name="create-new-app-passwords"></a>Nieuwe app-wachtwoorden maken
Als u verificatie in twee stappen met uw werk of school-account gebruiken en de beheerder de ervaring van de informatie over beveiliging is ingeschakeld, kunt u maken en verwijderen van uw app-wachtwoorden met behulp van de **beveiligingsgegevens** pagina.

>[!Note]
>Als de beheerder nog niet is ingeschakeld op de ervaring van de informatie over beveiliging, moet u Volg de instructies en informatie in de [beheren van app-wachtwoorden voor verificatie in twee stappen](multi-factor-authentication-end-user-app-passwords.md) sectie.

### <a name="to-create-a-new-app-password"></a>Een nieuw appwachtwoord maken
1. Meld u aan bij uw werk- of schoolaccount en gaat u naar uw https://myprofile.microsoft.com/ pagina.

    ![Mijn profiel-pagina, met gemarkeerde Security info koppelingen](media/security-info/securityinfo-myprofile-page.png)

2. Selecteer **beveiligingsgegevens** vanuit het navigatiedeelvenster links in of via de koppeling in de **beveiligingsgegevens** blokkeren, en selecteer vervolgens **-methode toevoegen** uit de **beveiligingsgegevens**  pagina.

    ![Pagina beveiligingsgegevens met optie van de methode gemarkeerde toevoegen](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Op de **toevoegen van een methode** weergeeft, schakelt **appwachtwoord** uit de vervolgkeuzelijst en selecteer vervolgens **toevoegen**.

    ![Methode vak met de geselecteerde App-wachtwoord toevoegen](media/security-info/securityinfo-myprofile-addpassword.png)

4. Typ de naam van de app waarvoor het app-wachtwoord en selecteer vervolgens **volgende**.

    ![Pagina App-wachtwoorden, met de naam van app](media/security-info/securityinfo-myprofile-password-appname.png)

5. Kopieer de tekst van de **wachtwoord** vak, plak het wachtwoord in het gebied van het wachtwoord van de app (in dit voorbeeld Outlook 2010) en selecteer vervolgens **gedaan**.

    ![Pagina App-wachtwoorden, met de naam van app](media/security-info/securityinfo-myprofile-password-copytext.png)
    
    Het wachtwoord is toegevoegd en u met succes kunt aanmelden bij uw app gaan.

## <a name="delete-your-app-passwords"></a>Uw app-wachtwoorden verwijderen
Als u niet meer nodig om een app waarvoor een app-wachtwoord te gebruiken, kunt u het bijbehorende app-wachtwoord kunt verwijderen. Verwijderen van het app-wachtwoord maakt u een van de beschikbare app-wachtwoord-punten voor gebruik in de toekomst.

>[!Important]
>Als u een app-wachtwoord per ongeluk verwijdert, is er geen manier om deze ongedaan te maken. U hebt te maken van een nieuw app-wachtwoord opnieuw invoeren in de app, de stappen in de [nieuwe app-wachtwoorden maken](#create-new-app-passwords) sectie van dit artikel.

### <a name="to-delete-an-app-password"></a>Verwijderen van een app-wachtwoord

1. Op de **beveiligingsgegevens** weergeeft, schakelt de **verwijderen** koppelen naast de **App-wachtwoord** optie voor de specifieke app.

    ![Koppeling naar de methode van de app-wachtwoord van beveiligingsgegevens verwijderen](media/security-info/securityinfo-myprofile-password-appdelete.png)

2. Selecteer **Ja** uit de bevestiging van verwijderen van de **App-wachtwoord**. Nadat de app-wachtwoord wordt verwijderd, wordt deze verwijderd uit je beveiligingsgegevens en, verdwijnt het uit de **beveiligingsgegevens** pagina.

## <a name="for-more-information"></a>Voor meer informatie
- Voor meer informatie over de **beveiligingsgegevens** pagina en hoe u deze instellen, Zie [info beveiligingsoverzicht](user-help-security-info-overview.md)
