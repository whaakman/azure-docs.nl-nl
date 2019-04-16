---
title: Uw Azure Data Box-Edge-apparaat controleren | Microsoft Docs
description: Beschrijft hoe u gebruik van de Azure-portal en de lokale webgebruikersinterface voor het bewaken van uw Azure Data Box-Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 4d4cc8a3eaf2bf806d5b265be226482b23804f82
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2019
ms.locfileid: "59580348"
---
# <a name="monitor-your-azure-data-box-edge"></a>Controleren van de rand van uw Azure Data Box

In dit artikel wordt beschreven hoe u uw Azure Data Box-Edge bewaken. Voor het bewaken van uw apparaat, kunt u Azure portal of de lokale webgebruikersinterface. De Azure portal gebruiken voor apparaatgebeurtenissen weergeven, configureren en waarschuwingen beheren en weergeven van metrische gegevens. Gebruik de lokale webgebruikersinterface van uw fysieke apparaat om de status van de hardware van de verschillende onderdelen van het apparaat weer te geven.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Apparaatgebeurtenissen en de bijbehorende waarschuwingen weergeven
> * Hardwarestatus weergeven van Apparaatonderdelen
> * Capaciteit en transactie metrische gegevens voor uw apparaat weergeven
> * Configureren en beheren van waarschuwingen

## <a name="view-device-events"></a>Apparaat-gebeurtenissen weergeven

De volgende stappen uitvoeren in Azure portal om een apparaatgebeurtenis weer te geven.

1. In de Azure-portal, gaat u naar uw Data Box-Edge / gegevensgateway in het resource- en ga vervolgens naar **bewaking > apparaatgebeurtenissen**.
2. Een gebeurtenis selecteren en weergeven van details van de waarschuwing. Neem passende maatregelen om op te lossen voorwaarde voor de waarschuwing.

    ![Details van gebeurtenis- en weergave selecteren](media/data-box-edge-monitor/view-device-events.png)

## <a name="view-hardware-status"></a>Hardwarestatus weergeven

De volgende stappen uitvoeren in de lokale webgebruikersinterface om de status van de hardware van de Apparaatonderdelen van uw weer te geven. Deze informatie is alleen beschikbaar voor een Data Box-Edge-apparaat.

1. Verbinding maken met de lokale webgebruikersinterface van uw apparaat.
2. Ga naar **onderhoud > hardwarestatus**. U kunt de status van de verschillende onderdelen van het apparaat weergeven.

    ![Hardwarestatus weergeven](media/data-box-edge-monitor/view-hardware-status.png)

## <a name="view-metrics"></a>Metrische gegevens bekijken

U kunt ook de metrische gegevens voor het controleren van de prestaties van het apparaat en in sommige gevallen voor het oplossen van problemen met het apparaat weergeven.

De volgende stappen uitvoeren in Azure portal een grafiek voor metrische gegevens voor geselecteerde apparaat wilt maken.

1. Voor uw resource in Azure portal, gaat u naar **bewaking > metrische gegevens** en selecteer **metrische waarde toevoegen**.

    ![Waarde toevoegen](media/data-box-edge-monitor/view-metrics-1.png)

2. De resource wordt automatisch ingevuld.  

    ![Huidige resource](media/data-box-edge-monitor/view-metrics-2.png)

    Als u wilt een andere bron opgeven, selecteert u de resource. Op **selecteert u een resource** blade, selecteer het abonnement, resourcegroep, resourcetype en de specifieke resource waarvoor u wenst te geven de metrische gegevens en selecteer **toepassen**.

    ![Kies een andere resource](media/data-box-edge-monitor/view-metrics-3.png)

