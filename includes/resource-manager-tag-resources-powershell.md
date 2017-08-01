Versie 3.0 van de module AzureRm.Resources bevatte belangrijke wijzigingen in de manier waarop u tags kunt gebruiken. Controleer uw versie voordat u doorgaat:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Als uw resultaten versie 3.0 of hoger weergeven, werken de voorbeelden in dit onderwerp met uw omgeving. Als u niet over versie 3.0 of hoger beschikt, [werkt u uw versie bij](/powershell/azureps-cmdlets-docs/) met behulp van PowerShell Gallery of Web Platform Installer voordat u verdergaat met dit onderwerp.

```powershell
Version
-------
3.5.0
```

Gebruik het volgende om de bestaande tags van een *resourcegroep* te bekijken:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Dat script retourneert de volgende indeling:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Gebruik het volgende om de bestaande tags te bekijken van een *resource met een opgegeven bron-id*:

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

Of gebruik het volgende om de bestaande tags te bekijken van een *resource met een opgegeven naam en resourcegroep*:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Gebruik het volgende om de *resourcegroepen met een specifieke tag* op te halen:

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

Gebruik het volgende om de *resources met een specifieke tag* op te halen:

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

Wanneer u tags op een resource of resourcegroep toepast, overschrijft u de bestaande tags van die resource of resourcegroep. Als u een resource of resourcegroep hebt met bestaande tags, gaat u anders te werk dan wanneer dat niet het geval is. 

Gebruik het volgende om tags toe te voegen aan een *resourcegroep zonder bestaande tags*:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Als u tags wilt toevoegen aan een *resourcegroep met bestaande tags*, haalt u de bestaande tags op, voegt u de nieuwe tag toe en past u de tags opnieuw toe:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

Gebruik het volgende om tags toe te voegen aan een *resource zonder bestaande tags*:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

Gebruik het volgende om tags toe te voegen aan een *resource met bestaande tags*:

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen *zonder de bestaande tags voor de resources te behouden*:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen *en de bestaande tags voor de resources die geen dubbele waarden zijn, wilt behouden*:

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

Geef een lege hash-tabel door om alle tags te verwijderen:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



