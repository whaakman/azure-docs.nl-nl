---
title: Een virtuele machine maken in DevTest Labs met Azure PowerShell | Microsoft Docs
description: Informatie over het gebruik van Azure DevTest Labs maken en beheren van virtuele machines met Azure PowerShell.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: df6d7e59c6c569a56162a738924b4ffd05b47da6
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896445"
---
# <a name="create-a-virtual-machine-with-devtest-labs-using-azure-powershell"></a>Een virtuele machine maken met DevTest Labs met behulp van Azure PowerShell
In dit artikel wordt beschreven hoe u een virtuele machine in Azure DevTest Labs maken met behulp van Azure PowerShell. U kunt PowerShell-scripts gebruiken voor het automatiseren van het maken van virtuele machines in een lab in Azure DevTest Labs. 

## <a name="prerequisites"></a>Vereisten
Voordat u begint:

- [Een lab maken](devtest-lab-create-lab.md) als u niet wilt gebruiken van een bestaande lab voor het testen van het script of de opdrachten in dit artikel. 
- [Installeer Azure PowerShell](/powershell/azure/azurerm/other-install) of Azure Cloud Shell die geïntegreerd in Azure portal gebruiken. 

## <a name="powershell-script"></a>PowerShell-script
Het voorbeeldscript in deze sectie maakt gebruik van de [Invoke-AzureRmResourceAction](/powershell/module/azurerm.resources/invoke-azurermresourceaction) cmdlet.  Deze cmdlet gebruikt van de testomgeving resource-ID, naam van de actie om uit te voeren (`createEnvironment`), en de parameters die nodig zijn die actie uit te voeren. De parameters zijn in een hash-tabel met de eigenschappen voor de beschrijving van virtuele machine. 

```powershell
[CmdletBinding()]

Param(
[Parameter(Mandatory = $false)]  $SubscriptionId,
[Parameter(Mandatory = $true)]   $LabResourceGroup,
[Parameter(Mandatory = $true)]   $LabName,
[Parameter(Mandatory = $true)]   $NewVmName,
[Parameter(Mandatory = $true)]   $UserName,
[Parameter(Mandatory = $true)]   $Password
)
 
pushd $PSScriptRoot

try {
    if ($SubscriptionId -eq $null) {
        $SubscriptionId = (Get-AzureRmContext).Subscription.SubscriptionId
    }

    $API_VERSION = '2016-05-15'
    $lab = Get-AzureRmResource -ResourceId "/subscriptions/$SubscriptionId/resourceGroups/$LabResourceGroup/providers/Microsoft.DevTestLab/labs/$LabName"

    if ($lab -eq $null) {
       throw "Unable to find lab $LabName resource group $LabResourceGroup in subscription $SubscriptionId."
    }

    #For this example, we are getting the first allowed subnet in the first virtual network
    #  for the lab.
    #If a specific virtual network is needed use | to find it. 
    #ie $virtualNetwork = @(Get-AzureRmResource -ResourceType  'Microsoft.DevTestLab/labs/virtualnetworks' -ResourceName $LabName -ResourceGroupName $lab.ResourceGroupName -ApiVersion $API_VERSION) | Where-Object Name -EQ "SpecificVNetName"

    $virtualNetwork = @(Get-AzureRmResource -ResourceType  'Microsoft.DevTestLab/labs/virtualnetworks' -ResourceName $LabName -ResourceGroupName $lab.ResourceGroupName -ApiVersion $API_VERSION)[0]

    $labSubnetName = $virtualNetwork.properties.allowedSubnets[0].labSubnetName

    #Prepare all the properties needed for the createEnvironment
    # call used to create the new VM.
    # The properties will be slightly different depending on the base of the vm
    # (a marketplace image, custom image or formula).
    # The setup of the virtual network to be used may also affect the properties.
    
    $parameters = @{
       "name"      = $NewVmName;
       "location"  = $lab.Location;
       "properties" = @{
          "labVirtualNetworkId"     = $virtualNetwork.ResourceId;
          "labSubnetName"           = $labSubnetName;
          "notes"                   = "Windows Server 2016 Datacenter";
          "osType"                  = "windows"
          "galleryImageReference"   = @{
             "offer"     = "WindowsServer";
             "publisher" = "MicrosoftWindowsServer";
             "sku"       = "2016-Datacenter";
             "osType"    = "Windows";
             "version"   = "latest"
          };
          "size"                    = "Standard_DS2_v2";
          "userName"                = $UserName;
          "password"                = $Password;
          "disallowPublicIpAddress" = $true;
       }
    }
    
    #The following line is the same as invoking
    # https://azure.github.io/projects/apis/#!/Labs/Labs_CreateEnvironment rest api

    Invoke-AzureRmResourceAction -ResourceId $lab.ResourceId -Action 'createEnvironment' -Parameters $parameters -ApiVersion $API_VERSION -Force -Verbose
}
finally {
   popd
}
```

