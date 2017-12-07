## <a name="how-to-create-a-virtual-network-using-a-network-config-file-from-powershell"></a>Het maken van een virtueel netwerk met een configuratiebestand netwerk vanuit PowerShell
Azure maakt gebruik van een xml-bestand voor het definiëren van alle virtuele netwerken die beschikbaar zijn in een abonnement. U kunt dit bestand downloaden, bewerkt om te wijzigen of verwijderen van de bestaande virtuele netwerken en nieuwe virtuele netwerken maken. In deze zelfstudie u meer informatie over het downloaden van dit bestand, netwerk-configuratie (of netcfg) bestand genoemd en bewerken om te maken van een nieuw virtueel netwerk. Zie voor meer informatie over het configuratiebestand van het netwerk, de [Azure virtual network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).

Als een virtueel netwerk maken met een netcfg-bestand met behulp van PowerShell, moet u de volgende stappen uitvoeren:

1. Als u Azure PowerShell nog nooit hebt gebruikt, voer de stappen in de [installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs) artikel, vervolgens aanmelden bij Azure en uw abonnement te selecteren.
2. Via de Azure PowerShell-console, gebruiken de **Get-AzureVnetConfig** cmdlet voor het downloaden van het configuratiebestand van het netwerk naar een map op uw computer met de volgende opdracht: 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Verwachte uitvoer:
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Open het bestand dat u in stap 2, gebruikmakend van een XML- of -editor toepassing opgeslagen en zoekt u de  **<VirtualNetworkSites>**  element. Als er geen netwerken hebt gemaakt, elk netwerk wordt weergegeven als een eigen  **<VirtualNetworkSite>**  element.
4. Toevoegen voor het maken van het virtuele netwerk in dit scenario wordt de volgende XML net onder de  **<VirtualNetworkSites>**  element:

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
6. Via de Azure PowerShell-console, gebruiken de **Set AzureVnetConfig** cmdlet voor het uploaden van het configuratiebestand van het netwerk met de volgende opdracht: 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Geretourneerd uitvoer:
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Als **OperationStatus** is niet *geslaagd* in de uitvoer van de geretourneerde controleren op fouten en volledige stap 6 van het xml-bestand opnieuw.

7. Via de Azure PowerShell-console, gebruiken de **Get-AzureVnetSite** cmdlet om te controleren of het nieuwe netwerk is toegevoegd met de volgende opdracht: 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   De uitvoer van de geretourneerde (afgekort) omvat de volgende tekst:
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
