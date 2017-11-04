## <a name="set-up-azure-powershell-for-azure-dns"></a>Azure PowerShell voor Azure DNS instellen

### <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u de volgende items hebt.

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* U moet de meest recente versie van de Azure Resource Manager PowerShell-cmdlets installeren. Zie [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) voor meer informatie.

### <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

Open de PowerShell-console en maak verbinding met uw account. Zie voor meer informatie [met behulp van PowerShell met Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a>Het abonnement selecteren
 
Controleer de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

Kies welk Azure-abonnement u wilt gebruiken.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Aangezien alle DNS-resources globaal en niet regionaal zijn, is de keuze van de locatie voor de resourcegroep niet van invloed op Azure DNS.

U kunt deze stap overslaan als u een bestaande resourcegroep gebruikt.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Resourceprovider registreren

De Azure DNS-service wordt beheerd door de Microsoft.Network-resourceprovider. Uw Azure-abonnement moet worden geregistreerd voor het gebruik van deze resourceprovider voordat u Azure DNS kunt gebruiken. Dit is een eenmalige bewerking voor elk abonnement.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```