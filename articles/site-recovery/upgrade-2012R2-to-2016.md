---
title: Upgrade van Windows Server 2012 R2-hosts en SCVMM geconfigureerd met Azure Site Recovery naar Windows Server 2016
description: Meer informatie over het instellen van herstel na noodgevallen naar Azure voor Azure Stack-VM's met de Azure Site Recovery-service.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.topic: conceptual
ms.service: site-recovery
ms.date: 12/03/2018
ms.author: rajanaki
ms.openlocfilehash: b67290f72f762331a6d699fb79aef0c0d7f9fb65
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57853270"
---
# <a name="upgrade-windows-server-2012-r2-hosts-scvmm-2012-r2-configured-with-azure-site-recovery-to-windows-server-2016--scvmm-2016"></a>Upgrade van Windows Server 2012 R2-hosts, SCVMM 2012 R2 is geconfigureerd met Azure Site Recovery naar Windows Server 2016 & SCVMM 2016

Dit artikel laat u het upgraden van Windows Server 2012 R2-hosts en SCVMM 2012 R2 die zijn geconfigureerd met Azure Site Recovery naar Windows Server 2016 & SCVMM 2016

Site Recovery draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR). De service zorgt ervoor dat uw VM-workloads beschikbaar wanneer verwacht blijven en onverwachte storingen optreden.

> [!IMPORTANT]
> Wanneer u een upgrade uitvoert van Windows Server 2012 R2-hosts die al zijn geconfigureerd voor replicatie met Azure Site Recovery, moet u de stappen in dit document volgen. Een alternatief pad voor de upgrade hebt gekozen, kan resulteren in niet-ondersteunde Staten en kan leiden tot een onderbreking in replicatie of de mogelijkheid om uit te voeren van failover.


In dit artikel leert u hoe u de volgende configuraties in uw omgeving bijwerken:

> [!div class="checklist"]
> * **Windows Server 2012 R2-hosts die niet worden beheerd door SCVMM** 
> * **Windows Server 2012 R2-hosts die worden beheerd door een zelfstandige SCVMM 2012 R2-server** 
> * **Windows Server 2012 R2-hosts die worden beheerd door de maximaal beschikbare SCVMM 2012 R2-server**


## <a name="prerequisites--factors-to-consider"></a>Vereisten en factoren om te overwegen

Voordat u een upgrade uitvoert, Let op het volgende:-

- Als u Windows Server 2012 R2-hosts die niet worden beheerd door SCVMM hebt en de instellingen van een zelfstandige omgeving is, zal er een onderbreking in de replicatie als u probeert uit te voeren van de upgrade.
- Als u hebt geselecteerd "*mijn sleutels niet opslaan in Active Directory onder Distributed Key Management*" tijdens het installeren van de eerste plaats in SCVMM 2012 R2, de upgrades niet worden voltooid.

- Als u van System Center 2012 R2 VMM gebruikmaakt 

    - Selectievakje Upgradegegevens voor de database in VMM: **VMM-console** -> **instellingen** -> **algemene** -> **databaseverbinding**
    - De serviceaccounts die wordt gebruikt voor System Center Virtual Machine Manager Agent-service controleren
    - Zorg ervoor dat u een back-up van de VMM-Database.
    - Noteer de naam van de database van de SCVMM-servers die deel uitmaken. Dit kan worden gedaan door te navigeren naar **VMM-console** -> **instellingen** -> **algemene** -> **databaseverbinding**
    - Noteer de VMM-ID van de primaire 2012R2 en de herstel-VMM-servers. VMM-ID kunt u vinden in het register 'HKLM:\SOFTWARE\Microsoft\Microsoft System Center Virtual Machine Manager Server\Setup'.
    - Zorg ervoor dat u de nieuwe SCVMMs die u aan het cluster toevoegt dezelfde namen als eerder was. 

- Als u tussen twee van uw sites beheerd door SCVMMs aan beide zijden repliceert, zorgt u ervoor dat u uw herstelzijde vóór de upgrade van de primaire zijde eerst hebt upgraden.
  > [!WARNING]
  > Selecteer tijdens het upgraden van het SCVMM 2012 R2, onder Distributed Key Management **opslag van versleutelingssleutels in Active Directory**. Kies de instellingen voor het serviceaccount en gedistribueerd Sleutelbeheer zorgvuldig. Op basis van uw selectie, versleutelde gegevens, zoals wachtwoorden in sjablonen mogelijk niet meer beschikbaar zijn na de upgrade, en de kan mogelijk van invloed zijn op replicatie met Azure Site Recovery

