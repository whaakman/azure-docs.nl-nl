---
title: Beheer toegang tot cloud-apps door het beperken van tenants - Azure | Microsoft Docs
description: Het gebruik van beperkingen van de Tenant te beheren welke gebruikers toegang apps op basis van hun Azure AD-tenant tot.
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
editor: yossib
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: kgremban
ms.openlocfilehash: 63e0fa54433a60fe7384d21cf7d215cc8283afca
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>Gebruik Tenant beperkingen voor het beheren van toegang tot SaaS cloudtoepassingen

Grote organisaties die Leg de nadruk op beveiliging wilt verplaatsen naar cloudservices zoals Office 365, maar u moet weten dat hun gebruikers alleen toegang goedgekeurde bronnen tot. Traditioneel beperken bedrijven domeinnamen of IP-adressen wanneer ze willen toegang beheren Deze aanpak is mislukt in een wereld waar SaaS-apps worden gehost in een openbare cloud, op gedeelde domeinnamen zoals outlook.office.com en login.microsoftonline.com uitgevoerd. Het blokkeren van deze adressen zou voorkomen dat gebruikers toegang tot de Outlook op het web volledig, in plaats van ze alleen beperken tot goedgekeurde identiteiten en resources.

Azure Active Directory-oplossing voor dit probleem is een functie beperkingen van de Tenant. Beperkingen van de tenant kunnen organisaties toegang tot SaaS-cloudtoepassingen, op basis van de Azure AD-tenant de toepassingen voor eenmalige aanmelding gebruiken. U wilt bijvoorbeeld toegang tot uw organisatie Office 365-toepassingen, zonder dat er toegang tot andere organisaties exemplaren van deze toepassingen dezelfde toestaan.  

Beperkingen van de tenant biedt organisaties de mogelijkheid om op te geven van de lijst met tenants die de gebruikers zijn gemachtigd voor toegang tot. Azure AD vervolgens verleent alleen toegang tot deze tenants toegestaan.

