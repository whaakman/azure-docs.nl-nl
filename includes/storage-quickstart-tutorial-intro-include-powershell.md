## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

Als u niet welke locatie die u wilt gebruiken weet, kunt u de beschikbare locaties weergeven. Nadat de lijst wordt weergegeven, vinden die u wilt gebruiken. Dit voorbeeld gebruiken **eastus**. Slaat u dit in een variabele en gebruik van de variabele, zodat u dit op één plek wijzigen kunt.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Een standaard algemeen opslagaccount maken met het gebruik van de replicatie LRS [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), haal vervolgens de context van het opslagaccount waarin de storage-account moet worden gebruikt. Wanneer optreedt op een storage-account, maar u verwijzen naar de context in plaats van herhaaldelijk geven de referenties. In dit voorbeeld maakt u een opslagaccount aangeroepen *mystorageaccount* met lokaal redundante opslag en blob versleuteling ingeschakeld.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx = $storageAccount.Context
```
