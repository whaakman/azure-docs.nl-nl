---
title: Beheer toegang tot cloud-apps door het beperken van tenants - Azure | Microsoft Docs
description: Het gebruik van de beperkingen van de Tenant voor het beheren van welke gebruikers toegang apps op basis van hun Azure AD-tenant tot.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: yossib
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: barbkess
ms.reviewer: richagi
ms.openlocfilehash: ca3396024fdab31217b00ef7cdd39e4e61504d27
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52851091"
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>Tenantbeperkingen gebruiken voor het beheren van toegang tot SaaS-cloudtoepassingen

Grote organisaties die beveiliging benadrukken wilt verplaatsen naar cloudservices zoals Office 365, maar u moet weten dat hun gebruikers alleen toegang goedgekeurde bronnen tot. Bedrijven beperken er traditioneel voor dat domeinnamen of IP-adressen wanneer ze willen om toegang te beheren. Deze aanpak is mislukt in een wereld waar SaaS-apps worden gehost in een openbare cloud, die worden uitgevoerd op gedeelde domeinnamen, zoals outlook.office.com en login.microsoftonline.com. Deze adressen blokkeert zou voorkomen dat gebruikers de webversie van Outlook helemaal openen in plaats van ze alleen beperken tot goedgekeurde-id's en resources.

Azure Active Directory-oplossing voor dit probleem is een functie met de naam van de beperkingen voor Tenants. Beperkingen voor tenants kunnen organisaties voor het beheren van toegang tot SaaS-cloudtoepassingen, op basis van de Azure AD-tenant de toepassingen voor eenmalige aanmelding gebruiken. Bijvoorbeeld, kunt u voor toegang tot Office 365-toepassingen van uw organisatie, bij het voorkomen van toegang tot de andere organisaties exemplaren van deze dezelfde toepassingen.  

Beperkingen voor tenants biedt organisaties de mogelijkheid om op te geven van de lijst met tenants die de gebruikers zijn gemachtigd voor toegang tot. Azure AD vervolgens verleent alleen toegang tot deze toegestane tenants.

In dit artikel is gericht op beperkingen voor Tenants voor Office 365, maar de functie werkt met alle SaaS-cloudtoepassingen die gebruikmaakt van moderne-verificatieprotocollen met Azure AD voor eenmalige aanmelding. Als u SaaS-apps met een andere Azure AD-van de tenant die wordt gebruikt door Office 365 tenant gebruikt, zorg ervoor dat alle vereiste tenants zijn toegestaan. Zie voor meer informatie over SaaS-cloud-apps, de [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).

## <a name="how-it-works"></a>Hoe werkt het?

De algehele oplossing omvat de volgende onderdelen: 

1. **Azure AD** : als de `Restrict-Access-To-Tenants: <permitted tenant list>` is aanwezig, Azure AD enige problemen beveiligingstokens voor de toegestane tenants. 

2. **On-premises proxy serverinfrastructuur** – een proxyapparaat kunnen SSL-inspectie, geconfigureerd voor het invoegen van de koptekst bevat de lijst met toegestane tenants in verkeer dat is bestemd voor Azure AD. 

3. **Clientsoftware** – ter ondersteuning van beperkingen voor Tenants, clientsoftware moet tokens aanvragen rechtstreeks vanuit Azure AD, zodat verkeer kan worden onderschept door de infrastructuur van webtoepassingsproxy. Beperkingen voor tenants wordt momenteel ondersteund door Office 365-toepassingen op basis van een browser en Office-clients wanneer moderne verificatie (zoals OAuth 2.0) wordt gebruikt. 

4. **Moderne verificatie** – cloudservices moeten moderne verificatie wordt gebruikt om te gebruiken van beperkingen voor Tenants en blokkeer de toegang tot alle niet-toegestane tenants. Office 365 cloud-services moeten worden geconfigureerd voor het gebruik van moderne-verificatieprotocollen standaard. Lees voor de meest recente informatie over Office 365-ondersteuning voor moderne verificatie, [moderne verificatie van Office 365 bijgewerkt](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/).

Het volgende diagram illustreert de werkstroom op hoog niveau verkeer. SSL-inspectie is alleen vereist op het verkeer naar Azure AD, niet op de Office 365-cloudservices. Dit verschil is belangrijk omdat het verkeersvolume voor verificatie met Azure AD doorgaans veel lager dan voor netwerkverkeer voor SaaS-toepassingen, zoals Exchange Online en SharePoint Online is.

