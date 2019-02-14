---
title: Azure CDN met PowerShell beheren | Microsoft Docs
description: Informatie over het gebruik van de Azure PowerShell-cmdlets voor het beheren van Azure CDN.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: magattus
ms.openlocfilehash: 5dc05b08e84decf958481d19b9cfb820947fae78
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237041"
---
# <a name="manage-azure-cdn-with-powershell"></a>Azure CDN met PowerShell beheren
PowerShell biedt een van de meest flexibele methoden voor het beheren van uw Azure CDN-profielen en eindpunten.  U kunt PowerShell interactief of door het schrijven van scripts gebruiken om beheertaken te automatiseren.  Deze zelfstudie ziet u enkele van de meest algemene taken die u kunt uitvoeren met PowerShell voor het beheren van uw Azure CDN-profielen en eindpunten.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voor het gebruik van PowerShell om uw Azure CDN-profielen en de eindpunten te beheren, moet u de Azure PowerShell-module geïnstalleerd hebben.  Voor meer informatie over hoe u Azure PowerShell installeren en verbinding maken met Azure via de `Connect-AzAccount` cmdlet, Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

> [!IMPORTANT]
> U moet zich aanmelden met `Connect-AzAccount` voordat u Azure PowerShell-cmdlets kunt uitvoeren.
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a>Lijst van de Azure CDN-cmdlets
U kunt vindt u al de Azure CDN-cmdlets met behulp van de `Get-Command` cmdlet.

```text
PS C:\> Get-Command -Module Az.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzCdnCustomDomain                         2.0.0      Az.Cdn
Cmdlet          Get-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          Get-AzCdnEndpointNameAvailability             2.0.0      Az.Cdn
Cmdlet          Get-AzCdnOrigin                               2.0.0      Az.Cdn
Cmdlet          Get-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Get-AzCdnProfileSsoUrl                        2.0.0      Az.Cdn
Cmdlet          New-AzCdnCustomDomain                         2.0.0      Az.Cdn
Cmdlet          New-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          New-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Publish-AzCdnEndpointContent                  2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnCustomDomain                      2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnEndpoint                          2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnProfile                           2.0.0      Az.Cdn
Cmdlet          Set-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          Set-AzCdnOrigin                               2.0.0      Az.Cdn
Cmdlet          Set-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Start-AzCdnEndpoint                           2.0.0      Az.Cdn
Cmdlet          Stop-AzCdnEndpoint                            2.0.0      Az.Cdn
Cmdlet          Test-AzCdnCustomDomain                        2.0.0      Az.Cdn
Cmdlet          Unpublish-AzCdnEndpointContent                2.0.0      Az.Cdn
```

## <a name="getting-help"></a>Ondersteuning vragen
U kunt hulp krijgen met een van deze cmdlets met behulp van de `Get-Help` cmdlet.  `Get-Help` biedt gebruik en de syntaxis en (optioneel) ziet u voorbeelden.

```text
PS C:\> Get-Help Get-AzCdnProfile

NAME
    Get-AzCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzCdnProfile -examples".
    For more information, type: "get-help Get-AzCdnProfile -detailed".
    For technical information, type: "get-help Get-AzCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Aanbieding bestaande Azure CDN-profielen
De `Get-AzCdnProfile` cmdlet zonder parameters haalt alle uw bestaande CDN-profielen.

```powershell
Get-AzCdnProfile
```

Deze uitvoer kan worden doorgesluisd naar cmdlets voor opsomming.

```powershell
# Output the name of all profiles on this subscription.
Get-AzCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzCdnProfile | Where-Object { $_.Sku.Name -eq "Standard_Verizon" }
```

U kunt ook één profiel terugkeren door de naam en resourcegroep profielgroep op te geven.

```powershell
Get-AzCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> Het is mogelijk om meerdere CDN-profielen met dezelfde naam, zolang ze zich in verschillende resourcegroepen bevinden.  Als u weglaat de `ResourceGroupName` parameter retourneert alle profielen met een overeenkomende naam.
> 
> 

## <a name="listing-existing-cdn-endpoints"></a>Aanbieding bestaande CDN-eindpunten
`Get-AzCdnEndpoint` een afzonderlijk eindpunt of de eindpunten in een profiel kunnen worden opgehaald.  

```powershell
# Get a single endpoint.
Get-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzCdnProfile | Get-AzCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzCdnProfile | Get-AzCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Het maken van het CDN-profielen en -eindpunten
`New-AzCdnProfile` en `New-AzCdnEndpoint` worden gebruikt voor het maken van CDN-profielen en -eindpunten. De volgende SKU's worden ondersteund:
- Standard_Verizon
- Premium_Verizon
- Custom_Verizon
- Standard_Akamai
- Standard_Microsoft
- Standard_ChinaCdn

```powershell
# Create a new profile
New-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US"

# Create a new endpoint
New-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US" | New-AzCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Beschikbaarheid van de eindpunt-naam controleren
`Get-AzCdnEndpointNameAvailability` retourneert een object waarmee wordt aangegeven of de naam van een eindpunt beschikbaar is.

```powershell
# Retrieve availability
$availability = Get-AzCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Een aangepast domein toevoegen
`New-AzCdnCustomDomain` Hiermee voegt u een aangepaste domeinnaam toe aan een bestaand eindpunt.

> [!IMPORTANT]
> U moet instellen de CNAME met uw DNS-provider zoals beschreven in [aangepast domein toewijzen aan Content Delivery Network (CDN) eindpunt](cdn-map-content-to-custom-domain.md).  U kunt de toewijzing voor het wijzigen van uw eindpunt met testen `Test-AzCdnCustomDomain`.
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Wijzigen van een eindpunt
`Set-AzCdnEndpoint` Hiermee wijzigt u een bestaand eindpunt.

```powershell
# Get an existing endpoint
$endpoint = Get-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Bezig met het verwijderen/Pre-loading CDN activa
`Unpublish-AzCdnEndpointContent` worden in de cache opgeslagen activa, terwijl `Publish-AzCdnEndpointContent` vooraf geladen assets op ondersteunde eindpunten.

```powershell
# Purge some assets.
Unpublish-AzCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzCdnProfile | Get-AzCdnEndpoint | Unpublish-AzCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Starten/stoppen CDN-eindpunten
`Start-AzCdnEndpoint` en `Stop-AzCdnEndpoint` kan worden gebruikt om te starten en stoppen van afzonderlijke eindpunten of groepen van eindpunten.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzCdnProfile | Get-AzCdnEndpoint | Stop-AzCdnEndpoint

# Start all endpoints
Get-AzCdnProfile | Get-AzCdnEndpoint | Start-AzCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN-resources verwijderen
`Remove-AzCdnProfile` en `Remove-AzCdnEndpoint` kan worden gebruikt om profielen en -eindpunten te verwijderen.

```powershell
# Remove a single endpoint
Remove-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzCdnEndpoint | Remove-AzCdnEndpoint -Force

# Remove a single profile
Remove-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe u Azure CDN kunt automatiseren met [.NET](cdn-app-dev-net.md) of [Node.js](cdn-app-dev-node.md).

Zie voor meer informatie over CDN-functies, [overzicht van CDN](cdn-overview.md).

