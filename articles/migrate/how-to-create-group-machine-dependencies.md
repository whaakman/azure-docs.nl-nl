---
title: Groep machines met afhankelijkheden van de machine met Azure migreren | Microsoft Docs
description: Beschrijft hoe u een evaluatie met afhankelijkheden van de machine met de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: a9850044266ec05cee5e32c6825609bcf969351d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34203192"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Groep machines met behulp van machine afhankelijkheid toewijzing

In dit artikel wordt beschreven hoe u een groep machines voor [Azure migreren](migrate-overview.md) beoordeling door het visualiseren van afhankelijkheden van machines. Gebruik deze methode meestal in als u wilt beoordelen groepen met VM's met hogere niveaus van vertrouwen door machine afhankelijkheden tussen verschillende controleren voordat u een beoordeling uitvoert. Afhankelijkheid visualisatie kunt u de migratie naar Azure effectief te plannen. Kunt u ervoor zorgen dat er niets is achtergelaten en onverwachte storingen niet bijgewerkt wanneer u naar Azure migreert. U kunt alle onderling afhankelijk systemen die samen migreren en bepalen of een actieve gebruikers nog steeds bedient of geschikt is voor het buiten gebruik stellen in plaats van de migratie moeten detecteren. 


## <a name="prepare-machines-for-dependency-mapping"></a>Machines voor het toewijzen van afhankelijkheid voorbereiden
Als u wilt weergeven van afhankelijkheden van machines, die u wilt downloaden en installeren van agents op elke on-premises machine die u wilt evalueren. Bovendien hebt u machines zonder internetverbinding, u moet downloaden en installeren [OMS gateway](../log-analytics/log-analytics-oms-gateway.md) erop.

### <a name="download-and-install-the-vm-agents"></a>De VM-agents downloaden en installeren
1. In **overzicht**, klikt u op **beheren** > **Machines**, en selecteer de computer vereist.
2. In de **afhankelijkheden** kolom, klikt u op **agents installeren**. 
3. Op de **afhankelijkheden** pagina, downloaden en installeren van Microsoft Monitoring Agent (MMA) en de afhankelijkheid-agent op elke virtuele machine die u wilt evalueren.
4. Kopieer de werkruimte-id en -sleutel. U moet deze als u de MMA op de on-premises machine installeren.

### <a name="install-the-mma"></a>De MMA installeren

De agent installeren op een Windows-computer:

1. Dubbelklik op de gedownloade agent.
2. Klik op de pagina **Welkom** op **Volgende**. Klik op de pagina **Licentievoorwaarden** op **Akkoord** om de licentie te accepteren.
3. In **doelmap**, behouden of wijzigen van de standaardinstallatiemap > **volgende**. 
4. In **installatieopties voor Agent**, selecteer **Azure Log Analytics** > **volgende**. 
5. Klik op **toevoegen** een nieuwe werkruimte voor logboekanalyse toevoegen. In de werkruimte-ID en sleutel die u hebt gekopieerd uit de portal te plakken. Klik op **Volgende**.


De agent te installeren op een Linux-machine:

1. De juiste bundel (x86 of x64) overbrengen naar de Linux-computer via scp/sftp.
2. De bundel installeren met behulp van het--installatie-argument.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>De afhankelijkheidsagent installeren
1. De afhankelijkheid om agent te installeren op een Windows-machine, dubbelklik op het setup-bestand en volg de wizard.
2. U installeert de agent voor afhankelijkheden op een Linux-machine te installeren als toegangspunt met de volgende opdracht:

    ```sh InstallDependencyAgent-Linux64.bin```

[Meer informatie](../monitoring/monitoring-service-map-configure.md#supported-operating-systems) over besturingssystemen die worden ondersteund door de agent voor afhankelijkheden. 

## <a name="create-a-group"></a>Groep maken

1. Nadat u de agents hebt geïnstalleerd, gaat u naar de portal en klik op **beheren** > **Machines**.
2. Zoeken naar de computer waarop u de agents hebt geïnstalleerd.
3. De **afhankelijkheden** kolom voor de machine wordt nu weergegeven als **afhankelijkheden weergeven**. Klik op de kolom om weer te geven van de afhankelijkheden van de machine.
4. De afhankelijkheidskaart voor de machine ziet u de volgende details:
    - (Clients) binnenkomende en uitgaande (Servers) TCP-verbindingen naar/van de machine
        - De afhankelijke machines die geen van de geïnstalleerde agent van MMA- en afhankelijkheidsmonitors worden gegroepeerd per poortnummers
        - De dependenct machines waarvoor de MMA en de afhankelijkheid agent is geïnstalleerd, worden weergegeven als afzonderlijke vakken 
    - Processen die worden uitgevoerd op de machine, kunt u elk machine vak om weer te geven van de processen uitbreiden
    - Eigenschappen zoals het volledig gekwalificeerde domeinnaam, besturingssysteem, enzovoort voor MAC-adres van elke machine die u kunt klikken op elke machine vak om deze gegevens weer te geven

 ![Machineafhankelijkheden weergeven](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. U kunt zoeken op afhankelijkheden verschillende tijdsduren door te klikken op de tijd in het bereik tijdlabel. Het bereik is standaard een uur. U kunt het tijdsbereik wijzigen of geef de begin- en einddatum en duur.
5. Nadat u hebt aangegeven dat afhankelijke machines die u wilt groeperen, gebruik Ctrl + klik om te selecteren van meerdere machines op de kaart en klikt u op **machines te groeperen**.
6. Geef een groepsnaam. Controleer of dat de afhankelijke machines worden gedetecteerd door Azure migreren. 

    > [!NOTE]
    > Als een afhankelijke machine niet wordt gedetecteerd door Azure migreren, u het niet toevoegen aan de groep. Dergelijke machines toevoegen aan de groep, moet u het detectieproces opnieuw uitvoeren met het juiste bereik in vCenter-Server en zorg ervoor dat de machine is gedetecteerd door Azure migreren.  

7. Als u maken van een beoordeling voor deze groep wilt, schakel het selectievakje voor het maken van een nieuwe beoordeling van de groep.
8. Klik op **OK** om op te slaan van de groep.

Zodra de groep is gemaakt, is het raadzaam agents installeren op alle computers van de groep en het verfijnen van de groep met de afhankelijkheid van de hele groep te visualiseren.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over hoe](how-to-create-group-dependencies.md) voor het verfijnen van de groep met groepsafhankelijkheden te visualiseren
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
