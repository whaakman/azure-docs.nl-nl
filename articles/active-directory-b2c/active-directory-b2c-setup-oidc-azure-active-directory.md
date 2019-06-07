---
title: Instellen van aanmelden voor een Azure Active Directory-organisatie - Azure Active Directory B2C | Microsoft Docs
description: Instellen van de aanmelding voor een specifieke Azure Active Directory-organisatie in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 28dbf0382ac151857e72d4bb59e207f07c8ad3f3
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66508427"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Aanmelden voor een specifieke Azure Active Directory-organisatie in Azure Active Directory B2C instellen

>[!NOTE]
> Deze functie is beschikbaar voor openbare preview. Gebruik de functie niet in een productieomgeving.

Een Azure Active Directory (Azure AD) gebruiken als een [id-provider](active-directory-b2c-reference-oauth-code.md) in Azure AD B2C, moet u een toepassing maakt die aangeeft. Dit artikel leest u hoe u aanmelding voor gebruikers van een specifieke Azure Active Directory inschakelen voor flow organisatie met behulp van een gebruiker in Azure AD B2C.

## <a name="create-an-azure-ad-app"></a>Een Azure AD-app maken

Om in te schakelen aanmelding voor gebruikers van een specifieke Azure AD-organisatie, moet u een toepassing registreren met de organisatie Azure AD-tenant die niet gelijk zijn aan uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD-tenant. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map met uw Azure AD-tenant. Dit is niet dezelfde tenant als uw Azure AD B2C-tenant.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **App-registraties**.
4. Selecteer **registratie van nieuwe**.
5. Voer een naam in voor de toepassing. Bijvoorbeeld `Azure AD B2C App`.
6. Accepteer de selectie van **Accounts in deze organisatie-map alleen** voor deze toepassing.
7. Voor de **omleidings-URI**, accepteert u de waarde van **Web**, en voer de volgende URL in kleine letters, waarbij `your-B2C-tenant-name` wordt vervangen door de naam van uw Azure AD B2C-tenant. Bijvoorbeeld, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your--B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Alle URL's moeten nu gebruikmaken van [b2clogin.com](b2clogin.md).

8. Klik op **registreren**. Kopieer de **(client) toepassings-ID** moet later worden gebruikt.
9. Selecteer **certificaten en geheimen** in het toepassingsmenu en selecteer vervolgens **nieuwe clientgeheim**.
10. Voer een naam voor het clientgeheim. Bijvoorbeeld `Azure AD B2C App Secret`.
11. Selecteer de verloopperiode. Voor deze toepassing, accepteert u de selectie van **In 1 jaar**.
12. Selecteer **toevoegen** en kopieer de waarde van het nieuwe clientgeheim dat wordt weergegeven voor later gebruik.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Azure AD als id-provider configureren

1. Zorg ervoor dat u de map met Azure AD B2C-tenant. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map met uw Azure AD B2C-tenant.
2. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
3. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
4. Voer een **naam**. Geef bijvoorbeeld `Contoso Azure AD` op.
5. Selecteer **type id-provider**, selecteer **Open ID Connect (Preview)** , en klik vervolgens op **OK**.
6. Selecteer **deze id-provider instellen**
7. Voor **metagegevens-url**, voer de volgende URL vervangt `your-AD-tenant-domain` met de domeinnaam van uw Azure AD-tenant. Bijvoorbeeld `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

8. Voor **Client-ID**, voer de toepassings-ID die u eerder hebt genoteerd en voor **clientgeheim**, voer het clientgeheim die u eerder hebt genoteerd.
9. Geef eventueel een waarde voor **Domain_hint**. Bijvoorbeeld `ContosoAD`. Dit is de waarde moet worden gebruikt als verwijzingen naar deze id-provider met behulp van *domain_hint* in de aanvraag. 
10. Klik op **OK**.
11. Selecteer **deze id-provider claims toewijzen** en stel de volgende claims:
    
    - Voor **gebruikers-ID**, voer `oid`.
    - Voor **weergavenaam**, voer `name`.
    - Voor **voornaam**, voer `given_name`.
    - Voor **achternaam**, voer `family_name`.
    - Voor **e**, voer `unique_name`.

12. Klik op **OK**, en klik vervolgens op **maken** aan uw configuratie op te slaan.
