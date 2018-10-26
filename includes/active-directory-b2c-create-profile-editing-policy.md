---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 9ad715f47f2de9c6f9032ed07232f45fb33b0114
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132959"
---
Als u inschakelen met het bewerken van profielen van uw toepassing wilt, gebruikt u een **bewerken van profielen** beleid. Dit beleid wordt de ervaring van klanten tijdens het bewerken van profielen en de inhoud van tokens die de toepassing bewerking is voltooid ontvangt loopt, via beschreven.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Selecteer bij instellingen in de sectie Beleid de optie **Beleid voor profielbewerking** en klik op **+Toevoegen**.

![Selecteer Beleid voor profielbewerking en klik op de knop Toevoegen](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

Geef een **naam** op voor het beleid waarnaar de toepassing kan verwijzen. Geef bijvoorbeeld `SiPe` op.

Selecteer **Id-providers** en schakel **Aanmelden met lokaal account** in. U kunt er ook voor kiezen om sociale id-providers te selecteren als dit al is geconfigureerd. Klik op **OK**.

![Selecteer Aanmelden met lokaal account als id-provider en klik op de knop OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

Selecteer **Profielkenmerken**. Kies kenmerken die de consument kan bekijken, en bewerk dit in het bijbehorende profiel. Schakel bijvoorbeeld **Land/regio**, **Weergavenaam** en **Postcode** in. Klik op **OK**.

![Selecteer enkele kenmerken en klik op de knop OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

Selecteer **Toepassingsclaims**. Kies de claims die u wilt laten retourneren in de autorisatietokens die, na een geslaagde profielbewerking, terug worden gestuurd naar de toepassing. Selecteer bijvoorbeeld **Weergavenaam**, **Postcode**.

![Selecteer enkele toepassingsclaims en klik op de knop OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

Klik op **Maken** om het beleid toe te voegen. Het beleid wordt vermeld als **B2C_1_SiPe**. Het voorvoegsel **B2C_1_** wordt toegevoegd aan de naam.

Open het beleid door **B2C_1_SiPe** te selecteren. Controleer de instellingen die zijn opgegeven in de tabel en klik vervolgens op **Nu uitvoeren**.

![Selecteer het beleid en voer dit uit](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| Instelling      | Waarde  |
| ------------ | ------ |
| **Toepassingen** | Contoso B2C-app |
| **Antwoord-URL selecteren** | `https://localhost:44316/` |

Er wordt een nieuw browsertabblad geopend. Hier kunt u controleren of het bewerken van profielen voor consumenten werkt zoals geconfigureerd.

> [!NOTE]
> Het duurt maximaal één minuut voordat het gemaakte beleid en de updates van kracht worden.
>