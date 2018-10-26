---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 511b05e6cae769a5b39ae81a3e67efd05d374511
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133017"
---
Als u wilt alleen in te schakelen Meld u aan op uw toepassing, gebruikt u een **aanmelding** beleid. Dit beleid wordt de ervaring van klanten tijdens de registratie via gaan beschreven en de inhoud van tokens die de toepassing ontvangt nadat registraties zijn voltooid.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Klik op **Registratiebeleid**.

Klik op **+Toevoegen** boven aan de blade.

De **Naam** bepaalt de naam van het registratiebeleid die wordt gebruikt voor de toepassing. Voer bijvoorbeeld **SiUp** in.

Klik op **Id-providers** en selecteer **Registreren met e-mailadres**. U kunt er ook voor kiezen om sociale id-providers te selecteren als dit al is geconfigureerd. Klik op **OK**.

Klik op **Registratiekenmerken**. Hier kiest u de kenmerken van de consument die u wilt verzamelen tijdens de registratie. Selecteer bijvoorbeeld **Land/regio**, **Weergavenaam** en **Postcode**. Klik op **OK**.

Klik op **Toepassingsclaims**. Hier kiest u de claims die u wilt laten retourneren in de tokens die, na een geslaagde registratie, terug worden gestuurd naar de toepassing. Selecteer bijvoorbeeld **Weergavenaam**, **Id-provider**, **Postcode**, **Gebruiker is nieuw** en **Object-id van gebruiker**.

Klik op **Create**. Let op: het beleid dat is gemaakt, wordt weergegeven als **B2C_1_SiIn** (het gedeelte **B2C\_1\_** wordt automatisch toegevoegd) op de blade **Registratiebeleid**.

Open het beleid door op **B2C_1_SiUp** te klikken.

Selecteer **Contoso B2C-app** in de vervolgkeuzelijst **Toepassingen** en `https://localhost:44321/` in de vervolgkeuzelijst **Antwoord-URL / Omleidings-URI**.

Klik op **Nu uitvoeren**. Er wordt een nieuw browsertabblad geopend, waar u kunt zien wat de ervaring van consumenten is wanneer ze zich registreren bij de toepassing.

> [!NOTE]
> Het duurt maximaal één minuut voordat het gemaakte beleid en de updates van kracht worden.
>