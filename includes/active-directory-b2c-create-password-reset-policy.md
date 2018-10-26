---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: bff2543ec48c66c10db697650def0077e3de28be
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133046"
---
Om in te schakelen fijnmazig wachtwoord opnieuw instellen van uw toepassing, gebruikt u een **wachtwoordherstel** beleid. Informatie over de optie voor het opnieuw instellen van het wachtwoord voor de tenant als geheel vindt u [hier](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Dit beleid beschrijft de ervaring van de klanten gaan voor wachtwoord opnieuw instellen en de inhoud van tokens die de toepassing bewerking is voltooid ontvangt.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Ga in Instellingen naar de sectie Beleid, kies de optie **Beleid voor het opnieuw instellen van het wachtwoord** en klik op **+ Toevoegen**.

![Beleid voor registreren of aanmelden selecteren en op de knop Toevoegen klikken](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

Geef een**naam** op voor het beleid waarnaar de toepassing kan verwijzen. Geef bijvoorbeeld `SSPR` op.

Selecteer **Id-providers** en schakel de optie **Het wachtwoord opnieuw instellen met e-mailadres** in. Klik op **OK**.

![De optie Het wachtwoord opnieuw instellen met e-mailadres selecteren als een id-provider en op de knop OK klikken](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Selecteer **Toepassingsclaims**. Kies de claims die u wilt opnemen in de autorisatietokens die worden geretourneerd naar de toepassing wanneer het opnieuw instellen van het wachtwoord is gelukt. Selecteer bijvoorbeeld **Object-ID van gebruiker**.

![Selecteer enkele toepassingsclaims en klik op de knop OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Klik op **Maken** om het beleid toe te voegen. Het beleid wordt vermeld als **B2C_1_SSPR**. Het voorvoegsel **B2C_1_** wordt toegevoegd aan de naam.

Open het beleid door **B2C_1_SSPR** te selecteren. Controleer de instellingen die zijn opgegeven in de tabel en klik vervolgens op **Nu uitvoeren**.

![Selecteer het beleid en voer dit uit](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| Instelling      | Waarde  |
| ------------ | ------ |
| **Toepassingen** | Contoso B2C-app |
| **Antwoord-URL selecteren** | `https://localhost:44316/` |

Er wordt een nieuw browsertabblad geopend. Hier kunt u controleren hoe het opnieuw instellen van het wachtwoord werkt voor consumenten.

> [!NOTE]
> Het duurt maximaal één minuut voordat het gemaakte beleid en de updates van kracht worden.
>
