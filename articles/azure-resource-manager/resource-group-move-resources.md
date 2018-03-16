---
title: Azure-resources naar nieuwe abonnement of de resource-groep verplaatsen | Microsoft Docs
description: Azure Resource Manager gebruiken voor resources verplaatsen naar een nieuwe resourcegroep of abonnement.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: tomfitz
ms.openlocfilehash: 4709ee707aa67c8de531b2b3e0b58dbed5c2667b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Resources verplaatsen naar de nieuwe resourcegroep of abonnement

In dit artikel leest u hoe resources verplaatsen naar een nieuw abonnement of een nieuwe resourcegroep in hetzelfde abonnement. U kunt de portal, PowerShell, Azure CLI of de REST-API resource verplaatsen. De verplaatsing bewerkingen in dit artikel voor u beschikbaar zijn zonder eventuele hulp van de ondersteuning van Azure.

Bij het verplaatsen van resources, worden zowel de bronnengroep als de doelgroep vergrendeld tijdens de bewerking. Schrijven en verwijderen van bewerkingen op de brongroepen worden geblokkeerd totdat de verplaatsing is voltooid. Deze vergrendeling betekent dat u niet kunt toevoegen, bijwerken of verwijderen van resources in de resourcegroepen, maar dit betekent niet dat de bronnen worden bevroren. Als u een SQL-Server en de bijbehorende database naar een nieuwe resourcegroep verplaatst, ervaringen een toepassing die gebruikmaakt van de database zonder uitvaltijd. Dit kan nog steeds lezen en schrijven naar de database.

U kunt de locatie van de resource niet wijzigen. Het verplaatsen van een resource alleen verplaatst naar een nieuwe resourcegroep. De nieuwe resourcegroep mag een andere locatie hebben, maar die niet de locatie van de bron wordt gewijzigd.

