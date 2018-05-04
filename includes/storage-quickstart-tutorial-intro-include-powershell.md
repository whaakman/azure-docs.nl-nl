## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Connect-AzureRmAccount
```

Als u niet weet welke locatie u kunt gebruiken, kunt u een lijst met de beschikbare locaties weergeven. Selecteer de gewenste locatie in de lijst. In dit voorbeeld wordt **eastus** gebruikt. Sla deze locatie op in een variabele en gebruik de variabele zodat u de locatie op één plek kunt wijzigen.

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

## <a name="create-a-storage-account"></a>Create a storage account

Maak een opslagaccount voor algemene opslag met LRS-replicatie met behulp van [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) en haal vervolgens de opslagaccountcontext op waarin de te gebruiken opslagaccount is gedefinieerd. Als u werkt met een opslagaccount, verwijst u naar de context in plaats van herhaaldelijk de referenties op te geven. In dit voorbeeld wordt een opslagaccount met de naam *mystorageaccount* met lokaal redundante opslag (LRS) en blob-codering (standaard ingeschakeld) gemaakt.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```
