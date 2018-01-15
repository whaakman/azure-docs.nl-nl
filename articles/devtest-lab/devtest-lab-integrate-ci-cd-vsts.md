---
title: Azure DevTest Labs integreren in uw VSTS continue integratie en levering pijplijn | Microsoft Docs
description: Meer informatie over het integreren van Azure DevTest Labs in uw VSTS continue integratie en levering pijplijn
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: v-craic
ms.openlocfilehash: 6c6bd4fbd89ec87cbbdbfb9ed42f86a484acf7ad
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="integrate-azure-devtest-labs-into-your-vsts-continuous-integration-and-delivery-pipeline"></a>Azure DevTest Labs integreren in uw VSTS continue integratie en de pijplijn voor levering
U kunt de *Azure DevTest Labs taken* -extensie die in Visual Studio Team Services (VSTS) op eenvoudige wijze geïnstalleerd uw build-en release CI/CD pijplijn integreren met Azure DevTest Labs. De uitbreiding is geïnstalleerd drie taken: 
* Een virtuele machine maken
* Een aangepaste installatiekopie van een virtuele machine maken
* Een VM verwijderen 

Het proces kunt u gemakkelijk, bijvoorbeeld 'gouden installatiekopie' voor een specifieke test taak snel implementeren en verwijderen wanneer de test is voltooid.

In dit artikel laat zien hoe maken en implementeren van een virtuele machine, een aangepaste installatiekopie maken en verwijder vervolgens de virtuele machine alle als één volledige pijplijn. U zou elke taak normaal afzonderlijk uitvoeren in uw eigen aangepaste build testimplementatie pijplijn.

## <a name="before-you-begin"></a>Voordat u begint
Voordat u uw CI/CD-pijplijn met Azure DevTest Labs integreren kunt, moet u de uitbreiding installeren vanuit Visual Studio Marketplace.
1. Ga naar [Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).
1. Selecteer **installeren**.
1. Voltooi de wizard.

## <a name="create-a-resource-manager-template"></a>Resource Manager-sjabloon maken
Deze sectie wordt beschreven hoe de Azure Resource Manager-sjabloon die u gebruikt voor het maken van een virtuele machine van Azure op aanvraag wilt maken.

1. Voor het maken van een Resource Manager-sjabloon in uw abonnement, voert u de procedure in [Resource Manager-sjabloon gebruiken](devtest-lab-use-resource-manager-template.md).
1. Voordat u de Resource Manager-sjabloon genereren, voeg de [WinRM artefact](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) als onderdeel van het maken van de virtuele machine.

   WinRM toegang is vereist om te gebruiken zoals implementatietaken *Azure bestandskopie* en *PowerShell op de doelmachines*.

   > [!NOTE]
   > Wanneer u WinRM met een gedeelde IP-adres gebruikt, moet u een NAT-regel om een externe poort toewijzen aan de WinRM-poort toevoegen. Deze stap is niet vereist als u de virtuele machine met een openbaar IP-adres maken.
   >
   >

1. De sjabloon opslaan als een bestand op uw computer. Noem het bestand **CreateVMTemplate.json**.
1. Controleer de sjabloon uw bronbeheersysteem.
1. Open een teksteditor en plak het volgende script in de App.

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

1. Controleer het script in uw bronbeheersysteem. Noem deze ongeveer **GetLabVMParams.ps1**.

   Als u dit script uitvoert op de agent als onderdeel van de definitie van de release en als u de stappen voor taken zoals *Azure bestandskopie* of *PowerShell op de doelmachines*, het script verzamelt de waarden die u wilt uw app implementeren op de virtuele machine. Normaal gesproken gebruikt u deze taken apps te implementeren op een virtuele machine in Azure. De taken moet waarden zoals de naam van de VM-resourcegroep, IP-adres en FQDN-naam (FULLY).

## <a name="create-a-release-definition-in-release-management"></a>Een release-definitie maken in de Release Management
Voor het maken van de definitie van de release, het volgende doen:

