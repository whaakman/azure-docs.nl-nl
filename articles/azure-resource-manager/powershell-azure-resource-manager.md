---
title: Beheren van Azure-oplossingen met PowerShell | Microsoft Docs
description: Azure PowerShell en Resource Manager gebruiken voor het beheren van uw resources.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 10/06/2017
ms.author: tomfitz
ms.openlocfilehash: ae5ccb83a0088cb7c9668f18620b74f9f3f1e9b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a>Resources beheren met Azure PowerShell en Resource Manager

In dit artikel leert u hoe u uw oplossingen met Azure PowerShell en Azure Resource Manager beheert. Als u niet bekend met Resource Manager, Zie [overzicht van Resource Manager](resource-group-overview.md). Dit artikel is gericht op beheertaken. U gaat het volgende doen:

1. Een resourcegroep maken
2. Een resource toevoegen aan de resourcegroep
3. Een label toevoegen aan de bron
4. Een query uitvoeren op basis van namen of labelwaarden resources
5. Toepassen en verwijderen van een vergrendeling op de bron
6. Verwijderen van een resourcegroep

In dit artikel wordt niet weergegeven voor het implementeren van een Resource Manager-sjabloon aan uw abonnement. Zie voor die informatie [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](resource-group-template-deploy.md).

## <a name="get-started-with-azure-powershell"></a>Aan de slag met Azure PowerShell

