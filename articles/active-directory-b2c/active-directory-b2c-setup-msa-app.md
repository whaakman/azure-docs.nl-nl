---
title: Instellen van zich kunnen registreren en aanmelden met een Microsoft-account - Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Microsoft-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 13e199a56a3cdd4f8e5a21f162fe0397c6f397cd
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428252"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Microsoft-account met behulp van Azure Active Directory B2C

## <a name="create-a-microsoft-account-application"></a>Een toepassing van Microsoft-account maken

Gebruik van een Microsoft-account als een [id-provider](active-directory-b2c-reference-oidc.md) in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Microsoft-account hebt, kunt u krijgen via [ https://www.live.com/ ](https://www.live.com/).

1. Aanmelden bij de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) met uw Microsoft-accountreferenties.
2. Selecteer in de rechterbovenhoek **een app toevoegen**.
3. Voer een **naam** voor uw toepassing. Bijvoorbeeld, *MSAapp1*.
4. Selecteer **nieuw wachtwoord genereren** en zorg ervoor dat u het wachtwoord moet worden gebruikt wanneer u de id-provider configureren. Ook kopiëren de **toepassings-Id**. 
5. Selecteer **toevoegen platform**, en vervolgens en kies **Web**.
4. Voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **Omleidings-URL's**. Vervang `your-tenant-name` met de naam van uw tenant.
5. Selecteer **Opslaan**.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Een Microsoft-account configureren als een id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *MSA*.
6. Selecteer **type id-provider**, selecteer **Microsoft-Account**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voer de toepassings-Id die u eerder hebt genoteerd als de **Client-ID** en voer het wachtwoord die u hebt genoteerd als de **clientgeheim**van de Microsoft-account-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de configuratie van uw Microsoft-account.

