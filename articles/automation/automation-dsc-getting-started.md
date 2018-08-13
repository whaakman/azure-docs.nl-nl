---
title: Aan de slag met Azure Automation State Configuration
description: Uitleg over en voorbeelden van de meest algemene taken in Azure Automation State Configuration (DSC)
services: automation
ms.service: automation
ms.component: dsc
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9a18855d11c0b367b7d58ffb0f4c62e752c05b89
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40004229"
---
# <a name="getting-started-with-azure-automation-state-configuration"></a>Aan de slag met Azure Automation State Configuration

In dit artikel wordt uitgelegd hoe u de meest algemene taken uitvoeren met Azure Automation State Configuration, zoals het maken, importeren, en -configuraties, onboarding van machines te beheren, compileren en weergeven van rapporten. Zie voor een overzicht van de configuratie van de status van de Azure Automation wordt [overzicht van Azure Automation-configuratie](automation-dsc-overview.md). Zie voor documentatie voor Desired State Configuration (DSC), [Windows PowerShell Desired State Configuration Overview](/powershell/dsc/overview).

Dit artikel bevat een stapsgewijze handleiding voor het gebruik van Azure Automation State Configuration. Als u een Voorbeeldomgeving die al is ingesteld wilt zonder dat u de stappen in dit artikel wordt beschreven, kunt u de volgende Resource Manager-sjabloon: [Azure Automation beheerd knooppunt sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-automation-configuration). Deze sjabloon stelt u een voltooide configuratie van Azure Automation-status-omgeving, met inbegrip van een Azure-VM die wordt beheerd door Azure Automation State Configuration.

## <a name="prerequisites"></a>Vereisten

Het volgende is vereist voor het voltooien van de voorbeelden in dit artikel:

