---
title: Instellen van zich kunnen registreren en aanmelden met een Facebook-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Facebook-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 855b43c39ac8bc3a1b6bb818a9770897e983c2ea
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54843596"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Facebook-account met behulp van Azure Active Directory B2C

## <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken

Voor het gebruik van een Facebook-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Facebook-account hebt, kunt u krijgen via [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Aanmelden bij [Facebook voor ontwikkelaars](https://developers.facebook.com/) met de referenties van uw Facebook-account.
2. Als u hebt nog niet gedaan, moet u registreren als een Facebook-ontwikkelaar. Om dit te doen, selecteert u **registreren** in de rechterbovenhoek van de pagina accepteren van Facebook-beleid en de registratie van stappen.
3. Selecteer **mijn Apps** en klik vervolgens op **toevoegen van een nieuwe App**. 
4. Voer een **weergavenaam** en een geldig **Neem contact op met e-mailadres**.
5. Klik op **maken van App-ID**. Mogelijk moet u beleid voor het platform van Facebook accepteren en een online beveiliging voltooien.
6. Selecteer **instellingen** > **Basic**.
7. Kies een **categorie**, bijvoorbeeld `Business and Pages`. Deze waarde is vereist voor Facebook, maar niet voor Azure AD B2C gebruikt.
8. Selecteer aan de onderkant van de pagina **Platform toevoegen**, en selecteer vervolgens **Website**.
9. In **Site-URL**, voer `https://your-tenant-name.b2clogin.com/` vervangen `your-tenant-name` met de naam van uw tenant. Voer een URL voor de **URL privacybeleid**, bijvoorbeeld `http://www.contoso.com`. De beleids-URL is een pagina die u moet onderhouden voor privacy-informatie voor uw toepassing.
10. Selecteer **wijzigingen opslaan**.
11. Aan de bovenkant van de pagina, Kopieer de waarde van **App-ID**. 
12. Klik op **weergeven** en kopieer de waarde van **Appgeheim**. U kunt van beide Facebook als id-provider configureren in uw tenant. **App-geheim** is een belangrijke beveiligingsreferentie.
13. Selecteer **producten**, en selecteer vervolgens **instellen** onder **aanmelden via Facebook**.
14. Selecteer **instellingen** onder **aanmelden via Facebook**.
15. In **geldig OAuth omleidings-URI's**, voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Vervang `your-tenant-name` met de naam van uw tenant. Klik op **wijzigingen opslaan** aan de onderkant van de pagina.
16. Als u uw Facebook-toepassing naar Azure AD B2C, klikt u op de kiezer Status aan de bovenkant van de pagina en schakel het **op** openbaar maken van de toepassing en klik vervolgens op **bevestigen**.  Op dit moment de Status te wijzigen van **ontwikkeling** naar **Live**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configureren van een Facebook-account als id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen. 
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Voer een **naam**. Voer bijvoorbeeld *Facebook*.
6. Selecteer **type id-provider**, selecteer **Facebook**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de App-ID die u eerder hebt genoteerd als de **Client-ID** en voert u de App-geheim die u hebt genoteerd als de **clientgeheim** van de Facebook-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** op uw Facebook-configuratie op te slaan.
