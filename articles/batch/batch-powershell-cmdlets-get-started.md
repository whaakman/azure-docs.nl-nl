---
title: Aan de slag met PowerShell - Azure Batch | Microsoft Docs
description: Een korte inleiding in de Azure PowerShell-cmdlets die u kunt gebruiken voor het beheren van Batch-resources.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 10/05/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: bd8e5425a09f0faeaa573cec58def88f352b9a1d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548065"
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a>Batch-resources beheren met PowerShell-cmdlets

Met de PowerShell-cmdlets voor Azure Batch kunt u veel van de taken die u uitvoert met de Batch-API's, de Azure-portal en de Azure-opdrachtregelinterface (CLI), uitvoeren en er scripts voor uitvoeren. Dit artikel is een korte inleiding in de cmdlets die u kunt gebruiken om uw Batch-accounts te beheren en te werken met uw Batch-resources, zoals pools en taken.

Zie [Naslaginformatie over Azure Batch-cmdlets](/powershell/module/azurerm.batch/#batch) voor een volledige lijst met Batch-cmdlets en gedetailleerde cmdlet-syntaxis.

Dit artikel is gebaseerd op cmdlets in Azure Batch-module 4.1.5. Het wordt aangeraden uw Azure PowerShell-modules regelmatig bij te werken om te profiteren van service-updates en -verbeteringen.

## <a name="prerequisites"></a>Vereisten

* [De Azure PowerShell-module installeren en configureren](/powershell/azure/overview). Zie de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.Batch/5.0.0-preview) voor het installeren van een specifieke Azure Batch-module, zoals een prerelease-module. 

* Voer de cmdlet **Connect-AzureRmAccount** uit om verbinding te maken met uw abonnement (de Azure Batch-cmdlets zijn meegeleverd in de Azure Resource Manager-module):

  ```PowerShell
  Connect-AzureRmAccount
  ```

* **Registreer bij de naamruimte van de Batch-provider**. U hoeft deze bewerking slechts **één keer per abonnement** uit te voeren.
  
  ```PowerShell
  Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`
  ```

## <a name="manage-batch-accounts-and-keys"></a>Batch-accounts en -sleutels beheren

### <a name="create-a-batch-account"></a>Batch-account maken

Met **New-AzureRmBatchAccount** wordt een Batch-account in een opgegeven resourcegroep gemaakt. Als u nog geen resourcegroep hebt, maakt u er een door de cmdlet [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) uit te voeren. Geef een van de Azure-gebieden op in de parameter **locatie**, bijvoorbeeld 'US - centraal'. Bijvoorbeeld:

```PowerShell
New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"
```

Maak vervolgens een Batch-account in de resourcegroep, waarbij u in <*accountnaam*> een naam voor het account opgeeft, en de locatie en naam van uw resourcegroep. Het kan even duren voordat het Batch-account is gemaakt. Bijvoorbeeld:

```PowerShell
New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>
```

> [!NOTE]
> De Batch-accountnaam moet uniek zijn voor de Azure-regio voor de resourcegroep, minimaal 3 en maximaal 24 tekens bevatten en alleen bestaan uit kleine letters en cijfers.
> 

### <a name="get-account-access-keys"></a>Toegangssleutels van account ophalen

Met **Get-AzureRmBatchAccountKeys** worden de toegangssleutels weergegeven die aan een Azure Batch-account zijn gekoppeld. Voer bijvoorbeeld de volgende opdracht uit om de primaire en secundaire sleutels op te halen van het account dat u hebt gemaakt.

 ```PowerShell
$Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

$Account.PrimaryAccountKey

