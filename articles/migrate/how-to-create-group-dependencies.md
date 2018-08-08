---
title: Een evaluatie van de groep met afhankelijkheidstoewijzing groep in Azure Migrate verfijnen | Microsoft Docs
description: Beschrijft hoe u een evaluatie met afhankelijkheidstoewijzing groep in de service Azure Migrate verfijnen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 37c4ce8638c8f0481151449317d6cd387b61b256
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622895"
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Een groep met afhankelijkheidstoewijzing groep verfijnen

In dit artikel wordt beschreven hoe u een groep verfijnen door te visualiseren afhankelijkheden van alle computers in de groep. Doorgaans gebruikt u deze methode als u lidmaatschap voor een bestaande groep verfijnen op groepsafhankelijkheden cross controleren wilt voordat u een evaluatie uitvoert. Een groep met visualisatie van afhankelijkheden verfijnen kunt help effectief plannen van uw migratie naar Azure.You kan detecteren alle onderling afhankelijke systemen die moeten tegelijk migreren. Hiermee kunt u ervoor zorgen dat er niets is overgebleven en onverwachte storingen niet wordt uitgevoerd wanneer u naar Azure migreert. 


> [!NOTE]
> Groepen waarvoor u wenst te visualiseren afhankelijkheden mag niet meer dan 10 machines bevatten. Als u meer dan 10 machines in de groep hebt, raden wij u aan deze opsplitsen in kleinere groepen gebruikmaken van de functie voor visualisatie van afhankelijkheden.


# <a name="prepare-the-group-for-dependency-visualization"></a>Voorbereiden van de groep voor visualisatie van afhankelijkheden
Als u afhankelijkheden van een groep, die u wilt downloaden en installeren van agents op elke on-premises computer die deel uitmaakt van de groep. Bovendien, als u computers geen verbinding met internet hebt, moet u om te downloaden en installeren [OMS-gateway](../log-analytics/log-analytics-oms-gateway.md) hierop.

### <a name="download-and-install-the-vm-agents"></a>De VM-agents downloaden en installeren
1. In **overzicht**, klikt u op **beheren** > **groepen**, gaat u naar de vereiste groep.
2. In de lijst met computers in de **agent voor afhankelijkheden** kolom, klikt u op **vereist installatie** om te zien van instructies over het downloaden en installeren van de agents.
3. Op de **afhankelijkheden** downloaden en installeren van Microsoft Monitoring Agent (MMA) en de agent voor afhankelijkheden op elke virtuele machine die deel uitmaakt van de groep.
4. Kopieer de werkruimte-id en -sleutel. U hebt deze nodig wanneer u de MMA op de on-premises computers installeren.

### <a name="install-the-mma"></a>De MMA installeren

De agent installeren op een Windows-machine:

1. Dubbelklik op de gedownloade agent.
2. Klik op de pagina **Welkom** op **Volgende**. Klik op de pagina **Licentievoorwaarden** op **Akkoord** om de licentie te accepteren.
3. In **doelmap**, houden of wijzigen van de standaardinstallatiemap > **volgende**. 
4. In **installatieopties voor Agent**, selecteer **Azure Log Analytics** > **volgende**. 
5. Klik op **toevoegen** om toe te voegen een nieuwe Log Analytics-werkruimte. Plak in de werkruimte-ID en de sleutel die u hebt gekopieerd uit de portal. Klik op **Volgende**.


De agent installeren op een Linux-machine:

1. De geschikte bundel (x86 of x64) overbrengen naar uw Linux-computer met scp/sftp.
2. De bundel installeren met behulp van de--install-argument.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>De afhankelijkheidsagent installeren
1. Voor het installeren van de agent voor afhankelijkheden op een Windows-machine, dubbelklik op het setup-bestand en volg de wizard.
2. Voor het installeren van de agent voor afhankelijkheden op een Linux-machine, installeert u toegangspunt met de volgende opdracht:

    ```sh InstallDependencyAgent-Linux64.bin```

Meer informatie over de ondersteuning van de agent afhankelijkheid voor de [Windows](../monitoring/monitoring-service-map-configure.md#supported-windows-operating-systems) en [Linux](../monitoring/monitoring-service-map-configure.md#supported-linux-operating-systems) besturingssystemen.

## <a name="refine-the-group-based-on-dependency-visualization"></a>De groep op basis van de visualisatie van afhankelijkheden verfijnen
Wanneer u agents hebt geïnstalleerd op alle computers van de groep, kunt u de afhankelijkheden van de groep visualiseren en Verfijn deze door de onderstaande stappen.

1. In de Azure Migrate-project, onder **beheren**, klikt u op **groepen**, en selecteer de groep.
2. Klik op de pagina **afhankelijkheden weergeven**, om te openen van de afhankelijkheidskaart van de groep.
3. De kaart van afhankelijkheden voor de groep bevat de volgende gegevens:
    - Inkomende (Clients) en uitgaande (Server) TCP-verbindingen naar/van de machines die deel uitmaken van de groep
        - De afhankelijke machines waarop geen de MMA- en afhankelijkheidsmonitors agent is geïnstalleerd zijn gegroepeerd op poortnummers
        - De dependenct-machines waarvoor de MMA en de agent voor afhankelijkheden geïnstalleerd worden als afzonderlijke vakken weergegeven 
    - Processen die op de machine, kunt u elk VM-vak om de processen weer te geven uitbreiden
    - Eigenschappen, zoals Fully Qualified Domain Name, besturingssysteem, enzovoort van MAC-adres van elke machine die u kunt klikken op elke VM-vak om deze gegevens weer te geven

     ![Groepsafhankelijkheden weergeven](./media/how-to-create-group-dependencies/view-group-dependencies.png)

3. Als u gedetailleerdere afhankelijkheden, klikt u op het tijdsbereik om dit te wijzigen. Het bereik is standaard een uur. U kunt het tijdsbereik wijzigen of geef de begin- en einddatums en duur.
4. Controleer of de afhankelijke machines het proces wordt uitgevoerd op elke machine en de machines identificeren die moeten worden toegevoegd of verwijderd uit de groep.
5. Gebruik Ctrl + klik Selecteer machines op de kaart toevoegen of verwijderen uit de groep.
    - U kunt alleen machines die zijn gedetecteerd toevoegen.
    - Ongeldig voorbij beoordelingen voor het toevoegen en verwijderen van computers uit een groep maken.
    - U kunt eventueel ook een nieuwe evaluatie maken wanneer u de groep wijzigen.
5. Klik op **OK** om op te slaan van de groep.

    ![Toevoegen of verwijderen van machines](./media/how-to-create-group-dependencies/add-remove.png)

Als u wilt controleren van de afhankelijkheden van een specifieke computer die wordt weergegeven in de afhankelijkheidskaart groep [afhankelijkheidstoewijzing machine instellen](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