In dit artikel is gericht op de Tenant beperkingen voor Office 365, maar de functie moet samenwerken met elke SaaS-cloud-app die gebruikmaakt van moderne verificatieprotocollen met Azure AD voor eenmalige aanmelding. Als u de SaaS-apps met een andere Azure AD-van de tenant die wordt gebruikt door Office 365 tenant gebruikt, zorg ervoor dat alle vereiste tenants zijn toegestaan. Zie voor meer informatie over SaaS-cloud-apps, de [Active Directory Marketplace](https://azure.microsoft.com/en-us/marketplace/active-directory/).

## <a name="how-it-works"></a>Hoe werkt het?

De algehele oplossing omvat de volgende onderdelen: 

1. **Azure AD** – als de `Restrict-Access-To-Tenants: <permitted tenant list>` is aanwezig, Azure AD enige problemen beveiligingstokens voor de toegestane tenants. 

2. **On-premises proxy serverinfrastructuur** – een proxyapparaat geschikt voor SSL-inspectie geconfigureerd voor het invoegen van de header bevat de lijst met tenants toegestaan in een verkeer dat is bestemd voor Azure AD. 

3. **Clientsoftware** – ter ondersteuning van de beperkingen van de Tenant, clientsoftware moet tokens aanvragen rechtstreeks vanuit Azure AD, zodat verkeer kan worden onderschept door de infrastructuur van webtoepassingsproxy. Beperkingen van de tenant wordt momenteel ondersteund door Office 365-toepassingen op basis van een browser en Office-clients als moderne verificatie (zoals OAuth 2.0) wordt gebruikt. 

4. **Moderne verificatie** – cloudservices moderne verificatie moeten gebruiken Tenant-beperkingen gebruiken en blokkeren van toegang tot alle huurders niet toegestaan. Office 365 cloudservices moeten worden geconfigureerd voor gebruik van moderne verificatieprotocollen standaard. Lees voor de meest recente informatie over Office 365-ondersteuning voor moderne verificatie [moderne authenticatie van Office 365 bijgewerkt](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/).

Het volgende diagram illustreert de werkstroom op hoog niveau verkeer. SSL-inspectie is alleen vereist op het verkeer naar Azure AD, niet op de Office 365-cloudservices. Dit onderscheid is belangrijk, omdat het netwerkverkeer voor verificatie met Azure AD doorgaans veel lager zijn dan de omvang van het verkeer naar de SaaS-toepassingen zoals Exchange Online en SharePoint Online is.

![Tenant-netwerkverkeer beperkingen - diagram](./media/active-directory-tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Tenant beperkingen instellen

Er zijn twee stappen aan de slag met Tenant-beperkingen. De eerste stap is om ervoor te zorgen dat uw clients verbinding naar de juiste adressen maken kunnen. De tweede is het configureren van uw infrastructuur van webtoepassingsproxy.

### <a name="urls-and-ip-addresses"></a>URL's en IP-adressen

Voor het gebruik van beperkingen van de Tenant uw clients verbinding kunnen maken met de volgende Azure AD-URL's voor de verificatie moeten zijn: login.microsoftonline.com login.microsoft.com en login.windows.net. Bovendien voor toegang tot Office 365, uw clients ook moet kunnen verbinding maken met de FQDN-namen/URL's en IP-adressen die zijn gedefinieerd in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Proxyconfiguratie en vereisten

De volgende configuratie is vereist voor het inschakelen van beperkingen van de Tenant via uw proxy-infrastructuur. In deze richtlijnen is algemeen, dus u naar de documentatie van uw proxy-leverancier voor specifieke implementatiestappen verwijzen moet.

#### <a name="prerequisites"></a>Vereisten

- De proxy moet kunnen SSL worden onderschept, HTTP-header invoegen, uitvoeren en filteren bestemmingen met behulp van FQDN's / URL's. 

- De certificaatketen die worden aangeboden door de proxy voor SSL-communicatie moeten worden vertrouwd door clients. Bijvoorbeeld, als u certificaten van een interne PKI gebruikt, moet de interne verlenende basis-CA-certificaat worden vertrouwd.

- Deze functie is opgenomen in Office 365-abonnementen, maar als u Tenant-beperkingen gebruiken wilt om de toegang tot andere SaaS-apps beheren vervolgens Azure AD Premium 1 licenties zijn vereist.

#### <a name="configuration"></a>Configuratie

Voor elke inkomende aanvraag login.microsoftonline.com, login.microsoft.com en login.windows.net invoegen twee HTTP-headers: *Restrict Access aan Tenants* en *Restrict Access Context*.

De headers moeten de volgende elementen zijn: 
- Voor *Restrict Access aan Tenants*, een waarde van \<tenant lijst toegestaan\>, dit is een door komma's gescheiden lijst met tenants die u wilt toestaan dat gebruikers toegang krijgen tot. Elk domein dat is geregistreerd bij een tenant kan worden gebruikt voor het identificeren van de tenant in deze lijst. Bijvoorbeeld, om toegang te verlenen aan zowel Contoso en Fabrikam-tenants, de naam/waarde-paar ziet eruit als:`Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com` 
- Voor *Restrict Access Context*, een waarde van één map-ID, declareren waarover de Tenant-beperkingen configureert. Bijvoorbeeld, om te declareren Contoso als de tenant die de beperkingen van de Tenant-beleid instellen, de naam/waarde-paar ziet eruit als:`Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> U vindt de ID van uw directory in de [Azure-portal](https://portal.azure.com). Meld u aan als beheerder, selecteer **Azure Active Directory**, selecteer daarna **eigenschappen**.

Om te voorkomen dat gebruikers hun eigen HTTP-header met niet-goedgekeurde tenants invoegen, moet de proxy de header Restrict Access aan Tenants vervangen als deze al aanwezig in de binnenkomende aanvraag is. 

Clients moeten worden afgedwongen voor de proxy gebruiken voor alle aanvragen voor login.microsoftonline.com login.microsoft.com en login.windows.net. Bijvoorbeeld, als het PAC-bestanden worden gebruikt voor de clients omleiden naar de proxy gebruiken, mag eindgebruikers geen kunnen bewerken of uitschakelen van de PAC-bestanden.

## <a name="the-user-experience"></a>De gebruikerservaring

Deze sectie vindt de ervaring voor eindgebruikers en beheerders.

### <a name="end-user-experience"></a>Eindgebruikerservaring

Een voorbeeld van de gebruiker is in de Contoso-netwerk, maar is toegang wilt krijgen tot het Fabrikam-exemplaar van een gedeelde SaaS-toepassingen zoals Outlook online. Als Contoso een tenant niet is toegestaan voor het exemplaar is, ziet de gebruiker de volgende pagina:

![Toegang geweigerd-pagina voor gebruikers in tenants niet toegestaan](./media/active-directory-tenant-restrictions/end-user-denied.png)

### <a name="admin-experience"></a>Ervaring beheerder

Tijdens het configureren van beperkingen van de Tenant wordt uitgevoerd op de bedrijfsproxy-infrastructuur, beheerders direct toegang tot de beperkingen van de Tenant-rapporten in de Azure-portal. Rapporten weergeven, gaat u naar de pagina overzicht van Azure Active Directory en vervolgens kijkt u onder andere mogelijkheden'.

De beheerder voor de tenant die is opgegeven als de tenant met beperkte toegang Context dit rapport gebruiken kan om te zien alle aanmeldingen geblokkeerd vanwege het beleid voor Tenant-beperkingen, met inbegrip van de identiteit die wordt gebruikt en de doelmap-id.

![De Azure portal gebruiken om beperkte aanmeldpogingen weer te geven](./media/active-directory-tenant-restrictions/portal-report.png)

Net als andere rapporten in de Azure portal, kunt u filters gebruiken om op te geven van het bereik van uw rapport. U kunt filteren op een specifieke gebruiker, de toepassing, de client of het tijdsinterval.

## <a name="office-365-support"></a>Ondersteuning voor Office 365

Office 365-toepassingen moeten voldoen aan twee criteria op voor de Tenant beperkingen volledig te ondersteunen:

1. De client wordt gebruikt ondersteuning biedt voor moderne verificatie
2. Moderne verificatie is ingeschakeld als het standaardverificatieprotocol voor de cloudservice.

Raadpleeg [moderne authenticatie van Office 365 bijgewerkt](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/) voor de nieuwste informatie waarop Office clients momenteel moderne verificatie ondersteunen. Deze pagina bevat ook koppelingen naar instructies voor het inschakelen van moderne verificatie op specifieke Exchange Online en Skype voor bedrijven Online tenants. Moderne verificatie is al in SharePoint Online standaard ingeschakeld.

Beperkingen van de tenant wordt momenteel ondersteund door Office 365-browser gebaseerde toepassingen (de Office-Portal, Yammer, SharePoint-sites, Outlook op het Web, enz.). Voor Rich clients (Outlook, Skype voor bedrijven, Word, Excel, PowerPoint, enz.) Beperkingen van de tenant kunnen alleen worden afgedwongen wanneer moderne verificatie wordt gebruikt.  

Outlook en Skype voor bedrijven-clients die moderne verificatie ondersteunen kunnen nog steeds gebruik van verouderde protocollen tegen tenants waarop moderne verificatie niet is ingeschakeld, effectief Tenant beperkingen omzeilen. Voor Outlook op Windows, kunnen klanten ervoor kiezen om te voorkomen dat eindgebruikers niet-goedgekeurde e-mailaccounts toevoegen aan hun profielen beperkingen implementeren. Zie bijvoorbeeld de [Voorkom het toevoegen van niet-standaard Exchange-accounts](http://gpsearch.azurewebsites.net/default.aspx?ref=1) instellen via Groepsbeleid. Voor Outlook op niet-Windows-platforms, en voor Skype voor bedrijven op alle platforms is volledige ondersteuning voor de beperkingen van de Tenant momenteel niet beschikbaar.

## <a name="testing"></a>Testen

Als u wilt uitproberen Tenant beperkingen voordat u deze voor uw hele organisatie implementeert, er zijn twee opties: een benadering op basis van een host met een hulpprogramma zoals Fiddler of een gefaseerde implementatie van de proxy-instellingen.

### <a name="fiddler-for-a-host-based-approach"></a>Fiddler voor een host gebaseerde methode

Fiddler is een gratis web proxy die kan worden gebruikt voor het vastleggen en HTTP/HTTPS-verkeer, inclusief invoegen HTTP-headers wijzigen foutopsporing. Als u wilt Fiddler om te testen Tenant beperkingen configureren, moet u de volgende stappen uitvoeren:

1.  [Download en installeer Fiddler](http://www.telerik.com/fiddler).
2.  Configureer Fiddler voor het ontsleutelen van HTTPS-verkeer [Fiddler van help-documentatie](http://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).
3.  Fiddler om in te voegen configureren de *Restrict Access aan Tenants* en *Restrict Access Context* headers met behulp van aangepaste regels:
  1. Selecteer in het foutopsporingsprogramma van Fiddler Web de **regels** menu en selecteer **regels aanpassen...** Open het bestand CustomRules.
  2. De volgende regels toevoegen aan het begin van de *OnBeforeRequest* functie. Vervang \<domein van de tenant\> met een domein geregistreerd bij uw tenant, bijvoorbeeld: contoso.onmicrosoft.com. Vervang \<map-ID\> met Azure AD-GUID-id van uw tenant.

  ```
  if (oSession.HostnameIs("login.microsoftonline.com") || oSession.HostnameIs("login.microsoft.com") || oSession.HostnameIs("login.windows.net")){      oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";      oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";}
  ```

  Als u toestaan dat meerdere tenants wilt, gebruikt u een komma te scheiden van de namen van de tenant. Bijvoorbeeld:

  ```
  oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";
  ```

4. Sla en sluit het bestand CustomRules.

Nadat u Fiddler hebt geconfigureerd, kunt u verkeer vastleggen door te gaan naar de **bestand** menu en selecteer **verkeer vastleggen**.

### <a name="staged-rollout-of-proxy-settings"></a>Gefaseerde rollout van proxy-instellingen

Afhankelijk van de mogelijkheden van uw proxy-infrastructuur, kunt u mogelijk fase van de implementatie-instellingen voor uw gebruikers. Hier volgen een aantal geavanceerde opties voor overweging:

1.  PAC bestanden testgebruikers verwijzen naar een test-proxy-infrastructuur, tijdens normale gebruikers blijven gebruiken van de productie-proxy-infrastructuur gebruiken.
2.  Bepaalde proxyservers kunnen ondersteuning voor verschillende configuraties met behulp van groepen.

Raadpleeg de documentatie van uw proxy-server voor specifieke informatie.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [moderne authenticatie van Office 365 bijgewerkt](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)

- Controleer de [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
