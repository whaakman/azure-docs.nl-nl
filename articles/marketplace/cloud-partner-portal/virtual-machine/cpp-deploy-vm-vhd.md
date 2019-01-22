---
title: Een VM implementeren vanaf uw VHD's voor de Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd over het registreren van een virtuele machine van een VHD voor Azure zijn geïmplementeerd.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/30/2018
ms.author: pbutlerm
ms.openlocfilehash: 3ab98669e01c9cfb2d4f46b8ddd83ff69653337b
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434169"
---
# <a name="deploy-a-vm-from-your-vhds"></a>Een VM implementeren vanaf uw VHD 's

Deze sectie wordt uitgelegd hoe u een virtuele machine (VM) implementeren van een Azure-geïmplementeerd virtuele harde schijf (VHD).  Geeft de hulpprogramma's die nodig zijn en hoe u kunt gebruiken voor het maken van een gebruiker VM-installatiekopie, en vervolgens implementeren op Azure met behulp van PowerShell-scripts.

Nadat u uw virtuele harde schijven (VHD's) hebt geüpload, het besturingssysteem gegeneraliseerde VHD en nul of meer gegevens schijf VHD's, naar uw Azure storage-account, kunt u deze registreren als een gebruiker VM-installatiekopie. Vervolgens kunt u die afbeelding testen. Omdat de VHD van het besturingssysteem is gegeneraliseerd, kunt u de virtuele machine niet rechtstreeks implementeren door op te geven van de VHD-URL.

Zie voor meer informatie over VM-installatiekopieën, de volgende blogberichten:

- [VM-installatiekopie](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM Image PowerShell 'Het'](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)


## <a name="prerequisite-install-the-necessary-tools"></a>Voorwaarde: Installeer de benodigde hulpprogramma 's

Als u hebt nog niet gedaan, installeert u Azure PowerShell en Azure CLI, met behulp van de volgende instructies:

- [Azure PowerShell installeren op Windows met PowerShellGet](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps)
- [Installeer Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="deployment-steps"></a>Implementatiestappen

U wilt de volgende stappen uit om te maken en implementeren van een gebruiker VM-installatiekopie gebruiken:

1. De gebruiker VM-installatiekopie, die inhoudt vast te leggen en de installatiekopie van het generaliseren maken. 
2. Certificaten maken en op te slaan in een nieuwe Azure Key Vault. Er is een certificaat vereist voor het tot stand brengen van een beveiligde WinRM-verbinding met de virtuele machine.  Een Azure Resource Manager-sjabloon en een Azure PowerShell-script worden geleverd. 
3. Implementeer de VM op basis van een gebruiker VM-installatiekopie met behulp van de opgegeven sjabloon en het script.

Nadat de virtuele machine is geïmplementeerd, bent u klaar om [certificeren van uw VM-installatiekopie](./cpp-certify-vm.md).

2.  Klik op **nieuw** en zoek naar de **sjabloonimplementatie**en selecteer vervolgens **bouw uw eigen sjabloon in de Editor**.  <br/>
  ![Sjabloon van de VHD-implementatie in Azure portal maken](./media/publishvm_021.png)

3. Kopieer en plak dit [JSON-sjabloon](./cpp-deploy-json-template.md) in de editor en klik op **opslaan**. <br/>
  ![VHD-implementatiesjabloon opslaan in Azure portal](./media/publishvm_022.png)

4. Geef de parameterwaarden voor de weergegeven **aangepaste implementatie** eigenschappenpagina's.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parameter**              |   **Beschrijving**                                                            |
   |  -------------              |   ---------------                                                            |
   | Gebruikersnaam van het Storage-Account   | Naam van het opslagaccount waar de gegeneraliseerde VHD zich bevindt                    |
   | Gebruikersnaam van het Storage-Container | Naam van de container waar de gegeneraliseerde VHD zich bevindt                          |
   | DNS-naam voor het openbare IP-adres      | Openbare IP-DNS-naam                                                           |
   | De naam van de gebruiker met beheerdersrechten             | Gebruikersnaam van de Administrator-account voor de nieuwe virtuele machine                                  |
   | Beheerderswachtwoord              | Wachtwoord van de Administrator-account voor de nieuwe virtuele machine                                  |
   | Het type besturingssysteem                     | VM-besturingssysteem: `Windows` \| `Linux`                                    |
   | Abonnements-id             | Id van het geselecteerde abonnement                                      |
   | Locatie                    | Geografische locatie van de implementatie                                        |
   | VM-grootte                     | [Azure VM-grootte](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), bijvoorbeeld `Standard_A2` |
   | Naam openbare IP-adres      | Naam van uw openbare IP-adres                                               |
   | VM-naam                     | Naam van de nieuwe virtuele machine                                                           |
   | Virtuele-netwerknaam        | Naam van het virtuele netwerk dat wordt gebruikt door de virtuele machine                                   |
   | Naam van de Bronnetwerkadapter                    | Naam van de netwerkinterfacekaart met het virtuele netwerk               |
   | VHD-URL                     | Volledige URL van de Besturingssysteemschijf VHD                                                     |
   |  |  |
            
5. Nadat u deze waarden opgeven, klikt u op **aankoop**. 

Azure wordt begint met implementeren: een nieuwe virtuele machine wordt gemaakt met de opgegeven niet-beheerde VHD in het pad van het opgegeven opslagaccount.  U kunt de voortgang volgen in Azure portal door te klikken op **virtuele Machines** aan de linkerkant van de portal.  Wanneer de virtuele machine is gemaakt, de status wordt gewijzigd van `Starting` naar `Running`. 


### <a name="deploy-a-vm-from-powershell"></a>Implementeren van een virtuele machine via PowerShell

Gebruik de volgende cmdlets voor het implementeren van een grote virtuele machine via de gegeneraliseerde VM-installatiekopie zojuist hebt gemaakt.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```


## <a name="next-steps"></a>Volgende stappen

Vervolgens wordt u [maken van een gebruiker VM-installatiekopie](cpp-create-user-image.md) voor uw oplossing.