> [!IMPORTANT]
> Raadpleeg de gedetailleerde documentatie van SCVMM van [vereisten](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#requirements-and-limitations)

## <a name="windows-server-2012-r2-hosts-which-arent-managed-by-scvmm"></a>Windows Server 2012 R2-hosts die niet worden beheerd door SCVMM 
De lijst met stappen die hieronder worden vermeld is van toepassing op de Gebruikersconfiguratie van [Hyper-V-hosts naar Azure](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-architecture) uitgevoerd door dit [zelfstudie](https://docs.microsoft.com/azure/site-recovery/hyper-v-prepare-on-premises-tutorial)

> [!WARNING]
> Zoals vermeld in de vereisten, deze stappen zijn alleen van toepassing op een geclusterde omgeving scenario en niet in een zelfstandige configuratie van de Hyper-V-host.

1. Volg de stappen om uit te voeren de [rolling clusterupgrade.](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) het proces voor het upgraden van cluster rolling uitvoeren.
2. Met elke nieuwe Windows Server 2016-host die is geïntroduceerd in het cluster, verwijdert u de verwijzing van een Windows Server 2012 R2-host uit Azure Site Recovery door [hier] stappen te volgen. Dit moet de host die u wilt leegmaken & verwijderen uit het cluster.
3. Zodra de *Update-VMVersion* opdracht is uitgevoerd voor alle virtuele machines, de updates zijn voltooid. 
4. Gebruik de stappen die worden vermeld [hier](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-tutorial#set-up-the-source-environment) voor het registreren van de nieuwe host met Windows Server 2016 met Azure Site Recovery. Houd er rekening mee dat de Hyper-V-site al actief is en u alleen hoeft te registreren van de nieuwe host in het cluster. 
5.  Ga naar Azure-portal en controleer of de gerepliceerde status binnen de Recovery Services

## <a name="upgrade-windows-server-2012-r2-hosts-managed-by-stand-alone-scvmm-2012-r2-server"></a>Upgrade van Windows Server 2012 R2 hosts die worden beheerd door de zelfstandige SCVMM 2012 R2-server
Vóór de upgrade van uw Windows Server 2012 R2-hosts, moet u de SCVMM 2012 R2 upgraden naar SCVMM 2016. Volg de onderstaande stappen uit:-

**Upgrade zelfstandige SCVMM 2012 R2 naar SCVMM 2016**

1.  Uninstall ASR provider door te navigeren naar het Configuratiescherm -> programma's -> programma's en onderdelen -> Microsoft Azure Site Recovery en klik op verwijderen
2. [De SCVMM-database behouden en werk het besturingssysteem](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#back-up-and-upgrade-the-operating-system)
3. In **toevoegen software**, selecteer **VMM** > **verwijderen**. b. Selecteer **functies verwijderen**, en selecteer vervolgens V**MM-beheerserver en VMM-Console**. c. In **databaseopties**, selecteer **database behouden**. d. Bekijk de samenvatting en klikt u op **verwijderen**.

4. [Installeer VMM 2016](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#install-vmm-2016)
5. SCVMM starten en controleer de status van alle hosts onder **Fabrics** tabblad. Klik op **vernieuwen** om op te halen van de meest recente status. U ziet de status als 'Aandacht'. 
17. Installeer de meest recente [Microsoft Azure Site Recovery Provider](https://aka.ms/downloaddra) op de SCVMM.
16. Installeer de meest recente [Microsoft Azure Recovery Service agent (MARS)](https://aka.ms/latestmarsagent) op elke host van het cluster. Vernieuwen om ervoor te zorgen dat SCVMM is op de hosts is een query uitgevoerd.

**Upgrade van de hosts Windows Server 2012 R2 naar Windows Server 2016**

1. Volg de stappen die [hier](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) om uit te voeren van het proces voor het upgraden van cluster rolling. 
2. Na het toevoegen van de nieuwe host met het cluster, vernieuw de host uit de SCVMM-console voor het installeren van de VMM-Agent op deze bijgewerkte host.
3. Voer *Update-VMVersion* om bij te werken van de VM-versies van de virtuele machines. 
4.  Ga naar Azure portal en controleer of de gerepliceerde status van de virtuele machines in de Recovery Services-kluis. 

## <a name="upgrade-windows-server-2012-r2-hosts-are-managed-by-highly-available-scvmm-2012-r2-server"></a>Upgrade Windows Server 2012 R2-hosts worden beheerd door de maximaal beschikbare SCVMM 2012 R2-server
Vóór de upgrade van uw Windows Server 2012 R2-hosts, moet u de SCVMM 2012 R2 upgraden naar SCVMM 2016. De volgende modi van de upgrade worden ondersteund tijdens het upgraden van SCVMM 2012 R2-servers die zijn geconfigureerd met Azure Site Recovery - gemengde modus zonder aanvullende VMM-servers en de gemengde modus met aanvullende VMM-servers.

**SCVMM 2012 R2 upgraden naar SCVMM 2016**

1.  Uninstall ASR provider door te navigeren naar het Configuratiescherm -> programma's -> programma's en onderdelen -> Microsoft Azure Site Recovery en klik op verwijderen
2. Volg de stappen die [hier](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#upgrade-a-standalone-vmm-server) op basis van de modus van de upgrade die u wilt uitvoeren.
3. Start de SCVMM-console en controleer de status van alle hosts onder **Fabrics** tabblad. Klik op **vernieuwen** om op te halen van de meest recente status. U ziet de status als 'Aandacht'.
4. Installeer de meest recente [Microsoft Azure Site Recovery Provider](https://aka.ms/downloaddra) op de SCVMM.
5. De meest recente update [Microsoft Azure Recovery Service agent (MARS)](https://aka.ms/latestmarsagent) op elke host van het cluster. Vernieuwen om ervoor te zorgen SC VMM op de hosts is een query uitgevoerd.


**Upgrade van de hosts Windows Server 2012 R2 naar Windows Server 2016**

1. Volg de stappen die [hier](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) om uit te voeren van het proces voor het upgraden van cluster rolling.
2. Na het toevoegen van de nieuwe host met het cluster, vernieuw de host uit de SCVMM-console voor het installeren van de VMM-Agent op deze bijgewerkte host.
3. Voer *Update-VMVersion* om bij te werken van de VM-versies van de virtuele machines. 
4.  Ga naar Azure portal en controleer of de gerepliceerde status van de virtuele machines in de Recovery Services-kluis. 

## <a name="next-steps"></a>Volgende stappen
Zodra de upgrade van de hosts wordt uitgevoerd, kunt u uitvoeren een [testfailover](tutorial-dr-drill-azure.md) voor het testen van de status van de status van de recovery-replicatie en noodherstel.

