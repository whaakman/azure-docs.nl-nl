---
title: Instellen van zich kunnen registreren en aanmelden met een Google-account - Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Google-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 1d4f7460d53e721ca1cba7aba6c6ef9c45111ee5
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58435437"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Google-account met behulp van Azure Active Directory B2C

## <a name="create-a-google-application"></a>Een Google-toepassing maken

Gebruik van een Google-account als een [id-provider](active-directory-b2c-reference-oauth-code.md) in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Google-account hebt kunt u krijgen via [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Aanmelden bij de [Google ontwikkelaars Console](https://console.developers.google.com/) met de referenties van uw Google-account.
2. In de linkerbovenhoek van de pagina, selecteert u de projectlijst en selecteer vervolgens **nieuw Project**.
3. Voer een **projectnaam**, klikt u op **maken**, en controleer vervolgens of u gebruikmaakt van het nieuwe project.
4. Selecteer **referenties** in het menu aan de linkerkant en selecteer vervolgens **referenties maken** > **Oauth-Clientidentiteit**.
5. Onder **toepassingstype**, selecteer **webtoepassing**.
6. Voer een **naam** voor uw toepassing, voert u `https://your-tenant-name.b2clogin.com` in **geautoriseerd JavaScript oorsprongen**, en `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **geautoriseerde omleidings-URI's**. Vervang `your-tenant-name` met de naam van uw tenant. U moet alle kleine letters gebruiken bij het invoeren van de tenantnaam van uw, zelfs als de tenant is gedefinieerd met behulp van hoofdletters in Azure AD B2C.
7. Klik op **Create**.
8. Kopieer de waarden van **Client-ID** en **clientgeheim**. U moet beide Google configureren als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Een Google-account configureren als een id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Voer een **naam**. Voer bijvoorbeeld *Google*.
6. Selecteer **type id-provider**, selecteer **Google**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de Client-ID die u eerder hebt genoteerd als de **Client-ID** en voer het Clientgeheim die u hebt genoteerd als de **clientgeheim**van de Google-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** aan uw Google-configuratie op te slaan.

