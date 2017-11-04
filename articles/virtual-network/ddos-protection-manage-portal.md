---
title: Beheren van Azure DDoS-bescherming standaard met de Azure portal | Microsoft Docs
description: Informatie over het Azure DDoS-bescherming standaard telemetrie in de Azure-Monitor om het risico van een aanval te gebruiken.
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 5c599b4cc867dbc9a081af3a081195b998f63954
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Azure DDoS-bescherming standaard met de Azure portal beheren

>[!IMPORTANT]
>Azure DDoS-bescherming Standard (DDoS-bescherming) is momenteel in preview. Een beperkt aantal Azure-resources ondersteuning DDoS-bescherming, en in een select aantal regio's. U moet [registreren voor de service](http://aka.ms/ddosprotection) tijdens de beperkte preview DDoS-bescherming is ingeschakeld voor uw abonnement ophalen. Contact met u door het team van Azure DDoS bij de registratie naar helpt u bij de ingeschakelde. DDoS-bescherming is beschikbaar in VS-Oost, VS-West en West-Centraal VS regio's. Tijdens de preview, er zijn niet in rekening gebracht voor het gebruik van de service.

Dit artikel laat zien hoe u met de Azure-portal DDoS-beveiliging inschakelen, uitschakelen DDoS-bescherming en telemetrie om het risico van een aanval te gebruiken. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="enable-ddos-protection"></a>DDoS-beveiliging inschakelen

DDoS-beveiliging inschakelen op een nieuw of bestaand virtueel netwerk.

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Een nieuw virtueel netwerk maken en inschakelen van DDoS-bescherming

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Geef informatie op het virtuele netwerk. Onder *DDoS-bescherming*, klikt u op **ingeschakeld**, en klik vervolgens op **maken**.

    ![Virtueel netwerk maken](./media/ddos-protection-manage-portal/ddos-create-vnet.png)   

Een waarschuwing wordt aangegeven dat het inschakelen van DDoS-bescherming leidt ertoe dat kosten. Er zijn geen kosten voor DDoS-bescherming verbonden zijn tijdens de preview. Kosten verbonden zijn met algemene beschikbaarheid (GA) en klanten ontvangt kennisgeving 30 dagen vóór het begin van de kosten en algemene beschikbaarheid.

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>DDoS-beveiliging inschakelen op een bestaand virtueel netwerk 

1. Klik op **virtuele netwerken** in het menu van Azure portal, en selecteer vervolgens het virtuele netwerk.
2. Klik op **DDoS-bescherming**, klikt u op **ingeschakeld** op de *DDoS-bescherming* scherm en klik vervolgens op **opslaan**. 

Een waarschuwing wordt aangegeven dat het inschakelen van DDoS-bescherming leidt ertoe dat kosten. Er zijn geen kosten voor DDoS-bescherming verbonden zijn tijdens de preview. Kosten verbonden zijn met algemene beschikbaarheid (GA) en klanten ontvangt kennisgeving 30 dagen vóór het begin van de kosten en algemene beschikbaarheid.

## <a name="disable-ddos-protection"></a>DDoS-bescherming uitschakelen

1. Klik op **virtuele netwerken** in het menu van Azure portal, en selecteer vervolgens het virtuele netwerk.
2. Klik op **DDoS-bescherming**, klikt u op **uitgeschakelde** op de *DDoS-bescherming* scherm en klik vervolgens op **opslaan**.

## <a name="configure-alerts-on-ddos-protection-metrics"></a>Meldingen configureren voor DDoS-bescherming metrische gegevens

Mogelijkheden van de configuratie van Azure-Monitor de waarschuwing, kunt u een van de beschikbare DDoS-bescherming metrische gegevens om te waarschuwen wanneer er een actieve risicobeperking tijdens een aanval. Wanneer de voorwaarden wordt voldaan, krijgt u een waarschuwingsbericht op het opgegeven adres.

