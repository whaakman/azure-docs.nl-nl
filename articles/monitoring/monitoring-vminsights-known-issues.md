---
title: Problemen met Azure Monitor voor bekende VM's (Preview) | Microsoft Docs
description: Azure Monitor voor virtuele machines is een oplossing in Azure en combineert status en prestaties bewaken van het besturingssysteem van de virtuele machine van Azure, evenals automatisch detecteren van onderdelen van de toepassing en afhankelijkheden met andere resources en de communicatie tussen de kaarten ze. In dit artikel bevat informatie over bekende problemen.
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
ms.openlocfilehash: 0f0ef83feac1a40c93910abdbd6abcada1a4d328
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613882"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Bekende problemen met Azure Monitor voor virtuele machines (Preview)

Hieronder volgen enkele bekende problemen met de Health-functie van Azure Monitor voor virtuele machines:

- De Health-functie is ingeschakeld voor alle virtuele machines die zijn verbonden met de Log Analytics-werkruimte, zelfs wanneer het wordt gestart en ingevuld op basis van een enkele virtuele machine.
- Als na het uitschakelen van de bewaking voor een virtuele machine met behulp van de ondersteunde methoden als voorbereiding opnieuw wordt geprobeerd, moet dit worden gedaan met dezelfde werkruimte.  Als een nieuwe werkruimte wordt gebruikt bij het weergeven van de status voor deze virtuele machine, kan afwijkend gedrag worden weergegeven.
- Als een Azure-VM niet meer bestaat, omdat deze is verwijderd of is verwijderd, wordt deze weergegeven in de lijstweergave van de virtuele machine met drie tot zeven dagen. Bovendien te klikken op de status van een virtuele machine verwijderd of verwijderde zou Start de **diagnostische gegevens over gezondheid** weergeven, en gaat u in een lus laden. Als u de naam van een verwijderde virtuele machine selecteert, wordt een blade gestart met een bericht waarin staat dat de virtuele machine is verwijderd.
- De periode en de frequentie van criteria voor beveiligingsstatus kunnen niet worden gewijzigd met deze release. 
- Criteria voor beveiligingsstatus kunnen niet worden uitgeschakeld. 
- Na de onboarding, kan het even voordat gegevens worden weergegeven in Azure Monitor -> virtuele Machines -> Health of vanaf de resourceblade van de virtuele machine -> Insights
- Diagnostische gegevens over gezondheid ervaring updates sneller dan een andere weergave, zodat u gegevens vertragingen optreden kan bij het schakelen tussen weergaven  
- Overzicht waarschuwingen die zijn opgenomen met Azure Monitor voor de virtuele machine zijn alleen voor waarschuwingen geactiveerd voor statusproblemen gedetecteerd met gecontroleerde Azure-VM's.
- De **lijst met waarschuwingen** weergave pagina in de virtuele machine en Azure Monitor ziet u enkele waarschuwingen waarvan bewakingsvoorwaarde is ingesteld op 'geactiveerde"in afgelopen 30 dagen.  Ze zijn niet configureerbaar. Echter na één keer te klikken op de samenvatting, de **lijst met waarschuwingen** -pagina weergeven wordt gestart, kunt u de filterwaarde van zowel de monitor-voorwaarde en de tijdsbereik wijzigen.
- Op de **lijst met waarschuwingen** -pagina weergeven, het is raadzaam niet te wijzigen de **resourcetype**, **Resource**, en **-controleservice** filters zodra ze Er zijn specifiek voor de oplossing (deze lijstweergave ziet u dat enkele extra velden als vergeleken met de Azure monitor -> Bekijk de waarschuwingen) geconfigureerd.    
- In de virtuele Linux-machines **diagnostische gegevens over gezondheid** weergave bevat de volledige domeinnaam van de virtuele machine in plaats van de naam van de gebruiker gedefinieerde VM.
- Afsluiten van virtuele machines wordt bijgewerkt enkele van de health-criteria voor een kritieke status en anderen die een goede status met net status van de virtuele machine in een kritieke status.
- Status van de ernst van waarschuwing kan niet worden gewijzigd, ze kunnen alleen worden ingeschakeld of uitgeschakeld.  Bovendien enkele dagen per week bijwerken op basis van de status van de van gezondheidscriteria.
- Alle instellingen van een exemplaar van het criterium status te wijzigen, zal leiden tot het wijzigen van de dezelfde instelling over alle health criteria exemplaren van hetzelfde type op de virtuele machine. Bijvoorbeeld, als de drempelwaarde van schijf vrije ruimte health criterium exemplaar overeenkomt met de logische schijf C: wordt gewijzigd, wordt deze drempelwaarde wordt toegepast op alle andere logische-schijven detecteren en bewaken voor dezelfde virtuele machine.   
- Drempelwaarden voor de volgende status criteria die zijn gericht op een Windows-VM niet kan worden gewijzigd, zijn omdat hun statussen zijn al ingesteld op **met** of **beschikbaar**. Wanneer een query uitgevoerd vanaf de [Workload Monitor API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager), de status health toont de *vergelijkingsoperator* waarde van **LessThan** of **groter dan**met een *drempelwaarde* waarde van **4** voor de service of de entiteit als:
   - DNS-Client-Service Health-Service wordt niet uitgevoerd 
   - DHCP-client-service health-Service wordt niet uitgevoerd 
   - RPC-Service Health-Service wordt niet uitgevoerd. 
   - Windows firewall-service health-Service wordt niet uitgevoerd.
   - Windows-gebeurtenislogboek service health-Service wordt niet uitgevoerd. 
   - Server-service health-Service wordt niet uitgevoerd. 
   - Windows remote management-service health-Service wordt niet uitgevoerd 
   - Bestandssysteemfout of beschadiging – logische schijf is niet beschikbaar

