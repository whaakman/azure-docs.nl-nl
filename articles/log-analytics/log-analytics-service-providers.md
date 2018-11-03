---
title: Log Analytics voor serviceproviders | Microsoft Docs
description: Log Analytics kunt Managed Service Providers (MSP's), grote ondernemingen Independent Software Vendors (ISV's) en hosting serviceproviders beheren en controleren van servers in van de klant on-premises of cloud-infrastructuur.
services: log-analytics
documentationcenter: ''
author: MeirMen
manager: jochan
editor: ''
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: meirm
ms.component: ''
ms.openlocfilehash: ef1956f5283105548ba8790b87e82c35d6c9e1cd
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960936"
---
# <a name="log-analytics-for-service-providers"></a>Log Analytics voor serviceproviders
Log Analytics kunt providers van beheerde services (MSP's), grote ondernemingen, onafhankelijke softwareleveranciers (ISV's) en hosting serviceproviders servers in van de klant on-premises of cloud-infrastructuur beheren en controleren. 

Grote ondernemingen delen veel overeenkomsten met serviceproviders, met name wanneer er een centrale IT-team dat verantwoordelijk is is voor het beheer van IT voor veel verschillende bedrijfseenheden. Voor het gemak in dit document wordt de term gebruikt *serviceprovider* maar dezelfde functionaliteit is ook beschikbaar voor ondernemingen en andere klanten.

