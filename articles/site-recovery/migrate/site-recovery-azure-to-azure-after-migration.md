---
title: Machines voor het instellen van herstel na noodgevallen tussen Azure-regio's na de migratie naar Azure met behulp van Site Recovery voorbereiden | Microsoft Docs
description: Dit artikel wordt beschreven hoe u voorbereidt machines voor het instellen van herstel na noodgevallen tussen Azure-regio's na de migratie naar Azure met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: ponatara
ms.openlocfilehash: d31eae21a246be97f0b50b9b773fcc63dfcbd084
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="replicate-azure-vms-to-another-region-after-migration-to-azure-by-using-azure-site-recovery"></a>Virtuele Azure-machines repliceren naar een andere regio na de migratie naar Azure met behulp van Azure Site Recovery

>[!NOTE]
> Azure Site Recovery-replicatie voor virtuele Azure-machines (VM's) is momenteel in preview.

## <a name="overview"></a>Overzicht

In dit artikel helpt u bij het voorbereiden van virtuele machines in Azure voor replicatie tussen twee Azure-regio's nadat deze machines zijn gemigreerd vanuit een on-premises-omgeving naar Azure met behulp van Azure Site Recovery.

## <a name="disaster-recovery-and-compliance"></a>Herstel na noodgevallen en naleving
Vandaag de dag verplaatst meer bedrijven hun werkbelasting naar Azure. Met ondernemingen verplaatsen bedrijfskritieke on-premises productie werkbelastingen naar Azure instellen van herstel na noodgevallen voor deze werkbelastingen is verplicht voor naleving en beveiliging tegen onderbrekingen in een Azure-regio.

## <a name="steps-for-preparing-migrated-machines-for-replication"></a>Stappen voor het voorbereiden van de gemigreerde machines voor replicatie
Gemigreerd machines voor het instellen van de replicatie naar een andere Azure-regio om voor te bereiden:

1. Voltooi de migratie.
2. Installeer de Azure-agent, indien nodig.
3. Verwijder de Mobility-service.  
4. Start de VM opnieuw.

Deze stappen worden in de volgende secties nader beschreven.

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-to-run-on-azure-vms"></a>Stap 1: Migreren van werkbelastingen die worden uitgevoerd op Hyper-V-machines, virtuele VMware-machines en fysieke servers worden uitgevoerd op Azure Virtual machines

De replicatie instellen en migreren van uw on-premises Hyper-V, VMware en fysieke werkbelasting naar Azure, volg de stappen in de [migreren Azure IaaS virtuele machines tussen Azure-regio's met Azure Site Recovery](site-recovery-migrate-azure-to-azure.md) artikel. 

Na de migratie moet u niet doorvoeren of verwijderen van een failover. Selecteer in plaats daarvan de **volledige migratie** optie voor elke computer die u wilt migreren:
1. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine en klik vervolgens op **Volledige migratie**. Klik op **OK** om de stap te voltooien. U kunt de voortgang in de VM-eigenschappen volgen door de bewaking van de taak uitvoeren van de migratie van **Site Recovery-taken**.
2. De **volledige migratie** action het migratieproces is voltooid, verwijdert u de replicatie voor de machine en Hiermee stopt u Site Recovery facturering voor de machine.

### <a name="step-2-install-the-azure-vm-agent-on-the-virtual-machine"></a>Stap 2: Installeer de Azure VM-agent op de virtuele machine
De Azure [VM-agent](../../virtual-machines/windows/agent-user-guide.md) moet worden geïnstalleerd op de virtuele machine voor de uitbreiding van de Site Recovery werkt en bescherming van de virtuele machine.

>[!IMPORTANT]
>Vanaf versie 9.7.0.0, op Windows virtuele machines, installeert het installatieprogramma van de Mobility-service ook de meest recente beschikbare Azure VM-agent. De virtuele machine voldoet aan de vereisten voor het gebruik van een VM-extensie, inclusief de Site Recovery-extensie de installatie van de agent op migratie. De Azure VM-agent moet handmatig worden geïnstalleerd alleen als de Mobility-service op de gemigreerde computer geïnstalleerde versie 9,6 of lager.

De volgende tabel bevat aanvullende informatie over het installeren van de VM-agent en valideren is geïnstalleerd:

| **Bewerking** | **Windows** | **Linux** |
| --- | --- | --- |
| De VM-agent installeren |Download en installeer de [agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet administrator-bevoegdheden om de installatie te voltooien. |Installeer de meest recente [Linux-agent](../../virtual-machines/linux/agent-user-guide.md). U moet administrator-bevoegdheden om de installatie te voltooien. Het is raadzaam om de installatie van de agent van uw opslagplaats voor distributie. We *wordt niet aanbevolen* installeren van de Linux-VM-agent rechtstreeks vanuit GitHub.  |
| De installatie van de VM-agent valideren |1. Blader naar de map C:\WindowsAzure\Packages in de Azure VM. Hier ziet u het bestand WaAppAgent.exe. <br>2. Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. De **productversie** veld moet 2.6.1198.718 of hoger. |N/A |


### <a name="step-3-remove-the-mobility-service-from-the-migrated-virtual-machine"></a>Stap 3: De Mobility-service van de gemigreerde virtuele machine verwijderen

Als u uw lokale VMware-machines of fysieke servers op Windows of Linux hebt gemigreerd, moet u handmatig verwijderen of het verwijderen van de gemigreerde virtuele machine van de Mobility-service.

>[!IMPORTANT]
>Deze stap is niet vereist voor Hyper-V-machines naar Azure gemigreerde.

#### <a name="uninstall-the-mobility-service-on-a-windows-server-vm"></a>Verwijder de Mobility-service op een virtuele machine van Windows Server
Gebruik een van de volgende methoden voor het verwijderen van de Mobility-service op een Windows Server-computer.

##### <a name="uninstall-by-using-the-windows-ui"></a>Verwijder met behulp van de gebruikersinterface van Windows
1. Selecteer in het Configuratiescherm **programma's**.
2. Selecteer **Microsoft Azure Site Recovery Mobility Service/hoofddoelserver**, en selecteer vervolgens **verwijderen**.

##### <a name="uninstall-at-a-command-prompt"></a>Bij een opdrachtprompt verwijderen
1. Open een opdrachtpromptvenster als administrator.
2. Voer de volgende opdracht voor het verwijderen van de Mobility-service:

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-the-mobility-service-on-a-linux-computer"></a>Verwijder de Mobility-service op een Linux-computer
1. Op uw Linux-server, moet u zich aanmelden als een **hoofdmap** gebruiker.
2. Ga in een terminal naar /user/local/ASR.
3. Voer de volgende opdracht voor het verwijderen van de Mobility-service:

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-the-vm"></a>Stap 4: De virtuele machine opnieuw opstarten

Nadat u de Mobility-service hebt verwijderd, start opnieuw op de virtuele machine voordat u de replicatie naar een andere Azure-regio instellen.


## <a name="next-steps"></a>Volgende stappen
- Beginnen met het beveiligen van uw werkbelastingen door [virtuele Azure-machines repliceren](../azure-to-azure-quickstart.md).
- Meer informatie over [richtlijnen voor het repliceren van virtuele machines van Azure toegang](../site-recovery-azure-to-azure-networking-guidance.md).
