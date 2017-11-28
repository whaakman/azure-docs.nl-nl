---
title: Groep machines met afhankelijkheden van de machine met Azure migreren | Microsoft Docs
description: Beschrijft hoe u een evaluatie met afhankelijkheden van de machine met de service Azure migreren.
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0527e34e-a078-405e-aeb9-c91a5808112a
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 5ed49f3dc71af6a8c1c7b6c9e38fd84452505aec
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Groep machines met behulp van machine afhankelijkheid toewijzing

In dit artikel wordt beschreven hoe u een groep machines voor [Azure migreren](migrate-overview.md) assessment met behulp van machine afhankelijkheid toewijzing. Gebruik deze methode meestal in als u wilt beoordelen groepen met VM's met hogere niveaus van vertrouwen door machine afhankelijkheden tussen verschillende controleren voordat u een beoordeling uitvoert.



## <a name="prepare-machines-for-dependency-mapping"></a>Machines voor het toewijzen van afhankelijkheid voorbereiden
Als u wilt opnemen machines in de toewijzing afhankelijkheid, die u wilt downloaden en installeren van agents op elke on-premises machine die u wilt evalueren. Bovendien hebt u machines zonder internetverbinding, u moet downloaden en installeren [OMS gateway](../log-analytics/log-analytics-oms-gateway.md) erop.

### <a name="download-and-install-the-vm-agents"></a>Download en installeer de VM-agents
1. In **overzicht**, klikt u op **beheren** > **Machines**, en selecteer de computer vereist.
2. In de **afhankelijkheden** kolom, klikt u op **agents installeren**. 
3. Op de **afhankelijkheden** pagina, downloaden en installeren van Microsoft Monitoring Agent (MMA) en de afhankelijkheid-agent op elke virtuele machine die u wilt evalueren.
4. Kopieer de werkruimte-ID en sleutel. U moet deze als u de MMA op de on-premises machine installeren.

### <a name="install-the-mma"></a>Installeer de MMA

De agent installeren op een Windows-computer:

1. Dubbelklik op de gedownloade agent.
2. Op de **Welkom** pagina, klikt u op **volgende**. Op de **licentievoorwaarden** pagina, klikt u op **ik ga akkoord** de licentievoorwaarden accepteren.
3. In **doelmap**, behouden of wijzigen van de standaardinstallatiemap > **volgende**. 
4. In **installatieopties voor Agent**, selecteer **Azure logboekanalyse (OMS)** > **volgende**. 
5. Klik op **toevoegen** toevoegen van een nieuwe OMS-werkruimte. In de werkruimte-ID en sleutel die u hebt gekopieerd uit de portal te plakken. Klik op **Volgende**.


De agent te installeren op een Linux-machine:

1. De juiste bundel (x86 of x64) overbrengen naar de Linux-computer via scp/sftp.
2. De bundel installeren met behulp van het--installatie-argument.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>De afhankelijkheid-agent installeren
1. De afhankelijkheid om agent te installeren op een Windows-machine, dubbelklik op het setup-bestand en volg de wizard.
2. U installeert de agent voor afhankelijkheden op een Linux-machine te installeren als toegangspunt met de volgende opdracht:

    ```sh InstallDependencyAgent-Linux64.bin```

[Meer informatie](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) over besturingssystemen die worden ondersteund door de agent voor afhankelijkheden. 

## <a name="create-a-group"></a>Een groep maken

1. Nadat u de agents hebt geïnstalleerd, gaat u naar de portal en klik op **beheren** > **Machines**.
2. De **afhankelijkheden** kolom moet nu worden weergegeven als **afhankelijkheden weergeven**. Klik op de kolom om de afhankelijkheden weer te geven.
3. Voor elke machine, kunt u het volgende controleren:
    - Hiermee wordt aangegeven of de MMA en de agent voor afhankelijkheden zijn geïnstalleerd en of de machine is gedetecteerd.
    - Het gastbesturingssysteem op de machine.
    - Binnenkomende en uitgaande IP-verbindingen en poorten.
    - De processen uitvoeren op virtuele machines.
    - De afhankelijkheden tussen machines.

4. Voor meer gedetailleerd afhankelijkheden, klikt u op het tijdsbereik om dit te wijzigen. Het bereik is standaard een uur. U kunt het tijdsbereik wijzigen of geef de begin- en einddatum en duur.
5. Nadat u hebt aangegeven dat afhankelijke machines die u wilt groeperen, selecteert u de machines die op de kaart en klikt u op **machines te groeperen**.
6. Geef een groepsnaam. Controleer of de computer wordt gedetecteerd door Azure migreren. Als deze wordt niet opnieuw de detectie proces on-premises uitgevoerd. Als u wilt, kunt u een evaluatie direct uitvoeren.
7. Klik op **OK** om op te slaan van de groep.

    ![Een groep met machine afhankelijkheden maken](./media/how-to-create-group-machine-dependencies/create-group.png)

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over hoe](how-to-create-group-dependencies.md) verfijnen van de groep door de groepsafhankelijkheden controleren
- [Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen zijn berekend.
