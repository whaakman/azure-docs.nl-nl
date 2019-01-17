---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 0ab34d6234db9c13ffe82ccd0e8580217085f631
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355522"
---
Als u wilt zodat alleen aanmelden bij uw toepassing, gebruikt u een **aanmelden** gebruikersstroom. Deze gebruikersstroom beschrijft de ervaring van klanten gaan aanmelden en de inhoud van tokens die de toepassing ontvangt op geslaagde aanmeldingen.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
Onder **beheren**, selecteer **gebruikersstromen**.

Klik op +**nieuwe gebruikersstroom** aan de bovenkant van de blade.

Onder **selecteert u een type gebruikersstroom**, selecteer **alle**, en selecteer vervolgens de versie van **aanmelden** u wilt gebruiken.

De **naam** bepaalt de aanmelding userjourney-naam wordt gebruikt door uw toepassing. Voer bijvoorbeeld **Siln** in.

Onder **id-providers**, selecteer een optie. U kunt ook sociale id-providers selecteren als al geconfigureerd. Klik op **OK**.

Onder **toepassingsclaims**, klikt u op **meer weergeven**.

In de **geretourneerde claim** kolom, kies de claims die u laten retourneren in de tokens die zijn verzonden naar de toepassing na een geslaagde aanmelding wilt. Selecteer bijvoorbeeld **Weergavenaam**, **Id-provider**, **Postcode** en **Object-id van gebruiker**. Klik op **OK**.

Klik op **Create**. Houd er rekening mee dat de zojuist gemaakte beleid wordt weergegeven als **B2C_1_SiIn** (de **B2C\_1\_**  wordt automatisch toegevoegd).

Klik op **gebruikersstroom uitvoeren**.

Selecteer **Contoso B2C-app** in de **toepassing** vervolgkeuzelijst en `https://localhost:44321/` in de **antwoord-URL** vervolgkeuzelijst.

Klik op **gebruikersstroom uitvoeren**. Er wordt een nieuw browsertabblad geopend, waar u kunt zien wat de ervaring van consumenten is wanneer ze zich aanmelden bij de toepassing.

> [!NOTE]
> Het duurt maximaal één minuut gebruiker stromen maken en updates van kracht.
>