---
title: Een gedeelde zelf-hostende integratieruntime maken in Azure Data Factory met PowerShell | Microsoft Docs
description: Informatie over het maken van een gedeelde zelf-hostende integratieruntime in Azure Data Factory, waarmee meerdere data factory's toegang tot de integratieruntime.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: abnarain
ms.openlocfilehash: d7f3fbcb3235c8c620876e68a62f3e491770779d
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252136"
---
# <a name="create-a-shared-self-hosted-integration-runtime-in-azure-data-factory-with-powershell"></a>Een gedeelde zelf-hostende integratieruntime maken in Azure Data Factory met PowerShell

Deze stapsgewijze handleiding laat zien hoe u een gedeelde zelf-hostende integratieruntime (IR) in Azure Data Factory maken met behulp van Azure PowerShell. Vervolgens gebruikt u de gedeelde zelf-hostende integratieruntime in een andere data factory. In deze zelfstudie voert u de volgende stappen uit: 

1. Een data factory maken. 
1. Een zelf-hostende Integration Runtime maken.
1. De zelf-hostende integratieruntime delen met andere data factory's.
1. Een gekoppelde integratieruntime maken.
1. Intrekken van het delen.

## <a name="prerequisites"></a>Vereisten 

- **Azure-abonnement**. Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint. 

- **Azure PowerShell**. Volg de instructies in [Azure PowerShell installeren op Windows](/powershell/azure/install-azurerm-ps). U kunt PowerShell gebruiken om uit te voeren een script voor het maken van een zelf-hostende integratieruntime die kan worden gedeeld met andere data factory's. 

> [!NOTE]
> Voor een lijst van Azure-regio's waarin Data Factory momenteel beschikbaar is is, selecteert u de regio's waarin u geïnteresseerd bent in de volgende pagina: [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory).

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Start de Windows PowerShell ISE.

1. Variabelen maken.

    Kopieer en plak het volgende script en vervang de variabelen voor (SubscriptionName, ResourceGroupName, enzovoort) met de werkelijke waarden. 

    ```powershell
    # If input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
    $SubscriptionName = "[Azure subscription name]" 
    $ResourceGroupName = "[Azure resource group name]" 
    $DataFactoryLocation = "EastUS" 

    # Shared Self-hosted integration runtime information. This is a Data Factory compute resource for running any activities 
    # Data factory name. Must be globally unique 
    $SharedDataFactoryName = "[Shared Data factory name]" 
    $SharedIntegrationRuntimeName = "[Shared Integration Runtime Name]" 
    $SharedIntegrationRuntimeDescription = "[Description for Shared Integration Runtime]"

    # Linked integration runtime information. This is a Data Factory compute resource for running any activities
    # Data factory name. Must be globally unique
    $LinkedDataFactoryName = "[Linked Data factory name]"
    $LinkedIntegrationRuntimeName = "[Linked Integration Runtime Name]"
    $LinkedIntegrationRuntimeDescription = "[Description for Linked Integration Runtime]"
    ```

1. Meld u aan en selecteer een abonnement.

    Voeg de volgende code aan het script om te melden en selecteer uw Azure-abonnement:

    ```powershell
    Connect-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

1. Maak een resourcegroep en een Data Factory.

    *(Deze stap is optioneel. Als u al een data factory, sla deze stap.)* 

    Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` op de locatie Europa West. 

    ```powershell
    New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
    ```

    Voer de volgende opdracht uit om een data factory te maken: 

    ```powershell
    Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                             -Location $DataFactoryLocation `
                             -Name $SharedDataFactoryName
    ```

## <a name="create-a-self-hosted-integration-runtime"></a>Een zelf-hostende Integration Runtime maken

*(Deze stap is optioneel. Als u al de zelf-hostende integratieruntime die u wilt delen met andere data factory's hebt, sla deze stap.)*

Voer de volgende opdracht om een zelf-hostende integratieruntime maken:

```powershell
$SharedIR = Set-AzureRmDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Type SelfHosted `
    -Description $SharedIntegrationRuntimeDescription
```

