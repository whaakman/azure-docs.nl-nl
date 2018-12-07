---
title: Autorisatie - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
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
ms.openlocfilehash: ddf40538fc3d6e39fe48ff49311f86314008b4ce
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994764"
---
# <a name="security-frame-authorization--mitigations"></a>Beveiliging-Frame: Autorisatie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Vertrouwensgrenzen van computer** | <ul><li>[Zorg ervoor dat de juiste ACL's zijn geconfigureerd voor niet-geautoriseerde toegang tot gegevens op het apparaat beperken](#acl-restricted-access)</li><li>[Ervoor te zorgen dat gevoelige gebruikersspecifieke toepassingsinhoud is opgeslagen in de map gebruiker profiel](#sensitive-directory)</li><li>[Zorg ervoor dat de geïmplementeerde toepassingen worden uitgevoerd met de minimaal benodigde bevoegdheden](#deployed-privileges)</li></ul> |
| **Web-App** | <ul><li>[Sequentiële volgorde afdwingen bij het verwerken van zakelijke logicastromen](#sequential-logic)</li><li>[Frequentie mechanisme om te voorkomen dat opsomming implementeren](#rate-enumeration)</li><li>[Zorg ervoor dat de juiste machtiging ingesteld is en principe van minimale bevoegdheden wordt gevolgd](#principle-least-privilege)</li><li>[Zakelijke logica en resource access autorisatie beslissingen te nemen moeten niet worden gebaseerd op de parameters van de inkomende aanvraag](#logic-request-parameters)</li><li>[Zorg ervoor dat die inhoud en resources zijn niet invoeroverzicht of toegankelijk is via geforceerde bladeren](#enumerable-browsing)</li></ul> |
| **Database** | <ul><li>[Zorg ervoor dat de minste bevoegdheden accounts worden gebruikt verbinding maken met Database-server](#privileged-server)</li><li>[Rij niveau van beveiliging RLS om te voorkomen dat tenants toegang tot elkaars gegevens implementeren](#rls-tenants)</li><li>[Rol sysadmin mag alleen geldige noodzakelijk gebruikers hebben.](#sysadmin-users)</li></ul> |
| **IoT-Cloud-Gateway** | <ul><li>[Verbinding maken met Cloud-Gateway met behulp van de minste bevoegdheden tokens](#cloud-least-privileged)</li></ul> |
| **Azure Event Hub** | <ul><li>[Gebruik van een verzenden alleen-lezen machtigingen SAS-sleutel voor het genereren van apparaattokens](#sendonly-sas)</li><li>[Gebruik geen toegangstokens die voorzien in directe toegang tot de Event Hub](#access-tokens-hub)</li><li>[Verbinding maken met Event Hub met behulp van SAS-sleutels met de minimale machtigingen die vereist zijn](#sas-minimum-permissions)</li></ul> |
| **Azure Documentdb** | <ul><li>[Resource-tokens gebruiken om te verbinden met Azure Cosmos DB indien mogelijk](#resource-docdb)</li></ul> |
| **Vertrouwensgrenzen van Azure** | <ul><li>[Verfijnd toegangsbeheer met RBAC met Azure-abonnement inschakelen](#grained-rbac)</li></ul> |
| **Vertrouwensgrenzen van service Fabric** | <ul><li>[Van de client toegang beperken tot bewerkingen voor een cluster met behulp van RBAC](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Beveiliging-modellen uitvoeren en gebruiken van veld beveiligingsniveau vereist](#modeling-field)</li></ul> |
| **Dynamics CRM-Portal** | <ul><li>[Modellering van de beveiliging van portal accounts, houd er rekening mee dat het beveiligingsmodel voor de portal wijkt af van de rest van CRM uitvoeren](#portal-security)</li></ul> |
| **Azure Storage** | <ul><li>[Fijnmazig machtiging verlenen voor een bereik van entiteiten in Azure Table Storage](#permission-entities)</li><li>[Rollen gebaseerd toegangsbeheer (RBAC) naar Azure storage-account met Azure Resource Manager inschakelen](#rbac-azure-manager)</li></ul> |
| **Mobiele Client** | <ul><li>[Impliciete jailbreak of detectie basismappen implementeren](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[Zwakke Class Reference in WCF](#weak-class-wcf)</li><li>[Besturingselement voor autorisatie van WCF-implementeren](#wcf-authz)</li></ul> |
| **Web-API** | <ul><li>[Juiste autorisatiemechanismen in ASP.NET-Web-API implementeren](#authz-aspnet)</li></ul> |
| **IoT-apparaat** | <ul><li>[Autorisatie-controles uitvoeren in het apparaat als deze ondersteuning biedt voor verschillende acties waarvoor verschillende machtigingsniveaus](#device-permission)</li></ul> |
| **Veld voor IoT-Gateway** | <ul><li>[Autorisatie-controles uitvoeren in de Gateway van het veld als deze ondersteuning biedt voor verschillende acties waarvoor verschillende machtigingsniveaus](#field-permission)</li></ul> |

## <a id="acl-restricted-access"></a>Zorg ervoor dat de juiste ACL's zijn geconfigureerd voor niet-geautoriseerde toegang tot gegevens op het apparaat beperken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van computer | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat de juiste ACL's zijn geconfigureerd voor niet-geautoriseerde toegang tot gegevens op het apparaat beperken|

## <a id="sensitive-directory"></a>Ervoor te zorgen dat gevoelige gebruikersspecifieke toepassingsinhoud is opgeslagen in de map gebruiker profiel

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van computer | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat gevoelige gebruikersspecifieke toepassingsinhoud wordt opgeslagen in gebruikersprofiel directory. Dit is om te voorkomen dat meerdere gebruikers van de computer toegang krijgen tot gegevens van elkaar.|

## <a id="deployed-privileges"></a>Zorg ervoor dat de geïmplementeerde toepassingen worden uitgevoerd met de minimaal benodigde bevoegdheden

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van computer | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat de geïmplementeerde toepassing wordt uitgevoerd met minimale bevoegdheden. |

## <a id="sequential-logic"></a>Sequentiële volgorde afdwingen bij het verwerken van zakelijke logicastromen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Om te controleren of deze fase is uitgevoerd via een legitieme gebruiker die u wilt afdwingen van de toepassing alleen zakelijke logica processtromen in opeenvolgende volgorde, met alle stappen realistische menselijke tijdstip wordt verwerkt en niet verwerken volgorde, overgeslagen stappen , stappen van een andere gebruiker verwerkt of verzonden te snel transacties.|

## <a id="rate-enumeration"></a>Frequentie mechanisme om te voorkomen dat opsomming implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat gevoelige id random. CAPTCHA-besturingselement op anonieme's implementeren. Zorg ervoor dat fout- en uitzondering niet specifieke gegevens bekendgemaakt moeten|

## <a id="principle-least-privilege"></a>Zorg ervoor dat de juiste machtiging ingesteld is en principe van minimale bevoegdheden wordt gevolgd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Het principe betekent dat een gebruikersaccount geven alleen deze bevoegdheden die essentieel zijn voor dat gebruikers werken. Back-up van een gebruiker heeft bijvoorbeeld niet nodig om software te installeren: Hierdoor kan de back-gebruiker rechten voor het uitvoeren van back-ups en back-up-gerelateerde toepassingen heeft. Andere bevoegdheden, zoals het installeren van nieuwe software, worden geblokkeerd. Het principe geldt ook voor een pc-gebruiker die normaal gesproken werkt in een normaal gebruikersaccount en opent een account met machtigingen, wachtwoord zijn beveiligd (dat wil zeggen, een supergebruiker) alleen wanneer de situatie absoluut vereist dit. </p><p>Dit principe kan ook worden toegepast op uw webtoepassingen. In plaats van alleen al naar gelang op rollen gebaseerde verificatiemethoden met behulp van sessies, in plaats daarvan gaan we machtigingen toewijzen aan gebruikers met behulp van een systeem met verificatie op basis van een Database. We nog steeds sessies gebruiken om te bepalen of de gebruiker is aangemeld correct alleen nu in plaats van die gebruiker met een specifieke functie die we hem toewijzen met bevoegdheden om te controleren welke acties hij bepaalde bevoegdheden heeft uit te voeren op het systeem toe te wijzen. Een grote pro van deze methode is ook, wanneer een gebruiker minder bevoegdheden die uw wijzigingen worden toegepast op elk gewenst moment na het toewijzen is niet afhankelijk van de sessie die anders verlopen eerst moest worden toegewezen.</p>|

## <a id="logic-request-parameters"></a>Zakelijke logica en resource access autorisatie beslissingen te nemen moeten niet worden gebaseerd op de parameters van de inkomende aanvraag

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Wanneer u controleert of een gebruiker beperkt is tot bepaalde gegevens bekijken, moet de toegangsbeperkingen verwerkte serverzijde. De gebruikers-id moet worden opgeslagen in een variabele van sessie op de aanmelding en om gebruikersgegevens te halen uit de database moet worden gebruikt |

### <a name="example"></a>Voorbeeld
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Een aanvaller mogelijk kan nu niet knoeien en wijzigen verwerkt de bewerking van de toepassing omdat de id voor het ophalen van de gegevens is serverzijde.

## <a id="enumerable-browsing"></a>Zorg ervoor dat die inhoud en resources zijn niet invoeroverzicht of toegankelijk is via geforceerde bladeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Gevoelige statische- en configuratiebestanden moeten niet worden opgeslagen in de hoofdmap van het web. Voor de inhoud is niet vereist voor het openbaar zijn, een van beide besturingselementen voor de juiste toegang moeten worden toegepast of verwijdering van de inhoud zelf.</p><p>Geforceerde bladeren wordt ook, gewoonlijk gecombineerd met Brute Force-technieken voor het verzamelen van gegevens door het openen van zo veel URL's mogelijk te inventariseren, mappen en bestanden op een server. Aanvallers kunnen controleren voor alle variaties van veel bestaande bestanden. Zoeken naar bestanden wachtwoord zou bijvoorbeeld bestanden met inbegrip van psswd.txt, password.htm password.dat en andere variaties omvatten.</p><p>Mogelijkheden voor de detectie van beveiligingsaanvallen pogingen moeten worden opgenomen als oplossing hiervoor kunt.</p>|

## <a id="privileged-server"></a>Zorg ervoor dat de minste bevoegdheden accounts worden gebruikt verbinding maken met Database-server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [SQL-Database machtigingen hiërarchie](https://msdn.microsoft.com/library/ms191465), [beveiligbare items SQL-database](https://msdn.microsoft.com/library/ms190401) |
| **Stappen** | Accounts van de minste bevoegdheden moeten worden gebruikt om verbinding met de database. Aanmelding van de toepassing moet worden beperkt in de database en moet alleen geselecteerde opgeslagen procedures worden uitgevoerd. Aanmelding van de toepassing mogen geen tabeltoegang hebben direct. |

## <a id="rls-tenants"></a>Rij niveau van beveiliging RLS om te voorkomen dat tenants toegang tot elkaars gegevens implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, on-premises |
| **Kenmerken**              | SQL-versie - V12, versie van SQL - MsSQL2016 |
| **Verwijzingen**              | [SQL Server-beveiliging voor beveiliging op rijniveau (RLS)](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Stappen** | <p>Met beveiliging op rijniveau kunnen klanten de toegang tot rijen in een databasetabel beheren op basis van de kenmerken van de gebruiker die een query uitvoert (bijvoorbeeld groepslidmaatschap of uitvoeringscontext).</p><p>Beveiliging op rijniveau (RLS) vereenvoudigt het ontwerp en de code van de beveiliging in uw toepassing. Met RLS kunt u beperkingen instellen voor de toegang tot gegevens in rijen. U kunt bijvoorbeeld bepalen dat werkrollen alleen toegang hebben tot de rijen met gegevens die relevant zijn voor hun afdeling, of de toegang van klanten beperken tot de gegevens die relevant zijn voor hun bedrijf.</p><p>De logica van de beperking van toegang is gevonden in de databaselaag in plaats van opslag van de gegevens in een andere toepassingslaag. De database-systeem is van toepassing de toegangsbeperkingen telkens wanneer die toegang tot gegevens van elke laag wordt uitgevoerd. Dit maakt het beveiligingssysteem betrouwbaarder en robuuste door te verminderen van het gebied van het beveiligingssysteem.</p><p>|

Houd er rekening mee dat beveiliging op Rijniveau als een out-of-the-box databasefunctie is alleen van toepassing op SQL-Server vanaf 2016 en Azure SQL-database. Als de out-of-the-box RLS-functie niet is geïmplementeerd, er moet voor worden gezorgd dat de gegevenstoegang beperkt met behulp van weergaven en Procedures is

## <a id="sysadmin-users"></a>Rol sysadmin mag alleen geldige noodzakelijk gebruikers hebben.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [SQL-Database machtigingen hiërarchie](https://msdn.microsoft.com/library/ms191465), [beveiligbare items SQL-database](https://msdn.microsoft.com/library/ms190401) |
| **Stappen** | Leden van de vaste serverrol SysAdmin moeten worden beperkt en bevat nooit de accounts die worden gebruikt door toepassingen.  Bekijk de lijst met gebruikers in de rol en alle onnodige accounts verwijderen|

## <a id="cloud-least-privileged"></a>Verbinding maken met Cloud-Gateway met behulp van de minste bevoegdheden tokens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-Cloud-Gateway | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Keuze van de gateway - Azure IoT Hub |
| **Verwijzingen**              | [IOT Hub-toegangsbeheer](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Stappen** | Minimale bevoegdheid machtigingen opgeven voor verschillende onderdelen die verbinding met Cloud-Gateway (IoT Hub maken). Typisch voorbeeld is – apparaat/inrichting onderdeel maakt gebruikt van registryread/schrijven, Event Processor (ASA) maakt gebruik van Service is verbonden. Afzonderlijke apparaten verbinding maken met behulp van de referenties van apparaat|

## <a id="sendonly-sas"></a>Gebruik van een verzenden alleen-lezen machtigingen SAS-sleutel voor het genereren van apparaattokens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Event-Hubs verificatie- en beveiligingsmodellen](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Een SAS-sleutel wordt gebruikt voor het genereren van afzonderlijke apparaattokens. Een alleen-verzenden machtigingen SAS-sleutel gebruiken tijdens het genereren van het apparaattoken voor een bepaalde uitgever|

## <a id="access-tokens-hub"></a>Gebruik geen toegangstokens die voorzien in directe toegang tot de Event Hub

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Event-Hubs verificatie- en beveiligingsmodellen](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Een token die directe toegang tot de event hub verleent moet niet worden toegekend aan het apparaat. Met behulp van een token van de minste bevoegdheden voor het apparaat dat toegang tot een uitgever alleen biedt te zou identificeren en op het zwarte als blijkt te zijn van een rogue of aangetast apparaat.|

## <a id="sas-minimum-permissions"></a>Verbinding maken met Event Hub met behulp van SAS-sleutels met de minimale machtigingen die vereist zijn

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Event-Hubs verificatie- en beveiligingsmodellen](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Minimale bevoegdheid machtigingen opgeven voor verschillende back-end-toepassingen die verbinding met de Event Hub maken. Afzonderlijke SAS-sleutels voor elke back-endtoepassing genereren en geef alleen de vereiste machtigingen - verzend-, ontvangen of beheren om ze te gaan.|

## <a id="resource-docdb"></a>Resource-tokens gebruiken om te verbinden met Cosmos DB indien mogelijk

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Documentdb | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Een resourcetoken is gekoppeld aan de bron van een Azure Cosmos DB machtiging en vastgelegd van de relatie tussen de gebruiker van een database en de machtiging die de gebruiker heeft voor een specifieke bron Azure Cosmos DB-toepassing (bijvoorbeeld verzameling en het document). Gebruik altijd een resourcetoken voor toegang tot de Azure Cosmos DB als de client niet kan vertrouwd worden met het verwerken van sleutels van hoofdsleutel of alleen-lezen -, zoals een eindgebruikerstoepassing is, zoals een mobiel of bureaublad-client. Gebruik de hoofdsleutel of alleen-lezensleutels van back endtoepassingen die deze sleutels veilig kunnen opslaan.|

## <a id="grained-rbac"></a>Verfijnd toegangsbeheer met RBAC met Azure-abonnement inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van Azure | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Stappen** | Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u alleen de toegangsrechten aan gebruikers verlenen die ze nodig hebben om hun taken uit te voeren.|

## <a id="cluster-rbac"></a>Van de client toegang beperken tot bewerkingen voor een cluster met behulp van RBAC

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van service Fabric | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure |
| **Verwijzingen**              | [Op rollen gebaseerd toegangsbeheer voor Service Fabric-clients](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Stappen** | <p>Azure Service Fabric ondersteunt twee verschillende typen voor clients die zijn verbonden met een Service Fabric-cluster: beheerder en gebruiker. Toegangsbeheer kan de Clusterbeheerder om te beperken van toegang tot bepaalde bewerkingen voor een cluster voor verschillende groepen gebruikers, waardoor het cluster beter te beveiligen.</p><p>Beheerders hebben volledige toegang tot de mogelijkheden voor beheer (met inbegrip van de mogelijkheden voor lezen/schrijven). Gebruikers hebben standaard alleen leestoegang tot de mogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.</p><p>U opgeven de twee clientrollen (de beheerder en client) op het moment van het maken van clusters door afzonderlijke certificaten voor elk.</p>|

## <a id="modeling-field"></a>Beveiliging-modellen uitvoeren en gebruiken van veld beveiligingsniveau vereist

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Beveiliging-modellen uitvoeren en gebruiken van veld beveiligingsniveau vereist|

## <a id="portal-security"></a>Modellering van de beveiliging van portal accounts, houd er rekening mee dat het beveiligingsmodel voor de portal wijkt af van de rest van CRM uitvoeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM-Portal | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Modellering van de beveiliging van portal accounts, houd er rekening mee dat het beveiligingsmodel voor de portal wijkt af van de rest van CRM uitvoeren|

## <a id="permission-entities"></a>Fijnmazig machtiging verlenen voor een bereik van entiteiten in Azure Table Storage

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - tabel |
| **Verwijzingen**              | [Hoe u toegang tot objecten in uw Azure storage-account via SAS delegeren](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Stappen** | In bepaalde zakelijke scenario's mogelijk Azure Table Storage worden vereist voor het opslaan van gevoelige gegevens die aan de verschillende partijen caters. Bijvoorbeeld, gevoelige gegevens die betrekking hebben op andere landen. In dergelijke gevallen kan SAS handtekeningen worden samengesteld door te geven van de partitie- en recordsleutels sleutelbereiken, zodat een gebruiker toegang heeft tot gegevens die specifiek zijn voor een bepaald land.| 

## <a id="rbac-azure-manager"></a>Rollen gebaseerd toegangsbeheer (RBAC) naar Azure storage-account met Azure Resource Manager inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Over het beveiligen van uw opslagaccount met Role-Based Access Control (RBAC)](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Stappen** | <p>Als u een nieuw opslagaccount maakt, selecteert u een van de klassieke of Azure Resource Manager-implementatiemodel. Het klassieke model voor het maken van resources in Azure kan alleen niks toegang tot het abonnement en op zijn beurt de storage-account.</p><p>Met de Azure Resource Manager-model plaatst u het opslagaccount in een resource group en beheren van toegang tot de beheerlaag van dat specifieke opslagaccount met behulp van Azure Active Directory. Bijvoorbeeld, u kunt specifieke gebruikers geven de mogelijkheid voor toegang tot de opslagaccountsleutels, terwijl andere gebruikers informatie over de storage-account bekijken kunnen, maar geen toegang de sleutels voor het opslagaccount tot.</p>|

## <a id="rooting-detection"></a>Impliciete jailbreak of detectie basismappen implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele Client | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Toepassing moet een eigen configuratie en de gebruiker gegevens in het geval als de telefoon is geroot of jailbroken beveiligen. Basismappen/belangrijke jailbroken impliceert dat onbevoegde toegang, welke normale gebruikers geen op hun eigen telefoons. Daarom moet de toepassing impliciete detectielogica hebben bij het opstarten van de toepassing, om te detecteren of de telefoon is geroot.</p><p>De logica voor het detecteren kan gewoon toegang tot bestanden die normaal gesproken alleen hoofdmap gebruiker toegang, bijvoorbeeld heeft:</p><ul><li>/System/App/SuperUser.APK</li><li>/sbin/su</li><li>/System/bin/su</li><li>/System/xbin/su</li><li>/Data/Local/xbin/su</li><li>/Data/local/bin/su</li><li>/System/SD/xbin/su</li><li>/System/bin/failsafe/su</li><li>/Data/Local/su</li></ul><p>Als de toepassing toegang heeft tot deze bestanden, geeft aan dat de toepassing wordt uitgevoerd als hoofdgebruiker.</p>|

## <a id="weak-class-wcf"></a>Zwakke Class Reference in WCF

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference) |
| **Stappen** | <p>Een klassenverwijzing zwakke, waardoor een aanvaller om uit te voeren niet-geautoriseerde code kan worden gebruikt door het systeem. Het programma verwijst naar een gebruiker gedefinieerde klasse die is niet uniek worden aangeduid. Wanneer .NET deze klasse neemt weinig geïdentificeerde laadt, zoekt het laadprogramma van het CLR-type voor de klasse in de volgende locaties in de opgegeven volgorde:</p><ol><li>Als de assembly van het type bekend is, zoekt het laadprogramma het configuratiebestand omleiden-locaties, GAC, de huidige assembly met behulp van configuratie-informatie en de basismap van de toepassing</li><li>Als de assembly onbekend is, het laadprogramma zoekt naar de huidige assembly mscorlib en de locatie die is geretourneerd door de gebeurtenis-handler TypeResolve</li><li>Deze zoekvolgorde CLR kan worden gewijzigd met haken, zoals het mechanisme voor het doorsturen van het Type en de gebeurtenis AppDomain.TypeResolve</li></ol><p>Als een aanvaller misbruik maakt van de CLR-zoekvolgorde door het maken van een andere klasse met dezelfde naam en dit plaatsen in een alternatieve locatie of de CLR wordt geladen eerst de CLR per ongeluk wordt de aanvaller geleverde code uitvoeren</p>|

### <a name="example"></a>Voorbeeld
De `<behaviorExtensions/>` element van het configuratiebestand WCF geeft WCF een klasse aangepaste gedrag toevoegen aan een bepaalde WCF-extensie.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
Met behulp van de FQDN-namen (sterke) een unieke identificeert een en verder verhoogt de beveiliging van uw systeem. Assemblynamen van de volledig gekwalificeerde gebruiken bij het registreren van typen in het machine.config en app.config-bestanden.

### <a name="example"></a>Voorbeeld
De `<behaviorExtensions/>` element van het configuratiebestand WCF geeft WCF aangepaste gedrag sterk verwezen klasse toevoegen aan een bepaalde WCF-extensie.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""Microsoft.ServiceModel.Samples.MyBehaviorSection, MyBehavior,
            Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```

## <a id="wcf-authz"></a>Besturingselement voor autorisatie van WCF-implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference) |
| **Stappen** | <p>Deze service wordt niet gebruikt voor een besturingselement voor autorisatie. Wanneer een client een bepaalde WCF-service wordt aangeroepen, biedt WCF verschillende autorisatie-schema's die controleren of de aanroeper is gemachtigd om uit te voeren van de servicemethode op de server. Als de besturingselementen voor autorisatie zijn niet ingeschakeld voor de WCF-services, kan een geverifieerde gebruiker maar liefst uitbreiding van bevoegdheden.</p>|

### <a name="example"></a>Voorbeeld
De volgende configuratie geeft WCF om te controleren op het autorisatieniveau van de client niet bij het uitvoeren van de service:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""None"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Een service-autorisatie-schema worden gebruikt om te controleren dat de oproepende functie van de servicemethode is gemachtigd om dit te doen. WCF biedt twee modi en kan de definitie van een aangepaste autorisatie-schema. De modus UseWindowsGroups maakt gebruik van Windows-rollen en gebruikers en de modus UseAspNetRoles wordt gebruikgemaakt van een provider van een ASP.NET-rol, zoals SQL Server, om te verifiëren.

### <a name="example"></a>Voorbeeld
De volgende configuratie geeft WCF om ervoor te zorgen dat de client deel van de groep Administrators uitmaakt alvorens de service toevoegen:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""UseWindowsGroups"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
De service wordt vervolgens gedeclareerd als het volgende:
```
[PrincipalPermission(SecurityAction.Demand,
Role = ""Builtin\\Administrators"")]
public double Add(double n1, double n2)
{
double result = n1 + n2;
return result;
}
```

## <a id="authz-aspnet"></a>Juiste autorisatiemechanismen in ASP.NET-Web-API implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, MVC5 |
| **Kenmerken**              | Niet van toepassing want id-Provider - ADFS, id-Provider - Azure AD |
| **Verwijzingen**              | [Verificatie en autorisatie in ASP.NET Web-API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Stappen** | <p>Informatie van rollen voor de gebruikers van de toepassing kan worden afgeleid van Azure AD of AD FS-claims als de toepassing afhankelijk is van deze als id-provider of de toepassing zelf mogelijk als er een hebt opgegeven. In al deze gevallen moet de aangepaste autorisatie-uitvoering informatie over de rol valideren.</p><p>Informatie van rollen voor de gebruikers van de toepassing kan worden afgeleid van Azure AD of AD FS-claims als de toepassing afhankelijk is van deze als id-provider of de toepassing zelf mogelijk als er een hebt opgegeven. In al deze gevallen moet de aangepaste autorisatie-uitvoering informatie over de rol valideren.</p>

### <a name="example"></a>Voorbeeld
```csharp
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
public class ApiAuthorizeAttribute : System.Web.Http.AuthorizeAttribute
{
        public async override Task OnAuthorizationAsync(HttpActionContext actionContext, CancellationToken cancellationToken)
        {
            if (actionContext == null)
            {
                throw new Exception();
            }

            if (!string.IsNullOrEmpty(base.Roles))
            {
                bool isAuthorized = ValidateRoles(actionContext);
                if (!isAuthorized)
                {
                    HandleUnauthorizedRequest(actionContext);
                }
            }

            base.OnAuthorization(actionContext);
        }

public bool ValidateRoles(actionContext)
{
   //Authorization logic here; returns true or false
}

}
```
Alle domeincontrollers en actiemethoden die moet worden beveiligd moeten worden voorzien van bovenstaande kenmerk.
```csharp
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a id="device-permission"></a>Autorisatie-controles uitvoeren in het apparaat als deze ondersteuning biedt voor verschillende acties waarvoor verschillende machtigingsniveaus

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Het apparaat moet de oproepende functie om te controleren of de aanroeper heeft de vereiste machtigingen om uit te voeren van de gevraagde actie autoriseren. Voor bijvoorbeeld kunt zeggen dat het apparaat is een Smart Lock klep die kan worden gecontroleerd vanuit de cloud, plus functies ingebouwd, zoals vergrendelen op afstand de deur biedt.</p><p>De Smart Lock van klep biedt functionaliteit voor ontgrendelen alleen als iemand anders is fysiek wordt geleverd in de buurt van de deur met een kaart. In dit geval moet de implementatie van de externe opdracht en controle worden gedaan zodanig dat deze functionaliteit biedt om de klep ontgrendelen als de cloudgateway is niet gemachtigd voor het verzenden van een opdracht voor het ontgrendelen van de deur.</p>|

## <a id="field-permission"></a>Autorisatie-controles uitvoeren in de Gateway van het veld als deze ondersteuning biedt voor verschillende acties waarvoor verschillende machtigingsniveaus

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld voor IoT-Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | De Gateway van het veld moet de oproepende functie om te controleren of de aanroeper heeft de vereiste machtigingen om uit te voeren van de gevraagde actie autoriseren. Voor bijvoorbeeld moet er verschillende machtigingen voor een beheerder gebruiker/API interface gebruikt voor het configureren van een veld gateway v/s-apparaten die verbinding mee maken.|
