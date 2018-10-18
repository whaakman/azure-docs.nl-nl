---
title: Waarschuwingen van andere bewakingsservices beheren
description: Beheren van Nagios en Zabbix van SCOM-waarschuwingen in Azure Monitor
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: d9d0cb326fb063e0a6bbfaab6a85961ab2b35416
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389369"
---
# <a name="manage-alerts-from-other-monitoring-services"></a>Waarschuwingen van andere bewakingsservices beheren

U ziet nu uw meldingen van Nagios en Zabbix System Center Operations Manager in de [ervaring voor waarschuwingen van geïntegreerde](https://aka.ms/azure-alerts-overview). Deze waarschuwingen zijn afkomstig uit integraties met Nagios/Zabbix-servers of System Center Operations Manager in Log Analytics. 

## <a name="prerequisites"></a>Vereisten
Alle records in de Log Analytics-opslagplaats met het type waarschuwing wordt geïmporteerd in de ervaring voor geïntegreerde waarschuwingen, zodat u de configuratie die is vereist voor het verzamelen van deze records moet uitvoeren.
1. Voor **Nagios** en **Zabbix** waarschuwingen, [configureren die servers](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) om waarschuwingen te verzenden naar Log Analytics.
1. Voor **System Center Operations Manager** waarschuwingen, [uw Operations Manager-beheergroep verbinden met uw Log Analytics-werkruimte](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Alle waarschuwingen die zijn gemaakt in System Center Operations Manager worden geïmporteerd in Log Analytics.

## <a name="view-your-alert-instances"></a>Uw waarschuwing instanties weergeven
Nadat u hebt geconfigureerd voor het importeren in Log Analytics, youd weergeven van waarschuwingen exemplaren van deze services in de bewaking kunt starten de [ervaring voor waarschuwingen van geïntegreerde](https://aka.ms/azure-alerts-overview). Zodra ze aanwezig in de ervaring voor geïntegreerde waarschuwingen zijn, kunt u [beheren van uw waarschuwingen exemplaren](https://aka.ms/managing-alert-instances), [slimme groepen die zijn gemaakt op deze waarschuwingen beheren](https://aka.ms/managing-smart-groups) en [wijzigen van de status van uw waarschuwingen en slimme groepen](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  Nagios-waarschuwingen in de ervaring voor geïntegreerde waarschuwingen zijn bijvoorbeeld niet stateful: de [voorwaarde controleren](https://aka.ms/azure-alerts-overview) van een waarschuwing niet verandert van 'Fired' in 'Opgelost'. In plaats daarvan worden de "Fired" en "Opgelost" als afzonderlijke exemplaren van de waarschuwing weergegeven. 