Als u Azure PowerShell nog niet hebt geïnstalleerd, raadpleegt u [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

Als u Azure PowerShell in het verleden hebt geïnstalleerd, maar hebben niet het onlangs bijgewerkt, kunt u de nieuwste versie installeert. U kunt de versie bijwerken met de dezelfde methode die u hebt gebruikt om deze te installeren. Als u het Webplatforminstallatieprogramma gebruikt, bijvoorbeeld opnieuw te starten en zoekt u een update.

Gebruik de volgende cmdlet te controleren van uw versie van de module voor Azure-Resources:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

In dit artikel is bijgewerkt voor versie 3.3.0. Als u een eerdere versie hebt, uw ervaring mogelijk niet overeen met de stappen in dit artikel. Zie voor documentatie over de cmdlets in deze versie [AzureRM.Resources Module](/powershell/module/azurerm.resources).

## <a name="log-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account
Voordat u aan uw oplossing werkt, moet u zich aanmelden bij uw account.

Voor het aanmelden bij uw Azure-account, gebruiken de **Login-AzureRmAccount** cmdlet.

```powershell
Login-AzureRmAccount
```

De cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-account. Na het aanmelden, worden de instellingen van uw account gedownload zodat ze beschikbaar zijn voor Azure PowerShell.

De cmdlet retourneert informatie over uw account en het abonnement moet worden gebruikt voor de taken.

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

Als u meer dan één abonnement hebt, kunt u overschakelen naar een ander abonnement. Eerst gaan we kijken alle abonnementen voor uw account.

```powershell
Get-AzureRmSubscription
```

Het resultaat van ingeschakelde en uitgeschakelde abonnementen.

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

Als u wilt overschakelen naar een ander abonnement, geef de naam van het abonnement met de **Set-AzureRmContext** cmdlet.

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u implementeert geen bronnen aan uw abonnement, moet u een resourcegroep met de bronnen.

Voor het maken van een resourcegroep gebruikt de **New-AzureRmResourceGroup** cmdlet. Gebruikt de opdracht de **naam** parameter om een naam voor de resourcegroep en de **locatie** parameter om de locatie.

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

De uitvoer is in de volgende indeling:

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

Als u de resourcegroep later nodig, gebruikt u de volgende cmdlet:

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

Geef een naam niet voor alle resourcegroepen in uw abonnement:

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-to-a-resource-group"></a>Resources toevoegen aan een resourcegroep

Als u wilt een resource toevoegen aan de resourcegroep, kunt u de **nieuw AzureRmResource** cmdlet of een cmdlet die specifiek is voor het type resource dat u maakt (zoals **nieuw AzureRmStorageAccount**). Wellicht eenvoudiger te gebruiken van een cmdlet die specifiek is voor een resourcetype omdat deze parameters voor de eigenschappen die nodig zijn voor de nieuwe resource bevat. Gebruik **nieuw AzureRmResource**, moet u de eigenschappen in te stellen zonder te worden gevraagd voor hen kennen.

Echter, toevoegen van een resource via cmdlets kan veroorzaken toekomstige verwarring omdat de nieuwe resource niet in een Resource Manager-sjabloon bestaat. Microsoft raadt u aan het definiëren van de infrastructuur voor uw Azure-oplossing in Resource Manager-sjabloon. Sjablonen kunnen u op betrouwbare wijze en herhaaldelijk implementeren van uw oplossing. Voor dit artikel hebt u een opslagaccount maken met een PowerShell-cmdlet, maar later het genereren van een sjabloon uit de resourcegroep.

De volgende cmdlet maakt een opslagaccount. In plaats van de naam die wordt weergegeven in het voorbeeld, Geef een unieke naam voor het opslagaccount. De naam moet tussen 3 en 24 tekens lang zijn en alleen cijfers en kleine letters gebruiken. Als u de naam die wordt weergegeven in het voorbeeld gebruikt, ontvangt u een fout opgetreden omdat deze naam al gebruikt wordt.

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

Als u deze bron later nodig, gebruikt u de volgende cmdlet:

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a>Een label toevoegen

Labels kunnen u om uw resources op basis van andere eigenschappen te organiseren. Bijvoorbeeld wellicht verschillende resources in verschillende resourcegroepen die deel uitmaken van dezelfde afdeling. U kunt een label voor afdeling en de waarde toepassen op deze resources om deze te markeren als onderdeel van dezelfde categorie. Of u kunt markeren of een resource wordt gebruikt in een productie- of testomgeving. In dit artikel leert u tags toepassen op slechts één resource, maar in uw omgeving is het waarschijnlijk wel zinvol tags toepassen op al uw resources.

De volgende cmdlet geldt twee tags voor uw opslagaccount:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

Labels zijn als een enkel object bijgewerkt. Als u wilt een code toevoegt aan een resource die al tags bevat, moet u eerst de bestaande labels ophalen. Nieuw label toevoegen aan het object dat de bestaande labels bevat en alle tags toepassen op de resource.

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a>Zoeken naar resources

Gebruik de **zoeken AzureRmResource** cmdlet voor het ophalen van bronnen voor verschillende zoekcriteria.

* Als u een resource met de naam, bieden de **ResourceNameContains** parameter:

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* Als u alle resources in een resourcegroep, bieden de **ResourceGroupNameContains** parameter:

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* Als u de resources met een naam en waarde, bieden de **TagName** en **TagValue** parameters:

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* Alle resources met een bepaald resourcetype, geeft u de **ResourceType** parameter:

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="get-resource-id"></a>Resource-ID ophalen

Veel opdrachten nemen een resource-ID als een parameter. Als u de ID voor een resource die en opslaan in een variabele, gebruikt u:

```powershell
$webappID = (Get-AzureRmResource -ResourceGroupName exampleGroup -ResourceName exampleSite).ResourceId
```

## <a name="lock-a-resource"></a>Vergrendelen van een resource

Als u controleren of een essentiële resource niet per ongeluk is verwijderd of gewijzigd wilt, moet u een vergrendeling toegepast op de resource. U kunt opgeven dat ofwel een **CanNotDelete** of **ReadOnly**.

Als u wilt maken of verwijderen van management vergrendelingen, u moet toegang hebben tot `Microsoft.Authorization/*` of `Microsoft.Authorization/locks/*` acties. Van de ingebouwde rollen worden alleen de eigenaar en de beheerder voor gebruikerstoegang die acties verleend.

Als u wilt toepassen op een vergrendeling, gebruikt u de volgende cmdlet:

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

De vergrendelde resource in het vorige voorbeeld kan niet worden verwijderd, totdat de vergrendeling wordt verwijderd. Gebruik voor het verwijderen van een vergrendeling:

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Zie voor meer informatie over de instelling vergrendelingen [resources met Azure Resource Manager vergrendelen](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Verwijder resources of resourcegroep
U kunt een resource of resourcegroep verwijderen. Wanneer u een resourcegroep verwijdert, verwijdert u ook alle resources in die resourcegroep.

* Als u wilt een bron verwijdert uit de resourcegroep, gebruiken de **verwijderen AzureRmResource** cmdlet. Deze cmdlet de resource wordt verwijderd, maar de resourcegroep niet verwijdert.

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* U een resourcegroep en de bijhorende resources verwijderen met de **Remove-AzureRmResourceGroup** cmdlet.

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

Voor beide cmdlets, wordt u gevraagd te bevestigen dat u wilt verwijderen van de resource of resourcegroep. Als de bewerking is Hiermee verwijdert u de resource of resourcegroep, retourneert **True**.

## <a name="run-resource-manager-scripts-with-azure-automation"></a>Resource Manager-scripts uitvoeren met Azure Automation

In dit artikel laat zien hoe basisbewerkingen op uw resources met Azure PowerShell uit te voeren. Voor meer geavanceerde scenario's voor beheer wilt u waarschijnlijk een script maken en hergebruiken script naar behoefte of volgens een schema. [Azure Automation](../automation/automation-intro.md) biedt een manier om u te vaak automatiseren gebruikt scripts waarmee uw Azure-oplossingen kunt beheren.

De volgende onderwerpen beschreven hoe u met Azure Automation, Resource Manager en PowerShell effectief beheertaken uitvoeren:

- Zie voor meer informatie over het maken van een runbook [Mijn eerste PowerShell-runbook](../automation/automation-first-runbook-textual-powershell.md).
- Zie voor meer informatie over het werken met galerieën scripts [galerieën Runbook en de module voor Azure Automation](../automation/automation-runbook-gallery.md).
- Zie voor runbooks die starten en stoppen van virtuele machines, [Azure Automation-scenario: labels met behulp van JSON-indeling voor het maken van een planning voor de virtuele machine van Azure opstarten en afsluiten](../automation/automation-scenario-start-stop-vm-wjson-tags.md).
- Zie voor runbooks die starten en stoppen van virtuele machines rustige uren, [starten/stoppen virtuele machines tijdens rustige uren oplossing in Automation](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken van Resource Manager-sjablonen, [Azure Resource Manager-sjablonen ontwerpen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van sjablonen, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).
* U kunt bestaande resources verplaatsen naar een nieuwe resourcegroep. Zie voor voorbeelden [Resources verplaatsen naar de nieuwe resourcegroep of abonnement](resource-group-move-resources.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

