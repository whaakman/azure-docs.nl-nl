---
title: Het maken van Windows Azure VM-installatiekopieën met verpakker | Microsoft Docs
description: Informatie over het gebruik van verpakker installatiekopieën maken van virtuele Windows-machines in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/29/2018
ms.author: iainfou
ms.openlocfilehash: b7f07ec8736086483f91746512f10118ee90762d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333155"
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a>Het gebruik van verpakker Windows-installatiekopieën voor virtuele machine maken in Azure
Elke virtuele machine (VM) in Azure wordt gemaakt van een afbeelding met de Windows-distributie en de versie van het besturingssysteem gedefinieerd. Voorbeelden van afbeeldingen zijn vooraf geïnstalleerde toepassingen en configuraties. Azure Marketplace biedt veel installatiekopieën van het eerste en derde partij voor de meest voorkomende OS en omgevingen met toepassingen, of u uw eigen aangepaste installatiekopieën die zijn afgestemd op uw behoeften kunt maken. Dit artikel wordt uitgelegd hoe u de open-source hulpprogramma [verpakker](https://www.packer.io/) om te definiëren en te maken van aangepaste installatiekopieën in Azure.


## <a name="create-azure-resource-group"></a>Azure-resourcegroep maken
Tijdens het maken maakt verpakker tijdelijke Azure-resources als de bron-VM-builds. Als u wilt vastleggen die bron-VM voor gebruik als een installatiekopie, moet u een resourcegroep definiëren. De uitvoer van het buildproces verpakker wordt opgeslagen in deze resourcegroep.

Maak een resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```powershell
$rgName = "myResourceGroup"
$location = "East US"
New-AzureRmResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a>Azure-referenties maken
Verpakker verifieert met Azure met behulp van een service-principal. Een Azure-service-principal is een beveiligings-id die u met apps, services en automatiseringsprogramma's zoals verpakker kunt gebruiken. U de machtigingen over welke bewerkingen die de service-principal in Azure uitvoeren kunt te definiëren en beheren.

Maken van een service principal met [nieuw AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) en toewijzen van machtigingen voor de service-principal maken en beheren van resources met [New AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```powershell
$sp = New-AzureRmADServicePrincipal -DisplayName "AzurePacker" `
    -Password (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force)
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Om te verifiëren naar Azure, moet u ook verkrijgen van uw Azure-tenant en de abonnement-id's met [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription):

```powershell
$sub = Get-AzureRmSubscription
$sub.TenantId[0]
$sub.SubscriptionId[0]
```

U gebruikt deze twee id's in de volgende stap.


## <a name="define-packer-template"></a>Verpakker sjabloon definiëren
Als u wilt maken van installatiekopieën, kunt u een sjabloon maken als een JSON-bestand. In de sjabloon definieert u opbouwfuncties en provisioners die het werkelijke buildproces uitvoeren. Verpakker heeft een [builder voor Azure](https://www.packer.io/docs/builders/azure.html) die kunt u voor het definiëren van de Azure-resources, zoals de Servicereferenties principal gemaakt in de voorgaande stap.

Maak een bestand met de naam *windows.json* en plak de volgende inhoud. Geef uw eigen waarden voor het volgende:

| Parameter                           | Waar u kunt verkrijgen |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Weergave service principal-ID met `$sp.applicationId` |
| *client_secret*                     | U hebt opgegeven in wachtwoord `$securePassword` |
| *tenant_id*                         | De uitvoer van `$sub.TenantId` opdracht |
| *subscription_id*                   | De uitvoer van `$sub.SubscriptionId` opdracht |
| *object_id*                         | Weergave service-principal-object-ID met `$sp.Id` |
| *managed_image_resource_group_name* | Naam van resourcegroep die u in de eerste stap hebt gemaakt |
| *managed_image_name*                | Naam voor de installatiekopie van de beheerde schijf die is gemaakt |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "0831b578-8ab6-40b9-a581-9a880a94aab1",
    "client_secret": "P@ssw0rd!",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "object_id": "a7dfb070-0d5b-47ac-b9a5-cf214fff0ae2",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": true,
    "winrm_insecure": true,
    "winrm_timeout": "3m",
    "winrm_username": "packer",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Add-WindowsFeature Web-Server",
      "& $env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit",
      "while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 10  } else { break } }"
    ]
  }]
}
```

Deze sjabloon maakt een virtuele machine van Windows Server 2016, installeert IIS en generaliseert van de virtuele machine met Sysprep. De IIS-installatie ziet u hoe u kunt de PowerShell-provisioner aanvullende opdrachten uit te voeren. De installatiekopie van het laatste verpakker bevat de vereiste software-installatie en configuratie.


## <a name="build-packer-image"></a>Verpakker installatiekopie maken
Als u nog niet geïnstalleerd op uw lokale machine verpakker hebt [Volg de instructies van de installatie verpakker](https://www.packer.io/docs/install/index.html).

Maak de installatiekopie door op te geven van uw verpakker sjabloonbestand als volgt:

```bash
./packer build windows.json
```

Een voorbeeld van de uitvoer van de bovenstaande opdrachten is als volgt:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> task : Image deployment
==> azure-arm:  ->> dept : Engineering
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the certificate’s URL ...
==> azure-arm:  -> Key Vault Name        : ‘pkrkvpq0mthtbtt’
==> azure-arm:  -> Key Vault Secret Name : ‘packerKeyVaultSecret’
==> azure-arm:  -> Certificate URL       : ‘https://pkrkvpq0mthtbtt.vault.azure.net/secrets/packerKeyVaultSecret/8c7bd823e4fa44e1abb747636128adbb'
==> azure-arm: Setting the certificate’s URL ...
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.55.35’
==> azure-arm: Waiting for WinRM to become available...
==> azure-arm: Connected to WinRM!
==> azure-arm: Provisioning with Powershell...
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-powershell-provisioner902510110
    azure-arm: #< CLIXML
    azure-arm:
    azure-arm: Success Restart Needed Exit Code      Feature Result
    azure-arm: ------- -------------- ---------      --------------
    azure-arm: True    No             Success        {Common HTTP Features, Default Document, D...
    azure-arm: <Objs Version=“1.1.0.1” xmlns=“http://schemas.microsoft.com/powershell/2004/04"><Obj S=“progress” RefId=“0"><TN RefId=“0”><T>System.Management.Automation.PSCustomObject</T><T>System.Object</T></TN><MS><I64 N=“SourceId”>1</I64><PR N=“Record”><AV>Preparing modules for first use.</AV><AI>0</AI><Nil /><PI>-1</PI><PC>-1</PC><T>Completed</T><SR>-1</SR><SD> </SD></PR></MS></Obj></Objs>
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-pq0mthtbtt/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Compute Name              : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

Het duurt enkele minuten duren voordat verpakker bouwen van de virtuele machine, het uitvoeren van de provisioners en het opschonen van de implementatie.


## <a name="create-a-vm-from-the-packer-image"></a>Een virtuele machine van de installatiekopie van het verpakker maken
U kunt nu een virtuele machine maken van de installatiekopie met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). De ondersteunende netwerkbronnen worden gemaakt als deze niet al bestaan. Voer desgevraagd een administratieve gebruikersnaam en het wachtwoord moet worden gemaakt op de virtuele machine. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* van *myPackerImage*:

```powershell
New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Image "myPackerImage"
```

Als u maken van virtuele machines in een andere resourcegroep of de regio dan uw installatiekopie verpakker wilt, geeft u de afbeeldings-ID in plaats van de installatiekopienaam van de. U kunt de afbeeldings-ID met verkrijgen [Get-AzureRmImage](/powershell/module/AzureRM.Compute/Get-AzureRmImage).

Het duurt enkele minuten aan de virtuele machine maken van uw installatiekopie verpakker.


## <a name="test-vm-and-webserver"></a>Testen van de virtuele machine en de webserver
Haal het openbare IP-adres van uw virtuele machine op met [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). In het volgende voorbeeld wordt het IP-adres opgehaald voor het eerder gemaakte *myPublicIP*:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIPAddress" | select "IpAddress"
```

Als u wilt zien van uw virtuele machine met de IIS installeren vanaf de provisioner verpakker in actie, Geef op het openbare IP-adres in naar een webbrowser.

![Standaardsite van IIS](./media/build-image-with-packer/iis.png) 


## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld gebruikt u verpakker maken van een VM-installatiekopie met IIS al geïnstalleerd. U kunt deze VM-installatiekopie samen met bestaande werkstromen voor de implementatie, zoals uw app implementeren in virtuele machines die zijn gemaakt op basis van de installatiekopie met Team Services, Ansible, Chef of Puppet.

Zie voor aanvullende voorbeeld verpakker sjablonen voor andere Windows-distributies, [deze GitHub-repo-](https://github.com/hashicorp/packer/tree/master/examples/azure).