1. Op de **Releases** tabblad van de **bouwen & Release** hub, selecteer de knop plusteken (+).
2. In de **maken release definitie** Selecteer de **leeg** sjabloon en selecteer vervolgens **volgende**.
3. Selecteer **Later kiezen**, en selecteer vervolgens **maken** de definitie van een nieuwe release met één standaard-omgeving en geen gekoppelde artefacten maken.
4. Om te openen in het snelmenu, in het nieuwe vrijgeven definition, selecteer het weglatingsteken (...) naast de naam van de omgeving en selecteer vervolgens **variabelen configureren**. 
5. In de **configureren - omgeving** venster voor de variabelen die u in de release definitie taken, voer de volgende waarden:

   a. Voor **vmName**, voer de naam die u hebt toegewezen aan de virtuele machine tijdens het maken van de Resource Manager-sjabloon in de Azure portal.

   b. Voor **gebruikersnaam**, geef de gebruikersnaam die u hebt toegewezen aan de virtuele machine tijdens het maken van de Resource Manager-sjabloon in de Azure portal.

   c. Voor **wachtwoord**, voer het wachtwoord die u hebt toegewezen aan de virtuele machine tijdens het maken van de Resource Manager-sjabloon in de Azure portal. Gebruik ' hangslot ' om te verbergen en het wachtwoord te beveiligen.

### <a name="create-a-vm"></a>Een virtuele machine maken

De volgende fase van de implementatie is het maken van de virtuele machine wilt gebruiken als de 'gouden installatiekopie' voor de volgende implementaties. U kunt de virtuele machine maken binnen uw Azure DevTest Lab-exemplaar met behulp van de taak die speciaal voor dit doel ontwikkeld. 

