---
title: Toewijzingen met PowerShell beheren
description: Informatie over het beheren van blueprint-toewijzingen met de officiële Azure blauwdrukken PowerShell-module, Az.Blueprint.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 47026975a5142760f90a1a5712b532519f969fd7
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/15/2019
ms.locfileid: "57995665"
---
# <a name="how-to-manage-assignments-with-powershell"></a>Toewijzingen met PowerShell beheren

Een blauwdruktoewijzing kan worden beheerd met behulp van de **Az.Blueprint** Azure PowerShell-module. De module biedt ondersteuning voor ophalen, maken, bijwerken en toewijzingen worden verwijderd. Details op bestaande blauwdrukdefinities kan ook door de module worden opgehaald. In dit artikel bevat informatie over het installeren van de module en gaan gebruiken.

## <a name="add-the-azblueprint-module"></a>De module Az.Blueprint toevoegen

De module moet worden toegevoegd zodat Azure PowerShell voor het beheren van blueprint-toewijzingen. Deze module kan worden gebruikt met lokaal geïnstalleerde PowerShell, met [Azure Cloud Shell](https://shell.azure.com) of met de [Azure PowerShell Docker-installatiekopie](https://hub.docker.com/r/azuresdk/azure-powershell/).

### <a name="base-requirements"></a>Basisvereisten

De module voor Azure blauwdrukken vereist de volgende software:

- Azure PowerShell 1.5.0 of hoger. Als deze nog niet is geïnstalleerd, volgt u [deze instructies](/powershell/azure/install-az-ps) op.
- PowerShellGet 2.0.1 of hoger. Als deze nog niet is geïnstalleerd of bijgewerkt, volgt u [deze instructies](/powershell/gallery/installing-psget) op.

### <a name="install-the-module"></a>Installeer de module

De blauwdrukken-module voor PowerShell is **Az.Blueprint**.

1. Voer vanuit een PowerShell-prompt met **beheerdersrechten** de volgende opdracht uit:

   ```azurepowershell-interactive
   # Install the Blueprints module from PowerShell Gallery
   Install-Module -Name Az.Blueprint
   ```

   > [!NOTE]
   > Als **Az.Accounts** is al geïnstalleerd, kan het nodig zijn om te gebruiken `-AllowClobber` om af te dwingen de installatie.

1. Valideren dat de module is geïmporteerd en de juiste versie (0.1.0):

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.Blueprint module
   Get-Command -Module 'Az.Blueprint' -CommandType 'Cmdlet'
   ```

## <a name="get-blueprint-definitions"></a>Blauwdrukdefinities ophalen

De eerste stap bij het werken met een toewijzing is vaak een verwijzing naar de blauwdrukdefinitie van een ophalen.
De `Get-AzBlueprint` -cmdlet krijgt u een of meer blauwdrukdefinities. De cmdlet blauwdrukdefinities kunt ophalen uit een beheergroep met `-ManagementGroupId {mgId}` of een abonnement met `-SubscriptionId {subId}`. De **naam** parameter een blauwdrukdefinitie opgehaald, maar moet worden gebruikt met **ManagementGroupId** of **SubscriptionId**. **Versie** kan worden gebruikt met **naam** moet explicieter aangeven van welke blauwdrukdefinitie wordt geretourneerd. In plaats van **versie**, de switch `-LatestPublished` grijpers het meest recent gepubliceerde versie.

Het volgende voorbeeld wordt `Get-AzBlueprint` om op te halen van alle versies van de blauwdrukdefinitie van een die met de naam ' 101-blauwdrukken-definition-abonnement is van een specifiek abonnement weergegeven als `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get all versions of the blueprint definition in the specified subscription
$blueprints = Get-AzBlueprint -SubscriptionId '{subId}' -Name '101-blueprints-definition-subscription'

# Display the blueprint definition object
$blueprints
```

De voorbeelduitvoer voor de blauwdrukdefinitie van een met meerdere versies er zo uit:

```output
Name                 : 101-blueprints-definition-subscription
Id                   : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprints/101
                       -blueprints-definition-subscription
DefinitionLocationId : {subId}
Versions             : {1.0, 1.1}
TimeCreated          : 2019-02-25
TargetScope          : Subscription
Parameters           : {storageAccount_storageAccountType, storageAccount_location,
                       allowedlocations_listOfAllowedLocations, [Usergrouporapplicationname]:Reader_RoleAssignmentName}
ResourceGroups       : ResourceGroup
```

De [blauwdruk parameters](../concepts/parameters.md#blueprint-parameters) op de blauwdruk definitie kan worden uitgebreid om meer informatie te verstrekken.

```azurepowershell-interactive
$blueprints.Parameters
```

```output
Key                                                    Value
---                                                    -----
storageAccount_storageAccountType                      Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
storageAccount_location                                Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
allowedlocations_listOfAllowedLocations                Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
[Usergrouporapplicationname]:Reader_RoleAssignmentName Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
```

## <a name="get-blueprint-assignments"></a>Blueprint-toewijzingen ophalen

Als de blauwdruktoewijzing al bestaat, krijgt u een verwijzing naar het met de `Get-AzBlueprintAssignment` cmdlet. De cmdlet voert **SubscriptionId** en **naam** als optionele parameters. Als **SubscriptionId** niet is opgegeven, de huidige context van het abonnement wordt gebruikt.

Het volgende voorbeeld wordt `Get-AzBlueprintAssignment` om op te halen van een enkele blauwdruktoewijzing "Toewijzing-lock-resource-groups" met de naam van een specifiek abonnement weergegeven als `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -SubscriptionId '{subId}' -Name 'Assignment-lock-resource-groups'

# Display the blueprint assignment object
$blueprintAssignment
```

In de voorbeelduitvoer voor een blauwdruktoewijzing ziet er zo uit:

```output
Name              : Assignment-lock-resource-groups
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssignme
                    nts/Assignment-lock-resource-groups
Scope             : /subscriptions/{subId}
LastModified      : 2019-02-19
LockMode          : AllResourcesReadOnly
ProvisioningState : Succeeded
Parameters        :
ResourceGroups    : ResourceGroup
```

## <a name="create-blueprint-assignments"></a>Blueprint-toewijzingen maken

Als de blauwdruktoewijzing nog niet bestaat, kunt u maken met de `New-AzBlueprintAssignment` cmdlet. Deze cmdlet maakt gebruik van de volgende parameters:

- **Naam** (vereist)
  - Hiermee geeft u de naam van de blauwdruktoewijzing
  - Moet uniek zijn en niet al bestaan in **abonnements-id**
- **Blauwdruk** (vereist)
  - Hiermee geeft u de blauwdrukdefinitie van de toewijzen
  - Gebruik `Get-AzBlueprint` ophalen van de referentieobject
- **Locatie** (vereist)
  - Hiermee geeft u de regio voor het systeem toegewezen beheerde identiteits- en abonnement implementatieobject moet worden gemaakt in
- **Abonnement** (optioneel)
  - Hiermee geeft u het abonnement dat de toewijzing wordt geïmplementeerd op
  - Als niet is opgegeven, wordt standaard de huidige context van het abonnement
- **Vergrendeling** (optioneel)
  - Definieert de [blauwdruk resource vergrendelen](../concepts/resource-locking.md) moet worden gebruikt voor geïmplementeerde resources
  - Ondersteunde opties: _None_, _AllResourcesReadOnly_, _AllResourcesDoNotDelete_
  - Als niet is opgegeven, standaard _None_
- **SystemAssignedIdentity** (optioneel)
  - Selecteer om te maken van een systeem toegewezen beheerde identiteit voor de toewijzing en de resources wilt implementeren
  - Standaard voor de parameter 'identiteit' is ingesteld
  - Kan niet worden gebruikt met **UserAssignedIdentity**
- **UserAssignedIdentity** (optioneel)
  - Hiermee geeft u de gebruiker toegewezen beheerde identiteit moet worden gebruikt voor de toewijzing en de resources wilt implementeren
  - Onderdeel van de parameter 'id' instellen
  - Kan niet worden gebruikt met **SystemAssignedIdentity**
- **Parameter** (optioneel)
  - Een [hash-tabel](/powershell/module/microsoft.powershell.core/about/about_hash_tables) van sleutel/waarde-paren voor instelling [dynamische parameters](../concepts/parameters.md#dynamic-parameters) voor de blauwdruktoewijzing
  - Een dynamische parameter is standaard de **defaultValue** in het definitie
  - Als een parameter niet wordt opgegeven en geen heeft **defaultValue**, de parameter niet optioneel

    > [!NOTE]
    > **Parameter** biedt geen ondersteuning voor secureStrings.

- **ResourceGroupParameter** (optioneel)
  - Een [hash-tabel](/powershell/module/microsoft.powershell.core/about/about_hash_tables) van resource-group-artefacten
  - Elke resource group artefact tijdelijke aanduiding heeft een sleutel/waarde-paren voor het instellen van dynamisch **naam** en/of **locatie** op deze resource group-artefact
  - Als een resource-group-parameter is niet beschikbaar en geen heeft **defaultValue**, de resource-group-parameter niet optioneel

Het volgende voorbeeld wordt een nieuwe toewijzing van versie '1.1' van de blauwdrukdefinitie van de 'Mijn-blauwdruk' opgehaald met `Get-AzBlueprint`, beheerde identiteit en de toewijzing object wordt de locatie ingesteld op 'westus2', wordt Hiermee vergrendelt u de resources met  _AllResourcesReadOnly_, en stelt u de hashtabellen voor zowel **Parameter** en **ResourceGroupParameter** op specifieke abonnement weergegeven als `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Create the hash table for Parameters
$bpParameters = @{storageAccount_storageAccountType='Standard_GRS'}

# Create the hash table for ResourceGroupParameters
# ResourceGroup is the resource group artifact placeholder name
$bpRGParameters = @{ResourceGroup=@{name='storage_rg';location='westus2'}}

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadyOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
```

In de voorbeelduitvoer voor het maken van een blauwdruktoewijzing van de ziet er zo uit:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesReadyOnly
ProvisioningState : Creating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

## <a name="update-blueprint-assignments"></a>Blueprint-toewijzingen bijwerken

Soms is het nodig zijn om bij te werken van de blauwdruktoewijzing van een die al is gemaakt. De `Set-AzBlueprintAssignment` cmdlet worden verwerkt met deze actie. De cmdlet voert de meeste van de parameters die de `New-AzBlueprintAssignment` cmdlet biedt, zodat alles dat is ingesteld voor de toewijzing moet worden bijgewerkt. De uitzonderingen voor dit zijn de _naam_, _blauwdruk_, en _SubscriptionId_. Alleen de opgegeven waarden worden bijgewerkt.

Zie voor meer informatie over wat er gebeurt tijdens het bijwerken van een blauwdruktoewijzing van de, [regels voor het bijwerken van toewijzingen](./update-existing-assignments.md#rules-for-updating-assignments).

- **Naam** (vereist)
  - Hiermee geeft u de naam van de blauwdruktoewijzing om bij te werken
  - Aan de hand van de toewijzing om bij te werken, niet op de toewijzing wijzigen
- **Blauwdruk** (vereist)
  - Hiermee geeft u de blauwdrukdefinitie van de blauwdruktoewijzing
  - Gebruik `Get-AzBlueprint` ophalen van de referentieobject
  - Aan de hand van de toewijzing om bij te werken, niet op de toewijzing wijzigen
- **Locatie** (optioneel)
  - Hiermee geeft u de regio voor het systeem toegewezen beheerde identiteits- en abonnement implementatieobject moet worden gemaakt in
- **Abonnement** (optioneel)
  - Hiermee geeft u het abonnement dat de toewijzing wordt geïmplementeerd op
  - Als niet is opgegeven, wordt standaard de huidige context van het abonnement
  - Aan de hand van de toewijzing om bij te werken, niet op de toewijzing wijzigen
- **Vergrendeling** (optioneel)
  - Definieert de [blauwdruk resource vergrendelen](../concepts/resource-locking.md) moet worden gebruikt voor geïmplementeerde resources
  - Ondersteunde opties: _None_, _AllResourcesReadOnly_, _AllResourcesDoNotDelete_
- **SystemAssignedIdentity** (optioneel)
  - Selecteer om te maken van een systeem toegewezen beheerde identiteit voor de toewijzing en de resources wilt implementeren
  - Standaard voor de parameter 'identiteit' is ingesteld
  - Kan niet worden gebruikt met **UserAssignedIdentity**
- **UserAssignedIdentity** (optioneel)
  - Hiermee geeft u de gebruiker toegewezen beheerde identiteit moet worden gebruikt voor de toewijzing en de resources wilt implementeren
  - Onderdeel van de parameter 'id' instellen
  - Kan niet worden gebruikt met **SystemAssignedIdentity**
- **Parameter** (optioneel)
  - Een [hash-tabel](/powershell/module/microsoft.powershell.core/about/about_hash_tables) van sleutel/waarde-paren voor instelling [dynamische parameters](../concepts/parameters.md#dynamic-parameters) voor de blauwdruktoewijzing
  - Een dynamische parameter is standaard de **defaultValue** in het definitie
  - Als een parameter niet wordt opgegeven en geen heeft **defaultValue**, de parameter niet optioneel

    > [!NOTE]
    > **Parameter** biedt geen ondersteuning voor secureStrings.

- **ResourceGroupParameter** (optioneel)
  - Een [hash-tabel](/powershell/module/microsoft.powershell.core/about/about_hash_tables) van resource-group-artefacten
  - Elke resource group artefact tijdelijke aanduiding heeft een sleutel/waarde-paren voor het instellen van dynamisch **naam** en/of **locatie** op deze resource group-artefact
  - Als een resource-group-parameter is niet beschikbaar en geen heeft **defaultValue**, de resource-group-parameter niet optioneel

Het volgende voorbeeld werkt u de toewijzing van versie '1.1' van de blauwdrukdefinitie van de 'Mijn-blauwdruk' opgehaald met `Get-AzBlueprint` door het wijzigen van de lock-modus:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
```

In de voorbeelduitvoer voor het maken van een blauwdruktoewijzing van de ziet er zo uit:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesDoNotDelete
ProvisioningState : Updating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

## <a name="remove-blueprint-assignments"></a>Blueprint-toewijzingen verwijderen

Wanneer het tijd voor de blauwdruktoewijzing van een moet worden verwijderd, is de `Remove-AzBlueprintAssignment` cmdlet worden verwerkt met deze actie. De cmdlet voert een **naam** of **InputObject** om op te geven welke blauwdruk toewijzing te verwijderen. **SubscriptionId** is _vereist_ en in alle gevallen moet worden opgegeven.

Het volgende voorbeeld haalt u een bestaande blauwdruktoewijzing van met `Get-AzBlueprintAssignment` en vervolgens verwijderd uit het specifieke abonnement weergegeven als `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -Name 'Assignment-lock-resource-groups'

# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $blueprintAssignment -SubscriptionId '{subId}'
```

## <a name="end-to-end-code-example"></a>Codevoorbeeld van de end-to-end

Alle stappen bijeenbrengt, het volgende voorbeeld opgehaald van de blauwdrukdefinitie, vervolgens wordt gemaakt, bijgewerkt en een blauwdruktoewijzing van de verwijdert in het specifieke abonnement weergegeven als `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

#region GetBlueprint
# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'
#endregion

#region CreateAssignment
# Create the hash table for Parameters
$bpParameters = @{storageAccount_storageAccountType='Standard_GRS'}

# Create the hash table for ResourceGroupParameters
# ResourceGroup is the resource group artifact placeholder name
$bpRGParameters = @{ResourceGroup=@{name='storage_rg';location='westus2'}}

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadyOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
#endregion CreateAssignment

# Wait for the blueprint assignment to finish deployment prior to the next steps

#region UpdateAssignment
# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
#endregion UpdateAssignment

# Wait for the blueprint assignment to finish deployment prior to the next steps

#region RemoveAssignment
# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $bpAssignment -SubscriptionId '{subId}'
#endregion
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [blauwdruk levenscyclus](../concepts/lifecycle.md).
- Meer informatie over het gebruik van [statische en dynamische parameters](../concepts/parameters.md).
- Meer informatie over het aanpassen van de [blauwdruk volgorde](../concepts/sequencing-order.md).
- Ontdek hoe u het gebruik van [blauwdruk resource vergrendelen](../concepts/resource-locking.md).
- Problemen oplossen bij het toewijzen van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md).