### <a name="get-the-integration-runtime-authentication-key-and-register-a-node"></a>De verificatiesleutel voor integration runtime halen en te registreren van een knooppunt

Voer de volgende opdracht uit om op te halen van de verificatiesleutel voor de zelf-hostende integratieruntime:

```powershell
Get-AzureRmDataFactoryV2IntegrationRuntimeKey `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName
```

Het antwoord bevat de verificatiesleutel voor deze zelf-hostende integratieruntime. U gebruikt deze sleutel wanneer u het knooppunt voor integration runtime registreren.

### <a name="install-and-register-the-self-hosted-integration-runtime"></a>Installeren en zelf-hostende integratieruntime registreren

1. Download het installatieprogramma van de zelf-hostende integration runtime van [Azure Data Factory Integration Runtime](https://aka.ms/dmg).

2. Voer het installatieprogramma voor het installeren van de zelf-hostende integration op een lokale computer.

3. De nieuwe zelf-hostende integration registreren met de verificatiesleutel die u in de vorige stap hebt opgehaald.

## <a name="share-the-self-hosted-integration-runtime-with-another-data-factory"></a>De zelf-hostende integratieruntime delen met een andere data factory

### <a name="create-another-data-factory"></a>Maak een andere data factory

*(Deze stap is optioneel. Als u al de data factory die u wilt delen hebt, sla deze stap.)*

```powershell
$factory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
    -Location $DataFactoryLocation `
    -Name $LinkedDataFactoryName
```
### <a name="grant-permission"></a>Toestemming geven

Machtigingen verlenen aan de Gegevensfactory die toegang nodig heeft tot de zelf-hostende integratieruntime u hebt gemaakt en geregistreerd.

> [!IMPORTANT]
> Sla deze stap niet!

```powershell
New-AzureRMRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId ` #MSI of the Data Factory with which it needs to be shared
    -RoleDefinitionId 'b24988ac-6180-42a0-ab88-20f7382dd24c' ` #This is the Contributor role
    -Scope $SharedIR.Id
```

## <a name="create-a-linked-self-hosted-integration-runtime"></a>Een gekoppelde zelf-hostende integratieruntime maken

Voer de volgende opdracht om een gekoppelde zelf-hostende integratieruntime maken:

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $LinkedDataFactoryName `
    -Name $LinkedIntegrationRuntimeName `
    -Type SelfHosted `
    -SharedIntegrationRuntimeResourceId $SharedIR.Id `
    -Description $LinkedIntegrationRuntimeDescription
```

U kunt nu deze gekoppelde integratieruntime gebruiken in een gekoppelde service. De gekoppelde integratieruntime maakt gebruik van de gedeelde integratieruntime om uit te voeren activiteiten.

## <a name="revoke-integration-runtime-sharing-from-a-data-factory"></a>Intrekken van integratieruntime delen van een data factory

Om in te trekken van de toegang van een data factory toegang krijgen tot de gedeelde integratieruntime, kunt u de volgende opdracht uitvoeren:

```powershell
Remove-AzureRMRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId `
    -RoleDefinitionId 'b24988ac-6180-42a0-ab88-20f7382dd24c' `
    -Scope $SharedIR.Id
```

Als u wilt verwijderen van de bestaande gekoppelde integratieruntime, kunt u de volgende opdracht uitvoeren op basis van de gedeelde integratieruntime:

```powershell
Remove-AzureRmDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Links `
    -LinkedDataFactoryName $LinkedDataFactoryName
```

## <a name="next-steps"></a>Volgende stappen

- Basisbegrippen voor integration runtime in [integratieruntime in Azure Data Factory](concepts-integration-runtime.md).

- Informatie over het maken van een zelf-hostende integratieruntime in Azure portal in [maken en configureren van een zelf-hostende integratieruntime](create-self-hosted-integration-runtime.md).
