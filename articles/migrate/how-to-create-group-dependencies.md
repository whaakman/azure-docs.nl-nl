---
title: Een evaluatie-groep met apparaatgroeptoewijzing van afhankelijkheid in Azure migreren verfijnen | Microsoft Docs
description: Beschrijft hoe een beoordeling in de service Azure migreren met behulp van afhankelijkheid apparaatgroeptoewijzing verfijnen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/22/2017
ms.author: raynew
ms.openlocfilehash: 3b10765894501791004e3a9221363f196cc0c91d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Een groep met apparaatgroeptoewijzing afhankelijkheid verfijnen

In dit artikel wordt beschreven hoe een groep verfijnen door het visualiseren van afhankelijkheden van alle machines in de groep. Gebruik deze methode doorgaans in als u lidmaatschap voor een bestaande groep verfijnen door groepsafhankelijkheden tussen verschillende controleren wilt voordat u een beoordeling uitvoert. Een groep met behulp van afhankelijkheid visualisatie verfijnen kan helpt u de migratie naar Azure.You effectief plannen kunt detecteren alle onderling afhankelijk systemen die nodig zijn voor het migreren van elkaar. Kunt u ervoor zorgen dat er niets is achtergelaten en onverwachte storingen niet bijgewerkt wanneer u naar Azure migreert. 


> [!NOTE]
> Groepen waarvoor u wilt visualiseren afhankelijkheden mag niet meer dan 10 machines bevatten. Als er meer dan 10 machines in de groep, wordt aanbevolen om te splitsen in kleinere groepen gebruikmaken van de functionaliteit van de visualisatie afhankelijkheid.


# <a name="prepare-the-group-for-dependency-visualization"></a>Voorbereiden van de groep voor afhankelijkheid visualisatie
Als u wilt weergeven van afhankelijkheden van een groep, die u wilt downloaden en installeren van agents op elke on-premises machine die deel uitmaakt van de groep. Bovendien hebt u machines zonder internetverbinding, u moet downloaden en installeren [OMS gateway](../log-analytics/log-analytics-oms-gateway.md) erop.

### <a name="download-and-install-the-vm-agents"></a>Download en installeer de VM-agents
1. In **overzicht**, klikt u op **beheren** > **groepen**, gaat u naar de vereiste groep.
2. In de lijst met computers in de **-agent voor afhankelijkheden** kolom, klikt u op **vereist installatie** voor instructies over het downloaden en installeren van de agents.
3. Op de **afhankelijkheden** pagina, downloaden en installeren van Microsoft Monitoring Agent (MMA) en de afhankelijkheid-agent op elke virtuele machine die deel uitmaakt van de groep.
4. Kopieer de werkruimte-ID en sleutel. U moet deze wanneer u de MMA op de lokale computers installeert.

### <a name="install-the-mma"></a>Installeer de MMA

De agent installeren op een Windows-computer:

1. Dubbelklik op de gedownloade agent.
2. Klik op de pagina **Welkom** op **Volgende**. Op de **licentievoorwaarden** pagina, klikt u op **ik ga akkoord** de licentievoorwaarden accepteren.
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

## <a name="refine-the-group-based-on-dependency-visualization"></a>De groep op basis van afhankelijkheid visualisatie verfijnen
Als u agents hebt geïnstalleerd op alle computers van de groep, kunt u de afhankelijkheden van de groep visualiseren en deze verfijnen door de onderstaande stappen te volgen.

1. Migreren in de Azure-project, onder **beheren**, klikt u op **groepen**, en selecteer de groep.
2. Klik op de pagina groep **afhankelijkheden weergeven**, om te openen van de groep afhankelijkheid-kaart.
3. De afhankelijkheidskaart voor de groep bevat de volgende details:
    - (Clients) binnenkomende en uitgaande (Servers) TCP-verbindingen naar/van de machines die deel uitmaken van de groep
        - De afhankelijke machines die geen van de geïnstalleerde agent van MMA- en afhankelijkheidsmonitors worden gegroepeerd per poortnummers
        - De dependenct machines waarvoor de MMA en de afhankelijkheid agent is geïnstalleerd, worden weergegeven als afzonderlijke vakken 
    - Processen die worden uitgevoerd op de machine, kunt u elk machine vak om weer te geven van de processen uitbreiden
    - Eigenschappen zoals het volledig gekwalificeerde domeinnaam, besturingssysteem, enzovoort voor MAC-adres van elke machine die u kunt klikken op elke machine vak om deze gegevens weer te geven

     ![Afhankelijkheden van de groep weergeven](./media/how-to-create-group-dependencies/view-group-dependencies.png)

3. Als u wilt meer gedetailleerd afhankelijkheden weergeven, klikt u op het tijdsbereik om dit te wijzigen. Het bereik is standaard een uur. U kunt het tijdsbereik wijzigen of geef de begin- en einddatum en duur.
4. Controleer of de afhankelijke machines het proces op elke machine en de machines die moeten worden toegevoegd of verwijderd uit de groep te identificeren.
5. Gebruik Ctrl + klik machines op de kaart toevoegen of verwijderen uit de groep selecteren.
    - U kunt alleen machines die zijn gedetecteerd toevoegen.
    - Ongeldig voorbij beoordelingen voor het toevoegen en verwijderen van computers uit een groep maken.
    - U kunt eventueel ook een nieuwe beoordeling maken wanneer u de groep wijzigt.
5. Klik op **OK** om op te slaan van de groep.

    ![Toevoegen of verwijderen van computers](./media/how-to-create-group-dependencies/add-remove.png)

Als u wilt controleren van de afhankelijkheden van een specifieke machine die wordt weergegeven in de groep afhankelijkheidskaart [machine afhankelijkheid toewijzing instellen](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen zijn berekend.
