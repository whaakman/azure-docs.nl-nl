Voordat u begint met deze configuratie, moet u zich aanmelden bij uw Azure-account. De cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-account. Na het aanmelden, worden de instellingen van uw account gedownload zodat ze beschikbaar zijn voor Azure PowerShell. Zie [Using Windows PowerShell with Resource Manager](../articles/powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account om u aan te melden. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Login-AzureRmAccount
```

Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

Geef het abonnement op dat u wilt gebruiken.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```