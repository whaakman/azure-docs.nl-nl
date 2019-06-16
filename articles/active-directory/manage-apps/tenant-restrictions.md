---
title: Cloudtoepassingen tenantbeperkingen gebruiken voor het beheren van toegang tot SaaS - Azure | Microsoft Docs
description: Het gebruik van de tenantbeperkingen voor het beheren van welke gebruikers toegang apps op basis van hun Azure AD-tenant tot.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: mimart
ms.reviewer: richagi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4a340663a1ec4ddf748c6dc2bc3a4e2ce0c4228e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65824385"
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>Beperkingen voor tenants gebruiken voor het beheren van toegang tot SaaS-cloudtoepassingen

Grote organisaties die beveiliging benadrukken wilt verplaatsen naar cloudservices zoals Office 365, maar u moet weten dat hun gebruikers alleen toegang goedgekeurde bronnen tot. Bedrijven beperken er traditioneel voor dat domeinnamen of IP-adressen wanneer ze willen om toegang te beheren. Deze aanpak is mislukt in een wereld waar software als een service (of SaaS)-apps worden gehost in een openbare cloud, die worden uitgevoerd op gedeelde domeinnamen, zoals [outlook.office.com](https://outlook.office.com/) en [login.microsoftonline.com](https://login.microsoftonline.com/). Deze adressen blokkeert zou voorkomen dat gebruikers de webversie van Outlook helemaal openen in plaats van ze alleen beperken tot goedgekeurde-id's en resources.

De Azure Active Directory (Azure AD)-oplossing voor dit probleem is een functie met de naam van de beperkingen voor tenants. Met beperkingen voor tenants, kunnen organisaties toegang tot SaaS-cloudtoepassingen, op basis van de Azure AD-tenant die de toepassingen voor eenmalige aanmelding gebruiken. Bijvoorbeeld, kunt u voor toegang tot Office 365-toepassingen van uw organisatie, bij het voorkomen van toegang tot de andere organisaties exemplaren van deze dezelfde toepassingen.  

Met beperkingen voor tenants, kunnen organisaties de lijst met tenants die de gebruikers zijn gemachtigd voor toegang tot opgeven. Azure AD vervolgens verleent alleen toegang tot deze toegestane tenants.

In dit artikel is gericht op de tenantbeperkingen voor Office 365, maar de functie werkt met alle SaaS-cloudtoepassingen die gebruikmaakt van moderne-verificatieprotocollen met Azure AD voor eenmalige aanmelding. Als u SaaS-apps met een andere Azure AD-van de tenant die wordt gebruikt door Office 365 tenant gebruikt, zorg ervoor dat alle vereiste tenants zijn toegestaan. Zie voor meer informatie over SaaS-cloud-apps, de [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).

## <a name="how-it-works"></a>Hoe werkt het?

De algehele oplossing omvat de volgende onderdelen:

1. **Azure AD**: Als de `Restrict-Access-To-Tenants: <permitted tenant list>` is aanwezig, Azure AD enige problemen beveiligingstokens voor de toegestane tenants.

2. **On-premises proxy serverinfrastructuur**: Deze infrastructuur is een geschikt voor inspectie van Secure Sockets Layer (SSL) proxyapparaat. U moet de proxy voor het invoegen van de header met de lijst met toegestane tenants in verkeer dat is bestemd voor Azure AD configureren.

3. **Clientsoftware**: Ter ondersteuning van beperkingen voor tenants, moet clientsoftware aanvragen tokens rechtstreeks vanuit Azure AD, zodat de infrastructuur van webtoepassingsproxy kan verkeer onderscheppen. Office 365-toepassingen op basis van een browser ondersteunen momenteel beperkingen voor tenants, evenals Office-clients die gebruikmaken van moderne verificatie (zoals OAuth 2.0).

4. **Moderne verificatie**: Cloudservices moeten moderne verificatie wordt gebruikt om te gebruiken van beperkingen voor tenants en blokkeer de toegang tot alle niet-toegestane tenants. U kunt Office 365 cloud-services voor het gebruik van moderne-verificatieprotocollen standaard moet configureren. Lees voor de meest recente informatie over Office 365-ondersteuning voor moderne verificatie, [moderne verificatie van Office 365 bijgewerkt](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

Het volgende diagram illustreert de werkstroom op hoog niveau verkeer. Beperkingen voor tenants vereist SSL-inspectie alleen op het verkeer naar Azure AD, niet op de Office 365-cloudservices. Dit verschil is belangrijk, omdat het verkeersvolume voor verificatie met Azure AD doorgaans veel lager dan voor netwerkverkeer voor SaaS-toepassingen, zoals Exchange Online en SharePoint Online is.

![Tenant-beperkingen verkeersstroom - diagram](./media/tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Instellen van beperkingen voor tenants

Er zijn twee stappen aan de slag met beperkingen voor tenants. Controleer eerst of dat uw clients verbinding naar de juiste adressen maken kunnen. Ten tweede uw infrastructuur van webtoepassingsproxy configureren.

### <a name="urls-and-ip-addresses"></a>URL's en IP-adressen

Voor het gebruik van beperkingen voor tenants, moeten de clients geen verbinding maken met de volgende Azure AD-URL's om te verifiëren zich: [login.microsoftonline.com](https://login.microsoftonline.com/), [login.microsoft.com](https://login.microsoft.com/), en [ Login.Windows.NET](https://login.windows.net/). Bovendien voor toegang tot Office 365, moeten ook toegang tot uw clients zich geen verbinding maken met de volledig gekwalificeerde domeinnamen (FQDN's), URL's en IP-adressen die zijn gedefinieerd in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Proxy-configuratie en vereisten

De volgende configuratie is vereist voor het inschakelen van beperkingen voor tenants via uw infrastructuur van webtoepassingsproxy. Deze handleiding is algemeen, dus u naar de documentatie van de leverancier van uw proxy voor specifieke stappen verwijzen moet.

#### <a name="prerequisites"></a>Vereisten

- De proxy moet kunt SSL worden onderschept, HTTP-header invoegen, uitvoeren en bestemmingen met behulp van FQDN's / URL's te filteren.

- De certificaatketen weergegeven door de proxy voor SSL-communicatie moeten worden vertrouwd door clients. Bijvoorbeeld, als certificaten van een interne [openbare-sleutelinfrastructuur (PKI)](/windows/desktop/seccertenroll/public-key-infrastructure) worden gebruikt, de interne verlenende basis-CA-certificaat moet worden vertrouwd.

- Deze functie is opgenomen in Office 365-abonnementen, maar als u gebruiken van beperkingen voor tenants voor het beheren van toegang tot andere SaaS-apps wilt, klikt u vervolgens Azure AD Premium 1-licenties zijn vereist.

#### <a name="configuration"></a>Configuratie

Voeg twee HTTP-headers voor elke inkomende aanvraag login.microsoftonline.com, login.microsoft.com en login.windows.net: *Beperken-Access-naar-Tenants* en *beperken de toegang-Context*.

De headers moeten zijn onder andere de volgende elementen:

- Voor *Restrict Access aan Tenants*, gebruikt u een waarde van \<tenantlijst toegestaan\>, dit is een door komma's gescheiden lijst met tenants die u wilt toestaan dat gebruikers toegang krijgen tot. Elk domein dat is geregistreerd bij een tenant kan worden gebruikt om de tenant in deze lijst te identificeren. Bijvoorbeeld, om toegang te verlenen aan zowel Contoso en Fabrikam-tenants, lijkt de naam/waarde-paar: `Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com`

- Voor *Restrict Access Context*, gebruikt u een waarde van een enkele map-ID, welke tenant declareren is de tenant beperkingen instellen. Bijvoorbeeld, om te declareren Contoso als de tenant die het beleid van de beperkingen voor tenant instellen, lijkt de naam/waarde-paar: `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> U vindt uw map-ID in de [Azure Active Directory-portal](https://aad.portal.azure.com/). Meld u aan als beheerder, selecteer **Azure Active Directory**en selecteer vervolgens **eigenschappen**.

Om te voorkomen dat gebruikers hun eigen HTTP-header met niet-goedgekeurde tenants invoegen, de proxy nodig heeft om te vervangen de *Restrict Access aan Tenants* header als deze al aanwezig in de inkomende aanvraag is.

Clients moeten worden afgedwongen voor gebruik van de proxy voor alle aanvragen naar login.microsoftonline.com login.microsoft.com en login.windows.net. Bijvoorbeeld, als PAC-bestanden worden gebruikt om clients omleiden naar de proxy gebruiken, mag niet eindgebruikers mogelijk om te bewerken of uitschakelen van de PAC-bestanden.

## <a name="the-user-experience"></a>De gebruikerservaring

Deze sectie beschrijft de ervaring voor eindgebruikers en beheerders.

### <a name="end-user-experience"></a>Eindgebruikerservaring

Een van de voorbeeldgebruiker is in het Contoso-netwerk, maar wil toegang tot de Fabrikam-exemplaar van een gedeelde SaaS-toepassing, zoals Outlook online. Als Fabrikam een tenant niet is toegestaan voor het Contoso-exemplaar is, ziet de gebruiker een bericht denial-of toegang, die u probeert te krijgen tot een resource die tot een niet-goedgekeurde door uw IT-afdeling organisatie behoort zegt.

### <a name="admin-experience"></a>Beheerervaring

Tijdens de configuratie van de tenantbeperkingen wordt uitgevoerd op de zakelijke proxy-infrastructuur, beheerders kunnen rechtstreeks toegang hebben tot de rapporten van de tenant-beperkingen in Azure portal. De rapporten bekijken:

1. Aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com/). De **Azure Active Directory-beheercentrum** dashboard wordt weergegeven.

2. Selecteer de knop **Azure Active Directory** in het linkerdeelvenster. De Azure Active Directory-overzichtspagina wordt weergegeven.

3. In de **andere mogelijkheden** kop, selecteer **beperkingen voor de Tenantsleutel**.

De beheerder voor de tenant die is opgegeven als de tenant van de Context van een beperkte toegang tot dit rapport gebruiken kunt om te zien van aanmeldingen die zijn geblokkeerd vanwege de beperkingen beleid van de tenant, met inbegrip van de identiteit die wordt gebruikt en de doelmap-id. Aanmeldingen worden opgenomen als de instelling van de beperking van de tenant is de tenant van de gebruiker of de resource-tenant voor de aanmelding.

Net als andere rapporten in Azure portal, kunt u filters gebruiken om op te geven van het bereik van uw rapport. U kunt filteren op een bepaald tijdsinterval, gebruiker, toepassing, client of status. Als u selecteert de **kolommen** knop, die u kunt kiezen om gegevens met een combinatie van de volgende velden weer te geven:

- **User**
- **Toepassing**
- **Status**
- **datum**
- **Datum (UTC)** (UTC is Coordinated Universal Time)
- **Methode voor verificatie via MFA** (meervoudige verificatiemethode)
- **Details van de verificatie via MFA** (detail multifactor-verificatie)
- **MFA-resultaat**
- **IP-adres**
- **Client**
- **Gebruikersnaam**
- **Location**
- **ID van doeltenant**

## <a name="office-365-support"></a>Ondersteuning voor Office 365

Office 365-toepassingen moeten voldoen aan twee criteria op om aan te bieden volledige ondersteuning voor beperkingen voor tenants:

1. Moderne verificatie biedt ondersteuning voor de client gebruikt.
2. Moderne verificatie is ingeschakeld als het standaardverificatieprotocol voor de cloudservice.

Raadpleeg [moderne verificatie van Office 365 bijgewerkt](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/) voor de meest recente informatie over welke Office clients momenteel moderne verificatie ondersteunen. Deze pagina bevat ook koppelingen naar instructies voor het inschakelen van moderne verificatie op specifieke Exchange Online en Skype voor bedrijven Online tenants. SharePoint Online al moderne verificatie standaard ingeschakeld.

Browser gebaseerde toepassingen van Office 365 (de Office-Portal, Yammer, SharePoint-sites, Outlook op het Web en meer) momenteel ondersteuning voor beperkingen voor tenants. Dikke clients (Outlook, Skype voor bedrijven, Word, Excel, PowerPoint en meer) kunnen afdwingen tenantbeperkingen alleen die moderne authenticatie gebruiken.  

Outlook en Skype voor bedrijven-clients die ondersteuning bieden voor moderne verificatie nog steeds kunnen verouderde protocollen op tenants gebruiken waarop moderne verificatie wordt niet ingeschakeld kunnen, effectief tenantbeperkingen overslaan. Beperkingen voor tenants kunnen voorkomen dat toepassingen die gebruikmaken van verouderde protocollen als ze contact opneemt met login.microsoftonline.com, login.microsoft.com of login.windows.net tijdens de verificatie.

Voor Outlook voor Windows, kunnen klanten ervoor kiezen om te voorkomen dat eindgebruikers niet-goedgekeurde e-mailaccounts toevoegen aan hun profielen beperkingen implementeren. Zie bijvoorbeeld de [te voorkomen dat niet-standaard Exchange-accounts toevoegen](https://gpsearch.azurewebsites.net/default.aspx?ref=1) instellen via Groepsbeleid.

## <a name="testing"></a>Testen

Als u wilt voor het uitproberen van beperkingen voor tenants voordat u deze voor uw hele organisatie implementeert, hebt u twee opties: een benadering op basis van een host met behulp van een hulpprogramma zoals Fiddler of een gefaseerde implementatie van proxy-instellingen.

### <a name="fiddler-for-a-host-based-approach"></a>Fiddler voor een host gebaseerde aanpak

Fiddler is een gratis proxy die kan worden gebruikt om te leggen en te wijzigen van de HTTP/HTTPS-verkeer, inclusief invoegen van HTTP-headers voor Webfoutopsporing. Als u wilt configureren Fiddler als u wilt testen van beperkingen voor tenants, moet u de volgende stappen uitvoeren:

1. [Download en installeer Fiddler](https://www.telerik.com/fiddler).

2. Configureren van Fiddler voor het ontsleutelen van HTTPS-verkeer, per [Fiddler van help-documentatie](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

3. Configureren van Fiddler om in te voegen de *Restrict Access aan Tenants* en *Restrict Access Context* headers met behulp van aangepaste regels:

   1. Selecteer in het foutopsporingsprogramma van Fiddler Web de **regels** menu en selecteer **Customize Rules...** de CustomRules om bestand te openen.

   2. Voeg de volgende regels toe aan het begin van de `OnBeforeRequest` functie. Vervang \<tenantdomein\> met een domein dat is geregistreerd bij uw tenant (bijvoorbeeld `contoso.onmicrosoft.com`). Vervang \<map-ID\> met Azure AD-GUID-id van uw tenant.

      ```JScript.NET
      if (
          oSession.HostnameIs("login.microsoftonline.com") ||
          oSession.HostnameIs("login.microsoft.com") ||
          oSession.HostnameIs("login.windows.net")
      )
      {
          oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";
          oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";
      }
      ```

      Als u toestaan dat meerdere tenants wilt, gebruikt u een door komma's om de namen van de tenant te scheiden. Bijvoorbeeld:

      `oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";`

4. Sla op en sluit het bestand CustomRules.

Nadat u Fiddler hebt geconfigureerd, kunt u verkeer vastleggen door te gaan naar de **bestand** menu en selecteer **verkeer vastleggen**.

### <a name="staged-rollout-of-proxy-settings"></a>Gefaseerde implementatie van proxy-instellingen

Afhankelijk van de mogelijkheden van uw infrastructuur van webtoepassingsproxy, is het mogelijk om te zetten van de implementatie van de instellingen voor uw gebruikers. Hier volgen enkele geavanceerde opties voor overweging:

1. PAC-bestanden gebruiken testgebruikers verwijzen naar een proxy testinfrastructuur tijdens normale gebruikers echter ook doorgaan met de productie-proxy-infrastructuur.
2. Sommige proxyservers kunnen bieden ondersteuning voor configuraties met behulp van groepen.

Raadpleeg de documentatie van uw proxy-server voor specifieke details.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [moderne verificatie van Office 365 bijgewerkt](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/)
- Controleer de [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
