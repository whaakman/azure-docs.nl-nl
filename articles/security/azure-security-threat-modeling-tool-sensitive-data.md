---
title: Gevoelige gegevens - hulpmiddel voor het modelleren van Microsoft Threat - Azure | Microsoft Docs
description: oplossingen voor bedreigingen die worden weergegeven in het hulpprogramma Threat Modeling
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 21d1ba02052862e16ef27ec313d53cd0bffcc21a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-sensitive-data--mitigations"></a>Beveiliging Frame: Gevoelige gegevens | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Een grens machine vertrouwensrelatie** | <ul><li>[Zorg ervoor dat de binaire bestanden worden verborgen als ze gevoelige informatie bevatten](#binaries-info)</li><li>[U kunt met behulp van Encrypted File System (EFS) wordt gebruikt om vertrouwelijke gebruikersspecifieke gegevens te beveiligen](#efs-user)</li><li>[Zorg ervoor dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestandssysteem worden versleuteld](#filesystem)</li></ul> | 
| **Webtoepassing** | <ul><li>[Zorg ervoor dat gevoelige inhoud niet in cache in de browser opgeslagen is](#cache-browser)</li><li>[Secties van Web-App-configuratiebestanden met gevoelige gegevens versleutelen](#encrypt-data)</li><li>[Het HTML-kenmerk voor automatisch aanvullen in gevoelige formulieren en invoer expliciet uitschakelen](#autocomplete-input)</li><li>[Zorg ervoor dat gevoelige gegevens die worden weergegeven op het scherm van de gebruiker wordt gemaskeerd](#data-mask)</li></ul> | 
| **Database** | <ul><li>[Dynamische-gegevensmaskering om te voorkomen dat gevoelige gegevens niet blootstelling bevoegde gebruikers implementeren](#dynamic-users)</li><li>[Zorg ervoor dat de wachtwoorden worden opgeslagen in gezouten hash-indeling](#salted-hash)</li><li>[Zorg ervoor dat gevoelige gegevens in de databasekolommen worden versleuteld](#db-encrypted)</li><li>[Zorg ervoor dat versleuteling databaseniveau (TDE) is ingeschakeld](#tde-enabled)</li><li>[Ervoor zorgen dat databaseback-ups worden gecodeerd](#backup)</li></ul> | 
| **Web-API** | <ul><li>[Zorg ervoor dat gevoelige gegevens met betrekking tot de Web-API niet zijn opgeslagen in de opslag van de browser](#api-browser)</li></ul> | 
| Azure Documentdb | <ul><li>[Versleutelen van gevoelige gegevens die zijn opgeslagen in DocumentDB](#encrypt-docdb)</li></ul> | 
| **Vertrouwensgrenzen van Azure IaaS VM** | <ul><li>[Azure Disk Encryption gebruiken voor het versleutelen van schijven die worden gebruikt door virtuele Machines](#disk-vm)</li></ul> | 
| **Service Fabric-Vertrouwensgrenzen** | <ul><li>[Versleutelen van geheimen in Service Fabric-toepassingen](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Beveiliging modellering uitvoeren en zakelijke eenheden/Teams gebruiken indien vereist](#modeling-teams)</li><li>[Beperken van toegang tot het onderdeel op Kritiek entiteiten delen](#entities)</li><li>[Gebruikers op de risico's die zijn gekoppeld aan de Dynamics CRM-Share-functie en de juiste beveiligingsprocedures trainen](#good-practices)</li><li>[Een ontwikkeling standaarden regel met config details in Uitzonderingsbeheer proscribing opnemen](#exception-mgmt)</li></ul> | 
| **Azure Storage** | <ul><li>[Versleuteling van Azure Storage-Service (SSE) gebruiken voor Data-at-Rest (Preview)](#sse-preview)</li><li>[Client-Side-versleuteling gebruiken voor het opslaan van gevoelige gegevens in Azure Storage](#client-storage)</li></ul> | 
| **Mobiele clients** | <ul><li>[Versleutelen van gevoelige of persoonlijke gegevens geschreven naar de lokale opslag telefoons](#pii-phones)</li><li>[Verbergen, gegenereerde binaire bestanden voordat u distribueert naar eindgebruikers weergeven](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[ClientCredentialType ingesteld op Certificate of Windows](#cert)</li><li>[WCF-beveiligingsmodus is niet ingeschakeld](#security)</li></ul> | 

## <a id="binaries-info"></a>Zorg ervoor dat de binaire bestanden worden verborgen als ze gevoelige informatie bevatten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Een grens machine vertrouwensrelatie | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Zorg ervoor dat de binaire bestanden worden verborgen als ze gevoelige informatie zoals handelsgeheimen, vertrouwelijke zakelijke logica die moet worden niet omgekeerd bevatten. Dit is om te stoppen reverse-engineering van assembly's. Hulpprogramma's zoals `CryptoObfuscator` kan worden gebruikt voor dit doel. |

## <a id="efs-user"></a>U kunt met behulp van Encrypted File System (EFS) wordt gebruikt om vertrouwelijke gebruikersspecifieke gegevens te beveiligen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Een grens machine vertrouwensrelatie | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | U kunt met behulp van Encrypted File System (EFS) wordt gebruikt om te voorkomen dat vertrouwelijke gebruikersspecifieke gegevens tegenstanders met fysieke toegang met de computer. |

## <a id="filesystem"></a>Zorg ervoor dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestandssysteem worden versleuteld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Een grens machine vertrouwensrelatie | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Ervoor zorgen dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestandssysteem is gecodeerd (bijvoorbeeld met behulp van DPAPI), als EFS kunnen niet worden afgedwongen. |

## <a id="cache-browser"></a>Zorg ervoor dat gevoelige inhoud niet in cache in de browser opgeslagen is

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, Web Forms, MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Browsers kunnen gegevens voor de doeleinden van opslaan in cache en geschiedenis opslaan. Deze bestanden in de cache worden opgeslagen in een map, zoals de map Tijdelijke internetbestanden in het geval van Internet Explorer. Wanneer deze pagina's opnieuw worden aangeduid, worden deze door de browser uit de cache weergegeven. Als gevoelige informatie wordt weergegeven voor de gebruiker (zoals hun adres, creditcardgegevens, sociaal-fiscaal nummer of gebruikersnaam), kan klikt u vervolgens deze informatie worden opgeslagen in de browser-cache en daarom worden opgehaald via de browser-cache onderzoeken of op de knop 'Terug' in de browser te drukken. Cache-control van antwoord header-waarde 'Nee-store' voor alle pagina's ingesteld. |

### <a name="example"></a>Voorbeeld
```XML
<configuration>
  <system.webServer>
   <httpProtocol>
    <customHeaders>
        <add name="Cache-Control" value="no-cache" />
        <add name="Pragma" value="no-cache" />
        <add name="Expires" value="-1" />
    </customHeaders>
  </httpProtocol>
 </system.webServer>
</configuration>
```

### <a name="example"></a>Voorbeeld
Dit kan worden geïmplementeerd door een filter. Voorbeeld van de volgende kan worden gebruikt: 
```C#
public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            if (filterContext == null || (filterContext.HttpContext != null && filterContext.HttpContext.Response != null && filterContext.HttpContext.Response.IsRequestBeingRedirected))
            {
                //// Since this is MVC pipeline, this should never be null.
                return;
            }

            var attributes = filterContext.ActionDescriptor.GetCustomAttributes(typeof(System.Web.Mvc.OutputCacheAttribute), false);
            if (attributes == null || **Attributes**.Count() == 0)
            {
                filterContext.HttpContext.Response.Cache.SetNoStore();
                filterContext.HttpContext.Response.Cache.SetCacheability(HttpCacheability.NoCache);
                filterContext.HttpContext.Response.Cache.SetExpires(DateTime.UtcNow.AddHours(-1));
                if (!filterContext.IsChildAction)
                {
                    filterContext.HttpContext.Response.AppendHeader("Pragma", "no-cache");
                }
            }

            base.OnActionExecuting(filterContext);
        }
``` 

## <a id="encrypt-data"></a>Secties van Web-App-configuratiebestanden met gevoelige gegevens versleutelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Procedure: Coderen configuratiesecties in ASP.NET 2.0 met behulp van DPAPI](https://msdn.microsoft.com/library/ff647398.aspx), [geven een Configuratieprovider beveiligd](https://msdn.microsoft.com/library/68ze1hb2.aspx), [met behulp van Azure Key Vault toepassing geheimen beveiligen](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Stappen** | Configuratiebestanden, bijvoorbeeld als het bestand Web.config, appsettings.json vaak worden gebruikt voor het opslaan van gevoelige gegevens, zoals gebruikersnamen, wachtwoorden, databaseverbindingsreeksen en versleutelingssleutels. Als u deze informatie niet beveiligt, is uw toepassing gevoelig zijn voor aanvallers of kwaadwillende gebruikers het verkrijgen van gevoelige informatie zoals gebruikersnamen en wachtwoorden, databasenamen en servernamen. Op basis van het implementatietype (azure/on-premises), de gevoelige secties van configuratiebestanden met DPAPI of services zoals Azure Key Vault versleutelen. |

## <a id="autocomplete-input"></a>Het HTML-kenmerk voor automatisch aanvullen in gevoelige formulieren en invoer expliciet uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [MSDN: automatisch aanvullen kenmerk](http://msdn.microsoft.com/library/ms533486(VS.85).aspx), [automatisch aanvullen gebruiken in HTML](http://msdn.microsoft.com/library/ms533032.aspx), [beveiligingslek in HTML opschoning](http://technet.microsoft.com/security/bulletin/MS10-071), [automatisch aanvullen., opnieuw?](http://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Stappen** | Het kenmerk autocomplete geeft aan of een formulier automatisch aanvullen in- of uitschakelen hebben moet. Wanneer automatisch aanvullen is ingeschakeld, worden de waarden op basis van waarden die de gebruiker heeft ingevoerd voor de browser automatisch aanvullen. Bijvoorbeeld, wanneer een nieuwe naam en het wachtwoord is ingevoerd in een formulier en het formulier wordt verzonden, de browser wordt u gevraagd als het wachtwoord moet worden opgeslagen. Daarna wanneer het formulier wordt weergegeven, de naam en het wachtwoord worden automatisch ingevuld of zijn voltooid, zoals de naam van de opgegeven. Een aanvaller met lokale toegang, kan het wachtwoord met ongecodeerde tekst ophalen uit de browsercache. Automatisch aanvullen is standaard ingeschakeld en moet expliciet worden uitgeschakeld. |

### <a name="example"></a>Voorbeeld
```C#
<form action="Login.aspx" method="post " autocomplete="off" >
      Social Security Number: <input type="text" name="ssn" />
      <input type="submit" value="Submit" />    
</form>
```

## <a id="data-mask"></a>Zorg ervoor dat gevoelige gegevens die worden weergegeven op het scherm van de gebruiker wordt gemaskeerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Gevoelige gegevens, zoals wachtwoorden, creditcardnummers, enz. sofi-nummer moet worden gemaskeerd wanneer weergegeven op het scherm. Hiermee wordt voorkomen dat onbevoegde personen toegang hebben tot de gegevens (bijvoorbeeld schouder surfen wachtwoorden, ondersteuningspersoneel SSN aantallen gebruikers weer te geven). Zorg ervoor dat de elementen van deze gegevens niet zichtbaar in tekst zonder opmaak zijn en op de juiste wijze worden gemaskeerd. Dit moet worden afgehandeld terwijl ze worden geaccepteerd als invoer (zoals. input type = 'password') en de weergeven terug op het scherm (bijvoorbeeld weergave alleen de laatste 4 cijfers van het creditcardnummer). |

## <a id="dynamic-users"></a>Dynamische-gegevensmaskering om te voorkomen dat gevoelige gegevens niet blootstelling bevoegde gebruikers implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, OnPrem |
| **Kenmerken**              | SQL-versie - V12, SQL-versie - MsSQL2016 |
| **Verwijzingen**              | [Maskering van dynamische gegevens](https://msdn.microsoft.com/library/mt130841) |
| **Stappen** | Het doel van de dynamische-gegevensmaskering is te beperken van blootstelling van gevoelige gegevens, te voorkomen dat gebruikers geen toegang tot de gegevens hebben mogen kunnen bekijken. Dynamische-gegevensmaskering is niet gericht zijn op te voorkomen dat databasegebruikers van een rechtstreekse verbinding met de database en u uitgebreide query's uitvoert die de gevoelige gegevens. Dynamische gegevensmaskering is een aanvulling op andere beveiligingsfuncties van SQL Server (controle, versleuteling en beveiliging op rijniveau...) en het is raadzaam deze functie wilt gebruiken in combinatie met deze bovendien beter beveiligen van de gevoelige gegevens in de de database. Houd er rekening mee dat deze functie wordt alleen ondersteund door SQL Server 2016 vanaf en Azure SQL Database. |

## <a id="salted-hash"></a>Zorg ervoor dat de wachtwoorden worden opgeslagen in gezouten hash-indeling

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Hashing van wachtwoord met .NET Crypto-API 's](http://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Stappen** | Wachtwoorden moeten niet worden opgeslagen in databases met aangepaste store. Wachtwoord-hashes moeten in plaats daarvan met salt waarden worden opgeslagen. Zorg ervoor dat de salt voor de gebruiker altijd uniek is en u b-crypt, s-crypt of PBKDF2 voordat het wachtwoord, met een minimale werk factor herhaling telling van 150.000 lussen om te voorkomen dat de van brute force wordt opgeslagen.| 

## <a id="db-encrypted"></a>Zorg ervoor dat gevoelige gegevens in de databasekolommen worden versleuteld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | SQL-versie - alle |
| **Verwijzingen**              | [Versleutelen van gevoelige gegevens in SQL server](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [hoe: een kolom van de gegevens in SQL Server versleutelen](https://msdn.microsoft.com/library/ms179331), [coderen van certificaat](https://msdn.microsoft.com/library/ms188061) |
| **Stappen** | Gevoelige gegevens, zoals creditcardnummers heeft in de database worden versleuteld. Gegevens kunnen worden versleuteld met behulp van versleuteling op kolomniveau of door de functie van een toepassing met behulp van de functies voor versleuteling. |

## <a id="tde-enabled"></a>Zorg ervoor dat versleuteling databaseniveau (TDE) is ingeschakeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Transparent Data Encryption (TDE) voor informatie over SQL Server](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Stappen** | Transparent Data Encryption (TDE)-functie in SQL server helpt bij het versleutelen van gevoelige gegevens in een database en Beveilig de sleutels die worden gebruikt voor het versleutelen van de gegevens met een certificaat. Dit voorkomt dat iedereen zonder de sleutels met behulp van de gegevens. TDE beschermt gegevens 'in rust', wat betekent dat de bestanden voor gegevens en logboekbestanden. Dit biedt de mogelijkheid om te voldoen aan veel wettelijke, en tot stand gebracht in verschillende branches richtlijnen. |

## <a id="backup"></a>Ervoor zorgen dat databaseback-ups worden gecodeerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, OnPrem |
| **Kenmerken**              | SQL-versie - V12, SQL-versie - MsSQL2014 |
| **Verwijzingen**              | [SQL database-back-versleuteling](https://msdn.microsoft.com/library/dn449489) |
| **Stappen** | SQL Server biedt de mogelijkheid voor het versleutelen van de gegevens bij het maken van een back-up. Door het versleutelingsalgoritme en de codeerder (certificaten of asymmetrische sleutel) te geven wanneer u een back-up maakt, een gecodeerde back-upbestand kan worden gemaakt. |

## <a id="api-browser"></a>Zorg ervoor dat gevoelige gegevens met betrekking tot de Web-API niet zijn opgeslagen in de opslag van de browser

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC 5, 6 MVC |
| **Kenmerken**              | ID-Provider - ADFS, identiteitsprovider - Azure AD |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>In bepaalde implementaties worden gevoelige artefacten die relevant is voor verificatie van de Web-API opgeslagen in de lokale opslag van de browser. Bijvoorbeeld, zoals Azure AD authentication artefacten adal.idtoken, adal.nonce.idtoken, adal.access.token.key, adal.token.keys, adal.state.login, adal.session.state, adal.expiration.key enzovoort.</p><p>Alle deze artefacten beschikbaar, zelfs na het zijn afmelden of de browser wordt gesloten. Als een adversary toegang tot deze artefacten krijgt, kan hij ze toegang krijgen tot de beveiligde bronnen (API's) opnieuw gebruiken. Zorg ervoor dat alle gevoelige artefacten die betrekking hebben op Web-API niet zijn opgeslagen in de opslag van de browser. In gevallen waarin de client-side '-opslag onvermijdelijk is (bijvoorbeeld één pagina toepassingen (SPA) die gebruikmaken van de impliciete OpenIdConnect/OAuth stromen nodig voor het opslaan van de toegangstokens lokaal), gebruik opslagopties met hebben geen persistentie. bijvoorbeeld liever SessionStorage LocalStorage.</p>| 

### <a name="example"></a>Voorbeeld
De onderstaande JavaScript codefragment is van een aangepaste verificatie-bibliotheek die verificatie artefacten in de lokale opslag opslaat. Deze implementaties moeten worden vermeden. 
```javascript
ns.AuthHelper.Authenticate = function () {
window.config = {
instance: 'https://login.microsoftonline.com/',
tenant: ns.Configurations.Tenant,
clientId: ns.Configurations.AADApplicationClientID,
postLogoutRedirectUri: window.location.origin,
cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
};
```

## <a id="encrypt-docdb"></a>Versleutelen van gevoelige gegevens die zijn opgeslagen in Cosmos-DB

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Documentdb | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Versleutelen van gevoelige gegevens op toepassingsniveau voordat u ze opslaan in een document DB of dat geen gevoelige gegevens worden opgeslagen in andere opslagoplossingen zoals Azure Storage of Azure SQL| 

## <a id="disk-vm"></a>Azure Disk Encryption gebruiken voor het versleutelen van schijven die worden gebruikt door virtuele Machines

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van Azure IaaS VM | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Met behulp van Azure Disk Encryption schijven die worden gebruikt door uw virtuele machines versleutelen](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Stappen** | <p>Azure Disk Encryption is een nieuwe functie die zich momenteel in preview. Deze functie kunt u de OS-schijven en gegevensschijven die wordt gebruikt door een virtuele Machine voor IaaS versleutelen. De stations zijn versleuteld met behulp van standaard-BitLocker-versleuteling technologie voor Windows. De schijven zijn versleuteld met behulp van de technologie DM-Crypt voor Linux. Dit is geïntegreerd met Azure Sleutelkluis kunt u te controleren en beheren van de versleutelingssleutels voor de schijf. De oplossing voor Azure Disk Encryption ondersteunt de volgende drie klant versleuteling scenario's:</p><ul><li>Schakel versleuteling in op de nieuwe IaaS VM's die worden gemaakt op basis van de klant versleuteld VHD-bestanden en klant geleverde versleutelingssleutels die zijn opgeslagen in Azure Sleutelkluis.</li><li>Schakel versleuteling in op de nieuwe IaaS VM's die worden gemaakt vanuit Azure Marketplace.</li><li>Schakel versleuteling in op bestaande IaaS VM's al wordt uitgevoerd in Azure.</li></ul>| 

## <a id="fabric-apps"></a>Versleutelen van geheimen in Service Fabric-toepassingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Service Fabric-Vertrouwensgrenzen | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure |
| **Verwijzingen**              | [Het beheren van geheimen in Service Fabric-toepassingen](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Stappen** | Geheimen mag gevoelige informatie, zoals de storage-verbindingsreeksen, wachtwoorden of andere waarden die niet moeten worden behandeld als tekst zonder opmaak. Gebruik Azure Key Vault voor het beheren van sleutels en geheimen in service fabric-toepassingen. |

## <a id="modeling-teams"></a>Beveiliging modellering uitvoeren en zakelijke eenheden/Teams gebruiken indien vereist

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Beveiliging modellering uitvoeren en zakelijke eenheden/Teams gebruiken indien vereist |

## <a id="entities"></a>Beperken van toegang tot het onderdeel op Kritiek entiteiten delen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Beperken van toegang tot het onderdeel op Kritiek entiteiten delen |

## <a id="good-practices"></a>Gebruikers op de risico's die zijn gekoppeld aan de Dynamics CRM-Share-functie en de juiste beveiligingsprocedures trainen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Gebruikers op de risico's die zijn gekoppeld aan de Dynamics CRM-Share-functie en de juiste beveiligingsprocedures trainen |

## <a id="exception-mgmt"></a>Een ontwikkeling standaarden regel met config details in Uitzonderingsbeheer proscribing opnemen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Een regel voor het standaarden van ontwikkeling met config details in Uitzonderingsbeheer buiten ontwikkeling proscribing opnemen. Test voor deze als onderdeel van de beoordelingen code of periodieke controle.|

## <a id="sse-preview"></a>Versleuteling van Azure Storage-Service (SSE) gebruiken voor Data-at-Rest (Preview)

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - Blob |
| **Verwijzingen**              | [Azure Storage-Service: versleuteling voor Data-at-Rest (Preview)](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Stappen** | <p>Azure Storage Service versleuteling (SSE) voor gegevens in rust helpt u bij het beveiligen en bescherming van uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving verplichtingen. Met deze functie wordt Azure Storage automatisch versleutelt uw gegevens voordat het behouden blijven van opslag en ontsleutelt vóór ophalen. De versleuteling, ontsleuteling en sleutelbeheer is volledig transparant voor gebruikers. SSE is alleen van toepassing op blok-blobs, pagina-blobs en toevoeg-blobs. De andere typen gegevens, waaronder tabellen, wachtrijen en -bestanden wordt niet versleuteld.</p><p>Versleuteling en ontsleuteling werkstroom:</p><ul><li>De klant schakelt u versleuteling in de storage-account</li><li>Wanneer de klant nieuwe gegevens (Blob plaatsen, blok plaatsen, pagina plaatsen, enz.) schrijft naar Blob storage; elke schrijfbewerking is versleuteld met behulp van 256-bits AES-versleuteling, een van de sterkste blokcodering beschikbaar</li><li>Wanneer de klant moet voor toegang tot gegevens (Blob ophalen, enzovoort), gegevens worden automatisch ontsleuteld voordat u terugkeert naar de gebruiker</li><li>Als versleuteling is uitgeschakeld, nieuwe schrijfbewerkingen zijn niet langer gecodeerd en bestaande versleutelde gegevens blijven versleuteld totdat herschreven door de gebruiker. Terwijl versleuteling is ingeschakeld, worden schrijfbewerkingen naar Blob storage wordt versleuteld. De status van de gegevens wijzigen niet met de gebruiker schakelen tussen in-/ uitschakelen versleuteling voor het opslagaccount</li><li>Alle versleutelingssleutels zijn opgeslagen, gecodeerd en beheerd door Microsoft</li></ul><p>Houd er rekening mee dat op dit moment de sleutels die worden gebruikt voor de codering worden beheerd door Microsoft. Microsoft oorspronkelijk de sleutels genereert en beheren van de veilige opslag van de sleutels, evenals de reguliere draaiing zoals gedefinieerd door intern beleid van Microsoft. In de toekomst klanten krijgen de mogelijkheid voor het beheren van hun eigen > versleutelingssleutels, en geef een migratiepad van door Microsoft beheerde sleutels aan de klant beheerd sleutels.</p>| 

## <a id="client-storage"></a>Client-Side-versleuteling gebruiken voor het opslaan van gevoelige gegevens in Azure Storage

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [zelfstudie: blobs in Microsoft Azure Storage met Azure Key Vault versleutelen en ontsleutelen](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/), [gegevens veilig opslaan in Azure Blob Opslag met versleuteling van Azure-extensies](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) |
| **Stappen** | <p>De Azure Storage-clientbibliotheek voor .NET-Nuget-pakket ondersteunt de versleuteling van gegevens binnen clienttoepassingen voordat uploaden naar Azure Storage en ontsleutelen van gegevens tijdens het downloaden van de client. De bibliotheek ondersteunt ook integratie met Azure Key Vault voor het beheer van sleutels voor opslagaccounts. Hier volgt een korte beschrijving van de werking van versleuteling aan de clientzijde:</p><ul><li>De client-SDK van Azure Storage genereert een inhoud versleutelingssleutel (CEK), die een symmetrische sleutel met een eenmalig gebruik</li><li>Gegevens van de klant is versleuteld met behulp van deze CEK</li><li>De CEK wordt vervolgens verpakt (versleuteld) met behulp van de belangrijkste versleutelingssleutel (KEK-sleutel). De KEK wordt aangeduid met een sleutel-id en een asymmetrisch sleutelpaar of een symmetrische sleutel en kan worden beheerd lokaal of opgeslagen in Azure Sleutelkluis. De opslag-client zichzelf heeft nooit toegang tot de KEK. Het aanroept zojuist de belangrijkste wrapping algoritme die wordt geleverd door Sleutelkluis. Klanten kunnen kiezen voor het gebruik van aangepaste providers voor sleutel als ze willen wrapping/uitpakken</li><li>De versleutelde gegevens is vervolgens naar de Azure Storage-service geüpload. Controleer de koppelingen in de sectie Verwijzingen voor op laag niveau implementatiegegevens.</li></ul>|

## <a id="pii-phones"></a>Versleutelen van gevoelige of persoonlijke gegevens geschreven naar de lokale opslag telefoons

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele clients | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, Xamarin  |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Instellingen en functies op uw apparaten met Microsoft Intune-beleid beheren](https://docs.microsoft.com/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies#create-a-configuration-policy), [sleutelhanger Valet](https://components.xamarin.com/view/square.valet) |
| **Stappen** | <p>Als de toepassing schrijft gevoelige informatie, zoals PII van gebruiker (e-mailadres, telefoonnummer, voornaam, achternaam, voorkeuren enz.) -in het bestandssysteem van mobile, waarna deze moet worden versleuteld voordat het schrijven naar het lokale bestandssysteem. Als de toepassing een bedrijfstoepassing is, klikt u vervolgens de mogelijkheid van publishing toepassing met behulp van Windows Intune te verkennen.</p>|

### <a name="example"></a>Voorbeeld
Intune kan worden geconfigureerd met de volgende beveiligingsbeleid ter bescherming van gevoelige gegevens: 
```C#
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Voorbeeld
Als de toepassing is niet een bedrijfstoepassing, vervolgens gebruiken platform opgegeven sleutelopslag, sleutelhangers versleutelingssleutels met behulp van welke cryptografische bewerking kunnen worden uitgevoerd op het bestandssysteem. Volgende codefragment ziet u hoe voor toegang tot de sleutel van de sleutelhanger met xamarin: 
```C#
        protected static string EncryptionKey
        {
            get
            {
                if (String.IsNullOrEmpty(_Key))
                {
                    var query = new SecRecord(SecKind.GenericPassword);
                    query.Service = NSBundle.MainBundle.BundleIdentifier;
                    query.Account = "UniqueID";

                    NSData uniqueId = SecKeyChain.QueryAsData(query);
                    if (uniqueId == null)
                    {
                        query.ValueData = NSData.FromString(System.Guid.NewGuid().ToString());
                        var err = SecKeyChain.Add(query);
                        _Key = query.ValueData.ToString();
                    }
                    else
                    {
                        _Key = uniqueId.ToString();
                    }
                }

                return _Key;
            }
        }
```

## <a id="binaries-end"></a>Verbergen, gegenereerde binaire bestanden voordat u distribueert naar eindgebruikers weergeven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele clients | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Crypto codeversleuteling voor .net](http://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Stappen** | Gegenereerde binaire bestanden (assembly's in apk) moeten worden verborgen als u wilt stoppen reverse-engineering van assembly's. Hulpprogramma's zoals `CryptoObfuscator` kan worden gebruikt voor dit doel. |

## <a id="cert"></a>ClientCredentialType ingesteld op Certificate of Windows

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET framework 3 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Voeg](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | Met behulp van een UsernameToken met een wachtwoord als leesbare tekst via een niet-versleutelde kanaal wordt het wachtwoord voor aanvallers die de SOAP-berichten kunt netwerkhulpprogramma. Serviceproviders die gebruikmaken van de UsernameToken mogelijk accepteren wachtwoorden als leesbare tekst verzonden. De referentie voor aanvallen die het SOAP-bericht kunt netwerkhulpprogramma kan worden blootgesteld ongecodeerde wachtwoorden via een niet-versleutelde kanaal te verzenden. | 

### <a name="example"></a>Voorbeeld
De volgende providerconfiguratie van de WCF-service gebruikt de UsernameToken: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
ClientCredentialType ingesteld op Certificate of Windows. 

## <a id="security"></a>WCF-beveiligingsmodus is niet ingeschakeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, .NET Framework 3 |
| **Kenmerken**              | Beveiliging modus - Transport, beveiligingsmodus - bericht |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Koninkrijk Voeg](https://vulncat.fortify.com/en/vulncat/index.html), [basisprincipes van beveiliging WCF CoDe Magazine](http://www.codemag.com/article/0611051) |
| **Stappen** | Er is geen beveiliging transport of het bericht is gedefinieerd. Toepassingen die berichten zonder transport of bericht beveiliging kan niet worden gegarandeerd de integriteit of vertrouwelijkheid van de berichten worden verzonden. Wanneer een WCF-security-binding is ingesteld op None, worden zowel transport en bericht-beveiliging uitgeschakeld. |

### <a name="example"></a>Voorbeeld
De volgende configuratie wordt de beveiligingsmodus ingesteld op None. 
```
<system.serviceModel> 
  <bindings> 
    <wsHttpBinding> 
      <binding name=""MyBinding""> 
        <security mode=""None""/> 
      </binding> 
  </bindings> 
</system.serviceModel> 
```

### <a name="example"></a>Voorbeeld
Beveiligingsmodus over alle servicebindingen er zijn vijf mogelijke beveiligingsmodi: 
* Geen. Beveiliging wordt uitgeschakeld. 
* Transport. Maakt gebruik van transport beveiliging voor wederzijdse verificatie en bericht-beveiliging. 
* Bericht. Maakt gebruik van berichtbeveiliging voor wederzijdse verificatie- en beveiliging. 
* Beide. Hiermee kunt u opgeven van instellingen voor het transport en bericht-beveiliging (alleen MSMQ ondersteunt dit). 
* TransportWithMessageCredential. Referenties worden doorgegeven met het bericht en de bericht-beveiliging en verificatie van de server worden geleverd door de transportlaag. 
* TransportCredentialOnly. Clientreferenties worden doorgegeven aan de transportlaag is opgehaald en geen bericht beveiliging wordt toegepast. Gebruik beveiliging transport en het bericht ter bescherming van de integriteit en vertrouwelijkheid van berichten. De onderstaande configuratie geeft aan dat de service om transportbeveiliging te gebruiken met bericht referenties.
```
<system.serviceModel>
  <bindings>
    <wsHttpBinding>
    <binding name=""MyBinding""> 
    <security mode=""TransportWithMessageCredential""/> 
    <message clientCredentialType=""Windows""/> 
    </binding> 
  </bindings> 
</system.serviceModel> 
```
