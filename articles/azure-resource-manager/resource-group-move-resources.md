---
title: Azure resources verplaatsen naar een nieuw abonnement of resourcegroep groep | Microsoft Docs
description: Azure Resource Manager gebruiken voor resources verplaatsen naar een nieuwe resourcegroep of abonnement.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/07/2018
ms.author: tomfitz
ms.openlocfilehash: e79419c764229e7dc52a32389b8b1116668dddfc
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039732"
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Resources verplaatsen naar een nieuwe resourcegroep of abonnement

In dit artikel wordt beschreven hoe u resources verplaatsen naar een nieuw abonnement of een nieuwe resourcegroep in hetzelfde abonnement. U kunt de portal, PowerShell, Azure CLI of de REST-API gebruiken om te verplaatsen van resources. De verplaatsingsbewerkingen in dit artikel zijn beschikbaar voor u zonder eventuele hulp van Azure-ondersteuning.

Bij het verplaatsen van resources, worden zowel de brongroep en de doelgroep vergrendeld tijdens de bewerking. Schrijf en verwijderbewerkingen op de brongroepen zijn geblokkeerd totdat de verplaatsing is voltooid. Deze vergrendeling betekent dat u kunt geen toevoegen, bijwerken of verwijderen van resources in de resourcegroepen, maar dit betekent niet dat de resources zijn geblokkeerd. Als u een SQL-Server en de bijbehorende database naar een nieuwe resourcegroep verplaatsen, er een toepassing die gebruikmaakt van de database zonder uitvaltijd. Dit kan nog steeds lezen en schrijven naar de database.

U kunt de locatie van de resource niet wijzigen. Verplaatsen van een resource alleen verplaatst naar een nieuwe resourcegroep. De nieuwe resourcegroep mogelijk een andere locatie, maar dat de locatie van de resource niet wijzigen.

