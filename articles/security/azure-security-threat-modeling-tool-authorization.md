---
title: Autorisatie - hulpmiddel voor het modelleren van Microsoft Threat - Azure | Microsoft Docs
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
ms.openlocfilehash: b9ad3ceeb77a4adc2c47b262aa40a48c14423198
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="security-frame-authorization--mitigations"></a>Beveiliging Frame: Autorisatie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Een grens machine vertrouwensrelatie** | <ul><li>[Zorg ervoor dat de juiste ACL's zijn geconfigureerd voor niet-geautoriseerde toegang tot gegevens op het apparaat beperken](#acl-restricted-access)</li><li>[Zorg ervoor dat gevoelige gebruikersspecifieke toepassingsinhoud wordt opgeslagen in gebruikersprofiel directory](#sensitive-directory)</li><li>[Zorg ervoor dat de geïmplementeerde toepassingen worden uitgevoerd met de minimaal benodigde bevoegdheden](#deployed-privileges)</li></ul> |
| **Webtoepassing** | <ul><li>[Sequentiële volgorde afdwingen bij het verwerken van zakelijke logica stromen](#sequential-logic)</li><li>[Snelheidsbeperking mechanisme om te voorkomen dat opsomming implementeren](#rate-enumeration)</li><li>[Zorg ervoor dat de juiste autorisatie geïmplementeerd is en principe van minimale bevoegdheden wordt gevolgd.](#principle-least-privilege)</li><li>[Zakelijke logica en resource toegang autorisatiebeslissingen moeten niet zijn gebaseerd op parameters van de binnenkomende aanvraag](#logic-request-parameters)</li><li>[Zorg ervoor dat de inhoud en resources zijn niet inventariseerbare of toegankelijk via de geforceerde bladeren](#enumerable-browsing)</li></ul> |
| **Database** | <ul><li>[Zorg ervoor dat de minst bevoegde accounts worden gebruikt voor het verbinding maken met Database-server](#privileged-server)</li><li>[Rij Level Security-RLS om te voorkomen dat tenants toegang tot elkaars gegevens implementeren](#rls-tenants)</li><li>[De rol sysadmin mag alleen geldige nodig gebruikers hebben.](#sysadmin-users)</li></ul> |
| **IoT Cloud Gateway** | <ul><li>[Verbinding maken met de Cloudgateway minder rechten tokens gebruiken](#cloud-least-privileged)</li></ul> |
| **Azure Event Hub** | <ul><li>[Een verzenden alleen-lezen machtigingen SAS-sleutel gebruiken voor het genereren van apparaattokens](#sendonly-sas)</li><li>[Toegangstokens die directe toegang tot de Event Hub verlenen niet gebruiken](#access-tokens-hub)</li><li>[Verbinding maken met Event Hub met SAS-sleutels die de vereiste minimale machtigingen](#sas-minimum-permissions)</li></ul> |
| **Azure Document DB** | <ul><li>[Resource-tokens gebruiken voor verbinding met Azure Cosmos DB indien mogelijk](#resource-docdb)</li></ul> |
| **Vertrouwensgrenzen van Azure** | <ul><li>[Geavanceerd toegangsbeheer voor Azure-abonnement met RBAC inschakelen](#grained-rbac)</li></ul> |
| **Service Fabric-Vertrouwensgrenzen** | <ul><li>[Van de client toegang tot de bewerkingen voor een cluster met RBAC beperken](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Veld beveiligingsniveau en beveiliging modellering uitgevoerd indien vereist](#modeling-field)</li></ul> |
| **Dynamics CRM Portal** | <ul><li>[Beveiliging modellering van portal accounts houden er rekening mee dat het beveiligingsmodel voor de portal van de rest van CRM verschilt uitvoeren](#portal-security)</li></ul> |
| **Azure Storage** | <ul><li>[Fijnmazig toestemming van een bereik van entiteiten in de Azure-tabelopslag](#permission-entities)</li><li>[Op rollen gebaseerde toegangsbeheer (RBAC) naar Azure storage-account met Azure Resource Manager inschakelen](#rbac-azure-manager)</li></ul> |
| **Mobiele clients** | <ul><li>[Impliciete jailbreakdetectie of detectie basismappen implementeren](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[Van zwakke klassenverwijzing in WCF](#weak-class-wcf)</li><li>[WCF-implementeren Authorization control](#wcf-authz)</li></ul> |
| **Web-API** | <ul><li>[Juiste autorisatiemechanismen in ASP.NET Web API implementeren](#authz-aspnet)</li></ul> |
| **IoT-apparaat** | <ul><li>[Autorisatie controles uitvoeren in het apparaat als deze ondersteuning biedt voor verschillende acties die verschillende machtigingsniveaus vereisen](#device-permission)</li></ul> |
| **IoT Field Gateway** | <ul><li>[Autorisatie controles uitvoeren in de Gateway van het veld als deze ondersteuning biedt voor verschillende acties die verschillende machtigingsniveaus vereisen](#field-permission)</li></ul> |

## <a id="acl-restricted-access"></a>Zorg ervoor dat de juiste ACL's zijn geconfigureerd voor niet-geautoriseerde toegang tot gegevens op het apparaat beperken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Een grens machine vertrouwensrelatie | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat de juiste ACL's zijn geconfigureerd voor niet-geautoriseerde toegang tot gegevens op het apparaat beperken|

## <a id="sensitive-directory"></a>Zorg ervoor dat gevoelige gebruikersspecifieke toepassingsinhoud wordt opgeslagen in gebruikersprofiel directory

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Een grens machine vertrouwensrelatie | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat gevoelige gebruikersspecifieke toepassingsinhoud wordt opgeslagen in gebruikersprofiel directory. Dit is om te voorkomen dat meerdere gebruikers van de machine toegang tot elkaars gegevens.|

## <a id="deployed-privileges"></a>Zorg ervoor dat de geïmplementeerde toepassingen worden uitgevoerd met de minimaal benodigde bevoegdheden

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Een grens machine vertrouwensrelatie | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat de gedistribueerde toepassing wordt uitgevoerd met de minimaal benodigde bevoegdheden. |

## <a id="sequential-logic"></a>Sequentiële volgorde afdwingen bij het verwerken van zakelijke logica stromen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Om te controleren of deze fase via werd uitgevoerd door een legitieme gebruiker die u wilt afdwingen van de toepassing alleen verwerken zakelijke logica stromen in opeenvolgende volgorde, met alle stappen in de realistische menselijke tijd wordt verwerkt en niet verwerken volgorde, overgeslagen stappen verwerkte stappen van een andere gebruiker of transacties te snel worden verzonden.|

## <a id="rate-enumeration"></a>Snelheidsbeperking mechanisme om te voorkomen dat opsomming implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat gevoelige id willekeurige's. Controle van de CAPTCHA op anonieme's implementeren. Zorg ervoor dat fout en uitzondering niet specifieke gegevens bekendmaken moeten|

## <a id="principle-least-privilege"></a>Zorg ervoor dat de juiste autorisatie geïmplementeerd is en principe van minimale bevoegdheden wordt gevolgd.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Het principe betekent dat een gebruikersaccount geeft alleen de bevoegdheden die essentieel zijn voor dat gebruikers werken. Bijvoorbeeld, een back-gebruiker niet hoeft te installeren: daarom de back-gebruiker beschikt over rechten voor het uitvoeren van back-ups en back-up-gerelateerde toepassingen. Andere bevoegdheden, zoals het installeren van nieuwe software, worden geblokkeerd. Het principe geldt ook voor een pc-gebruiker die meestal werkt in een normaal gebruikersaccount en opent een account met machtigingen, een wachtwoord beveiligd (dat wil zeggen, een superuser) alleen wanneer de situatie absoluut vraagt. </p><p>Dit principe kan ook worden toegepast op uw webtoepassingen. In plaats van alleen afhankelijk van de op rollen gebaseerde verificatiemethoden met behulp van sessies, in plaats daarvan willen we rechten toewijzen aan gebruikers met behulp van een systeem met verificatie op basis van de Database. We nog steeds sessies gebruiken om u te identificeren als de gebruiker is aangemeld correct nu alleen in plaats van die gebruiker met een specifieke rol die we toe te wijzen met bevoegdheden om te controleren welke acties die hij bepaalde bevoegdheden heeft uit te voeren op het systeem toewijzen. Een groot pro van deze methode is ook, wanneer een gebruiker heeft minder bevoegdheden die uw wijzigingen worden toegepast op elk gewenst moment omdat het toewijzen is niet afhankelijk van de sessie die anders eerst vervallen moest worden toegewezen.</p>|

## <a id="logic-request-parameters"></a>Zakelijke logica en resource toegang autorisatiebeslissingen moeten niet zijn gebaseerd op parameters van de binnenkomende aanvraag

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Wanneer u controleert of een gebruiker beperkt is tot bepaalde gegevens bekijken, moet de toegangsbeperkingen verwerkte serverzijde. De gebruikers-id moet worden opgeslagen in een sessievariabele aanmelden en moet worden gebruikt voor het ophalen van gebruikersgegevens uit de database |

### <a name="example"></a>Voorbeeld
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Een aanvaller mogelijk kan nu niet kunnen knoeien en wijzig verwerkt de bewerking van de toepassing omdat de id voor het ophalen van de gegevens is serverzijde.

## <a id="enumerable-browsing"></a>Zorg ervoor dat de inhoud en resources zijn niet inventariseerbare of toegankelijk via de geforceerde bladeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Gevoelige statisch en configuratie-bestanden moeten niet worden opgeslagen in de hoofdmap van het web. Voor de inhoud niet als public is vereist, de juiste toegangscontroles moeten worden toegepast of het verwijderen van de inhoud zelf.</p><p>Geforceerde bladeren wordt ook meestal gecombineerd met Brute kracht technieken gegevens verzamelen door het openen van zoveel URL's mogelijk bij het opsommen van mappen en bestanden op een server. Aanvallers kunnen controleren voor alle variaties van vaak bestaande bestanden. Een zoekbewerking wachtwoord zou bijvoorbeeld bestanden met inbegrip van psswd.txt, password.htm password.dat en andere variaties omvatten.</p><p>Mogelijkheden voor de detectie van beveiligingsaanvallen pogingen moeten worden opgenomen als oplossing hiervoor kunt.</p>|

## <a id="privileged-server"></a>Zorg ervoor dat de minst bevoegde accounts worden gebruikt voor het verbinding maken met Database-server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [SQL-Database machtigingen hiërarchie](https://msdn.microsoft.com/library/ms191465), [securables voor SQL-database](https://msdn.microsoft.com/library/ms190401) |
| **Stappen** | Minst bevoegde accounts moeten worden gebruikt voor verbinding met de database. Aanmelding van de toepassing moet worden beperkt in de database en moet alleen geselecteerde opgeslagen procedures worden uitgevoerd. Aanmelding van de toepassing moet hebben geen directe tabeltoegang. |

## <a id="rls-tenants"></a>Rij Level Security-RLS om te voorkomen dat tenants toegang tot elkaars gegevens implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, OnPrem |
| **Kenmerken**              | SQL-versie - V12, SQL-versie - MsSQL2016 |
| **Verwijzingen**              | [SQL Server Row-Level Security (RLS)](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Stappen** | <p>Met beveiliging op rijniveau kunnen klanten de toegang tot rijen in een databasetabel beheren op basis van de kenmerken van de gebruiker die een query uitvoert (bijvoorbeeld groepslidmaatschap of uitvoeringscontext).</p><p>Rij-Level Security (RLS) vereenvoudigt het ontwerp en het coderen van beveiliging in uw toepassing. Met RLS kunt u beperkingen instellen voor de toegang tot gegevens in rijen. U kunt bijvoorbeeld bepalen dat werkrollen alleen toegang hebben tot de rijen met gegevens die relevant zijn voor hun afdeling, of de toegang van klanten beperken tot de gegevens die relevant zijn voor hun bedrijf.</p><p>De beperking toegang logica is gevonden in de databaselaag in plaats van opslag van de gegevens in een andere toepassingslaag. Elke keer dat de toegang tot die gegevens van elke categorie wordt uitgevoerd, past het databasesysteem de toegangsbeperkingen. Dit maakt het beveiligingssysteem betrouwbaarder en robuuste doordat het oppervlak van het beveiligingssysteem.</p><p>|

Houd er rekening mee dat RLS als een out-of-the-box databasefunctie is alleen van toepassing op SQL-Server vanaf 2016 en Azure SQL database. Als de out-of-the-box-RLS-functie niet is geïmplementeerd, er moet voor worden gezorgd dat de gegevenstoegang beperkt met behulp van weergaven en Procedures is

## <a id="sysadmin-users"></a>De rol sysadmin mag alleen geldige nodig gebruikers hebben.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [SQL-Database machtigingen hiërarchie](https://msdn.microsoft.com/library/ms191465), [securables voor SQL-database](https://msdn.microsoft.com/library/ms190401) |
| **Stappen** | Leden van de vaste serverrol sysadmin moeten zeer beperkt en nooit accounts die worden gebruikt door toepassingen bevatten.  Bekijk de lijst met gebruikers in de rol en verwijdert u eventuele overbodige accounts|

## <a id="cloud-least-privileged"></a>Verbinding maken met de Cloudgateway minder rechten tokens gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT Cloud Gateway | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Keuze van de gateway - Azure IoT Hub |
| **Verwijzingen**              | [Toegangsbeheer IOT-Hub](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Stappen** | Minimale bevoegdheid machtigingen opgeven voor verschillende onderdelen die verbinding met Cloud-Gateway (IoT Hub maken). Typisch voorbeeld is: apparaat/inrichting component registryread schrijftijd gebruikt, gebeurtenis-Processor (ASA) maakt gebruik van Service verbinding maken. Afzonderlijke apparaten verbinding maken met de referenties van het apparaat|

## <a id="sendonly-sas"></a>Een verzenden alleen-lezen machtigingen SAS-sleutel gebruiken voor het genereren van apparaattokens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Verificatie en beveiliging model overzicht van Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Een SAS-sleutel wordt gebruikt voor het genereren van afzonderlijke apparaattokens. Een SAS-sleutel verzenden alleen-lezen machtigingen gebruiken tijdens het genereren van het apparaattoken voor een opgegeven uitgever|

## <a id="access-tokens-hub"></a>Toegangstokens die directe toegang tot de Event Hub verlenen niet gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Verificatie en beveiliging model overzicht van Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Een token die directe toegang tot de event hub verleent mogen niet worden toegekend aan het apparaat. Met behulp van een minstens een token voor het apparaat dat toegang tot een uitgever alleen geeft te zou identificeren en afgekeurde deze als een rogue worden gevonden of aangetast apparaat.|

## <a id="sas-minimum-permissions"></a>Verbinding maken met Event Hub met SAS-sleutels die de vereiste minimale machtigingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Verificatie en beveiliging model overzicht van Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Minimale bevoegdheid machtigingen opgeven voor verschillende back-end-toepassingen die verbinding met de Event Hub maken. Afzonderlijke SAS-sleutels voor elke back-endtoepassing genereren en leveren alleen de vereiste machtigingen - verzenden, ontvangen of beheren toe.|

## <a id="resource-docdb"></a>Resource-tokens gebruiken voor verbinding met Cosmos DB indien mogelijk

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Document DB | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Een resource-token is gekoppeld aan een resource van de machtiging Azure Cosmos DB en vastlegt de relatie tussen de gebruiker van een database en de machtiging die de gebruiker heeft voor een specifieke bron Azure DB die Cosmos-toepassing (bijvoorbeeld verzameling en het document). Gebruik altijd het token van een resource voor toegang tot de database van de Cosmos Azure als de client niet kan vertrouwd worden met de verwerking van sleutels in master of alleen-lezen - als een eindgebruikerstoepassing zoals een mobiele of bureaubladtoepassingen client. Gebruik hoofdsleutel of alleen-lezen sleutels van de back-end-toepassingen die deze sleutels veilig kunnen opslaan.|

## <a id="grained-rbac"></a>Geavanceerd toegangsbeheer voor Azure-abonnement met RBAC inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Vertrouwensgrenzen van Azure | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Stappen** | Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u alleen de toegangsrechten aan gebruikers verlenen die ze nodig hebben om hun taken uit te voeren.|

## <a id="cluster-rbac"></a>Van de client toegang tot de bewerkingen voor een cluster met RBAC beperken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Service Fabric-Vertrouwensgrenzen | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Omgeving - Azure |
| **Verwijzingen**              | [Toegangsbeheer op basis van rollen voor Service Fabric-clients](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Stappen** | <p>Azure Service Fabric ondersteunt twee verschillende toegangsrechten besturingselementtypen voor clients die zijn verbonden met een Service Fabric-cluster: beheerder en gebruiker. Toegangsbeheer kan de Clusterbeheerder om te beperken van toegang tot bepaalde clusterbewerkingen voor verschillende groepen gebruikers, zodat het cluster beter te beveiligen.</p><p>Beheerders hebben volledige toegang tot de beheerfuncties (inclusief lezen/schrijven-mogelijkheden). Gebruikers hebben standaard alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.</p><p>U opgeven de twee client-rollen (administrator en client) op het moment van het maken van het cluster door afzonderlijke certificaten voor elk.</p>|

## <a id="modeling-field"></a>Veld beveiligingsniveau en beveiliging modellering uitgevoerd indien vereist

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Veld beveiligingsniveau en beveiliging modellering uitgevoerd indien vereist|

## <a id="portal-security"></a>Beveiliging modellering van portal accounts houden er rekening mee dat het beveiligingsmodel voor de portal van de rest van CRM verschilt uitvoeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM-Portal | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Beveiliging modellering van portal accounts houden er rekening mee dat het beveiligingsmodel voor de portal van de rest van CRM verschilt uitvoeren|

## <a id="permission-entities"></a>Fijnmazig toestemming van een bereik van entiteiten in de Azure-tabelopslag

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - tabel |
| **Verwijzingen**              | [Het overdragen van toegang tot objecten in uw Azure storage-account via SAS](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Stappen** | In bepaalde gevallen bedrijven, kunnen Azure Table Storage worden vereist voor het opslaan van gevoelige gegevens die aan andere partijen caters. Bijvoorbeeld gevoelige gegevens die betrekking hebben op andere landen. In dergelijke gevallen kan SAS handtekeningen worden samengesteld door te geven van de partitie- en rijkoppen sleutelbereiken, zodat een gebruiker toegang de gegevens die specifiek zijn voor een bepaald land tot.| 

## <a id="rbac-azure-manager"></a>Op rollen gebaseerde toegangsbeheer (RBAC) naar Azure storage-account met Azure Resource Manager inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Het beveiligen van uw opslagaccount met op rollen gebaseerde toegangsbeheer (RBAC)](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Stappen** | <p>Als u een nieuw opslagaccount maakt, selecteert u een van de klassieke weergave of Azure Resource Manager-implementatiemodel. Het klassieke model van het maken van resources in Azure kunt alleen ofwel volledig, ofwel toegang tot het abonnement, en daarmee ook de storage-account.</p><p>Met het Azure Resource Manager-model plaatst u het opslagaccount in een resource group en beheren van toegang op het beheervlak van het specifieke storage-account met Azure Active Directory. Bijvoorbeeld, kunt u specifieke gebruikers de mogelijkheid bieden toegang tot de opslagaccountsleutels, terwijl andere gebruikers informatie over het opslagaccount kunnen bekijken, maar heeft geen toegang de opslagaccountsleutels tot.</p>|

## <a id="rooting-detection"></a>Impliciete jailbreakdetectie of detectie basismappen implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele clients | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Toepassing moet een eigen configuratie- en gebruikersbestanden gegevens in de aanvraag als telefoon is geroot of heeft een jailbreak beveiligt. Basismappen/geroot op te splitsen impliceert onbevoegde toegang, welke normale gebruikers niet op hun eigen telefoons. Daarom moet toepassing impliciete detectielogica hebben voor opstarten van de toepassing, om te detecteren of de telefoon is geroot.</p><p>De detectielogica kan eenvoudig toegang krijgen tot bestanden die normaal gesproken alleen basis-gebruiker toegang, bijvoorbeeld heeft:</p><ul><li>/System/App/SuperUser.APK</li><li>/sbin/su</li><li>/System/bin/su</li><li>/System/xbin/su</li><li>/Data/Local/xbin/su</li><li>/Data/local/bin/su</li><li>/system/sd/xbin/su</li><li>/System/bin/failsafe/su</li><li>/Data/Local/su</li></ul><p>Als de toepassing toegang heeft tot deze bestanden, geeft u aan dat de toepassing wordt uitgevoerd als hoofdgebruiker.</p>|

## <a id="weak-class-wcf"></a>Van zwakke klassenverwijzing in WCF

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | <p>Een verwijzing zwakke klasse, waardoor een aanvaller kan uitvoeren van niet-geautoriseerde code wordt gebruikt door het systeem. Het programma verwijst naar een gebruiker gedefinieerde klasse die is niet uniek geïdentificeerd. Wanneer .NET wordt geladen voor deze licht geïdentificeerde klasse, zoekt de lader van de CLR-type voor de klasse in de volgende locaties in de opgegeven volgorde:</p><ol><li>Als de assembly van het type bekend is, zoekt de lader van de locaties in het configuratiebestand omleiding, GAC. de huidige assembly met configuratie-informatie en de basismap van de toepassing</li><li>Als de assembly onbekend is, zoekt de lader van de huidige assembly van mscorlib en de locatie die is geretourneerd door de gebeurtenis-handler TypeResolve</li><li>Deze zoekvolgorde CLR kan worden gewijzigd met hooks zoals het mechanisme voor het doorsturen van het Type en de gebeurtenis AppDomain.TypeResolve</li></ol><p>Als een hacker de CLR-zoekvolgorde door het maken van een andere klasse met dezelfde naam en het onbedoeld plaatsen hiervan op een andere locatie of de CLR wordt geladen eerst de CLR wordt de code aanvaller opgegeven uitvoeren</p>|

### <a name="example"></a>Voorbeeld
De `<behaviorExtensions/>` element van het configuratiebestand WCF Hiermee geeft u WCF een aangepaste gedrag klasse toevoegen aan een bepaalde WCF-extensie.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
Met behulp van de FQDN-namen (sterk) unieke wijze identificeert een en verhoogt bovendien beveiliging van uw systeem. Volledig gekwalificeerde assembly-namen gebruiken bij het registreren van typen in het machine.config- en app.config-bestanden.

### <a name="example"></a>Voorbeeld
De `<behaviorExtensions/>` element van het configuratiebestand WCF Hiermee geeft u WCF aangepaste gedrag sterk verwezen klasse toevoegen aan een bepaalde WCF-extensie.
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

## <a id="wcf-authz"></a>WCF-implementeren Authorization control

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | <p>Deze service wordt niet gebruikt voor een besturingselement voor autorisatie. Wanneer een client een bepaalde WCF-service wordt aangeroepen, biedt WCF verschillende autorisatie-schema's die controleren dat de aanroeper toegangsrechten heeft voor de servicemethode niet uitvoeren op de server. Als autorisatie besturingselementen zijn niet ingeschakeld voor de WCF-services, kunt een geverifieerde gebruiker uitbreiding van bevoegdheden te bereiken.</p>|

### <a name="example"></a>Voorbeeld
De volgende configuratie wordt WCF Autorisatieniveau voor de client niet moet controleren bij het uitvoeren van de service:
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
Gebruik een schema van de autorisatie service om te controleren dat de aanroeper van de methode is gemachtigd om dit te doen. WCF biedt twee modi en kan de definitie van een aangepaste autorisatie-schema. De modus UseWindowsGroups Windows-rollen en voor gebruikers en de modus UseAspNetRoles gebruikt een ASP.NET-rolprovider, zoals SQL Server om te verifiëren.

### <a name="example"></a>Voorbeeld
De volgende configuratie wordt WCF om ervoor te zorgen dat de client deel van de groep Administrators uitmaakt voor het uitvoeren van de service toevoegen:
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

## <a id="authz-aspnet"></a>Juiste autorisatiemechanismen in ASP.NET Web API implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, MVC5 |
| **Kenmerken**              | N.V.T. identiteitsprovider Provider - ADFS, Identity - Azure AD |
| **Verwijzingen**              | [Verificatie en autorisatie in ASP.NET Web-API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Stappen** | <p>Rolgegevens voor de gebruikers van toepassingen kan worden afgeleid van Azure AD of AD FS-claims als de toepassing afhankelijk van deze als id-provider is of de toepassing zelf kan het opgegeven. In al deze gevallen moet de aangepaste autorisatie-uitvoering de gebruikersgegevens van de rol valideren.</p><p>Rolgegevens voor de gebruikers van toepassingen kan worden afgeleid van Azure AD of AD FS-claims als de toepassing afhankelijk van deze als id-provider is of de toepassing zelf kan het opgegeven. In al deze gevallen moet de aangepaste autorisatie-uitvoering de gebruikersgegevens van de rol valideren.</p>

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
Alle domeincontrollers en actiemethoden dat vereist is voor beveiligde moeten gedecoreerd worden met hierboven kenmerk.
```csharp
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a id="device-permission"></a>Autorisatie controles uitvoeren in het apparaat als deze ondersteuning biedt voor verschillende acties die verschillende machtigingsniveaus vereisen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Het apparaat moet de aanroeper om te controleren of de aanroeper heeft de vereiste machtigingen om uit te voeren van de gevraagde actie te autoriseren. Voor bijvoorbeeld kunnen aannemen dat het apparaat is een Smart Lock deur die kan worden gecontroleerd vanuit de cloud, plus het functies zoals het vergrendelen op afstand de deur biedt.</p><p>De Smart Lock van klep biedt ontgrendelen functionaliteit, alleen wanneer iemand is fysiek wordt geleverd bij de deur met een kaart. In dit geval moet de implementatie van de externe opdracht en controle in zodanig dat deze niet functionaliteit om te ontgrendelen de deur biedt als de cloudgateway is niet geautoriseerd voor het verzenden van een opdracht voor het ontgrendelen van de deur worden uitgevoerd.</p>|

## <a id="field-permission"></a>Autorisatie controles uitvoeren in de Gateway van het veld als deze ondersteuning biedt voor verschillende acties die verschillende machtigingsniveaus vereisen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld IoT Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | De Gateway van het veld moet de aanroeper om te controleren of de aanroeper heeft de vereiste machtigingen om uit te voeren van de gevraagde actie te autoriseren. Voor bijvoorbeeld moet er andere machtigingen voor een beheerder gebruiker/API interface gebruikt voor het configureren van een veld gateway v/s-apparaten die verbinding maken.|
