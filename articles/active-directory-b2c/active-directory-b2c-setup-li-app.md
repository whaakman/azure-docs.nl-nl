---
title: Instellen van zich kunnen registreren en aanmelden met een LinkedIn-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met LinkedIn-accounts in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 8388baf88f5bb723e5b0e47bc93b100d5ce8e3e2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159794"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een LinkedIn-account met behulp van Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Een LinkedIn-toepassing maken

Voor het gebruik van een LinkedIn-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een LinkedIn-account hebt, kunt u krijgen via [ https://www.linkedin.com/ ](https://www.linkedin.com/).

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

