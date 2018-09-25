---
title: Azure Monitor voor bekende problemen met VM's | Microsoft Docs
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
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: 819c3e74355cf80c7a998abb8b02b10c9e077059
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47062765"
---
# <a name="known-issues-with-azure-monitor-for-vms"></a>Bekende problemen met Azure Monitor voor virtuele machines

Hieronder volgen enkele bekende problemen met de Health-functie van Azure Monitor voor virtuele machines:

- De periode en de frequentie van criteria voor beveiligingsstatus kunnen niet worden gewijzigd met deze release. 
- Criteria voor beveiligingsstatus kunnen niet worden uitgeschakeld. 
- Na de onboarding, kan het even voordat gegevens worden weergegeven in Azure Monitor -> virtuele Machines -> Health of vanaf de resourceblade van de virtuele machine -> Insights
- Diagnostische gegevens over gezondheid ervaring updates sneller dan een andere weergave, zodat u gegevens vertragingen optreden kan bij het schakelen tussen weergaven  
- Waarschuwingen samenvatting beschikbaar in Azure Monitor voor VM-status zijn alleen voor waarschuwingen geactiveerd voor statusproblemen gedetecteerd met gecontroleerde Azure-VM's.
- De **lijst met waarschuwingen** weergave pagina in de virtuele machine en Azure Monitor ziet u enkele waarschuwingen waarvan bewakingsvoorwaarde is ingesteld op 'geactiveerde"in afgelopen 30 dagen.  Ze zijn niet configureerbaar. Echter na één keer te klikken op de samenvatting, de **lijst met waarschuwingen** -pagina weergeven wordt gestart, kunt u de filterwaarde van zowel de monitor-voorwaarde en de tijdsbereik wijzigen.
- Op de **lijst met waarschuwingen** -pagina weergeven, het is raadzaam niet te wijzigen de **resourcetype**, **Resource**, en **-controleservice** filters zodra ze Er zijn specifiek voor de oplossing (deze lijstweergave ziet u dat enkele extra velden als vergeleken met de Azure monitor -> Bekijk de waarschuwingen) geconfigureerd.    
- In de virtuele Linux-machines **diagnostische gegevens over gezondheid** weergave bevat de volledige domeinnaam van de virtuele machine in plaats van de naam van de gebruiker gedefinieerde VM.
- Afsluiten van virtuele machines wordt bijgewerkt enkele van de health-criteria voor een kritieke status en anderen die een goede status met net status van de virtuele machine in een kritieke status.
- Status van de ernst van waarschuwing kan niet worden gewijzigd, ze kunnen alleen worden ingeschakeld of uitgeschakeld.  Bovendien enkele dagen per week bijwerken op basis van de status van de van gezondheidscriteria.
- Alle instellingen van een exemplaar van het criterium status te wijzigen, zal leiden tot het wijzigen van de dezelfde instelling over alle health criteria exemplaren van hetzelfde type op de virtuele machine. Bijvoorbeeld, als de drempelwaarde van schijf vrije ruimte health criterium exemplaar overeenkomt met de logische schijf C: wordt gewijzigd, wordt deze drempelwaarde wordt toegepast op alle andere logische-schijven detecteren en bewaken voor dezelfde virtuele machine.   
- Drempelwaarden voor sommige Windows health criteria, zoals de status van DNS Client-Service niet kan worden gewijzigd, worden omdat de status in orde is al gekoppeld aan de **met**, **beschikbaar** status van de service of de entiteit afhankelijk van de context.  In plaats daarvan de waarde wordt vertegenwoordigd door cijfer 4, deze worden geconverteerd naar de werkelijke weergegeven tekenreeks in een toekomstige release.  
- Drempelwaarden voor bepaalde Linux-criteria voor beveiligingsstatus niet kan worden gewijzigd, zoals de status van de logische schijf, omdat ze al zijn ingesteld voor het activeren van in orde.  Dit geeft aan of er iets is online of offline zijn, of in- of uitschakelen, en worden weergegeven en hetzelfde door de waarde 1 of 0 weer te geven.
- Het filter van de groep resources in elke willekeurige resourcegroep bijwerken tijdens het gebruik van het op schaal met Azure monitor -> virtuele Machines Health -> een lijstweergave met vooraf geselecteerde groep van abonnement en de resourcegroep ->, zorgt ervoor dat de lijst weer te geven **geen resultaat**.  Ga terug naar de Azure Monitor -> virtuele Machines -> tabblad status en selecteer het gewenste abonnement en de resourcegroep en navigeer vervolgens naar de lijstweergave.

## <a name="next-steps"></a>Volgende stappen
Beoordeling [ingebouwde Azure-Monitor voor virtuele machines](monitoring-vminsights-onboard.md) voor informatie over vereisten en methoden voor bewaking van uw virtuele machines wilt inschakelen.