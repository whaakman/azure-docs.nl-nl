---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 17c0213d63879687e9c6d5f8dca06b9113c44af8
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742560"
---
Als u wilt alleen in te schakelen Meld u aan op uw toepassing, gebruikt u een **aanmelding** gebruikersstroom. Deze gebruikersstroom beschrijft de ervaring van klanten tijdens de registratie via gaan en de inhoud van tokens die de toepassing ontvangt nadat registraties zijn voltooid.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Onder **beheren**, selecteer **gebruikersstromen**.

Klik op +**nieuwe gebruikersstroom** aan de bovenkant van de blade.

Onder **selecteert u een type gebruikersstroom**, selecteer **alle**, en selecteer vervolgens de versie van **aanmelden** u wilt gebruiken.

De **naam** bepaalt de naam van de aanmelding gebruiker-stroom wordt gebruikt door uw toepassing. Voer bijvoorbeeld **SiUp** in.

Onder **id-providers**, selecteer **e-mailregistratie**. U kunt er ook voor kiezen om sociale id-providers te selecteren als dit al is geconfigureerd.

Onder **gebruikerskenmerken en claims**, klikt u op **meer weergeven**.

In de **verzamelen kenmerk** kolom, kies kenmerken die u wilt verzamelen van de gebruiker tijdens de registratie. Selecteer bijvoorbeeld **Land/regio**, **Weergavenaam** en **Postcode**.

In de **geretourneerde claim** kolom, kies de claims die u laten retourneren in de tokens die zijn verzonden naar de toepassing na een geslaagde aanmelding wilt. Selecteer bijvoorbeeld **Weergavenaam**, **Id-provider**, **Postcode**, **Gebruiker is nieuw** en **Object-id van gebruiker**.

Klik op **OK**.

Klik op **Create**. De gebruikersstroom die is gemaakt wordt weergegeven als **B2C_1_SiUp** (de **B2C\_1\_**  wordt automatisch toegevoegd).

Klik op **gebruikersstroom uitvoeren**.

Selecteer **Contoso B2C-app** in de **toepassing** vervolgkeuzelijst en `https://localhost:44321/` in de **antwoord-URL** vervolgkeuzelijst.

Klik op **gebruikersstroom uitvoeren**. Er wordt een nieuw browsertabblad geopend, waar u kunt zien wat de ervaring van consumenten is wanneer ze zich registreren bij de toepassing.

> [!NOTE]
> Het duurt maximaal één minuut gebruiker stromen maken en updates van kracht.
>