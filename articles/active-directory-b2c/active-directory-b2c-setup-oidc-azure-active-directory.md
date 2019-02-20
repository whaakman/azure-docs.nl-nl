---
title: Instellen van aanmelden voor een Azure Active Directory-organisatie - Azure Active Directory B2C | Microsoft Docs
description: Instellen van de aanmelding voor een specifieke Azure Active Directory-organisatie in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 0da27c5ce872d1b4e1b4c63f6f4207bb5ca4d6ef
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428071"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Aanmelden voor een specifieke Azure Active Directory-organisatie in Azure Active Directory B2C instellen

>[!NOTE]
> Deze functie is beschikbaar voor openbare preview. Gebruik de functie niet in een productieomgeving.

Een Azure Active Directory (Azure AD) gebruiken als een [id-provider](active-directory-b2c-reference-oauth-code.md) in Azure AD B2C, moet u een toepassing maakt die aangeeft. Dit artikel leest u hoe u aanmelding voor gebruikers van een specifieke Azure Active Directory inschakelen voor flow organisatie met behulp van een gebruiker in Azure AD B2C.

## <a name="create-an-azure-ad-app"></a>Een Azure AD-app maken

Om in te schakelen aanmelding voor gebruikers van een specifieke Azure AD-organisatie, moet u een toepassing registreren met de organisatie Azure AD-tenant die niet gelijk zijn aan uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD-tenant door te klikken op de map- en abonnementsfilter in het bovenste menu en de map met uw Azure AD-tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **App-registraties**.
4. Selecteer **Nieuwe toepassing registreren**.
5. Voer een naam in voor de toepassing. Bijvoorbeeld `Azure AD B2C App`.
6. Voor de **toepassingstype**, selecteer `Web app / API`.
7. Voor de **aanmeldings-URL**, voer de volgende URL in kleine letters, waarbij `your-B2C-tenant-name` wordt vervangen door de naam van uw Azure AD B2C-tenant. Bijvoorbeeld, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Alle URL's moeten nu gebruikmaken van [b2clogin.com](b2clogin.md).

8. Klik op **Create**. Kopieer de **toepassings-ID** moet later worden gebruikt.
9. Selecteer de toepassing en selecteer vervolgens **instellingen**.
10. Selecteer **sleutels**, geef de beschrijving van de sleutel, selecteer een tijdsduur en klik vervolgens op **opslaan**. Kopieer de waarde van de sleutel die wordt weergegeven voor later gebruik.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Azure AD als id-provider configureren

1. Zorg ervoor dat u de map met Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map met uw Azure AD B2C-tenant te kiezen.
2. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
3. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
4. Voer een **naam**. Voer bijvoorbeeld 'Contoso Azure AD'.
5. Selecteer **type id-provider**, selecteer **Open ID Connect (Preview)**, en klik vervolgens op **OK**.
6. Klik op **deze id-provider instellen**
7. Voor **metagegevens-url**, voer de volgende URL vervangt `your-AD-tenant-domain` met de domeinnaam van uw Azure AD-tenant. Bijvoorbeeld `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

8. Voor **Client-id**, voer de toepassings-ID die u eerder hebt genoteerd en voor **clientgeheim**, voer de waarde van de sleutel die u eerder hebt genoteerd.
9. Geef eventueel een waarde voor **Domain_hint**. Bijvoorbeeld `ContosoAD`. Dit is de waarde moet worden gebruikt als verwijzingen naar deze id-provider met behulp van *domain_hint* in de aanvraag. 
10. Klik op **OK**.
11. Selecteer **deze id-provider claims toewijzen** en stel de volgende claims:
    
    - Voor **gebruikers-ID**, voer `oid`.
    - Voor **weergavenaam**, voer `name`.
    - Voor **voornaam**, voer `given_name`.
    - Voor **achternaam**, voer `family_name`.
    - Voor **e**, voer `unique_name`.

12. Klik op **OK**, en klik vervolgens op **maken** aan uw configuratie op te slaan.
