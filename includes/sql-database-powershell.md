
## <a name="start-your-powershell-session"></a>Een PowerShell-sessie starten
Eerst moet u de nieuwste versie van Azure PowerShell installeren en uitvoeren. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Veel nieuwe functies van SQL Database worden alleen ondersteund als u het [Azure Resource Manager-implementatiemodel](../articles/azure-resource-manager/resource-group-overview.md) gebruikt. Daarom worden in voorbeelden de [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) voor Resource Manager gebruikt. De [cmdlets van Azure SQL Database Service Management](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) van het klassieke Service Management-implementatiemodel worden ondersteund voor compatibiliteit met eerdere versies, maar we raden u aan om de Resource Manager-cmdlets te gebruiken.
> 
> 

Wanneer u de cmdlet [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) uitvoert, wordt er een aanmeldingsscherm geopend waarin u uw referenties kunt invoeren. Gebruik de referenties waarmee u zich aanmeldt bij de Azure-portal.

```PowerShell
Add-AzureRmAccount
```

Als u meerdere abonnementen hebt, gebruik dan de cmdlet [**Set-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) om te selecteren welk abonnement u voor de PowerShell-sessie wilt gebruiken. Voer [**Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx) uit als u wilt zien welke abonnement door de huidige PowerShell-sessie wordt gebruikt. Voer [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx) uit als u al uw abonnementen wilt weergeven.

```PowerShell
Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```
