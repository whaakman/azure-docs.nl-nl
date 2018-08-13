
## <a name="start-your-powershell-session"></a>Een PowerShell-sessie starten
U moet eerst de meest recente versie [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) geïnstalleerd en worden uitgevoerd. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> De voorbeelden in dit onderwerp gebruiken [Azure Resource Manager-implementatiemodel](../articles/azure-resource-manager/resource-group-overview.md), zodat de voorbeelden gebruiken de [Azure Resource Manager-cmdlets](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Voer de [ **Connect-AzureRmAccount** ](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) -cmdlet en u krijgt een aanmeldingsscherm uw referenties in te voeren. Gebruik de referenties waarmee u zich aanmeldt bij de Azure-portal.

    Connect-AzureRmAccount

Als u meerdere abonnementen gebruiken de [ **Set-AzureRmContext** ](https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext) cmdlet om te selecteren welk abonnement uw PowerShell-sessie wilt gebruiken. Voer [**Get-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermcontext) uit als u wilt zien welke abonnement door de huidige PowerShell-sessie wordt gebruikt. Voer [**Get-AzureRmSubscription**](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/get-azurermsubscription) uit als u al uw abonnementen wilt weergeven.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

