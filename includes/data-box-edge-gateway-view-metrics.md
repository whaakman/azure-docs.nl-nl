---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: e02c0b86cd542b3ea12914e35a6577cf4e9b43d8
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59618509"
---
U kunt ook de metrische gegevens voor het controleren van de prestaties van het apparaat en in sommige gevallen voor het oplossen van problemen met het apparaat weergeven.

De volgende stappen uitvoeren in Azure portal een grafiek voor metrische gegevens voor geselecteerde apparaat wilt maken.

1. Voor uw resource in Azure portal, gaat u naar **bewaking > metrische gegevens** en selecteer **metrische waarde toevoegen**.

    ![Waarde toevoegen](media/data-box-edge-gateway-view-metrics/view-metrics-1.png)

2. De resource wordt automatisch ingevuld.  

    ![Huidige resource](media/data-box-edge-gateway-view-metrics/view-metrics-2.png)

    Als u wilt een andere bron opgeven, selecteert u de resource. Op **selecteert u een resource** blade, selecteer het abonnement, resourcegroep, resourcetype en de specifieke resource waarvoor u wenst te geven de metrische gegevens en selecteer **toepassen**.

    ![Kies een andere resource](media/data-box-edge-gateway-view-metrics/view-metrics-3.png)

3. Selecteer een metrische waarde voor het bewaken van uw apparaat in de vervolgkeuzelijst. De metrische gegevens kunnen worden **metrische gegevens over capaciteit** of **metrische gegevens die transactie**. De metrische gegevens over de capaciteit zijn gerelateerd aan de capaciteit van het apparaat. De metrische gegevens transactie zijn gerelateerd aan de lees- en schrijfbewerkingen naar Azure Storage.

    |Capaciteit metrische gegevens                     |Description  |
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
    |**Edge-compute - gebruik van geheugen**      | Deze waarde is niet van toepassing op Data Box-Gateway en is daarom niet ingevuld.          |
    |**Edge-compute - percentage CPU**    | Deze waarde is niet van toepassing op Data Box-Gateway en is daarom niet ingevuld.         |

4. Wanneer een waarde in de vervolgkeuzelijst is geselecteerd, kan ook aggregatie worden gedefinieerd. Aggregatie verwijst naar de werkelijke waarde samengevoegd tijdens een opgegeven tijdsduur. De geaggregeerde waarden kunnen worden gemiddelde, minimum of de toegestane maximumwaarde. Selecteer de aggregatie van de Avg, Max of Min.

    ![Grafiek weergeven](media/data-box-edge-gateway-view-metrics/view-metrics-4.png)

5. Als de metrische gegevens die u hebt geselecteerd, meerdere exemplaren heeft, zijn de splitsen optie is beschikbaar. Selecteer **toepassen splitsen** en selecteer vervolgens de waarde waarmee u wilt zien van de verdeling.

    ![Splitsing toepassen](media/data-box-edge-gateway-view-metrics/view-metrics-5.png)

6. Als u nu wilt om te zien van de uitsplitsing van slechts voor enkele exemplaren, kunt u de gegevens kunt filteren. Bijvoorbeeld: in dit geval, als u zien van de netwerkdoorvoer alleen voor de twee verbonden netwerkinterfaces op uw apparaat wilt, u kunt filteren die interfaces. Selecteer **filter toevoegen** en geeft u de naam van de netwerk-interface voor het filteren.

    ![Filter toevoegen](media/data-box-edge-gateway-view-metrics/view-metrics-6.png)

7. U kunt ook de grafiek vastmaken aan dashboard voor eenvoudige toegang.

    ![Vastmaken aan dashboard](media/data-box-edge-gateway-view-metrics/view-metrics-7.png)

8. Gegevens exporteren naar een Excel-spreadsheet of een koppeling naar de grafiek die u kunt delen, selecteert u de optie delen vanuit de opdrachtbalk.

    ![Gegevens exporteren](media/data-box-edge-gateway-view-metrics/view-metrics-8.png)