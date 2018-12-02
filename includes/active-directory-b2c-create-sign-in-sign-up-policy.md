---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: f23d2b02bc2a23c5333a48a50532c03f3aa6a031
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742455"
---
[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Onder **beheren**, selecteer **gebruikersstromen** en klik op +**nieuwe gebruikersstroom**.

![Nieuwe gebruikersstroom selecteren](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-user-flow.png)

Op de **aanbevolen** tabblad **aanmelden en aanmelden**.

![Selecteer aanmelden en meld u aan de gebruikersstroom](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-user-flow-type.png)

Voer een gebruikersstroom **naam** voor uw toepassing om te verwijzen. Geef bijvoorbeeld `SiUpIn` op.

Onder **id-providers** en Controleer **e-mailregistratie**. U kunt er ook voor kiezen om sociale id-providers te selecteren als dit al is geconfigureerd.

Onder **Multifactor-verificatie**, kiest u **ingeschakeld** of **uitgeschakelde**.

![Voer een naam en e-mailregistratie selecteert als een id-provider](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-name-identity-providers.png)

Onder **gebruikerskenmerken en claims**, selecteer **meer weergeven** om te zien van de volledige lijst met kenmerken en claims die u kunt kiezen uit.

In de **verzamelen kenmerk** kolom, kiest u de kenmerken die u wenst te verzamelen van de gebruiker tijdens de registratie. Schakel bijvoorbeeld **Land/regio**, **Weergavenaam** en **Postcode** in.

In de **geretourneerde claim** kolom, kies de claims die u laten retourneren in de autorisatietokens die is verzonden naar de toepassing na een geslaagde registratie- of aanmelden wilt. Selecteer bijvoorbeeld **Weergavenaam**, **Id-provider**, **Postcode**, **Gebruiker is nieuw** en **Object-id van gebruiker**.

Klik op **OK**.

![Selecteer enkele gebruikerskenmerken en claims en klikt u op de knop OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-all-attributes.png)

Klik op **maken** om toe te voegen van de gebruikersstroom. De gebruikersstroom wordt vermeld als **B2C_1_SiUpIn**. Het voorvoegsel **B2C_1_** wordt toegevoegd aan de naam.

Selecteer **gebruikersstroom uitvoeren**. Controleer of de instellingen die zijn opgegeven in de tabel en klik vervolgens op **gebruikersstroom uitvoeren**.

![Selecteer de gebruikersstroom uitvoeren](media/active-directory-b2c-create-sign-in-sign-up-policy/run-user-flow-b2c-signup-signin.png)

| Instelling      | Waarde  |
| ------------ | ------ |
| **Toepassing** | Contoso B2C-app |
| **Antwoord-URL** | `https://localhost:44316/` |

Er wordt een nieuw browsertabblad geopend. Hier kunt u controleren of het registreren of aanmelden voor consumenten werkt zoals geconfigureerd.

> [!NOTE]
> Het duurt maximaal één minuut gebruiker stromen maken en updates van kracht.
>