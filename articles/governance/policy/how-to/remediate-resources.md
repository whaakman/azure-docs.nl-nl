---
title: Niet-compatibele resources herstellen
description: Deze instructies begeleidt u bij het herstel van resources die niet aan beleidsregels in Azure Policy voldoen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 086ef9030451632ee4defa39a402e4d62c897f20
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342113"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Herstellen van niet-compatibele resources met Azure Policy

Resources die niet voldoen aan een **deployIfNotExists** beleid kan worden geplaatst in een compatibele status via **herstel**. Herstel wordt bereikt door het beleid om uit te voeren dat de **deployIfNotExists** effect van het toegewezen beleid op uw bestaande resources. Dit artikel laat de stappen die nodig zijn om te begrijpen en uitvoeren van herstel met het beleid.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="how-remediation-security-works"></a>Hoe herstel beveiliging werkt

Wanneer de sjabloon in beleid wordt uitgevoerd de **deployIfNotExists** beleidsdefinitie, dit gebeurt met behulp van een [beheerde identiteit](../../../active-directory/managed-identities-azure-resources/overview.md).
Beleid moet wordt gemaakt van een beheerde identiteit voor elke toewijzing echter over meer informatie over welke functies voor het verlenen van de beheerde identiteit. Als de beheerde identiteit rollen ontbreekt, wordt deze fout weergegeven tijdens de toewijzing van het beleid of een initiatief. Wanneer u de portal, beleid wordt automatisch de beheerde identiteit de vermelde rollen toewijzen als toewijzing is gestart.

