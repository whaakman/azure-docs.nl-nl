---
title: Groep machines met behulp van machineafhankelijkheden met Azure Migrate | Microsoft Docs
description: Beschrijft hoe u een evaluatie met behulp van machineafhankelijkheden met de service Azure Migrate maakt.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 09/21/2018
ms.author: raynew
ms.openlocfilehash: 2755cc4e8e0e5a1b2a0e491b00fc73530dd9b958
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635676"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Machines groeperen met behulp van machine-afhankelijkheidstoewijzing

In dit artikel wordt beschreven hoe u een groep machines voor [Azure Migrate](migrate-overview.md) beoordelen door te visualiseren afhankelijkheden van machines. Doorgaans gebruikt u deze methode als u wilt beoordelen van groepen van virtuele machines met een hogere mate van vertrouwen door machineafhankelijkheden cross controleren voordat u een evaluatie uitvoert. Visualisatie van afhankelijkheden kunt u effectief plan uw migratie naar Azure. Hiermee kunt u ervoor zorgen dat er niets is overgebleven en onverwachte storingen niet wordt uitgevoerd wanneer u naar Azure migreert. U kunt alle onderling afhankelijk zijn systemen die tegelijk migreren en bepalen of een actief systeem gebruikers nog steeds bedient of een kandidaat is voor het buiten gebruik stellen in plaats van de migratie moeten detecteren.


## <a name="prepare-for-dependency-visualization"></a>Voorbereiden voor visualisatie van afhankelijkheden
Azure Migrate maakt gebruik van de oplossing Serviceoverzicht in Log Analytics om in te schakelen visualisatie van afhankelijkheden van machines.

### <a name="associate-a-log-analytics-workspace"></a>Een Log Analytics-werkruimte koppelen
Als u wilt gebruikmaken van de visualisatie van afhankelijkheden, moet u een Log Analytics-werkruimte, nieuwe of bestaande koppelen met een Azure Migrate-project. U kunt alleen maken of koppelen van een werkruimte in hetzelfde abonnement waar het migratieproject is gemaakt.

- Een Log Analytics-werkruimte koppelen aan een project in **overzicht**, gaat u naar **Essentials** sectie van het project, klikt u op **configuratie is vereist**

    ![Log Analytics-werkruimte koppelen](./media/concepts-dependency-visualization/associate-workspace.png)

- Wanneer u een nieuwe werkruimte maakt, moet u een naam opgeven voor de werkruimte. De werkruimte wordt vervolgens gemaakt in hetzelfde abonnement als de migration-project en in een regio in dezelfde [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) als het migratieproject.
- De **gebruik bestaande** optie een lijst met alleen deze werkruimten die zijn gemaakt in regio's waar Serviceoverzicht beschikbaar is. Hebt u een werkruimte in een regio waar Serviceoverzicht niet beschikbaar is, wordt deze niet weergegeven in de vervolgkeuzelijst.

> [!NOTE]
> U kunt de werkruimte die is gekoppeld aan een migratieproject niet wijzigen.

### <a name="download-and-install-the-vm-agents"></a>De VM-agents downloaden en installeren
Wanneer u een werkruimte configureert, moet u om te downloaden en installeren van agents op elke on-premises computer die u wilt evalueren. Bovendien, als u computers geen verbinding met internet hebt, moet u om te downloaden en installeren [Log Analytics gateway](../azure-monitor/platform/gateway.md) hierop.

1. In **overzicht**, klikt u op **beheren** > **Machines**, en selecteer de vereiste machine.
2. In de **afhankelijkheden** kolom, klikt u op **agents installeren**.
3. Op de **afhankelijkheden** pagina, downloaden en installeren van de Microsoft Monitoring Agent (MMA) en de agent voor afhankelijkheden op elke virtuele machine die u wilt evalueren.
4. Kopieer de werkruimte-id en -sleutel. U hebt deze nodig wanneer u de MMA op de on-premises machine installeren.

> [!NOTE]
> Voor het automatiseren van de installatie van agents kunt u een implementatie-hulpprogramma, zoals System Center Configuration Manager gebruiken of gebruik onze hulpprogramma partner [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), waarvoor een implementatieoplossing agent voor Azure Migrate.

### <a name="install-the-mma"></a>De MMA installeren

De agent installeren op een Windows-machine:

