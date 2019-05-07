---
title: Web-app die zich aanmeldt gebruikers (app-registratie) - Microsoft identity-platform
description: Informatie over het bouwen van een web-app die gebruikers (app-registratie) worden aangemeld
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6f512911811535818f4ad857c5c3b956870f619
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074543"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>Web-app die zich in gebruikers - app-registratie

Deze pagina leest u de details van de app-registratie voor een web-app waarmee gebruikers zich aanmeldt.

Voor het registreren van uw toepassing, kunt u het volgende gebruiken:

- De [web app QuickStart](#register-an-app-using-the-quickstarts) -naast een goede eerste ervaring met het maken van een toepassing, snelstartgidsen in Azure portal een knop met de naam bevatten **deze wijziging hebt aangebracht voor mij**. Deze knop kunt u de eigenschappen die u nodig, zelfs voor een bestaande app hebt instellen. U moet de waarden van deze eigenschappen aan uw eigen aanvraag aan te passen. In het bijzonder gaat de web-API-URL voor uw app waarschijnlijk afwijken van de voorgestelde standaardoptie, hierbij wordt ook van invloed op de afmeldings-URI.
- De Azure portal om te [handmatig uw toepassing registreren](#register-an-app-using-azure-portal)
- PowerShell en de opdrachtregelprogramma 's

## <a name="register-an-app-using-the-quickstarts"></a>Een app met behulp van de snelstartgidsen registreren

Als u deze koppeling navigeren, kunt u maken bootstrap het maken van uw webtoepassing:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

### <a name="register-an-app-using-azure-portal"></a>Registreren van een app met behulp van Azure portal

> [!NOTE]
> de portal te gebruiken is afhankelijk van verschillende als uw toepassing wordt uitgevoerd in de openbare cloud van Microsoft Azure of in een nationale of onafhankelijke cloud. Zie voor meer informatie, [nationale Clouds](./authentication-national-cloud.md#app-registration-endpoints)

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account. U kunt ook aanmelden bij de nationale cloud-Azure portal naar keuze.
1. Als uw account hebt u toegang tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en instellen van uw portal-sessie op de gewenste Azure AD-tenant.
1. Selecteer in het navigatiedeelvenster links het **Azure Active Directory** service en selecteer vervolgens **App-registraties** > **registratie van nieuwe**.
1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
   - Kies de ondersteunde typen voor uw toepassing (Zie [ondersteund accounttypen](./v2-supported-account-types.md))
   - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `AspNetCore-WebApp`.
   - In **antwoord-URL**, toevoegen de antwoord-URL voor uw app, bijvoorbeeld `https://localhost:44321/`, en selecteer **registreren**.
1. Selecteer het menu **Verificatie** en voeg dan de volgende gegevens toe:
- Bij **Antwoord-URL** voegt u `https://localhost:44321/signin-oidc` toe en selecteert u **Registreren**.
- In de **geavanceerde instellingen** sectie, stelt u de **afmeldings-URL** naar `https://localhost:44321/signout-oidc`.
- Bij **Impliciete toekenning** controleert u de **id-tokens**.
- Selecteer **Opslaan**.

### <a name="register-an-app-using-powershell"></a>Registreren van een app met behulp van PowerShell

> [!NOTE]
> Azure AD PowerShell worden momenteel alleen de toepassingen gemaakt met de volgende ondersteunde typen:
>
> - MijnOrg (Accounts in deze organisatie-map alleen)
> - AnyOrg (Accounts in een organisatie-map).
>
> Als u wilt maken van een toepassing waarmee gebruikers zich aanmeldt met hun persoonlijke Microsoft-Accounts (zoals Skype, XBox, Outlook.com), kunt u eerst een toepassing met meerdere tenants maken (ondersteund accounttypen = Accounts in een organisatie-map), en wijzig vervolgens de de `signInAudience` eigenschap in het toepassingsmanifest vanuit Azure portal. Dit wordt uitgelegd in de details in de stap [1.3](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) van de zelfstudie voor ASP.NET Core (en kan worden gebruikt op web-apps in elke taal).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De configuratie van App-code](scenario-web-app-sign-user-app-configuration.md)
