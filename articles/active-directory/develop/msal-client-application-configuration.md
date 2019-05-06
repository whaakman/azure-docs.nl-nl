---
title: Configuratie van client-toepassing (Microsoft Authentication Library) | Azure
description: Meer informatie over de configuratieopties voor openbare-client en vertrouwelijke client toepassingen in de Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
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
ms.openlocfilehash: 3a48eea9fedd2d82f44693d58b31ee0d5c8c288d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138535"
---
# <a name="application-configuration-options"></a>Opties voor toepassingsconfiguraties

In uw code kunt u een nieuwe openbare of vertrouwelijke client (of gebruikersagent voor MSAL.js) initialiseren toepassing om te verifiëren en verkrijgen van tokens.  Er zijn een aantal verschillende configuratieopties die kan worden ingesteld bij het initialiseren van de clienttoepassing in MSAL. Deze opties kunnen worden opgesplitst in twee groepen:

- Opties voor registratie, met inbegrip van:
    - [Instantie](#authority) (bestaat uit de id-provider [exemplaar](#cloud-instance) en meld u [doelgroep](#application-audience) voor de toepassing, en mogelijk de tenant-ID).
    - [Client-ID](#client-id)
    - [Omleidings-URI](#redirect-uri)
    - [clientgeheim](#client-secret) (voor toepassingen van vertrouwelijke client).
- [Opties voor logboekregistratie](#logging), met inbegrip van logboek-niveau, beheer van persoonlijke gegevens en de naam van het onderdeel met behulp van de bibliotheek.

## <a name="authority"></a>Instantie
De instantie is een URL die wijzen op een map die u MSAL kunt tokens van aanvragen. Gebruikelijke instanties zijn:

- https://login.microsoftonline.com/&lt; tenant&gt;/ waar &lt;tenant&gt; is de tenant-ID van de Azure AD-tenant of een domein dat is gekoppeld aan deze Azure AD-tenant.  Alleen aan te melden bij de gebruikers van een specifieke organisatie gebruikt.
- https://login.microsoftonline.com/common/. Gebruikt voor het aanmelden van gebruikers met werk en schoolaccounts of een persoonlijk Microsoft-account.
- https://login.microsoftonline.com/organizations/. Gebruikt voor het aanmelden van gebruikers met een werk-en schoolaccounts.
- https://login.microsoftonline.com/consumers/. Gebruikt voor het aanmelden van gebruikers met alleen persoonlijke Microsoft-account (live).

De instelling van de instantie moet overeenkomen met wat is gedeclareerd in de portal voor appregistratie zijn.

URL van de instantie van het exemplaar en de doelgroep bestaat.

De instantie kan zijn:
- een Azure Active directory-Cloud-instantie
- een Azure AD B2C-instantie. Zie [B2C specificaties](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics).
- een AD FS-instantie. Zie [ondersteuning voor AD FS](https://aka.ms/msal-net-adfs-support).

Instanties van de cloud Azure AD heeft twee onderdelen:
- de id-provider **exemplaar**
- de aanmelding **doelgroep** voor de toepassing

Het exemplaar en de doelgroep kunnen worden samengevoegd en geleverd als URL van de instantie. In versies van MSAL.NET vóór MSAL 3.x, moest u voor het opstellen van de instantie zelf afhankelijk van de cloud die u wilde doel en de doelgroep aanmelden.  Het volgende diagram toont hoe de URL van de instantie is samengesteld.

![Instantie](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Cloud-exemplaar
De **exemplaar** wordt gebruikt om op te geven als uw toepassing is worden gebruikt voor het aanmelden van gebruikers van de openbare cloud van Microsoft Azure, of van nationale clouds. Met MSAL in uw code, het Azure-cloud-exemplaar kan worden ingesteld met behulp van een opsomming of door de URL van de [nationale cloudinstantie](authentication-national-cloud.md#azure-ad-authentication-endpoints) als de `Instance` lid (als u deze kent).

MSAL.NET genereert een expliciete uitzondering als beide `Instance` en `AzureCloudInstance` zijn opgegeven. 

Als u een exemplaar niet opgeeft, uw app heeft betrekking op de openbare cloud van Azure-instantie (het exemplaar van de URL `https://login.onmicrosoftonline.com`).

## <a name="application-audience"></a>Doelgroep van toepassing

De doelgroep aanmelden, is afhankelijk van de zakelijke behoeften voor uw toepassing:
- Als u een line-of-business (LOB)-ontwikkelaar bent, zult u waarschijnlijk produceren van een toepassing met één tenant, die alleen in uw organisatie worden gebruikt. In dat geval moet u opgeven wat deze organisatie is, door de tenant-ID (ID van uw Azure Active Directory) of een domeinnaam die is gekoppeld aan dit Azure Active Directory.
- Als u een ISV bent, kunt u aan te melden bij de gebruikers met hun werk en schoolaccounts in elke organisatie, of sommige organisaties (multitenant-app), maar u kunt ook dat gebruikers zich aanmelden met hun persoonlijke Microsoft-account.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Het opgeven van de doelgroep in uw code/configuratie
Met MSAL in uw code, opgeven u de doelgroep met behulp van:
- een van beide de Azure AD-instantie doelgroep opsomming. 
- of de tenant-ID, die kan zijn:
  - een GUID (de ID van uw Azure Active Directory) voor toepassingen met één tenant
  - een domeinnaam die is gekoppeld aan uw Azure Active Directory (ook voor toepassingen met één tenant)
- of een van deze tijdelijke aanduidingen als een tenant-ID in plaats van de opsomming van Azure AD-instantie doelgroep:
    - `organizations` voor een toepassing met meerdere tenants
    - `consumers` aan te melden bij gebruikers alleen met hun persoonlijke account
    - `common` aanmelden van gebruikers met hun werk- en schoolaccount of persoonlijk Microsoft-account

MSAL een zinvolle uitzondering genereert als u zowel Azure AD-instantie doelgroep en de tenant-ID. 

Als u geen dat een een publiek opgeeft uw app heeft betrekking op Azure AD en privé-Microsoft-accounts als een specifieke doelgroep (dat wil zeggen `common`).

### <a name="effective-audience"></a>Effectieve doelgroep
De effectieve doelgroep voor uw toepassing is de minimale (als er een snijpunt) van de doelgroep die u in uw toepassing instellen en de doelgroep die is opgegeven in de registratie van de toepassing. Inderdaad, de toepassing registratie-ervaring ([App-registratie](https://aka.ms/appregistrations)) kunt u de doelgroep (ondersteunde accounttypen) opgeven voor de toepassing. Zie [Quickstart: Een toepassing registreren met het Microsoft identity-platform](quickstart-register-app.md) voor meer informatie.

Op dit moment is de enige manier om een toepassing aan te melden bij de gebruikers met alleen persoonlijke Microsoft-accounts om in te stellen:
- de app-registratie doelgroep ingesteld op 'Werk en schoolaccounts en persoonlijke accounts' en,
- en stel de doelgroep in uw code / configuratie `AadAuthorityAudience.PersonalMicrosoftAccount` (of `TenantID `= 'gebruikers')

## <a name="client-id"></a>Client-id
De unieke toepassings (client)-ID die wordt toegewezen aan uw app door Azure AD wanneer de app is geregistreerd.

## <a name="redirect-uri"></a>Omleidings-URI
De omleidings-URI is de URI waar de id-provider de beveiligingstokens terug verzendt. 

### <a name="redirect-uri-for-public-client-applications"></a>Omleidings-URI voor openbare-clienttoepassingen
Voor ontwikkelaars van openbare clienttoepassingen met MSAL:
- U hoeft niet om door te geven de ``RedirectUri`` zoals deze wordt automatisch berekend door MSAL. Deze omleidings-URI is ingesteld op de volgende waarden afhankelijk van het platform:

- ``urn:ietf:wg:oauth:2.0:oob`` voor alle Windows-platforms
- ``msal{ClientId}://auth`` voor Xamarin Android en iOS

Echter, de redirect URI moet worden geconfigureerd de [App-registratie](https://aka.ms/appregistrations).

![Omleidings-URI in de portal](media/msal-client-application-configuration/redirect-uri.png)

Het is mogelijk om op te heffen de omleidings-URI met behulp van de `RedirectUri` eigenschap, bijvoorbeeld als u brokers. Hier volgen enkele voorbeelden van de omleidings-URI's in dat geval:

- `RedirectUriOnAndroid` = "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos` = $"msauth. {Bundle.ID}://auth';

Zie voor meer informatie Android specificaties en [iOS-specificaties](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS)

### <a name="redirect-uri-for-confidential-client-applications"></a>Omleidings-URI voor vertrouwelijke client-toepassingen
Is de URI waarop Azure AD neemt contact met weer de toepassing met het token voor web-apps, de omleidings-URI (of de antwoord-URI). Dit kan zijn dat de URL van de Web-App / Web-API als de vertrouwelijke een van deze is.  Deze omleidings-URI moet worden geregistreerd in de app-registratie. Dit is vooral belangrijk bij het implementeren van een toepassing die u in eerste instantie lokaal hebt getest. Vervolgens moet u de antwoord-URL van de geïmplementeerde toepassing toevoegen in de portal voor appregistratie.

Voor daemon-apps moet u niet een omleidings-URI opgeven.

## <a name="client-secret"></a>Clientgeheim
Het clientgeheim voor de toepassing vertrouwelijke client. Dit geheim (toepassingswachtwoord) is geleverd door de portal voor appregistratie of Azure AD tijdens de registratie van de toepassing die met de PowerShell AzureAD, PowerShell AzureRM of Azure CLI.

## <a name="logging"></a>Logboekregistratie
De andere opties schakelt u logboekregistratie en oplossen van problemen. Zie voor meer informatie de [logboekregistratie](msal-logging.md) pagina voor meer informatie over het gebruik ervan.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [instantiëren clienttoepassingen met behulp van MSAL.NET](msal-net-initializing-client-applications.md).

Meer informatie over [instantiëren-clienttoepassingen via MSAL.js](msal-js-initializing-client-applications.md).