> [!NOTE]
> Dit artikel wordt beschreven hoe u resources binnen een bestaande Azure-account aanbieden verplaatst. Als u daadwerkelijk wijzigen van uw Azure-account biedt (zoals een upgrade van betalen naar gebruik vooraf betalen wilt) maar blijft werken met uw bestaande bronnen, Zie [overschakelen van uw Azure-abonnement op een andere aanbieding](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Controlelijst voor het verplaatsen van resources

Voordat u een resource verplaatst, moeten er enkele belangrijke stappen worden uitgevoerd. U kunt fouten voorkomen door te controleren of aan de volgende voorwaarden is voldaan.

1. De bron- en doelserver abonnementen moeten bestaan binnen dezelfde [Azure Active Directory-tenant](../active-directory/active-directory-howto-tenant.md). Om te controleren of beide abonnementen dezelfde tenant-ID hebben, gebruikt u Azure PowerShell of Azure CLI.

  Azure PowerShell gebruiken:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Gebruik voor Azure CLI:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Als de tenant-id's voor de bron- en doelserver abonnementen niet hetzelfde zijn, moet u de volgende methoden gebruiken om af te stemmen van de tenant-id's: 

  * [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md)
  * [Het koppelen of een Azure-abonnement toevoegen aan Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md)

2. De service moet de mogelijkheid activeren om resources te verplaatsen. Dit artikel vindt welke services kunnen verplaatsen bronnen en welke services zwevende resources niet inschakelen.
3. Het doelabonnement moet zijn geregistreerd voor de resourceprovider van de resource die wordt verplaatst. Als u niet het geval is, er een foutbericht met de mededeling dat de **abonnement is niet geregistreerd voor een brontype**. U kunt dit probleem tegenkomen bij het verplaatsen van een resource naar een nieuw abonnement, terwijl dat abonnement nooit is gebruikt met dat type resource.

  Gebruik de volgende opdrachten om de registratiestatus voor PowerShell:

  ```powershell
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Voor het registreren van een resourceprovider gebruiken:

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
  ```

  Gebruik de volgende opdrachten om de registratiestatus voor Azure CLI:

  ```azurecli-interactive
  az account set -s <destination-subscription-name-or-id>
  az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
  ```

  Voor het registreren van een resourceprovider gebruiken:

  ```azurecli-interactive
  az provider register --namespace Microsoft.Batch
  ```

## <a name="when-to-call-support"></a>Het aanroepen van ondersteuning

U kunt de meeste resources via de selfservice bewerkingen die wordt weergegeven in dit artikel kunt verplaatsen. De bewerkingen selfservice gebruiken:

* Resource Manager-resources wilt verplaatsen.
* Verplaatsen van klassieke resources volgens de [klassieke implementatie beperkingen](#classic-deployment-limitations).

Neem contact op met [ondersteunen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) wanneer u moet:

* Uw resources verplaatsen naar een nieuwe Azure-account (en Azure Active Directory-tenant) en u hulp nodig bij de instructies in de vorige sectie.
* Klassieke resources verplaatsen, maar hebben problemen met de beperkingen.

## <a name="services-that-can-be-moved"></a>Services die kunnen worden verplaatst

De services waarmee verplaatsen naar een nieuwe resourcegroep en een abonnement zijn:

* API Management
* App Service-apps (web-apps) - Zie [App Service-beperkingen](#app-service-limitations)
* Application Insights
* Automatisering
* Azure Cosmos DB
* Batch
* Bing Maps
* CDN
* Cloud Services - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
* Cognitive Services
* Content Moderator
* Data Catalog
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Event Hubs
* HDInsight-clusters - Zie [HDInsight-beperkingen](#hdinsight-limitations)
* IoT Hubs
* Key Vault
* Load Balancers - Zie [Load Balancer-beperkingen](#lb-limitations)
* Logic Apps
* Machine Learning
* Media Services
* Mobile Engagement
* Notification Hubs
* Operational Insights
* Operations Management
* Power BI
* Openbare IP - Zie [openbare IP-beperkingen](#pip-limitations)
* Redis Cache
* Scheduler
* Search
* Server Management
* Service Bus
* Service Fabric
* Storage
* Opslag (klassiek) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
* Stream Analytics - Stream Analytics-taken kunnen niet worden verplaatst, bij uitvoering in status.
* SQL Database-server - database en de server moet zich bevinden in dezelfde resourcegroep. Wanneer u een SQL-server hebt verplaatst, worden ook alle databases die zijn verplaatst.
* Traffic Manager
* Virtuele Machines - beheerde schijven kunnen niet worden verplaatst. Zie [beperkingen van de virtuele Machines](#virtual-machines-limitations)
* Virtuele Machines (klassiek) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
* Virtuele-Machineschaalsets - Zie [beperkingen van de virtuele Machines](#virtual-machines-limitations)
* Virtuele netwerken - Zie [beperkingen van de virtuele netwerken](#virtual-networks-limitations)
* VPN Gateway

## <a name="services-that-cannot-be-moved"></a>Services die kunnen niet worden verplaatst

De services die op dit moment niet inschakelen voor het verplaatsen van een resource zijn:

* AD Domain Services
* AD Hybrid Health Service
* Application Gateway
* BizTalk Services
* Container Service
* ExpressRoute
* DevTest Labs - te verplaatsen naar een nieuwe resourcegroep in hetzelfde abonnement is ingeschakeld, maar cross abonnement verplaatsen is niet ingeschakeld.
* Dynamics LCS
* Load Balancers - Zie [Load Balancer-beperkingen](#lb-limitations)
* Beheerde toepassingen
* Beheerde schijven - Zie [beperkingen van de virtuele Machines](#virtual-machines-limitations)
* Openbare IP - Zie [openbare IP-beperkingen](#pip-limitations)
* Recovery Services-kluis - ook komen niet verplaatsen van de Compute, Network en Storage-resources die zijn gekoppeld aan de Recovery Services-kluis, Zie [Recovery Services-beperkingen](#recovery-services-limitations).
* Beveiliging
* StorSimple-apparaatbeheerfunctie
* Virtuele netwerken (klassiek) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Beperkingen van de virtuele Machines

Beheerde schijven bieden geen ondersteuning voor verplaatsen. Deze beperking betekent dat meerdere verwante bronnen te kunnen niet worden verplaatst. U kunt niet verplaatsen:

* Managed Disks
* Virtuele machines met de beheerde schijven
* Installatiekopieën die zijn gemaakt op basis van beheerde schijven
* Momentopnamen die zijn gemaakt van beheerde schijven
* Beschikbaarheidssets met virtuele machines met beheerde schijven

Virtuele machines die zijn gemaakt op basis van de Marketplace-resources met plannen gekoppeld kan niet worden verplaatst tussen resourcegroepen of abonnementen. De virtuele machine in het huidige abonnement inrichting ervan ongedaan en implementeer opnieuw in het nieuwe abonnement.

Virtuele Machines met Sleutelkluis opgeslagen certificaat kan worden verplaatst naar een nieuwe resourcegroep in hetzelfde abonnement, maar niet voor abonnementen.

## <a name="virtual-networks-limitations"></a>Beperkingen voor virtuele netwerken

Als u wilt een peered virtueel netwerk verplaatsen, moet u eerst het virtuele netwerk van de peering uitschakelen. Als uitgeschakeld, kunt u het virtuele netwerk kunt verplaatsen. Na de verplaatsing opnieuw inschakelen het virtuele netwerk van de peering.

U kunt een virtueel netwerk niet verplaatsen naar een ander abonnement als het virtuele netwerk een subnet met resourcenavigatiekoppelingen bevat. Als een Redis-Cache-bron in een subnet is geïmplementeerd, heeft dat subnet een navigatiekoppeling resource.

## <a name="app-service-limitations"></a>App Service-beperkingen

De beperkingen voor het verplaatsen van resources in App Service verschillen op basis van of het verplaatsen van de resources binnen een abonnement of in een nieuw abonnement.

### <a name="moving-within-the-same-subscription"></a>Verplaatsen binnen hetzelfde abonnement

Wanneer u een Web-App verplaatst _binnen hetzelfde abonnement_, u kunt de geüploade SSL-certificaten niet verplaatsen. Echter kunt u een Web-App in de nieuwe resourcegroep zonder het verplaatsen van de geüploade SSL-certificaat en de SSL-functionaliteit van uw app werkt nog altijd. 

Als u verplaatsen van het SSL-certificaat met de Web-App wilt, voert u de volgende stappen uit:

1.  Het geüploade certificaat verwijderen uit de Web-App.
2.  Verplaats de Web-App.
3.  Upload het certificaat naar de verplaatste Web-App.

### <a name="moving-across-subscriptions"></a>Verplaatsen tussen abonnementen

Wanneer u een Web-App verplaatst _voor abonnementen_, gelden de volgende beperkingen:

- De resourcegroep bestemming mag geen alle bestaande App Service-resources. App Service-resources omvatten:
    - Web Apps
    - App Service-abonnementen
    - Geüploade of geïmporteerde SSL-certificaten
    - App Service-omgevingen
- Alle App Service-resources in de resourcegroep moeten samen worden verplaatst.
- App Service-bronnen kunnen alleen worden verplaatst van de resourcegroep waarin ze werden gemaakt. Als een App Service-resource niet meer in de oorspronkelijke resourcegroep is, deze moet worden teruggezet naar die oorspronkelijke resourcegroep eerst en vervolgens kunnen worden verplaatst tussen abonnementen. 

## <a name="classic-deployment-limitations"></a>Beperkingen voor klassieke implementatie

De opties voor het verplaatsen van resources die zijn geïmplementeerd via het klassieke model verschillen op basis van of het verplaatsen van de resources binnen een abonnement of in een nieuw abonnement.

### <a name="same-subscription"></a>Hetzelfde abonnement

Bij het verplaatsen van resources van een resourcegroep naar een andere resourcegroep binnen hetzelfde abonnement, gelden de volgende beperkingen:

* Virtuele netwerken (klassiek) kunnen niet worden verplaatst.
* Virtuele machines (klassiek) moeten worden verplaatst met de cloudservice.
* Cloudservice kan alleen worden verplaatst, wanneer de verplaatsing alle virtuele machines bevat.
* Slechts één cloudservice kan tegelijk worden verplaatst.
* Slechts één opslagaccount (klassiek) kan tegelijk worden verplaatst.
* Storage-account (klassiek) kan niet worden verplaatst in dezelfde bewerking met een virtuele machine of een cloudservice.

Klassieke om resources te verplaatsen naar een nieuwe resourcegroep binnen hetzelfde abonnement, gebruiken de standaard migratiebewerkingen via de [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), of [REST-API](#use-rest-api). U dezelfde bewerkingen zoals u voor het verplaatsen van Resource Manager-resources.

### <a name="new-subscription"></a>Nieuw abonnement

Bij het verplaatsen van resources in een nieuw abonnement, gelden de volgende beperkingen:

* Alle klassieke resources in het abonnement moeten worden verplaatst in dezelfde bewerking.
* Het doelabonnement mag geen andere klassieke resources bevatten.
* De verplaatsing kan alleen worden aangevraagd via een afzonderlijke REST-API voor klassieke verplaatst. De standaardopdrachten verplaatsen van Resource Manager werken niet bij het klassieke resources verplaatsen naar een nieuw abonnement.

Klassieke om resources te verplaatsen naar een nieuw abonnement, de REST-bewerkingen die specifiek voor klassieke resources zijn te gebruiken. Voor het gebruik van REST, moet u de volgende stappen uitvoeren:

1. Controleer als het bronabonnement deel uitmaken van een verplaatsing abonnementoverschrijdende. Gebruik de volgende bewerking:

  ```HTTP
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     In de aanvraagtekst zijn onder andere:

  ```json
  {
    "role": "source"
  }
  ```

     De reactie voor de validatiebewerking is in de volgende indeling:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Controleer of het doelabonnement in een verplaatsing abonnementoverschrijdende deelnemen kan. Gebruik de volgende bewerking:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     In de aanvraagtekst zijn onder andere:

  ```json
  {
    "role": "target"
  }
  ```

     De reactie is in dezelfde indeling als de validatie van het abonnement.
3. Verplaats alle klassieke resources uit één abonnement als beide abonnementen gevalideerd worden, een ander abonnement met de volgende bewerking:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    In de aanvraagtekst zijn onder andere:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

De bewerking kan enkele minuten uitgevoerd.

## <a name="recovery-services-limitations"></a>Recovery Services-beperkingen

Verplaatsen is niet ingeschakeld voor de opslag, netwerk, of Compute-resources die worden gebruikt voor het instellen van herstel na noodgevallen met Azure Site Recovery.

Stel dat u de replicatie van uw lokale machines naar een opslagaccount (Storage1) hebt ingesteld en wilt dat de beveiligde machine actief na een failover naar Azure als een virtuele machine (VM1) gekoppeld aan een virtueel netwerk (Network1). U kunt deze Azure-resources - Storage1, VM1 en Network1 - niet verplaatsen, tussen resourcegroepen binnen hetzelfde abonnement of tussen abonnementen.

Verplaatsen van een virtuele machine geregistreerd bij **Azure backup** tussen resourcegroepen:
 1. Tijdelijk stoppen van de back-up en bewaren van back-upgegevens
 2. Verplaats de VM naar de doelresourcegroep
 3. Beveiligt onder de dezelfde/nieuwe kluis die gebruikers met de beschikbare herstelpunten gemaakt vóór de verplaatsing herstellen kunnen.
Als de gebruiker de virtuele machine van een back-up voor abonnementen verplaatst, blijven stap 1 en 2 hetzelfde. Gebruiker moet de virtuele machine met een nieuwe kluis aanwezig / worden gemaakt in het doelabonnement beveiligen in stap 3. Recovery Services-kluis biedt geen ondersteuning voor back-ups van cross-abonnement.

## <a name="hdinsight-limitations"></a>HDInsight-beperkingen

U kunt de HDInsight-clusters verplaatsen naar een nieuw abonnement of resourcegroep. U kunt echter niet verplaatsen tussen abonnementen de netwerkresources die zijn gekoppeld aan het HDInsight-cluster (zoals het virtuele netwerk, een NIC of een load balancer). Bovendien verplaatsen u niet naar een nieuwe resourcegroep een NIC die is gekoppeld aan een virtuele machine voor het cluster.

Wanneer u een HDInsight-cluster naar een nieuw abonnement verplaatst, moet u eerst andere bronnen (zoals het storage-account) verplaatsen. Verplaats het HDInsight-cluster op zichzelf.

## <a name="search-limitations"></a>Beperkingen voor zoeken

U kunt meerdere zoeken in resources in één keer in verschillende regio's geplaatst niet verplaatsen.
In dat geval moet u deze afzonderlijk verplaatsen.

## <a name="lb-limitations"></a> Load Balancer-beperkingen

Basic SKU Load Balancer kan worden verplaatst.
Standaard SKU Load Balancer kan niet worden verplaatst.

## <a name="pip-limitations"></a> Openbare IP-beperkingen

Basic SKU openbare IP-adres kan worden verplaatst.
Standaard SKU openbare IP-adres kan niet worden verplaatst.

## <a name="use-portal"></a>De portal gebruiken

Selecteer de resourcegroep met deze bronnen om resources te verplaatsen, en selecteer vervolgens de **verplaatsen** knop.

![Verplaatsen van resources](./media/resource-group-move-resources/select-move.png)

Selecteer of u de resources naar een nieuwe resourcegroep of een nieuw abonnement verplaatsen wilt.

Selecteer de resources te verplaatsen en de resourcegroep voor de bestemming. Bevestig dat u wilt bijwerken scripts voor deze bronnen, en selecteer **OK**. Als u het pictogram van het abonnement bewerken in de vorige stap hebt geselecteerd, moet u ook het doelabonnement.

![Selecteer bestemming](./media/resource-group-move-resources/select-destination.png)

In **meldingen**, ziet u dat de move-bewerking wordt uitgevoerd.

![verplaatsing status weergeven](./media/resource-group-move-resources/show-status.png)

Wanneer deze is voltooid, wordt u gewaarschuwd van het resultaat.

![resultaat van de verplaatsing weergeven](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>PowerShell gebruiken

U kunt bestaande resources verplaatsen naar een andere resourcegroep of abonnement met de [verplaatsen AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) opdracht. Het volgende voorbeeld laat zien hoe meerdere resources verplaatsen naar een nieuwe resourcegroep.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Als u wilt verplaatsen naar een nieuw abonnement, een waarde bevatten voor de `DestinationSubscriptionId` parameter.

## <a name="use-azure-cli"></a>Azure CLI gebruiken

U kunt bestaande resources verplaatsen naar een andere resourcegroep of abonnement met de [az resource verplaatsen](/cli/azure/resource?view=azure-cli-latest#az_resource_move) opdracht. Geef de bron-id's van de resources te verplaatsen. Het volgende voorbeeld laat zien hoe meerdere resources verplaatsen naar een nieuwe resourcegroep. In de `--ids` parameter, Geef een door spaties gescheiden lijst van de resource-id's te verplaatsen.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Als u wilt verplaatsen naar een nieuw abonnement, bieden de `--destination-subscription-id` parameter.

## <a name="use-rest-api"></a>REST API gebruiken

Bestaande om resources te verplaatsen naar een andere resourcegroep of abonnement, uitvoeren:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

In de aanvraagtekst geeft u de doelresourcegroep en de resources te verplaatsen. Zie voor meer informatie over de REST-verplaatsbewerking [verplaatsen van resources](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over PowerShell-cmdlets voor het beheren van uw abonnement, [Azure PowerShell gebruiken met Resource Manager](powershell-azure-resource-manager.md).
* Zie voor meer informatie over Azure CLI-opdrachten voor het beheren van uw abonnement, [met de Azure CLI met Resource Manager](xplat-cli-azure-resource-manager.md).
* Zie voor meer informatie over portal functies voor het beheren van uw abonnement, [om resources te beheren met behulp van de Azure-portal](resource-group-portal.md).
* Zie voor meer informatie over het toepassen van een logische organisatie op uw resources, [met labels om uw resources te organiseren](resource-group-using-tags.md).