- Een Azure Automation-account. Zie [Azure Uitvoeren-als-account](automation-sec-configure-azure-runas-account.md) voor instructies over het maken van een Azure Automation Uitvoeren-als-account.
- Een Azure Resource Manager VM (niet klassiek) met Windows Server 2008 R2 of hoger. Zie [Uw eerste virtuele Windows-machine maken met behulp van Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor instructies voor het maken van een VM

## <a name="creating-a-dsc-configuration"></a>Het maken van een DSC-configuratie

U maakt een eenvoudige [DSC-configuratie](/powershell/dsc/configurations) die zorgt ervoor dat de aanwezigheid of afwezigheid van de **-webserver** Windows functie (IIS), afhankelijk van hoe u knooppunten wilt toewijzen.

1. Start [VSCode](https://code.visualstudio.com/docs) (of een teksteditor).
1. Typ de volgende tekst:

    ```powershell
    configuration TestConfig
    {
        Node IsWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
            }
        }

        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
            }
        }
    }
    ```
1. Sla het bestand op als `TestConfig.ps1`.

Deze configuratie een resource in elk blok knooppunt roept de [WindowsFeature-resource](/powershell/dsc/windowsfeatureresource), die zorgt ervoor dat de aanwezigheid of afwezigheid van de **-webserver** functie.

## <a name="importing-a-configuration-into-azure-automation"></a>Een configuratie importeren in Azure Automation

Vervolgens importeert u de configuratie in het Automation-account.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** weergeeft, schakelt **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **configuraties** tabblad en klik vervolgens op **+ toevoegen**.
1. Op de **configuratie importeren** pagina, blader naar de `TestConfig.ps1` bestand op uw computer.

   ![Schermafbeelding van de ** importeren configuratie ** blade](./media/automation-dsc-getting-started/AddConfig.png)

1. Klik op **OK**.

## <a name="viewing-a-configuration-in-azure-automation"></a>Een configuratie weergeven in Azure Automation

Nadat u een configuratie hebt geïmporteerd, kunt u deze bekijken in Azure portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** weergeeft, schakelt **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **configuraties** tabblad en klik vervolgens op **TestConfig** (dit is de naam van de configuratie die u hebt geïmporteerd in de vorige procedure).
1. Op de **TestConfig configuratie** pagina, klikt u op **configuratiebron weergeven**.

   ![Schermafbeelding van de blade van de configuratie van TestConfig](./media/automation-dsc-getting-started/ViewConfigSource.png)

   Een **TestConfig configuratiebron** pagina geopend waarin de PowerShell-code voor de configuratie.

## <a name="compiling-a-configuration-in-azure-automation"></a>Compileren van een configuratie in Azure Automation

Voordat u een gewenste status op een knooppunt toepassen kunt, moet een DSC-configuratie definiëren die status worden gecompileerd tot een of meer knooppuntconfiguraties (MOF-document) en op de Automation DSC Pull-Server geplaatst. Zie voor een gedetailleerde beschrijving van de configuraties in Azure Automation-staat configuratie compileren [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md).
Zie voor meer informatie over het compileren van configuraties [DSC-configuraties](/powershell/dsc/configurations).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** pagina, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **configuraties** tabblad en klik vervolgens op **TestConfig** (de naam van de configuratie van de eerder geïmporteerd).
1. Op de **TestConfig configuratie** pagina, klikt u op **compileren**, en klik vervolgens op **Ja**. Hiermee start u een Compilatietaak.

   ![Schermafbeelding van de configuratiepagina van de TestConfig compileren knop markeren](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Wanneer u een configuratie in Azure Automation compileren, worden automatisch alle gemaakte knooppuntconfiguratie MOF-bestanden naar de pull-server implementeert.

## <a name="viewing-a-compilation-job"></a>Een Compilatietaak weergeven

Nadat u een compilatie start, kunt u het weergeven in de **compilatietaken** tegel de **configuratie** pagina. De **compilatietaken** tegel op dat moment wordt uitgevoerd, is voltooid en mislukte taken. Wanneer u een pagina van de taak compilatie opent, ziet u informatie over deze taak met inbegrip van eventuele fouten of waarschuwingen opgetreden, invoerparameters die wordt gebruikt in de configuratie en de compilatie Logboeken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** pagina, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **configuraties** tabblad en klik vervolgens op **TestConfig** (de naam van de configuratie van de eerder geïmporteerd).
1. Onder **compilatietaken**, selecteert u de Compilatietaak die u wilt weergeven. Een **Compilatietaak** pagina wordt geopend met het label met de datum waarop de Compilatietaak is gestart.

   ![Schermafbeelding van de pagina Compilatietaak](./media/automation-dsc-getting-started/CompilationJob.png)

1. Klik op een willekeurige tegel in de **Compilatietaak** pagina om te bekijken om meer details over de taak.

## <a name="viewing-node-configurations"></a>Knooppuntconfiguraties weergeven

Voltooiing van een Compilatietaak maakt u een of meer nieuwe knooppuntconfiguraties. De configuratie van een knooppunt is een MOF-document dat is geïmplementeerd naar de pull-server en klaar om te worden opgehaald en toegepast door een of meer knooppunten. U kunt de knooppuntconfiguraties weergeven in uw Automation-account in de **State configuration (DSC)** pagina. De configuratie van een knooppunt heeft een naam met het formulier *ConfigurationName*.* Knooppuntnaam*.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** blade, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **gecompileerd configuraties** tabblad.

   ![Schermopname van het tabblad gecompileerd configuraties](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration"></a>Onboarding van een Azure-VM voor beheer met Azure Automation State Configuration

U kunt Azure Automation State Configuration gebruiken voor het beheren van Azure-VM's (zowel klassieke als Resource Manager), on-premises VM's, Linux machines, AWS-VM's en on-premises fysieke computers. In dit artikel leert u hoe u onboarding alleen Azure Resource Manager-VM's. Zie voor meer informatie over onboarding andere typen machines, [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-state-configuration"></a>Gebruik een Azure Resource Manager-VM voor beheer met Azure Automation State Configuration

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** blade, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, terwijl op de **knooppunten** tabblad **+ toevoegen**.

   ![Schermafbeelding van de DSC-knooppunten pagina markeren van de knop Azure-VM toevoegen](./media/automation-dsc-getting-started/OnboardVM.png)

1. Op de **virtuele Machines** pagina, selecteert u uw virtuele machine.
1. Op de **virtuele machine** details van de pagina, klikt u op **+ Connect**.

   > [!IMPORTANT]
   > Dit moet een Azure Resource Manager-VM met Windows Server 2008 R2 of hoger.

1. In de **registratie** pagina, selecteert u de naam van de configuratie van de knooppunten die u wilt toepassen op de virtuele machine in de **knooppuntconfiguratienaam** vak. Op dit moment een naam opgeven is optioneel. U kunt de toegewezen knooppuntconfiguratie nadat het knooppunt wijzigen.
   Controleer **knooppunt opnieuw opstarten indien nodig**, klikt u vervolgens op **OK**.

   ![Schermafbeelding van de registratie-blade](./media/automation-dsc-getting-started/RegisterVM.png)

   De configuratie van de knooppunten die u hebt opgegeven, worden toegepast op de virtuele machine met intervallen opgegeven door de **configuratiemodus**, en de virtuele machine wordt gecontroleerd op updates voor de knooppuntconfiguratie met intervallen opgegeven door de **vernieuwen Frequentie**. Zie voor meer informatie over het gebruik van deze waarden [de Local Configuration Manager configureren](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
1. In de **Azure-VM's toevoegen** blade, klikt u op **maken**.

Azure begint met het proces van het onboarding van de virtuele machine. Wanneer deze voltooid is, de virtuele machine wordt weergegeven in de **knooppunten** tabblad van de **State configuration (DSC)** pagina in het Automation-account.

## <a name="viewing-the-list-of-managed-nodes"></a>De lijst met beheerde knooppunten weergeven

U kunt de lijst weergeven met alle machines die zijn vrijgegeven voor beheer in uw Automation-account in de **knooppunten** tabblad van de **State configuration (DSC)** pagina.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** blade, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **knooppunten** tabblad.

## <a name="viewing-reports-for-managed-nodes"></a>Rapporten voor beheerde knooppunten weergeven

Telkens wanneer die Azure Automation State Configuration een consistentiecontrole uit op een beheerde knooppunt voert, verzendt het knooppunt een statusrapport terug naar de pull-server. U kunt deze rapporten kunt weergeven op de pagina voor dat knooppunt.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** blade, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **knooppunten** tabblad. Hier ziet u het overzicht van de status van de configuratie en de details voor elk knooppunt.

   ![Schermafbeelding van knooppunt-pagina](./media/automation-dsc-getting-started/NodesTab.png)

1. Terwijl op de **knooppunten** en klik op de record knooppunt om te openen met de rapportage. Klik op het rapport dat u wilt om extra reporting details weer te geven.

   ![Schermafbeelding van het tabblad rapport](./media/automation-dsc-getting-started/NodeReport.png)

Op de blade voor een bepaald rapport ziet u de volgende statusinformatie voor de bijbehorende consistentiecontrole:

- De rapportstatus, of het knooppunt 'Compatibele', de configuratie 'Mislukt', of het knooppunt is 'Niet-compatibel' (wanneer het knooppunt zich in **ApplyandMonitor** modus en de machine is niet in de gewenste status).
- De begintijd voor de consistentiecontrole.
- De totale runtime voor de consistentiecontrole.
- Het type van een consistentiecontrole uit.
- Eventuele fouten, met inbegrip van de code als een foutbericht weergegeven.
- Een DSC-resources die worden gebruikt in de configuratie en de status van elke resource (of het knooppunt zich in de gewenste status voor de resource), kunt u klikken op elke resource op meer gedetailleerde informatie voor die bron.
- De naam, het IP-adres en de configuratiemodus van het knooppunt.

U kunt ook klikken op **onbewerkt rapport weergeven** om te zien van de werkelijke gegevens waarmee het knooppunt wordt verzonden naar de server.
Zie voor meer informatie over het gebruik van die gegevens [met behulp van een DSC-rapportserver](/powershell/dsc/reportserver).

Het kan even wanneer een knooppunt geïmplementeerd is voordat de eerste rapport beschikbaar is. U moet mogelijk wacht u tot 30 minuten voor het eerste rapport nadat u onboarding een knooppunt.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Opnieuw toewijzen van een knooppunt naar een ander knooppunt-configuratie

U kunt een knooppunt in het gebruik van de configuratie van een ander knooppunt dan de versie die u in eerste instantie toegewezen toewijzen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** blade, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **knooppunten** tabblad.
1. Op de **knooppunten** tabblad, klik op de naam van het knooppunt dat u wilt toewijzen.
1. Klik op de pagina voor dat knooppunt **knooppuntconfiguratie toewijzen**.

    ![Schermafbeelding van de pagina knooppunt met de knop toewijzen knooppunt configuratie markeren](./media/automation-dsc-getting-started/AssignNode.png)

1. Op de **knooppuntconfiguratie toewijzen** pagina, selecteert u de configuratie van de knooppunten die u wilt toewijzen van het knooppunt en klik vervolgens op **OK**.

    ![Schermafbeelding van de pagina knooppuntconfiguratie toewijzen](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Registratie van een knooppunt

Als u niet langer een knooppunt kan worden beheerd door Azure Automation DSC wilt, kunt u de registratie verwijderen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** blade, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klikt u op de **knooppunten** tabblad.
1. Op de **knooppunten** tabblad, klik op de naam van het knooppunt dat u wilt de registratie ongedaan maken.
1. Klik op de pagina voor dat knooppunt **registratie**.

    ![Schermafbeelding van de detailpagina van knooppunt markeren van de registratie ongedaan maken-knop](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Gerelateerde artikelen

- [Overzicht van Azure Automation-status](automation-dsc-overview.md)
- [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md)
- [Windows PowerShell Desired State Configuration-overzicht](/powershell/dsc/overview)
- [Configuratie-cmdlets voor Azure Automation-status](/powershell/module/azurerm.automation/#automation)
- [Prijzen van Azure Automation State-configuratie](https://azure.microsoft.com/pricing/details/automation/)