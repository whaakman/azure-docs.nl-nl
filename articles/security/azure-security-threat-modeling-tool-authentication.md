---
title: Verificatie - hulpmiddel voor het modelleren van Microsoft Threat - Azure | Microsoft Docs
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
ms.openlocfilehash: e547469dc61eddd1d772571ab0919532ac91f128
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-authentication--mitigations"></a>Beveiliging Frame: Verificatie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Webtoepassing**    | <ul><li>[Overweeg het gebruik van een standaard verificatiemechanisme voor de verificatie-webtoepassing](#standard-authn-web-app)</li><li>[Toepassingen moeten veilig mislukte verificatiepogingen scenario's verwerken](#handle-failed-authn)</li><li>[Stap up inschakelen of adaptieve verificatie](#step-up-adaptive-authn)</li><li>[Zorg ervoor dat administratieve interfaces op de juiste wijze zijn vergrendeld](#admin-interface-lockdown)</li><li>[Implementeer vergeten wachtwoord functionaliteiten veilig](#forgot-pword-fxn)</li><li>[Zorg ervoor dat de wachtwoord- en -beleid worden toegepast](#pword-account-policy)</li><li>[Besturingselementen om te voorkomen dat de gebruikersnaam opsomming implementeren](#controls-username-enum)</li></ul> |
| **Database** | <ul><li>[Gebruik zo mogelijk Windows-verificatie voor de verbinding met SQL Server](#win-authn-sql)</li><li>[Gebruik zo mogelijk Azure Active Directory-verificatie om verbinding te maken met SQL-Database](#aad-authn-sql)</li><li>[SQL-verificatiemodus wordt gebruikt, zorg er bij dat account en wachtwoord beleid worden afgedwongen op SQL server](#authn-account-pword)</li><li>[Gebruik geen SQL-verificatie in ingesloten databases](#autn-contained-db)</li></ul> |
| **Azure Event Hub** | <ul><li>[Per apparaat referenties voor verificatie met behulp van SaS-tokens worden gebruikt](#authn-sas-tokens)</li></ul> |
| **Vertrouwensgrenzen van Azure** | <ul><li>[Azure multi-factor Authentication voor beheerders voor Azure inschakelen](#multi-factor-azure-admin)</li></ul> |
| **Service Fabric-Vertrouwensgrenzen** | <ul><li>[Anonieme toegang beperken tot Service Fabric-Cluster](#anon-access-cluster)</li><li>[Zorg ervoor dat Service Fabric-clientknooppunt certificaat verschilt van het knooppunt naar certificaat](#fabric-cn-nn)</li><li>[AAD voor verificatie van clients naar service fabric-clusters gebruiken.](#aad-client-fabric)</li><li>[Zorg ervoor dat service fabric-certificaten van een erkende certificeringsinstantie (CA) worden verkregen](#fabric-cert-ca)</li></ul> |
| **Identiteitsserver** | <ul><li>[Gebruik standaard verificatie scenario's ondersteund door Identiteitsserver](#standard-authn-id)</li><li>[Standaard identiteit token cachegeheugen van de Server met een schaalbare alternatief overschrijven](#override-token)</li></ul> |
| **Een grens machine vertrouwensrelatie** | <ul><li>[Zorg ervoor dat de binaire bestanden van geïmplementeerde toepassingen digitaal zijn ondertekend](#binaries-signed)</li></ul> |
| **WCF** | <ul><li>[-Verificatie inschakelen bij het verbinden met de MSMQ-wachtrijen in WCF](#msmq-queues)</li><li>[WCF-Do bericht clientCredentialType niet ingesteld op none](#message-none)</li><li>[WCF-Do Transport clientCredentialType niet ingesteld op none](#transport-none)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat de standaard verificatie technieken worden gebruikt voor het beveiligen van Web-API 's](#authn-secure-api)</li></ul> |
| **Azure AD** | <ul><li>[Gebruik standaard verificatie scenario's ondersteund door Azure Active Directory](#authn-aad)</li><li>[De standaard ADAL tokencache met een schaalbare alternatief overschrijven](#adal-scalable)</li><li>[Zorg dat TokenReplayCache wordt gebruikt om te voorkomen dat de replay van tokens voor ADAL-verificatie](#tokenreplaycache-adal)</li><li>[ADAL-bibliotheken gebruiken voor het beheren van aanvragen voor beveiligingstokens van OAuth2-clients voor AAD (of on-premises AD)](#adal-oauth2)</li></ul> |
| **Veld IoT Gateway** | <ul><li>[Verificatie van apparaten die verbinding maken met de Gateway veld](#authn-devices-field)</li></ul> |
| **IoT-Cloudgateway** | <ul><li>[Zorg ervoor dat de apparaten die verbinding maken met Cloud-gateway worden geverifieerd](#authn-devices-cloud)</li><li>[Referenties voor verificatie per apparaat gebruiken](#authn-cred)</li></ul> |
| **Azure Storage** | <ul><li>[Zorg ervoor dat alleen de vereiste containers en blobs anonieme leestoegang krijgen](#req-containers-anon)</li><li>[Beperkte toegang verlenen tot objecten in Azure-opslag met SAS of SAP](#limited-access-sas)</li></ul> |

## <a id="standard-authn-web-app"></a>Overweeg het gebruik van een standaard verificatiemechanisme voor de verificatie-webtoepassing

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| Details | <p>Verificatie is het proces waarbij een entiteit aantoont de identiteit, doorgaans via referenties, zoals een gebruikersnaam en wachtwoord dat. Er zijn verschillende verificatieprotocollen beschikbaar die kunnen worden beschouwd. Enkele ervan worden hieronder weergegeven:</p><ul><li>Clientcertificaten</li><li>Op basis van Windows</li><li>Op basis van formulieren</li><li>Federation - ADFS</li><li>Federation - Azure AD</li><li>Federation - Identiteitsserver</li></ul><p>Overweeg het gebruik van een standaard verificatiemechanisme voor het identificeren van de bronproces</p>|

## <a id="handle-failed-authn"></a>Toepassingen moeten veilig mislukte verificatiepogingen scenario's verwerken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| Details | <p>Toepassingen die expliciet verificatie van gebruikers moeten veilig mislukte verificatiepogingen scenario's verwerken. Het verificatiemechanisme moet:</p><ul><li>Toegang tot beschermde bronnen weigeren als verificatie mislukt</li><li>Een algemeen foutbericht weergegeven na een mislukte verificatie en toegang is geweigerd vindt plaats</li></ul><p>Test voor:</p><ul><li>Beveiliging van bevoegde resources na mislukte aanmeldingen</li><li>Een algemene foutmelding wordt weergegeven op mislukte verificatie en toegang geweigerd gebeurtenis(sen)</li><li>Accounts zijn uitgeschakeld na een uitzonderlijk groot aantal mislukte pogingen</li><ul>|

## <a id="step-up-adaptive-authn"></a>Stap up inschakelen of adaptieve verificatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| Details | <p>Controleer of de toepassing heeft extra autorisatie (zoals stap omhoog of adaptieve verificatie via meervoudige verificatie, zoals het zenden van OTP in SMS, e-mail, enz., of vragen om nieuwe verificatie) zodat de gebruiker wordt gevraagd voordat het wordt toegang verleend tot gevoelige informatie. Deze regel geldt ook voor essentiële wijzigingen aanbrengt in een account of een actie</p><p>Dit betekent ook dat de aanpassing van de verificatie moet worden geïmplementeerd zodanig dat de toepassing correct contextgevoelige autorisatie afdwingt zodat geen onbevoegde manipulatie door middel van in voorbeeld heeft de parameter knoeien</p>|

## <a id="admin-interface-lockdown"></a>Zorg ervoor dat administratieve interfaces op de juiste wijze zijn vergrendeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| Details | De eerste oplossing is om toegang te verlenen alleen via een bepaalde bron-IP-adresbereik voor de beheerinterface. Als deze oplossing is niet mogelijk dan altijd aanbevolen voor het afdwingen van een geslaagde of adaptieve verificatie voor logboekregistratie in in de beheerinterface |

## <a id="forgot-pword-fxn"></a>Implementeer vergeten wachtwoord functionaliteiten veilig

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| Details | <p>Het eerste wat is om te controleren of wachtwoord vergeten en andere herstel naar een koppeling met inbegrip van een token tijdelijke activering in plaats van het wachtwoord zelf verzenden. Aanvullende verificatie op basis van de soft-tokens (bijvoorbeeld SMS token mobiele toepassingen, etc.) kan worden vereist ook voordat de koppeling wordt verzonden via. Ten tweede moet niet vergrendelt u het account van de gebruikers terwijl het proces van het ophalen van een nieuw wachtwoord uitgevoerd wordt.</p><p>Dit kan leiden tot een Denial of service-aanval telkens wanneer een aanvaller wil de gebruikers met een geautomatiseerde aanval opzettelijk worden vergrendeld. Derde, wanneer het nieuwe wachtwoord aanvraag bezig was ingesteld, kan het bericht dat u weer te geven moet worden gegeneraliseerd om te voorkomen dat de opsomming van de gebruikersnaam. Vierde altijd niet toestaan van het gebruik van de oude wachtwoorden en een sterk wachtwoordbeleid implementeert.</p> |

## <a id="pword-account-policy"></a>Zorg ervoor dat de wachtwoord- en -beleid worden toegepast

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| Details | <p>Beleid voor wachtwoorden en voldoen aan het organisatiebeleid en best practices moet worden geïmplementeerd.</p><p>Om te beschermen tegen gewelddadige en op basis van woordenlijst raden: beleid voor sterke wachtwoorden moet worden geïmplementeerd om ervoor te zorgen dat gebruikers complex wachtwoord (bijv, de minimumlengte 12 tekens, alfanumerieke en speciale tekens) maken.</p><p>Het beleid voor accountvergrendeling mogen worden uitgevoerd in de volgende manier:</p><ul><li>**Voorlopig lock-out:** dit is een goede optie voor het beveiligen van uw gebruikers tegen gewelddadige aanvallen. Bijvoorbeeld, wanneer de gebruiker een onjuist wachtwoord invoert driemaal de toepassing kan worden vergrendeld het account voor een minuut om het proces van het forceren van zijn/haar wachtwoord zodat u minder winstgevend voor de aanvaller om door te gaan brute vertragen. Als u tegenmaatregelen harde lock-out voor dit voorbeeld zou worden behaald een 'Dos' door permanent accounts worden vergrendeld. U kunt ook toepassing kan een OTP (één keer wachtwoord) genereren en verzenden out-of-band (via e-mail, sms, enz.) aan de gebruiker. Een andere benadering mogelijk CAPTCHA implementeren als een drempelwaarde voor aantal mislukte pogingen is bereikt.</li><li>**Vaste lock-out:** dit type vergrendeling moet worden toegepast wanneer u een gebruiker een aanval op uw toepassing te detecteren en hem teller met behulp van hun account permanent accountvergrendeling totdat een team antwoord gehad hun forensische doen. Na dit proces die u bepalen kunt dat de gebruiker back hun account of maatregelen verdere juridische tegen hem. Dit type benadering wordt voorkomen dat de aanvaller verdere vocht uw toepassingen en infrastructuur.</li></ul><p>Als u wilt beschermen tegen aanvallen op standaard en voorspelbare accounts, controleren of alle sleutels en wachtwoorden replaceable zijn, en zijn gegenereerd of na het tijdstip van de vervangen.</p><p>Als de toepassing heeft voor het automatisch genereren van wachtwoorden, zorg ervoor dat de gegenereerde wachtwoorden willekeurige worden en hoge entropie hebben.</p>|

## <a id="controls-username-enum"></a>Besturingselementen om te voorkomen dat de gebruikersnaam opsomming implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Alle foutberichten moeten worden gegeneraliseerd om te voorkomen dat de gebruikersnaam-opsomming. Ook soms kan niet worden vermeden informatie in functies zoals een registratiepagina lekken. U moet hier snelheidsbeperking methoden zoals CAPTCHA gebruiken om te voorkomen dat een geautomatiseerde aanval door een aanvaller. |

## <a id="win-authn-sql"></a>Gebruik zo mogelijk Windows-verificatie voor de verbinding met SQL Server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | OnPrem |
| **Kenmerken**              | SQL-versie - alle |
| **Verwijzingen**              | [SQL Server - een verificatiemodus kiezen](https://msdn.microsoft.com/library/ms144284.aspx) |
| **Stappen** | Windows-verificatie gebruikt Kerberos veiligheidsprotocol, voorziet wachtwoord-beleidsafdwinging voor validatie op complexiteit voor sterke wachtwoorden, biedt ondersteuning voor accountvergrendeling en ondersteunt verlopen van wachtwoorden.|

## <a id="aad-authn-sql"></a>Gebruik zo mogelijk Azure Active Directory-verificatie om verbinding te maken met SQL-Database

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure |
| **Kenmerken**              | SQL-versie - V12 |
| **Verwijzingen**              | [Verbinding maken met SQL-Database via Azure Active Directory-verificatie](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/) |
| **Stappen** | **Minimale versie:** Azure SQL Database V12 vereist voor het toestaan van Azure SQL Database AAD-verificatie op basis van de Microsoft-Directory gebruiken |

## <a id="authn-account-pword"></a>SQL-verificatiemodus wordt gebruikt, zorg er bij dat account en wachtwoord beleid worden afgedwongen op SQL server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [SQL Server-wachtwoordbeleid](https://technet.microsoft.com/library/ms161959(v=sql.110).aspx) |
| **Stappen** | Wanneer u SQL Server-verificatie gebruikt, worden de aanmeldingen gemaakt in SQL Server die niet zijn gebaseerd op Windows-gebruikersaccounts. Zowel de gebruikersnaam en het wachtwoord zijn gemaakt met behulp van SQL Server en opgeslagen in SQL Server. SQL Server kan mechanismen voor beleid voor Windows-wachtwoord gebruiken. De dezelfde complexiteit en het verloopbeleid gebruikt in Windows om wachtwoorden in SQL Server gebruikt kan worden toegepast. |

## <a id="autn-contained-db"></a>Gebruik geen SQL-verificatie in ingesloten databases

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | OnPrem, Azure SQL |
| **Kenmerken**              | SQL - MSSQL2012, SQL-versie --versie V12 |
| **Verwijzingen**              | [Best Practices voor beveiliging met ingesloten Databases](http://msdn.microsoft.com/library/ff929055.aspx) |
| **Stappen** | Het ontbreken van een wachtwoordbeleid afgedwongen kunt vergroten door de kans op een zwakke referentie in een ingesloten database tot stand wordt gebracht. Maak gebruik van Windows-verificatie. |

## <a id="authn-sas-tokens"></a>Per apparaat referenties voor verificatie met behulp van SaS-tokens worden gebruikt

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Verificatie en beveiliging model overzicht van Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | <p>Het beveiligingsmodel van Event Hubs is gebaseerd op een combinatie van Shared Access Signature (SAS)-tokens en gebeurtenisuitgevers. Naam van de uitgever vertegenwoordigt de apparaat-id die de token ontvangt. Hierdoor zou de tokens die zijn gegenereerd met de betreffende apparaten koppelen.</p><p>Alle berichten zijn gelabeld met de oorspronkelijke aanvrager op servicezijde zodat de detectie van in de nettolading oorsprong adresvervalsing (spoofing) pogingen. Bij het verifiëren van apparaten genereren een per SaS-token van een apparaat binnen het bereik van een unieke uitgever.</p>|

## <a id="multi-factor-azure-admin"></a>Azure multi-factor Authentication voor beheerders voor Azure inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van Azure | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Wat is Azure Multi-Factor Authentication?](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) |
| **Stappen** | <p>Multi-factor authentication (MFA) is een authenticatiemethode die meer dan één verificatiemethode is vereist en een kritieke tweede beveiligingslaag wordt toegevoegd aan de gebruikersaanmeldingen en transacties. Het werkt door te vereisen van twee of meer van de volgende verificatiemethoden:</p><ul><li>Iets u weet (doorgaans een wachtwoord)</li><li>Iets er (een vertrouwd apparaat die niet eenvoudig wordt gedupliceerd, zoals een telefoon)</li><li>Iets dat u (biometrie)</li><ul>|

## <a id="anon-access-cluster"></a>Anonieme toegang beperken tot Service Fabric-Cluster

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Service Fabric-Vertrouwensgrenzen | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure  |
| **Verwijzingen**              | [Scenario's voor beveiliging van service Fabric-cluster](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security) |
| **Stappen** | <p>Clusters moeten altijd worden beveiligd om te voorkomen dat onbevoegde gebruikers verbinding maken met uw cluster, vooral wanneer er productieworkloads uitgevoerd.</p><p>Zorg ervoor dat de beveiligingsmodus is ingesteld op "veilig" en het configureren van het certificaat van de vereiste x.509-server tijdens het maken van een service fabric-cluster. Maken van een cluster 'onveilig', krijgt elke anonieme gebruiker verbinding kunnen maken als eindpunten voor beheer met het openbare Internet worden getoond.</p>|

## <a id="fabric-cn-nn"></a>Zorg ervoor dat Service Fabric-clientknooppunt certificaat verschilt van het knooppunt naar certificaat

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Service Fabric-Vertrouwensgrenzen | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Zelfstandige omgeving - Azure, omgeving- |
| **Verwijzingen**              | [Beveiliging van service Fabric-clientknooppunt certificate](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#_client-to-node-certificate-security), [verbinding maken met een beveiligde cluster met behulp van clientcertificaat](https://azure.microsoft.com/documentation/articles/service-fabric-connect-to-secure-cluster/) |
| **Stappen** | <p>De beveiliging van client-naar-node certificate is tijdens het maken van het cluster via de Azure-portal Resource Manager-sjablonen of een zelfstandige JSON-sjabloon door op te geven van een clientcertificaat voor de beheerder en/of een clientcertificaat voor de gebruiker geconfigureerd.</p><p>De client en de gebruiker beheerclientcertificaten die u opgeeft moet verschillen van de primaire en secundaire certificaten die u voor knooppunt naar beveiliging opgeeft.</p>|

## <a id="aad-client-fabric"></a>AAD voor verificatie van clients naar service fabric-clusters gebruiken.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Service Fabric-Vertrouwensgrenzen | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure |
| **Verwijzingen**              | [Scenario's voor beveiliging - aanbevelingen voor beveiliging](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#security-recommendations) |
| **Stappen** | Clusters die worden uitgevoerd op Azure kunnen ook beveiligde toegang tot de eindpunten voor beheer met behulp van Azure Active Directory (AAD), afgezien van clientcertificaten. Voor Azure clusters, wordt het aanbevolen gebruik van AAD-beveiliging voor verificatie van clients en certificaten voor knooppunt naar beveiliging.|

## <a id="fabric-cert-ca"></a>Zorg ervoor dat service fabric-certificaten van een erkende certificeringsinstantie (CA) worden verkregen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Service Fabric-Vertrouwensgrenzen | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure |
| **Verwijzingen**              | [X.509-certificaten en Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#x509-certificates-and-service-fabric) |
| **Stappen** | <p>Service Fabric maakt gebruik van x.509-certificaten voor het verifiëren van de knooppunten en clients.</p><p>Enkele belangrijke overwegingen bij het gebruik van certificaten in de service-fabrics:</p><ul><li>Certificaten worden gebruikt in clusters met productieworkloads moeten worden gemaakt met een juist geconfigureerde service voor Windows Server-certificaat of verkregen van een erkende certificeringsinstantie (CA). De CA mag een goedgekeurde externe Certificeringsinstantie of een goed beheerde interne Public Key Infrastructure (PKI)</li><li>Gebruik nooit een tijdelijke en certificaten in productie die zijn gemaakt met de hulpprogramma's zoals MakeCert.exe testen</li><li>U kunt een zelfondertekend certificaat gebruiken, maar moet alleen doen voor testclusters en niet in productie</li></ul>|

## <a id="standard-authn-id"></a>Gebruik standaard verificatie scenario's ondersteund door Identiteitsserver

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [IdentityServer3 - de grote afbeelding](https://identityserver.github.io/Documentation/docsv2/overview/bigPicture.html) |
| **Stappen** | <p>Hieronder volgen de typische interacties ondersteund door de identiteit van Server:</p><ul><li>Browsers communiceren met webtoepassingen</li><li>Webtoepassingen communiceren met de web-API's (soms op hun eigen soms namens een gebruiker)</li><li>Browser gebaseerde toepassingen communiceren met de web-API 's</li><li>Systeemeigen toepassingen communiceren met de web-API 's</li><li>Servertoepassingen communiceren met de web-API 's</li><li>Web-API's communiceren met de web-API's (soms op hun eigen soms namens een gebruiker)</li></ul>|

## <a id="override-token"></a>Standaard identiteit token cachegeheugen van de Server met een schaalbare alternatief overschrijven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Implementatie van de Server Identity - Caching](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Stappen** | <p>IdentityServer heeft een eenvoudige ingebouwde geheugencache. Dit is geschikt voor kleine schaal systeemeigen apps, deze niet kan worden uitgebreid voor mid-laag en back-end-toepassingen voor de volgende redenen:</p><ul><li>Deze toepassingen zijn toegankelijk door veel gebruikers tegelijk. Alle toegangstokens opslaan op dezelfde locatie isolatie problemen maakt en geeft de uitdagingen bij het besturingssysteem op grote schaal: veel gebruikers, elk met zoveel tokens terwijl de resources die de app toegang heeft tot namens hen kunnen betekenen grote aantallen en de bewerkingen erg kostbaar lookup</li><li>Deze toepassingen worden doorgaans geïmplementeerd op gedistribueerde topologieën, waarbij meerdere knooppunten toegang tot de dezelfde cache hebben</li><li>In de cache tokens moeten overleven wordt gerecycled en deactivations</li><li>Voor de bovenstaande redenen wordt tijdens de implementatie van web-apps, aanbevolen voor het onderdrukken van de standaardserver identiteit tokencache met een schaalbare alternatief zoals Azure Redis-cache</li></ul>|

## <a id="binaries-signed"></a>Zorg ervoor dat de binaire bestanden van geïmplementeerde toepassingen digitaal zijn ondertekend

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Een grens machine vertrouwensrelatie | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Zorg ervoor dat binaire bestanden van geïmplementeerde toepassingen digitaal zijn ondertekend, zodat de integriteit van de binaire bestanden kan worden geverifieerd.|

## <a id="msmq-queues"></a>-Verificatie inschakelen bij het verbinden met de MSMQ-wachtrijen in WCF

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N.v.t. |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx) |
| **Stappen** | Programma is mislukt-verificatie inschakelen bij het verbinden met de MSMQ-wachtrijen, een aanvaller kan anoniem indienen berichten naar de wachtrij voor verwerking. Als verificatie geen verbinding maken met een MSMQ-wachtrij gebruikt voor het afleveren van een bericht naar een ander programma gebruikt is, kan een aanvaller een anonieme bericht dat schadelijk is verzonden.|

### <a name="example"></a>Voorbeeld
De `<netMsmqBinding/>` element van het configuratiebestand WCF Hiermee geeft u WCF verificatie uitschakelen bij het verbinden met een MSMQ-wachtrij voor de levering van berichten.
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
Configureer MSMQ vereist verificatie van Windows-domein of certificaat altijd voor binnenkomende of uitgaande berichten.

### <a name="example"></a>Voorbeeld
De `<netMsmqBinding/>` element van het configuratiebestand WCF Hiermee geeft u WCF verificatie via certificaat inschakelen bij het verbinden met een MSMQ-wachtrij. De client is geverifieerd met behulp van x.509-certificaten. Het certificaat moet aanwezig zijn in het certificaatarchief van de server.
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

## <a id="message-none"></a>WCF-Do bericht clientCredentialType niet ingesteld op none

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET framework 3 |
| **Kenmerken**              | Het referentietype client - geen |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | Het ontbreken van verificatie betekent dat iedereen toegang kunnen krijgen tot deze service. Een service die de clients niet geverifieerd staat toegang tot alle gebruikers. De toepassing worden geverifieerd bij de clientreferenties configureren. Dit kan worden gedaan door de bericht-clientCredentialType in te stellen op Windows of het certificaat. |

### <a name="example"></a>Voorbeeld
```
<message clientCredentialType=""Certificate""/>
```

## <a id="transport-none"></a>WCF-Do Transport clientCredentialType niet ingesteld op none

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, .NET Framework 3 |
| **Kenmerken**              | Het referentietype client - geen |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | Het ontbreken van verificatie betekent dat iedereen toegang kunnen krijgen tot deze service. Een service die de clients niet geverifieerd kan alle gebruikers toegang krijgen tot de functionaliteit hiervan. De toepassing worden geverifieerd bij de clientreferenties configureren. Dit kan worden gedaan door het transport clientCredentialType in te stellen op Windows of het certificaat. |

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
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Verificatie en autorisatie in ASP.NET Web-API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api), [externe verificatieservices met ASP.NET-Web-API (C#)](http://www.asp.net/web-api/overview/security/external-authentication-services) |
| **Stappen** | <p>Verificatie is het proces waarbij een entiteit aantoont de identiteit, doorgaans via referenties, zoals een gebruikersnaam en wachtwoord dat. Er zijn verschillende verificatieprotocollen beschikbaar die kunnen worden beschouwd. Enkele ervan worden hieronder weergegeven:</p><ul><li>Clientcertificaten</li><li>Op basis van Windows</li><li>Op basis van formulieren</li><li>Federation - ADFS</li><li>Federation - Azure AD</li><li>Federation - Identiteitsserver</li></ul><p>Koppelingen in de sectie verwijzingen bevatten op laag niveau details over hoe elk van de verificatieschema's kan worden geïmplementeerd voor het beveiligen van een Web-API.</p>|

## <a id="authn-aad"></a>Gebruik standaard verificatie scenario's ondersteund door Azure Active Directory

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Verificatie-scenario's voor Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/), [Azure Active Directory-codevoorbeelden](https://azure.microsoft.com/documentation/articles/active-directory-code-samples/), [ontwikkelaarshandleiding Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-developers-guide/) |
| **Stappen** | <p>Azure Active Directory (Azure AD) vereenvoudigt de verificatie voor ontwikkelaars dankzij de identiteit als een service met ondersteuning voor de industriestandaard-protocollen zoals OAuth 2.0 en OpenID Connect. Hieronder worden de vijf primaire toepassing scenario's ondersteund door Azure AD:</p><ul><li>Webbrowser naar webtoepassing: een gebruiker moet zich aanmelden bij een webtoepassing die wordt beveiligd door Azure AD</li><li>Één pagina toepassing (SPA): Een gebruiker moet zich aanmelden bij een toepassing met één pagina die wordt beveiligd door Azure AD</li><li>Systeemeigen toepassing naar de Web-API: een systeemeigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC moet een gebruiker van een web-API die wordt beveiligd door Azure AD om bronnen te verifiëren</li><li>Webtoepassing voor Web-API: een webtoepassing moet bronnen ophalen van een web-API die zijn beveiligd door Azure AD</li><li>Daemon of Server-toepassing naar de Web-API: een daemontoepassing of een servertoepassing zonder gebruikersinterface web moet bronnen ophalen van een web-API die zijn beveiligd door Azure AD</li></ul><p>Raadpleeg de koppelingen in de sectie Verwijzingen voor op laag niveau implementatiegegevens</p>|

## <a id="adal-scalable"></a>De standaard ADAL tokencache met een schaalbare alternatief overschrijven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Moderne verificatie met Azure Active Directory voor webtoepassingen](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/), [als ADAL-token cache met behulp van Redis](https://blogs.msdn.microsoft.com/mrochon/2016/09/19/using-redis-as-adal-token-cache/)  |
| **Stappen** | <p>De standaardcache ADAL (Active Directory Authentication Library) gebruikt een cache in het geheugen die afhankelijk is van een statische opslag proces hele beschikbaar is. Terwijl dit voor toepassingen werkt, deze niet kan worden uitgebreid voor mid-laag en back-end-toepassingen voor de volgende redenen:</p><ul><li>Deze toepassingen zijn toegankelijk door veel gebruikers tegelijk. Alle toegangstokens opslaan op dezelfde locatie isolatie problemen maakt en geeft de uitdagingen bij het besturingssysteem op grote schaal: veel gebruikers, elk met zoveel tokens terwijl de resources die de app toegang heeft tot namens hen kunnen betekenen grote aantallen en de bewerkingen erg kostbaar lookup</li><li>Deze toepassingen worden doorgaans geïmplementeerd op gedistribueerde topologieën, waarbij meerdere knooppunten toegang tot de dezelfde cache hebben</li><li>In de cache tokens moeten overleven wordt gerecycled en deactivations</li></ul><p>Om de bovenstaande redenen tijdens de implementatie van web-apps, verdient het overschrijven van de standaard ADAL tokencache met een schaalbare alternatief zoals Azure Redis-cache.</p>|

## <a id="tokenreplaycache-adal"></a>Zorg dat TokenReplayCache wordt gebruikt om te voorkomen dat de replay van tokens voor ADAL-verificatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Moderne verificatie met Azure Active Directory voor webtoepassingen](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/) |
| **Stappen** | <p>De eigenschap TokenReplayCache kan ontwikkelaars een cache tokenreplay, een store, die kan worden gebruikt voor het opslaan van tokens om na te gaan dat geen token kan meer dan eenmaal worden gebruikt.</p><p>Dit is een meting op basis van een algemene-aanval neemt de aanval toepasselijke aangeroepen tokenreplay: een aanvaller onderscheppen van het token verzonden bij het aanmelden te verzenden aan de app opnieuw proberen ("opnieuw afspelen') voor het tot stand brengen van een nieuwe sessie. Bijvoorbeeld In OIDC code grant stroom na een geslaagde verificatie, een aanvraag voor het "/ signin-oidc ' eindpunt van de relying party wordt gemaakt met 'id_token', 'code' en 'status'-parameters.</p><p>De relying party deze aanvraag valideert en er wordt een nieuwe sessie. Als een adversary deze aanvraag wordt vastgelegd en opnieuw wordt weergegeven, kan hij een geslaagde sessie tot stand brengen en de gebruiker nabootsen. De aanwezigheid van de nonce in OpenID Connect kunt beperken, maar niet volledig elimineren de omstandigheden waarin de aanval met succes kan worden vastgesteld. Omwille van hun toepassingen kunnen ontwikkelaars een implementatie leveren van ITokenReplayCache en toewijzen aan een exemplaar TokenReplayCache.</p>|

### <a name="example"></a>Voorbeeld
```C#
// ITokenReplayCache defined in ADAL
public interface ITokenReplayCache
{
bool TryAdd(string securityToken, DateTime expiresOn);
bool TryFind(string securityToken);
}
```

### <a name="example"></a>Voorbeeld
Hier volgt een Voorbeeldimplementatie van de interface ITokenReplayCache. (Geef aanpassen en implementeren van uw project-specifieke cache-framework)
```C#
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
De geïmplementeerde cache is verwezen in de opties OIDC via de eigenschap 'TokenValidationParameters' als volgt.
```C#
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

Houd er rekening mee dat de effectiviteit van deze configuratie kunt aanmelden bij uw lokale OIDC beveiligde toepassingen testen en vastleggen van de aanvraag voor `"/signin-oidc"` eindpunt in fiddler. Deze aanvraag in fiddler herstelbewerking wordt wanneer de beveiliging niet aanwezig is, een nieuwe sessiecookie ingesteld. Wanneer de aanvraag wordt opnieuw afgespeeld nadat de beveiliging TokenReplayCache is toegevoegd, wordt de toepassing Veroorzaak een exception als volgt:`SecurityTokenReplayDetectedException: IDX10228: The securityToken has previously been validated, securityToken: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ1......`

## <a id="adal-oauth2"></a>ADAL-bibliotheken gebruiken voor het beheren van aanvragen voor beveiligingstokens van OAuth2-clients voor AAD (of on-premises AD)

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure AD | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [ADAL](https://azure.microsoft.com/documentation/articles/active-directory-authentication-libraries/) |
| **Stappen** | <p>De Azure AD authentication Library (ADAL) kan toepassingsontwikkelaars client eenvoudig om gebruikers te verifiëren naar de cloud of on-premises Active Directory (AD), en vervolgens verkrijgen toegangstokens voor het beveiligen van API-aanroepen.</p><p>ADAL bevat veel functies die verificatie maken eenvoudiger voor ontwikkelaars, zoals ondersteuning voor asynchrone, een configureerbare tokencache waarmee toegangstokens en vernieuwen van tokens, automatisch token vernieuwen wanneer een toegangstoken is verlopen en een vernieuwingstoken beschikbaar is, worden opgeslagen en meer.</p><p>Door het verwerken van de meeste van de complexiteit ADAL een ontwikkelaar focus op bedrijfslogica in de toepassing in help en eenvoudig resources beveiligen zonder een expert over de beveiliging. Afzonderlijke bibliotheken zijn beschikbaar voor .NET, JavaScript (client en Node.js), iOS, Android en Java.</p>|

## <a id="authn-devices-field"></a>Verificatie van apparaten die verbinding maken met de Gateway veld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld IoT Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Zorg ervoor dat elk apparaat wordt geverifieerd door de Gateway veld voordat u gegevens uit deze accepteert en vóór de upstream-communicatie met de Gateway van de Cloud. Zorg er ook voor dat apparaten verbinding met maken een per apparaat credential zodat afzonderlijke apparaten uniek kunnen worden geïdentificeerd.|

## <a id="authn-devices-cloud"></a>Zorg ervoor dat de apparaten die verbinding maken met Cloud-gateway worden geverifieerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-Cloudgateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, C#, Node.JS,  |
| **Kenmerken**              | N.V.T., Gateway keuze - Azure IoT Hub |
| **Verwijzingen**              | N.V.T., [Azure IoT hub met .NET](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/), [wih IoT-hub aan de slag en knooppunt JS](https://azure.microsoft.com/documentation/articles/iot-hub-node-node-getstarted), [IoT met SAS en certificaten beveiligen](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/), [Git-opslagplaats](https://github.com/Azure/azure-iot-sdks/tree/master/node) |
| **Stappen** | <ul><li>**Algemene:** verifiëren van het apparaat met Transport Layer Security (TLS) of IPSec. Infrastructuur moet ondersteuning met behulp van vooraf gedeelde sleutel (PSK) op apparaten die volledige asymmetrische cryptografie kunnen niet worden verwerkt. Maak gebruik van Azure AD, Oauth.</li><li>**C#:** bij het maken van een exemplaar DeviceClient standaard, de methode Create maakt een DeviceClient-exemplaar dat het AMQP-protocol gebruikt om te communiceren met IoT Hub. U kunt het HTTPS-protocol, gebruiken de onderdrukking van de methode Create waarmee u het protocol opgeven. Als u het HTTPS-protocol gebruikt, moet u ook toevoegen de `Microsoft.AspNet.WebApi.Client` NuGet-pakket aan uw project om op te nemen de `System.Net.Http.Formatting` naamruimte.</li></ul>|

### <a name="example"></a>Voorbeeld
```C#
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
* Een iothub maken in azure
* Maken van een vermelding in het register voor apparaat-id 's
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
#### <a name="sas-token"></a>SAS-Token
* Opgehaald intern zijn gegenereerd wanneer u symmetrische sleutel, maar we kunnen genereren en deze ook expliciet gebruiken
* Een protocol definiëren:`var Http = require('azure-iot-device-http').Http;`
* Een sas-token maken:
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
* Verbinding maken via sas-token: 
    ```javascript
    Client.fromSharedAccessSignature(sas, Http); 
    ```
#### <a name="certificates"></a>Certificaten
* Een zelf-ondertekend X509 genereren van het certificaat met een hulpprogramma zoals OpenSSL voor het genereren van een .cert en .key bestanden voor het opslaan van het certificaat en de sleutel respectievelijk
* Inrichten van een apparaat dat accepteert beveiligde verbinding met behulp van certificaten.
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
| **Onderdeel**               | IoT-Cloudgateway  | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Keuze van de gateway - Azure IoT Hub |
| **Verwijzingen**              | [Azure IoT Hub beveiligingstokens](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/) |
| **Stappen** | Gebruik per apparaat referenties voor verificatie met behulp van SaS-tokens op basis van de sleutel voor apparaat of een clientcertificaat, in plaats van IoT Hub-niveau gedeelde toegangsbeleid. Hiermee voorkomt u dat het hergebruik van verificatietokens van een apparaat of het veld gateway door een andere |

## <a id="req-containers-anon"></a>Zorg ervoor dat alleen de vereiste containers en blobs anonieme leestoegang krijgen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - Blob |
| **Verwijzingen**              | [Anonieme leestoegang tot containers en blobs beheren](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/), [handtekeningen voor gedeelde toegang, deel 1: inzicht in het SAS-model](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) |
| **Stappen** | <p>Standaard kunnen een container en alle bestaande blobs erin alleen worden geopend door de eigenaar van het opslagaccount. Als u anonieme gebruikers leesmachtigingen voor een container en de blobs geven, kan een de machtigingen van de container om openbare toegang te verlenen ingesteld. Anonieme gebruikers kunnen BLOB's binnen een container openbaar toegankelijk lezen zonder verificatie van de aanvraag.</p><p>Containers bieden de volgende opties voor het beheren van toegang tot de container:</p><ul><li>Volledige openbare leestoegang: Container en de blob-gegevens kunnen worden gelezen via anonieme aanvraag. Clients kunnen inventariseren blobs in de container via anonieme aanvraag, maar kunnen containers in het opslagaccount niet inventariseren.</li><li>Openbare leestoegang voor blobs alleen: Blob-gegevens in deze container via anonieme aanvragen kunnen worden gelezen, maar de containergegevens is niet beschikbaar. Clients kunnen blobs in de container via anonieme aanvragen niet inventariseren</li><li>Geen enkele openbare leestoegang: Container en de blob-gegevens kunnen worden gelezen door alleen de eigenaar van het account</li></ul><p>Anonieme toegang wordt aanbevolen voor scenario's waarin bepaalde blobs altijd beschikbaar voor anonieme toegang voor lezen zijn moeten. Voor nauwkeurigere beheer, kan een shared access signature, waardoor beperkte Gemachtigdentoegang met behulp van andere machtigingen en gedurende een opgegeven tijdsinterval worden gemaakt. Zorg ervoor dat containers en blobs, dat mogelijk gevoelige gegevens bevatten kunnen, niet anonieme toegang per ongeluk opgegeven zijn</p>|

## <a id="limited-access-sas"></a>Beperkte toegang verlenen tot objecten in Azure-opslag met SAS of SAP

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t. |
| **Verwijzingen**              | [Shared Access Signatures, deel 1: Inzicht in het SAS-model](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/), [handtekeningen voor gedeelde toegang, deel 2: maken en gebruiken van een SAS met Blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/), [het overdragen van toegang tot objecten in uw account met gedeeld Handtekeningen voor toegang en opgeslagen toegangsbeleid](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies) |
| **Stappen** | <p>Met behulp van een shared access signature is (SAS) een krachtige manier om beperkte toegang tot objecten in een opslagaccount verlenen aan andere clients zonder zichtbaar maken accounttoegangssleutel. De SAS is een URI die in de queryparameters omvat alle informatie die nodig zijn voor geverifieerde toegang tot een opslagresource. Voor toegang tot de storage-resources met de SA's, moet de client alleen worden doorgegeven in de SA's aan de juiste constructor of methode.</p><p>U kunt een SAS gebruiken wanneer u wilt toegang bieden tot bronnen in uw opslagaccount naar een client die niet kan vertrouwd met de accountsleutel worden. De opslagaccountsleutels bevatten zowel een primaire en secundaire sleutel, die beide beheerderstoegang tot uw account en alle resources daarin Verleen. Uw account met de mogelijkheid van kwaadwillende of Achteloos gebruik blootstellen van een van de sleutels van uw account worden geopend. Handtekeningen voor gedeelde toegang bieden een veilige alternatief waarmee andere clients kunnen lezen, schrijven en verwijderen van gegevens in uw opslagaccount volgens de machtigingen die u hebt verleend en zonder dat nodig is voor de accountsleutel.</p><p>Als u een logische set parameters die vergelijkbaar telkens zijn hebt, is gebruik van opgeslagen Access Policy (SAP) een beter idee. Omdat met behulp van een SAS afgeleid van een opgeslagen-beleid u de mogelijkheid om in te trekken die SAS onmiddellijk biedt, is het aanbevolen gebruik altijd toegangsbeleid opgeslagen indien mogelijk.</p>|