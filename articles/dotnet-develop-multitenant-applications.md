---
title: Multitenant Web Application patroon | Microsoft Docs
description: Architectuur overzichten en ontwerppatronen die wordt beschreven hoe u een multitenant-webtoepassing implementeren op Azure vinden.
services: 
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: 
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 57ba0e46139bda2d74c9f7db0ffab2f2122b0df2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="multitenant-applications-in-azure"></a>Multitenant-toepassingen in Azure
Een multitenant-toepassing is een gedeelde bron waarmee afzonderlijke gebruikers of 'tenants' om weer te geven van de toepassing, alsof het was hun eigen. Een typisch scenario die gepaard met een multitenant-toepassing is een waarin alle gebruikers van de toepassing wilt aanpassen van de gebruikerservaring maar anders hebben dezelfde elementaire zakelijke vereisten. Voorbeelden van grote multitenant toepassingen zijn Office 365, Outlook.com en visualstudio.com.

Vanuit het perspectief van de toepassingsprovider van een, is de voordelen van multitenancy voornamelijk betrekking hebben op de operationele en kosten-efficiëntie. Een versie van uw toepassing kunt voldoen aan de behoeften van veel tenants/klanten, zodat de consolidatie van systeem beheertaken, zoals controle, prestaties afstemmen, onderhoud en gegevensback-ups.

Hieronder vindt u een lijst van de belangrijkste doelstellingen en vereisten uit het perspectief van de provider.

* **Inrichting**: U moet zich kunnen voor het inrichten van nieuwe tenants voor de toepassing.  Voor multitenant-toepassingen met een groot aantal tenants is het normaal gesproken moet u dit proces ook automatiseren door in te schakelen selfservice inrichting.
* **Onderhoud**: U moet zich kunnen upgraden van de toepassing en andere onderhoudstaken uitvoeren terwijl's hier gebruik van meerdere tenants.
* **Bewaking**: U moet kunnen om de toepassing te allen tijde problemen identificeren en oplossen van de fouten te bewaken. Dit omvat bewaking hoe elke tenant wordt gebruik van de toepassing.

Een goed geïmplementeerde multitenant toepassing biedt de volgende voordelen voor gebruikers.

* **Isolatie**: de activiteiten van individuele tenants hebben geen invloed op het gebruik van de toepassing van andere tenants. Tenants geen toegang tot elkaars gegevens. Deze wordt weergegeven aan de tenant, alsof er exclusief gebruik van de toepassing.
* **Beschikbaarheid**: individuele tenants wilt dat de toepassing mogelijk voortdurend beschikbaar, worden gedefinieerd in een SLA garanties. De activiteiten van andere tenants moeten ook geen invloed op de beschikbaarheid van de toepassing.
* **Schaalbaarheid**: de toepassing wordt aangepast aan het voldoen aan de vraag van individuele tenants. De prestaties van de toepassing moeten niet van invloed op de aanwezigheid en acties van andere tenants.
* **Kosten**: kosten zijn lager is dan het uitvoeren van een toepassing speciale, één tenant omdat multi-tenancymodus in staat stelt het delen van bronnen.
* **Aanpassingsmogelijkheden**. De mogelijkheid voor het aanpassen van de toepassing voor een afzonderlijke tenant op verschillende manieren zoals toevoegen of verwijderen van functies, kleuren en logo's wijzigen of zelfs de toevoeging van hun eigen code of script.

Kort gezegd: Er zijn een aantal zaken die u in aanmerking nemen moet om een zeer schaalbare service te bieden, er zijn ook een aantal van de doelstellingen en vereisten die gemeenschappelijk voor veel multitenant-toepassingen zijn. Sommige is mogelijk niet relevant zijn in bepaalde scenario's en het belang van afzonderlijke doelstellingen en vereisten zullen verschillen in elk scenario. Als een provider van de toepassing multitenant ook hebt u doelen en vereisten zoals, voldoen aan de tenants doelstellingen en vereisten, winstgevendheid, facturering, meerdere serviceniveaus, inrichting, bewaking van onderhoud en automatisering.