Voor partners en leveranciers van de service die deel uitmaken van de [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programma, Log Analytics is een van de Azure-services beschikbaar zijn in [Azure CSP-abonnementen](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

## <a name="architectures-for-service-providers"></a>Architecturen voor serviceproviders

Log Analytics-werkruimten bieden een methode voor de beheerder bepalen de stroom en de isolatie van de logboeken en het maken van een architectuur met logboekbestanden die de specifieke zakelijke behoeften. [In dit artikel](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) wordt de algemene aandachtspunten rond de Werkruimtebeheer beschreven. Serviceproviders zijn aanvullende overwegingen.

Er zijn drie mogelijke architecturen voor serviceproviders met betrekking tot de Log Analytics-werkruimten:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Gedistribueerd - worden logboeken opgeslagen in werkruimten die zich in de tenant van de klant 

Een werkruimte wordt in deze architectuur wordt geïmplementeerd in de tenant van de klant die wordt gebruikt voor de logboeken van de betreffende klant. De serviceprovider-beheerders krijgen toegangsrechten voor het gebruik van deze werkruimte [gastgebruikers van Azure Active Directory (B2B)](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b). De serviceprovider-beheerders hebben om over te schakelen naar de map van de klant in Azure portal om toegang tot deze werkruimten te kunnen.

De voordelen van deze architectuur zijn:
* De klant kunt beheren de toegang tot de logboeken met behulp van hun eigen [op basis van de rol](https://docs.microsoft.com/azure/role-based-access-control/overview).
* Elke klant kan hebben verschillende instellingen voor hun werkruimte zoals bewaren en gegevens beperking.
* Isolatie tussen klanten voor regelgeving en nalevingsbeleid.
* De kosten in rekening gebracht voor elke werkruimte worden doorgevoerd in het abonnement van de klant.
* Logboeken kunnen worden verzameld vanuit allerlei typen resources, niet alleen-agent op basis van. Bijvoorbeeld: auditlogboeken van Azure.

De nadelen van deze architectuur zijn:
* Het is moeilijker om de service-provider voor het beheren van een groot aantal klanten tenants in één keer.
* Serviceprovider-beheerders hebben om te worden ingericht in de map van de klant.
* De service-provider kan geen gegevens analyseren in zijn klanten.

### <a name="2-central---logs-are-stored-in-a-workspace-located-in-the-service-provider-tenant"></a>2. Centraal - logboeken worden opgeslagen in een werkruimte zich bevindt in de tenant van serviceprovider

In deze architectuur, worden de logboeken worden niet opgeslagen in de tenants van de klant, maar alleen op een centrale locatie binnen een van de serviceprovider-abonnementen. De agents die zijn geïnstalleerd op virtuele machines van de klant worden geconfigureerd voor het verzenden van hun logboeken voor deze werkruimte met behulp van de werkruimte-ID en de geheime sleutel.

De voordelen van deze architectuur zijn:
* Het is eenvoudig voor het beheren van een groot aantal klanten en ze naar verschillende back-endsystemen integreren.
* De serviceprovider volledig eigenaar is van de logboeken en de verschillende artefacten, zoals functies en opgeslagen query's.
* De serviceprovider kan analyses uitvoeren voor alle klanten.

De nadelen van deze architectuur zijn:
* Deze architectuur is alleen van toepassing op de VM-gegevens op basis van een agent, er wordt geen betrekking op gegevensbronnen PaaS, SaaS en Azure-infrastructuur.
* Het is mogelijk moeilijk te scheiden van de gegevens tussen de klanten wanneer ze worden samengevoegd in één werkruimte. De enige methode om dit te doen is voor het gebruik van de computer volledig gekwalificeerde domeinnaam (FQDN) of via het Azure-abonnement-id. 
* Alle gegevens van alle klanten worden, opgeslagen in dezelfde regio met één factuur en dezelfde bewaartermijn en configuratie-instellingen.
* Azure-infrastructuur- en PaaS-services zoals Azure Diagnostics- en auditlogboeken van Azure moet de werkruimte zich in dezelfde tenant als de resource, dus kunnen ze de logboeken verzenden naar de centrale werkruimte.
* Alle VM-agents van alle klanten wordt geverifieerd op de cental-werkruimte met behulp van dezelfde werkruimte-ID en sleutel. Er is geen methode voor het blokkeren van Logboeken van een specifieke klant zonder te onderbreken van andere klanten.


### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Hybride - logboeken worden opgeslagen in de werkruimte zich in de tenant van de klant en sommige hiervan naar een centrale locatie worden opgehaald.

De derde architectuur combineren tussen de twee opties. Deze is gebaseerd op de eerste gedistribueerde architectuur waar de logboeken lokaal op elke klant worden, maar sommige mechanisme gebruiken voor het maken van een centrale opslagplaats voor Logboeken. Een gedeelte van de logboeken worden opgehaald in een centrale locatie voor rapportage en analyse. Dit gedeelte wordt mogelijk beperkt aantal gegevenstypen of een samenvatting van de activiteit zoals dagelijks statistieken.

Er zijn twee opties voor het implementeren van de centrale locatie in Log Analytics:

1. Centrale werkruimte: de serviceprovider kunt maken van een werkruimte in de tenant en gebruiken van een script dat gebruikmaakt van de [Query-API](https://dev.loganalytics.io/) met de [Data Collection API](log-analytics-data-collector-api.md) om de gegevens uit de verschillende werkruimten aan deze centrale locatie. Een andere optie, dan een script, is het gebruik [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).

2. Power BI als een centrale locatie: Power BI kan fungeren als de centrale locatie als de verschillende werkruimten gegevens exporteren naar met behulp van de integratie tussen Log Analytics en [Power BI](log-analytics-powerbi.md). 


## <a name="next-steps"></a>Volgende stappen
* Automatiseer het maken en de configuratie van werkruimten met behulp van [Resource Manager-sjablonen](log-analytics-template-workspace-configuration.md)
* Maken van werkruimten met behulp van automatiseren [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Gebruik [waarschuwingen](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) om te integreren met bestaande systemen
* Samenvatting rapporten genereren met [Power BI](log-analytics-powerbi.md)
* Bekijk het proces van [configureren van Log Analytics en Power BI voor het bewaken van meerdere CSP-klanten](https://docs.microsoft.com/azure/cloud-solution-provider/support/monitor-multiple-customers)
