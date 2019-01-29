---
title: Instellen van zich kunnen registreren en aanmelden met een GitHub-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een GitHub-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 60fd7ec53681f9b96970d8a33e5610115e579615
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166081"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een GitHub-account met behulp van Azure Active Directory B2C

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

Voor het gebruik van een GitHub-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een GitHub-account hebt, kunt u krijgen via [ https://www.github.com/ ](https://www.github.com/).

## <a name="create-a-github-oauth-application"></a>Een GitHub-OAuth-toepassing maken

1. Aanmelden bij de [GitHub Developer](https://github.com/settings/developers) -website met uw GitHub-referenties.
2. Selecteer **OAuth Apps** en selecteer vervolgens **nieuwe OAuth-App**.
3. Voer een **toepassingsnaam** en uw **URL van de startpagina**.
4. Voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **URL voor terugbellen voor autorisatie**. Vervang `your-tenant-name` met de naam van uw Azure AD B2C-tenant. Gebruik alleen kleine letters bij het invoeren van de tenantnaam van uw, zelfs als de tenant is gedefinieerd met behulp van hoofdletters in Azure AD B2C.
5. Klik op **toepassing registreren**.
6. Kopieer de waarden van **Client-ID** en **Clientgeheim**. U moet zowel de id-provider toevoegen aan uw tenant.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Een GitHub-account configureren als een id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *GitHub*.
6. Selecteer **type id-provider**, selecteer **GitHub (Preview)**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de Client-Id die u eerder hebt genoteerd als de **Client-ID** en voer het Clientgeheim die u hebt genoteerd als de **clientgeheim**van de GitHub-account-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de configuratie van uw GitHub-account.