![Tenant-beperkingen verkeersstroom - diagram](./media/tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Instellen van beperkingen voor Tenants

Er zijn twee stappen aan de slag met beperkingen voor Tenants. De eerste stap is om ervoor te zorgen dat uw clients verbinding naar de juiste adressen maken kunnen. De tweede is het configureren van uw infrastructuur van webtoepassingsproxy.

### <a name="urls-and-ip-addresses"></a>URL's en IP-adressen

Voor het gebruik van beperkingen voor Tenants, moeten de clients geen verbinding maken met de volgende Azure AD-URL's om te verifiëren zich: login.microsoftonline.com login.microsoft.com en login.windows.net. Bovendien voor toegang tot Office 365, moeten ook toegang tot uw clients zich geen verbinding maken met de FQDN-namen/URL's en IP-adressen die zijn gedefinieerd in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Proxy-configuratie en vereisten

De volgende configuratie is vereist voor het inschakelen van beperkingen voor Tenants via uw infrastructuur van webtoepassingsproxy. Deze handleiding is algemeen, dus u naar de documentatie van de leverancier van uw proxy voor specifieke stappen verwijzen moet.

#### <a name="prerequisites"></a>Vereisten

- De proxy moet kunt SSL worden onderschept, HTTP-header invoegen, uitvoeren en bestemmingen met behulp van FQDN's / URL's te filteren. 

- De certificaatketen weergegeven door de proxy voor SSL-communicatie moeten worden vertrouwd door clients. Bijvoorbeeld, als certificaten van een interne PKI worden gebruikt, moet de interne verlenende basis-CA-certificaat worden vertrouwd.

- Deze functie is opgenomen in Office 365-abonnementen, maar als u wilt gebruiken van beperkingen voor Tenants voor het beheren van toegang tot andere SaaS-apps vervolgens Azure AD Premium 1-licenties zijn vereist.

#### <a name="configuration"></a>Configuratie

Voeg twee HTTP-headers voor elke inkomende aanvraag login.microsoftonline.com, login.microsoft.com en login.windows.net: *Restrict Access aan Tenants* en *Restrict Access Context*.

De headers moeten zijn onder andere de volgende elementen: 
- Voor *Restrict Access aan Tenants*, een waarde van \<tenantlijst toegestaan\>, dit is een door komma's gescheiden lijst met tenants die u wilt toestaan dat gebruikers toegang krijgen tot. Elk domein dat is geregistreerd bij een tenant kan worden gebruikt om de tenant in deze lijst te identificeren. Bijvoorbeeld, om toegang te verlenen aan zowel Contoso en Fabrikam-tenants, lijkt de naam/waarde-paar:  `Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com` 
- Voor *Restrict Access Context*, een waarde van één map-ID, declareren welke tenant de beperkingen voor Tenants wordt ingesteld. Bijvoorbeeld, om te declareren Contoso als de tenant die stelt u het beleid voor beperkingen voor Tenants, lijkt de naam/waarde-paar: `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> U vindt uw map-ID in de [Azure-portal](https://portal.azure.com). Meld u aan als beheerder, selecteer **Azure Active Directory**en selecteer vervolgens **eigenschappen**.

Om te voorkomen dat gebruikers hun eigen HTTP-header met niet-goedgekeurde tenants invoegen, moet de proxy vervangen door de header Restrict Access aan Tenants als deze al aanwezig in de inkomende aanvraag is. 

Clients moeten worden afgedwongen voor gebruik van de proxy voor alle aanvragen naar login.microsoftonline.com login.microsoft.com en login.windows.net. Bijvoorbeeld, als het PAC-bestanden worden gebruikt om clients omleiden naar de proxy gebruiken, moet eindgebruikers niet kunnen om te bewerken of uitschakelen van de PAC-bestanden.

## <a name="the-user-experience"></a>De gebruikerservaring

Deze sectie wordt de ervaring voor eindgebruikers en beheerders.

### <a name="end-user-experience"></a>Eindgebruikerservaring

Een van de voorbeeldgebruiker is in het Contoso-netwerk, maar wil toegang tot de Fabrikam-exemplaar van een gedeelde SaaS-toepassing, zoals Outlook online. Als Contoso een niet-toegestane tenant voor die instantie is, ziet de gebruiker de volgende pagina:

![Pagina voor gebruikers in niet-toegestane tenants toegang geweigerd](./media/tenant-restrictions/end-user-denied.png)

### <a name="admin-experience"></a>Beheerervaring

Tijdens de configuratie van de Tenantbeperkingen wordt uitgevoerd op de zakelijke proxy-infrastructuur, beheerders kunnen rechtstreeks toegang hebben tot de rapporten beperkingen voor Tenants in de Azure-portal. Aan de rapporten bekijken, gaat u naar de pagina overzicht van Azure Active Directory en kijk onder 'Andere mogelijkheden'.

De beheerder voor de tenant die is opgegeven als de tenant van de Context van een beperkte toegang tot dit rapport gebruiken kunt om te zien van aanmeldingen die zijn geblokkeerd vanwege het beleid voor beperkingen voor Tenants, met inbegrip van de identiteit die wordt gebruikt en de doelmap-id. Aanmeldingen worden opgenomen als de instelling van de beperking van de tenant is de tenant van de gebruiker of de resource-tenant voor de aanmelding.

![Azure portal gebruiken om beperkte aanmeldpogingen weer te geven](./media/tenant-restrictions/portal-report.png)

Net als andere rapporten in Azure portal, kunt u filters gebruiken om op te geven van het bereik van uw rapport. U kunt filteren op een specifieke gebruiker, toepassing, client of tijdsinterval.

## <a name="office-365-support"></a>Ondersteuning voor Office 365

Office 365-toepassingen moeten voldoen aan twee criteria op om aan te bieden volledige ondersteuning voor beperkingen voor Tenants:

1. De client gebruikt biedt ondersteuning voor moderne verificatie
2. Moderne verificatie is ingeschakeld als het standaardverificatieprotocol voor de cloudservice.

Raadpleeg [moderne verificatie van Office 365 bijgewerkt](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/) voor de meest recente informatie over welke Office clients momenteel moderne verificatie ondersteunen. Deze pagina bevat ook koppelingen naar instructies voor het inschakelen van moderne verificatie op specifieke Exchange Online en Skype voor bedrijven Online tenants. Moderne verificatie is al ingeschakeld standaard in SharePoint Online.

Beperkingen voor tenants wordt momenteel ondersteund door Office 365-toepassingen op basis van een browser (de Office-Portal, Yammer, SharePoint-sites, Outlook op het Web, enz.). Voor thick clients (Outlook, Skype voor bedrijven, Word, Excel, PowerPoint, enz.) Beperkingen voor tenants kunnen alleen worden afgedwongen wanneer moderne verificatie wordt gebruikt.  

Outlook en Skype voor bedrijven-clients die ondersteuning bieden voor moderne verificatie nog steeds kunnen verouderde protocollen op tenants gebruiken waarop moderne verificatie niet is ingeschakeld kunnen, effectief Tenantbeperkingen overslaan. Toepassingen die gebruikmaken van verouderde protocollen mogelijk geblokkeerd door Tenantbeperkingen als ze contact opneemt met login.microsoftonline.com, login.microsoft.com of login.windows.net tijdens de verificatie.

Voor Outlook voor Windows, kunnen klanten ervoor kiezen om te voorkomen dat eindgebruikers niet-goedgekeurde e-mailaccounts toevoegen aan hun profielen beperkingen implementeren. Zie bijvoorbeeld de [te voorkomen dat niet-standaard Exchange-accounts toevoegen](https://gpsearch.azurewebsites.net/default.aspx?ref=1) instellen via Groepsbeleid.

## <a name="testing"></a>Testen

Als u wilt voor het uitproberen van beperkingen voor Tenants voordat u deze voor uw hele organisatie implementeert, er zijn twee opties: een benadering op basis van een host met behulp van een hulpprogramma zoals Fiddler of een gefaseerde implementatie van proxy-instellingen.

### <a name="fiddler-for-a-host-based-approach"></a>Fiddler voor een host gebaseerde aanpak

Fiddler is een gratis proxy die kan worden gebruikt om te leggen en te wijzigen van de HTTP/HTTPS-verkeer, inclusief invoegen van HTTP-headers voor Webfoutopsporing. Als u wilt configureren Fiddler als u wilt testen van beperkingen voor Tenants, moet u de volgende stappen uitvoeren:

1.  [Download en installeer Fiddler](https://www.telerik.com/fiddler).
2.  Configureren van Fiddler voor het ontsleutelen van HTTPS-verkeer, per [Fiddler van help-documentatie](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).
3.  Configureren van Fiddler om in te voegen de *Restrict Access aan Tenants* en *Restrict Access Context* headers met behulp van aangepaste regels:
  1. Selecteer in het foutopsporingsprogramma van Fiddler Web de **regels** menu en selecteer **Customize Rules...** de CustomRules om bestand te openen.
  2. Voeg de volgende regels toe aan het begin van de *OnBeforeRequest* functie. Vervang \<tenantdomein\> met een domein geregistreerd bij uw tenant, bijvoorbeeld contoso.onmicrosoft.com. Vervang \<map-ID\> met Azure AD-GUID-id van uw tenant.

  ```
  if (oSession.HostnameIs("login.microsoftonline.com") || oSession.HostnameIs("login.microsoft.com") || oSession.HostnameIs("login.windows.net")){      oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";      oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";}
  ```

  Als u toestaan dat meerdere tenants wilt, gebruikt u een door komma's om de namen van de tenant te scheiden. Bijvoorbeeld:

  ```
  oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";
  ```

4. Sla op en sluit het bestand CustomRules.

Nadat u Fiddler hebt geconfigureerd, kunt u verkeer vastleggen door te gaan naar de **bestand** menu en selecteer **verkeer vastleggen**.

### <a name="staged-rollout-of-proxy-settings"></a>Gefaseerde implementatie van proxy-instellingen

Afhankelijk van de mogelijkheden van uw infrastructuur van webtoepassingsproxy, is het mogelijk om te zetten van de implementatie van de instellingen voor uw gebruikers. Hier volgen enkele geavanceerde opties voor overweging:

1.  PAC-bestanden gebruiken testgebruikers verwijzen naar een proxy testinfrastructuur tijdens normale gebruikers echter ook doorgaan met de productie-proxy-infrastructuur.
2.  Sommige proxyservers kunnen bieden ondersteuning voor configuraties met behulp van groepen.

Raadpleeg de documentatie van uw proxy-server voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [moderne verificatie van Office 365 bijgewerkt](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)

- Controleer de [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
