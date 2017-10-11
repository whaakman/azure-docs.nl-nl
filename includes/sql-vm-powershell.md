
## <a name="start-your-powershell-session"></a>Een PowerShell-sessie starten
U moet eerst de meest recente hebben [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) geïnstalleerd en wordt uitgevoerd. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> De voorbeelden in dit onderwerp gebruiken [Azure Resource Manager-implementatiemodel](../articles/azure-resource-manager/resource-group-overview.md), zodat de voorbeelden gebruiken de [Azure Resource Manager-cmdlets](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Voer de [ **Add-AzureRmAccount** ](http://msdn.microsoft.com/library/mt619267.aspx) cmdlet en u krijgt een teken in het scherm uw referenties in te voeren. Gebruik de referenties waarmee u zich aanmeldt bij Azure Portal.

    Add-AzureRmAccount

Als u meerdere abonnementen gebruiken de [ **Set-AzureRmContext** ](http://msdn.microsoft.com/library/mt619263.aspx) cmdlet om te selecteren welke abonnement u uw PowerShell-sessie moet worden gebruikt. Voer [**Get-AzureRmContext**](http://msdn.microsoft.com/library/mt619265.aspx) uit als u wilt zien welke abonnement door de huidige PowerShell-sessie wordt gebruikt. Voer [**Get-AzureRmSubscription**](http://msdn.microsoft.com/library/mt619284.aspx) uit als u al uw abonnementen wilt weergeven.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