1. Dubbelklik op de gedownloade agent.
2. Klik op de pagina **Welkom** op **Volgende**. Klik op de pagina **Licentievoorwaarden** op **Akkoord** om de licentie te accepteren.
3. In **doelmap**, houden of wijzigen van de standaardinstallatiemap > **volgende**.
4. In **installatieopties voor Agent**, selecteer **Azure Log Analytics** > **volgende**.
5. Klik op **toevoegen** om toe te voegen een nieuwe Log Analytics-werkruimte. Plak in de werkruimte-ID en de sleutel die u hebt gekopieerd uit de portal. Klik op **Volgende**.

[Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) over de lijst met ondersteuning voor Windows-besturingssystemen door MMA.

De agent installeren op een Linux-machine:

1. De geschikte bundel (x86 of x64) overbrengen naar uw Linux-computer met scp/sftp.
2. De bundel installeren met behulp van de--install-argument.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) over de lijst met ondersteuning voor Linux-besturingssystemen door MMA.

### <a name="install-the-dependency-agent"></a>De afhankelijkheidsagent installeren
1. Voor het installeren van de agent voor afhankelijkheden op een Windows-machine, dubbelklik op het setup-bestand en volg de wizard.
2. Voor het installeren van de agent voor afhankelijkheden op een Linux-machine, installeert u toegangspunt met de volgende opdracht:

    ```sh InstallDependencyAgent-Linux64.bin```

Meer informatie over de ondersteuning van de agent afhankelijkheid voor de [Windows](../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems) en [Linux](../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems) besturingssystemen.

[Meer informatie](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) over hoe u scripts gebruiken kunt voor het installeren van de agent voor afhankelijkheden.

## <a name="create-a-group"></a>Een groep maken

1. Nadat u de agents hebt geïnstalleerd, gaat u naar de portal en klik op **beheren** > **Machines**.
2. Zoeken naar de machine waarop u de agents hebt geïnstalleerd.
3. De **afhankelijkheden** kolom voor de machine moet nu worden weergegeven als **afhankelijkheden weergeven**. Klik op de kolom om de afhankelijkheden van de machine weer te geven.
4. De afhankelijkheidskaart voor de machine wordt de volgende gegevens:
    - Inkomende (Clients) en uitgaande (Server) TCP-verbindingen naar/van de machine
        - De afhankelijke machines waarop geen de MMA- en afhankelijkheidsmonitors agent is geïnstalleerd zijn gegroepeerd op poortnummers
        - De afhankelijke machines waarvoor de MMA en de agent voor afhankelijkheden geïnstalleerd worden als afzonderlijke vakken weergegeven
    - Processen die op de machine, kunt u elk VM-vak om de processen weer te geven uitbreiden
    - Eigenschappen, zoals Fully Qualified Domain Name, besturingssysteem, enzovoort van MAC-adres van elke machine die u kunt klikken op elke VM-vak om deze gegevens weer te geven

 ![Machineafhankelijkheden weergeven](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. U kunt zoeken op afhankelijkheden verschillende tijdsduren door te klikken op de duur in het bereik tijdlabel. Het bereik is standaard een uur. U kunt het tijdsbereik wijzigen of geef de begin- en einddatums en duur.
5. Nadat u hebt vastgesteld dat afhankelijke machines die u wilt groeperen, gebruikt u Ctrl + klikken om te selecteren van meerdere virtuele machines op de kaart en klikt u op **machines groeperen**.
6. Geef een groepsnaam op. Controleer of dat de afhankelijke machines zijn gedetecteerd door Azure Migrate.

    > [!NOTE]
    > Als een afhankelijke virtuele machine wordt niet gedetecteerd door Azure Migrate, kunt u deze niet toevoegen aan de groep. Als u wilt deze computers toevoegen aan de groep, moet u het detectieproces opnieuw uitvoeren met het juiste bereik in de vCenter-Server en zorg ervoor dat de machine is gedetecteerd door Azure Migrate.  

7. Als u wilt om een evaluatie voor deze groep te maken, selecteert u het selectievakje in om een nieuwe evaluatie voor de groep te maken.
8. Klik op **OK** om op te slaan van de groep.

Nadat de groep is gemaakt, is het aanbevolen om agents te installeren op alle computers van de groep en de groep verfijnen door te visualiseren van de afhankelijkheid van de hele groep.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) over de veelgestelde vragen over de visualisatie van afhankelijkheden.
- [Informatie over hoe](how-to-create-group-dependencies.md) aan de groep verfijnen door groepsafhankelijkheden visualiseren.
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
