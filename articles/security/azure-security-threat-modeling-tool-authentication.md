---
title: Verificatie - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
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
ms.openlocfilehash: 23e219fa49146158c97f392427eee7c42c347a7a
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307417"
---
# <a name="security-frame-authentication--mitigations"></a>Beveiliging-Frame: Verificatie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Web-App**    | <ul><li>[Overweeg het gebruik van een standaard-verificatiemechanisme voor verificatie bij de Web-App](#standard-authn-web-app)</li><li>[Toepassingen moeten omgaan met scenario's voor mislukte verificatie veilig](#handle-failed-authn)</li><li>[Stap van inschakelen of adaptieve verificatie](#step-up-adaptive-authn)</li><li>[Zorg ervoor dat administratieve interfaces op de juiste wijze zijn vergrendeld](#admin-interface-lockdown)</li><li>[Implementeer vergeten wachtwoord functionaliteiten veilig](#forgot-pword-fxn)</li><li>[Zorg ervoor dat de wachtwoord- en -beleid worden toegepast](#pword-account-policy)</li><li>[Besturingselementen om te voorkomen dat de gebruikersnaam (opsomming) worden geïmplementeerd.](#controls-username-enum)</li></ul> |
| **Database** | <ul><li>[Indien mogelijk, Windows-verificatie gebruiken om verbinding te maken met SQL Server](#win-authn-sql)</li><li>[Indien mogelijk gebruik van Azure Active Directory-verificatie om verbinding te maken met SQL-Database](#aad-authn-sql)</li><li>[Wanneer de modus SQL-authenticatie wordt gebruikt, zorgt u ervoor dat account en wachtwoord beleid worden afgedwongen op de SQL server](#authn-account-pword)</li><li>[Gebruik geen SQL-verificatie in ingesloten databases](#autn-contained-db)</li></ul> |
| **Azure Event Hub** | <ul><li>[Per apparaatreferenties voor verificatie met behulp van SaS-tokens gebruiken](#authn-sas-tokens)</li></ul> |
| **Vertrouwensgrenzen van Azure** | <ul><li>[Azure multi-factor Authentication voor Azure-beheerders inschakelen](#multi-factor-azure-admin)</li></ul> |
| **Vertrouwensgrenzen van service Fabric** | <ul><li>[Anonieme toegang beperken tot Service Fabric-Cluster](#anon-access-cluster)</li><li>[Zorg ervoor dat certificaat voor Service Fabric-client-naar-knooppunt van het certificaat van knooppunt-naar-knooppunt verschilt](#fabric-cn-nn)</li><li>[Gebruik van AAD om clients naar service fabric-clusters te verifiëren](#aad-client-fabric)</li><li>[Zorg ervoor dat de service fabric-certificaten zijn opgehaald uit een erkende certificeringsinstantie (CA)](#fabric-cert-ca)</li></ul> |
| **Identiteitsserver** | <ul><li>[Gebruik standaard verificatiescenario's die worden ondersteund door Identiteitsserver](#standard-authn-id)</li><li>[Standaard Identity token cachegeheugen van de Server met een schaalbare alternatief overschrijven](#override-token)</li></ul> |
| **Vertrouwensgrenzen van computer** | <ul><li>[Zorg ervoor dat de binaire waarden van geïmplementeerde toepassingen digitaal zijn ondertekend](#binaries-signed)</li></ul> |
| **WCF** | <ul><li>[Verificatie inschakelen bij het verbinden met de MSMQ-wachtrijen in WCF](#msmq-queues)</li><li>[Bericht clientCredentialType WCF-Do niet ingesteld op none](#message-none)</li><li>[Transport clientCredentialType WCF-Do niet ingesteld op none](#transport-none)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat de standaard verificatie technieken worden gebruikt voor het beveiligen van Web-API 's](#authn-secure-api)</li></ul> |
| **Azure AD** | <ul><li>[Standard verificatiescenario's die worden ondersteund door Azure Active Directory gebruiken](#authn-aad)</li><li>[De standaard ADAL-token-cache met een schaalbare alternatief overschrijven](#adal-scalable)</li><li>[Zorg ervoor dat TokenReplayCache wordt gebruikt om te voorkomen dat de herhaling van tokens voor ADAL-verificatie](#tokenreplaycache-adal)</li><li>[ADAL-bibliotheken voor het beheren van token aanvragen van OAuth2-clients naar AAD gebruiken (of on-premises AD)](#adal-oauth2)</li></ul> |
| **Veld voor IoT-Gateway** | <ul><li>[Verificatie van apparaten die verbinding maken met de Gateway van het veld](#authn-devices-field)</li></ul> |
| **IoT-Cloud-Gateway** | <ul><li>[Zorg ervoor dat apparaten verbinding maken met de cloudgateway worden geverifieerd](#authn-devices-cloud)</li><li>[Referenties voor verificatie per apparaat gebruiken](#authn-cred)</li></ul> |
| **Azure Storage** | <ul><li>[Zorg ervoor dat alleen de benodigde containers en blobs anonieme leestoegang krijgen](#req-containers-anon)</li><li>[Beperkte toegang verlenen tot objecten in Azure storage met behulp van SAS- of SAP](#limited-access-sas)</li></ul> |

## <a id="standard-authn-web-app"></a>Overweeg het gebruik van een standaard-verificatiemechanisme voor verificatie bij de Web-App

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| Details | <p>Verificatie is het proces waarbij een entiteit aantoont de identiteit, doorgaans via referenties, zoals een gebruikersnaam en wachtwoord dat. Er zijn meerdere protocollen voor verificatie beschikbaar die kunnen worden beschouwd. Sommige hiervan worden hieronder vermeld:</p><ul><li>Clientcertificaten</li><li>Op Windows gebaseerd</li><li>Op basis van formulieren</li><li>Federation - AD FS</li><li>Federation - Azure AD</li><li>Federation - Identiteitsserver</li></ul><p>Overweeg het gebruik van een standaard-verificatiemechanisme voor het identificeren van de bronproces</p>|

## <a id="handle-failed-authn"></a>Toepassingen moeten omgaan met scenario's voor mislukte verificatie veilig

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| Details | <p>Mislukte verificatiescenario's moeten veilig omgaan met toepassingen die gebruikers expliciet worden geverifieerd. Het verificatiemechanisme moet:</p><ul><li>Toegang tot beschermde bronnen weigeren als verificatie is mislukt</li><li>Een algemene foutmelding worden weergegeven na een mislukte authenticatie en geweigerde toegang vindt plaats</li></ul><p>Test voor:</p><ul><li>Beveiliging van bevoegde resources na mislukte aanmeldingen</li><li>Een algemene foutmelding wordt weergegeven op mislukte verificatie en toegang geweigerd gebeurtenis(sen)</li><li>De accounts zijn uitgeschakeld na een uitzonderlijk groot aantal mislukte pogingen</li><ul>|

## <a id="step-up-adaptive-authn"></a>Stap van inschakelen of adaptieve verificatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| Details | <p>Controleer of de toepassing heeft extra autorisatie (zoals stap omhoog of adaptieve verificatie via MFA zoals het verzenden van OTP in SMS, e-mail enzovoort of vragen om nieuwe verificatie), zodat de gebruiker wordt gevraagd voordat toegang tot wordt verleend gevoelige gegevens. Deze regel geldt ook voor belangrijke wijzigingen in een account of een actie</p><p>Dit betekent ook dat de aanpassing van de verificatie worden geïmplementeerd in zodanig moet dat de toepassing correct wordt afgedwongen contextgevoelige autorisatie zodat deze niet-geautoriseerde manipulatie door middel van toestaan in een voorbeeld parameter knoeien</p>|

## <a id="admin-interface-lockdown"></a>Zorg ervoor dat administratieve interfaces op de juiste wijze zijn vergrendeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| Details | De eerste oplossing is voor het verlenen van toegang alleen vanaf een bepaalde bron IP-adresbereik aan de beheerinterface. Als u die oplossing niet mogelijk dan is het altijd aanbevolen voor het afdwingen van een meer bevoegdheden of adaptieve-verificatie voor logboekregistratie in de beheerinterface |

## <a id="forgot-pword-fxn"></a>Implementeer vergeten wachtwoord functionaliteiten veilig

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| Details | <p>Het eerste wat is om te controleren of wachtwoord bent vergeten en andere paden recovery een koppeling met inbegrip van een token gedurende beperkte tijd activering in plaats van het wachtwoord zelf verzenden. Aanvullende verificatie op basis van voorlopig-tokens (bijvoorbeeld SMS token, systeemeigen mobiele toepassingen, enzovoort) kan worden vereist ook voordat de koppeling wordt verzonden via. Ten tweede moet u niet vergrendelen van het gebruikersaccount tijdens het proces van het ophalen van een nieuw wachtwoord uitgevoerd wordt.</p><p>Dit kan leiden tot een Denial of service-aanval wanneer een aanvaller wil de gebruikers met een geautomatiseerde aanval opzettelijk worden vergrendeld. Derde, wanneer de aanvraag voor het nieuwe wachtwoord in voortgang is ingesteld, kan het bericht dat u moet worden gegeneraliseerd om te voorkomen dat de gebruikersnaam-opsomming. Vierde altijd het gebruik van de oude wachtwoorden weigeren en een sterk wachtwoordbeleid implementeren.</p> |

## <a id="pword-account-policy"></a>Zorg ervoor dat de wachtwoord- en -beleid worden toegepast

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| Details | <p>Wachtwoord- en beleid voldoen aan het organisatiebeleid en aanbevolen procedures moet worden geïmplementeerd.</p><p>Bescherming tegen brute-force en woordenlijst op basis van raden: beleid voor sterke wachtwoorden moet worden geïmplementeerd om ervoor te zorgen dat gebruikers een complex wachtwoord (bijvoorbeeld de minimale lengte 12 tekens, alfanumerieke tekens en speciale tekens) maken.</p><p>Het beleid voor accountvergrendeling mogen worden uitgevoerd in de volgende manier:</p><ul><li>**Voorlopig lock-out:** dit is een goede optie voor het beveiligen van uw gebruikers tegen beveiligingsaanvallen. Bijvoorbeeld, wanneer de gebruiker een onjuist wachtwoord invoert drie keer de toepassing kan worden vergrendeld in het account voor een minuut om het proces van het forceren van zijn/haar wachtwoord minder winstgevende voor de aanvaller om door te gaan waardoor brute vertragen. Als u tegenmaatregelen harde lock-out voor dit voorbeeld dat u een 'Dos' door accounts definitief worden vergrendeld. U kunt ook toepassing mag een OTP (één keer wachtwoord) genereren en te verzenden out-of-band (via e-mail, sms, enz.) aan de gebruiker. Een andere benadering mogelijk voor het implementeren van CAPTCHA nadat een drempelwaarde voor aantal mislukte pogingen is bereikt.</li><li>**Vaste lock-out:** dit type vergrendeling moet worden toegepast wanneer u een gebruiker is een aanval op uw toepassing te detecteren en hem teller met behulp van permanent zijn account vergrendelen totdat een antwoord-team heeft in hun forensische doen. Na dit proces die u bepalen kunt dat de gebruiker back-ups maken zijn account of verder juridische acties tegen hem uitvoeren. Dit soort aanpak voorkomt dat de aanvaller verder die uw toepassing en de infrastructuur.</li></ul><p>Om u te beschermen tegen aanvallen op de standaard- en voorspelbare accounts, controleert u of alle sleutels en wachtwoorden vervangen worden, en die zijn gegenereerd of vervangen na de installatie.</p><p>Als de toepassing heeft voor het automatisch genereren van wachtwoorden, ervoor zorgen dat de gegenereerde wachtwoorden willekeurige en hoge entropie hebben.</p>|

## <a id="controls-username-enum"></a>Besturingselementen om te voorkomen dat de gebruikersnaam (opsomming) worden geïmplementeerd.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Alle foutberichten moeten worden gegeneraliseerd om te voorkomen dat de gebruikersnaam-opsomming. Ook soms kan niet worden vermeden gegevens lekken in functies zoals een pagina voor de registratie. Hier moet u frequentiebeperkende methoden, zoals CAPTCHA gebruiken om te voorkomen dat een geautomatiseerde aanval door een aanvaller. |

## <a id="win-authn-sql"></a>Indien mogelijk, Windows-verificatie gebruiken om verbinding te maken met SQL Server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | OnPrem |
| **Kenmerken**              | Versie van SQL - alle |
| **Verwijzingen**              | [SQL Server - een verificatiemodus kiezen](https://msdn.microsoft.com/library/ms144284.aspx) |
| **Stappen** | Windows-verificatie gebruikt Kerberos veiligheidsprotocol, voorziet wachtwoord-beleidsafdwinging voor validatie op complexiteit voor sterke wachtwoorden, biedt ondersteuning voor accountvergrendeling en ondersteunt het verlopen van.|

## <a id="aad-authn-sql"></a>Indien mogelijk gebruik van Azure Active Directory-verificatie om verbinding te maken met SQL-Database

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure |
| **Kenmerken**              | Versie van SQL - V12 |
| **Verwijzingen**              | [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/) |
| **Stappen** | **Minimale versie:** Azure SQL Database V12 vereist voor de Azure SQL Database om het gebruik van AAD-verificatie op basis van de Microsoft-Directory |

## <a id="authn-account-pword"></a>Wanneer de modus SQL-authenticatie wordt gebruikt, zorgt u ervoor dat account en wachtwoord beleid worden afgedwongen op de SQL server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [SQL Server-wachtwoordbeleid](https://technet.microsoft.com/library/ms161959(v=sql.110).aspx) |
| **Stappen** | Wanneer u SQL Server-verificatie gebruikt, worden aanmeldingen worden gemaakt in SQL Server die niet zijn gebaseerd op Windows-gebruikersaccounts. Zowel de gebruikersnaam en het wachtwoord worden gemaakt met behulp van SQL Server en opgeslagen in SQL Server. SQL Server kunt mechanismen voor beleid voor Windows-wachtwoord gebruiken. De dezelfde complexiteit en de verloopbeleid voor wachtwoorden die worden gebruikt in SQL Server in Windows waarmee kan worden toegepast. |

## <a id="autn-contained-db"></a>Gebruik geen SQL-verificatie in ingesloten databases

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | OnPrem, SQL Azure |
| **Kenmerken**              | SQL-versie - MSSQL2012, versie van SQL - V12 |
| **Verwijzingen**              | [Aanbevolen procedures voor beveiliging met ingesloten Databases](http://msdn.microsoft.com/library/ff929055.aspx) |
| **Stappen** | Het ontbreken van een wachtwoordbeleid afgedwongen verhogen de kans op een zwakke referentie in een ingesloten database tot stand wordt gebracht. Maak gebruik van Windows-verificatie. |

## <a id="authn-sas-tokens"></a>Per apparaatreferenties voor verificatie met behulp van SaS-tokens gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Event-Hubs verificatie- en beveiligingsmodellen](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | <p>Het beveiligingsmodel van Event Hubs is gebaseerd op een combinatie van Shared Access Signature (SAS)-tokens en gebeurtenisuitgevers. Naam van de uitgever vertegenwoordigt de apparaat-id die de token ontvangt. Zo zou u de tokens die zijn gegenereerd met de betreffende apparaten koppelen.</p><p>Alle berichten worden gemarkeerd met de oorspronkelijke aanvrager op servicezijde zodat de detectie van in de nettolading oorsprong spoofing aanvallen. Bij het verifiëren van apparaten, genereert een per apparaat SaS-token binnen het bereik van een unieke uitgever.</p>|

## <a id="multi-factor-azure-admin"></a>Azure multi-factor Authentication voor Azure-beheerders inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van Azure | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Wat is Azure Multi-Factor Authentication?](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) |
| **Stappen** | <p>Multi-factor authentication (MFA) is een verificatiemethode die meer dan één verificatiemethode is vereist en wordt een essentiële tweede beveiligingslaag toegevoegd aan gebruikersaanmeldingen en transacties. Dit gebeurt door de vereist twee of meer van de volgende verificatiemethoden:</p><ul><li>Iets dat u weet (doorgaans een wachtwoord)</li><li>Iets dat u hebt (een vertrouwd apparaat dat is niet eenvoudig worden gedupliceerd, zoals een telefoon)</li><li>Iets dat u bent (biometrie)</li><ul>|

## <a id="anon-access-cluster"></a>Anonieme toegang beperken tot Service Fabric-Cluster

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van service Fabric | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure  |
| **Verwijzingen**              | [Service Fabric-clusterbeveiligingsscenario 's](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security) |
| **Stappen** | <p>Clusters moeten altijd zijn beveiligd om te voorkomen dat onbevoegde gebruikers verbinding maken met uw cluster, met name wanneer er productieworkloads die erop worden uitgevoerd.</p><p>Tijdens het maken van een service fabric-cluster, zorg ervoor dat de beveiligingsmodus is ingesteld op "veilig" en het vereiste certificaat van de X.509-server configureren. Het maken van een 'onveilig' cluster kunnen een anonieme gebruiker verbinding maken met het als-beheereindpunten met het openbare Internet beschikbaar worden gemaakt.</p>|

## <a id="fabric-cn-nn"></a>Zorg ervoor dat certificaat voor Service Fabric-client-naar-knooppunt van het certificaat van knooppunt-naar-knooppunt verschilt

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van service Fabric | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Zelfstandige omgeving - Azure, omgeving: |
| **Verwijzingen**              | [Service Fabric-Client-naar-knooppunt Certificaatbeveiliging](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#_client-to-node-certificate-security), [verbinding maken met een beveiligd cluster met behulp van clientcertificaat](https://azure.microsoft.com/documentation/articles/service-fabric-connect-to-secure-cluster/) |
| **Stappen** | <p>Client-naar-knooppunt Certificaatbeveiliging is geconfigureerd tijdens het maken van het cluster via de Azure portal, Resource Manager-sjablonen of een zelfstandige JSON-sjabloon door op te geven van een certificaat voor de Beheerclient en/of een clientcertificaat voor gebruikers.</p><p>De client en de gebruiker-clientcertificaten van beheerder, u moet verschillen van de primaire en secundaire certificaten die u voor de beveiliging van knooppunt-naar-knooppunt opgeeft.</p>|

## <a id="aad-client-fabric"></a>Gebruik van AAD om clients naar service fabric-clusters te verifiëren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van service Fabric | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure |
| **Verwijzingen**              | [Clusterbeveiligingsscenario's - aanbevelingen voor beveiliging](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#security-recommendations) |
| **Stappen** | Clusters die worden uitgevoerd op Azure kunnen ook beveiligde toegang tot de eindpunten voor beheer met behulp van Azure Active Directory (AAD), naast de clientcertificaten. Voor clusters met Azure, is het raadzaam dat u AAD-beveiliging gebruikt voor verificatie van clients en certificaten voor de beveiliging van knooppunt-naar-knooppunt.|

## <a id="fabric-cert-ca"></a>Zorg ervoor dat de service fabric-certificaten zijn opgehaald uit een erkende certificeringsinstantie (CA)

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van service Fabric | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure |
| **Verwijzingen**              | [X.509-certificaten en Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#x509-certificates-and-service-fabric) |
| **Stappen** | <p>Service Fabric maakt gebruik van x.509-certificaten voor het verifiëren van knooppunten en -clients.</p><p>Enkele belangrijke aandachtspunten voor tijdens het gebruik van certificaten in de service fabrics:</p><ul><li>Certificaten die worden gebruikt in clusters met werkbelastingen voor productie moeten worden gemaakt met een correct geconfigureerde service voor Windows Server-certificaat of verkregen van een erkende certificeringsinstantie (CA). De CA mag bestaan uit een goedgekeurde externe CA of een goed beheerde interne Public Key Infrastructure (PKI)</li><li>Gebruik nooit een tijdelijke of certificaten testen in productie die zijn gemaakt met hulpprogramma's zoals MakeCert.exe</li><li>U kunt een zelfondertekend certificaat gebruiken, maar moet alleen doen voor testclusters en niet in productie</li></ul>|

## <a id="standard-authn-id"></a>Gebruik standaard verificatiescenario's die worden ondersteund door Identiteitsserver

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [IdentityServer3: het totaalbeeld](https://identityserver.github.io/Documentation/docsv2/overview/bigPicture.html) |
| **Stappen** | <p>Hieronder vindt u de typische interacties ondersteund door identiteits-Server:</p><ul><li>Browsers communiceren met de web-apps</li><li>Webtoepassingen die communiceren met de web-API's (soms op hun eigen, soms namens een gebruiker)</li><li>Browser gebaseerde toepassingen communiceren met de web-API 's</li><li>Systeemeigen toepassingen communiceren met de web-API 's</li><li>Server gebaseerde toepassingen communiceren met de web-API 's</li><li>Web-API's communiceren met de web-API's (soms op hun eigen, soms namens een gebruiker)</li></ul>|

## <a id="override-token"></a>Standaard Identity token cachegeheugen van de Server met een schaalbare alternatief overschrijven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Implementatie van de Server Identity - Caching](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Stappen** | <p>IdentityServer is een eenvoudige ingebouwde in-memory-cache. Dit is handig voor kleine schaal systeemeigen apps, schaalt het niet voor mid tier en back-end-toepassingen voor de volgende redenen:</p><ul><li>Deze toepassingen worden gebruikt door veel gebruikers tegelijk. Alle toegangstokens opslaan in het archief met dezelfde isolatie problemen maakt en uitdagingen geeft als het wordt uitgevoerd op schaal: veel gebruikers, elk met zo veel tokens als de resource die de app toegang heeft tot namens hen kunnen betekenen enorme aantallen en bijzonder veeleisende opzoekbewerkingen</li><li>Deze toepassingen worden doorgaans geïmplementeerd op gedistribueerde topologieën, waarbij meerdere knooppunten toegang tot de dezelfde cache hebben moeten</li><li>In de cache tokens moeten overleven proces wordt gerecycled en deactivations</li><li>Voor de bovenstaande redenen, tijdens het implementeren van web-apps, is het raadzaam om op te heffen tokencache van de standaard Identity-Server met een schaalbare alternatief zoals Azure Redis cache</li></ul>|

## <a id="binaries-signed"></a>Zorg ervoor dat de binaire waarden van geïmplementeerde toepassingen digitaal zijn ondertekend

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van computer | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat binaire waarden van geïmplementeerde toepassingen digitaal zijn ondertekend, zodat de integriteit van de binaire bestanden kan worden gecontroleerd|

## <a id="msmq-queues"></a>Verificatie inschakelen bij het verbinden met de MSMQ-wachtrijen in WCF

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx) |
| **Stappen** | Programma is mislukt voor het inschakelen van verificatie bij het verbinden met de MSMQ-wachtrijen, een aanvaller kunt anoniem verzenden van berichten naar de wachtrij voor verwerking. Als verificatie geen verbinding maken met een MSMQ-wachtrij gebruikt voor het leveren van een bericht naar een ander programma gebruikt is, kan een aanvaller een anonieme bericht dat schadelijk is verzonden.|

### <a name="example"></a>Voorbeeld
De `<netMsmqBinding/>` element van het configuratiebestand WCF geeft WCF verificatie uitschakelen bij het verbinden met een MSMQ-wachtrij voor de bezorging van berichten.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""None"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```
MSMQ om te allen tijde Windows-domein of het certificaat verificatie vereisen voor binnenkomende of uitgaande berichten configureren.

### <a name="example"></a>Voorbeeld
De `<netMsmqBinding/>` element van het configuratiebestand WCF Hiermee geeft u WCF-certificaatverificatie inschakelen bij het verbinden met een MSMQ-wachtrij. De client is geverifieerd met X.509-certificaten. Het certificaat moet aanwezig zijn in het certificaatarchief van de server.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""Certificate"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```

## <a id="message-none"></a>Bericht clientCredentialType WCF-Do niet ingesteld op none

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET framework 3 |
| **Kenmerken**              | Het referentietype client - None |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg](https://vulncat.fortify.com/en/detail?id=desc.semantic.dotnet.wcf_misconfiguration_anonymous_message_client) |
| **Stappen** | Het ontbreken van verificatie betekent dat iedereen toegang kunnen krijgen tot deze service is. Een service die de clients niet geverifieerd biedt toegang tot alle gebruikers. De toepassing voor verificatie op basis van clientreferenties configureren. Dit kan worden gedaan door de clientCredentialType bericht in te stellen op Windows of het certificaat. |

### <a name="example"></a>Voorbeeld
```
<message clientCredentialType=""Certificate""/>
```

## <a id="transport-none"></a>Transport clientCredentialType WCF-Do niet ingesteld op none

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, .NET Framework 3 |
| **Kenmerken**              | Het referentietype client - None |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg](https://vulncat.hpefod.com/en/detail?id=desc.semantic.dotnet.wcf_misconfiguration_anonymous_transport_client) |
| **Stappen** | Het ontbreken van verificatie betekent dat iedereen toegang kunnen krijgen tot deze service is. Een service die de clients niet geverifieerd kan alle gebruikers toegang tot de functionaliteit. De toepassing voor verificatie op basis van clientreferenties configureren. Dit kan worden gedaan door het transport clientCredentialType te stellen op Windows of het certificaat. |

### <a name="example"></a>Voorbeeld
```
<transport clientCredentialType=""Certificate""/>
```

## <a id="authn-secure-api"></a>Zorg ervoor dat de standaard verificatie technieken worden gebruikt voor het beveiligen van Web-API 's

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Verificatie en autorisatie in ASP.NET Web API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api), [Services van externe verificatie met ASP.NET Web-API (C#)](http://www.asp.net/web-api/overview/security/external-authentication-services) |
| **Stappen** | <p>Verificatie is het proces waarbij een entiteit aantoont de identiteit, doorgaans via referenties, zoals een gebruikersnaam en wachtwoord dat. Er zijn meerdere protocollen voor verificatie beschikbaar die kunnen worden beschouwd. Sommige hiervan worden hieronder vermeld:</p><ul><li>Clientcertificaten</li><li>Op Windows gebaseerd</li><li>Op basis van formulieren</li><li>Federation - AD FS</li><li>Federation - Azure AD</li><li>Federation - Identiteitsserver</li></ul><p>Koppelingen in de sectie Verwijzingen bieden details op laag niveau over hoe elk van de verificatiemethoden voor het beveiligen van een Web-API kan worden geïmplementeerd.</p>|

## <a id="authn-aad"></a>Standard verificatiescenario's die worden ondersteund door Azure Active Directory gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Verificatiescenario's voor Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/), [codevoorbeelden van Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-code-samples/), [ontwikkelaarsgids van Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-developers-guide/) |
| **Stappen** | <p>Azure Active Directory (Azure AD) vereenvoudigt verificatie voor ontwikkelaars door op te geven van de identiteit als een service, met ondersteuning voor standaardprotocollen zoals OAuth 2.0 en OpenID Connect. Hieronder worden de vijf primaire toepassing-scenario's ondersteund door Azure AD:</p><ul><li>Webbrowser voor Web-App: een gebruiker moet zich aanmelden bij een webtoepassing die wordt beveiligd door Azure AD</li><li>Toepassing met één pagina (SPA): Een gebruiker moet zich aanmelden bij een toepassing met één pagina die wordt beveiligd door Azure AD</li><li>Systeemeigen toepassing voor de Web-API: een systeemeigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC moet een gebruiker voor resources van een web-API die wordt beveiligd door Azure AD verifiëren</li><li>Webtoepassing tot Web-API: een web-App nodig heeft om op te halen van resources van een web-API is beveiligd door Azure AD</li><li>Daemon of servertoepassing tot Web-API: een daemon-toepassing of een servertoepassing zonder gebruikersinterface web moet de resources ophalen uit een web-API is beveiligd door Azure AD</li></ul><p>Raadpleeg de koppelingen in de sectie Verwijzingen van laag niveau implementatiegegevens</p>|

## <a id="adal-scalable"></a>De standaard ADAL-token-cache met een schaalbare alternatief overschrijven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Moderne verificatie met Azure Active Directory voor webtoepassingen](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/), [als ADAL-tokencache met behulp van Redis](https://blogs.msdn.microsoft.com/mrochon/2016/09/19/using-redis-as-adal-token-cache/)  |
| **Stappen** | <p>De standaardcache die ADAL (Active Directory Authentication Library) gebruikt een in-memory-cache die is gebaseerd op een statische opslag beschikbaar hele van proces is. Terwijl dit voor systeemeigen toepassingen werkt, wordt deze niet voor mid tier en back-end-toepassingen schalen om de volgende redenen:</p><ul><li>Deze toepassingen worden gebruikt door veel gebruikers tegelijk. Alle toegangstokens opslaan in het archief met dezelfde isolatie problemen maakt en uitdagingen geeft als het wordt uitgevoerd op schaal: veel gebruikers, elk met zo veel tokens als de resource die de app toegang heeft tot namens hen kunnen betekenen enorme aantallen en bijzonder veeleisende opzoekbewerkingen</li><li>Deze toepassingen worden doorgaans geïmplementeerd op gedistribueerde topologieën, waarbij meerdere knooppunten toegang tot de dezelfde cache hebben moeten</li><li>In de cache tokens moeten overleven proces wordt gerecycled en deactivations</li></ul><p>Voor de bovenstaande redenen, tijdens het implementeren van web-apps, het verdient aanbeveling voor de onderdrukking van de standaard ADAL-token-cache met een schaalbare alternatief zoals Azure Redis-cache.</p>|

## <a id="tokenreplaycache-adal"></a>Zorg ervoor dat TokenReplayCache wordt gebruikt om te voorkomen dat de herhaling van tokens voor ADAL-verificatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Moderne verificatie met Azure Active Directory voor webtoepassingen](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/) |
| **Stappen** | <p>De eigenschap TokenReplayCache kan ontwikkelaars voor het definiëren van een token opnieuw afspelen-cache, een store, die kan worden gebruikt voor het opslaan van tokens om na te gaan dat er geen token kan meer dan eenmaal worden gebruikt.</p><p>Dit is een meting op basis van een veelvoorkomende aanval, de toepasselijke aangeroepen token opnieuw afspelen aanvallen: een aanvaller die het token verzonden bij het aanmelden onderscheppen deze opnieuw verzenden naar de app proberen ("opnieuw afspelen") voor het opzetten van een nieuwe sessie. Bijvoorbeeld In OIDC-verlenen stroom, na een geslaagde verificatie, een aanvraag voor ' / aanmelding oidc '-eindpunt van de relying party is gemaakt met 'id_token', 'code' en 'staat'-parameters.</p><p>De relying party wordt deze aanvraag wordt gevalideerd en maakt een nieuwe sessie. Als een kwaadwillende persoon deze aanvraag wordt vastgelegd en opnieuw wordt weergegeven, kan hij een geslaagde sessie tot stand brengen en vervalsen van de gebruiker. De aanwezigheid van de nonce in OpenID Connect kunt beperken, maar niet volledig Elimineer de omstandigheden waarin de aanval is kan worden vastgesteld. Ter bescherming van hun toepassingen, ontwikkelaars biedt een implementatie van ITokenReplayCache en een exemplaar van aan TokenReplayCache toewijzen.</p>|

### <a name="example"></a>Voorbeeld
```csharp
// ITokenReplayCache defined in ADAL
public interface ITokenReplayCache
{
bool TryAdd(string securityToken, DateTime expiresOn);
bool TryFind(string securityToken);
}
```

### <a name="example"></a>Voorbeeld
Hier volgt een Voorbeeldimplementatie van de ITokenReplayCache-interface. (Geef aanpassen en implementeren van uw project-specifieke opslaan in cache framework)
```csharp
public class TokenReplayCache : ITokenReplayCache
{
    private readonly ICacheProvider cache; // Your project-specific cache provider
    public TokenReplayCache(ICacheProvider cache)
    {
        this.cache = cache;
    }
    public bool TryAdd(string securityToken, DateTime expiresOn)
    {
        if (this.cache.Get<string>(securityToken) == null)
        {
            this.cache.Set(securityToken, securityToken);
            return true;
        }
        return false;
    }
    public bool TryFind(string securityToken)
    {
        return this.cache.Get<string>(securityToken) != null;
    }
}
```
De geïmplementeerde cache is als volgt in OIDC-opties via de eigenschap 'TokenValidationParameters' worden verwezen.
```csharp
OpenIdConnectOptions openIdConnectOptions = new OpenIdConnectOptions
{
    AutomaticAuthenticate = true,
    ... // other configuration properties follow..
    TokenValidationParameters = new TokenValidationParameters
    {
        TokenReplayCache = new TokenReplayCache(/*Inject your cache provider*/);
    }
}
```

Houd er rekening mee dat als u wilt de effectiviteit van deze configuratie, meld u aan bij uw lokale OIDC-beveiligde toepassing testen en de aanvraag voor het vastleggen `"/signin-oidc"` eindpunt in fiddler. Deze aanvraag in fiddler spelen wordt bij de beveiliging niet ingesteld is, een nieuwe sessiecookie ingesteld. Wanneer de aanvraag is opnieuw afgespeeld nadat de beveiliging TokenReplayCache is toegevoegd, wordt de toepassing als volgt een uitzondering genereert: `SecurityTokenReplayDetectedException: IDX10228: The securityToken has previously been validated, securityToken: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ1......`

## <a id="adal-oauth2"></a>ADAL-bibliotheken voor het beheren van token aanvragen van OAuth2-clients naar AAD gebruiken (of on-premises AD)

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [ADAL](https://azure.microsoft.com/documentation/articles/active-directory-authentication-libraries/) |
| **Stappen** | <p>De Azure AD authentication Library (ADAL) kan ontwikkelaars van clienttoepassingen gemakkelijk om gebruikers te verifiëren naar de cloud of on-premises Active Directory (AD), en vervolgens toegangstokens voor het beveiligen van API-aanroepen te verkrijgen.</p><p>ADAL bevat veel functies die verificatie maken beter voor ontwikkelaars, zoals asynchrone ondersteuning, een configureerbare tokencache waarmee toegangstokens en vernieuwen van tokens, automatisch token vernieuwen wanneer een toegangstoken is verlopen en een vernieuwingstoken beschikbaar is, worden opgeslagen en meer.</p><p>Doordat de meeste van de complexiteit, ADAL een ontwikkelaar gefocust op bedrijfslogica in de toepassing in en gemakkelijk resources beveiligen zonder een expert over de beveiliging. Afzonderlijke bibliotheken zijn beschikbaar voor .NET, JavaScript (client en Node.js), iOS, Android- en Java.</p>|

## <a id="authn-devices-field"></a>Verificatie van apparaten die verbinding maken met de Gateway van het veld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld voor IoT-Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat elk apparaat wordt geverifieerd door de Veldgateway voordat u gegevens uit deze accepteert en vóór upstream communicatie met de Cloudgateway. Zorg er ook voor dat apparaten verbinding met maken een per apparaat zodat de afzonderlijke apparaten kunnen worden onderscheiden van referenties.|

## <a id="authn-devices-cloud"></a>Zorg ervoor dat apparaten verbinding maken met de cloudgateway worden geverifieerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-Cloud-Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, C#, Node.JS,  |
| **Kenmerken**              | Niet van toepassing want keuze van de Gateway - Azure IoT Hub |
| **Verwijzingen**              | Niet van toepassing want [Azure IoT hub met .NET](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/), [wih IoT-hub aan de slag en Node JS](https://azure.microsoft.com/documentation/articles/iot-hub-node-node-getstarted), [IoT met SAS en certificaten beveiligen](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/), [Git-opslagplaats](https://github.com/Azure/azure-iot-sdks/tree/master/node) |
| **Stappen** | <ul><li>**Algemene:** het apparaat met behulp van Transport Layer Security (TLS) of IPSec-verificatie. Infrastructuur moet ondersteunen met behulp van vooraf gedeelde sleutel (PSK) op apparaten die volledige asymmetrische cryptografische kunnen niet worden verwerkt. Gebruik Azure AD Oauth.</li><li>**C#:** bij het maken van een exemplaar DeviceClient standaard, de methode maken maakt een DeviceClient-exemplaar dat het AMQP-protocol gebruikt om te communiceren met IoT Hub. Als u het HTTPS-protocol, gebruikt u de onderdrukking van de methode voor maken waarmee u zelf het protocol bepalen. Als u het HTTPS-protocol gebruikt, moet u ook toevoegen de `Microsoft.AspNet.WebApi.Client` NuGet-pakket aan uw project op te nemen de `System.Net.Http.Formatting` naamruimte.</li></ul>|

### <a name="example"></a>Voorbeeld
```csharp
static DeviceClient deviceClient;

static string deviceKey = "{device key}";
static string iotHubUri = "{iot hub hostname}";

var messageString = "{message in string format}";
var message = new Message(Encoding.ASCII.GetBytes(messageString));

deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

await deviceClient.SendEventAsync(message);
```

### <a name="example"></a>Voorbeeld
**Node.JS: verificatie**
#### <a name="symmetric-key"></a>Symmetrische sleutel
* Een IoT-hub in azure maken
* Een vermelding maken in het register voor apparaat-id 's
    ```javascript
    var device = new iothub.Device(null);
    device.deviceId = <DeviceId >
    registry.create(device, function(err, deviceInfo, res) {})
    ```
* Een gesimuleerd apparaat maken
    ```javascript
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>SharedAccessKey=<SharedAccessKey>';
    var client = clientFromConnectionString(connectionString);
    ```
#### <a name="sas-token"></a>SAS-token
* Met deze eigenschap wordt intern gegenereerd bij het gebruik van de symmetrische sleutel, maar we kunnen genereren en deze ook expliciet gebruiken
* Definieer een protocol: `var Http = require('azure-iot-device-http').Http;`
* Maak een sas-token:
    ```javascript
    resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();
    var deviceName = "<deviceName >";
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    var toSign = resourceUri + '\n' + expires;
    // using crypto
    var decodedPassword = new Buffer(signingKey, 'base64').toString('binary');
    const hmac = crypto.createHmac('sha256', decodedPassword);
    hmac.update(toSign);
    var base64signature = hmac.digest('base64');
    var base64UriEncoded = encodeURIComponent(base64signature);
    // construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "%2fdevices%2f"+deviceName+"&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
    ```
* Verbinding maken met behulp van sas-token: 
    ```javascript
    Client.fromSharedAccessSignature(sas, Http); 
    ```
#### <a name="certificates"></a>Certificaten
* Een zelf-ondertekend X509 genereren van het certificaat met behulp van een hulpprogramma zoals OpenSSL voor het genereren van een .cert- en .key-bestanden voor het opslaan van het certificaat en de sleutel respectievelijk
* Inrichten van een apparaat dat beveiligde verbinding met behulp van certificaten accepteert.
    ```javascript
    var connectionString = '<connectionString>';
    var registry = iothub.Registry.fromConnectionString(connectionString);
    var deviceJSON = {deviceId:"<deviceId>",
    authentication: {
        x509Thumbprint: {
        primaryThumbprint: "<PrimaryThumbprint>",
        secondaryThumbprint: "<SecondaryThumbprint>"
        }
    }}
    var device = deviceJSON;
    registry.create(device, function (err) {});
    ```
* Verbinding maken met een apparaat met een certificaat
    ```javascript
    var Protocol = require('azure-iot-device-http').Http;
    var Client = require('azure-iot-device').Client;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>x509=true';
    var client = Client.fromConnectionString(connectionString, Protocol);
    var options = {
        key: fs.readFileSync('./key.pem', 'utf8'),
        cert: fs.readFileSync('./server.crt', 'utf8')
    }; 
    // Calling setOptions with the x509 certificate and key (and optionally, passphrase) will configure the client //transport to use x509 when connecting to IoT Hub
    client.setOptions(options);
    //call fn to execute after the connection is set up
    client.open(fn);
    ```

## <a id="authn-cred"></a>Referenties voor verificatie per apparaat gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-Cloud-Gateway  | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Keuze van de gateway - Azure IoT Hub |
| **Verwijzingen**              | [Beveiligingstokens van Azure IoT Hub](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/) |
| **Stappen** | Gebruik per apparaatreferenties voor verificatie met behulp van SaS-tokens op basis van de sleutel voor apparaat of een clientcertificaat, in plaats van IoT Hub-niveau gedeelde toegangsbeleid. Hiermee voorkomt u dat de verificatietokens van een apparaat of het veld gateway niet opnieuw gebruiken door een andere |

## <a id="req-containers-anon"></a>Zorg ervoor dat alleen de benodigde containers en blobs anonieme leestoegang krijgen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - Blob |
| **Verwijzingen**              | [Anonieme leestoegang tot containers en blobs beheren](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/), [handtekeningen voor gedeelde toegang, deel 1: inzicht in het SAS-model](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) |
| **Stappen** | <p>Standaard kunnen een container en alle bestaande blobs erin alleen worden geopend door de eigenaar van het storage-account. Een kan de containermachtigingen voor openbare toegang tot ingesteld zodat anonieme gebruikers leesmachtigingen heeft voor een container en de blobs. Anonieme gebruikers kunnen blobs in een openbaar toegankelijke container lezen zonder verificatie van de aanvraag.</p><p>Containers bieden de volgende opties voor het beheren van toegang tot de container:</p><ul><li>Openbare leestoegang volledige: Container en blob-gegevens kunnen worden gelezen via anonieme aanvragen. Clients kunnen opsommen en blobs in de container via anonieme aanvragen, maar kunnen de containers in het opslagaccount niet opsommen.</li><li>Openbare leestoegang alleen voor blobs: Blob-gegevens in deze container via anonieme aanvragen kunnen worden gelezen, maar de containergegevens is niet beschikbaar. Clients kunnen blobs in de container via anonieme aanvragen niet opsommen</li><li>Er zijn geen openbare leestoegang: Container en blob-gegevens kunnen worden gelezen door alleen de eigenaar van het account</li></ul><p>Anonieme toegang wordt aanbevolen voor scenario's waarin bepaalde blobs altijd beschikbaar voor anonieme toegang voor lezen zijn moeten. Voor nauwkeurigere besturing mogelijk, kan een shared access signature, waardoor beperkte Gemachtigdentoegang met behulp van verschillende machtigingen en gedurende een opgegeven tijdsinterval worden gemaakt. Zorg ervoor dat containers en blobs die mogelijk gevoelige gegevens bevatten, worden geen anonieme toegang per ongeluk</p>|

## <a id="limited-access-sas"></a>Beperkte toegang verlenen tot objecten in Azure storage met behulp van SAS- of SAP

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A |
| **Verwijzingen**              | [Handtekeningen voor gedeelde toegang, deel 1: Inzicht in het SAS-model](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/), [handtekeningen voor gedeelde toegang, deel 2: maken en een SAS gebruiken met Blob-opslag](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/), [hoe u toegang tot objecten in uw account met behulp van gedeelde delegeren Handtekeningen voor toegang en opgeslagen toegangsbeleid](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies) |
| **Stappen** | <p>Met behulp van een shared access signature is (SAS) een krachtige manier om beperkte toegang tot objecten in een opslagaccount verlenen aan andere clients zonder toegangssleutel voor het beschikbaar stellen. De SAS is een URI die in de queryparameters omvat alle informatie die nodig zijn voor geverifieerde toegang tot een opslagresource. Voor toegang tot de storage-resources met de SAS moet moet de client alleen worden doorgegeven in de SAS tot de juiste constructor of methode.</p><p>U kunt een SAS gebruiken als u wilt toegang bieden tot bronnen in uw opslagaccount naar een client die niet kan vertrouwd met de accountsleutel worden. De opslagaccountsleutels zijn zowel een primaire en secundaire sleutel, die beide beheerderstoegang tot uw account en alle resources in het verlenen. Uw account met de mogelijkheid van kwaadwillende of uit nalatigheid wordt geopenbaard gebruik blootstellen van een van de sleutels van uw account worden geopend. Handtekeningen voor gedeelde toegang bieden een veilige alternatief waarmee andere clients om te lezen, schrijven en verwijderen van gegevens in uw storage-account volgens de machtigingen die u hebt opgegeven, en zonder dat nodig is voor de accountsleutel.</p><p>Als u een logische set parameters die vergelijkbaar telkens zijn hebt, is met behulp van een opgeslagen toegang beleid (SAP) een beter idee. Omdat met behulp van een SAS die is afgeleid van een opgeslagen toegangsbeleid u de mogelijkheid om in te trekken die SAS onmiddellijk biedt, is de aanbevolen toegangsbeleid opgeslagen indien mogelijk altijd wilt gebruiken.</p>|