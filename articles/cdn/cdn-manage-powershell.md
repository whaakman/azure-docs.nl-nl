---
title: Azure CDN met PowerShell beheren | Microsoft Docs
description: Informatie over het gebruik van de Azure PowerShell-cmdlets voor het beheren van Azure CDN.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: mazha
ms.openlocfilehash: 15feb7b1d2873bc3f088eaad78079df2e063d73b
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114069"
---
# <a name="manage-azure-cdn-with-powershell"></a>Azure CDN met PowerShell beheren
PowerShell biedt een van de meest flexibele methoden voor het beheren van uw Azure CDN-profielen en eindpunten.  U kunt PowerShell interactief of door het schrijven van scripts gebruiken om beheertaken te automatiseren.  Deze zelfstudie ziet u enkele van de meest algemene taken die u kunt uitvoeren met PowerShell voor het beheren van uw Azure CDN-profielen en eindpunten.

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van PowerShell om uw Azure CDN-profielen en de eindpunten te beheren, moet u de Azure PowerShell-module geïnstalleerd hebben.  Voor meer informatie over hoe u Azure PowerShell installeren en verbinding maken met Azure via de `Connect-AzureRmAccount` cmdlet, Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

> [!IMPORTANT]
> U moet zich aanmelden met `Connect-AzureRmAccount` voordat u Azure PowerShell-cmdlets kunt uitvoeren.
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a>Lijst van de Azure CDN-cmdlets
U kunt vindt u al de Azure CDN-cmdlets met behulp van de `Get-Command` cmdlet.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Ondersteuning vragen
U kunt hulp krijgen met een van deze cmdlets met behulp van de `Get-Help` cmdlet.  `Get-Help` biedt gebruik en de syntaxis en (optioneel) ziet u voorbeelden.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Aanbieding bestaande Azure CDN-profielen
De `Get-AzureRmCdnProfile` cmdlet zonder parameters haalt alle uw bestaande CDN-profielen.

```powershell
Get-AzureRmCdnProfile
```

Deze uitvoer kan worden doorgesluisd naar cmdlets voor opsomming.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "Standard_Verizon" }
```

U kunt ook één profiel terugkeren door de naam en resourcegroep profielgroep op te geven.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> Het is mogelijk om meerdere CDN-profielen met dezelfde naam, zolang ze zich in verschillende resourcegroepen bevinden.  Als u weglaat de `ResourceGroupName` parameter retourneert alle profielen met een overeenkomende naam.
> 
> 

## <a name="listing-existing-cdn-endpoints"></a>Aanbieding bestaande CDN-eindpunten
`Get-AzureRmCdnEndpoint` een afzonderlijk eindpunt of de eindpunten in een profiel kunnen worden opgehaald.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Het maken van het CDN-profielen en -eindpunten
`New-AzureRmCdnProfile` en `New-AzureRmCdnEndpoint` worden gebruikt voor het maken van CDN-profielen en -eindpunten. De volgende SKU's worden ondersteund:
- Standard_Verizon
- Premium_Verizon
- Custom_Verizon
- Standard_Akamai
- Standard_ChinaCdn

> [!NOTE]
> De SKU Standard_Microsoft wordt niet ondersteund wanneer het is in preview.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Beschikbaarheid van de eindpunt-naam controleren
`Get-AzureRmCdnEndpointNameAvailability` retourneert een object waarmee wordt aangegeven of de naam van een eindpunt beschikbaar is.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Een aangepast domein toevoegen
`New-AzureRmCdnCustomDomain` Hiermee voegt u een aangepaste domeinnaam toe aan een bestaand eindpunt.

> [!IMPORTANT]
> U moet instellen de CNAME met uw DNS-provider zoals beschreven in [aangepast domein toewijzen aan Content Delivery Network (CDN) eindpunt](cdn-map-content-to-custom-domain.md).  U kunt de toewijzing voor het wijzigen van uw eindpunt met testen `Test-AzureRmCdnCustomDomain`.
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Wijzigen van een eindpunt
`Set-AzureRmCdnEndpoint` Hiermee wijzigt u een bestaand eindpunt.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Bezig met het verwijderen/Pre-loading CDN activa
`Unpublish-AzureRmCdnEndpointContent` worden in de cache opgeslagen activa, terwijl `Publish-AzureRmCdnEndpointContent` vooraf geladen assets op ondersteunde eindpunten.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Starten/stoppen CDN-eindpunten
`Start-AzureRmCdnEndpoint` en `Stop-AzureRmCdnEndpoint` kan worden gebruikt om te starten en stoppen van afzonderlijke eindpunten of groepen van eindpunten.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN-resources verwijderen
`Remove-AzureRmCdnProfile` en `Remove-AzureRmCdnEndpoint` kan worden gebruikt om profielen en -eindpunten te verwijderen.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe u Azure CDN kunt automatiseren met [.NET](cdn-app-dev-net.md) of [Node.js](cdn-app-dev-node.md).

Zie voor meer informatie over CDN-functies, [overzicht van CDN](cdn-overview.md).

