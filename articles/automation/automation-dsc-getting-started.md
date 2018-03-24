---
title: Aan de slag met Azure Automation DSC
description: Uitleg en voorbeelden van de meest algemene taken in Azure Automation Desired State Configuration (DSC)
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 0833005963fa8728c2a52cc9cf5968bf4afe147d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-azure-automation-dsc"></a>Aan de slag met Azure Automation DSC
Dit artikel wordt uitgelegd hoe u de meest algemene taken met Azure Automation Desired State Configuration (DSC), zoals het maken, importeren, en compileren configuraties, voorbereiding-machines te beheren, en het weergeven van rapporten. Zie voor een overzicht van wat Azure Automation DSC is, [overzicht van Azure Automation DSC](automation-dsc-overview.md). Zie voor documentatie DSC [Windows PowerShell Desired Configuration overzicht](https://msdn.microsoft.com/PowerShell/dsc/overview).

Dit artikel bevat een stapsgewijze handleiding voor het gebruik van Azure Automation DSC. Als u een Voorbeeldomgeving is al ingesteld zonder de stappen in dit artikel wilt, kunt u de volgende Resource Manager-sjabloon: met deze sjabloon heeft een voltooide Azure Automation DSC-omgeving, met inbegrip van een virtuele machine van Azure die is ingesteld beheerd door Azure Automation DSC.

## <a name="prerequisites"></a>Vereisten
Het volgende is vereist voor het voltooien van de voorbeelden in dit artikel:

* Een Azure Automation-account. Zie [Azure Uitvoeren-als-account](automation-sec-configure-azure-runas-account.md) voor instructies over het maken van een Azure Automation Uitvoeren-als-account.
* Een Azure Resource Manager VM (niet-klassieke) met Windows Server 2008 R2 of hoger. Zie [Uw eerste virtuele Windows-machine maken met behulp van Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor instructies voor het maken van een VM

## <a name="creating-a-dsc-configuration"></a>Maken van een DSC-configuratie
U maakt een eenvoudige [DSC-configuratie](https://msdn.microsoft.com/powershell/dsc/configurations) die ervoor zorgt de aanwezigheid of afwezigheid van de **webserver** Windows functie (IIS), afhankelijk van hoe u knooppunten toewijzen.

1. Start de Windows PowerShell ISE (of een teksteditor).
2. Typ de volgende tekst:
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
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
3. Sla het bestand als `TestConfig.ps1`.

Deze configuratie een resource in elk blok knooppunt roept de [WindowsFeature resource](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), die zorgt ervoor dat de aanwezigheid of afwezigheid van de **webserver** functie.

## <a name="importing-a-configuration-into-azure-automation"></a>Een configuratie te importeren in Azure Automation
Vervolgens moet importeren u de configuratie in het Automation-account.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina **DSC-configuraties** onder **Configuratiebeheer**.
4. Op de **DSC-configuraties** pagina, klikt u op **+ toevoegen van een configuratie**.
5. Op de **configuratie importeren** pagina, blader naar de `TestConfig.ps1` bestand op uw computer.
   
    ![Schermafbeelding van de ** importeren configuratie ** blade](./media/automation-dsc-getting-started/AddConfig.png)
6. Klik op **OK**.

## <a name="viewing-a-configuration-in-azure-automation"></a>Een configuratie weergeven in Azure Automation
Nadat u een configuratie hebt geïmporteerd, kunt u deze bekijken in de Azure portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina **DSC-configuraties** onder **Configuratiebeheer**.
4. Op de **DSC-configuraties** pagina, klikt u op **TestConfig** (dit is de naam van de configuratie die u hebt geïmporteerd in de vorige procedure).
5. Op de **TestConfig configuratie** pagina, klikt u op **weergave configuratiebron**.
   
    ![Schermafbeelding van het tabblad TestConfig-configuratie](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    Een **TestConfig configuratiebron** blade wordt geopend, om de PowerShell-code voor de configuratie weer te geven.

## <a name="compiling-a-configuration-in-azure-automation"></a>Een Azure Automation-configuratie compileren
Voordat u een gewenste status op een knooppunt toepassen kunt, moet een DSC-configuratie definiëren die status worden gecompileerd naar een of meer knooppuntconfiguraties (MOF document) en op de Automation DSC-Pull-Server geplaatst. Zie voor een gedetailleerdere beschrijving van het compileren van configuraties in Azure Automation DSC [compileren van configuraties in Azure Automation DSC](automation-dsc-compile.md). Zie voor meer informatie over het compileren van configuraties [DSC-configuraties](https://msdn.microsoft.com/PowerShell/DSC/configurations).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina, klikt u op **DSC-configuraties** onder **Configuratiebeheer**.
4. Op de **DSC-configuraties** pagina, klikt u op **TestConfig** (de naam van de configuratie van de eerder geïmporteerd).
5. Op de **TestConfig configuratie** pagina, klikt u op **compileren**, en klik vervolgens op **Ja**. Hiermee start u een Compilatietaak.
   
    ![Schermafbeelding van de pagina TestConfig compileren knop markeren](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Wanneer u een configuratie in Azure Automation compileert, worden automatisch alle gemaakte knooppuntconfiguratie MOF-bestanden naar de pull-server implementeert.
> 
> 

## <a name="viewing-a-compilation-job"></a>Een Compilatietaak weergeven
Nadat u een compilatie start, kunt u het weergeven in de **compilatietaken** -tegel in de **configuratie** blade. De **compilatietaken** tegel bevat momenteel wordt uitgevoerd, voltooid en mislukte taken. Wanneer u een taak compilatie blade opent, ziet u informatie over deze taak inclusief eventuele fouten of waarschuwingen opgetreden, invoerparameters gebruikt in de configuratie en compilatie Logboeken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina, klikt u op **DSC-configuraties** onder **Configuratiebeheer**.
4. Op de **DSC-configuraties** pagina, klikt u op **TestConfig** (de naam van de configuratie van de eerder geïmporteerd).
5. Onder **compilatietaken**, selecteert u de Compilatietaak die u wilt weergeven. Een **Compilatietaak** pagina wordt geopend, gelabeld met de datum waarop de Compilatietaak is gestart.
   
    ![Schermafbeelding van de pagina Compilatietaak](./media/automation-dsc-getting-started/CompilationJob.png)
6. Klik op een tegel in de **Compilatietaak** pagina om te bekijken om meer details over de taak.

## <a name="viewing-node-configurations"></a>Knooppuntconfiguraties weergeven
Voltooiing van een Compilatietaak maakt een of meer nieuwe knooppuntconfiguraties. De configuratie van een knooppunt is een MOF-document dat is geïmplementeerd naar de pull-server en klaar om te worden opgehaald en toegepast door een of meer knooppunten. U kunt de knooppuntconfiguraties weergeven in uw Automation-account in de **DSC-knooppuntconfiguraties** blade. De configuratie van een knooppunt heeft een naam met het formulier *ConfigurationName*. *NodeName*.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu Hub op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** blade, klikt u op **DSC-knooppuntconfiguraties**.
   
    ![Schermafbeelding van het tabblad DSC-knooppuntconfiguraties](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Voorbereiden op een virtuele machine van Azure voor beheer met Azure Automation DSC
Azure Automation DSC kunt u virtuele Azure-machines (Classic en Resource Manager), lokale virtuele machines Linux machines, AWS virtuele machines en fysieke machines lokale beheren. In dit artikel leert u hoe u kunt alleen Azure Resource Manager VM's vrijgeven. Zie voor informatie over de voorbereiding andere soorten machines, [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>Voorbereiden van een Azure Resource Manager VM voor beheer door Azure Automation DSC
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina, klikt u op **DSC-knooppunten** onder **Configuratiebeheer**.
4. In de **DSC-knooppunten** pagina, klikt u op **toevoegen Azure VM**.
   
    ![Schermafbeelding van de DSC-knooppunten pagina markering van de knop toevoegen Azure VM](./media/automation-dsc-getting-started/OnboardVM.png)
5. Selecteer uw virtuele machine op de pagina virtuele Machines. **Azure Virtual machines toevoegen** pagina, klikt u op **selecteert u virtuele machines om vrij te geven**.
6. Klik op **Verbinden**.
   
   > [!IMPORTANT]
   > Dit moet een Azure Resource Manager-VM met Windows Server 2008 R2 of hoger.
   > 
   > 
1. In de **registratie** pagina, voer de naam van de knooppuntconfiguratie die u wilt toepassen op de virtuele machine in de **knooppunt configuratienaam** vak. Dit moet exact overeenkomen met de naam van de knooppuntconfiguratie van een in het Automation-account. Op dit moment een naam opgeven is optioneel. U kunt de toegewezen knooppuntconfiguratie na het voorbereiden op het knooppunt wijzigen.
   Controleer **knooppunt opnieuw opstarten indien nodig**, en klik vervolgens op **OK**.
   
    ![Schermafbeelding van de registratie-blade](./media/automation-dsc-getting-started/RegisterVM.png)
   
    De knooppuntconfiguratie die u hebt opgegeven, worden toegepast op de virtuele machine met een interval dat is opgegeven door de **frequentie van de configuratiemodus**, en de virtuele machine wordt gecontroleerd op updates voor de knooppuntconfiguratie met een interval dat is opgegeven door de **vernieuwen Frequentie**. Zie voor meer informatie over het gebruik van deze waarden [configureren van de lokale Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
9. In de **Azure Virtual machines toevoegen** blade, klikt u op **maken**.

Azure begint het proces van het voorbereiden op de virtuele machine. Wanneer deze voltooid is, de virtuele machine wordt weergegeven in de **DSC-knooppunten** blade in de Automation-account.

## <a name="viewing-the-list-of-dsc-nodes"></a>De lijst van DSC-knooppunten
U kunt de lijst weergeven met alle machines die zijn vrijgegeven voor management in uw Automation-account in de **DSC-knooppunten** blade.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina, klikt u op **DSC-knooppunten**.

## <a name="viewing-reports-for-dsc-nodes"></a>Weergeven van rapporten voor DSC-knooppunten
Elke keer dat Azure Automation DSC een consistentiecontrole uit op een beheerde knooppunt voert stuurt het knooppunt een statusrapport terug naar de pull-server. U kunt deze rapporten kunt weergeven op de pagina voor dat knooppunt.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina, klikt u op **DSC-knooppunten**.
4. In de **DSC-knooppunten** , selecteert u het knooppunt dat u wilt weergeven.
5. Op de **knooppunt** pagina, klikt u op het rapport dat u weergeven wilt onder **rapporten**.
   
    ![Schermafbeelding van het tabblad rapport](./media/automation-dsc-getting-started/NodeReport.png)

Op de blade voor een afzonderlijk rapport, kunt u de volgende statusgegevens voor de bijbehorende consistentiecontrole zien:

* De rapportstatus, of het knooppunt 'Compatibele', 'Mislukt' van de configuratie of het knooppunt is 'Niet-compatibel' (wanneer het knooppunt is in **applyandmonitor** modus en de machine zich niet in de gewenste status).
* De begintijd voor de consistentiecontrole.
* De totale runtime voor de consistentiecontrole.
* Het type van een consistentiecontrole uit.
* Eventuele fouten, met inbegrip van de foutcode en het foutbericht. 
* Eventuele DSC-resources in de configuratie en de status van elke bron (of het knooppunt bevindt zich in de gewenste status voor die bron) gebruikt, kunt u klikken op elke bron voor meer gedetailleerde informatie voor die bron.
* De naam, het IP-adres en de configuratiemodus van het knooppunt.

U kunt ook klikken op **onbewerkte rapport weergeven** om te zien van de feitelijke gegevens die het knooppunt naar de server verzendt. Zie voor meer informatie over het gebruik van die gegevens [met behulp van een DSC-rapportserver](https://msdn.microsoft.com/powershell/dsc/reportserver).

Het kan even duren wanneer een knooppunt vrijgegeven is voordat het rapport eerste beschikbaar is. U moet mogelijk wacht u tot 30 minuten voor het eerste rapport na u vrijgeven een knooppunt.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Opnieuw toewijzen van een knooppunt naar een ander knooppunt-configuratie
U kunt een knooppunt als de configuratie van een ander knooppunt dan de die oorspronkelijk was toegewezen gebruiken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina, klikt u op **DSC-knooppunten**.
4. Op de **DSC-knooppunten** pagina, klikt u op de naam van het knooppunt dat u wilt toewijzen.
5. Klik op de pagina voor dat knooppunt **toewijzen knooppunt**.
   
    ![Schermafbeelding van het tabblad knooppunt is de knop toewijzen knooppunt markeren](./media/automation-dsc-getting-started/AssignNode.png)
6. Op de **knooppuntconfiguratie toewijzen** pagina, selecteert u de configuratie van het knooppunt waarnaar u wilt toewijzen van het knooppunt en klik vervolgens op **OK**.
   
    ![Schermafbeelding van het tabblad knooppuntconfiguratie toewijzen](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>De registratie van een knooppunt
Als u niet langer een knooppunt kan worden beheerd door Azure Automation DSC wilt, kunt u de registratie verwijderen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **alle resources** en vervolgens de naam van uw Automation-account.
3. Op de **Automation-account** pagina, klikt u op **DSC-knooppunten**.
4. Op de **DSC-knooppunten** pagina, klikt u op de naam van het knooppunt dat u wilt registratie.
5. Klik op de pagina voor dat knooppunt **Unregister**.
   
    ![Schermafbeelding van de markering van de knop Unregister knooppunt-blade](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Gerelateerde artikelen
* [Overzicht van Azure Automation DSC](automation-dsc-overview.md)
* [Computers voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md)
* [Windows PowerShell Desired State Configuration-overzicht](https://msdn.microsoft.com/powershell/dsc/overview)
* [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)
* [Azure Automation DSC-prijzen](https://azure.microsoft.com/pricing/details/automation/)