1. Klik op **Monitor**, en klik vervolgens op **metrische gegevens**.
2. Op de *metrische gegevens* scherm, selecteert u de resourcegroep, brontype van **openbaar IP-adres**, en uw Azure openbare IP-adres.
3. Klik voor het configureren van een e-mailmelding voor een metriek **Klik hier om een waarschuwing**. Een e-mailmelding kan worden gemaakt op een metriek, maar de meest voor de hand liggende meetwaarde is **onder DDoS-aanvallen of niet**. Dit is een Booleaanse waarde 1 of 0. Een **1** betekent dat u bent bij een aanval. Een **0** betekent dat u bent geen aangevallen.
4. Om te worden verzonden wanneer aangevallen, stelt u de metriek voor **onder DDoS-aanvallen of niet** en **voorwaarde moet groter zijn dan nul (0) in de afgelopen 5 minuten**. Soortgelijke waarschuwingen kunnen worden ingesteld voor andere metrische gegevens.

    ![Metrische gegevens configureren](./media/ddos-protection-manage-portal/ddos-metrics.png)

    U wordt gewaarschuwd met Azure Monitor metrische gegevens binnen een paar minuten van aanvalsdetectie.

    ![Aanval waarschuwing](./media/ddos-protection-manage-portal/ddos-alert.png) 

U kunt ook meer informatie over [configureren van webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md) en [logische apps](../logic-apps/logic-apps-what-are-logic-apps.md) voor het maken van waarschuwingen.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Logboekregistratie configureren voor DDoS-bescherming metrische gegevens

1. Klik op **Monitor**, en klik vervolgens op **diagnostische instellingen**.
2. Op de *metrische gegevens* scherm, selecteert u de resourcegroep, brontype van **openbaar IP-adres**, en uw Azure openbare IP-adres.
3. Klik op **diagnostische gegevens voor het verzamelen van de volgende gegevens inschakelen**.

Er zijn drie opties beschikbaar voor logboekregistratie:

- **Archiveren naar een opslagaccount** – schrijft de logboeken naar een opslagaccount.
- **Stream naar een event hub** – Hiermee kunt u een ontvanger logboek om op te halen logboeken met behulp van een event hub. Hierdoor integratie met Splunk of andere SIEM-systemen.
- **Verzenden met logboekanalyse** – schrijft de logboeken met Azure-logboekanalyse OMS-service.

## <a name="use-ddos-protection-telemetry"></a>DDoS-bescherming telemetrie gebruiken

Telemetrie voor een aanval is beschikbaar via de Azure-Monitor in realtime. De telemetrie is alleen beschikbaar voor de duur die een openbaar IP-adres wordt risicobeperking. Worden er geen telemetrie vóór of na een aanval is verholpen.

1. Klik op **Monitor**, en klik vervolgens op **metrische gegevens**. 
2. Op de *metrische gegevens* scherm, selecteert u de resourcegroep, brontype van **openbaar IP-adres**, en uw Azure openbare IP-adres. Er wordt een reeks met de beschikbare metrische gegevens weergegeven aan de linkerkant van het scherm. Deze metrische gegevens wanneer u selecteert, diagram worden weergegeven aan in de grafiek Azure Monitor metrische gegevens op het scherm overzicht. 

De namen van de metrische opleveren typen pakketten en bytes versus pakketten, een elementaire constructie van labelnamen op elke metriek als volgt:

- **Naam van de tag verloren gegane (bijvoorbeeld inkomende pakketten verwijderd DDoS):** het aantal pakketten verwijderd/verwijderd door het systeem DDoS-bescherming.
- **Doorgestuurde tagnaam (bijvoorbeeld: inkomende pakketten doorgestuurd DDoS):** het aantal pakketten die zijn doorgestuurd door het systeem DDoS naar de bestemming VIP-verkeer dat niet is gefilterd.
- **Er is geen tagnaam (bijvoorbeeld: inkomende pakketten DDoS):** het totale aantal pakketten dat in het reinigen systeem – de som van de pakketten die afkomstig zijn verwijderd en wordt doorgestuurd.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Logboeken van de Azure-opslag met logboekanalyse analyseren](../log-analytics/log-analytics-azure-storage.md)
- [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)