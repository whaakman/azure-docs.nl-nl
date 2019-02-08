---
title: Gevoelige gegevens - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: oplossingen voor bedreigingen die beschikbaar zijn in de Threat Modeling Tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 5e9104f59173c3d39ef2f2232ed2a9c6864cf84f
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892555"
---
# <a name="security-frame-sensitive-data--mitigations"></a>Beveiliging-Frame: Gevoelige gegevens | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Vertrouwensgrenzen van computer** | <ul><li>[Zorg ervoor dat de binaire bestanden worden verborgen als ze gevoelige informatie bevatten](#binaries-info)</li><li>[U kunt met behulp van versleuteld bestand System (EFS) wordt gebruikt voor het beveiligen van vertrouwelijke gegevens van specifieke gebruikers](#efs-user)</li><li>[Zorg ervoor dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestandssysteem is versleuteld](#filesystem)</li></ul> | 
| **Web-App** | <ul><li>[Zorg ervoor dat gevoelige inhoud niet in cache in de browser opgeslagen is](#cache-browser)</li><li>[Secties van Web-App-configuratiebestanden met gevoelige gegevens versleutelen](#encrypt-data)</li><li>[Het automatisch aanvullen HTML-kenmerk in gevoelige formulieren en invoer expliciet uitschakelen](#autocomplete-input)</li><li>[Zorg ervoor dat gevoelige gegevens die worden weergegeven op het scherm van de gebruiker wordt gemaskeerd](#data-mask)</li></ul> | 
| **Database** | <ul><li>[Dynamische gegevensmaskering implementeren om te voorkomen dat gevoelige gegevens niet in beschermde modus blootstelling gebruikers](#dynamic-users)</li><li>[Zorg ervoor dat wachtwoorden worden opgeslagen in gezouten hash-indeling](#salted-hash)</li><li>[ Zorg ervoor dat gevoelige gegevens in de databasekolommen is versleuteld](#db-encrypted)</li><li>[Zorg ervoor dat op databaseniveau encryption (TDE) is ingeschakeld](#tde-enabled)</li><li>[Ervoor zorgen dat databaseback-ups worden gecodeerd](#backup)</li></ul> | 
| **Web-API** | <ul><li>[Zorg ervoor dat gevoelige gegevens die relevant zijn voor Web-API niet zijn opgeslagen in de opslag van browser](#api-browser)</li></ul> | 
| Azure Document DB | <ul><li>[Versleutelen van gevoelige gegevens die zijn opgeslagen in Azure Cosmos DB](#encrypt-docdb)</li></ul> | 
| **Vertrouwensgrenzen van Azure IaaS-VM** | <ul><li>[Azure Disk Encryption gebruiken voor het versleutelen van schijven die worden gebruikt door virtuele Machines](#disk-vm)</li></ul> | 
| **Vertrouwensgrenzen van service Fabric** | <ul><li>[Versleutelen van geheimen in Service Fabric-toepassingen](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Uitvoeren van beveiliging modelleren en zakelijke eenheden/Teams gebruiken indien nodig](#modeling-teams)</li><li>[Beperkt u de toegangsrechten voor het delen van de functie voor kritieke entiteiten](#entities)</li><li>[Gebruikers van de trein op de risico's die zijn gekoppeld aan de Dynamics CRM-Share-functie en de procedures voor goede beveiliging](#good-practices)</li><li>[Een ontwikkeling standaarden regel met config details in Uitzonderingsbeheer proscribing opnemen](#exception-mgmt)</li></ul> | 
| **Azure Storage** | <ul><li>[Azure Storage Service Encryption (SSE) gebruiken voor Data-at-Rest (Preview)](#sse-preview)</li><li>[Client-Side-versleuteling gebruiken voor het opslaan van gevoelige gegevens in Azure Storage](#client-storage)</li></ul> | 
| **Mobiele Client** | <ul><li>[Versleutelen van gevoelige of persoonlijke gegevens geschreven naar de lokale opslag voor telefoons](#pii-phones)</li><li>[Gegenereerde binaire bestanden, onleesbaar maakt voordat u distribueert voor eindgebruikers](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[ ClientCredentialType ingesteld op certificaten of Windows](#cert)</li><li>[WCF-beveiligingsmodus is niet ingeschakeld](#security)</li></ul> | 

## <a id="binaries-info"></a>Zorg ervoor dat de binaire bestanden worden verborgen als ze gevoelige informatie bevatten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van computer | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat de binaire bestanden worden verborgen als deze bevatten vertrouwelijke gegevens, zoals handelsgeheimen, gevoelige zakelijke logica die moet niet omgekeerd. Dit is om te stoppen reverse-engineering van assembly's. Hulpprogramma's zoals `CryptoObfuscator` kan worden gebruikt voor dit doel. |

## <a id="efs-user"></a>U kunt met behulp van versleuteld bestand System (EFS) wordt gebruikt voor het beveiligen van vertrouwelijke gegevens van specifieke gebruikers

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van computer | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | U kunt u vertrouwelijke gebruikersspecifieke gegevens beschermen tegen aanvallers met fysieke toegang tot de computer met behulp van versleuteld bestand System (EFS) wordt gebruikt. |

## <a id="filesystem"></a>Zorg ervoor dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestandssysteem is versleuteld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van computer | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Ervoor zorgen dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestandssysteem is gecodeerd (bijvoorbeeld met behulp van DPAPI), als EFS kunnen niet worden afgedwongen. |

## <a id="cache-browser"></a>Zorg ervoor dat gevoelige inhoud niet in cache in de browser opgeslagen is

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Generic, Web Forms, MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Browsers kunnen gegevens voor de doeleinden van caching en geschiedenis opslaan. Deze bestanden in de cache worden opgeslagen in een map, zoals de map Tijdelijke internetbestanden in het geval van Internet Explorer. Wanneer deze pagina's opnieuw worden aangeduid, worden deze door de browser uit de cache weergegeven. Als gevoelige informatie wordt weergegeven voor de gebruiker (zoals het adres, creditcardgegevens, sociaal-fiscaal nummer of gebruikersnaam), kan klikt u vervolgens deze informatie worden opgeslagen in de cache van de webbrowser, en daarom worden opgehaald via het onderzoeken van de cache van de browser of door op de knop 'Terug' in de browser te drukken. Cache-control-antwoord header-waarde 'no-store"voor alle pagina's instellen. |

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
```csharp
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
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Procedure: Versleutelen Netwerkconfiguratiesecties in ASP.NET 2.0 met behulp van DPAPI](https://msdn.microsoft.com/library/ff647398.aspx), [op te geven een Configuratieprovider beveiligd](https://msdn.microsoft.com/library/68ze1hb2.aspx), [met behulp van Azure Key Vault om toepassingsgeheimen te beveiligen](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Stappen** | Configuratie van bestanden, als het bestand Web.config, appsettings.json vaak worden gebruikt voor het opslaan van gevoelige gegevens, zoals gebruikersnamen, wachtwoorden, databaseverbindingsreeksen en versleutelingssleutels. Als u deze informatie niet beveiligt, is uw toepassing kwetsbaar zijn voor aanvallers of kwaadwillende gebruikers die gevoelige gegevens, zoals gebruikersnamen en wachtwoorden, databasenamen en servernamen. De gevoelige secties van de config-bestanden met behulp van DPAPI- of -services zoals Azure Key Vault op basis van het implementatietype (azure/on-premises), worden versleuteld. |

## <a id="autocomplete-input"></a>Het automatisch aanvullen HTML-kenmerk in gevoelige formulieren en invoer expliciet uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN: automatisch aanvullen kenmerk](https://msdn.microsoft.com/library/ms533486(VS.85).aspx), [automatisch aanvullen gebruiken in HTML](https://msdn.microsoft.com/library/ms533032.aspx), [beveiligingslek met betrekking tot het opschonen van het HTML-](https://technet.microsoft.com/security/bulletin/MS10-071), [automatisch aanvullen., opnieuw?!](http://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Stappen** | Het automatisch aanvullen-kenmerk geeft aan of een formulier automatisch aanvullen in- of uitschakelen zijn moet. Als automatisch aanvullen is ingeschakeld, worden de waarden op basis van waarden die de gebruiker heeft ingevoerd voordat de browser automatisch aanvullen. Bijvoorbeeld, wanneer een nieuwe naam en het wachtwoord is ingevoerd in een formulier en het formulier wordt ingediend, de browser wordt gevraagd als het wachtwoord moet worden opgeslagen. Daarna wanneer het formulier wordt weergegeven, de naam en het wachtwoord worden automatisch ingevuld of zijn voltooid, zoals de naam van de opgegeven. Een aanvaller met lokale toegang, kan het wachtwoord niet-versleutelde tekst ophalen uit de cache van de browser. Automatisch aanvullen is standaard ingeschakeld en moet expliciet worden uitgeschakeld. |

### <a name="example"></a>Voorbeeld
```csharp
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
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Gevoelige gegevens zoals wachtwoorden, creditcardnummers, SSN enzovoort moet worden gemaskeerd wanneer weergegeven op het scherm. Dit is om te voorkomen dat onbevoegden toegang tot de gegevens (bijvoorbeeld schouder meekijkt surfen wachtwoorden, ondersteunend personeel sofi-nummer en de aantallen gebruikers weergeven). Zorg ervoor dat de elementen van deze gegevens niet zichtbaar in tekst zonder opmaak zijn en op de juiste wijze worden gemaskeerd. Dit moet worden afgehandeld tijdens het akkoord gaan ze als invoer (bijvoorbeeld,). input type = "password") en weer terug op het scherm (bijvoorbeeld weergave alleen de laatste 4 cijfers van het creditcardnummer). |

## <a id="dynamic-users"></a>Dynamische gegevensmaskering implementeren om te voorkomen dat gevoelige gegevens niet in beschermde modus blootstelling gebruikers

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Sql Azure, OnPrem |
| **Kenmerken**              | SQL-versie - V12, versie van SQL - MsSQL2016 |
| **Verwijzingen**              | [Dynamische Gegevensmaskering](https://msdn.microsoft.com/library/mt130841) |
| **Stappen** | Het doel van dynamische gegevensmaskering is om te beperken van blootstelling van gevoelige gegevens, te voorkomen dat gebruikers geen toegang tot de gegevens hebben mogen kunnen bekijken. Dynamische gegevensmaskering beoogt niet om te voorkomen dat databasegebruikers van directe verbinding met de database en uitvoeren van uitgebreide query's die beschikbaar maken van de gevoelige gegevens. Dynamische gegevensmaskering is een aanvulling op andere beveiligingsfuncties van SQL Server (controle, versleuteling en beveiliging op rijniveau...) en is het raadzaam deze functie wilt gebruiken in combinatie met hen ook betere beveiliging van de gevoelige gegevens in de de database. Houd er rekening mee dat deze functie wordt alleen ondersteund door SQL Server vanaf 2016 en Azure SQL Database. |

## <a id="salted-hash"></a>Zorg ervoor dat wachtwoorden worden opgeslagen in gezouten hash-indeling

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Hashing van wachtwoord met behulp van .NET Crypto-API 's](http://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Stappen** | Wachtwoorden moeten niet worden opgeslagen in de store van aangepaste gebruikersdatabases. Wachtwoord-hashes moeten in plaats daarvan met salt waarden worden opgeslagen. Zorg ervoor dat de salt voor de gebruiker altijd uniek is en u b-crypt, s-crypt of PBKDF2 toepassen voordat u het wachtwoord, met een minimale werk factor iteratie telling van 150.000 lussen te voorkomen dat de van brute force opslaat.| 

## <a id="db-encrypted"></a>Zorg ervoor dat gevoelige gegevens in de databasekolommen is versleuteld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Versie van SQL - alle |
| **Verwijzingen**              | [Versleutelen van gevoelige gegevens in SQL server](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [het: Een kolom met gegevens in SQL Server versleutelen](https://msdn.microsoft.com/library/ms179331), [versleutelen met certificaat](https://msdn.microsoft.com/library/ms188061) |
| **Stappen** | Gevoelige gegevens, zoals creditcardnummers moet worden gecodeerd in de database. Gegevens kunnen worden versleuteld met behulp van versleuteling op kolomniveau of door de functie van een toepassing met behulp van de functies voor versleuteling. |

## <a id="tde-enabled"></a>Zorg ervoor dat op databaseniveau encryption (TDE) is ingeschakeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Wat is SQL Server transparante gegevensversleuteling (TDE)?](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Stappen** | Transparante gegevensversleuteling (TDE)-functie in SQL server helpt bij het versleutelen van gevoelige gegevens in een database en de sleutels die worden gebruikt voor het versleutelen van de gegevens met een certificaat te beschermen. Dit voorkomt personen zonder de sleutels met behulp van de gegevens. De gegevens worden beschermd door TDE 'in rust', wat betekent dat de gegevens en logboekbestanden bestanden. Biedt de mogelijkheid om te voldoen aan veel wetgeving, regelgeving en richtlijnen voor het tot stand gebracht in verschillende branches. |

## <a id="backup"></a>Ervoor zorgen dat databaseback-ups worden gecodeerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, OnPrem |
| **Kenmerken**              | SQL-versie - V12, versie van SQL - MsSQL2014 |
| **Verwijzingen**              | [SQL database-back-upversleuteling](https://msdn.microsoft.com/library/dn449489) |
| **Stappen** | SQL Server heeft de mogelijkheid voor het versleutelen van de gegevens tijdens het maken van een back-up. Door het versleutelingsalgoritme en de encryptor (een certificaat of asymmetrische sleutel) op te geven bij het maken van een back-up, een gecodeerde back-upbestand kan worden gemaakt. |

## <a id="api-browser"></a>Zorg ervoor dat gevoelige gegevens die relevant zijn voor Web-API niet zijn opgeslagen in de opslag van browser

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC 5, MVC 6 |
| **Kenmerken**              | ID-Provider - ADFS, id-Provider - Azure AD |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>In bepaalde implementaties worden gevoelige artefacten die relevant zijn voor verificatie van de Web-API opgeslagen in de lokale opslag van de browser. Bijvoorbeeld, zoals Azure AD-verificatie artefacten adal.idtoken, adal.nonce.idtoken, adal.access.token.key, adal.token.keys, adal.state.login, adal.session.state, adal.expiration.key enzovoort.</p><p>Alle deze artefacten zijn beschikbaar, zelfs na het afmelden of de browser is gesloten. Als een kwaadwillende persoon toegang tot deze artefacten, komt opnieuw kan worden gebruikt voor toegang tot de beveiligde resources (API's). Zorg ervoor dat alle gevoelige artefacten met betrekking tot Web-API niet zijn opgeslagen in de opslag van de browser. In gevallen waarin de client-side-opslag onvermijdelijk is (bijvoorbeeld één pagina toepassingen (SPA) die gebruikmaken van de impliciete OpenIdConnect/OAuth stromen nodig voor het opslaan van lokaal toegangstokens), opslag-opties voor gebruik met nog geen persistentie. bijvoorbeeld, liever SessionStorage LocalStorage.</p>| 

### <a name="example"></a>Voorbeeld
De onderstaande JavaScript codefragment is van een bibliotheek met aangepaste verificatie waarbij verificatie artefacten in lokale opslag worden opgeslagen. Deze implementaties moeten worden vermeden. 
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

## <a id="encrypt-docdb"></a>Versleutelen van gevoelige gegevens die zijn opgeslagen in Cosmos DB

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Document DB | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Versleutelen van gevoelige gegevens op het toepassingsniveau van de voordat u opslaat in document DB of gevoelige gegevens opslaan in andere opslagoplossingen zoals Azure Storage of Azure SQL| 

## <a id="disk-vm"></a>Azure Disk Encryption gebruiken voor het versleutelen van schijven die worden gebruikt door virtuele Machines

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van Azure IaaS-VM | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Met behulp van Azure Disk Encryption voor het versleutelen van schijven die worden gebruikt door uw virtuele machines](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Stappen** | <p>Azure Disk Encryption is een nieuwe functie die momenteel in preview. Deze functie kunt u voor het versleutelen van de besturingssysteemschijven en gegevensschijven die zijn gebruikt door een virtuele IaaS-Machine. De stations zijn versleuteld met versleutelingstechnologie van industriestandaard BitLocker voor Windows. De schijven worden versleuteld met behulp van de technologie DM-Crypt voor Linux. Dit is geïntegreerd met Azure Key Vault kunt u controleren en beheren van de versleutelingssleutels voor de schijf. De Azure Disk Encryption-oplossing ondersteunt de volgende drie klant versleuteling scenario's:</p><ul><li>Hiermee schakelt u versleuteling op nieuwe IaaS VM's die worden gemaakt op basis van VHD-bestanden klant versleuteld en klant versleutelingssleutels, die zijn opgeslagen in Azure Key Vault.</li><li>Hiermee schakelt u versleuteling op nieuwe IaaS VM's die worden gemaakt op basis van de Azure Marketplace.</li><li>Hiermee schakelt u versleuteling op bestaande IaaS-VM's al wordt uitgevoerd in Azure.</li></ul>| 

## <a id="fabric-apps"></a>Versleutelen van geheimen in Service Fabric-toepassingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van service Fabric | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure |
| **Verwijzingen**              | [Geheimen in Service Fabric-toepassingen beheren](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Stappen** | Geheimen kunnen gevoelige informatie, zoals storage-verbindingsreeksen, wachtwoorden, of andere waarden die niet moeten worden verwerkt als tekst zonder opmaak zijn. Azure Key Vault gebruiken voor het beheren van sleutels en geheimen in service fabric-toepassingen. |

## <a id="modeling-teams"></a>Uitvoeren van beveiliging modelleren en zakelijke eenheden/Teams gebruiken indien nodig

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Uitvoeren van beveiliging modelleren en zakelijke eenheden/Teams gebruiken indien nodig |

## <a id="entities"></a>Beperkt u de toegangsrechten voor het delen van de functie voor kritieke entiteiten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Beperkt u de toegangsrechten voor het delen van de functie voor kritieke entiteiten |

## <a id="good-practices"></a>Gebruikers van de trein op de risico's die zijn gekoppeld aan de Dynamics CRM-Share-functie en de procedures voor goede beveiliging

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Gebruikers van de trein op de risico's die zijn gekoppeld aan de Dynamics CRM-Share-functie en de procedures voor goede beveiliging |

## <a id="exception-mgmt"></a>Een ontwikkeling standaarden regel met config details in Uitzonderingsbeheer proscribing opnemen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Een regel voor het standaarden van ontwikkeling met config details in Uitzonderingsbeheer buiten ontwikkeling proscribing opnemen. Test voor deze als onderdeel van, coderevisies of periodieke controle.|

## <a id="sse-preview"></a>Azure Storage Service Encryption (SSE) gebruiken voor Data-at-Rest (Preview)

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - Blob |
| **Verwijzingen**              | [Azure Storage-Serviceversleuteling voor Data-at-Rest (Preview)](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Stappen** | <p>Azure Storage Service Encryption (SSE) voor Data-at-Rest helpt u bij het beschermen en beveiligen van uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsverplichtingen. Met deze functie worden uw gegevens automatisch versleuteld door Azure Storage voordat deze worden opgeslagen en ontsleutelt voordat ze weer worden opgehaald. De versleuteling, ontsleuteling en sleutelbeheer is volledig transparant voor gebruikers. SSE is alleen van toepassing op blok-blobs, pagina-blobs en toevoeg-blobs. De andere typen gegevens, waaronder tabellen, wachtrijen en -bestanden niet versleuteld.</p><p>Versleuteling en ontsleuteling werkstroom:</p><ul><li>De klant Hiermee schakelt u versleuteling in de storage-account</li><li>Wanneer de klant nieuwe gegevens (PUT Blob, PUT-blokken, pagina plaatsen, enzovoort) schrijft naar Blob-opslag; elke schrijfbewerking is versleuteld met 256-bits AES-versleuteling, een van de krachtigste blokversleutelingsmethoden die</li><li>Wanneer de klant nodig heeft voor toegang tot gegevens (Blob ophalen, enzovoort), gegevens worden automatisch ontsleuteld voordat u terugkeert naar de gebruiker</li><li>Als versleuteling is uitgeschakeld, nieuwe schrijfbewerkingen zijn niet langer gecodeerd en bestaande versleutelde gegevens blijven versleuteld totdat opnieuw geschreven door de gebruiker. Versleuteling is ingeschakeld, kunnen schrijfbewerkingen naar de Blob-opslag, worden versleuteld. De status van gegevens wijzigen niet met de gebruiker bij het omschakelen van tussen de in-/ uitschakelen versleuteling voor het opslagaccount</li><li>Alle versleutelingssleutels worden opgeslagen, versleuteld en beheerd door Microsoft</li></ul><p>Houd er rekening mee dat de sleutels die worden gebruikt voor de versleuteling op dit moment door Microsoft worden beheerd. Microsoft oorspronkelijk de sleutels genereert en beheren van de beveiligde opslag van de sleutels, evenals de draaihoek van het normale zoals gedefinieerd door het interne Microsoft-beleid. In de toekomst krijgen ondernemingen de mogelijkheid voor het beheren van hun eigen > sleutels voor gegevenscodering, en geef een migratiepad van Microsoft beheerde sleutels die naar de klant beheerde sleutels.</p>| 

## <a id="client-storage"></a>Client-Side-versleuteling gebruiken voor het opslaan van gevoelige gegevens in Azure Storage

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [zelfstudie: Blobs in Microsoft Azure Storage met behulp van Azure Key Vault versleutelen en ontsleutelen](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/), [gegevens veilig opslaan in Azure Blob-opslag met de extensies voor Azure-versleuteling](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) |
| **Stappen** | <p>De Azure Storage-clientbibliotheek voor .NET Nuget-pakket ondersteunt de versleuteling van gegevens binnen clienttoepassingen voordat u uploadt naar Azure Storage en de ontsleuteling van gegevens tijdens het downloaden van de client. De bibliotheek ondersteunt ook integratie met Azure Key Vault voor het beheer van sleutels voor opslagaccounts. Hier volgt een korte beschrijving van de werking van versleuteling aan de clientzijde:</p><ul><li>De Azure Storage client-SDK genereert een sleutel met versleuteling van inhoud (CEK), dit een symmetrische sleutel van één permanente gebruiken is</li><li>Gegevens van de klant is versleuteld met behulp van deze CEK</li><li>De CEK wordt vervolgens verpakt (versleuteld) met behulp van de sleutel van versleutelingssleutel (KEK-sleutel). De KEK kan wordt aangeduid met een sleutel-id en een asymmetrisch sleutelpaar of een symmetrische sleutel en kan worden lokaal beheerd of die zijn opgeslagen in Azure Key Vault. De opslag-client zichzelf heeft nooit toegang tot de KEK-sleutel. Deze roept alleen de belangrijkste wrapping-algoritme die wordt geleverd door Key Vault. Klanten kunnen kiezen voor aangepaste providers voor sleutel verpakken/uitpakken als ze willen gebruiken</li><li>De versleutelde gegevens wordt vervolgens naar de Azure Storage-service geüpload. Controleer de koppelingen in de sectie Verwijzingen van laag niveau implementatiegegevens.</li></ul>|

## <a id="pii-phones"></a>Versleutelen van gevoelige of persoonlijke gegevens geschreven naar de lokale opslag voor telefoons

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele Client | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Generic, Xamarin  |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Instellingen en functies op uw apparaten met Microsoft Intune-beleid beheren](https://docs.microsoft.com/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies), [sleutelhanger Valet](https://components.xamarin.com/view/square.valet) |
| **Stappen** | <p>Als de toepassing schrijft gevoelige informatie, zoals PII van gebruiker (e-mailadres, telefoonnummer, voornaam, achternaam, voorkeuren enz.) -in het bestandssysteem van mobile, klikt u vervolgens deze moet worden versleuteld voordat het schrijven naar het lokale bestandssysteem. Als de toepassing een bedrijfstoepassing is, klikt u vervolgens de mogelijkheid om publiceren met behulp van Windows Intune-toepassing te bekijken.</p>|

### <a name="example"></a>Voorbeeld
Intune kan worden geconfigureerd met de volgende beveiligingsbeleid ter bescherming van gevoelige gegevens: 
```csharp
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Voorbeeld
Als de toepassing is niet een bedrijfstoepassing, klikt u vervolgens gebruiken platform opgegeven keystore, sleutelhangers versleutelingssleutels, met behulp van welke cryptografische bewerking kunnen worden uitgevoerd op het bestandssysteem. Volgende codefragment toont hoe u toegang tot de sleutel van de sleutelhanger met xamarin: 
```csharp
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

## <a id="binaries-end"></a>Gegenereerde binaire bestanden, onleesbaar maakt voordat u distribueert voor eindgebruikers

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele Client | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Crypto codeversleuteling voor .net](http://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Stappen** | Gegenereerde binaire bestanden (assembly's in apk) moeten worden verborgen als u wilt stoppen, reverse-engineering van assembly's. Hulpprogramma's zoals `CryptoObfuscator` kan worden gebruikt voor dit doel. |

## <a id="cert"></a>ClientCredentialType ingesteld op certificaten of Windows

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Voeg](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_token) |
| **Stappen** | Met behulp van een UsernameToken met een wachtwoord als leesbare tekst via een niet-versleutelde kanaal wordt aangegeven dat het wachtwoord voor kwaadwillende gebruikers die de SOAP-berichten kunt netwerkhulpprogramma. Serviceproviders die gebruikmaken van de UsernameToken mogelijk accepteren wachtwoorden als leesbare tekst verzonden. Leesbare wachtwoorden verzenden via een niet-versleutelde kanaal kan de referentie op die moet kwaadwillende gebruikers die de SOAP-bericht kunt netwerkhulpprogramma worden blootgesteld. | 

### <a name="example"></a>Voorbeeld
De volgende WCF-service-providerconfiguratie maakt gebruik van de UsernameToken: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
Stel clientCredentialType op certificaat of Windows. 

## <a id="security"></a>WCF-beveiligingsmodus is niet ingeschakeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, .NET Framework 3 |
| **Kenmerken**              | Modus - Transport, beveiligingsmodus - bericht |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Koninkrijk Voeg](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference), [basisprincipes van beveiliging van WCF-CoDe Magazine](http://www.codemag.com/article/0611051) |
| **Stappen** | Er is geen beveiliging transport of het bericht is gedefinieerd. Toepassingen die berichten zonder transport of bericht beveiliging kan niet garanderen de integriteit of vertrouwelijkheid van de berichten worden verzonden. Wanneer een WCF-security-binding is ingesteld op None, worden zowel transport en bericht-beveiliging uitgeschakeld. |

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
Modus voor beveiliging voor alle servicebindingen er zijn vijf mogelijk beveiligingsmodi: 
* Geen. Beveiliging wordt uitgeschakeld. 
* Transport. Maakt gebruik van transport security voor wederzijdse verificatie en bericht-beveiliging. 
* Bericht. Maakt gebruik van berichtbeveiliging voor wederzijdse verificatie en bericht-beveiliging. 
* Beide. Hiermee kunt u instellingen voor het transport en bericht-beveiliging (alleen MSMQ ondersteunt dit) opgeven. 
* TransportWithMessageCredential. Referenties worden doorgegeven met het bericht en de bericht-beveiliging en verificatie van de server worden geleverd door de transportlaag is opgehaald. 
* TransportCredentialOnly. Clientreferenties worden doorgegeven met de transportlaag is opgehaald en er geen bericht-beveiliging wordt toegepast. Transport en bericht-beveiliging gebruiken om u te beschermen van de integriteit en vertrouwelijkheid van berichten. De onderstaande configuratie geeft aan dat de service moet transportbeveiliging gebruiken met de referenties van het bericht.
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
