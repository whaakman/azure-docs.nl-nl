---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 0d9f0a24d84bd18bdf1fac84c744cc34a7d89ab3
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742310"
---
Als u inschakelen met het bewerken van profielen van uw toepassing wilt, gebruikt u een **profielbewerking** gebruikersstroom. Deze gebruikersstroom beschrijft de ervaring van klanten doorlopen tijdens het bewerken van profielen en de inhoud van tokens die de toepassing bewerking is voltooid ontvangt.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Onder **beheren**, selecteer **gebruikersstromen** en klik op +**nieuwe gebruikersstroom**.

![Nieuwe gebruikersstroom selecteren](media/active-directory-b2c-create-profile-editing-policy/add-b2c-new-user-flow.png)

Op de **aanbevolen** tabblad **profielbewerking**.

Voer een gebruikersstroom **naam** voor uw toepassing om te verwijzen. Geef bijvoorbeeld `SiPe` op.

Onder **id-providers**, Controleer **aanmelden met lokaal Account**. U kunt er ook voor kiezen om sociale id-providers te selecteren als dit al is geconfigureerd.

![Selecteer Aanmelden met lokaal account als id-provider en klik op de knop OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-profile-editing-identity-providers.png)

Onder **gebruikerskenmerken**, klikt u op **meer weergeven**. In de **verzamelen kenmerk** kolom, kies kenmerken de consument kan bekijken en bewerken in het bijbehorende profiel. Schakel bijvoorbeeld **Land/regio**, **Weergavenaam** en **Postcode** in.

In de **geretourneerde claim** kolom, kies de claims die u wilt laten retourneren in de autorisatietokens die is verzonden naar de toepassing na een geslaagde profielbewerking. Selecteer bijvoorbeeld **Weergavenaam**, **Postcode**.

Klik op **OK**.

![Selecteer enkele toepassingsclaims en klik op de knop OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-user-attributes.png)

Klik op **maken** om toe te voegen van de gebruikersstroom. De gebruikersstroom wordt vermeld als **B2C_1_SiPe**. Het voorvoegsel **B2C_1_** wordt toegevoegd aan de naam.

Selecteer **gebruikersstroom uitvoeren**. Controleer of de instellingen die zijn opgegeven in de tabel en klik vervolgens op **gebruikersstroom uitvoeren**.

![Gebruikersstroom selecteren en de App uitvoeren](media/active-directory-b2c-create-profile-editing-policy/add-b2c-profile-editing-run-user-flow.png)

| Instelling      | Waarde  |
| ------------ | ------ |
| **Toepassing** | Contoso B2C-app |
| **Antwoord-URL** | `https://localhost:44316/` |

Er wordt een nieuw browsertabblad geopend. Hier kunt u controleren of het bewerken van profielen voor consumenten werkt zoals geconfigureerd.

> [!NOTE]
> Het duurt maximaal één minuut gebruiker stromen maken en updates van kracht.
>