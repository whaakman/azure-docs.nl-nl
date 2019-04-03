---
title: Instellen van zich kunnen registreren en aanmelden met een account LinkedIn - Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met LinkedIn-accounts in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: de58f960842e0a4f8e9b964774ce62b3e2772113
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58863169"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een LinkedIn-account met behulp van Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Een LinkedIn-toepassing maken

Gebruik een LinkedIn-account als een [id-provider](active-directory-b2c-reference-oauth-code.md) in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een LinkedIn-account hebt, kunt u krijgen via [ https://www.linkedin.com/ ](https://www.linkedin.com/).

1. Aanmelden bij de [LinkedIn ontwikkelaars website](https://www.developer.linkedin.com/) met de referenties van uw LinkedIn-account.
2. Selecteer **mijn Apps**, en klik vervolgens op **toepassing maken**.
3. Voer **bedrijfsnaam**, **toepassingsnaam**, **Toepassingsbeschrijving**, **toepassing Logo**, **toepassing gebruiken** , **Website-URL**, **zakelijke e-mailadres**, en **telefoon**.
4. Ga akkoord met de **LinkedIn gebruiksvoorwaarden API** en klikt u op **indienen**.
5. Kopieer de waarden van **Client-ID** en **Clientgeheim**. U vindt deze onder **verificatiesleutels**. U moet beide LinkedIn configureren als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.
6. Voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **Omleidings-URL's toegestaan**. Vervang `your-tenant-name` met de naam van uw tenant. U moet alle kleine letters gebruiken bij het invoeren van de tenantnaam van uw, zelfs als de tenant is gedefinieerd met behulp van hoofdletters in Azure AD B2C. Selecteer **toevoegen**, en klik vervolgens op **Update**.

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Een LinkedIn-account configureren als een id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *LinkedIn*.
6. Selecteer **type id-provider**, selecteer **LinkedIn**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de Client-Id die u eerder hebt genoteerd als de **Client-ID** en voer het Clientgeheim die u hebt genoteerd als de **clientgeheim**van de LinkedIn-account-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de configuratie van uw LinkedIn-account.

## <a name="migration-from-v10-to-v20"></a>Migratie van v1.0 naar v2.0

Onlangs LinkedIn [bijgewerkt van hun API's van v1.0 naar v2.0](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Als onderdeel van de migratie kan Azure AD B2C alleen verkrijgen van de volledige naam van de gebruiker LinkedIn tijdens de aanmelding. Als een e-mailadres is een van de kenmerken die worden verzameld tijdens het aanmelden, de gebruiker moet handmatig invoeren van het e-mailadres en te valideren.
