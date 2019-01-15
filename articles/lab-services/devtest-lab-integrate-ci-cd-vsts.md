---
title: Azure DevTest Labs te integreren in uw Azure-pijplijnen continue integratie en levering pijplijn | Microsoft Docs
description: Meer informatie over het integreren van Azure DevTest Labs in uw Azure pijplijnen continue integratie en leveringspijplijn
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: bcc39f2d8cf1ca0440f8028464d9041435914477
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263404"
---
# <a name="integrate-azure-devtest-labs-into-your-azure-devops-continuous-integration-and-delivery-pipeline"></a>Azure DevTest Labs integreren in uw Azure DevOps, continue integratie en van leveringspijplijn
U kunt de *Azure DevTest Labs-taken* -extensie die geïnstalleerd in Azure DevOps tot eenvoudig uw CI/CD-build-en release-pijplijn integreren met Azure DevTest Labs. De extensie installeert drie taken: 
* Een virtuele machine maken
* Een aangepaste installatiekopie van een virtuele machine maken
* Een VM verwijderen 

Het proces kunt u eenvoudig is een 'gouden installatiekopie' voor een specifieke test taak bijvoorbeeld snel implementeren en vervolgens te verwijderen wanneer de test is voltooid.

In dit artikel laat zien hoe maken en implementeren van een virtuele machine, een aangepaste installatiekopie maken en verwijder vervolgens de virtuele machine, alle als één volledige pijplijn. U zou elke taak gewoonlijk afzonderlijk uitvoeren in uw eigen aangepaste pijplijn voor het build-testen-implementeren.

## <a name="before-you-begin"></a>Voordat u begint
Voordat u uw CI/CD-pijplijn met Azure DevTest Labs integreren kunt, moet u de extensie installeren vanuit Visual Studio Marketplace.
1. Ga naar [Azure DevTest Labs-taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).
1. Selecteer **Installeren**.
1. Voltooi de wizard.

## <a name="create-a-resource-manager-template"></a>Een Resource Manager-sjabloon maken
In deze sectie wordt beschreven hoe u de Azure Resource Manager-sjabloon die u gebruiken voor het maken van een virtuele machine van Azure op aanvraag maken.

1. Voor het maken van een Resource Manager-sjabloon in uw abonnement, voert u de procedure in [Resource Manager-sjabloon gebruiken](devtest-lab-use-resource-manager-template.md).
1. Voordat u de Resource Manager-sjabloon hebt gegenereerd, toevoegen de [WinRM artefact](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) als onderdeel van het maken van de virtuele machine.

   WinRM-toegang is vereist om te gebruiken, zoals implementatietaken *Azure bestandskopie* en *PowerShell op de doelmachines*.

   > [!NOTE]
   > Als u WinRM met een gedeelde IP-adres gebruikt, moet u een NAT-regel voor het toewijzen van een externe poort op de WinRM-poort toevoegen. Deze stap is niet vereist als u de virtuele machine met een openbaar IP-adres maken.
   >
   >

1. De sjabloon opslaan als een bestand op uw computer. Noem het bestand **CreateVMTemplate.json**.
1. Controleer de sjabloon uw bronbeheersysteem.
1. Open een teksteditor en plak het volgende script erin.

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

1. Controleer het script in uw bronbeheersysteem. Noem het er ongeveer als **GetLabVMParams.ps1**.

   Als u dit script uitvoert op de agent als onderdeel van de release-pijplijn, en als u de stappen zoals *Azure bestandskopie* of *PowerShell op de doelmachines*, het script de waarden die u nodig hebt om te worden verzameld uw app implementeren op de virtuele machine. Normaal gesproken gebruikt u deze taken om apps te implementeren met een Azure-VM. De taken vereist waarden zoals de naam van de VM-resourcegroep, het IP-adres en de volledig gekwalificeerde domeinnaam (FQDN).

## <a name="create-a-release-pipeline-in-release-management"></a>In de Release Management maakt een release-pijplijn
Voor het maken van de release-pijplijn, het volgende doen:

