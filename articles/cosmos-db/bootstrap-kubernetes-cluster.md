---
title: Het gebruik van Azure Kubernetes met Azure Cosmos DB
description: Meer informatie over het bootstrap-een Kubernetes-cluster in Azure die gebruikmaakt van Azure Cosmos DB (preview)
author: SnehaGunda
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/06/2019
ms.author: sngun
ms.openlocfilehash: 7d60e71fa0768b46042c31f9d79dd8ca6a41b2cc
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65080854"
---
# <a name="how-to-use-azure-kubernetes-with-azure-cosmos-db-preview"></a>Het gebruik van Azure Kubernetes met Azure Cosmos DB (preview)

De etcd API in Azure Cosmos DB kunt u Azure Cosmos DB gebruiken als het archief met back-end voor Azure Kubernetes. Azure Cosmos DB implementeert de etcd wire-protocol, waardoor het hoofdknooppunt API servers voor het gebruik van Azure Cosmos DB, net als het een lokaal geïnstalleerde etcd zou openen. etcd API in Azure Cosmos DB is momenteel in preview. Wanneer u Azure Cosmos etcd API als het archief met back-ups voor Kubernetes gebruikt, krijgt u de volgende voordelen: 

* U hoeft handmatig configureren en beheren van etcd.
* Hoge beschikbaarheid van etcd, gegarandeerd door Cosmos (99,99% in één regio, 99,999% in meerdere regio's).
* Flexibele schaalbaarheid van etcd.
* Beveilig standaard & geschikt voor bedrijven.
* Toonaangevende, uitgebreide Sla's.

Zie voor meer informatie over etcd API in Azure Cosmos DB, de [overzicht](etcd-api-introduction.md) artikel. In dit artikel leest u hoe u [Azure Kubernetes-Engine](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md) (aks-engine) als u een Kubernetes-cluster in Azure die gebruikmaakt van opstarten [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) in plaats van een lokaal geïnstalleerde en geconfigureerde etcd. 

## <a name="prerequisites"></a>Vereisten

1. Installeer de nieuwste versie van [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). U kunt Azure CLI specifiek voor uw besturingssysteem downloaden en installeren.

1. Installeer de [v0.32.3](https://github.com/Azure/aks-engine/releases/tag/v0.32.3) versie van Azure Kubernetes-Engine. De installatie-instructies voor verschillende besturingssystemen zijn beschikbaar in [Azure Kubernetes-Engine](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md#install-aks-engine) pagina. U hoeft alleen maar de stappen bij **AKS-Engine installeren** sectie van de gekoppelde doc-bestand. Nadat u hebt gedownload, pak het zip-bestand.

   De Azure Kubernetes-Engine (**aks-engine**) genereert Azure Resource Manager-sjablonen voor Kubernetes-clusters op Azure. De invoer voor de aks-engine is een cluster-definitiebestand die het gewenste cluster beschrijft, met inbegrip van orchestrator, functies en -agents. De structuur van de invoerbestanden is vergelijkbaar met de openbare API voor Azure Kubernetes Service.

1. De etcd API in Azure Cosmos DB is momenteel in preview. Aanmelden voor de preview-versie op: https://aka.ms/cosmosetcdapi-signup. Nadat u het formulier hebt verzonden, worden uw abonnement in de whitelist opgenomen gebruik van de Azure Cosmos-etcd API. 

## <a name="deploy-the-cluster-with-azure-cosmos-db"></a>Implementeren van het cluster met Azure Cosmos DB

1. Open een opdrachtpromptvenster en meld u aan bij Azure met de volgende opdracht:

   ```azurecli-interactive
   az login 
   ```

1. Als u meer dan één abonnement hebt, kunt u overschakelen naar het abonnement dat opgenomen in de whitelist voor Azure Cosmos DB etcd API is. U kunt overschakelen naar het vereiste abonnement met de volgende opdracht:

   ```azurecli-interactive
   az account set --subscription "<Name of your subscription>"
   ```
1. Maak vervolgens een nieuwe resourcegroep waar u de resources die vereist zijn de Azure Kubernetes-cluster implementeren. Zorg ervoor dat u de resourcegroep in de regio 'centralus' maken. Het is niet verplicht voor de resourcegroep zich echter in 'centralus' regio, Azure Cosmos etcd API is momenteel beschikbaar voor het implementeren in de regio 'centralus'. Het is dus raadzaam te hebben van het Kubernetes-cluster moet dezelfde locatie bevindt als het exemplaar van de etcd Cosmos:

   ```azurecli-interactive
   az group create --name <Name> --location "centralus"
   ```

1. Naast een service-principal maken voor de Azure Kubernetes-cluster zo dat deze kan communiceren met de resources die deel van dezelfde resourcegroep bevinden uitmaken. U kunt een service-principal met behulp van Azure CLI, PowerShell of Azure-portal, in dit voorbeeld wordt u u CLI om deze te maken.

   ```azurecli-interactive
   az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<Your_Azure_subscription_ID>/resourceGroups/<Your_resource_group_name>"
   ```
   Deze opdracht levert de details van een service-principal, bijvoorbeeld:
   
   ```cmd
   Retrying role assignment creation: 1/36
   {
     "appId": "8415a4e9-4f83-46ca-a704-107457b2e3ab",
     "displayName": "azure-cli-2019-04-19-19-01-46",
     "name": "http://azure-cli-2019-04-19-19-01-46",
     "password": "102aecd3-5e37-4f3d-8738-2ac348c2e6a7",
     "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
   }
   ```
   
   Noteer de **appId** en de **wachtwoord** velden, omdat u deze parameters in de volgende stappen. 

1. Navigeer vanaf de opdrachtprompt naar de map waar het Azure Kubernetes-Engine uitvoerbare bestand zich bevindt. Bijvoorbeeld, kunt u op de opdrachtprompt ook navigeren naar de map als:

   ```cmd
   cd "\aks-engine-v0.32.3-windows-amd64\aks-engine-v0.32.3-windows-amd64"
   ```

1. Open een teksteditor van uw keuze en definiëren van een Resource Manager-sjabloon die de Azure Kubernetes-cluster met Azure Cosmos DB etcd API implementeert. De volgende JSON-definitie kopiëren naar een teksteditor en sla het bestand als `apiModel.json`:

   ```json

   {
    "apiVersion": "vlabs",
    "properties": {
        "orchestratorProfile": {
            "orchestratorType": "Kubernetes",
            "kubernetesConfig": {
                "useManagedIdentity": false
            }
        },
        "masterProfile": {
            "count": 1,
            "dnsPrefix": "",
            "vmSize": "Standard_D2_v3",
            "cosmosEtcd": true
        },
        "agentPoolProfiles": [
            {
                "name": "agent",
                "count": 1,
                "vmSize": "Standard_D2_v3",
                "availabilityProfile": "AvailabilitySet"
            }
        ],
        "linuxProfile": {
            "adminUsername": "azureuser",
            "ssh": {
                "publicKeys": [
                    {
                        "keyData": ""
                    }
                ]
            }
        }
    }
   }
   ```

   In het definitiebestand JSON/cluster de sleutelparameter om te weten is **"cosmosEtcd": true**. Deze parameter is in de eigenschappen van "masterProfile" en wordt aangegeven dat de implementatie voor het gebruik van Azure Cosmos etcd API in plaats van reguliere etcd. 

1. Implementeer de Azure Kubernetes-cluster dat gebruik maakt van Azure Cosmos DB met de volgende opdracht:

   ```cmd
   aks-engine deploy \
     --subscription-id <Your_Azure_subscription_ID> \
     --client-id <Service_ principal_appId> \
     --client-secret <Service_ principal_password> \
     --dns-prefix <Region_unique_dns_name > \
     --location centralus \
     --api-model <Fully_qualified_path_to_the_template_file>  \
     --force-overwrite
   ```

   Azure Kubernetes-Engine verbruikt de clusterdefinitie van een waarin de gewenste vorm, de grootte en de configuratie van de Kubernetes Azure. Er zijn verschillende functies die kunnen worden ingeschakeld via de clusterdefinitie van de. In dit voorbeeld gebruikt u de volgende parameters:

   * **abonnement-id:** Azure-abonnement-ID met Azure Cosmos DB etcd API is ingeschakeld.
   * **client-id:** De service-principal appId. De `appId` is geretourneerd als uitvoer in stap 4.
   * **Client-secret:** Wachtwoord van de service-principal of een willekeurig gegenereerd wachtwoord. Deze waarde is geretourneerd als uitvoer in de parameter 'password' in stap 4. 
   * **dnsPrefix:** Een regio-unieke DNS-naam. Deze waarde wordt deel uitmaken van de hostnaam (voorbeeld van de waarden zijn-myprod1, fasering).
   * **Locatie:**  Locatie waar het cluster moet geïmplementeerd in, momenteel wordt alleen 'centralus' wordt ondersteund.

   > [!Note]
   > Etcd-API van Azure Cosmos is momenteel beschikbaar voor implementatie in de regio 'centralus'. 
 
   * **api-model:** Volledig gekwalificeerde pad naar het sjabloonbestand.
   * **Force overschrijven:** Deze optie wordt gebruikt om de bestaande bestanden in de uitvoermap automatisch te overschrijven.
 
   De volgende opdracht toont een voorbeeld van een implementatie:

   ```cmd
   aks-engine deploy \
     --subscription-id 1234fc61-1234-1234-1234-be1234d12c1b \
     --client-id 1234a4e9-4f83-46ca-a704-107457b2e3ab \
     --client-secret 123aecd3-5e37-4f3d-8738-2ac348c2e6a7 \
     --dns-prefix aks-sg-test \
     --location centralus \
     --api-model "C:\Users\demouser\Downloads\apiModel.json"  \
     --force-overwrite
   ```

## <a name="verify-the-deployment"></a>De implementatie controleren

De sjabloonimplementatie duurt enkele minuten om te voltooien. Nadat de implementatie is voltooid, ziet u de volgende uitvoer in de opdrachtprompt:

```cmd
WARN[0006] apimodel: missing masterProfile.dnsPrefix will use "aks-sg-test"
WARN[0006] --resource-group was not specified. Using the DNS prefix from the apimodel as the resource group name: aks-sg-test
INFO[0025] Starting ARM Deployment (aks-sg-test-546247491). This will take some time...
INFO[0587] Finished ARM Deployment (aks-sg-test-546247491). Succeeded
```

Nu bevat resources zoals de resourcegroep - virtuele machine, Azure Cosmos-account (etcd API), virtueel netwerk, en beschikbaarheidsset, en andere resources die zijn vereist voor het Kubernetes-cluster. 

Naam van de Azure Cosmos-account komt overeen met de opgegeven DNS-voorvoegsel dat wordt toegevoegd met k8s. Uw Azure Cosmos-account automatisch worden ingericht met een database met de naam **EtcdDB** en een container met de naam **EtcdData**. De container slaat alle etcd gerelateerde gegevens. De container is ingericht met een bepaald aantal aanvraageenheden en kunt u [schaal (vergroten/verkleinen) de doorvoer](scaling-throughput.md) op basis van uw werkbelasting. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [werken met Azure Cosmos-database, containers en objecten](databases-containers-items.md)
* Meer informatie over het [ingerichte doorvoer kosten optimaliseren](optimize-cost-throughput.md)