1. Selecteer in de definitie release **taken toevoegen**.
2. Op de **implementeren** tabblad, het toevoegen van een *Azure DevTest Labs maken VM* taak. De taak als volgt configureren:

   > [!NOTE]
   > Zie het maken van de virtuele machine moet worden gebruikt voor toekomstige implementaties [Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Voor **Azure RM-abonnement**, selecteert u een verbinding in de **beschikbare Azure serviceverbindingen** lijst of maak een meer beperkte machtigingen verbinding met uw Azure-abonnement. Zie voor meer informatie [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   b. Voor **Labnaam**, selecteert u de naam van het exemplaar dat u eerder hebt gemaakt.

   c. Voor **sjabloonnaam**, voer het volledige pad en de naam van de sjabloon die u hebt opgeslagen naar uw opslagplaats bron-code. U kunt de ingebouwde eigenschappen van Release Management gebruiken voor het vereenvoudigen van het pad, bijvoorbeeld:

   ```
   $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
   ```

   d. Voor **Sjabloonparameters**, geef de parameters voor de variabelen die zijn gedefinieerd in de sjabloon. Gebruik de namen van de variabelen die u hebt gedefinieerd in de omgeving:

   ```
   -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
   ```

   e. Voor **Uitvoervariabelen - ID van de virtuele testomgeving**, moet u de ID van de zojuist gemaakte virtuele machine voor de volgende stappen. Instellen van de standaardnaam van de omgevingsvariabele die automatisch wordt ingevuld met deze ID in de **Uitvoervariabelen** sectie. U kunt de variabele indien nodig bewerken, maar moet u de juiste naam gebruiken in de volgende taken. De ID van de virtuele testomgeving is geschreven in de volgende notatie:

   ```
   /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
   ```

3. Voer het script dat u eerder hebt gemaakt voor de details van DevTest Labs VM verzamelen. 
4. Selecteer in de definitie release **taken toevoegen** en klik op de **implementeren** tabblad, het toevoegen van een *Azure PowerShell* taak. De taak als volgt configureren:

   > [!NOTE]
   > Zie voor het verzamelen van de details van DevTest Labs VM [implementeren: Azure PowerShell](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/AzurePowerShell) en voer het script.

   a. Voor **Azure verbindingstype**, selecteer **Azure Resource Manager**.

   b. Voor **Azure RM-abonnement**, selecteer een verbinding in de lijst onder **beschikbare Azure serviceverbindingen**, of een meer beperkte machtigingen verbinding maken met uw Azure-abonnement. Zie voor meer informatie [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   c. Voor **scripttype**, selecteer **scriptbestand**.
 
   d. Voor **scriptpad**, voer het volledige pad en de naam van het script die u hebt opgeslagen naar uw opslagplaats bron-code. U kunt de ingebouwde eigenschappen van Release Management gebruiken voor het vereenvoudigen van het pad, bijvoorbeeld:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   e. Voor **scriptparameters**, voer de naam van de variabele die is automatisch ingevuld met de ID van het testlab VM door de vorige taak, bijvoorbeeld: 
      ```
      -labVmId '$(labVMId)'
      ```
    Het script worden de vereiste waarden verzameld en opgeslagen in omgevingsvariabelen in de release-definitie zodat u eenvoudig naar deze bij volgende stappen verwijzen kunt.

5. Uw app implementeren in de nieuwe DevTest Labs virtuele machine. De taken die u normaal gesproken gebruikt om de app te implementeren zijn *Azure bestandskopie* en *PowerShell op de doelmachines*.
   De informatie over de virtuele machine die u nodig hebt voor de parameters van deze taken wordt opgeslagen in drie variabelen met de naam **labVmRgName**, **labVMIpAddress**, en **labVMFqdn**in de release-definitie. Als u alleen om te experimenteren met het maken van een DevTest Labs virtuele machine en een aangepaste installatiekopie zonder tot het implementeren van een app wilt, kunt u deze stap overslaan.

### <a name="create-an-image"></a>Een installatiekopie maken

De volgende fase is het maken van een installatiekopie van de geïmplementeerde virtuele machine in uw Azure DevTest Labs-exemplaar. U kunt vervolgens de installatiekopie van het kopieën te maken van de virtuele machine op verzoek als u wilt een dev-taak uitvoeren of bepaalde tests worden uitgevoerd. 

1. Selecteer in de definitie release **taken toevoegen**.
2. Op de **implementeren** tabblad, het toevoegen van een **Azure DevTest Labs maken aangepaste installatiekopie** taak. Als volgt configureren:

   > [!NOTE]
   > Zie het maken van de installatiekopie van het [Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Voor **Azure RM-abonnement**, in de **beschikbare Azure serviceverbindingen** lijst, selecteert u een verbinding in de lijst of maak een meer beperkte machtigingen verbinding met uw Azure-abonnement. Zie voor meer informatie [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   b. Voor **Labnaam**, selecteert u de naam van het exemplaar dat u eerder hebt gemaakt.

   c. Voor **aangepaste installatiekopie met de naam**, voer een naam voor de aangepaste installatiekopie.

   d. (Optioneel) Voor **beschrijving**, voer een beschrijving waarmee u eenvoudig de juiste installatiekopie om later te selecteren.

   e. Voor **bron Lab VM - VM-ID van berichtbron Lab**, als u de standaardnaam van de omgevingsvariabele die automatisch is ingevuld met de ID van het testlab VM door een eerdere taak gewijzigd hier bewerken. De standaardwaarde is **$(labVMId)**.

   f. Voor **Uitvoervariabelen - ID van de afbeelding aangepast**, moet u de ID van de installatiekopie van het zojuist gemaakte wanneer u wilt beheren of te verwijderen. De standaardnaam van de omgevingsvariabele die automatisch wordt ingevuld met deze ID is ingesteld in de **Uitvoervariabelen** sectie. Indien nodig, kunt u de variabele bewerken.

### <a name="delete-the-vm"></a>De virtuele machine verwijderen

De laatste fase is het verwijderen van de virtuele machine die u hebt geïmplementeerd in uw Azure DevTest Labs-exemplaar. U zou de virtuele machine normaal verwijderen nadat u de dev-taken uitvoeren of de tests die u moet uitvoeren op de geïmplementeerde virtuele machine. 

1. Selecteer in de definitie release **taken toevoegen** en klik op de **implementeren** tabblad, het toevoegen van een *Azure DevTest Labs verwijderen VM* taak. Als volgt configureren:

      > [!NOTE]
      > Zie het verwijderen van de virtuele machine [Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Voor **Azure RM-abonnement**, selecteert u een verbinding in de **beschikbare Azure serviceverbindingen** lijst of maak een meer beperkte machtigingen verbinding met uw Azure-abonnement. Zie voor meer informatie [Azure Resource Manager-service-eindpunt](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
 
   b. Voor **ID van de virtuele testomgeving**, als u de standaardnaam van de omgevingsvariabele die automatisch is ingevuld met de ID van het testlab VM door een eerdere taak gewijzigd hier bewerken. De standaardwaarde is **$(labVMId)**.

2. Voer een naam voor de definitie van de release en sla deze op.
3. Een nieuwe release gemaakt, selecteert u de laatste build en deze implementeren in de één-omgeving in de definitie.

Vernieuw de weergave van uw exemplaar DevTest Labs in de Azure portal om weer te geven van de virtuele machine en de installatiekopie die worden gemaakt en de virtuele machine die opnieuw wordt verwijderd in elk stadium.

U kunt nu de aangepaste installatiekopie gebruiken voor het maken van virtuele machines wanneer ze vereist zijn.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [multi-VM-omgevingen maken met Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md).
* Ontdek meer Quick Start-Resource Manager-sjablonen voor het automatiseren van DevTest Labs vanuit de [openbare DevTest Labs GitHub-repo-](https://github.com/Azure/azure-quickstart-templates).
* Indien nodig, Zie de [VSTS probleemoplossing](https://docs.microsoft.com/vsts/build-release/actions/troubleshooting) pagina.
 
