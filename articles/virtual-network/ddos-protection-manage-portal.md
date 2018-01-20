---
title: Beheren van Azure DDoS-bescherming standaard met de Azure portal | Microsoft Docs
description: Informatie over het Azure DDoS-bescherming standaard telemetrie in de Azure-Monitor om het risico van een aanval te gebruiken.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: ee418537fb52dbfb3eacca6e99d8572152a60910
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Azure DDoS-bescherming standaard met de Azure portal beheren

Informatie over het inschakelen en uitschakelen van gedistribueerde denial-of service (DDoS) beveiliging en telemetrie om het risico van een DDoS-aanval met Azure DDoS-bescherming Standard te gebruiken. DDoS-bescherming standaard beveiligt Azure-resources zoals virtuele machines en netwerktaakverdelers Toepassingsgateways waarvoor een Azure [openbaar IP-adres](virtual-network-public-ip-address.md) toegewezen. Zie voor meer informatie over DDoS-bescherming standaard en de mogelijkheden ervan, [DDoS-bescherming standaard overzicht](ddos-protection-overview.md). 

>[!IMPORTANT]
>Azure DDoS-bescherming Standard (DDoS-bescherming) is momenteel in preview. Een beperkt aantal Azure-resources DDoS-beveiliging ondersteunen en het is alleen beschikbaar in een select aantal regio's. Zie voor een lijst met beschikbare regio's, [DDoS-bescherming standaard overzicht](ddos-protection-overview.md). U moet [registreren voor de service](http://aka.ms/ddosprotection) tijdens de beperkte preview DDoS-bescherming is ingeschakeld voor uw abonnement ophalen. Na de registratie contact met u door het Azure DDoS-team dat u door het proces inschakelen leidt. 

## <a name="enable-ddos-protection-standard---new-virtual-network"></a>Standaard in DDoS-bescherming - nieuw virtueel netwerk inschakelen

1. Meld u via http://portal.azure.com aan bij Azure Portal. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
2. Klik op **maken van een resource** in de linkerbovenhoek van de Azure portal.
3. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
4. Een virtueel netwerk maken met de gekozen instellingen. Zie voor meer informatie over het maken van virtuele netwerken [een virtueel netwerk maken](virtual-networks-create-vnet-arm-pportal.md). Onder **DDoS-bescherming**, klikt u op **ingeschakeld**, en klik vervolgens op **maken**. Als er geen **DDoS-bescherming**, een waarschijnlijke oorzaak is dat uw abonnement is niet geregistreerd voor de functie. U moet voltooien [registratie](http://aka.ms/ddosprotection), en melding dat uw abonnement is ingeschakeld voor de functie voordat **DDoS-bescherming** wordt weergegeven.

    ![Virtueel netwerk maken](./media/ddos-protection-manage-portal/ddos-create-vnet.png)   

    > [!WARNING]
    > Als u een regio, kiest u een ondersteunde regio in de lijst in [overzicht van Azure DDoS-bescherming Standard](ddos-protection-overview.md). Als u een ondersteunde regio niet selecteert, mislukt het maken van het virtuele netwerk.

    Een waarschuwing wordt aangegeven dat het inschakelen van DDoS-bescherming leidt ertoe dat kosten. Er zijn geen kosten voor DDoS-bescherming verbonden zijn tijdens de preview. Kosten, worden algemene beschikbaarheid. U ontvangt kennisgeving 30 dagen vóór het begin van de kosten en algemene beschikbaarheid.

## <a name="enable-ddos-protection-standard---existing-virtual-network"></a>DDoS-bescherming Standard - bestaand virtueel netwerk inschakelen 

1. Klik op **virtuele netwerken** in het menu van Azure portal, en selecteer vervolgens het virtuele netwerk.
2. Klik op **DDoS-bescherming**, klikt u op **ingeschakeld** op de *DDoS-bescherming* scherm en klik vervolgens op **opslaan**. Als er geen **DDoS-bescherming**, een waarschijnlijke oorzaak is dat uw abonnement is niet geregistreerd voor de functie. U moet voltooien [registratie](http://aka.ms/ddosprotection), en melding dat uw abonnement is ingeschakeld voor de functie voordat **DDoS-bescherming** wordt weergegeven. 

    > [!WARNING]
    > Het virtuele netwerk moet zich in een ondersteunde regio. Zie voor een lijst met ondersteunde regio's, [overzicht van Azure DDoS-bescherming Standard](ddos-protection-overview.md).

    Een waarschuwing wordt aangegeven dat het inschakelen van DDoS-bescherming leidt ertoe dat kosten. Er zijn geen kosten voor DDoS-bescherming verbonden zijn tijdens de preview. Kosten, worden algemene beschikbaarheid. U ontvangt kennisgeving 30 dagen vóór het begin van de kosten en algemene beschikbaarheid.

## <a name="disable-ddos-protection-on-a-virtual-network"></a>DDoS-bescherming uitschakelen op een virtueel netwerk

1. Klik op **virtuele netwerken** in het menu van Azure portal, en selecteer vervolgens het virtuele netwerk.
2. Klik op **DDoS-bescherming**, klikt u op **uitgeschakelde** op de *DDoS-bescherming* scherm en klik vervolgens op **opslaan**.

## <a name="configure-alerts-on-ddos-protection-metrics"></a>Meldingen configureren voor DDoS-bescherming metrische gegevens

U kunt een van de beschikbare DDoS-bescherming metrische gegevens om te waarschuwen wanneer er een actieve risicobeperking tijdens een aanval met behulp van de configuratie van Azure-Monitor de waarschuwing selecteren. Wanneer de voorwaarden wordt voldaan, wordt in het opgegeven adres via e-mail een waarschuwing ontvangt.

1. Klik op **Monitor**, en klik vervolgens op **metrische gegevens**.
2. Op de *metrische gegevens* scherm, selecteert u de resourcegroep, brontype van **openbaar IP-adres**, en uw Azure openbare IP-adres.
3. Klik voor het configureren van een e-mailmelding voor een metriek **metrische waarschuwing toevoegen**. Een e-mailmelding kan worden gemaakt op een metriek, maar de meest voor de hand liggende meetwaarde is **onder DDoS-aanvallen of niet**. Dit is een Booleaanse waarde 1 of 0. Een **1** betekent dat u bent bij een aanval. Een **0** betekent dat u bent geen aangevallen.
4. Om te worden verzonden wanneer aangevallen, stelt u de metriek voor **onder DDoS-aanvallen of niet** en **voorwaarde moet groter zijn dan nul (0) in de afgelopen 5 minuten**. Soortgelijke waarschuwingen kunnen worden ingesteld voor andere metrische gegevens.

    ![Metrische gegevens configureren](./media/ddos-protection-manage-portal/ddos-metrics.png)

    U wordt gewaarschuwd met Azure Monitor metrische gegevens binnen een paar minuten van aanvalsdetectie.

    ![Aanval waarschuwing](./media/ddos-protection-manage-portal/ddos-alert.png) 

U kunt ook meer informatie over [configureren van webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md) en [logische apps](../logic-apps/logic-apps-overview.md) voor het maken van waarschuwingen.

## <a name="configure-logging-on-ddos-protection-standard-metrics"></a>Logboekregistratie configureren voor DDoS-bescherming standaard metrische gegevens

1. Klik op **Monitor**, en klik vervolgens op **diagnostische instellingen**.
2. Op de *metrische gegevens* scherm, selecteert u de resourcegroep, brontype van **openbaar IP-adres**, en uw Azure openbare IP-adres.
3. Klik op **diagnostische gegevens voor het verzamelen van de volgende gegevens inschakelen**.

Er zijn drie opties beschikbaar voor logboekregistratie:

- **Archiveren naar een opslagaccount**: schrijft de logboeken naar een opslagaccount.
- **Stream naar een event hub**: Hiermee kunt u een ontvanger logboek om op te halen logboeken met behulp van een event hub. Hierdoor integratie met Splunk of andere SIEM-systemen.
- **Verzenden met logboekanalyse**: schrijft de logboeken naar de Azure-logboekanalyse OMS-service.

## <a name="use-ddos-protection-telemetry"></a>DDoS-bescherming telemetrie gebruiken

Telemetrie voor een aanval is beschikbaar via de Azure-Monitor in realtime. De telemetrie is alleen beschikbaar voor de duur die een openbaar IP-adres wordt risicobeperking. Worden er geen telemetrie vóór of na een aanval is verholpen.

1. Klik op **Monitor**, en klik vervolgens op **metrische gegevens**. 
2. Op de *metrische gegevens* scherm, selecteert u de resourcegroep, brontype van **openbaar IP-adres**, en uw Azure openbare IP-adres. Een reeks **beschikbare metrische gegevens** wordt weergegeven aan de linkerkant van het scherm. Deze metrische gegevens, wanneer u selecteert, diagram worden weergegeven aan de **Azure Monitor metrische gegevens grafiek** op het scherm overzicht. 

De namen van de metrische opleveren typen pakketten en het aantal bytes versus pakketten, een elementaire constructie van labelnamen op elke metriek als volgt:

- **Naam van de tag verloren gegane (bijvoorbeeld inkomende pakketten verwijderd DDoS)**: het aantal pakketten verwijderd/verwijderd door het systeem DDoS-bescherming.
- **Doorgestuurde tagnaam (bijvoorbeeld: inkomende pakketten doorgestuurd DDoS)**: het aantal pakketten die zijn doorgestuurd door het systeem DDoS naar de bestemming VIP-verkeer dat niet is gefilterd.
- **Er is geen tagnaam (bijvoorbeeld: inkomende pakketten DDoS):** het totale aantal pakketten dat in het reinigen systeem – de som van de pakketten die afkomstig zijn verwijderd en wordt doorgestuurd.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Logboeken van de Azure-opslag met logboekanalyse analyseren](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md?toc=%2fazure%2fvirtual-network%2ftoc.json)