> [!NOTE]
> Dit artikel wordt beschreven hoe u verplaatst resources binnen een bestaand Azure-account aanbod. Als u daadwerkelijk wijzigen van uw Azure-account (zoals het bijwerken van betalen per gebruik om te vooraf betalen) bieden wilt terwijl u verdergaat met het werken met uw bestaande bronnen, raadpleegt u [uw Azure-abonnement overstappen naar een andere aanbieding](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Controlelijst voor het verplaatsen van resources

Voordat u een resource verplaatst, moeten er enkele belangrijke stappen worden uitgevoerd. U kunt fouten voorkomen door te controleren of aan de volgende voorwaarden is voldaan.

1. De bron- en doelabonnementen moeten aanwezig zijn in dezelfde [Azure Active Directory-tenant](../active-directory/develop/quickstart-create-new-tenant.md). Om te controleren dat beide abonnementen dezelfde tenant-ID hebben, gebruikt u Azure PowerShell of Azure CLI.

  Voor Azure PowerShell gebruiken:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Gebruik voor Azure CLI:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Als de tenant-id's voor de bron- en doelabonnementen niet hetzelfde zijn, moet u de volgende methoden gebruiken om af te stemmen van de tenant-id's:

  * [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md)
  * [Een Azure-abonnement koppelen of toevoegen aan Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Het doelabonnement moet zijn geregistreerd voor de resourceprovider van de resource die wordt verplaatst. Als u niet het geval is, ontvangt u een foutmelding waarin staat dat de **abonnement is niet geregistreerd voor een resourcetype**. U kunt dit probleem tegenkomen bij het verplaatsen van een resource naar een nieuw abonnement, terwijl dat abonnement nooit is gebruikt met dat type resource.

  Gebruik de volgende opdrachten om op te halen van de registratiestatus voor PowerShell:

  ```powershell
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Gebruik het volgende voor het registreren van een resourceprovider:

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
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

1. Indien mogelijk is grote break verplaatst naar afzonderlijke verplaatsingsbewerkingen. Resource Manager mislukt onmiddellijk pogingen meer dan 800 om resources te verplaatsen in één bewerking. Verplaatsen van minder dan 800 resources kan echter ook mislukken door een time-out optreedt.

1. De service moet de mogelijkheid activeren om resources te verplaatsen. Om te bepalen of de verplaatsing slaagt, [valideren van uw aanvraag voor verplaatsen](#validate-move). Zie de secties hieronder in dit artikel die [services verplaatsing van resources activeren](#services-that-can-be-moved) en welke [services verplaatsing van resources niet activeren](#services-that-cannot-be-moved).

## <a name="when-to-call-support"></a>Wanneer u contact opnemen met ondersteuning

U kunt de meeste resources via de selfservice-bewerkingen die worden weergegeven in dit artikel kunt verplaatsen. Gebruik de bewerkingen selfservice zijn:

* Resource Manager-resources wilt verplaatsen.
* Verplaatsen van klassieke resources volgens de [klassieke Implementatiebeperkingen](#classic-deployment-limitations).

Neem contact op met [ondersteunen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) wanneer u moet:

* Resources wilt verplaatsen naar een nieuwe Azure-account (en Azure Active Directory-tenant) en u hulp nodig met de instructies in de voorgaande sectie.
* Klassieke resources wilt verplaatsen, maar problemen hebt met de beperkingen.

## <a name="validate-move"></a>Verplaatsen valideren

De [verplaatsingsbewerking valideren](/rest/api/resources/resources/validatemoveresources) kunt u het testen van uw scenario verplaatsen zonder daadwerkelijk de resources te verplaatsen. Deze bewerking gebruiken om te bepalen als de verplaatsing slaagt. Als u wilt deze bewerking uitvoeren, moet u de:

* naam van de resourcegroep
* Resource-ID van de doelresourcegroep
* Resource-ID van elke resource verplaatsen
* de [toegangstoken](/rest/api/azure/#acquire-an-access-token) voor uw account

De volgende aanvraag verzenden:

```
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2018-02-01
Authorization: Bearer <access-token>
Content-type: application/json
```

Met een aanvraagtekst:

```json
{
 "resources": ['<resource-id-1>', '<resource-id-2>'],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Als de aanvraag is ingedeeld, retourneert de bewerking:

```
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

```
GET <location-url>
Authorization: Bearer <access-token>
```

Terwijl de bewerking wordt nog steeds uitgevoerd, wordt u voor het ontvangen van de statuscode 202 blijven. Wacht het aantal seconden aangegeven in de `retry-after` waarde voordat u doorgaat. Als de verplaatsing is gevalideerd, ontvangt u de statuscode 204. Als de validatie voor het verplaatsen is mislukt, ontvangt u een foutbericht weergegeven, zoals:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="services-that-can-be-moved"></a>Services die kunnen worden verplaatst

De volgende lijst bevat een algemeen overzicht van Azure-services die kunnen worden verplaatst naar een nieuwe resourcegroep en een abonnement. Zie voor meer details, [bewerking ondersteuning voor resources verplaatsen](move-support-resources.md).

* Analysis Services
* API Management
* App Service-apps (web-apps) - Zie [beperkingen voor App Service](#app-service-limitations)
* App Service Certificates
* Application Insights
* Automation
* Azure Active Directory B2C
* Azure Cosmos DB
* Azure Database for MySQL
* Azure Database for PostgreSQL
* Azure DevOps - Azure DevOps-organisaties met niet-Microsoft-extensie aankopen moet [hun aankopen annuleren](https://go.microsoft.com/fwlink/?linkid=871160) voordat hij verder kan het account voor abonnementen.
* Azure Maps
* Azure Relay
* Azure Stack - registraties
* Batch
* BizTalk Services
* Bot-service
* CDN
* Cloud Services - Zie [klassieke Implementatiebeperkingen](#classic-deployment-limitations)
* Cognitive Services
* Container Registry
* Content Moderator
* Cost Management
* Customer Insights
* Data Catalog
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Event Grid
* Event Hubs
* HDInsight-clusters - Zie [HDInsight beperkingen](#hdinsight-limitations)
* IoT Central
* IoT Hubs
* Key Vault
* Load Balancers - Zie [beperkingen van de Load Balancer](#lb-limitations)
* Log Analytics
* Logic Apps
* Machine Learning - Machine Learning Studio-webservices kunnen worden verplaatst naar een resourcegroep in hetzelfde abonnement, maar niet een ander abonnement. Andere Machine Learning-resources kunnen worden uitgewisseld tussen abonnementen.
* Beheerde schijven - Zie [beperkingen van de virtuele Machines voor beperkingen](#virtual-machines-limitations)
* Beheerde identiteit - gebruiker toegewezen
* Media Services
* Mobile Engagement
* Notification Hubs
* Operational Insights
* Operations Management
* Portal-dashboards
* Power BI - zowel Power BI Embedded en Power BI-Werkruimteverzameling
* Openbaar IP-adres - Zie [openbaar IP-beperkingen](#pip-limitations)
* Redis-Cache - als de Redis-Cache-exemplaar is geconfigureerd met een virtueel netwerk, het exemplaar kan niet worden verplaatst naar een ander abonnement. Zie [beperkingen in virtuele netwerken](#virtual-networks-limitations).
* Scheduler
* Search
* Service Bus
* Service Fabric
* Service Fabric Mesh
* SignalR-Service
* Opslag - storage-accounts in verschillende regio's kan niet worden verplaatst in dezelfde bewerking. Gebruik in plaats daarvan de afzonderlijke bewerkingen voor elke regio.
* Opslag (klassiek) - Zie [klassieke Implementatiebeperkingen](#classic-deployment-limitations)
* Stream Analytics - status voor Stream Analytics taken bij het uitvoeren van in kunnen niet worden verplaatst.
* SQL Database-server - database en server moet zich in dezelfde resourcegroep bevinden. Wanneer u een SQL-server hebt verplaatst, worden ook alle bijbehorende databases verplaatst. Dit gedrag is van toepassing op Azure SQL Database en Azure SQL Data Warehouse-databases.
* Time Series Insights
* Traffic Manager
* Virtuele Machines - VM's met beheerde schijven kunnen niet worden verplaatst. Zie [beperkingen van de virtuele Machines](#virtual-machines-limitations)
* Virtuele Machines (klassiek) - Zie [klassieke Implementatiebeperkingen](#classic-deployment-limitations)
* Virtual Machine Scale Sets - Zie [beperkingen van de virtuele Machines](#virtual-machines-limitations)
* Virtuele netwerken - Zie [beperkingen in virtuele netwerken](#virtual-networks-limitations)
* VPN Gateway

## <a name="services-that-cannot-be-moved"></a>Services die kunnen niet worden verplaatst

De volgende lijst bevat een algemeen overzicht van Azure-services die niet worden verplaatst naar een nieuwe resourcegroep en een abonnement. Zie voor meer details, [bewerking ondersteuning voor resources verplaatsen](move-support-resources.md).

* AD Domain Services
* Hybride AD Health-Service
* Application Gateway
* Azure Database Migration
* Azure Databricks
* Azure Migrate
* Batch AI
* Certificaten - App Service-certificaten kunnen worden verplaatst, maar de geüploade certificaten hebben [beperkingen](#app-service-limitations).
* Container Instances
* Container Service
* Data Box
* Dev-opslagruimten
* Dynamics LCS
* ExpressRoute
* Kubernetes Service
* Lab-Services - verplaatsen naar de nieuwe resourcegroep in hetzelfde abonnement is ingeschakeld, maar de verplaatsing van kruislings abonnement is niet ingeschakeld.
* Load Balancers - Zie [beperkingen van de Load Balancer](#lb-limitations)
* Managed Applications
* Microsoft Genomics
* NetApp
* Openbaar IP-adres - Zie [openbaar IP-beperkingen](#pip-limitations)
* Recovery Services-kluis - ook niet de Reken-, netwerk- en resources die zijn gekoppeld aan de Recovery Services-kluis wilt verplaatsen, Zie [beperkingen voor Recovery Services](#recovery-services-limitations).
* SAP HANA op Azure
* Beveiliging
* Site Recovery
* StorSimple Device Manager
* Virtuele netwerken (klassiek) - Zie [klassieke Implementatiebeperkingen](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Beperkingen van de virtuele Machines

Beheerde schijven worden ondersteund voor de verplaatsing vanaf 24 September 2018. U zult moeten zich registreren voor deze functie inschakelen

#### <a name="powershell"></a>PowerShell
`Register-AzureRmProviderFeature -FeatureName ManagedResourcesMove -ProviderNamespace Microsoft.Compute`
#### <a name="cli"></a>CLI
`az feature register Microsoft.Compute ManagedResourcesMove`


Dit betekent dat u kunt ook verplaatsen:

* Virtuele machines met beheerde schijven
* Beheerde installatiekopieën
* Beheerde momentopnamen
* Beschikbaarheidssets met virtuele machines met beheerde schijven

Hier volgen de beperkingen die nog niet ondersteund

* Virtuele Machines met een certificaat dat is opgeslagen in Key Vault kan worden verplaatst naar een nieuwe resourcegroep in hetzelfde abonnement, maar niet tussen meerdere abonnementen.
* Virtuele Machines met Azure Backup zijn geconfigureerd. Gebruik de volgende tijdelijke oplossing voor deze virtuele Machines verplaatsen
  * Ga naar de locatie van uw virtuele Machine.
  * Zoek een resourcegroep met de volgende naamgevingspatroon: "AzureBackupRG_<location of your VM>_1 ' bijvoorbeeld AzureBackupRG_westus2_1
  * Als in Azure Portal, klikt u vervolgens selectievakje ' verborgen typen weergeven"
  * Als in PowerShell, gebruikt u de `Get-AzureRmResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet
  * Als u in de CLI, gebruikt u de `az resource list -g AzureBackupRG_<location of your VM>_1`
  * Zoek nu de resource met type `Microsoft.Compute/restorePointCollections` waarvoor het naamgevingspatroon `AzureBackup_<name of your VM that you're trying to move>_###########`
  * Deze resource verwijderen
  * Nadat het verwijderen is voltooid, kunt u zich aan uw virtuele Machine verplaatsen
* Virtual Machine Scale Sets met standaard SKU Load Balancer of een standaard SKU en openbare IP kan niet worden verplaatst.
* Virtuele machines die zijn gemaakt op basis van Marketplace-resources met een abonnement dat is gekoppeld kan niet worden verplaatst tussen resourcegroepen of abonnementen. Inrichting ongedaan maken van de virtuele machine in het huidige abonnement en opnieuw implementeren in het nieuwe abonnement.


## <a name="virtual-networks-limitations"></a>Beperkingen voor virtuele netwerken

Bij het verplaatsen van een virtueel netwerk, moet u ook de afhankelijke resources verplaatsen. Voor VPN-Gateways, moet u IP-adressen, virtuele netwerkgateways en alle bijbehorende verbindingsresources verplaatsen. Lokale netwerkgateways kunnen zich in een andere resourcegroep.

Voor het verplaatsen van een gekoppeld virtueel netwerk, moet u eerst de virtueel-netwerkpeering uitschakelen. Als uitgeschakeld, kunt u het virtuele netwerk kunt verplaatsen. Na de verplaatsing opnieuw de peering op virtueel netwerk.

U kunt een virtueel netwerk niet verplaatsen naar een ander abonnement, als het virtuele netwerk een subnet met resourcenavigatiekoppelingen bevat. Bijvoorbeeld, als een Redis-Cache-bron wordt geïmplementeerd in een subnet, heeft dat subnet een resourcenavigatiekoppeling.

## <a name="app-service-limitations"></a>Beperkingen voor App Service

De beperkingen voor het verplaatsen van resources van App Service zijn afhankelijk van of u de resources binnen een abonnement of naar een nieuw abonnement verplaatst.

De beperkingen die worden beschreven in deze secties van toepassing op geüploade certificaten, geen App Service-certificaten. U kunt App Service-certificaten verplaatsen naar een nieuwe resourcegroep of abonnement zonder beperkingen. Als u meerdere WebApps die gebruikmaken van dezelfde App Service Certificate heeft, verplaatst u eerst alle WebApps, klikt u vervolgens het certificaat te verplaatsen.

### <a name="moving-within-the-same-subscription"></a>Verplaatsen binnen hetzelfde abonnement

Wanneer u een Web-App verplaatst _binnen hetzelfde abonnement_, u kunt de geüploade SSL-certificaten niet verplaatsen. Echter kunt u een Web-App naar de nieuwe resourcegroep verplaatsen zonder te verplaatsen van de geüpload SSL-certificaat en de SSL-functionaliteit van uw app werkt nog altijd.

Als u verplaatsen van het SSL-certificaat met de Web-App wilt, volgt u deze stappen:

1.  Het geüploade certificaat verwijderen uit de Web-App.
2.  Verplaats de Web-App.
3.  Upload het certificaat naar de verplaatste Web-App.

### <a name="moving-across-subscriptions"></a>Verplaatsen tussen abonnementen

Wanneer u een Web-App verplaatst _voor abonnementen_, gelden de volgende beperkingen:

- De doelresourcegroep mag geen bestaande App Service-resources. App Service-resources zijn onder andere:
    - Web Apps
    - App Service-abonnementen
    - Geüpload of geïmporteerd SSL-certificaten
    - App Service-omgevingen
- Alle App Service-resources in de resourcegroep, moeten tegelijk worden verplaatst.
- App Service-resources kunnen alleen worden verplaatst uit de resourcegroep waarin ze oorspronkelijk zijn gemaakt. Als een resource App Service niet langer in de oorspronkelijke resourcegroep is, het moet worden teruggezet naar die oorspronkelijke resourcegroep eerst en vervolgens kunnen worden verplaatst tussen abonnementen.

## <a name="classic-deployment-limitations"></a>Klassieke Implementatiebeperkingen

De opties voor het verplaatsen van resources die zijn geïmplementeerd via het klassieke model verschillen afhankelijk van of u de resources binnen een abonnement of naar een nieuw abonnement verplaatst.

### <a name="same-subscription"></a>Hetzelfde abonnement

Bij het verplaatsen van resources van een resourcegroep naar een andere resourcegroep binnen hetzelfde abonnement, gelden de volgende beperkingen:

* Virtuele netwerken (klassiek) kunnen niet worden verplaatst.
* Virtuele machines (klassiek) moeten worden verplaatst met de cloudservice.
* Cloudservice kan alleen worden verplaatst wanneer de verplaatsing alle virtuele machines bevat.
* Slechts één cloudservice kan tegelijk worden verplaatst.
* Slechts één opslagaccount (klassiek) kan tegelijk worden verplaatst.
* Opslagaccount (klassiek) kan niet worden verplaatst in dezelfde bewerking met een virtuele machine of een service in de cloud.

Voor klassieke resources verplaatsen naar een nieuwe resourcegroep binnen hetzelfde abonnement, gebruikt u de verplaatsingsbewerkingen voor standard via de [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), of [REST-API](#use-rest-api). U dezelfde bewerkingen als die u gebruikt voor het verplaatsen van Resource Manager-resources.

### <a name="new-subscription"></a>Nieuw abonnement

Wanneer u resources verplaatst naar een nieuw abonnement, gelden de volgende beperkingen:

* Alle klassieke resources in het abonnement moeten worden verplaatst in dezelfde bewerking.
* Het doelabonnement mag geen andere klassieke resources bevatten.
* De overstap kan alleen worden aangevraagd door een afzonderlijke REST-API voor klassieke verplaatst. De standaardopdrachten verplaatsen van Resource Manager werken niet bij het klassieke resources verplaatsen naar een nieuw abonnement.

Om klassieke resources verplaatsen naar een nieuw abonnement, de REST-bewerkingen die specifiek voor klassieke resources zijn te gebruiken. Voor het gebruik van REST, moet u de volgende stappen uitvoeren:

1. Controleer als van het bronabonnement deel uitmaken van een verplaatsing van kruislings abonnement. Gebruik de volgende bewerking:

  ```HTTP
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     In de hoofdtekst van de aanvraag, zijn onder andere:

  ```json
  {
    "role": "source"
  }
  ```

     Het antwoord voor de validatiebewerking is in de volgende indeling:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Controleer als het doelabonnement deel uitmaken van een verplaatsing van kruislings abonnement. Gebruik de volgende bewerking:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     In de hoofdtekst van de aanvraag, zijn onder andere:

  ```json
  {
    "role": "target"
  }
  ```

     Het antwoord is in dezelfde indeling als de validatie van het abonnement.
3. Als beide abonnementen zijn gevalideerd, alle klassieke resources wilt verplaatsen van één abonnement naar een ander abonnement met de volgende bewerking:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    In de hoofdtekst van de aanvraag, zijn onder andere:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

De bewerking kan enkele minuten uitgevoerd.

## <a name="recovery-services-limitations"></a>Recovery Services-beperkingen

Verplaatsing is niet ingeschakeld voor opslag, netwerk- of Compute-resources die worden gebruikt voor het instellen van herstel na noodgeval met Azure Site Recovery.

Stel bijvoorbeeld dat u replicatie van uw on-premises machines naar een opslagaccount (Storage1) hebt ingesteld en wilt dat de beveiligde machine zijn beschikbaar na een failover naar Azure als een virtuele machine (VM1) die is gekoppeld aan een virtueel netwerk (Network1). U kunt een van deze Azure-resources - Storage1, VM1 en Network1 - niet verplaatsen tussen resourcegroepen binnen hetzelfde abonnement of tussen abonnementen.

Verplaatsen van een virtuele machine geregistreerd bij **Azure backup** tussen resourcegroepen:
 1. Tijdelijk back-up stoppen en back-upgegevens behouden
 2. De virtuele machine verplaatsen naar de doelresourcegroep
 3. Beveilig deze onder de dezelfde/nieuwe kluis die gebruikers vanuit de beschikbare herstelpunten die zijn gemaakt vóór de verplaatsing herstellen kunnen opnieuw.
Als de gebruiker wordt verplaatst van de VM waarvan een back-up is gemaakt in abonnementen, stap 1 en stap 2 blijven hetzelfde. Gebruiker moet de virtuele machine onder een nieuwe kluis aanwezig / worden gemaakt in het doelabonnement beveiligen in stap 3. Recovery Services-kluis biedt geen ondersteuning voor back-ups van kruislings abonnement.

## <a name="hdinsight-limitations"></a>HDInsight-beperkingen

U kunt de HDInsight-clusters verplaatsen naar een nieuw abonnement of resourcegroep. U kunt echter niet verplaatsen tussen abonnementen de netwerkresources die worden gekoppeld aan het HDInsight-cluster (zoals het virtuele netwerk, een NIC of een load balancer). Bovendien verplaatsen u niet naar een nieuwe resourcegroep een NIC die is gekoppeld aan een virtuele machine voor het cluster.

Bij het verplaatsen van een HDInsight-cluster naar een nieuw abonnement, verplaatst u eerst andere bronnen (zoals de storage-account). Verplaats het HDInsight-cluster op zichzelf.

## <a name="search-limitations"></a>Beperkingen voor zoeken

U kunt meerdere resources voor zoeken in verschillende regio's in één keer geplaatst niet verplaatsen.
In dat geval moet u ze afzonderlijk verplaatsen.

## <a name="lb-limitations"></a> Load Balancer-beperkingen

Basis SKU Load Balancer kan worden verplaatst.
Standaard SKU Load Balancer kan niet worden verplaatst.

## <a name="pip-limitations"></a> Openbare IP-beperkingen

Basic SKU en openbare IP kunnen worden verplaatst.
Standaard SKU en openbare IP kan niet worden verplaatst.

## <a name="use-portal"></a>De portal gebruiken

Selecteer de resourcegroep die met deze bronnen voor het verplaatsen van resources, en selecteer vervolgens de **verplaatsen** knop.

![resources verplaatsen](./media/resource-group-move-resources/select-move.png)

Selecteer of u de resources naar een nieuwe resourcegroep of een nieuw abonnement verplaatsen wilt.

Selecteer de resources wilt verplaatsen en de resourcegroep van de bestemming. Bevestig dat u wilt bijwerken scripts voor deze resources, en selecteer **OK**. Als u het pictogram bewerken abonnement in de vorige stap hebt geselecteerd, moet u ook het doelabonnement.

![doel selecteren](./media/resource-group-move-resources/select-destination.png)

In **meldingen**, ziet u dat de verplaatsing wordt uitgevoerd.

![move-status weergeven](./media/resource-group-move-resources/show-status.png)

Wanneer deze is voltooid, krijgt u een melding van het resultaat.

![Zet resultaten weergeven](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>PowerShell gebruiken

Als u bestaande resources naar een andere resourcegroep of abonnement, gebruikt u de [Move-AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) opdracht. Het volgende voorbeeld ziet hoe u meerdere resources verplaatsen naar een nieuwe resourcegroep.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Als u wilt verplaatsen naar een nieuw abonnement, een waarde bevatten voor de `DestinationSubscriptionId` parameter.

## <a name="use-azure-cli"></a>Azure CLI gebruiken

Als u bestaande resources naar een andere resourcegroep of abonnement, gebruikt u de [verplaatsen van de az resource](/cli/azure/resource?view=azure-cli-latest#az-resource-move) opdracht. Geef de resource-id's van de resources te verplaatsen. Het volgende voorbeeld ziet hoe u meerdere resources verplaatsen naar een nieuwe resourcegroep. In de `--ids` parameter, Geef een door spaties gescheiden lijst van de resource-id's te verplaatsen.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Als u wilt verplaatsen naar een nieuw abonnement, bieden de `--destination-subscription-id` parameter.

## <a name="use-rest-api"></a>REST API gebruiken

Om bestaande resources naar een andere resourcegroep of abonnement verplaatsen, voert u de volgende uit:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

In de hoofdtekst van de aanvraag geeft u de doelresourcegroep die is en de resources te verplaatsen. Zie voor meer informatie over de REST-bewerking gaan [resources verplaatsen](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over PowerShell-cmdlets voor het beheer van uw abonnement, [met behulp van Azure PowerShell met Resource Manager](powershell-azure-resource-manager.md).
* Zie voor meer informatie over Azure CLI-opdrachten voor het beheren van uw abonnement, [met de Azure CLI met Resource Manager](xplat-cli-azure-resource-manager.md).
* Zie voor meer informatie over portal functies voor het beheren van uw abonnement, [om resources te beheren met behulp van de Azure-portal](resource-group-portal.md).
* Zie voor meer informatie over het toepassen van een logische organisatie aan uw resources, [tags gebruiken om uw resources te organiseren](resource-group-using-tags.md).
