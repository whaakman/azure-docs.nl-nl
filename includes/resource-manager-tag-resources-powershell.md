Versie 3.0 van de module AzureRm.Resources bevatte belangrijke wijzigingen in de manier waarop u tags kunt gebruiken. Controleer uw versie voordat u doorgaat:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Als uw resultaten versie 3.0 of hoger weergeven, werken de voorbeelden in dit onderwerp met uw omgeving. Als u niet versie 3.0 of hoger hebt, [werkt u uw versie bij](/powershell/azureps-cmdlets-docs/) met behulp van PowerShell Gallery of Web Platform Installer voordat u verdergaat met dit onderwerp.

```powershell
Version
-------
3.5.0
```

Wanneer u tags op een resource of resourcegroep toepast, overschrijft u de bestaande tags van die resource of resourcegroep. Als u een resource of resourcegroep hebt waarvan u de bestaande tags wilt behouden, gaat u anders te werk dan wanneer dat niet het geval is. Tags toevoegen aan een:

* resourcegroep zonder bestaande tags.

  ```powershell
  Set-AzureRmResourceGroup -Name TagTestGroup -Tag @{ Dept="IT"; Environment="Test" }
  ```

* resourcegroep met bestaande tags.

  ```powershell
  $tags = (Get-AzureRmResourceGroup -Name TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResourceGroup -Tag $tags -Name TagTestGroup
  ```

* resource zonder bestaande tags.

  ```powershell
  Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

* resource met bestaande tags.

  ```powershell
  $tags = (Get-AzureRmResource -ResourceName storageexample -ResourceGroupName TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResource -Tag $tags -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen **zonder de bestaande tags voor de resources te behouden**:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen **en de bestaande tags voor de resources die geen dubbele waarden zijn, wilt behouden**:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Geef een lege hash-tabel door om alle tags te verwijderen.

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name TagTestGgroup
```

Gebruik de cmdlet `Find-AzureRmResourceGroup` om resourcegroepen met een specifieke tag op te halen.

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

Gebruik de cmdlet `Find-AzureRmResource` om alle resources met een bepaalde tag en waarde op te halen.

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```



<!--HONumber=Feb17_HO3-->


