---
title: Instellen van zich kunnen registreren en aanmelden met een Microsoft-account - Azure Active Directory B2C
description: Meld u aan en meld u bieden aan klanten met een Microsoft-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 82c1be335bfd39d641f0203116e68a4cb4c0a674
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67654213"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Microsoft-account met behulp van Azure Active Directory B2C

## <a name="create-a-microsoft-account-application"></a>Een toepassing van Microsoft-account maken

Gebruik van een Microsoft-account als een [id-provider](active-directory-b2c-reference-oidc.md) in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de Azure AD-tenant. De Azure AD-tenant is niet gelijk zijn aan uw Azure AD B2C-tenant. Als u nog een Microsoft-account hebt, krijgt u één voor één [ https://www.live.com/ ](https://www.live.com/).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zorg ervoor dat u de map met uw Azure AD-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map met uw Azure AD-tenant te kiezen.
1. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **App-registraties**.
1. Selecteer **registratie van nieuwe**.
1. Voer een **naam** voor uw toepassing. Bijvoorbeeld, *MSAapp1*.
1. Onder **ondersteund accounttypen**, selecteer **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts (zoals Skype, Xbox, Outlook.com)** . Deze optie is bedoeld voor de breedste reeks identiteiten van Microsoft.

   Zie voor meer informatie over het selecteren van het type ander account [Quick Start: Een toepassing registreren met het Microsoft identity-platform](../active-directory/develop/quickstart-register-app.md).
1. Onder **omleidings-URI (optioneel)** , selecteer **Web** en voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in het tekstvak in. Vervang `your-tenant-name` met de naam van uw Azure AD B2C-tenant.
1. Selecteer **registreren**
1. Record de **(client) toepassings-ID** weergegeven op de overzichtspagina van de toepassing. U hebt deze nodig wanneer u de id-provider in de volgende sectie configureren.
1. Selecteer **certificaten en geheimen**
1. Klik op **nieuwe clientgeheim**
1. Voer een **beschrijving** voor het geheim, bijvoorbeeld *toepassingswachtwoord 1*, en klik vervolgens op **toevoegen**.
1. Noteer het wachtwoord wordt weergegeven in de **waarde** kolom. U hebt deze nodig wanneer u de id-provider in de volgende sectie configureren.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Een Microsoft-account configureren als een id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
1. Geef een **naam**. Voer bijvoorbeeld *MSA*.
1. Selecteer **type id-provider**, selecteer **Microsoft-Account**, en klikt u op **OK**.
1. Selecteer **instellen van deze id-provider** en voer de toepassing (client)-ID die u hebt genoteerd eerder in de **Client-ID** tekst in en voer het clientgeheim die u hebt genoteerd in de **Client geheim** in het tekstvak.
1. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de configuratie van uw Microsoft-account.