- Drempelwaarden voor de volgende criteria voor Linux-status niet gewijzigd worden, omdat hun status zijn al ingesteld op **waar**.  De status health toont de *vergelijkingsoperator* met een waarde **LessThan** en *drempelwaarde* waarde van **1** wanneer opgevraagd uit de werkbelasting Bewakings-API voor de entiteit, afhankelijk van de context:
   - Status van logische schijf – logische schijf is niet online / beschikbaar
   - De Status van de schijf-schijf is niet online / beschikbaar
   - Status van Network Adapter - netwerkadapter is uitgeschakeld  

- **Totale CPU-gebruik** health criterium in Windows ziet u een drempelwaarde voor **niet gelijk zijn aan 4** vanuit de portal en wanneer opgevraagd uit de bewakings-API van werkbelasting wanneer CPU-gebruik groter dan 95 is % en systeemwachtrijlengte groter is dan 15. Dit criterium status kan niet worden gewijzigd in deze release.  
- Wijzigingen in de configuratie, zoals het bijwerken van een drempelwaarde wordt pas van kracht zelfs als de portal of Workload Monitor API kan direct bijgewerkt tot 30 minuten.  
- Afzonderlijke processor en het logische processor op de gezondheid van criteria zijn niet beschikbaar in Windows, alleen **totale CPU-gebruik** is beschikbaar voor Windows-VM's.  
- Waarschuwingsregels is gedefinieerd voor elk criterium status worden niet weergegeven in de Azure-portal. Ze worden alleen geconfigureerd vanuit de [Workload Monitor API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager) inschakelen of uitschakelen van een waarschuwingsregel health.  
- Toewijzen van een [Azure Monitor actiegroep](../monitoring-and-diagnostics/monitoring-action-groups.md) voor health waarschuwingen niet mogelijk is vanuit de Azure-portal. U kunt alleen de melding instelling-API gebruiken voor het configureren van een actiegroep wordt geactiveerd wanneer een statuswaarschuwing wordt geactiveerd. Op dit moment actiegroepen kunnen worden toegewezen op basis van een virtuele machine, zodat alle *statusmeldingen* de dezelfde groep(en) van actie op basis van de VM-trigger wordt geactiveerd. Er is geen concept van een aparte actiegroep voor elke status waarschuwingsregel, zoals traditionele Azure-waarschuwingen. Bovendien worden alleen de actiegroepen die zijn geconfigureerd op de hoogte stellen door te sturen een e-mailadres of SMS ondersteund wanneer de gezondheid van waarschuwingen worden geactiveerd. 

## <a name="next-steps"></a>Volgende stappen
Beoordeling [ingebouwde Azure-Monitor voor virtuele machines](monitoring-vminsights-onboard.md) voor informatie over vereisten en methoden voor bewaking van uw virtuele machines wilt inschakelen.