---
title: Azure-resources verplaatsen naar een nieuw abonnement of de resource-groep | Microsoft Docs
description: Azure Resource Manager gebruiken voor resources verplaatsen naar een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 01ec8facf2771de9ec01b9470521340a59ee4d0d
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721380"
---
# <a name="move-resources-to-a-new-resource-group-or-subscription"></a>Resources verplaatsen naar een nieuwe resourcegroep of abonnement

In dit artikel wordt beschreven hoe u Azure-resources verplaatsen naar een andere Azure-abonnement of een andere resourcegroep onder hetzelfde abonnement. U kunt de Azure portal, Azure PowerShell, Azure CLI of de REST-API gebruiken om resources te verplaatsen.

Zowel de brongroep en de doelgroep worden vergrendeld tijdens de verplaatsing. Schrijf en verwijderbewerkingen op de brongroepen zijn geblokkeerd totdat de verplaatsing is voltooid. Deze vergrendeling betekent dat u kunt geen toevoegen, bijwerken of verwijderen van resources in de resourcegroepen, maar dit betekent niet dat de resources zijn geblokkeerd. Als u een SQL-Server en de bijbehorende database naar een nieuwe resourcegroep verplaatsen, er een toepassing die gebruikmaakt van de database zonder uitvaltijd. Dit kan nog steeds lezen en schrijven naar de database.

Verplaatsen van een resource alleen verplaatst naar een nieuwe resourcegroep of abonnement. Het verandert niet de locatie van de resource.

## <a name="checklist-before-moving-resources"></a>Controlelijst voor het verplaatsen van resources

Voordat u een resource verplaatst, moeten er enkele belangrijke stappen worden uitgevoerd. U kunt fouten voorkomen door te controleren of aan de volgende voorwaarden is voldaan.

1. De resources die u wilt verplaatsen, moeten de verplaatsing ondersteunen. Zie voor een lijst van deze ondersteuning voor resources verplaatsen, [bewerking ondersteuning voor resources verplaatsen](move-support-resources.md).