Zie voor meer informatie over aanvullende ontwerpoverwegingen van een toepassing met multitenant [die als host fungeert voor een Multitenant-toepassing in Azure][Hosting a Multi-Tenant Application on Azure]. Zie voor informatie over algemene gegevensarchitectuurpatronen van multitenant software as a service (SaaS)-databasetoepassingen, [Ontwerppatronen voor multitenant SaaS-toepassingen met Azure SQL Database](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure biedt een groot aantal functies waarmee u voor het oplossen van de belangrijkste problemen aangetroffen bij het ontwerpen van een multitenant-systeem.

**Isolatie**

* Tenants segment Website door hostheaders met of zonder SSL-communicatie
* Tenants segment Website door queryparameters
* Web-Services werkrollen
  * Werkrollen. die gegevens op de back-end van een toepassing doorgaans niet verwerken.
  * Web-functies die gewoonlijk als de frontend voor toepassingen optreden.

**Storage**

Gegevensbeheer zoals Azure SQL Database- of Azure Storage-services zoals de tabel-service die services voor de opslag van grote hoeveelheden ongestructureerde gegevens biedt en de Blob-service waarmee services voor het opslaan van grote hoeveelheden ongestructureerde tekst of binaire gegevens zoals video, audio en afbeeldingen.

* Het beveiligen van Multitenant-gegevens in de juiste SQL-Database per-tenant SQL Server-aanmeldingen.
* Met Azure-tabellen voor de toepassing Resources door te geven een container niveau toegangsbeleid kunt u de mogelijkheid machtigingen aanpassen zonder nieuwe URL's voor de resources die zijn beveiligd met handtekeningen voor gedeelde toegang verlenen.
* Azure wachtrijen voor toepassing Resources Azure wachtrijen wordt vaak gebruikt op de verwerking van het station namens tenants, maar kunnen ook worden gebruikt voor het werk dat nodig is voor het inrichten of management distribueren.
* Service Bus-wachtrijen voor toepassingsbronnen die pushes naar een gedeelde werken met een service, kunt u een enkele wachtrij waarbij elke tenant afzender alleen machtigingen heeft (zoals hiervan is afgeleid van ACS uitgegeven claims) te drukken naar die wachtrij terwijl u alleen de ontvangers van de service machtiging voor het ophalen van gegevens die afkomstig zijn van meerdere tenants uit de wachtrij.

**Verbinding en beveiligingsservices**

* Azure Service Bus een messaging-infrastructuur die tussen toepassingen zodat ze voor het uitwisselen van berichten in een losse manier voor verbeterde schaal en tolerantie.

**Netwerkservices**

Azure biedt verschillende netwerkservices die ondersteuning bieden voor verificatie en de beheerbaarheid van uw gehoste toepassingen. Deze services omvatten het volgende:

* Azure Virtual Network-Hiermee kunt u inrichten en beheren van virtuele particuliere netwerken (VPN's) in Azure, alsmede veilig koppelen aan alle lokale IT-infrastructuur.
* Virtueel netwerk Traffic Manager kunt u saldo binnenkomend verkeer op meerdere gehoste Azure services laden of ze in hetzelfde datacenter of in verschillende datacenters over de hele wereld uitvoert.
* Azure Active Directory (Azure AD) is een moderne, REST gebaseerde service die mogelijkheden voor beheer en toegang beheren voor uw cloudtoepassingen biedt. Met behulp van Azure AD voor toepassingsbronnen Azure AD biedt een eenvoudige manier voor verificatie en autoriseren van gebruikers om toegang te krijgen tot uw webtoepassingen en services terwijl u de functies van verificatie en autorisatie voor moet rekening worden gehouden van uw code.
* Azure Service Bus biedt een beveiligde berichten en gegevensstroom mogelijkheid voor gedistribueerd en hybride toepassingen, zoals communicatie tussen Azure gehoste toepassingen en on-premises toepassingen en services, zonder complexe firewall en beveiliging infrastructuren. Met behulp van Service Bus Relay tot de services die beschikbaar worden gesteld als eindpunten voor toepassingsbronnen kan behoren tot de tenant (bijvoorbeeld buiten het systeem, bijvoorbeeld on-premises gehost) of ze kunnen services die ingericht zijn specifiek voor de tenant omdat (zijn gevoelige, tenant-specifieke gegevens passeert ze).

**Inrichting van Resources**

Azure biedt een aantal manieren inrichten van nieuwe tenants voor de toepassing. Voor multitenant-toepassingen met een groot aantal tenants is het normaal gesproken moet u dit proces ook automatiseren door in te schakelen selfservice inrichting.

* Werkrollen kunt u toestaan om te leveren en intrekken per tenant resources (zoals wanneer een nieuwe tenant tekenen-up of geannuleerd), verzamelen van meetgegevens over voor softwarelicentiecontrole gebruiken en beheren van de schaal na een bepaalde planning of in reactie op het overschrijden van de drempelwaarden van prestatie indicatoren. Deze dezelfde rol kan ook worden gebruikt voor de push-updates en upgrades van de oplossing.
* Azure BLOB's kunnen worden gebruikt voor het inrichten van compute of vooraf worden geïnitialiseerd opslagbronnen voor nieuwe tenants en container niveau toegangsbeleid tegelijkertijd beveiligen van de compute-service-pakketten, VHD-installatiekopieën en andere resources.
* Opties voor het inrichten van SQL Database-resources voor een tenant zijn onder andere:
  
  * DDL in scripts of ingesloten als bronnen binnen de assembly's
  * SQL Server 2008 R2 DAC-pakketten via een programma wordt geïmplementeerd.
  * Kopiëren van een database master verwijzing
  * Met behulp van de database importeren en exporteren voor het inrichten van nieuwe databases uit een bestand.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