De eigenschappen voor de virtuele machine in het bovenstaande script kunnen we het maken van een virtuele machine met Windows Server 2016 DataCenter als het besturingssysteem. Deze eigenschappen, kunnen enigszins verschillen voor elk type van de virtuele machine. De [virtuele machine definiëren](#define-virtual-machine) sectie wordt beschreven hoe u om te bepalen welke eigenschappen in dit script gebruiken.

De volgende opdracht geeft een voorbeeld van het script dat is opgeslagen in een bestandsnaam is uitgevoerd: Create-LabVirtualMachine.ps1. 

```powershell
 PS> .\Create-LabVirtualMachine.ps1 -ResourceGroupName 'MyLabResourceGroup' -LabName 'MyLab' -userName 'AdminUser' -password 'Password1!' -VMName 'MyLabVM'
```

## <a name="define-virtual-machine"></a>Virtuele machine definiëren
Deze sectie leest u hoe u Hiermee worden de eigenschappen die specifiek zijn voor een type virtuele machine die u wilt maken. 

### <a name="use-azure-portal"></a>Azure Portal gebruiken
Bij het maken van een virtuele machine in Azure portal, kunt u een Azure Resource Manager-sjabloon maken. U hoeft niet om het proces voor het maken van de virtuele machine te voltooien. U alleen stappen de tot u de sjabloon. Dit is de beste manier om op te halen van de beschrijving van de vereiste JSON als u nog geen een lab die virtuele machine gemaakt. 

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** in het linkermenu van de navigatie.
3. Zoek en selecteer **DevTest Labs** uit de lijst met services. 
4. Op de **DevTest Labs** pagina, selecteert u uw lab in de lijst met labs.
5. Selecteer op de startpagina van uw lab, **+ toevoegen** op de werkbalk. 
6. Selecteer een **basisinstallatiekopie** voor de virtuele machine. 
7. Selecteer **automatiseringsopties** onderaan de pagina de **indienen** knop. 
8. U ziet de **Azure Resource Manager-sjabloon** voor het maken van de virtuele machine. 
9. Het JSON-segment in de **resources** sectie bevat de definitie van het afbeeldingstype die u eerder hebt geselecteerd. 

    ```json
    {
      "apiVersion": "2018-10-15-preview",
      "type": "Microsoft.DevTestLab/labs/virtualmachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "labVirtualNetworkId": "[variables('labVirtualNetworkId')]",
        "notes": "Windows Server 2019 Datacenter",
        "galleryImageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2019-Datacenter",
          "osType": "Windows",
          "version": "latest"
        },
        "size": "[parameters('size')]",
        "userName": "[parameters('userName')]",
        "password": "[parameters('password')]",
        "isAuthenticationWithSshKey": false,
        "labSubnetName": "[variables('labSubnetName')]",
        "disallowPublicIpAddress": true,
        "storageType": "Standard",
        "allowClaim": false,
        "networkInterface": {
          "sharedPublicIpAddressConfiguration": {
            "inboundNatRules": [
              {
                "transportProtocol": "tcp",
                "backendPort": 3389
              }
            ]
          }
        }
      }
    }
    ```

In dit voorbeeld ziet u hoe u een definitie van een Azure Marketplace-installatiekopie. Op dezelfde manier krijgt u een definitie van een aangepaste installatiekopie, een formule of een omgeving. Alle artefacten die nodig zijn voor de virtuele machine toevoegen en instellen van een geavanceerde instellingen die vereist zijn. Na het opgeven van de waarden voor de vereiste velden en eventuele optionele velden, voordat u de **automatiseringsopties** knop.

### <a name="use-azure-rest-api"></a>Azure REST-API gebruiken
De volgende procedure bevat stappen voor het ophalen van eigenschappen van een installatiekopie met behulp van de REST-API: Deze stappen werken alleen voor een bestaande virtuele machine in een testomgeving. 

1. Navigeer naar de [Virtual Machines - lijst](/rest/api/dtl/virtualmachines/list) weergeeft, schakelt **uitproberen** knop. 
2. Selecteer uw **Azure-abonnement**.
3. Voer de **resourcegroep voor de testomgeving**.
4. Voer de **naam van het testlab**. 
5. Selecteer **Uitvoeren**.
6. U ziet de **eigenschappen voor de afbeelding** op basis van de virtuele machine is gemaakt. 



## <a name="next-steps"></a>Volgende stappen
Zie de volgende inhoud: [Azure PowerShell-documentatie voor Azure DevTest Labs](/powershell/module/az.devtestlabs/)