$Account.SecondaryAccountKey
```

### <a name="generate-a-new-access-key"></a>Een nieuwe toegangssleutel genereren

Met **New-AzureRmBatchAccountKey** wordt een nieuwe primaire of secundaire accountsleutel voor een Azure Batch-account gegenereerd. Typ bijvoorbeeld de volgende opdracht voor het genereren van een nieuwe primaire sleutel voor het Batch-account:

```PowerShell
New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary
```

> [!NOTE]
> Als u een nieuwe secundaire sleutel wilt genereren, geeft u 'Secundair' op voor de parameter **KeyType**. U moet de primaire en secundaire sleutels afzonderlijk opnieuw genereren.
> 

### <a name="delete-a-batch-account"></a>Batch-account verwijderen

Met **Remove-AzureRmBatchAccount** wordt een Batch-account verwijderd. Bijvoorbeeld:

```PowerShell
Remove-AzureRmBatchAccount -AccountName <account_name>
```

Als hierom wordt gevraagd, bevestigt u dat u het account wilt verwijderen. Het kan even duren voordat het account is verwijderd.

## <a name="create-a-batchaccountcontext-object"></a>Een BatchAccountContext-object maken

Voor het beheren van Batch-resources kunt u verificatie op basis van gedeelde sleutels of Azure Active Directory-verificatie gebruiken. Voor verificatie met behulp van de Batch PowerShell-cmdlets maakt u eerst een BatchAccountContext-object om uw accountreferenties of identiteit in op te slaan. U geeft het BatchAccountContext-object door aan cmdlets waarvoor de parameter **BatchContext** wordt gebruikt. 

### <a name="shared-key-authentication"></a>Gedeelde sleutelverificatie

```PowerShell
$context = Get-AzureRmBatchAccountKeys -AccountName <account_name>
```

> [!NOTE]
> Standaard wordt de primaire sleutel van het account gebruikt voor verificatie, maar u kunt expliciet de te gebruiken sleutel selecteren door de eigenschap **KeyInUse** van het object BatchAccountContext te wijzigen: `$context.KeyInUse = "Secondary"`.
> 

### <a name="azure-active-directory-authentication"></a>Verificatie via Azure Active Directory

```PowerShell
$context = Get-AzureRmBatchAccount -AccountName <account_name>
```

## <a name="create-and-modify-batch-resources"></a>Batch-resources maken en wijzigen
Gebruik cmdlets zoals **New-AzureBatchPool**, **New-AzureBatchJob** en **New-AzureBatchTask** om resources onder een Batch-account te maken. Er zijn overeenkomende **Get-**- en **Set-**-cmdlets voor het bijwerken van de eigenschappen van bestaande resources en **Remove-**-cmdlets om resources onder een Batch-account te verwijderen.

Wanneer u veel van deze cmdlets gebruikt, moet u niet alleen een BatchContext-object doorgeven, maar ook objecten maken of doorgeven die gedetailleerde resource-instellingen bevatten, zoals weergegeven in het volgende voorbeeld. Raadpleeg de gedetailleerde Help-informatie voor elke cmdlet voor meer voorbeelden.

### <a name="create-a-batch-pool"></a>Batch-pool maken

Wanneer u een Batch-pool maakt of bijwerkt, selecteert u de cloudserviceconfiguratie of de virtuele-machineconfiguratie voor het besturingssysteem op de rekenknooppunten (zie [Overzicht van Batch-functies](batch-api-basics.md#pool)). Als u de cloudserviceconfiguratie opgeeft, worden uw rekenknooppunten gerepliceerd met één van de [Azure-gastbesturingssysteemversies](../cloud-services/cloud-services-guestos-update-matrix.md#releases). Als u de VM-configuratie opgeeft, kunt u één van de ondersteunde Linux- of Windows-VM-installatiekopieën opgeven die worden vermeld in de [Azure Virtual Machines Marketplace][vm_marketplace], of u geeft een aangepaste installatiekopie op die u hebt gemaakt.

Bij het uitvoeren van **New-AzureBatchPool** geeft u de instellingen van het besturingssysteem door in een PSCloudServiceConfiguration- of PSVirtualMachineConfiguration-object. Met het volgende codefragment wordt bijvoorbeeld een nieuwe Batch-groep gemaakt met rekenknooppunten met de grootte Standard_a1 in de configuratie van de virtuele machine, gerepliceerd met Ubuntu Server 16.04-LTS. Hier geeft de parameter **VirtualMachineConfiguration** de variabele *$configuration* op als het PSVirtualMachineConfiguration-object. Met de parameter **BatchContext** wordt een eerder gedefinieerde variabele *$context* opgegeven als het BatchAccountContext-object.

```PowerShell
$imageRef = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSImageReference" -ArgumentList @("UbuntuServer","Canonical","16.04.0-LTS")

$configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSVirtualMachineConfiguration" -ArgumentList @($imageRef, "batch.node.ubuntu 16.04")

New-AzureBatchPool -Id "mypool" -VirtualMachineSize "Standard_a1" -VirtualMachineConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context
```

Het doelaantal rekenknooppunten in de nieuwe pool wordt bepaald met een formule voor automatisch schalen. In dit geval is de formule gewoon **$TargetDedicated=4**, waarmee wordt aangegeven dat het aantal rekenknooppunten in de pool maximaal 4 is.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Query voor pools, jobs, taken en andere details

Gebruik cmdlets zoals **Get-AzureBatchPool**, **Get-AzureBatchJob** en **Get-AzureBatchTask** om een query uit te voeren voor entiteiten die zijn gemaakt onder een Batch-account.

### <a name="query-for-data"></a>Query voor gegevens

Gebruik bijvoorbeeld **Get-AzureBatchPools** om uw pools te vinden. Standaard wordt deze query voor alle pools onder uw account uitgevoerd, in de veronderstelling dat u het object BatchAccountContext al hebt opgeslagen in *$context*:

```PowerShell
Get-AzureBatchPool -BatchContext $context
```

### <a name="use-an-odata-filter"></a>Een OData-filter gebruiken

U kunt een OData-filter opgeven met behulp van de parameter **Filter** om alleen de objecten te zoeken waarin u bent geïnteresseerd. U kunt bijvoorbeeld alle pools zoeken met id's die beginnen met 'myPool':

```PowerShell
$filter = "startswith(id,'myPool')"

Get-AzureBatchPool -Filter $filter -BatchContext $context
```

Deze methode is niet zo flexibel als het gebruik van een 'Where-Object' in een lokale pijplijn. De query wordt rechtstreeks naar de Batch-service verzonden zodat al het filteren aan de serverzijde plaatsvindt, waardoor er internetbandbreedte wordt bespaard.

### <a name="use-the-id-parameter"></a>De parameter Id gebruiken

Een alternatief voor een OData-filter is het gebruik van de parameter **Id**. Als u een query uit wilt voeren voor een specifieke pool met id 'myPool':

```PowerShell
Get-AzureBatchPool -Id "myPool" -BatchContext $context
```

Met de parameter **Id** wordt alleen een zoekopdracht voor een volledige id ondersteund, geen jokertekens of filters van het OData-type.

### <a name="use-the-maxcount-parameter"></a>De parameter MaxCount gebruiken

Standaard worden met elke cmdlet maximaal 1000 objecten geretourneerd. Als deze limiet is bereikt, moet u uw filter verfijnen zodat er minder objecten worden opgehaald, of moet u expliciet een maximum instellen met de parameter **MaxCount**. Bijvoorbeeld:

```PowerShell
Get-AzureBatchTask -MaxCount 2500 -BatchContext $context
```

Als u de bovengrens wilt verwijderen, stelt u **MaxCount** in op 0 of minder.

### <a name="use-the-powershell-pipeline"></a>De PowerShell-pijplijn gebruiken

Met Batch-cmdlets kunt u gebruikmaken van de PowerShell-pijplijn om gegevens tussen cmdlets te verzenden. Dit heeft hetzelfde effect als het opgeven van een parameter, maar het vergemakkelijkt het werken met meerdere entiteiten.

Zo kunt u bijvoorbeeld alle taken onder uw account zoeken en weergeven:

```PowerShell
Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context
```

Start elk computerknooppunt in een groep opnieuw op:

```PowerShell
Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context
```

## <a name="application-package-management"></a>Beheer van toepassingspakketten

Toepassingspakketten bieden een vereenvoudigde manier om toepassingen te implementeren op de rekenknooppunten in groepen. Met de Batch PowerShell-cmdlets kunt u toepassingspakketten in uw Batch-account uploaden en beheren, en pakketversies implementeren op rekenknooppunten.

Een toepassing **maken**:

```PowerShell
New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"
```

Een toepassingspakket **toevoegen**:

```PowerShell
New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip
```

Stel de **standaardversie** voor de toepassing in:

```PowerShell
Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"
```

**Geeft een overzicht van** de pakketten van een toepassing

```PowerShell
$application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

