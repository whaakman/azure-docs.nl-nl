---
title: Bijhouden van wijzigingen in uw virtuele machines in Azure | Microsoft Docs
description: Gebruik bijhouden van wijzigingen voor het bijhouden van wijzigingen in de bestanden en het register op uw virtuele machines.
services: automation
documentationcenter: automation
author: georgewallace
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: gwallace
ms.custom: 
ms.openlocfilehash: 3a661fada2e768c2206183c125593d019b557c1d
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="track-changes-in-your-azure-virtual-machines"></a>Bijhouden van wijzigingen in uw virtuele machines in Azure

Door wijzigingen bijhouden inschakelen, kunt u wijzigingen bijhouden op bestanden en registersleutels van Windows op uw virtuele machines. Wijzigingen in de configuratie te identificeren, kunt u operationele problemen te lokaliseren.

U kunt bijhouden van wijzigingen rechtstreeks vanuit uw virtuele machine van Azure inschakelen.

Als u geen Azure een virtuele machine, kunt u een door de instructies in de [Windows-snelstartgids](../virtual-machines/windows/quick-create-portal.md) of [Linux Quick Start](../virtual-machines/linux/quick-create-portal.md) artikel.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="enable-change-tracking-for-an-azure-virtual-machine"></a>Bijhouden van wijzigingen voor een virtuele machine van Azure inschakelen

1. Selecteer in het linkerdeelvenster van de Azure-portal de optie **Virtuele machines**.
2. Selecteer een virtuele machine in de lijst.
3. In de virtuele machine venster onder **Operations**, selecteer **bijhouden van wijzigingen**. 

   ![Bijhouden van wijzigingen vrijgeven vm](./media/automation-vm-change-tracking/change-onboard-vm-blade.png)  
    De **updatebeheer inschakelen** venster wordt geopend.

    Validatie is uitgevoerd om te bepalen of het bijhouden van wijzigingen is ingeschakeld voor deze virtuele machine. Als het bijhouden van wijzigingen niet is ingeschakeld, wordt een banner weergegeven, zodat u de optie voor het inschakelen van de oplossing.

   ![Wijzigingen bijhouden vrijgeven configuratie banner](./media/automation-vm-change-tracking/change-onboard-banner.png)

4. Selecteer de banner om de oplossing in te schakelen. Als u niet de volgende items hebt, worden ze automatisch toegevoegd:

   * [Meld u Analytics](../log-analytics/log-analytics-overview.md) werkruimte
   * [Automation](../automation/automation-offering-get-started.md) account

5. Selecteer een Log Analytics-werkruimte wilt gegevenslogboeken over het bijhouden van wijzigingen opslaan, selecteert u een Automation-account moet het bijhouden van wijzigingen en selecteer vervolgens **inschakelen**.  
    In een statusbalk ziet u de melding dat de oplossing wordt ingeschakeld. Dit proces duurt maximaal 15 minuten.

## <a name="configure-change-tracking"></a>Wijzigingen bijhouden configureren

Nadat wijzigingen bijhouden is ingeschakeld, de **bijhouden** venster wordt weergegeven. 

Selecteer om te selecteren welke bestanden en registersleutels om bij te houden, **instellingen bewerken**.

   ![Instellingen bewerken voor het bijhouden](./media/automation-vm-change-tracking/change-edit-settings.png)

   De **configuratie van de werkruimte** venster wordt geopend. 

In de **configuratie van de werkruimte** venster toevoegen van de Windows-registersleutels, bestanden van Windows of Linux-bestanden worden bijgehouden, zoals wordt beschreven in de volgende drie secties.

### <a name="add-a-windows-registry-key"></a>Een Windows-registersleutel toevoegen

1. Op de **Windows-register** tabblad **toevoegen**.  
    De **Windows-register toevoegen voor het bijhouden** venster wordt geopend.

   ![Wijzigingen bijhouden toevoegen register](./media/automation-vm-change-tracking/change-add-registry.png)

2. Onder **ingeschakeld**, selecteer **True**.
3. In de **itemnaam** Voer een beschrijvende naam.
4. (Optioneel) In de **groep** Voer een groepsnaam.
5. In de **Windows-registersleutel** Voer de naam van de registersleutel die u wilt traceren.
6. Selecteer **Opslaan**.

### <a name="add-a-windows-file"></a>Een Windows-bestand toevoegen

1. Op de **Windows-bestanden** tabblad **toevoegen**.  
    De **Windows-bestand toevoegen voor het bijhouden** venster wordt geopend.

   ![Bijhouden van toevoegen Windows-bestand](./media/automation-vm-change-tracking/change-add-win-file.png)

2. Onder **ingeschakeld**, selecteer **True**.
3. In de **itemnaam** Voer een beschrijvende naam.
4. (Optioneel) In de **groep** Voer een groepsnaam.
5. In de **pad Voer** Geef het volledige pad en de naam van het bestand dat u wilt traceren.
6. Selecteer **Opslaan**.

### <a name="add-a-linux-file"></a>Een Linux-bestand toevoegen

1. Op de **Linux bestanden** tabblad **toevoegen**.  
    De **Linux-bestand toevoegen voor het bijhouden** venster wordt geopend.

   ![Bijhouden van toevoegen Linux-bestand](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. Onder **ingeschakeld**, selecteer **True**.
3. In de **itemnaam** Voer een beschrijvende naam.
4. (Optioneel) In de **groep** Voer een groepsnaam.
5. In de **pad Voer** Geef het volledige pad en de naam van het bestand dat u wilt traceren.
6. In de **padtype** selecteert u **bestand** of **Directory**.
7. Onder **recursie**, wilt bijhouden van wijzigingen voor het opgegeven pad en alle bestanden en paden eronder, selecteert u **op**. Als u wilt bijhouden alleen de geselecteerde pad of het bestand, selecteer **uit**.
8. Onder **gebruik Sudo**bij te houden van bestanden die moeten worden de `sudo` opdracht voor access, selecteer **op**. Selecteer anders **uit**.
9. Selecteer **Opslaan**.

## <a name="view-changes"></a>Wijzigingen weergeven

In de **bijhouden** venster vindt u de wijzigingen in elk van de verschillende categorieën voor uw virtuele machine na verloop van tijd.

   ![Bijhouden van toevoegen wijzigingen van de weergave](./media/automation-vm-change-tracking/change-view-changes.png)

U kunt de categorieën en het tijdsbereik van wijzigingen om weer te geven. U kunt een grafische weergave van wijzigingen na verloop van tijd weergeven aan de bovenkant van het venster. Aan de onderkant van het venster kunt u een lijst met recente wijzigingen zien.

## <a name="view-change-tracking-log-data"></a>Wijzigingen bijhouden logboekgegevens weergeven

Wijzigingen bijhouden genereert logboekgegevens die worden verzonden met logboekanalyse. Selecteer om te zoeken naar de logboeken door het uitvoeren van query's, **logboekanalyse** boven aan de **bijhouden** venster.

   ![Bijhouden van logboekanalyse](./media/automation-vm-change-tracking/change-log-analytics.png)

Zie voor meer informatie over het uitvoeren en zoeken van de logboekbestanden in Log Analytics, [logboekanalyse](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het bijhouden van wijzigingen, [bijhouden van wijzigingen in de software in uw omgeving met de oplossing wijzigen bijhouden](../log-analytics/log-analytics-change-tracking.md).
* Zie voor meer informatie over het beheren van updates voor uw virtuele machines, [Update beheeroplossing in OMS](../operations-management-suite/oms-solution-update-management.md).
