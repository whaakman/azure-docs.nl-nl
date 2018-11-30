---
title: Patroon voor Multitenant-Web-toepassingen | Microsoft Docs
description: Vind architectuur overzichten en ontwerppatronen die wordt beschreven hoe u een multitenant-toepassing implementeren op Azure.
services: ''
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: ''
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 342c7903e58a5c3bc41278152630187fa0c63b7b
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425037"
---
# <a name="multitenant-applications-in-azure"></a>Multitenant-toepassingen in Azure
Een multitenant-toepassing is een gedeelde resource waarmee afzonderlijke gebruikers of 'tenants,' om weer te geven van de toepassing alsof deze is hun eigen. Een typisch scenario die gepaard met een multitenant-toepassing is een waarin alle gebruikers van de toepassing desgewenst kunnen de gebruikerservaring aanpassen maar hebben verder dezelfde elementaire zakelijke vereisten. Voorbeelden van grote multitenant-toepassingen zijn Office 365, Outlook.com en visualstudio.com.

Vanuit het perspectief van de toepassingsprovider van een, van betrekking de voordelen van multitenancy voornamelijk op operationele en kosten besparen. Een versie van uw toepassing kunt voldoen aan de behoeften van veel tenants/klanten, zodat de consolidatie van systeem beheertaken, zoals controle, afstemmen van prestaties, softwareonderhoud en back-ups van gegevens.

De volgende geeft een lijst van de belangrijkste doelstellingen en vereisten vanuit het perspectief van een provider van.

* **Inrichting**: U moet kunnen zijn voor het inrichten van nieuwe tenants voor de toepassing.  Voor multitenant-toepassingen met een groot aantal tenants is het meestal nodig voor het automatiseren van dit proces door in te schakelen selfservice inrichting.
* **Onderhoud**: U moet kunnen om te upgraden van de toepassing en andere onderhoudstaken uitvoeren terwijl's hier gebruik van meerdere tenants.
* **Bewaking**: U moet kunnen om de toepassing te allen tijde om eventuele problemen te identificeren en oplossen van deze te bewaken. Dit omvat controleren hoe de toepassing op elke tenant wordt gebruikt.

Een goed geïmplementeerde multitenant-toepassing biedt de volgende voordelen voor gebruikers.

* **Isolatie**: de activiteiten van afzonderlijke tenants hebben geen invloed op het gebruik van de toepassing door andere tenants. Tenants geen toegang tot elkaars gegevens. Deze wordt weergegeven aan de tenant alsof ze exclusief gebruik van de toepassing hebben.
* **Beschikbaarheid**: afzonderlijke tenants wilt dat de toepassing voortdurend beschikbaar zijn, mogelijk met garanties die zijn gedefinieerd in een SLA. Nogmaals, de activiteiten van andere tenants geen invloed op de beschikbaarheid van de toepassing.
* **Schaalbaarheid**: de toepassing kan worden geschaald om te voldoen aan de vraag van afzonderlijke tenants. De prestaties van de toepassing moeten niet van invloed op de aanwezigheid en acties van andere tenants.
* **Kosten**: kosten zijn lager is dan het uitvoeren van een toepassing toegewezen, één tenant, omdat meerdere tenants in staat stelt het delen van resources.
* **Aanpasbaarheid**. De mogelijkheid om aan te passen van de toepassing voor een afzonderlijke tenant op verschillende manieren, zoals het toevoegen of verwijderen van functies, kleuren en logo's wijzigen of zelfs de toevoeging van hun eigen code of scripts.

Kort gezegd, terwijl er veel overwegingen waarmee u rekening houden moet om een zeer schaalbare service te bieden zijn, zijn er ook een aantal van de doelstellingen en vereisten die gemeenschappelijk voor veel toepassingen voor meerdere tenants zijn. Sommige mogelijk niet relevant voor specifieke scenario's en het belang van afzonderlijke doelstellingen en vereisten zullen variëren in elk scenario. Als aanbieder van de multitenant-toepassing, ook hebt u doelstellingen en vereisten zoals, voldoen aan de tenants doelstellingen en vereisten, winstgevendheid, facturering, meerdere serviceniveaus, inrichten, bewaking van onderhoud en automation.

