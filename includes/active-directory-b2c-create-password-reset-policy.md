---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 78abb190dccd27c5bf70dfe12f978e1118601815
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355531"
---
Om in te schakelen fijnmazig wachtwoord opnieuw instellen van uw toepassing, gebruikt u een **wachtwoordherstel** gebruikersstroom. Houd er rekening mee dat de tenant-brede wachtwoordherstel optie is opgegeven [hier](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Deze gebruikersstroom beschrijft de ervaring van de klanten gaan voor wachtwoord opnieuw instellen en de inhoud van tokens die de toepassing bewerking is voltooid ontvangt.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Onder **beheren**, selecteer **gebruikersstromen** en klik op +**nieuwe gebruikersstroom**.

![Nieuwe gebruikersstroom selecteren](media/active-directory-b2c-create-password-reset-policy/add-b2c-new-user-flow.png)

Op de **aanbevolen** tabblad **wachtwoordherstel**.

Voer een gebruikersstroom **naam** voor uw toepassing om te verwijzen. Geef bijvoorbeeld `SSPR` op.

Onder **id-providers**, Controleer **wachtwoord opnieuw instellen met e-mailadres**.

![Voer naam en selecteer opnieuw instellen van wachtwoord met behulp van e-mailadres als id-provider](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Onder **toepassingsclaims**, klikt u op **meer weergeven** en kies de claims die u laten retourneren in de autorisatietokens die is verzonden naar de toepassing wilt na een geslaagde wachtwoord opnieuw instellen van het. Selecteer bijvoorbeeld **Object-ID van gebruiker**.

Klik op **OK**.

![Selecteer enkele toepassingsclaims en klik op de knop OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Klik op **maken** om toe te voegen van de gebruikersstroom. De gebruikersstroom wordt vermeld als **B2C_1_SSPR**. Het voorvoegsel **B2C_1_** wordt toegevoegd aan de naam.

Klik op **gebruikersstroom uitvoeren**. Controleer of de instellingen die zijn opgegeven in de tabel en klik vervolgens op **gebruikersstroom uitvoeren**.

![Gebruikersstroom selecteren en de App uitvoeren](media/active-directory-b2c-create-password-reset-policy/add-b2c-sspr-run-user-flow.png)

| Instelling      | Waarde  |
| ------------ | ------ |
| **Toepassing** | Contoso B2C-app |
| **Antwoord-URL selecteren** | `https://localhost:44316/` |

Er wordt een nieuw browsertabblad geopend. Hier kunt u controleren hoe het opnieuw instellen van het wachtwoord werkt voor consumenten.

> [!NOTE]
> Het duurt maximaal één minuut voordat het gemaakte beleid en de updates van kracht worden.
>
