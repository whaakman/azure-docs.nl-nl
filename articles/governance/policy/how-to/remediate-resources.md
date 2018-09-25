---
title: Herstellen van niet-compatibele resources met Azure Policy
description: Deze instructies begeleidt u bij het herstel van resources die niet aan beleidsregels in Azure Policy voldoen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 747650bc47644cdca07f705f42d063c995ebe9bf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980250"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Herstellen van niet-compatibele resources met Azure Policy

Resources die niet voldoen aan een **deployIfNotExists** beleid kan worden geplaatst in een compatibele status via **herstel**. Herstel wordt bereikt door het beleid om uit te voeren dat de **deployIfNotExists** effect van het toegewezen beleid op uw bestaande resources. Deze procedures worden de stappen die nodig zijn voor u doen dit beschreven.

## <a name="how-remediation-security-works"></a>Hoe herstel beveiliging werkt

Wanneer de sjabloon in beleid wordt uitgevoerd de **deployIfNotExists** beleidsdefinitie, dit gebeurt met behulp van een [beheerde identiteit](../../../active-directory/managed-identities-azure-resources/overview.md).
Beleid wordt gemaakt van een beheerde identiteit voor elke toewijzing voor u, maar meer informatie over welke functies voor het verlenen van de beheerde identiteit moet worden opgegeven. Als de beheerde identiteit rollen ontbreekt, wordt dit tijdens de toewijzing van het beleid of een initiatief met het beleid weergegeven. Wanneer u de portal, beleid wordt automatisch de beheerde identiteit het vermelde rollen toewijzen nadat de toewijzing wordt gestart.

![Beheerde identiteit - ontbrekende functie](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> Als een resource die is gewijzigd door **deployIfNotExists** buiten het bereik van de beleidstoewijzing, beheerde identiteit van de toewijzing via een programma moet toegang wordt verleend of mislukt de implementatie van het herstel.

## <a name="configure-policy-definition"></a>Beleidsdefinitie configureren

De eerste stap is het definiëren van de rollen die **deployIfNotExists** moet in de beleidsdefinitie voor een succesvolle implementatie van de inhoud van de sjabloon opgenomen. Onder de **details** eigenschap toevoegen een **roleDefinitionIds** eigenschap. Dit is een matrix met tekenreeksen die overeenkomen met de rollen in uw omgeving.
Zie voor een compleet voorbeeld de [deployIfNotExists voorbeeld](../concepts/effects.md#deployifnotexists-example).

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
Get-AzureRmRoleDefinition -Name 'Contributor'
```

## <a name="create-a-remediation-task"></a>Een herstel-taak maken

Tijdens de evaluatie, de beleidstoewijzing met **deployIfNotExists** effect bepaalt u of er niet-compatibele resources zijn. Als niet-compatibele resources worden gevonden, de details zijn opgegeven op de **herstel** pagina. Samen met de lijst met beleidsregels die u niet-compatibele resources hebt is de optie voor het activeren van een **herstel taak**. Dit is wat maakt een implementatie van de **deployIfNotExists** sjabloon.

Maakt een **herstel taak**, als volgt te werk:

1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

   ![Zoeken naar beleid](../media/remediate-resources/search-policy.png)

1. Selecteer **herstel** aan de linkerkant van de Azure Policy-pagina.

   ![Herstel selecteren](../media/remediate-resources/select-remediation.png)

1. Alle **deployIfNotExists** beleidstoewijzingen met niet-compatibele resources zijn opgenomen in de **beleidsregels voor het herstellen** tabblad en een gegevenstabel. Klik op een beleid met de resources die niet compatibel zijn. De **nieuwe herstel taak** pagina wordt geopend.

   > [!NOTE]
   > Een alternatieve manier om te openen de **herstel taak** pagina is om te zoeken en klik op het beleid op basis van de **naleving** pagina en klik vervolgens op de **herstel-taak maken** knop.

1. Op de **nieuwe herstel taak** pagina, filteren de resources voor het herstellen met behulp van de **bereik** weglatingstekens om op te halen van de onderliggende resources waar het beleid is toegewezen (inclusief omlaag naar de afzonderlijke resource objecten). Bovendien gebruiken de **locaties** vervolgkeuzelijst als filter de resources. Alleen de resources die worden vermeld in de tabel worden hersteld.

   ![Herstellen - resources selecteren](../media/remediate-resources/select-resources.png)

1. De herstel-taak starten wanneer de resources zijn gefilterd door te klikken op **herstellen**. De pagina van de naleving van het beleid wordt geopend aan de **herstel taken** tabblad om de status van de voortgang van de taken weer te geven.

   ![Herstellen - taak wordt uitgevoerd](../media/remediate-resources/task-progress.png)

1. Klik op de **herstel taak** van de pagina voor de naleving van beleid voor meer informatie over de voortgang. De filters die wordt gebruikt voor de taak worden weergegeven, samen met een lijst van de resources die worden hersteld.

1. Uit de **remedation taak** pagina, met de rechtermuisknop op een resource om de implementatie van de herstel-taak weer te geven of de resource. Aan het einde van de rij, klikt u op **gerelateerde gebeurtenissen** om details, zoals een foutbericht te bekijken.

   ![Herstellen - contextmenu van de resource-taak](../media/remediate-resources/resource-task-context-menu.png)

Resources worden geïmplementeerd via een **herstel taak** wordt toegevoegd aan de **geïmplementeerd Resources** tabblad op de pagina voor de naleving van beleid na een korte vertraging.

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [Azure Policy-voorbeelden](../samples/index.md)
- Controleer de [structuur van beleidsdefinities](../concepts/definition-structure.md)
- Beoordeling [effecten beleid begrijpen](../concepts/effects.md)
- Begrijpen hoe u [programmatisch beleid maken](programmatically-create.md)
- Meer informatie over het [Nalevingsgegevens ophalen](getting-compliance-data.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../../management-groups/overview.md)