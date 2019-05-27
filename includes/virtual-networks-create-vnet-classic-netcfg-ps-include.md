---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: bda289e73b9a782cd56c0c94b8f53e8002b1ccf4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66116854"
---
## <a name="how-to-create-a-virtual-network-using-a-network-config-file-from-powershell"></a>Over het maken van een virtueel netwerk met een netwerk-configuratiebestand vanuit PowerShell
Azure maakt gebruik van een xml-bestand voor het definiëren van alle virtuele netwerken die beschikbaar op een abonnement. U kunt dit bestand downloaden, kunt u deze wijzigen of verwijderen van bestaande virtuele netwerken bewerken en maken van nieuwe virtuele netwerken. In deze zelfstudie hebt u meer informatie over het downloaden van dit bestand, aangeduid als network configuration netcfg)-bestand (of, en bewerken voor het maken van een nieuw virtueel netwerk. Zie voor meer informatie over het netwerkconfiguratiebestand, de [Azure virtual network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).

Voor het maken van een virtueel netwerk met een netcfg-bestand met behulp van PowerShell, voert u de volgende stappen uit:

1. Als u Azure PowerShell nog nooit hebt gebruikt, voert u de stappen in de [installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs) artikel, en vervolgens aanmelden bij Azure en selecteer uw abonnement.
2. Vanuit de Azure PowerShell-console, gebruiken de **Get-AzureVnetConfig** cmdlet voor het downloaden van het netwerkconfiguratiebestand naar een map op uw computer met de volgende opdracht: 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Verwachte uitvoer:
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Open het bestand dat u hebt opgeslagen in stap 2 met behulp van een toepassing XML- of -editor en zoek naar de  **\<VirtualNetworkSites >** element. Als u geen netwerken hebt gemaakt hebt, elk netwerk wordt weergegeven als een eigen  **\<VirtualNetworkSite >** element.
4. Voor het maken van het virtuele netwerk dat wordt beschreven in dit scenario, net onder het volgende XML-bestand toevoegen de  **\<VirtualNetworkSites >** element:

   ```xml
         <?xml version="1.0" encoding="utf-8"?>
         <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
           <VirtualNetworkConfiguration>
             <VirtualNetworkSites>
                 <VirtualNetworkSite name="TestVNet" Location="East US">
                   <AddressSpace>
                     <AddressPrefix>192.168.0.0/16</AddressPrefix>
                   </AddressSpace>
                   <Subnets>
                     <Subnet name="FrontEnd">
                       <AddressPrefix>192.168.1.0/24</AddressPrefix>
                     </Subnet>
                     <Subnet name="BackEnd">
                       <AddressPrefix>192.168.2.0/24</AddressPrefix>
                     </Subnet>
                   </Subnets>
                 </VirtualNetworkSite>
             </VirtualNetworkSites>
           </VirtualNetworkConfiguration>
         </NetworkConfiguration>
   ```
   
5. Sla het configuratiebestand van het netwerk.
6. Vanuit de Azure PowerShell-console, gebruiken de **Set AzureVnetConfig** cmdlet voor het uploaden van het netwerkconfiguratiebestand met de volgende opdracht: 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Geretourneerd uitvoer:
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Als **OperationStatus** is niet *geslaagd* in de uitvoer van de geretourneerde opnieuw het xml-bestand voor fouten en volledige stap 6 controleren.

7. Vanuit de Azure PowerShell-console, gebruiken de **Get-AzureVnetSite** cmdlet om te controleren of het nieuwe netwerk is toegevoegd door het uitvoeren van de volgende opdracht uit: 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   De uitvoer van de geretourneerde (afgekort) bevat de volgende tekst:
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
