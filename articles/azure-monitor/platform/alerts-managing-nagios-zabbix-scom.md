---
title: Waarschuwingen beheren van SCOM, Zabbix en Nagios in Azure Monitor
description: Waarschuwingen beheren van SCOM, Zabbix en Nagios in Azure Monitor
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: 48fb9d8eaf2003834a420b48d649c830c608fd6e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54421212"
---
# <a name="manage-alerts-from-scom-zabbix-and-nagios-in-azure-monitor"></a>Waarschuwingen beheren van SCOM, Zabbix en Nagios in Azure Monitor

U kunt nu uw meldingen van Nagios en Zabbix System Center Operations Manager in weergeven [Azure Monitor](https://aka.ms/azure-alerts-overview). Deze waarschuwingen zijn afkomstig uit integraties met Nagios/Zabbix-servers of System Center Operations Manager in Log Analytics. 

## <a name="prerequisites"></a>Vereisten
Alle records in de Log Analytics-opslagplaats met het type waarschuwing wordt geïmporteerd in Azure Monitor, zodat u de configuratie die is vereist voor het verzamelen van deze records moet uitvoeren.
1. Voor **Nagios** en **Zabbix** waarschuwingen, [configureren die servers](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) naar [waarschuwingen verzenden](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-alerts-nagios-zabbix?toc=%2Fazure%2Fazure-monitor%2Ftoc.json) naar Log Analytics.
1. Voor **System Center Operations Manager** waarschuwingen, [uw Operations Manager-beheergroep verbinden met uw Log Analytics-werkruimte](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Na deze implementatie van de [Waarschuwingsbeheer](https://docs.microsoft.com/azure/azure-monitor/platform/alert-management-solution) oplossing uit de marketplace met Azure-oplossingen. Zodra u klaar bent, worden alle waarschuwingen die zijn gemaakt in System Center Operations Manager geïmporteerd in Log Analytics.

## <a name="view-your-alert-instances"></a>Uw waarschuwing instanties weergeven
Nadat u het importeren in Log Analytics hebt geconfigureerd, kunt u weergeven van waarschuwingen exemplaren van deze services in de bewaking starten [Azure Monitor](https://aka.ms/azure-alerts-overview). Zodra ze aanwezig in Azure Monitor zijn, kunt u [beheren van uw waarschuwingen exemplaren](https://aka.ms/managing-alert-instances), [slimme groepen die zijn gemaakt op deze waarschuwingen beheren](https://aka.ms/managing-smart-groups) en [invloed op de status van uw waarschuwingen en slimme groepen](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  1. Deze oplossing kunt u alleen SCOM/Zabbix/Nagios geactiveerde waarschuwing exemplaren weergeven in Azure Monitor. Configuratie van waarschuwingsregel kan alleen worden weergegeven/bewerkt in de respectieve controleprogramma's. 
>  1. Alle exemplaren van geactiveerde waarschuwingen zijn beschikbaar in Azure Monitor en Azure Log Analytics. Er is momenteel geen manier om te kiezen tussen de twee of alleen specifieke waarschuwingen geactiveerde voor opnemen.
>  1. Alle waarschuwingen van SCOM, Zabbix en Nagios hebben de signaaltype 'Onbekend' omdat het onderliggende telemetrietype niet beschikbaar is.
>  1. Nagios-meldingen zijn bijvoorbeeld niet stateful – de [voorwaarde controleren](https://aka.ms/azure-alerts-overview) van een waarschuwing niet verandert van 'Fired' in 'Opgelost'. In plaats daarvan worden de "Fired" en "Opgelost" als afzonderlijke exemplaren van de waarschuwing weergegeven. 

