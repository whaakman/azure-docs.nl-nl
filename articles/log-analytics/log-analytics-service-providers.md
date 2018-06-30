---
title: Meld u Analytics-functies voor serviceproviders | Microsoft Docs
description: Log Analytics kunt beheerd serviceproviders (MSPs) grote ondernemingen Independent Software Vendors (ISV's) en hosting serviceproviders beheren en bewaken van servers in een van de klant on-premises of cloudinfrastructuur.
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
ms.openlocfilehash: 73f199b83fc5fc56259a2fbbc9dce4bbd8b23998
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129726"
---
# <a name="log-analytics-features-for-service-providers"></a>Log Analytics-functies voor serviceproviders
Log Analytics kunt beheerde serviceproviders (MSPs), grote ondernemingen, onafhankelijke softwareleveranciers (ISV's) en hosting serviceproviders beheren en bewaken van servers in een van de klant on-premises of cloudinfrastructuur. 

Grote ondernemingen delen veel overeenkomsten met serviceproviders, vooral wanneer u een centrale IT-team dat verantwoordelijk is voor het beheren van IT voor veel verschillende bedrijfseenheden. Voor het gemak, dit document wordt de term *serviceprovider* maar dezelfde functionaliteit is ook beschikbaar voor ondernemingen en andere klanten.

Voor partners en leveranciers van een service die deel uitmaken van de [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programma, Log Analytics is een van de Azure-services beschikbaar zijn in [CSP Azure-abonnement](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

## <a name="architectures-for-service-providers"></a>Architecturen voor serviceproviders

Log Analytics-werkruimten bieden een methode voor de beheerder om te bepalen van de stroom en de isolatie van Logboeken en maak een logboek-architectuur die zijn gericht op de specifieke bedrijfsbehoeften. [In dit artikel](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-manage-access) wordt uitgelegd van de algemene aandachtspunten rond de werkruimte beheer. Serviceproviders zijn aanvullende overwegingen.

Er zijn drie mogelijke architecturen voor serviceproviders met betrekking tot Log Analytics-werkruimten:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Gedistribueerd - worden logboeken opgeslagen in de werkruimten die zich in de tenant van de klant 

In deze architectuur is in van de klant tenant die wordt gebruikt voor de logboeken van de betreffende klant werkruimte geïmplementeerd. De serviceprovider-beheerders krijgen toegang tot deze werkruimte met [gastgebruikers Azure Active Directory (B2B)](https://docs.microsoft.com/en-us/azure/active-directory/b2b/what-is-b2b). Beheerder van de serviceprovider wordt over te schakelen in Azure-portal naar de directory van de klant kunnen toegang krijgen tot deze werkruimten.

De voordelen van deze architectuur zijn:
* De klant kunt beheren van toegang tot de logboeken met behulp van hun eigen [toegangsgroepen op basis van](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview).
* Elke klant kan verschillende instellingen voor hun werkruimte zoals bewaren en gegevens beperking hebben.
* Isolatie tussen klanten voor regelgeving en compliancy.
* De kosten voor elke werkruimte worden doorgevoerd in het abonnement van de klant.
* Logboeken kunnen worden verzameld van alle soorten resources, niet alleen agent gebaseerde. Bijvoorbeeld: Azure controleren.

De nadelen van deze architectuur zijn:
* Is het moeilijker voor de serviceprovider groot aantal klant tenants tegelijk beheren.
* Servicebeheerders provider moeten worden ingericht op de map van de klant.
* De serviceprovider kan geen gegevens over klanten analyseren.

### <a name="2-central---logs-are-stored-in-workspace-located-in-the-service-provider-tenant"></a>2. Centraal - logboeken worden opgeslagen in de werkruimte die zich in de serviceprovider tenant

In deze architectuur, worden de logboeken worden niet opgeslagen in van de klant tenants, maar alleen in een centrale locatie in een van de serviceprovider abonnementen. De agents die zijn geïnstalleerd op virtuele machines van de klant zijn geconfigureerd voor de logboeken verzenden naar deze werkruimte met behulp van de werkruimte-ID en een geheime sleutel.

De voordelen van deze architectuur zijn:
* Het is gemakkelijk is te groot aantal klanten beheren en verschillende back-end-systemen worden geïntegreerd.
* De serviceprovider heeft volledige eigendom via de logboeken en de verschillende artefacten, zoals functies en opgeslagen query's.
* De serviceprovider kan analyses uitvoeren voor alle klanten.

De nadelen van deze architectuur zijn:
* Dit is moeilijk te scheiden van de gegevens tussen de klanten. De enige methode om dit te doen is het gebruik van de domeinnaam van de computer.
* Alle gegevens van alle klanten worden in dezelfde regio met een enkele factuur en dezelfde bewaartermijn en configuratie-instellingen opgeslagen.
* Azure-infrastructuur en PaaS-services zoals Azure Diagnostics- en Azure controle vereist de werkruimte zich in dezelfde tenant als de resource waardoor ze de logboeken naar de centrale werkruimte kunnen niet verzenden.

### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Hybride - logboeken worden opgeslagen in de werkruimte die zich in de tenant van de klant en enkele ervan naar een centrale locatie worden opgehaald.

De derde architectuur meng tussen de twee opties. Deze is gebaseerd op de eerste gedistribueerde architectuur waarin de logboeken voor elke klant lokale zijn, maar sommige mechanisme gebruiken voor het maken van een centrale opslagplaats van Logboeken. Een gedeelte van de logboeken is opgehaald in een centrale locatie voor rapportage en analyse. Dit gedeelte wordt mogelijk klein aantal gegevenstypen of een overzicht van de activiteit zoals dagelijkse statistiek.

Er zijn twee opties voor het implementeren van de centrale locatie in logboekanalyse:

1. Centrale werkruimte: de serviceprovider kunt maken van een werkruimte in de tenant en gebruiken van een script dat gebruikmaakt van de [Query API](https://dev.loganalytics.io/) met de [Data Collection API](log-analytics-data-collector-api.md) om de gegevens uit de verschillende werkruimten hierop centrale locatie. Een andere optie, dan het script is het gebruik [Azure Logic App](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview).

2. Power BI als een centrale locatie: Power BI kan fungeren als de centrale locatie wanneer de verschillende werkruimten gegevens exporteren naar met behulp van de integratie tussen Log Analytics en [Power BI](log-analytics-powerbi.md). 


## <a name="next-steps"></a>Volgende stappen
* Het maken en de configuratie van werkruimten met automatiseren [Resource Manager-sjablonen](log-analytics-template-workspace-configuration.md)
* Het maken van werkruimten met automatiseren [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Gebruik [waarschuwingen](log-analytics-alerts.md) integreren met bestaande systemen
* Samenvatting rapporten genereren met de [Power BI](log-analytics-powerbi.md)