Zie voor meer informatie over aanvullende ontwerpoverwegingen van een multitenant-toepassing [die als host fungeert voor een toepassing met meerdere Tenants op Azure][Hosting a Multi-Tenant Application on Azure]. Zie voor informatie over algemene gegevensarchitectuurpatronen van multitenant software as a service (SaaS)-databasetoepassingen, [Ontwerppatronen voor multitenant SaaS-toepassingen met Azure SQL Database](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure biedt veel functies waarmee u kunt de belangrijkste problemen opgetreden bij het ontwerpen van een systeem met meerdere tenants te verhelpen.

**Isolatie**

* Segment Website Tenants door Host-Headers met of zonder SSL-communicatie
* Segment Website Tenants door queryparameters
* Webservices in werkrollen
  * Werkrollen. die doorgaans gegevens verwerken op de back-end van een toepassing.
  * Webrollen die doorgaans als de front-end voor toepassingen fungeren.

**Storage**

Gegevensbeheer, zoals Azure SQL Database of Azure Storage-services, zoals de Table-service waarmee u services voor opslag van grote hoeveelheden ongestructureerde gegevens en de Blob-service waarmee u services voor het opslaan van grote hoeveelheden ongestructureerde tekst of binaire gegevens zoals video, audio en afbeeldingen.

* Het beveiligen van Multitenant-gegevens in de juiste SQL-Database per tenant SQL Server-aanmeldingen.
* Met behulp van Azure-tabellen voor de toepassing Resources door een container niveau toegangsbeleid op te geven, kunt u de mogelijkheid machtigingen aanpassen zonder om uit te geven van de nieuwe URL's voor de resources die zijn beveiligd met handtekeningen voor gedeelde toegang.
* Azure-wachtrijen voor de toepassing Resources Azure-wachtrijen worden veelvuldig gebruikt om de verwerking van station namens tenants, maar kunnen ook worden gebruikt voor het distribueren van werk dat nodig is voor het inrichten of beheren.
* Service Bus-wachtrijen voor Toepassingsresources die pushes werken aan een gedeelde service, kunt u met één wachtrij waarbij elke tenant afzender alleen machtigingen heeft (zoals hiervan is afgeleid van ACS uitgegeven claims) om te pushen naar die wachtrij terwijl alleen de ontvangers van de service de machtiging om op te halen uit de wachtrij van de gegevens die afkomstig zijn van meerdere tenants.

**Verbinding en Security-Services**

* Azure Service Bus, een infrastructuur voor berichten waarmee toepassingen toe te staan voor het uitwisselen van berichten in een licht gekoppelde manier voor betere schaalbaarheid en tolerantie kunnen uitwisselen.

**Netwerkservices**

Azure biedt verschillende netwerkservices die ondersteuning voor verificatie en de beheerbaarheid van uw gehoste toepassingen te verbeteren. Deze services omvatten het volgende:

* Met Azure Virtual Network kunt u inrichten en beheren van virtuele particuliere netwerken (VPN's) in Azure, evenals deze veilig koppelen on-premises IT-infrastructuur.
* Virtueel netwerk Traffic Manager kunt u inkomend verkeer verdelen over meerdere gehoste Azure-services of deze nu worden uitgevoerd in hetzelfde datacenter of in verschillende datacenters over de hele wereld.
* Azure Active Directory (Azure AD) is een moderne, op REST gebaseerde service die mogelijkheden voor beheer en toegangsbeheer voor uw cloudtoepassingen biedt. Met behulp van Azure AD voor Toepassingsresources Azure AD biedt een eenvoudige manier van verifiëren en autoriseren van gebruikers om toegang te krijgen tot uw web-apps en services terwijl u de functies van verificatie en autorisatie voor moet rekening worden gehouden van uw code.
* Azure Service Bus biedt een beveiligde berichten en gegevens flow-mogelijkheden voor gedistribueerd en hybride toepassingen, zoals communicatie tussen Azure gehoste toepassingen en on-premises toepassingen en services, zonder complexe firewall- en beveiliging infrastructuur. Met behulp van Service Bus-omleiding voor Toepassingsresources voor de services die worden weergegeven als eindpunten kan behoren tot de tenant (bijvoorbeeld, die buiten het systeem, bijvoorbeeld on-premises wordt gehost), of ze mogelijk services die zijn ingericht specifiek voor de tenant omdat ( vertrouwelijke, tenant-specifieke gegevens wordt verzonden via deze).

**De inrichting van Resources**

Azure biedt een aantal manieren om nieuwe tenants inrichten voor de toepassing. Voor multitenant-toepassingen met een groot aantal tenants is het meestal nodig voor het automatiseren van dit proces door in te schakelen selfservice inrichting.

* Werkrollen kunt u de inrichting en ongedaan maken inrichting per tenant resources (zoals wanneer een nieuwe tenant zich registreert of geannuleerd), verzamelen van metrische gegevens voor softwarelicentiecontrole gebruiken en beheren van schaal na een bepaalde planning of als reactie op de overschrijding van drempels van prestatie indicatoren. Deze dezelfde rol kan ook worden gebruikt om updates en upgrades van de oplossing.
* Azure-Blobs kunnen worden gebruikt voor het inrichten van compute of vooraf geïnitialiseerd storage-resources voor nieuwe tenants terwijl container toegangsniveau van de beleidsregels voor het beveiligen van de compute-service-pakketten, VHD-installatiekopieën en andere bronnen.
* Opties voor het inrichten van SQL Database-resources voor een tenant zijn onder andere:
  
  * DDL in scripts of ingesloten als resources in assembly's
  * SQL Server 2008 R2 DAC-pakketten via een programma wordt geïmplementeerd.
  * Kopiëren uit een database master-verwijzing
  * Met behulp van de database importeren en exporteren voor het inrichten van nieuwe databases uit een bestand.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: https://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: https://msdn.microsoft.com/library/windowsazure/hh689716