3. Selecteer een metrische waarde voor het bewaken van uw apparaat in de vervolgkeuzelijst. De metrische gegevens kunnen worden **metrische gegevens over capaciteit** of **metrische gegevens die transactie**. De metrische gegevens over de capaciteit zijn gerelateerd aan de capaciteit van het apparaat. De metrische gegevens transactie zijn gerelateerd aan de lees- en schrijfbewerkingen naar Azure Storage.

    |Capaciteit metrische gegevens                     |Beschrijving  |
    |-------------------------------------|-------------|
    |**Beschikbare capaciteit**               | Verwijst naar de grootte van de gegevens die kunnen worden geschreven naar het apparaat. Dit is met andere woorden, de capaciteit die beschikbaar kan worden gemaakt op het apparaat. <br></br>U kunt de capaciteit van het apparaat vrij door het verwijderen van de lokale kopie van bestanden die een kopie op zowel het apparaat als de cloud hebben.        |
    |**Totale capaciteit**                   | Verwijst naar het totaal aantal bytes op het apparaat om gegevens op te schrijven. Dit is ook aangeduid als de totale grootte van de lokale cache. <br></br> Nu kunt u de capaciteit van een bestaand virtueel apparaat verhogen door een gegevensschijf toe te voegen. Een gegevensschijf via de hypervisor-voor de virtuele machine toevoegt en opnieuw opstarten van uw virtuele machine. De groep lokale opslag van het Gateway-apparaat wilt uitbreiden om te voldoen aan de toegevoegde gegevensschijf. <br></br>Ga voor meer informatie naar [toevoegen van een harde schijf voor Hyper-V virtuele machine](https://www.youtube.com/watch?v=EWdqUw9tTe4). |
    
    |Metrische gegevens de transactie              | Description         |
    |-------------------------------------|---------|
    |**Verzonden bytes (apparaat) in de cloud**    | Som van alle bytes wordt geüpload voor alle shares op uw apparaat        |
    |**Verzonden bytes (delen) in de cloud**     | Bytes verzonden per share. Dit kan zijn: <br></br> Gem. Dit is de (som van alle bytes verzonden per share / het nummer van shares),  <br></br>Limiet is bereikt, is het maximum aantal bytes van een share geüpload <br></br>Minimum, dit het minimale aantal bytes dat is geüpload van een share is      |
    |**Doorvoer van cloud-download (delen)**| Bytes gedownload per share. Dit kan zijn: <br></br> Gem. Dit is de (som van alle bytes lezen of gedownload naar een share / het nummer van shares) <br></br> Limiet is bereikt, is het maximum aantal bytes gedownload van een share<br></br> en Min, dit het minimale aantal bytes dat is gedownload van een share is  |
    |**Cloud leesdoorvoer**            | Som van alle bytes lezen vanuit de cloud via alle shares op uw apparaat     |
    |**Cloud uploaden doorvoer**          | Som van alle bytes geschreven naar de cloud via alle shares op uw apparaat     |
    |**Doorvoer van cloud-uploaden (delen)**  | Som van alle bytes geschreven naar de cloud van een share / totaal aantal shares is average, max en min per share      |
    |**Lezen doorvoer (netwerk)**           | Bevat de netwerkdoorvoer systeem voor alle bytes lezen vanuit de cloud. In deze weergave kan gegevens die is niet beperkt tot bestandsshares bevatten. <br></br>Splitsen, wordt het verkeer via de netwerkadapters op het apparaat weergegeven. Dit omvat netwerkadapters die niet zijn verbonden of ingeschakeld.      |
    |**Doorvoer (netwerk) schrijven**       | Bevat de netwerkdoorvoer systeem voor alle bytes geschreven naar de cloud. In deze weergave kan gegevens die is niet beperkt tot bestandsshares bevatten. <br></br>Splitsen, wordt het verkeer via de netwerkadapters op het apparaat weergegeven. Dit omvat netwerkadapters die niet zijn verbonden of ingeschakeld.          |
    |**Edge-compute - gebruik van geheugen**      | Het geheugengebruik voor de IoT Edge-apparaat voor uw gegevens in Edge. Als er een intensief gebruik en prestaties van uw apparaat wordt beïnvloed door de huidige werkbelasting die u hebt geïmplementeerd, moet u contact op met Microsoft Support om te bepalen van de volgende stappen. <br></br>Met deze metriek is niet ingevuld voor Data Box-Gateway.          |
    |**Edge-compute - percentage CPU**    | CPU-gebruik voor IoT Edge-apparaat voor uw gegevens in Edge. Als er een intensief gebruik en prestaties van uw apparaat wordt beïnvloed door de huidige werkbelasting die u hebt geïmplementeerd, moet u contact op met Microsoft Support om te bepalen van de volgende stappen. <br></br>Met deze metriek is niet ingevuld voor Data Box-Gateway.        |
1. Wanneer een waarde in de vervolgkeuzelijst is geselecteerd, kan ook aggregatie worden gedefinieerd. Aggregatie verwijst naar de werkelijke waarde samengevoegd tijdens een opgegeven tijdsduur. De geaggregeerde waarden kunnen worden gemiddelde, minimum of de toegestane maximumwaarde. Selecteer de aggregatie van de Avg, Max of Min.

    ![Grafiek weergeven](media/data-box-edge-monitor/view-metrics-4.png)

5. Als de metrische gegevens die u hebt geselecteerd, meerdere exemplaren heeft, zijn de splitsen optie is beschikbaar. Selecteer **toepassen splitsen** en selecteer vervolgens de waarde waarmee u wilt zien van de verdeling.

    ![Splitsing toepassen](media/data-box-edge-monitor/view-metrics-5.png)

6. Als u nu wilt om te zien van de uitsplitsing van slechts voor enkele exemplaren, kunt u de gegevens kunt filteren. Bijvoorbeeld: in dit geval, als u zien van de netwerkdoorvoer alleen voor de twee verbonden netwerkinterfaces op uw apparaat wilt, u kunt filteren die interfaces. Selecteer **filter toevoegen** en geeft u de naam van de netwerk-interface voor het filteren.

    ![Filter toevoegen](media/data-box-edge-monitor/view-metrics-6.png)

7. U kunt ook de grafiek vastmaken aan dashboard voor eenvoudige toegang.

    ![Vastmaken aan dashboard](media/data-box-edge-monitor/view-metrics-7.png)

8. Gegevens exporteren naar een Excel-spreadsheet of een koppeling naar de grafiek die u kunt delen, selecteert u de optie delen vanuit de opdrachtbalk.

    ![Gegevens exporteren](media/data-box-edge-monitor/view-metrics-8.png)

## <a name="manage-alerts"></a>Waarschuwingen beheren

Waarschuwingsregels melding van waarschuwingen voorwaarden met betrekking tot het verbruik van resources op uw apparaat configureren. U kunt regels voor het bewaken van uw apparaat voor waarschuwing voorwaarden voor waarschuwingen configureren. Voor meer informatie over waarschuwingen, gaat u naar [maken, weergeven en beheren van metrische waarschuwingen in Azure monitor](../azure-monitor/platform/alerts-metric.md).

## <a name="next-steps"></a>Volgende stappen 

Meer informatie over [Bandbreedte beheren](data-box-edge-manage-bandwidth-schedules.md).