![Beheerde identiteit - ontbrekende functie](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> Als een resource die is gewijzigd door **deployIfNotExists** valt buiten het bereik van de beleidstoewijzing of de sjabloon voor eigenschappen van de toegang tot bronnen buiten het bereik van de beleidstoewijzing, beheerde identiteit van de toewijzing moet [handmatig toegang verleend](#manually-configure-the-managed-identity) of mislukt de implementatie van het herstel.

## <a name="configure-policy-definition"></a>Beleidsdefinitie configureren

De eerste stap is het definiëren van de rollen die **deployIfNotExists** moet in de beleidsdefinitie voor een succesvolle implementatie van de inhoud van de sjabloon opgenomen. Onder de **details** eigenschap toevoegen een **roleDefinitionIds** eigenschap. Deze eigenschap is een matrix met tekenreeksen die overeenkomen met de rollen in uw omgeving. Zie voor een compleet voorbeeld de [deployIfNotExists voorbeeld](../concepts/effects.md#deployifnotexists-example).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

**roleDefinitionIds** maakt gebruik van de volledige resource-id en neemt de korte **roleName** van de rol. Als u de ID voor de rol 'Inzender in uw omgeving, gebruikt u de volgende code:

```azurecli-interactive
az role definition list --name 'Contributor'
```

```azurepowershell-interactive
Get-AzRoleDefinition -Name 'Contributor'
```

## <a name="manually-configure-the-managed-identity"></a>Handmatig configureren van de beheerde identiteit

Wanneer u een toewijzing met behulp van de portal maakt, beleid zowel genereert de beheerde identiteit en verleent het de rollen die zijn gedefinieerd in **roleDefinitionIds**. Stappen voor het maken van de beheerde identiteit en machtigingen toewijzen moeten handmatig worden gedaan in de volgende voorwaarden:

- Tijdens het gebruik van de SDK (zoals Azure PowerShell)
- Wanneer een resource buiten het bereik van de roltoewijzing is gewijzigd door de sjabloon
- Wanneer een resource buiten het bereik van de roltoewijzing wordt gelezen door de sjabloon

> [!NOTE]
> Azure PowerShell en .NET zijn de enige SDK's die momenteel ondersteuning voor deze mogelijkheid.

### <a name="create-managed-identity-with-powershell"></a>Beheerde identiteit maken met PowerShell

Een beheerde identiteit maken tijdens de toewijzing van het beleid, **locatie** moet worden gedefinieerd en **AssignIdentity** gebruikt. Het volgende voorbeeld wordt de definitie van het ingebouwde beleid **transparent data encryption voor SQL DB implementeren**, de doelresourcegroep die is ingesteld en vervolgens de toewijzing maakt.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the built-in "Deploy SQL DB transparent data encryption" policy definition
$policyDef = Get-AzPolicyDefinition -Id '/providers/Microsoft.Authorization/policyDefinitions/86a912f6-9a06-4e26-b447-11b16ba8659f'

# Get the reference to the resource group
$resourceGroup = Get-AzResourceGroup -Name 'MyResourceGroup'

# Create the assignment using the -Location and -AssignIdentity properties
$assignment = New-AzPolicyAssignment -Name 'sqlDbTDE' -DisplayName 'Deploy SQL DB transparent data encryption' -Scope $resourceGroup.ResourceId -PolicyDefinition $policyDef -Location 'westus' -AssignIdentity
```

De `$assignment` variabele bevat nu de principal-ID van de beheerde identiteit, samen met de waarden die zijn geretourneerd bij het maken van een beleidstoewijzing. Dit kan worden geopend via `$assignment.Identity.PrincipalId`.

### <a name="grant-defined-roles-with-powershell"></a>Verleen gedefinieerd rollen met PowerShell

De nieuwe beheerde identiteit moet replicatie via Azure Active Directory voltooien voordat deze de vereiste rollen kan worden verleend. Wanneer replicatie voltooid is, het volgende voorbeeld doorloopt de beleidsdefinitie in `$policyDef` voor de **roleDefinitionIds** en maakt gebruik van [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) te kennen van de nieuwe beheerde identiteit het rollen.

```azurepowershell-interactive
# Use the $policyDef to get to the roleDefinitionIds array
$roleDefinitionIds = $policyDef.Properties.policyRule.then.details.roleDefinitionIds

if ($roleDefinitionIds.Count -gt 0)
{
    $roleDefinitionIds | ForEach-Object {
        $roleDefId = $_.Split("/") | Select-Object -Last 1
        New-AzRoleAssignment -Scope $resourceGroup.ResourceId -ObjectId $assignment.Identity.PrincipalId -RoleDefinitionId $roleDefId
    }
}
```

### <a name="grant-defined-roles-through-portal"></a>Verleen rollen via portal gedefinieerd

Er zijn twee manieren om toegang te verlenen beheerde identiteit van een toewijzing van de gedefinieerde rollen met behulp van de portal, met behulp van **toegangsbeheer (IAM)** of door te klikken en de toewijzing van een beleid of initiatief bewerken **opslaan**.

Een rol toevoegen aan de beheerde identiteit van de toewijzing, de volgende stappen uit:

1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

1. Selecteer **Toewijzingen** in het linkerdeelvenster van de Azure Policy-pagina.

1. Zoek de toewijzing die een beheerde identiteit heeft en klik op de naam.

1. Zoek de **toewijzings-ID** eigenschap op de pagina bewerken. De toewijzings-ID is ongeveer als volgt:

   ```output
   /subscriptions/{subscriptionId}/resourceGroups/PolicyTarget/providers/Microsoft.Authorization/policyAssignments/2802056bfc094dfb95d4d7a5
   ```

   De naam van de beheerde identiteit is het laatste gedeelte van de toewijzing van resource-ID, die is `2802056bfc094dfb95d4d7a5` in dit voorbeeld. Kopieer dit gedeelte van de toewijzing van resource-ID.

1. Navigeer naar de resource of de resources bovenliggende container (resourcegroep, abonnement, beheergroep) waarvoor de roldefinitie die handmatig zijn toegevoegd.

1. Klik op de **toegangsbeheer (IAM)** op de pagina resources koppelen en klik op **+ roltoewijzing toevoegen** aan de bovenkant van de pagina voor het beheer van toegang.

1. Selecteer de juiste rol die overeenkomt met een **roleDefinitionIds** van de beleidsdefinitie. Laat **toegang toewijzen aan** ingesteld op de standaardwaarde van 'Azure AD gebruiker, groep of toepassing'. In de **Selecteer** vak, plakt of typt u het gedeelte van de resource-ID van toewijzing zich eerder hebt. Wanneer de zoekopdracht is voltooid, klikt u op het object met dezelfde naam-ID selecteren en op **opslaan**.

## <a name="create-a-remediation-task"></a>Een herstel-taak maken

Tijdens de evaluatie, de beleidstoewijzing met **deployIfNotExists** effect bepaalt u of er niet-compatibele resources zijn. Als niet-compatibele resources worden gevonden, de details zijn opgegeven op de **herstel** pagina. Samen met de lijst met beleidsregels die u niet-compatibele resources hebt is de optie voor het activeren van een **herstel taak**. Deze optie is wat maakt een implementatie van de **deployIfNotExists** sjabloon.

Maakt een **herstel taak**, als volgt te werk:

1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

   ![Zoeken naar beleid](../media/remediate-resources/search-policy.png)

1. Selecteer **herstel** aan de linkerkant van de Azure Policy-pagina.

   ![Herstel selecteren](../media/remediate-resources/select-remediation.png)

1. Alle **deployIfNotExists** beleidstoewijzingen met niet-compatibele resources zijn opgenomen in de **beleidsregels voor het herstellen** tabblad en een gegevenstabel. Klik op een beleid met de resources die niet compatibel zijn. De **nieuwe herstel taak** pagina wordt geopend.

   > [!NOTE]
   > Een alternatieve manier om te openen de **herstel taak** pagina is om te zoeken en klik op het beleid op basis van de **naleving** pagina en klik vervolgens op de **herstel-taak maken** knop.

1. Op de **nieuwe herstel taak** pagina, filteren de resources voor het herstellen met behulp van de **bereik** weglatingstekens om op te halen van de onderliggende resources van waaraan het beleid is toegewezen (inclusief omlaag naar de afzonderlijke resource objecten). Bovendien gebruiken de **locaties** vervolgkeuzelijst als filter de resources. Alleen de resources die worden vermeld in de tabel worden hersteld.

   ![Herstellen - resources selecteren](../media/remediate-resources/select-resources.png)

1. De herstel-taak starten wanneer de resources zijn gefilterd door te klikken op **herstellen**. De pagina van de naleving van het beleid wordt geopend aan de **herstel taken** tabblad om de status van de voortgang van de taken weer te geven.

   ![Herstellen - taak wordt uitgevoerd](../media/remediate-resources/task-progress.png)

1. Klik op de **herstel taak** van de pagina voor de naleving van beleid voor meer informatie over de voortgang. De filters die wordt gebruikt voor de taak wordt weergegeven, samen met een lijst van de resources die worden hersteld.

1. Uit de **herstel taak** pagina, met de rechtermuisknop op een resource om de implementatie van de herstel-taak weer te geven of de resource. Aan het einde van de rij, klikt u op **gerelateerde gebeurtenissen** om details, zoals een foutbericht te bekijken.

   ![Herstellen - contextmenu van de resource-taak](../media/remediate-resources/resource-task-context-menu.png)

Resources worden geïmplementeerd via een **herstel taak** worden toegevoegd aan de **geïmplementeerd Resources** tabblad op de pagina voor naleving van beleid.

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [Azure Policy-voorbeelden](../samples/index.md)
- Controleer de [structuur van beleidsdefinities](../concepts/definition-structure.md)
- Beoordeling [effecten beleid begrijpen](../concepts/effects.md)
- Begrijpen hoe u [programmatisch beleid maken](programmatically-create.md)
- Meer informatie over het [Nalevingsgegevens ophalen](getting-compliance-data.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../../management-groups/overview.md)