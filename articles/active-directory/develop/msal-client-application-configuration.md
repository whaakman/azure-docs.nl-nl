---
title: Configuratie van client-toepassing (Microsoft Authentication Library) | Azure
description: Meer informatie over de configuratieopties voor openbare-client en vertrouwelijke client toepassingen in de Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5e175a8cdd1622add90bd80df63303fe914ab9c
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430805"
---
# <a name="application-configuration-options"></a>Opties voor toepassingsconfiguraties

In uw code kunt u een nieuwe openbare of vertrouwelijke client (of gebruikersagent voor MSAL.js) initialiseren toepassing om te verifiëren en verkrijgen van tokens. U kunt een aantal configuratieopties instellen wanneer u de client-app in Microsoft Authentication Library (MSAL) initialiseren. Deze opties kunnen worden onderverdeeld in twee groepen:

- Opties voor registratie, met inbegrip van:
    - [Instantie](#authority) (bestaat uit de id-provider [exemplaar](#cloud-instance) en meld u [doelgroep](#application-audience) voor de app en mogelijk de tenant-ID).
    - [Client-ID](#client-id).
    - [Omleidings-URI](#redirect-uri).
    - [clientgeheim](#client-secret) (voor toepassingen van vertrouwelijke client).
- [Opties voor logboekregistratie](#logging), met inbegrip van logboek-niveau, beheer van persoonlijke gegevens en de naam van het onderdeel met behulp van de bibliotheek.

## <a name="authority"></a>Instantie
De instantie is een URL die een map met MSAL tokens van vragen kan aangeeft. Algemene instanties zijn:

- https://login.microsoftonline.com/&lt; tenant&gt;/ waar &lt;tenant&gt; is de tenant-ID van de tenant Azure Active Directory (Azure AD) of een domein dat is gekoppeld aan deze Azure AD-tenant. Alleen aan te melden bij de gebruikers van een specifieke organisatie gebruikt.
- https://login.microsoftonline.com/common/. Gebruikt voor het aanmelden van gebruikers met werk en schoolaccounts of persoonlijke Microsoft-accounts.
- https://login.microsoftonline.com/organizations/. Gebruikt voor het aanmelden van gebruikers met een werk-en schoolaccounts.
- https://login.microsoftonline.com/consumers/. Gebruikt voor het aanmelden van gebruikers met alleen persoonlijke Microsoft-accounts (voorheen bekend als Windows Live ID-accounts).

De instelling van de instantie moet consistent zijn met wat in de portal voor appregistratie gedeclareerd.

URL van de instantie van het exemplaar en de doelgroep bestaat.

De instantie kan zijn:
- Een Azure AD-cloud-instantie.
- een Azure AD B2C-instantie. Zie [B2C specificaties](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics).
- Een Active Directory Federation Services (ADFS)-instantie. Zie [ondersteuning voor AD FS](https://aka.ms/msal-net-adfs-support).

Instanties van de cloud Azure AD heeft twee onderdelen:
- de id-provider *exemplaar*
- De aanmelding *doelgroep* voor de app.

Het exemplaar en de doelgroep kunnen worden samengevoegd en geleverd als URL van de instantie. In eerdere versies van MSAL.NET dan MSAL 3. *x*, u moest de instantie zelf samenstellen, op basis van de cloud u wilde doel en de doelgroep aanmelden.  In dit diagram ziet u hoe de URL van de instantie is samengesteld:

![Hoe de URL van de instantie wordt samengesteld](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Cloud-exemplaar
De *exemplaar* wordt gebruikt om op te geven als uw app is worden gebruikt voor het aanmelden van gebruikers van de openbare cloud van Azure of van nationale clouds. Met MSAL in uw code, kunt u de Azure-cloud-instantie instellen met behulp van een opsomming of door de URL van de [nationale cloudinstantie](authentication-national-cloud.md#azure-ad-authentication-endpoints) als de `Instance` lid (als u deze kent).

MSAL.NET genereert een expliciete uitzondering als beide `Instance` en `AzureCloudInstance` zijn opgegeven.

Als u een exemplaar niet opgeeft, uw app heeft betrekking op de openbare cloud van Azure-instantie (het exemplaar van de URL `https://login.onmicrosoftonline.com`).

## <a name="application-audience"></a>Doelgroep van toepassing

De doelgroep aanmelden, is afhankelijk van de zakelijke behoeften voor uw app:
- Als u een line-of-business (LOB)-ontwikkelaar bent, zult u waarschijnlijk een toepassing met één tenant die wordt gebruikt in uw organisatie alleen produceren. In dat geval moet u de organisatie, door de tenant-ID (de ID van uw exemplaar van de Azure AD) of een domeinnaam die is gekoppeld aan de Azure AD-exemplaar opgeven.
- Als u een ISV bent, is het raadzaam aan te melden bij de gebruikers met hun werk- en schoolaccounts accounts in elke organisatie of in sommige organisaties (multitenant-app). Maar mogelijk wilt u ook dat gebruikers zich aanmelden met hun persoonlijke Microsoft-account.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Het opgeven van de doelgroep in uw code/configuratie
Met MSAL in uw code, opgeven u de doelgroep met behulp van een van de volgende waarden:
- De opsomming van Azure AD-instantie doelgroep
- De tenant-ID, die kan zijn:
  - Een GUID (de ID van uw Azure AD-exemplaar) voor toepassingen met één tenant
  - De domeinnaam van een is gekoppeld aan uw Azure AD-exemplaar (ook voor toepassingen met één tenant)
- Een van deze tijdelijke aanduidingen als een tenant-ID in plaats van de opsomming van Azure AD-instantie doelgroep:
    - `organizations` voor een multitenant-toepassing
    - `consumers` aan te melden bij gebruikers alleen met hun persoonlijke account
    - `common` aanmelden van gebruikers met hun werk en schoolaccounts of hun persoonlijke Microsoft-account

MSAL een zinvolle uitzondering genereert als u zowel de doelgroep van Azure AD-instantie en de tenant-ID.

Als u een doelgroep niet opgeeft, Azure AD heeft betrekking op uw app en persoonlijke Microsoft-accounts als een een publiek. (Dat wil zeggen, het gedraagt zich alsof `common` zijn opgegeven.)

### <a name="effective-audience"></a>Effectieve doelgroep
De effectieve doelgroep voor uw toepassing is het minimum van de doelgroep die u in uw app hebt ingesteld en de doelgroep die opgegeven in de app-registratie (als er een intersection bevatten). In feite de [App-registraties](https://aka.ms/appregistrations) ervaring kunt u de doelgroep (de ondersteunde accounttypen) voor de app opgeven. Zie voor meer informatie [Snelstart: Een toepassing registreren met het Microsoft identity-platform](quickstart-register-app.md).

Op dit moment is de enige manier om een app aan te melden bij de gebruikers met alleen persoonlijke Microsoft-accounts beide van deze instellingen configureren:
- De doelgroep van app-registratie instellen op `Work and school accounts and personal accounts`.
- Instellen van de doelgroep in uw code/configuratie te `AadAuthorityAudience.PersonalMicrosoftAccount` (of `TenantID` = 'gebruikers').

## <a name="client-id"></a>Client-id
De client-ID is de unieke toepassings (client)-ID die is toegewezen aan uw app door Azure AD wanneer de app is geregistreerd.

## <a name="redirect-uri"></a>Omleidings-URI
De omleidings-URI is de URI die de id-provider verzendt dat de beveiligingstokens back-ups te maken.

### <a name="redirect-uri-for-public-client-apps"></a>Omleidings-URI voor een openbare client-apps
Als u een openbare client app-ontwikkelaar die van MSAL gebruikmaakt:
- U hoeft niet om door te geven `RedirectUri` omdat deze automatisch wordt berekend door MSAL. Deze omleidings-URI is ingesteld op een van deze waarden, afhankelijk van het platform:
   - `urn:ietf:wg:oauth:2.0:oob` voor alle Windows-platforms
   - `msal{ClientId}://auth` voor Xamarin Android en iOS

- U hoeft te configureren van de omleiding URI in [App-registraties](https://aka.ms/appregistrations):

   ![Omleidings-URI in App-registraties](media/msal-client-application-configuration/redirect-uri.png)

U kunt de omleidings-URI overschrijven met behulp van de `RedirectUri` eigenschap (bijvoorbeeld als u brokers). Hier volgen enkele voorbeelden van de omleidings-URI's voor het betreffende scenario:

- `RedirectUriOnAndroid` = "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos` = $"msauth. {Bundle.ID}://auth';

Zie voor meer informatie, de [documentatie voor Android en iOS](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS).

### <a name="redirect-uri-for-confidential-client-apps"></a>Omleidings-URI voor vertrouwelijke client-apps
Is de URI die Azure AD wordt gebruikt voor het verzenden van het token terug naar de toepassing voor web-apps, de omleidings-URI (of antwoord-URI). Dit kan de URL van de web-app/Web-API zijn als de vertrouwelijke app een van deze is. De omleidings-URI moet worden geregistreerd in de app-registratie. Deze registratie is vooral belangrijk bij het implementeren van een app die u in eerste instantie lokaal hebt getest. Vervolgens moet u de antwoord-URL van de geïmplementeerde app in de portal voor appregistratie toevoegen.

Voor daemon-apps moet u niet een omleidings-URI opgeven.

## <a name="client-secret"></a>Clientgeheim
Deze optie geeft u het clientgeheim voor de vertrouwelijke client-app. Dit geheim (app-wachtwoord) is geleverd door de portal voor appregistratie of opgegeven naar Azure AD tijdens de appregistratie met PowerShell AzureAD, PowerShell AzureRM of Azure CLI.

## <a name="logging"></a>Logboekregistratie
De andere configuratieopties inschakelen logboekregistratie en oplossen van problemen. Zie de [logboekregistratie](msal-logging.md) artikel voor meer informatie over het gebruik ervan.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [clienttoepassingen instantiëren met behulp van MSAL.NET](msal-net-initializing-client-applications.md).

Meer informatie over [clienttoepassingen instantiëren met behulp van MSAL.js](msal-js-initializing-client-applications.md).
