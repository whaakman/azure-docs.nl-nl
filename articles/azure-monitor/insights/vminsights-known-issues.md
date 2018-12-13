---
title: Azure Monitor voor bekende problemen-machines (preview) | Microsoft Docs
description: In dit artikel bevat informatie over bekende problemen met Azure Monitor voor virtuele machines, een oplossing in Azure en combineert statussen en prestaties van het besturingssysteem van de virtuele machine van Azure. Azure Monitor voor virtuele machines onderdelen van de toepassing en afhankelijkheden met andere resources wordt ook automatisch gedetecteerd en de communicatie tussen deze toewijst.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/07/2018
ms.author: magoedte
ms.openlocfilehash: d720a7401b9ed1188a01d3cc2cc9ec7b66b640ce
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091542"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Bekende problemen met Azure Monitor voor virtuele machines (preview)

In dit artikel bevat informatie over bekende problemen met Azure Monitor voor virtuele machines, een oplossing in Azure en combineert statussen en prestaties van het besturingssysteem van de virtuele machine van Azure. 

De volgende volgen bekende problemen met de functie voor Health:

- De Health-functie is ingeschakeld voor alle virtuele machines die zijn verbonden met de Log Analytics-werkruimte. Dit is dus zelfs als de actie begint en eindigt in een enkele virtuele machine.
- Nadat u Schakel bewaking voor een virtuele machine met behulp van de ondersteunde methoden en u probeert u deze opnieuw implementeert, moet u deze implementeren in dezelfde werkruimte. Als u een nieuwe werkruimte en probeer het gebruikt om de status voor deze virtuele machine weer te geven, kan het afwijkend gedrag wordt weergegeven.
- Als een Azure-VM is verwijderd of wordt gewist, wordt deze weergegeven in de VM-lijstweergave voor drie tot zeven dagen. Bovendien op de status van een virtuele machine verwijderd of verwijderde klikt, wordt de **diagnostische gegevens over gezondheid** weergeven en start vervolgens een lus laden. Als u de naam van de verwijderde virtuele machine selecteert, wordt een deelvenster geopend met een bericht weergegeven dat de virtuele machine is verwijderd.
- De periode en de frequentie van criteria voor beveiligingsstatus kunnen niet worden gewijzigd met deze release. 
- Criteria voor beveiligingsstatus kunnen niet worden uitgeschakeld. 
- Na de implementatie, kan het even voordat gegevens worden weergegeven in de **Azure Monitor** > **virtuele Machines** > **Health** deelvenster of de  **VM-resource** > **Insights** deelvenster.
- Diagnostische gegevens over de status van updates sneller dan een andere weergave-ervaring. De gegevens kan zijn vertraagd wanneer u tussen weergaven schakelen. 
- De samenvatting van waarschuwingen die is opgenomen met Azure Monitor voor de virtuele machine bevat alleen de waarschuwingen die zijn dat resultaat van de gezondheid van problemen die worden gedetecteerd met virtuele Azure-machines bewaakt.
- De **lijst met waarschuwingen** deelvenster in de enkele virtuele machine en de Azure Monitor worden waarschuwingen waarvan bewakingsvoorwaarde is ingesteld op weergegeven *geactiveerd* in de afgelopen 30 dagen. De waarschuwingen niet kunnen worden geconfigureerd. Echter, nadat de **lijst met waarschuwingen** deelvenster wordt geopend, klikt u op de samenvatting van de filterwaarde van zowel de monitor-voorwaarde en de tijdbereik wilt wijzigen.
- In de **lijst met waarschuwingen** in het deelvenster wordt aangeraden niet wijzigen van de **resourcetype**, **Resource**, en **-controleservice** filters. Ze hebben geconfigureerd specifiek zijn voor de oplossing. In deze lijstweergave geeft meer velden dan de **Azure monitor** > **waarschuwingen** lijstweergave biedt.   
- In Linux-VM's, de **diagnostische gegevens over gezondheid** weergave bevat de volledige domeinnaam van de virtuele machine in plaats van de naam van de gebruiker gedefinieerde VM.
- Enkele van de van gezondheidscriteria voor het afsluiten van virtuele machines updates *kritieke* en andere naar *in orde*. De net VM-status wordt weergegeven als *kritieke*.
- Status van de ernst van waarschuwing kan niet worden gewijzigd. Het kan alleen worden ingeschakeld of uitgeschakeld. Bovendien worden enkele dagen per week bijgewerkt op basis van de status van de van gezondheidscriteria.
- Als u alle instellingen van een health criterium-instantie wijzigt, worden alle exemplaren van de gezondheid van criteria van hetzelfde type op de virtuele machine worden gewijzigd. Bijvoorbeeld, als de drempelwaarde van de status van de vrije ruimte criterium schijfexemplaar die overeenkomt met de logische schijf C: wordt gewijzigd, geldt deze drempelwaarde voor alle andere logische schijven die zijn gedetecteerd en bewaakt voor dezelfde virtuele machine.  
- Drempelwaarden voor de health-criteria die zijn gericht op een Windows-VM zijn gewijzigd, niet omdat hun statussen zijn ingesteld op *met* of *beschikbaar*. Wanneer u de status van een query de [Workload Monitor API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager), wordt de *vergelijkingsoperator* waarde van **LessThan** of **groter dan** met een *drempelwaarde* waarde van **4** voor de service of de entiteit als:
   - DNS-Client-Service Health-Service niet actief is. 
   - DHCP-client-service health-Service niet actief is. 
   - RPC-Service Health-Service niet actief is. 
   - Windows firewall-service health-Service niet actief is.
   - Windows-gebeurtenislogboek service health-Service niet actief is. 
   - Server-service health-Service niet actief is. 
   - Windows remote management-service health-Service wordt niet uitgevoerd. 
   - Bestandssysteemfout of beschadiging – logische schijf is niet beschikbaar.

