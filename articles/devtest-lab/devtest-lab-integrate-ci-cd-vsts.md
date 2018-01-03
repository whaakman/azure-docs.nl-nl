---
title: Azure DevTest Labs integreren in uw VSTS continue integratie en levering pijplijn | Microsoft Docs
description: Meer informatie over het integreren van Azure DevTest Labs in uw VSTS continue integratie en levering pijplijn
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: tarcher
ms.openlocfilehash: 3cd6939e890f59e7c5b188edd8620da5c9fcc935
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="integrate-azure-devtest-labs-into-your-vsts-continuous-integration-and-delivery-pipeline"></a>Azure DevTest Labs integreren in uw VSTS continue integratie en de pijplijn voor levering
U kunt de **Azure DevTest Labs taken extensie** in Visual Studio Team Services (VSTS) te gemakkelijk integreren van uw CI/CD-build en de vrijgave van de pijplijn met Azure DevTest Labs geïnstalleerd. De uitbreiding drie taken voor het maken van een virtuele machine, een aangepaste installatiekopie van een virtuele machine maken en verwijderen van een virtuele machine is geïnstalleerd. Dit proces kunt u gemakkelijk, bijvoorbeeld snel implementeren 'gouden installatiekopie' voor de taak specifieke testen en vervolgens verwijderen wanneer de test is voltooid.

In dit artikel laat zien hoe maken en implementeren van een virtuele machine, een aangepaste installatiekopie maken en vervolgens de virtuele machine alle als één volledige pijplijn verwijderen. Normaal gesproken, gebruikt u de taken afzonderlijk in uw eigen aangepaste build testimplementatie pijplijn.

## <a name="before-you-begin"></a>Voordat u begint
Voordat u uw CI/CD-pijplijn met Azure DevTest Labs integreren kunt, moet u eerst de uitbreiding van Visual Studio Marketplace installeren.
1. Ga naar [Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)
1. Kies installeren
1. De wizard voltooien

## <a name="create-a-resource-manager-template"></a>Resource Manager-sjabloon maken
Deze sectie wordt beschreven hoe de Azure Resource Manager-sjabloon die u gebruikt voor het maken van een virtuele Machine van Azure op aanvraag wilt maken.

1. Volg de stappen in [Resource Manager-sjabloon gebruiken](devtest-lab-use-resource-manager-template.md) Resource Manager-sjabloon maken in uw abonnement.
1. Voeg de [WinRM artefact](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) als onderdeel van het maken van de virtuele machine (vóór het genereren van de Resource Manager-sjabloon).

   WinRM toegang is vereist voor het gebruik implementeren taken zoals **Azure bestandskopie** en **PowerShell op de doelmachines**.

   > [!NOTE]
   > Als u WinRM met een gedeelde IP-adres, moet u een NAT-regel om een externe poort toewijzen aan de WinRM-poort toevoegen. Dit is niet vereist als de virtuele machine is gemaakt met een openbare IP-adres.
   >
   >

1. De sjabloon opslaan als een bestand op uw computer. Noem het bestand **CreateVMTemplate.json**.
1. Controleer de sjabloon in uw bronbeheersysteem.
1. Open een teksteditor en kopieer het volgende script in de App.

   ```powershell
   Param( [string] $labVmId)

   $labVmComputeId = (Get-AzureRmResource -Id $labVmId).Properties.ComputeId

   # Get lab VM resource group name
   $labVmRgName = (Get-AzureRmResource -Id $labVmComputeId).ResourceGroupName

   # Get the lab VM Name
   $labVmName = (Get-AzureRmResource -Id $labVmId).Name

   # Get lab VM public IP address
   $labVMIpAddress = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
                   -Name $labVmName).IpAddress

   # Get lab VM FQDN
   $labVMFqdn = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
              -Name $labVmName).DnsSettings.Fqdn

   # Set a variable labVmRgName to store the lab VM resource group name
   Write-Host "##vso[task.setvariable variable=labVmRgName;]$labVmRgName"

   # Set a variable labVMIpAddress to store the lab VM Ip address
   Write-Host "##vso[task.setvariable variable=labVMIpAddress;]$labVMIpAddress"

   # Set a variable labVMFqdn to store the lab VM FQDN name
   Write-Host "##vso[task.setvariable variable=labVMFqdn;]$labVMFqdn"
   ```

1. Controleer het script in uw bronbeheersysteem. Vergelijkbaar met een andere naam **GetLabVMParams.ps1**.

   Dit script wanneer op de agent uitgevoerd als onderdeel van de definitie van de release, verzamelt waarden die u nodig hebt voor het implementeren van uw app naar de virtuele machine als u de stappen voor taken zoals **Azure bestandskopie** of **PowerShell op de doelmachines**. Doorgaans gebruik van deze taken apps te implementeren op een virtuele machine van Azure en er waarden zoals de naam van de VM-resourcegroep, IP-adres en FQDN-naam (FULLY) wordt vereist.

