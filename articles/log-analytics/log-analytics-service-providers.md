---
title: Log Analytics voor serviceproviders | Microsoft Docs
description: Log Analytics kunt Managed Service Providers (MSP's), grote ondernemingen Independent Software Vendors (ISV's) en hosting serviceproviders beheren en controleren van servers in van de klant on-premises of cloud-infrastructuur.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: meirm
ms.component: na
ms.openlocfilehash: 13f36f67e76b75176940a0f36121be30ba27d519
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37340862"
---
# <a name="log-analytics-features-for-service-providers"></a>Log Analytics-functies voor serviceproviders
Log Analytics kunt providers van beheerde services (MSP's), grote ondernemingen, onafhankelijke softwareleveranciers (ISV's) en hosting serviceproviders servers in van de klant on-premises of cloud-infrastructuur beheren en controleren. 

Grote ondernemingen delen veel overeenkomsten met serviceproviders, met name wanneer er een centrale IT-team dat verantwoordelijk is is voor het beheer van IT voor veel verschillende bedrijfseenheden. Voor het gemak in dit document wordt de term gebruikt *serviceprovider* maar dezelfde functionaliteit is ook beschikbaar voor ondernemingen en andere klanten.

Voor partners en leveranciers van de service die deel uitmaken van de [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programma, Log Analytics is een van de Azure-services beschikbaar zijn in [Azure CSP-abonnement](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

## <a name="architectures-for-service-providers"></a>Architecturen voor serviceproviders

Log Analytics-werkruimten bieden een methode voor de beheerder bepalen de stroom en de isolatie van de logboeken en het maken van een architectuur met logboekbestanden die de specifieke zakelijke behoeften. [In dit artikel](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-manage-access) wordt de algemene aandachtspunten rond de Werkruimtebeheer beschreven. Serviceproviders zijn aanvullende overwegingen.

Er zijn drie mogelijke architecturen voor serviceproviders met betrekking tot de Log Analytics-werkruimten:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Gedistribueerd - worden logboeken opgeslagen in werkruimten die zich in de tenant van de klant 

Werkruimte wordt in deze architectuur wordt geïmplementeerd in de tenant van de klant die wordt gebruikt voor de logboeken van de betreffende klant. De serviceprovider-beheerders krijgen toegangsrechten voor het gebruik van deze werkruimte [gastgebruikers van Azure Active Directory (B2B)](https://docs.microsoft.com/en-us/azure/active-directory/b2b/what-is-b2b). Beheerder van de serviceprovider heeft om over te schakelen in Azure portal naar de map van de klant moet toegang hebben tot deze werkruimten.

De voordelen van deze architectuur zijn:
* De klant kunt beheren de toegang tot de logboeken met behulp van hun eigen [op basis van de rol](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview).
* Elke klant kan hebben verschillende instellingen voor hun werkruimte zoals bewaren en gegevens beperking.
* Isolatie tussen klanten voor regelgeving en nalevingsbeleid.
* De kosten in rekening gebracht voor elke werkruimte worden doorgevoerd in het abonnement van de klant.
* Logboeken kunnen worden verzameld vanuit allerlei typen resources, niet alleen-agent op basis van. Bijvoorbeeld: controle van Azure.

De nadelen van deze architectuur zijn:
* Het is moeilijker om de service-provider om te groot aantal klanten tenants tegelijk beheren.
* Serviceprovider-beheerders hebben om te worden ingericht op de map van de klant.
* De service-provider kan geen gegevens analyseren in zijn klanten.

### <a name="2-central---logs-are-stored-in-workspace-located-in-the-service-provider-tenant"></a>2. Centraal - logboeken worden opgeslagen in de werkruimte zich bevindt in de tenant van serviceprovider

In deze architectuur, worden de logboeken worden niet opgeslagen in de tenants van de klant, maar alleen op een centrale locatie binnen een van de serviceprovider-abonnementen. De agents die zijn geïnstalleerd op virtuele machines van de klant worden geconfigureerd voor het verzenden van hun logboeken voor deze werkruimte met behulp van de werkruimte-ID en de geheime sleutel.

De voordelen van deze architectuur zijn:
* Het is gemakkelijk om te groot aantal klanten beheren en integreren ze naar verschillende back-endsystemen.
* De serviceprovider volledig eigenaar is van de logboeken en de verschillende artefacten, zoals functies en opgeslagen query's.
* De serviceprovider kan analyses uitvoeren voor alle klanten.

De nadelen van deze architectuur zijn:
* Het is moeilijk te scheiden van de gegevens tussen de klanten. De enige methode om dit te doen is het gebruik van de domeinnaam van de computer.
* Alle gegevens van alle klanten worden, opgeslagen in dezelfde regio met één factuur en dezelfde bewaartermijn en configuratie-instellingen.
* Azure-infrastructuur- en PaaS-services zoals Azure Diagnostics en controle van Azure moet de werkruimte zich in dezelfde tenant als de resource dus kunnen ze de logboeken verzenden naar de centrale werkruimte.
* Alle VM-agents van alle klanten wordt geverifieerd op de cental-werkruimte met behulp van dezelfde werkruimte-ID en sleutel. Er is geen methode voor het blokkeren van Logboeken van een specifieke klant zonder te onderbreken van andere klanten.


### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Hybride - logboeken worden opgeslagen in de werkruimte zich in de tenant van de klant en sommige hiervan naar een centrale locatie worden opgehaald.

De derde architectuur combineren tussen de twee opties. Deze is gebaseerd op de eerste gedistribueerde architectuur waar de logboeken lokaal op elke klant worden, maar sommige mechanisme gebruiken voor het maken van een centrale opslagplaats voor Logboeken. Een gedeelte van de logboeken worden opgehaald in een centrale locatie voor rapportage en analyse. Dit gedeelte wordt mogelijk beperkt aantal gegevenstypen of een samenvatting van de activiteit zoals dagelijkse statistiek.

Er zijn twee opties voor het implementeren van de centrale locatie in Log Analytics:

1. Centrale werkruimte: de serviceprovider kunt maken van een werkruimte in de tenant en gebruiken van een script dat gebruikmaakt van de [Query-API](https://dev.loganalytics.io/) met de [Data Collection API](log-analytics-data-collector-api.md) om de gegevens uit de verschillende werkruimten aan deze centrale locatie. Een andere optie, dan het script is met [Azure Logic App](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview).

2. Power BI als een centrale locatie: Power BI kan fungeren als de centrale locatie als de verschillende werkruimten gegevens exporteren naar met behulp van de integratie tussen Log Analytics en [Power BI](log-analytics-powerbi.md). 


## <a name="next-steps"></a>Volgende stappen
* Automatiseer het maken en de configuratie van werkruimten met behulp van [Resource Manager-sjablonen](log-analytics-template-workspace-configuration.md)
* Maken van werkruimten met behulp van automatiseren [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Gebruik [waarschuwingen](log-analytics-alerts.md) om te integreren met bestaande systemen
* Samenvatting rapporten genereren met [Power BI](log-analytics-powerbi.md)