- Drempelwaarden voor de volgende criteria voor Linux-status zijn gewijzigd, niet omdat hun status is al ingesteld op *waar*. De status wordt weergegeven de *vergelijkingsoperator* met een waarde **LessThan** en *drempelwaarde* waarde van **1** wanneer opgevraagd uit de Workload bewakings-API voor de entiteit, afhankelijk van de context:
   - Status van logische schijf – logische schijf is niet online / beschikbaar
   - De Status van de schijf-schijf is niet online / beschikbaar
   - Status van Network Adapter - netwerkadapter is uitgeschakeld  

- De *totale CPU-gebruik* health criterium in Windows wordt een drempelwaarde voor **niet gelijk zijn aan 4** in zowel de portal als de werkbelasting bewakings-API. De drempelwaarde wordt bereikt wanneer *totale CPU-gebruik* is groter dan 95 procent en de wachtrijlengte groter is dan 15. Dit criterium status kan niet worden gewijzigd in deze release. 
- Wijzigingen in de configuratie, zoals het bijwerken van een drempelwaarde, wordt er maximaal 30 minuten duren, zelfs als de portal of Workload Monitor API kan ze onmiddellijk bijwerken. 
- Afzonderlijke processor en het logische processor op de gezondheid van criteria zijn niet beschikbaar in Windows. Totale CPU-gebruik is beschikbaar voor Windows-VM's. 
- Regels voor waarschuwingen die zijn gedefinieerd voor elk criterium status worden niet weergegeven in de Azure-portal. U kunt inschakelen of uitschakelen van een statuswaarschuwing voor de regel alleen in de [Workload Monitor API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager). 
- U kunt geen toewijzen een [Azure Monitor actiegroep](../../monitoring-and-diagnostics/monitoring-action-groups.md) voor de gezondheid van waarschuwingen in Azure portal. U kunt alleen de melding instelling API het configureren van een actiegroep wordt geactiveerd wanneer een statuswaarschuwing wordt geactiveerd. Op dit moment kunt u actiegroepen op basis van een virtuele machine toewijzen zodat alle *statusmeldingen* dezelfde actiegroepen op basis van de VM-trigger wordt geactiveerd. In tegenstelling tot traditionele Azure-waarschuwingen is er geen concept van een aparte actiegroep voor elke status waarschuwingsregel. Bovendien worden alleen actiegroepen die zijn geconfigureerd voor e-mailadres of SMS-berichten ondersteund wanneer de gezondheid van waarschuwingen worden geactiveerd. 

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over de vereisten en -methoden voor het inschakelen van bewaking van uw virtuele machines, [implementeert Azure Monitor voor virtuele machines](vminsights-onboard.md).