$application.ApplicationPackages
```

Een toepassingspakket **verwijderen**

```PowerShell
Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"
```

Een toepassing **verwijderen**

```PowerShell
Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"
```

> [!NOTE]
> Voordat u de toepassing verwijdert, moet u alle versies van de toepassingspakketten van een toepassing verwijderen. U ontvangt een 'conflictfout' als u een toepassing probeert te verwijderen die nog toepassingspakketten heeft.
> 

### <a name="deploy-an-application-package"></a>Een toepassingspakket implementeren

U kunt een of meer toepassingspakketten voor implementatie opgeven wanneer u een groep maakt. Wanneer u een pakket opgeeft tijdens het maken van een groep, wordt dit pakket geïmplementeerd op elk knooppunt dat wordt gekoppeld aan de groep. Pakketten worden ook geïmplementeerd als een knooppunt opnieuw wordt gestart of teruggezet.

Geef de optie voor `-ApplicationPackageReference` op als u een groep maakt, zodat een toepassingspakket wordt geïmplementeerd naar de knooppunten van de groep wanneer deze lid worden van de groep. Maak eerst een **PSApplicationPackageReference**-object en configureer dit met de toepassings-id en pakketversie die u wilt implementeren op de rekenknooppunten van de groep:

```PowerShell
$appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

$appPackageReference.ApplicationId = "MyBatchApplication"

$appPackageReference.Version = "1.0"
```

Nu maakt u de adresgroep en geeft u het pakketverwijzingsobject op als het argument voor de optie `ApplicationPackageReferences`:

```PowerShell
New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference
```

Zie [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md) (Toepassingen implementeren naar rekenknooppunten met Batch-toepassingspakketten) voor meer informatie over toepassingspakketten.

> [!IMPORTANT]
> U moet [een Azure Storage-account koppelen](#linked-storage-account-autostorage) aan het Batch-account om toepassingspakketten te gebruiken.
> 
> 

### <a name="update-a-pools-application-packages"></a>De toepassingspakketten van een groep bijwerken

Als u de toepassingen wilt bijwerken die zijn toegewezen aan een bestaande groep, maakt u eerst een PSApplicationPackageReference-object met de gewenste eigenschappen (toepassings-id en pakketversie):

```PowerShell
$appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

$appPackageReference.ApplicationId = "MyBatchApplication"

$appPackageReference.Version = "2.0"

```

Vervolgens haalt u de adresgroep op uit Batch, wist u eventuele bestaande pakketten, voegt u de nieuwe pakketverwijzing toe en werkt u de Batch-service bij met de nieuwe adresgroepinstellingen:

```PowerShell
$pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

$pool.ApplicationPackageReferences.Clear()

$pool.ApplicationPackageReferences.Add($appPackageReference)

Set-AzureBatchPool -BatchContext $context -Pool $pool
```

Nu hebt u de eigenschappen van de adresgroep bijgewerkt in de Batch-service. Voor de werkelijke implementatie van het nieuwe toepassingspakket op rekenknooppunten in de adresgroep moet u deze knooppunten echter opnieuw starten of de installatiekopie ervan terugzetten. U kunt elk knooppunt in een adresgroep met de volgende opdracht opnieuw starten:

```PowerShell
Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context
```

> [!TIP]
> U kunt meerdere toepassingspakketten op de rekenknooppunten in de adresgroep implementeren. Als u een toepassingspakket wilt *toevoegen* in plaats van de huidige geïmplementeerde pakketten te vervangen, laat dan de `$pool.ApplicationPackageReferences.Clear()`-regel hierboven weg.
> 
> 

## <a name="next-steps"></a>Volgende stappen

* Zie [Naslaginformatie over Azure Batch-cmdlets](/powershell/module/azurerm.batch/#batch) voor gedetailleerde cmdlet-syntaxis en voorbeelden.
* Zie [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md) (Toepassingen implementeren naar rekenknooppunten met Batch-toepassingspakketten) voor meer informatie over toepassingen en toepassingspakketten in Batch.

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
