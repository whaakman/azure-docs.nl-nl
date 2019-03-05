---
title: Azure Monitor op Azure Stack | Microsoft Docs
description: Meer informatie over Azure Monitor op Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2018
ms.author: mabrigg
ms.lastreviewed: 12/01/2018
ms.openlocfilehash: b8df87e5d5091219b9b44a6b957250e254c97fec
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57341787"
---
# <a name="azure-monitor-on-azure-stack"></a>Azure Monitor op Azure Stack

*Van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel bevat een overzicht van de Azure Monitor-service in Azure Stack. Hierin worden de werking van Azure Monitor en aanvullende informatie over het gebruik van Azure Monitor in Azure Stack. 

Voor een introductie, overzicht en hoe u aan de slag met Azure Monitor, raadpleegt u de globale Azure-artikel [aan de slag met Azure Monitor in Azure Stack](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

![Azure Stack-Monitor-blade](./media/azure-stack-metrics-azure-data/azs-monitor.png)

Azure Monitor is de platformservice die één bron biedt voor het bewaken van Azure-resources. Met Azure Monitor kunt u visualiseren, query's uitvoeren, routeren, archiveren en anders doen op de metrische gegevens en logboeken van resources in Azure. U kunt werken met deze gegevens met behulp van de Azure Stack-beheerportal, PowerShell-Cmdlets voor Monitor, platformoverschrijdende CLI of Azure Monitor REST API's. Zie voor de specifieke connectiviteit wordt ondersteund door Azure Stack, [bewakingsgegevens van Azure Stack gebruiken](azure-stack-metrics-monitor.md)

> [!Note]  
Diagnostische logboeken en metrische gegevens zijn niet beschikbaar voor de Azure Stack Development Kit.

## <a name="prerequisites-for-azure-monitor-on-azure-stack"></a>Vereisten voor Azure Monitor op Azure Stack

Registreert de **Microsoft.insights** resourceprovider op uw abonnement aanbieding broninstellingen providers. U kunt controleren of de resourceprovider beschikbaar in uw aanbieding is gekoppeld aan uw abonnement is:

1. Open de Azure Stack-beheerportal.
2. Selecteer **biedt**.
3. Selecteer de aanbieding die zijn gekoppeld aan het abonnement.
4. Selecteer **resourceproviders** onder **instellingen.** 
5. Zoek **Microsoft.Insights** in de lijst en controleer of de status **geregistreerd.**.

## <a name="overview-of-azure-monitor-on-azure-stack"></a>Overzicht van Azure Monitor op Azure Stack

Zoals Azure Monitor op Azure biedt Azure Monitor in Azure Stack-infrastructuur op basisniveau metrische gegevens en logboeken voor de meeste services.

## <a name="azure-monitor-sources-compute-subset"></a>Azure Monitor-bronnen: compute subset

![Azure Monitor in Azure Stack gegevensbronnen - compute subset](media//azure-stack-metrics-azure-data/azs-monitor-computersubset.png)

De **Microsoft.Compute** resourceprovider in Azure Stack bevat:
 - Virtuele machines 
 - Schaalsets voor virtuele Machines

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Toepassing - Logboeken met diagnostische gegevens, toepassingslogboeken en metrische gegevens

Toepassingen kunnen worden uitgevoerd in het besturingssysteem van een virtuele machine met de **Microsoft.Compute** resourceprovider. Deze toepassingen en virtuele machines verzenden hun eigen set met Logboeken en metrische gegevens. Voor het verzamelen van de meeste metrische gegevens en logboeken op toepassingsniveau is Azure Monitor afhankelijk van de Azure-extensie voor diagnostische gegevens (Windows of Linux). 

De typen metingen zijn onder andere:
 - Prestatiemeteritems
 - Toepassingslogboeken
 - Windows-gebeurtenislogboeken
 - De bron van de .NET-gebeurtenis
 - IIS-logboeken
 - Op manifest gebaseerde ETW
 - Crashdumps
 - Klant-foutenlogboeken

> [!Note]  
> Extensie voor Linux diagnostische gegevens in Azure Stack worden niet ondersteund.

### <a name="host-and-guest-vm-metrics"></a>Metrische host- en gastgegevens

De eerder vermelde compute-resources hebben een toegewezen host-VM en het gastbesturingssysteem. De VM-host en de Gast-OS komen overeen met de basis-VM en de Gast-VM in Hyper-V-hypervisor. U kunt metrische gegevens verzamelen voor de host-VM en het gastbesturingssysteem te installeren. Bovendien kunt u logboeken met diagnostische gegevens verzamelen voor het gastbesturingssysteem te installeren. Een lijst met verzamelobjecten: metrische gegevens voor metrische gegevens voor Host en de Gast-VM in Azure Stack zijn beschikbaar op [ondersteunde metrische gegevens met Azure Monitor in Azure Stack](azure-stack-metrics-supported.md). 

### <a name="activity-log"></a>Activiteitenlogboek

U kunt de activiteitenlogboeken voor meer informatie zoeken over de compute-resources zoals gezien door de Azure Stack-infrastructuur. Het logboek bevat informatie zoals de tijden waarop resources zijn gemaakt of vernietigd. De activiteitenlogboeken in Azure Stack is consistent met Azure. Zie voor meer informatie, de beschrijving van [overzicht voor activiteit op Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). 


## <a name="azure-monitor-sources-everything-else"></a>Met Azure monitor bronnen: alles wat anders

![Azure Monitor op bronnen van Azure Stack - rest](media//azure-stack-metrics-azure-data/azs-monitor-othersubset.png)

### <a name="resources---metrics-and-diagnostics-logs"></a>Bronnen - metrische gegevens en diagnostische logboeken

Verzamelobjecten: metrische gegevens en diagnostische logboeken hangen af van het resourcetype. Een lijst met verzamelobjecten: metrische gegevens voor elke resource in Azure Stack is beschikbaar op ondersteunde metrische gegevens. Zie voor meer informatie, [ondersteunde metrische gegevens met Azure Monitor in Azure Stack](azure-stack-metrics-supported.md).

### <a name="activity-log"></a>Activiteitenlogboek

Het activiteitenlogboek is dat hetzelfde voor rekenresources. 

### <a name="uses-for-monitoring-data"></a>Wordt gebruikt voor het bewaken van gegevens

**Store- en archiveren**  

Sommige bewakingsgegevens worden al opgeslagen en blijven gedurende een bepaalde tijd beschikbaar in Azure Monitor. 
 - Metrische gegevens worden 90 dagen bewaard. 
 - Vermeldingen in het activiteitenlogboek worden 90 dagen bewaard. 
 - Logboeken met diagnostische gegevens worden niet opgeslagen.
 - De gegevens naar een opslagaccount voor langere bewaartermijn archiveren.

**Query**  

U kunt de Azure Monitor REST API, platformoverschrijdende opdrachtregelinterface (CLI)-opdrachten, PowerShell-cmdlets of de .NET SDK gebruiken voor toegang tot de gegevens in het systeem of de Azure-opslag. 

**Visualisatie**

Door de bewakingsgegevens te visualiseren in afbeeldingen en grafieken kunt u gemakkelijker en sneller trends opsporen dan wanneer u de gegevens zelf bekijkt. 

Enkele visualisatie methoden zijn:
 - Gebruik de Azure Stack-gebruiker en admin-portal
 - Gegevens routeren naar Microsoft Power BI
 - De gegevens met behulp van live streamen doorsturen naar een visualisatieprogramma van derden, of de gegevens door een hulpprogramma laten lezen uit een archief in Azure-opslag

## <a name="methods-of-accessing-azure-monitor-on-azure-stack"></a>Methoden voor het verkrijgen van toegang tot Azure bewaken in Azure Stack

In het algemeen kunt u het bijhouden, routeren en ophalen van gegevens manipuleren met een van de volgende methoden. Niet alle methoden zijn beschikbaar voor alle acties of gegevenstypen.

 - [Azure Stack Portal](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-use-portal)
 - [PowerShell](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples)
 - [Platformoverschrijdende opdrachtregel Interface(CLI)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples)
 - [REST API](https://docs.microsoft.com/rest/api/monitor)
 - [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de opties van het controleprogramma gegevensverbruik in Azure Stack in het artikel [verbruiken bewakingsgegevens van Azure Stack](azure-stack-metrics-monitor.md).
