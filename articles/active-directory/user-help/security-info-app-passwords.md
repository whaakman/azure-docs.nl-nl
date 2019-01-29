---
title: Appwachtwoorden met beveiligingsgegevens - Azure Active Directory instellen | Microsoft Docs
description: Instellen van de automatisch gegenereerde wachtwoorden (app-wachtwoorden) voor gebruik met elke niet-browser-app te scheiden van een normaal wachtwoord, met beveiligingsgegevens.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 185c6e18a84369702de3bd0c398a9cc0c64b8217
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191377"
---
# <a name="manage-app-passwords-using-security-info-preview"></a>Beheren van app-wachtwoorden met beveiligingsgegevens (preview)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Bepaalde niet-browsertoepassingen, zoals Outlook 2010, biedt geen ondersteuning voor verificatie in twee stappen. Dit gebrek aan ondersteuning betekent dat als u verificatie in twee stappen, de app werkt niet. Als u lost dit probleem, kunt u een automatisch gegenereerd wachtwoord gebruiken met elke niet-browser-app, los van uw normale wachtwoord maken.

Wanneer u app-wachtwoorden, is het belangrijk om te onthouden:

- App-wachtwoorden zijn automatisch gegenereerd en slechts één keer per app worden opgegeven.

- Er is een limiet van 40 wachtwoorden per gebruiker. Als u probeert een na deze limiet te maken, wordt u gevraagd om te verwijderen van een bestaand wachtwoord naar de nieuwe maken te mogen.

- Gebruik één appwachtwoord per apparaat, niet per app. Maak bijvoorbeeld een enkel wachtwoord voor alle apps op uw laptop en vervolgens op een andere enkel wachtwoord voor alle apps op uw bureaublad.

    >[!Note]
    >Nieuwe verificatieprotocollen worden ondersteund door Office 2013 clients (inclusief Outlook) en kan worden gebruikt met verificatie in twee stappen. Deze ondersteuning betekent dat na verificatie in twee stappen is ingeschakeld, niet meer u app-wachtwoorden voor Office 2013 clients moet. Zie voor meer informatie de [hoe moderne verificatie werkt voor client-apps voor Office 2013 en Office 2016](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) artikel.

## <a name="create-and-delete-app-passwords-using-security-info"></a>Maken en verwijderen van app-wachtwoorden met beveiligingsgegevens

Als u verificatie in twee stappen met uw werk gebruiken of school-account en de beheerder de ervaring van de informatie over beveiliging is ingeschakeld, kunt u maken en verwijderen van uw app-wachtwoorden met behulp van de portal mijn Apps.

Als de beheerder nog niet is ingeschakeld op de ervaring van de informatie over beveiliging, moet u Volg de instructies en informatie in de [beheren van app-wachtwoorden voor verificatie in twee stappen](multi-factor-authentication-end-user-app-passwords.md) sectie.

### <a name="to-create-app-passwords-using-the-my-apps-portal"></a>Met behulp van de portal mijn Apps van appwachtwoorden te maken

1. Meld u aan bij uw werk- of schoolaccount.

2. Ga naar myapps.microsoft.com, selecteert uw naam in de rechterbovenhoek van de pagina en selecteer vervolgens **profiel**.

3. In de **-account beheren** gedeelte **beveiligingsgegevens bewerken**.

    ![Scherm profiel met de koppeling bewerken security info gemarkeerd](media/security-info/security-info-profile.png)

4. In de **je account blijft beveiligd** scherm, selecteer **toevoegen van beveiligingsgegevens**.

    ![Security info scherm met bestaande, bewerkbare info](media/security-info/security-info-edit-add-info.png)

5. In de **toevoegen van beveiligingsgegevens** scherm, selecteer **App-wachtwoord**.

6. In de **uw app-wachtwoord maken** scherm, typt u een naam voor uw app-wachtwoord en selecteer vervolgens **volgende**.

    ![Scherm waar u uw app-wachtwoord naam](media/security-info/security-info-name-app-password.png)

7. Selecteer **kopie** moet het wachtwoord naar Klembord kopiëren en selecteer vervolgens **volgende**.

    ![Met app-wachtwoord voor het kopiëren van het scherm](media/security-info/security-info-create-app-password.png)
    
8. Zorg ervoor dat het app-wachtwoord wordt weergegeven op de **je account blijft beveiligd** scherm.

    ![Beveiligde scherm, met app-wachtwoord houden](media/security-info/security-info-keep-secure-app-password.png)

### <a name="to-delete-app-passwords-using-the-my-apps-portal"></a>Met behulp van de portal mijn Apps van app-wachtwoorden verwijderen

1. Op de **je account blijft beveiligd** scherm, selecteer de **X** naast het app-wachtwoord om te verwijderen.

    ![Scherm veilig te houden, verwijdert u app-wachtwoord](media/security-info/security-info-keep-secure-delete-app-password.png)

2. In de **verwijderen toepassingswachtwoord** scherm, selecteer **verwijderen**.

    ![Scherm app-wachtwoord verwijderen](media/security-info/security-info-keep-secure-delete-app-password2.png)

## <a name="next-steps"></a>Volgende stappen

- Als u uw beveiligingsgegevens bijwerken wilt, volgt u de instructies in de [je beveiligingsgegevens beheren](security-info-manage-settings.md) artikel.

- Zie voor meer algemene informatie over beveiligingsgegevens en kunt u doen [info beveiligingsoverzicht](user-help-security-info-overview.md) 
