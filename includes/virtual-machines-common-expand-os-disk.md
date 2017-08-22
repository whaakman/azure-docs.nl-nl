## <a name="overview"></a>Overzicht
Wanneer u een nieuwe virtuele machine (VM) maakt in een resourcegroep door een installatiekopie van [Azure Marketplace](https://azure.microsoft.com/marketplace/) te implementeren, is het standaardstation voor het besturingssysteem 127 GB groot. Hoewel het mogelijk is om gegevensschijven toe te voegen aan de VM (het aantal is afhankelijk van de SKU die u hebt gekozen) en het bovendien wordt aangeraden om toepassingen en CPU-intensieve werkbelastingen te installeren op deze aanvullende schijven, moeten klanten vaak ook de besturingssysteemschijf uitbreiden om bepaalde scenario's te ondersteunen, zoals de volgende:

1. Ondersteuning voor oudere toepassingen die onderdelen op de besturingssysteemschijf installeren.
2. Een fysieke computer of virtuele machine migreren van on-premises met een grotere besturingssysteemschijf.

> [!IMPORTANT]
> In Azure zijn twee verschillende implementatiemodellen beschikbaar voor het maken van en werken met resources: Resource Manager en het klassieke model. In dit artikel wordt het Resource Manager-model beschreven. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> 
> 

## <a name="resize-the-os-drive"></a>Besturingssysteemschijf uitbreiden
In dit artikel gaan we de grootte van de besturingssysteemschijf aanpassen met behulp van Resource Manager-modules van [Azure Powershell](/powershell/azureps-cmdlets-docs). Open de Powershell ISE of het Powershell-venster in de beheerdersmodus en voer de volgende stappen uit:

1. Meld u in de modus voor resourcebeheer aan bij uw Microsoft Azure-account en selecteer uw abonnement als volgt:
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Stel de naam van de resourcegroep en de VM als volgt in:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Vraag als volgt een verwijzing op naar uw VM:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Ga als volgt te werk om de VM te stoppen voordat u de grootte van de schijf aanpast:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. En dan nu het moment waarop we allemaal gewacht hebben! Stel de grootte van de besturingssysteemschijf in op de gewenste waarde en werk de VM als volgt bij:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > De nieuwe grootte moet groter zijn dan de bestaande schijfgrootte. De toegestane maximale waarde is 1023 GB.
   > 
   > 
6. Het bijwerken van de VM kan een paar seconden duren. Zodra de opdracht is voltooid, start u de VM als volgt opnieuw op:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Dat is alles. Ga nu met RDP naar de VM, open Computerbeheer (of Schijfbeheer) en vouw het station met de zojuist toegewezen ruimte uit.

## <a name="summary"></a>Samenvatting
In dit artikel hebben we Azure Resource Manager-modules van Powershell gebruikt om de besturingssysteemschijf van een virtuele IaaS-machine uit te breiden. Hieronder ziet u het volledige script ter referentie:

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a>Volgende stappen
Hoewel we in dit artikel voornamelijk hebben gekeken naar het uitbreiden van de besturingssysteemschijf van de VM, kan het script ook worden gebruikt voor het uitbreiden van de gegevensschijven die zijn gekoppeld aan de VM. Hiervoor hoeft maar één regel met code te worden gewijzigd. Als u bijvoorbeeld de eerste gegevensschijf die is gekoppeld aan de VM wilt uitbreiden, vervangt u het object ```OSDisk``` van ```StorageProfile``` door de matrix ```DataDisks``` en gebruikt u een numerieke index om een verwijzing naar de eerste gekoppelde schijf te verkrijgen, zoals hieronder wordt weergegeven:

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
Op dezelfde manier kunt u verwijzen naar andere gegevensschijven die aan de VM zijn gekoppeld. Dit kan met behulp van een index, zoals hierboven, of met de eigenschap ```Name``` van de schijf, zoals hieronder wordt weergegeven:

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

Als u wilt weten hoe u schijven koppelt aan een Azure Resource Manager-VM, raadpleegt u dit [artikel](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

