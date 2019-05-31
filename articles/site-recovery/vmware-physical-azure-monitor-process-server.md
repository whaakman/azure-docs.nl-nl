---
title: Monitor voor de processerver Azure Site Recovery
description: In dit artikel wordt beschreven hoe u Azure Site Recovery-processerver bewaken.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 4ff52e737438210296b8f2201d5e66e1d38b7bc9
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418284"
---
# <a name="monitor-the-process-server"></a>Monitor voor de processerver

In dit artikel wordt beschreven hoe u voor het bewaken van de [Site Recovery](site-recovery-overview.md) processerver.

- De processerver wordt gebruikt bij het instellen van herstel na noodgevallen van on-premises VMware-machines en fysieke servers naar Azure.
- De processerver wordt standaard uitgevoerd op de configuratieserver. Dit wordt standaard geïnstalleerd wanneer u de configuratieserver implementeren.
- (Optioneel) om te schalen en verwerk grotere aantallen gerepliceerde machines en grotere volumes van replicatieverkeer, kunt u aanvullende, scale-out processervers implementeren.

[Meer informatie](vmware-physical-azure-config-process-server-overview.md) over de rol en de implementatie van processervers.

## <a name="monitoring-overview"></a>Bewakingsoverzicht

Omdat de processerver vele rollen, met name in de gerepliceerde gegevens caching, compressie en overbrengen naar Azure heeft, is het belangrijk om te controleren van de serverstatus proces continu.

Er zijn een aantal situaties die vaak invloed hebben op prestaties van server. Problemen die betrekking hebben op prestaties heeft een trapsgewijze effect op de VM-status, uiteindelijk zowel de processerver en de gerepliceerde machines pushen naar een kritieke status. Situaties zijn onder andere:

- Groot aantal virtuele machines gebruiken een processerver nadert of aanbevolen beperkingen overschreden.
- Virtuele machines met behulp van de processerver hebben een hoog verloop.
- Netwerkdoorvoer tussen virtuele machines en de processerver is niet voldoende zijn voor het uploaden van gegevens van replicatie naar de processerver.
- Netwerkdoorvoer tussen de processerver en Azure is niet voldoende om replicatiegegevens van de processerver naar Azure te uploaden.

Al deze problemen kunnen invloed hebben op het beoogde herstelpunt (RPO) van virtuele machines. 

**Waarom?** Omdat het genereren van een herstelpunt voor een virtuele machine vereist dat alle schijven op de virtuele machine een gemeenschappelijk tijdstip. Als één schijf een hoog verloop heeft, replicatie traag is of de processerver niet optimaal, heeft invloed op hoe efficiënt herstelpunten worden gemaakt.

## <a name="monitor-proactively"></a>Proactief bewaken

Om te voorkomen dat problemen met de processerver, is het belangrijk om:

- Informatie over specifieke vereisten voor processervers met behulp van [capaciteit en de grootte van de richtlijnen](site-recovery-plan-capacity-vmware.md#capacity-considerations), en zorg ervoor dat processervers zijn geïmplementeerd en uitgevoerd op basis van aanbevelingen.
- Waarschuwingen controleren en problemen oplossen wanneer deze zich voordoen, processervers efficiënt te houden.


## <a name="process-server-alerts"></a>Waarschuwingen van de processerver

De processerver genereert een aantal statusmeldingen, in de volgende tabel samengevat.

**Waarschuwingstype** | **Details**
--- | ---
![In orde][green] | Processerver is verbonden en in orde is.
![Waarschuwing][yellow] | CPU-gebruik > 80% voor de laatste 15 minuten
![Waarschuwing][yellow] | Geheugen gebruik > 80% voor de laatste 15 minuten
![Waarschuwing][yellow] | Cachemap vrije ruimte < 30% van de laatste 15 minuten
![Waarschuwing][yellow] | Services van de processerver worden niet uitgevoerd voor de laatste 15 minuten
![Kritiek][red] | CPU-gebruik > 95% voor de laatste 15 minuten
![Kritiek][red] | Geheugen gebruik > 95% voor de laatste 15 minuten
![Kritiek][red] | Cachemap vrije ruimte < 25% van de laatste 15 minuten
![Kritiek][red] | Geen heartbeat van de processerver gedurende 15 minuten.

![tabelsleutel](./media/vmware-physical-azure-monitor-process-server/table-key.png)

> [!NOTE]
> De algehele status van de processerver is gebaseerd op de slechtste waarschuwing gegenereerd.



## <a name="monitor-process-server-health"></a>Status van proces bewaken

U kunt de status van uw processervers als volgt controleren: 

1. Voor het bewaken van de replicatiestatus en de status van een gerepliceerde machine en de processerver, in de kluis > **gerepliceerde items**, klikt u op de computer die u wilt bewaken.
2. In **replicatiestatus**, kunt u de status van de VM-status bewaken. Klik op de status om in te zoomen voor foutgegevens.

    ![Status voor proces-server in het dashboard voor VM 's](./media/vmware-physical-azure-monitor-process-server/vm-ps-health.png)

4. In **proces serverstatus**, kunt u de status van de processerver bewaken. Inzoomen voor meer informatie.

    ![Details van proces-server in het dashboard voor VM 's](./media/vmware-physical-azure-monitor-process-server/ps-summary.png)

5. Status kan ook worden gecontroleerd met behulp van de grafische weergave op de pagina van de virtuele machine.
    - Een uitbreidbare processerver is gemarkeerd in de oranje als er waarschuwingen die zijn gekoppeld aan deze en rood als er kritieke problemen. 
    - Als de processerver wordt uitgevoerd in de standaardimplementatie op de configuratieserver, klikt u vervolgens de configuratieserver dienovereenkomstig gemarkeerd.
    - Als u wilt inzoomen, klikt u op de configuratieserver of processerver. Let op eventuele problemen en geen herstelaanbevelingen.

U kunt ook bewaken servers in de kluis onder verwerken **Site Recovery-infrastructuur**. In **beheren van uw infrastructuur voor Site Recovery**, klikt u op **configuratieservers**. Selecteer de configuratieserver die is gekoppeld aan de processerver en inzoomen naar beneden naar details over de processerver.


## <a name="next-steps"></a>Volgende stappen

- Als een proces voor problemen met servers, gaat u als volgt onze [richtlijnen voor probleemoplossing](vmware-physical-azure-troubleshoot-process-server.md)
- Als u meer hulp nodig hebt, stel uw vraag in de [Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). 

[green]: ./media/vmware-physical-azure-monitor-process-server/green.png
[yellow]: ./media/vmware-physical-azure-monitor-process-server/yellow.png
[red]: ./media/vmware-physical-azure-monitor-process-server/red.png