1. Op de **Releases** tabblad van de **Build & Release** hub, selecteer de knop plusteken (+).
1. In de **release-definitie maken** venster de **leeg** sjabloon en selecteer vervolgens **volgende**.
1. Selecteer **Kies Later**, en selecteer vervolgens **maken** naar een nieuwe release-pijplijn maken met een standaard-omgeving en er is geen gekoppelde artefacten.
1. Als u wilt het snelmenu in de nieuwe release-pijplijn openen, selecteer het weglatingsteken (...) naast de omgevingsnaam van de en selecteer vervolgens **variabelen configureren**. 
1. In de **configureren - omgeving** venster voor de variabelen die u in de release-pijplijn-taken gebruikt, voer de volgende waarden:

   a. Voor **vmName**, voer de naam die u hebt toegewezen aan de virtuele machine tijdens het maken van de Resource Manager-sjabloon in Azure portal.

   b. Voor **gebruikersnaam**, voer de gebruikersnaam die u hebt toegewezen aan de virtuele machine tijdens het maken van de Resource Manager-sjabloon in Azure portal.

   c. Voor **wachtwoord**, voer het wachtwoord die u hebt toegewezen aan de virtuele machine tijdens het maken van de Resource Manager-sjabloon in Azure portal. Gebruik "hangslot" verbergen en het wachtwoord kunt beveiligen.

### <a name="create-a-vm"></a>Een virtuele machine maken

De volgende fase van de implementatie is het maken van de virtuele machine als de 'gouden installatiekopie' voor latere implementaties wilt gebruiken. U kunt de virtuele machine maken in uw Azure DevTest Lab-instantie met behulp van de taak die speciaal voor dit doel ontwikkeld. 

