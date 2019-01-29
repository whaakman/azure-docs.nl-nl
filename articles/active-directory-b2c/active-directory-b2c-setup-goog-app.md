---
title: Instellen van zich kunnen registreren en aanmelden met een Google-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Google-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: af8a727bf9733b744b1ed429420cd8fde6e3e6f1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176128"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Google-account met behulp van Azure Active Directory B2C

## <a name="create-a-google-application"></a>Een Google-toepassing maken

Voor het gebruik van een Google-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Google-account hebt kunt u krijgen via [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Aanmelden bij de [Google ontwikkelaars Console](https://console.developers.google.com/) met de referenties van uw Google-account.
2. Selecteer **project maken**, en klik vervolgens op **maken**. Als u projecten voordat u hebt gemaakt, selecteert u de projectlijst en selecteer vervolgens **nieuw Project**.
3. Voer een **projectnaam**, klikt u op **maken**, en controleer vervolgens of u gebruikmaakt van het nieuwe project.
3. Selecteer **referenties** in het menu aan de linkerkant en selecteer vervolgens **referenties maken** > **Oauth-Clientidentiteit**.
4. Selecteer **configureren instemmingsscherm**.
5. Selecteer of geef een geldige **e-mailadres**, bieden een **productnaam weergegeven voor gebruikers**, toevoegen `b2clogin.com` naar **gemachtigde domeinen**, en klikt u op **opslaan** .
6. Onder **toepassingstype**, selecteer **webtoepassing**.
7. Voer een **naam** voor uw toepassing, voert u `https://your-tenant-name.b2clogin.com` in **geautoriseerd JavaScript oorsprongen**, en `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **geautoriseerde omleidings-URI's**. Vervang `your-tenant-name` met de naam van uw tenant. U moet alle kleine letters gebruiken bij het invoeren van de tenantnaam van uw, zelfs als de tenant is gedefinieerd met behulp van hoofdletters in Azure AD B2C.
8. Klik op **Create**.
9. Kopieer de waarden van **Client-ID** en **clientgeheim**. U moet beide Google configureren als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Een Google-account configureren als een id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Voer een **naam**. Voer bijvoorbeeld *Google*.
6. Selecteer **type id-provider**, selecteer **Google**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de Client-ID die u eerder hebt genoteerd als de **Client-ID** en voer het Clientgeheim die u hebt genoteerd als de **clientgeheim**van de Google-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** aan uw Google-configuratie op te slaan.

