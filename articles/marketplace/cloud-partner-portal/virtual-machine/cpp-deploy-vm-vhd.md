---
title: Een VM implementeren vanaf uw VHD's voor de Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd over het registreren van een virtuele machine van een VHD voor Azure zijn geïmplementeerd.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 06ef4247d3cd7f87d763feb3f61cb8101d17a2e4
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877113"
---
# <a name="deploy-a-vm-from-your-vhds"></a>Een VM implementeren vanaf uw VHD 's

Dit artikel wordt uitgelegd hoe u een virtuele machine (VM) registreren van een Azure-geïmplementeerd virtuele harde schijf (VHD).  Een lijst met de hulpprogramma's die nodig zijn en hoe u kunt gebruiken voor het maken van een gebruiker VM-installatiekopie, en vervolgens implementeert naar Azure met behulp van de [Microsoft Azure portal](https://ms.portal.azure.com/) of PowerShell-scripts. 

Nadat u uw virtuele harde schijven (VHD's) hebt geüpload, het besturingssysteem gegeneraliseerde VHD en nul of meer gegevens schijf VHD's, naar uw Azure storage-account, kunt u deze registreren als een gebruiker VM-installatiekopie. Vervolgens kunt u die afbeelding testen. Omdat de VHD van het besturingssysteem is gegeneraliseerd, kunt u de virtuele machine niet rechtstreeks implementeren door op te geven van de VHD-URL.

Zie voor meer informatie over VM-installatiekopieën, de volgende blogberichten:

- [VM-installatiekopie](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM Image PowerShell 'Het'](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)


## <a name="set-up-the-necessary-tools"></a>Instellen van de benodigde hulpprogramma 's

Als u hebt nog niet gedaan, installeert u Azure PowerShell en Azure CLI, met behulp van de volgende instructies:

<!-- TD: Change the following URLs (in this entire topic) to relative paths.-->

- [Azure PowerShell installeren op Windows met PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)
- [Installeer Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="create-a-user-vm-image"></a>De VM-installatiekopie van een gebruiker maken

Vervolgens maakt u een niet-beheerde installatiekopie vanaf uw gegeneraliseerde VHD.

#### <a name="capture-the-vm-image"></a>De VM-installatiekopie vastleggen

Volg de instructies in het volgende artikel over het vastleggen van de virtuele machine die overeenkomt met de methode met toegang:

-  PowerShell: [over het maken van een niet-beheerde VM-installatiekopie van een Azure VM](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLI: [over het maken van een installatiekopie van een virtuele machine of VHD](../../../virtual-machines/linux/capture-image.md)
-  API: [Virtual Machines - vastleggen](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)

### <a name="generalize-the-vm-image"></a>Generaliseren van de VM-installatiekopie

Omdat u hebt de gebruikersinstallatiekopie gegenereerd op basis van een eerder gegeneraliseerde VHD, moet ook worden gegeneraliseerd.  Selecteer opnieuw het volgende artikel die overeenkomt met uw toegangsmechanisme voor.  (U mag hebben al gegeneraliseerd uw schijf wanneer u het vastgelegd.)

-  PowerShell: [de virtuele machine generaliseren](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLI: [stap 2: Maak VM-installatiekopie](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [Virtual Machines - generaliseren](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="deploy-a-vm-from-a-user-vm-image"></a>Een VM implementeren vanaf een VM-installatiekopie voor gebruikers

Vervolgens implementeert u een VM op basis van een gebruiker VM-installatiekopie met behulp van Azure portal of PowerShell.

<!-- TD: Recapture following hilited images and replace with red-box. -->

### <a name="deploy-a-vm-from-azure-portal"></a>Een virtuele machine van Azure portal implementeren

Gebruik het volgende proces voor het implementeren van uw gebruikers-VM vanuit de Azure-portal.

1.  Meld u aan bij de [Azure Portal](https://portal.azure.com).

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

Nadat de virtuele machine is geïmplementeerd, bent u klaar om [de virtuele machine configureren](./cpp-configure-vm.md).