1. Bepaalde services zijn specifieke beperkingen of vereisten bij het verplaatsen van resources. Als u een van de volgende services te verplaatsen hebt, controleert u die richtlijnen voordat u doorgaat.

   * [App Services verplaatsen richtlijnen](./move-limitations/app-service-move-limitations.md)
   * [Azure DevOps-Services verplaatsen richtlijnen](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
   * [Uitleg van de klassieke implementatie model verplaatsen](./move-limitations/classic-model-move-limitations.md) -klassieke Compute, klassieke opslag, klassieke virtuele netwerken en Cloud Services
   * [Recovery Services verplaatsen richtlijnen](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
   * [Virtuele Machines verplaatsen richtlijnen](./move-limitations/virtual-machines-move-limitations.md)
   * [Virtuele netwerken verplaatsen richtlijnen](./move-limitations/virtual-network-move-limitations.md)

1. De bron- en doelabonnementen moeten actief zijn. Als u problemen ondervindt bij het inschakelen van een account dat is uitgeschakeld, [maken van een Azure-ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer **Abonnementsbeheer** voor het probleemtype.

1. De bron- en doelabonnementen moeten aanwezig zijn in dezelfde [Azure Active Directory-tenant](../active-directory/develop/quickstart-create-new-tenant.md). Om te controleren dat beide abonnementen dezelfde tenant-ID hebben, gebruikt u Azure PowerShell of Azure CLI.

   Voor Azure PowerShell gebruiken:

   ```azurepowershell-interactive
   (Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
   (Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
   ```

   Gebruik voor Azure CLI:

   ```azurecli-interactive
   az account show --subscription <your-source-subscription> --query tenantId
   az account show --subscription <your-destination-subscription> --query tenantId
   ```

   Als de tenant-id's voor de bron- en doelabonnementen niet hetzelfde zijn, moet u de volgende methoden gebruiken om af te stemmen van de tenant-id's:

   * [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md)
   * [Een Azure-abonnement koppelen of toevoegen aan Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Het doelabonnement moet zijn geregistreerd voor de resourceprovider van de resource die wordt verplaatst. Als u niet het geval is, ontvangt u een foutmelding waarin staat dat de **abonnement is niet geregistreerd voor een resourcetype**. U kunt deze fout tegenkomen wanneer u een resource verplaatst naar een nieuw abonnement maar in dat abonnement nooit is gebruikt met dat resourcetype.

   Gebruik de volgende opdrachten om op te halen van de registratiestatus voor PowerShell:

   ```azurepowershell-interactive
   Set-AzContext -Subscription <destination-subscription-name-or-id>
   Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
   ```

   Gebruik het volgende voor het registreren van een resourceprovider:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
   ```

   Gebruik de volgende opdrachten om op te halen van de status van de registratie voor Azure CLI:

   ```azurecli-interactive
   az account set -s <destination-subscription-name-or-id>
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
   ```

   Gebruik het volgende voor het registreren van een resourceprovider:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Batch
   ```

1. Het verplaatsen van de resources account moet ten minste de volgende machtigingen hebben:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** op de resourcegroep.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** op de doel-resourcegroep.

1. Controleer voordat u de resources verplaatst, de abonnementquota voor het abonnement dat u de resources wilt verplaatsen. Als het verplaatsen van de resources betekent dat het abonnement overschrijdt de grenzen, moet u om te controleren of u een verhoging van de quota kan opvragen. Zie voor een lijst van beperkingen en hoe u een verhoging [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).

## <a name="validate-move"></a>Verplaatsen valideren

De [verplaatsingsbewerking valideren](/rest/api/resources/resources/validatemoveresources) kunt u het testen van uw scenario verplaatsen zonder daadwerkelijk de resources te verplaatsen. Deze bewerking gebruiken om te controleren als de verplaatsing slaagt. Validatie wordt automatisch aangeroepen wanneer u een aanvraag voor verplaatsen verzenden. Gebruik deze bewerking alleen als u wilt de resultaten vooraf. Als u wilt deze bewerking uitvoeren, moet u de:

* naam van de resourcegroep
* Resource-ID van de doelresourcegroep
* Resource-ID van elke resource verplaatsen
* de [toegangstoken](/rest/api/azure/#acquire-an-access-token) voor uw account

De volgende aanvraag verzenden:

```HTTP
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2019-05-10
Authorization: Bearer <access-token>
Content-type: application/json
```

Met een aanvraagtekst:

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Als de aanvraag is ingedeeld, retourneert de bewerking:

```HTTP
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

De statuscode 202 geeft aan dat de validatieaanvraag is geaccepteerd, maar deze nog niet nog bepaald als de bewerking voor verplaatsen slaagt. De `location` waarde bevat een URL die u gebruikt om de status van de langdurige bewerking te controleren.  

Om te controleren of de status, de volgende aanvraag te verzenden:

```HTTP
GET <location-url>
Authorization: Bearer <access-token>
```

Terwijl de bewerking wordt nog steeds uitgevoerd, wordt u voor het ontvangen van de statuscode 202 blijven. Wacht het aantal seconden aangegeven in de `retry-after` waarde voordat u doorgaat. Als de verplaatsing is gevalideerd, ontvangt u de statuscode 204. Als de validatie voor het verplaatsen is mislukt, ontvangt u een foutbericht weergegeven, zoals:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="use-the-portal"></a>Gebruik de portal

Selecteer de resourcegroep met de resources voor het verplaatsen van resources, en selecteer vervolgens de **verplaatsen** knop.

![resources verplaatsen](./media/resource-group-move-resources/select-move.png)

Selecteer of u de resources naar een nieuwe resourcegroep of een nieuw abonnement verplaatsen wilt.

Selecteer de resources wilt verplaatsen en de resourcegroep van de bestemming. Bevestig dat u wilt bijwerken scripts voor deze resources, en selecteer **OK**. Als u het pictogram bewerken abonnement in de vorige stap hebt geselecteerd, moet u ook het doelabonnement.

![doel selecteren](./media/resource-group-move-resources/select-destination.png)

In **meldingen**, ziet u dat de verplaatsing wordt uitgevoerd.

![move-status weergeven](./media/resource-group-move-resources/show-status.png)

Wanneer deze is voltooid, krijgt u een melding van het resultaat.

![Zet resultaten weergeven](./media/resource-group-move-resources/show-result.png)

Als er een fout optreedt, raadpleegt u [problemen met Azure-resources verplaatsen naar nieuwe resourcegroep of abonnement](troubleshoot-move.md).

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

Als u bestaande resources naar een andere resourcegroep of abonnement, gebruikt u de [verplaatsen AzResource](/powershell/module/az.resources/move-azresource) opdracht. Het volgende voorbeeld ziet hoe u verschillende resources verplaatsen naar een nieuwe resourcegroep.

```azurepowershell-interactive
$webapp = Get-AzResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Als u wilt verplaatsen naar een nieuw abonnement, een waarde bevatten voor de `DestinationSubscriptionId` parameter.

Als er een fout optreedt, raadpleegt u [problemen met Azure-resources verplaatsen naar nieuwe resourcegroep of abonnement](troubleshoot-move.md).

## <a name="use-azure-cli"></a>Azure CLI gebruiken

Als u bestaande resources naar een andere resourcegroep of abonnement, gebruikt u de [verplaatsen van de az resource](/cli/azure/resource?view=azure-cli-latest#az-resource-move) opdracht. Geef de resource-id's van de resources te verplaatsen. Het volgende voorbeeld ziet hoe u verschillende resources verplaatsen naar een nieuwe resourcegroep. In de `--ids` parameter, Geef een door spaties gescheiden lijst van de resource-id's te verplaatsen.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Als u wilt verplaatsen naar een nieuw abonnement, bieden de `--destination-subscription-id` parameter.

Als er een fout optreedt, raadpleegt u [problemen met Azure-resources verplaatsen naar nieuwe resourcegroep of abonnement](troubleshoot-move.md).

## <a name="use-rest-api"></a>REST API gebruiken

Als u bestaande resources naar een andere resourcegroep of abonnement, gebruikt u de [resources verplaatsen](/rest/api/resources/Resources/MoveResources) bewerking.

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

In de hoofdtekst van de aanvraag geeft u de doelresourcegroep die is en de resources te verplaatsen.

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Als er een fout optreedt, raadpleegt u [problemen met Azure-resources verplaatsen naar nieuwe resourcegroep of abonnement](troubleshoot-move.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst van deze ondersteuning voor resources verplaatsen, [bewerking ondersteuning voor resources verplaatsen](move-support-resources.md).
