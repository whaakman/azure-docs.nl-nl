## <a name="setting-up-powershell-for-resource-manager-templates"></a>PowerShell in te stellen voor Resource Manager-sjablonen
Voordat u Azure PowerShell met Resource Manager gebruiken kunt, moet u het recht Windows PowerShell en Azure PowerShell versies hebben.

### <a name="verify-powershell-versions"></a>Controleer of de PowerShell-versies
Controleer of dat u Windows PowerShell versie 3.0 of 4.0 hebt. Ga voor de versie van Windows PowerShell, typ de volgende opdracht achter de opdrachtprompt van Windows PowerShell.

    $PSVersionTable

U ontvangt het volgende type informatie:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Controleer de waarde van **PSVersion** 3.0 of 4.0. Als dit niet het geval is, Zie [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Uw Azure-account en -abonnement instellen
Als u nog geen Azure-abonnement hebt, kunt u activeren uw [voordelen als MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

Open een Azure PowerShell-opdrachtprompt en aanmelden bij Azure met deze opdracht.

    Connect-AzureRmAccount

Als u meerdere Azure-abonnementen hebt, kunt u uw Azure-abonnementen met deze opdracht weergeven.

    Get-AzureRmSubscription

U ontvangt het volgende type informatie:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

U kunt de huidige Azure-abonnement instellen door het uitvoeren van deze opdrachten bij de Azure PowerShell-opdrachtprompt. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens lang zijn en de juiste naam.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Zie voor meer informatie over Azure-abonnementen en accounts [hoe: verbinding maken met uw abonnement](/powershell/azureps-cmdlets-docs#step-3-connect).