1. Selecteer in de release-pijplijn **toevoegen taken**.
1. Op de **implementeren** tabblad, voegt u een *Azure DevTest Labs maken VM* taak. De taak als volgt configureren:

   > [!NOTE]
   > Zie voor het maken van de virtuele machine moet worden gebruikt voor latere implementaties [Azure DevTest Labs-taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Voor **Azure RM-abonnement**, selecteert u een verbinding in de **beschikbare verbindingen van Azure Service** lijst of maak een meer beperkte machtigingen voor verbinding met uw Azure-abonnement. Zie voor meer informatie, [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

   b. Voor **Labnaam**, selecteert u de naam van het exemplaar dat u eerder hebt gemaakt.

   c. Voor **sjabloonnaam**, geef het volledige pad en de naam van de sjabloon die u hebt opgeslagen naar uw opslagplaats broncode. U kunt de ingebouwde eigenschappen van Release Management gebruiken voor het vereenvoudigen van het pad, bijvoorbeeld:

   ```
   $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
   ```

   d. Voor **Sjabloonparameters**, geef de parameters voor de variabelen die zijn gedefinieerd in de sjabloon. Gebruik de namen van de variabelen die u hebt gedefinieerd in de omgeving, bijvoorbeeld:

   ```
   -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
   ```

   e. Voor **Uitvoervariabelen - Lab VM-ID**, moet u de ID van de zojuist gemaakte virtuele machine voor de volgende stappen. U stelt de standaardnaam van de omgevingsvariabele die automatisch wordt ingevuld met deze ID in de **Uitvoervariabelen** sectie. U kunt de variabele indien nodig bewerken, maar vergeet niet dat u de juiste naam in de volgende taken. De ID van de Lab-VM is geschreven in de volgende notatie:

   ```
   /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
   ```

1. Voer het script dat u eerder hebt voor het verzamelen van de details van de DevTest Labs-virtuele machine gemaakt. 
1. Selecteer in de release-pijplijn **toevoegen taken** en klik op de **implementeren** tabblad, voegt u een *Azure PowerShell* taak. De taak als volgt configureren:

   > [!NOTE]
   > Zie voor het verzamelen van de details van de virtuele machine DevTest-Labs, [implementeren: Azure PowerShell](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzurePowerShellV3) en voer het script uit.

   a. Voor **Azure verbindingstype**, selecteer **Azure Resource Manager**.

   b. Voor **Azure RM-abonnement**, selecteer een verbinding in de lijst onder **beschikbare verbindingen van Azure Service**, of een meer beperkte machtigingen verbinding maken met uw Azure-abonnement. Zie voor meer informatie, [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

   c. Voor **scripttype**, selecteer **scriptbestand**.
 
   d. Voor **scriptpad**, geef het volledige pad en de naam van het script dat u in uw opslagplaats broncode opgeslagen. U kunt de ingebouwde eigenschappen van Release Management gebruiken voor het vereenvoudigen van het pad, bijvoorbeeld:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   e. Voor **scriptargumenten**, voer de naam van de omgevingsvariabele die is automatisch ingevuld met de ID van het testlab VM door de vorige taak, bijvoorbeeld: 
      ```
      -labVmId '$(labVMId)'
      ```
    Het script worden de vereiste waarden verzameld en opgeslagen in omgevingsvariabelen in de release-pijplijn zodat u eenvoudig naar deze in de volgende stappen verwijzen kunt.

1. Uw app implementeren in de nieuwe virtuele machine een DevTest-Labs. De taken die u normaal gesproken gebruikt om de app te implementeren zijn *Azure bestandskopie* en *PowerShell op de doelmachines*.
   De informatie over de virtuele machine die u nodig hebt voor de parameters van deze taken wordt opgeslagen in drie variabelen met de naam **labVmRgName**, **labVMIpAddress**, en **labVMFqdn**binnen de release-pijplijn. Als u wilt dat alleen om te experimenteren met het maken van een DevTest Labs-virtuele machine en een aangepaste installatiekopie, zonder dat een app implementeren op deze, kunt u deze stap overslaan.

### <a name="create-an-image"></a>Een installatiekopie maken

De volgende fase is het maken van een installatiekopie van de geïmplementeerde virtuele machine in uw Azure DevTest Labs-exemplaar. U kunt vervolgens de installatiekopie te maken van kopieën van de virtuele machine op aanvraag wanneer u wilt uitvoeren van een taak ontwikkelen of bepaalde tests worden uitgevoerd. 

1. Selecteer in de release-pijplijn **toevoegen taken**.
1. Op de **implementeren** tabblad, voegt u een **Azure DevTest Labs maken Custom-Image** taak. Configureer deze als volgt:

   > [!NOTE]
   > Zie voor het maken van de installatiekopie, [Azure DevTest Labs-taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Voor **Azure RM-abonnement**, in de **beschikbare verbindingen van Azure Service** lijst, selecteer een verbinding in de lijst of maak een meer beperkte machtigingen voor verbinding met uw Azure-abonnement. Zie voor meer informatie, [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

   b. Voor **Labnaam**, selecteert u de naam van het exemplaar dat u eerder hebt gemaakt.

   c. Voor **aangepaste installatiekopie met de naam**, voer een naam voor de aangepaste installatiekopie.

   d. (Optioneel) Voor **beschrijving**, voer een beschrijving kunt u eenvoudig de juiste installatiekopie later selecteren.

   e. Voor **bron-VM voor Lab - VM-ID van bron-Lab**, als u de standaardnaam van de omgevingsvariabele die automatisch is gevuld met de ID van het lab VM door een eerdere taak gewijzigd hier bewerken. De standaardwaarde is **$(labVMId)**.

   f. Voor **Uitvoervariabelen - aangepaste installatiekopie-ID**, moet u de ID van de installatiekopie van het zojuist gemaakte als u wilt beheren of te verwijderen. De standaardnaam van de omgevingsvariabele die automatisch wordt ingevuld met deze ID is ingesteld in de **Uitvoervariabelen** sectie. Indien nodig, kunt u de variabele bewerken.

### <a name="delete-the-vm"></a>De VM verwijderen

De laatste fase is het verwijderen van de virtuele machine die u hebt geïmplementeerd in uw Azure DevTest Labs-exemplaar. Normaal gesproken verwijdert u de virtuele machine nadat u de dev-taken uitvoeren of Voer de tests die u nodig hebt op de geïmplementeerde virtuele machine. 

1. Selecteer in de release-pijplijn **toevoegen taken** en klik op de **implementeren** tabblad, voegt u een *Azure DevTest Labs verwijderen VM* taak. Configureer deze als volgt:

      > [!NOTE]
      > Als u wilt verwijderen van de virtuele machine, Zie [Azure DevTest Labs-taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Voor **Azure RM-abonnement**, selecteert u een verbinding in de **beschikbare verbindingen van Azure Service** lijst of maak een meer beperkte machtigingen voor verbinding met uw Azure-abonnement. Zie voor meer informatie, [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).
 
   b. Voor **Lab VM-ID**, als u de standaardnaam van de omgevingsvariabele die automatisch is gevuld met de ID van het lab VM door een eerdere taak gewijzigd hier bewerken. De standaardwaarde is **$(labVMId)**.

1. Voer een naam voor de release-pijplijn en vervolgens op te slaan.
1. Maak een nieuwe versie, selecteert u de nieuwste build en deze implementeren in één omgeving in de pijplijn.

In elk stadium, vernieuw de weergave van uw DevTest Labs-exemplaar in de Azure-portal om weer te geven van de virtuele machine en de installatiekopie die worden gemaakt en de virtuele machine die opnieuw wordt verwijderd.

U kunt nu de aangepaste installatiekopie gebruiken op virtuele machines maken als ze vereist.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [multi-VM-omgevingen maken met Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md).
* Meer snelstartsjablonen van het type Resource Manager voor het automatiseren van DevTest Labs verkennen de [openbare DevTest Labs GitHub-opslagplaats](https://github.com/Azure/azure-quickstart-templates).
* Indien nodig, Zie de [probleemoplossing van Azure DevOps](https://docs.microsoft.com/azure/devops/pipelines/troubleshooting) pagina.
 