## <a name="create-the-release-definition-in-release-management"></a>De release-definitie maken in de Release Management
Deze stappen voor het maken van de definitie van de release uitvoeren.

1. Open de **Releases** tabblad van de **bouwen & Release** hub en kies de '**+**' pictogram.
1. In de **maken release definitie** dialoogvenster, selecteer de **leeg** sjabloon, en kies **volgende**.
1. Selecteer in de volgende pagina **Later kiezen** en kies vervolgens **maken** de definitie van een nieuwe release met één standaard-omgeving en geen gekoppelde artefacten maken.
1. In het nieuwe release definition, kiest u de weglatingstekens (...) naast de naam van de omgeving het snelmenu openen en selecteer **variabelen configureren**. 
1. In de **configureren - omgeving** dialoogvenster, voer de volgende waarden voor de variabelen die u in de release definitie taken gebruiken:
   - **vmName**: Voer de naam die u aan de virtuele machine toegewezen tijdens het maken van de Resource Manager-sjabloon in de Azure portal.
   - **Gebruikersnaam**: Geef de gebruikersnaam die u aan de virtuele machine toegewezen tijdens het maken van de Resource Manager-sjabloon in de Azure portal.
   - **wachtwoord**: Voer het wachtwoord die u aan de virtuele machine toegewezen tijdens het maken van de Resource Manager-sjabloon in de Azure portal. Gebruik ' hangslot ' om te verbergen en het wachtwoord te beveiligen.

   <a name="create-a-vm"></a>Een virtuele machine maken
   ---

   De eerste fase in deze implementatie is het maken van de virtuele machine wilt gebruiken als de 'gouden installatiekopie' voor de volgende implementaties. U kunt de virtuele machine maken binnen uw Azure DevTest Lab-exemplaar met behulp van de speciaal voor dit doel is ontwikkeld. Selecteer in de definitie release **+ taken toevoegen** en voeg een **Azure DevTest Labs maken VM** taak op het tabblad implementeren. Als volgt configureren:

   Zie [Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) voor het maken van de virtuele machine moet worden gebruikt voor toekomstige implementaties.
   - **Abonnement voor Azure RM**: Selecteer een verbinding in de lijst onder **beschikbare Azure serviceverbindingen** of een meer beperkte machtigingen verbinding maken met uw Azure-abonnement. Zie voor meer informatie [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **De Labnaam**: Selecteer de naam van het exemplaar dat u eerder hebt gemaakt.
   - **Sjabloonnaam**: Voer het volledige pad en de naam van het sjabloonbestand die u hebt opgeslagen in de opslagplaats van uw bron code. U kunt de ingebouwde eigenschappen van Release Management gebruiken voor het vereenvoudigen van het pad, bijvoorbeeld:
      ```
      $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
      ```
   - **Sjabloonparameters**: Geef de parameters voor de variabelen in de sjabloon worden gedefinieerd. Gebruik de namen van de variabelen die u hebt gedefinieerd in de omgeving:
      ```
      -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
      ```
   - **Variabelen - ID van de virtuele testomgeving uitvoer**: U moet de ID van de zojuist gemaakte virtuele machine voor de volgende stappen. De standaardnaam van de omgevingsvariabele die automatisch wordt ingevuld met deze ID is ingesteld in de **Uitvoervariabelen** sectie. U kunt de variabele indien nodig bewerken, maar moet u de juiste naam gebruiken in de volgende taken. De ID van de virtuele testomgeving is in het formulier:
      ```
      /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
      ```
1. Voer het script dat u eerder hebt gemaakt voor de details van DevTest Labs VM verzamelen. Selecteer in de definitie release **+ taken toevoegen** en voeg een **Azure PowerShell** taak de **implementeren** tabblad. De taak als volgt configureren:

   Zie [implementeren: Azure PowerShell](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/AzurePowerShell) en voer het script voor het verzamelen van de details van DevTest Labs VM.

   - **Azure verbindingstype**: Azure Resource Manager.
   - **Abonnement voor Azure RM**: Selecteer een verbinding in de lijst onder **beschikbare Azure serviceverbindingen** of een meer beperkte machtigingen verbinding maken met uw Azure-abonnement. Zie voor meer informatie [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Type script**: Script bestand.
   - **Pad naar een script**: Voer het volledige pad en de naam van het script dat u hebt opgeslagen in de opslagplaats van uw bron code. U kunt de ingebouwde eigenschappen van Release Management gebruiken voor het vereenvoudigen van het pad, bijvoorbeeld:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   - **Argumenten script**: Voer als het scriptargument de naam van de variabele die is automatisch ingevuld met de ID van het testlab VM door de vorige taak, bijvoorbeeld: 
      ```
      -labVmId '$(labVMId)'
      ```
   Het script worden de vereiste waarden verzameld en opgeslagen in omgevingsvariabelen in de release-definitie zodat u eenvoudig deze in taakstappen van de volgende raadplegen kunt.

1. U kunt nu uw app implementeren naar de nieuwe DevTest Labs virtuele machine. De taken die u doorgaans gebruiken om te implementeren zijn **Azure bestandskopie** en **PowerShell op de doelmachines**.
   - De informatie over de virtuele machine die u nodig hebt voor de parameters van deze taken wordt opgeslagen in drie variabelen met de naam **labVmRgName**, **labVMIpAddress**, en **labVMFqdn**in de release-definitie.
   - Als u alleen experimenteren wilt met het maken van een DevTest Labs virtuele machine en een aangepaste installatiekopie zonder een app implementeren op deze, kunt u deze stap overslaan.

   <a name="create-an-image"></a>Een installatiekopie maken
   ---

   De volgende fase is het maken van een installatiekopie van de geïmplementeerde virtuele machine in uw Azure DevTest Labs-exemplaar. U kunt vervolgens deze installatiekopie kopieën te maken van de virtuele machine op verzoek, wanneer u wilt een dev-taak uitvoeren of bepaalde tests worden uitgevoerd. Selecteer in de definitie release **+ taken toevoegen** en voeg een **Azure DevTest Labs maken aangepaste installatiekopie** taak de **implementeren** tabblad. Als volgt configureren:

   Zie [Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) om de installatiekopie te maken.
   - **Abonnement voor Azure RM**: Selecteer een verbinding in de lijst onder **beschikbare Azure serviceverbindingen** of een meer beperkte machtigingen verbinding maken met uw Azure-abonnement. Zie voor meer informatie [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **De Labnaam**: Selecteer de naam van het exemplaar dat u eerder hebt gemaakt.
   - **De naam van een aangepaste installatiekopie**: Voer een naam voor de aangepaste installatiekopie.
   - **Beschrijving**: Geef eventueel een beschrijving kunt u eenvoudig de juiste installatiekopie om later te selecteren.
   - **Bron Lab VM - VM-ID van berichtbron Lab**: als u de standaardnaam van de omgevingsvariabele die automatisch is ingevuld met de ID van het testlab VM door een eerdere taak hebt gewijzigd, bewerkt het hier. De standaardwaarde is *$(labVMId)*.
   - **Variabelen - ID van de afbeelding aangepast uitvoer**: U moet de ID van de installatiekopie van het zojuist gemaakte als u wilt beheren of te verwijderen. De standaardnaam van de omgevingsvariabele die automatisch wordt ingevuld met deze ID is ingesteld in de **Uitvoervariabelen** sectie. Indien nodig, kunt u de variabele bewerken.
   
   <a name="delete-the-vm"></a>De virtuele machine verwijderen
   ---
   
   De laatste fase in dit voorbeeld is het verwijderen van de virtuele machine die u hebt geïmplementeerd in uw Azure DevTest Labs-exemplaar. Normaal gesproken verwijderen u de virtuele machine nadat u de dev-taken uitvoeren of de tests die u moet uitvoeren op de geïmplementeerde virtuele machine. Selecteer in de definitie release **+ taken toevoegen** en voeg een **Azure DevTest Labs verwijderen VM** taak de **implementeren** tabblad. Als volgt configureren:

   Zie [Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) verwijderen van de virtuele machine.
   - **Abonnement voor Azure RM**: Selecteer een verbinding in de lijst onder **beschikbare Azure serviceverbindingen** of een meer beperkte machtigingen verbinding maken met uw Azure-abonnement. Zie voor meer informatie [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **ID van de virtuele testomgeving**: als u de standaardnaam van de omgevingsvariabele die automatisch is ingevuld met de ID van het testlab VM door een eerdere taak hebt gewijzigd, bewerkt het hier. De standaardwaarde is *$(labVMId)*.
1. Voer een naam voor de definitie van de release en sla deze op.
1. Een nieuwe release gemaakt, selecteert u de laatste build en deze implementeren in de één-omgeving in de definitie.

Vernieuw de weergave van uw exemplaar DevTest Labs in de Azure portal om te zien van de virtuele machine en de installatiekopie wordt gemaakt en de virtuele machine opnieuw wordt verwijderd in elk stadium.

U kunt nu de aangepaste installatiekopie gebruiken voor het maken van virtuele machines, indien nodig.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [multi-VM-omgevingen maken met Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md).
* Ontdek meer Quick Start-Resource Manager-sjablonen voor het automatiseren van DevTest Labs vanuit de [openbare DevTest Labs GitHub-repo-](https://github.com/Azure/azure-quickstart-templates).
* Indien nodig, Zie de [VSTS probleemoplossing](https://docs.microsoft.com/vsts/build-release/actions/troubleshooting) pagina.
