---
title: Azure HDInsight-Go-SDK
description: Naslaginformatie voor Azure HDInsight Go SDK
services: hdinsight
author: tylerfox
ms.service: hdinsight
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: tyfox
ms.custom: seodec18
ms.openlocfilehash: ac3296937c02c3021c49dafc9a6ed7c249fe37af
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57769315"
---
# <a name="hdinsight-go-management-sdk-preview"></a>Ga naar HDInsight Management SDK-Preview

## <a name="overview"></a>Overzicht
De Go SDK voor HDInsight biedt klassen en -functies waarmee u kunt voor het beheren van uw HDInsight-clusters. Dit omvat bewerkingen als u wilt maken, verwijderen, bijwerken, weergeven, vergroten of verkleinen, scriptacties uitvoeren, controleren en ophalen van de eigenschappen van HDInsight-clusters, en meer.

> [!NOTE]  
>Referentiemateriaal voor GoDoc voor deze SDK is ook [beschikbaar hier](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight).

## <a name="prerequisites"></a>Vereisten

* Een Azure-account. Als u nog geen hebt, [een gratis proefversie](https://azure.microsoft.com/free/).
* [Ga](https://golang.org/dl/).

## <a name="sdk-installation"></a>SDK-installatie

Uitvoeren vanaf de locatie van uw GOPATH, `go get github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight`

## <a name="authentication"></a>Authentication

De SDK moet eerst worden geverifieerd bij uw Azure-abonnement.  Ga als volgt het voorbeeld hieronder een service-principal maken en deze gebruiken om te verifiëren. Nadat dit is gebeurd, hebt u een exemplaar van een `ClustersClient`, die bevat veel functies (zoals beschreven onderstaande secties) die kunnen worden gebruikt voor het uitvoeren van beheerbewerkingen.

> [!NOTE]  
> Er zijn andere manieren om te verifiëren naast het onderstaande voorbeeld dat mogelijk beter geschikt voor uw behoeften zijn kan. Alle functies worden hier beschreven: [Verificatiefuncties in de Azure SDK voor Go](https://docs.microsoft.com/go/azure/azure-sdk-go-authorization)

### <a name="authentication-example-using-a-service-principal"></a>Voorbeeld van verificatie met behulp van een service-principal

Eerste, meld u aan bij [Azure Cloud Shell](https://shell.azure.com/bash). Controleer of dat u het abonnement waarin u wilt dat de service-principal gemaakt momenteel gebruikt. 

```azurecli-interactive
az account show
```

Gegevens van uw abonnement wordt weergegeven als JSON.

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

Als u bent niet aangemeld bij het juiste abonnement, selecteert u het juiste abonnement door te voeren: 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]  
> Als u nog niet hebt geregistreerd de HDInsight-Resourceprovider door een andere functie (zoals door het maken van een HDInsight-Cluster via de Azure Portal), moet u dit doen zodra voordat u kunt verifiëren. Dit kan worden gedaan vanuit de [Azure Cloud Shell](https://shell.azure.com/bash) door het uitvoeren van de volgende opdracht uit:
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

Vervolgens kiest u een naam op voor uw service-principal en deze te maken met de volgende opdracht:

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

De gegevens voor de service-principal wordt weergegeven als JSON.

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
Kopieer het onderstaande codefragment en vul `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, en `SUBSCRIPTION_ID` met de tekenreeksen uit de JSON die is geretourneerd na het uitvoeren van de opdracht om de serviceprincipal te maken.

```golang
package main

import (
    "context"
    "github.com/Azure/go-autorest/autorest/azure/auth"
    hdi "github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight"
    "github.com/Azure/go-autorest/autorest/to"
)

func main() {
    var err error

    // Tenant ID for your Azure Subscription
    var TENANT_ID = ""
    // Your Service Principal App Client ID
    var CLIENT_ID = ""
    // Your Service Principal Client Secret
    var CLIENT_SECRET = ""
    // Azure Subscription ID
    var SUBSCRIPTION_ID = ""

    var credentials = auth.NewClientCredentialsConfig(CLIENT_ID, CLIENT_SECRET, TENANT_ID)
    var client = hdi.NewClustersClient(SUBSCRIPTION_ID)

    client.Authorizer, err = credentials.Authorizer()
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
```

## <a name="cluster-management"></a>Clusterbeheer

> [!NOTE]  
> In deze sectie wordt ervan uitgegaan dat u al hebt geverifieerd en gebouwd een `ClusterClient` exemplaar en op te slaan in een variabele met de naam `client`. Instructies voor het verifiëren en verkrijgen van een `ClusterClient` vindt u in de bovenstaande sectie voor verificatie.

### <a name="create-a-cluster"></a>Een cluster maken

Een nieuw cluster kan worden gemaakt door het aanroepen van `client.Create()`. 

#### <a name="example"></a>Voorbeeld

In dit voorbeeld laat zien over het maken van een [Apache Spark](https://spark.apache.org/) cluster met 2 hoofdknooppunten en 1 worker-knooppunt.

> [!NOTE]  
> U moet eerst een resourcegroep en de Storage-Account, zoals hieronder wordt uitgelegd. Als u deze al hebt gemaakt, kunt u deze stappen overslaan.

##### <a name="creating-a-resource-group"></a>Het maken van een resourcegroep

U kunt maken met een resource-groep met de [Azure Cloud Shell](https://shell.azure.com/bash) door uit te voeren
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>Een opslagaccount maken

U kunt maken met een storage-account met de [Azure Cloud Shell](https://shell.azure.com/bash) door uit te voeren:
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
Nu uitvoeren op de volgende opdracht uit om op te halen van de sleutel voor uw storage-account (u moet dit een cluster te maken):
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
De hieronder gaan fragment maakt u een Spark-cluster met 2 hoofdknooppunten en 1 worker-knooppunt. Vul de lege variabelen, zoals wordt beschreven in de opmerkingen en gerust om andere parameters aan de behoeften van uw specifieke behoeften te wijzigen.

```golang
// The name for the cluster you are creating
var clusterName = "";
// The name of your existing Resource Group
var resourceGroupName = "";
// Choose a username
var username = "";
// Choose a password
var password = "";
// Replace <> with the name of your storage account
var storageAccount = "<>.blob.core.windows.net";
// Storage account key you obtained above
var storageAccountKey = "";
// Choose a region
var location = "";
var container = "default";

var parameters = hdi.ClusterCreateParametersExtended {
    Location: to.StringPtr(location),
    Tags: make(map[string]*string),
    Properties: &hdi.ClusterCreateProperties {
        ClusterVersion: to.StringPtr("3.6"),
        OsType: hdi.Linux,
        ClusterDefinition: &hdi.ClusterDefinition {
            Kind: to.StringPtr("spark"),
            Configurations: map[string]map[string]interface{}{
                "gateway": {
                    "restAuthCredential.isEnabled": "True",
                    "restAuthCredential.username":  username,
                    "restAuthCredential.password":  password,
                },
            },
        },
        Tier: hdi.Standard,
        ComputeProfile: &hdi.ComputeProfile {
            Roles: &[]hdi.Role {
                hdi.Role {
                    Name: to.StringPtr("headnode"),
                    TargetInstanceCount: to.Int32Ptr(2),
                    HardwareProfile: &hdi.HardwareProfile {
                        VMSize: to.StringPtr("Large"),
                    },
                    OsProfile: &hdi.OsProfile {
                        LinuxOperatingSystemProfile: &hdi.LinuxOperatingSystemProfile {
                            Username: to.StringPtr(username),
                            Password: to.StringPtr(password),
                        },
                    },
                },
                hdi.Role {
                    Name: to.StringPtr("workernode"),
                    TargetInstanceCount: to.Int32Ptr(1),
                    HardwareProfile: &hdi.HardwareProfile {
                        VMSize: to.StringPtr("Large"),
                    },
                    OsProfile: &hdi.OsProfile {
                        LinuxOperatingSystemProfile: &hdi.LinuxOperatingSystemProfile {
                            Username: to.StringPtr(username),
                            Password: to.StringPtr(password),
                        },
                    },
                },
            },
        },
        StorageProfile: &hdi.StorageProfile {
            Storageaccounts: &[]hdi.StorageAccount {
                hdi.StorageAccount {
                    Name: to.StringPtr(storageAccount),
                    Key: to.StringPtr(storageAccountKey),
                    Container: to.StringPtr(container),
                    IsDefault: to.BoolPtr(true),
                },
            },
        },
    },
}
client.Create(context.Background(), resourceGroupName, clusterName, parameters)
```

### <a name="get-cluster-details"></a>Clusterdetails van het ophalen

Eigenschappen voor een bepaald cluster ophalen:

```golang
client.Get(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Voorbeeld

U kunt `get` om te bevestigen dat u uw cluster hebt gemaakt.

```golang
cluster, err := client.Get(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
fmt.Println(*cluster.Name)
fmt.Println(*cluster.ID
```

De uitvoer moet eruitzien als:

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a>Clusters groeperen

#### <a name="list-clusters-under-the-subscription"></a>Clusters groeperen onder het abonnement
```golang
client.List()
```
#### <a name="list-clusters-by-resource-group"></a>Clusters groeperen op resourcegroep
```golang
client.ListByResourceGroup("<Resource Group Name>")
```

> [!NOTE]  
> Beide `List()` en `ListByResourceGroup()` retourneren een `ClusterListResultPage` struct. Als u de volgende pagina, kunt u bellen `Next()`. Dit kan worden herhaald totdat `ClusterListResultPage.NotDone()` retourneert `false`, zoals wordt weergegeven in het onderstaande voorbeeld.

#### <a name="example"></a>Voorbeeld
Het volgende voorbeeld worden de eigenschappen van alle clusters voor het huidige abonnement:

```golang
page, err := client.List(context.Background())
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, cluster := range page.Values() {
        fmt.Println(*cluster.Name)
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

### <a name="delete-a-cluster"></a>Een cluster verwijderen

Een cluster verwijderen:

```golang
client.Delete(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a>Bijwerken van cluster-tags

U kunt de labels van een bepaald cluster bijwerken als volgt te werk:

```golang
client.Update(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ClusterPatchParameters{<map[string]*string} of Tags>)
```
#### <a name="example"></a>Voorbeeld

```golang
client.Update(context.Background(), "SDKTestRG", "SDKTest", hdi.ClusterPatchParameters{map[string]*string{"tag1Name" : to.StringPtr("tag1Value"), "tag2Name" : to.StringPtr("tag2Value")}})
```

### <a name="resize-cluster"></a>Grootte van cluster wijzigen

U kunt het formaat van het aantal worker-knooppunten van een bepaald cluster door een nieuwe grootte op te geven als volgt te werk:

```golang
client.Resize(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ClusterResizeParameters{<Num of Worker Nodes (int)>})
```

## <a name="cluster-monitoring"></a>Clusterbewaking

De Management-SDK voor HDInsight kan ook worden gebruikt voor het beheren van controle op uw clusters via de Operations Management Suite (OMS).

Op soortgelijke wijze aan hoe u hebt gemaakt `ClusterClient` wilt gebruiken voor beheertaken uit te voeren, moet u maken een `ExtensionClient` moet worden gebruikt voor het bewaken van bewerkingen. Nadat u de bovenstaande sectie verificatie hebt voltooid, kunt u een `ExtensionClient` als volgt te werk:

```golang
extClient := hdi.NewExtensionsClient(SUBSCRIPTION_ID)
extClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> De onderstaande bewaking voorbeelden wordt ervan uitgegaan u hebt al geïnitialiseerd een `ExtensionClient` met de naam `extClient` en stel de `Authorizer` zoals hierboven.

### <a name="enable-oms-monitoring"></a>OMS-controle inschakelen

> [!NOTE]  
> Om in te schakelen OMS-controle, moet u een bestaande Log Analytics-werkruimte hebben. Als u nog geen hebt gemaakt een, leert u hoe u dat hier doen: [Een Log Analytics-werkruimte maken in Azure portal](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).

Om in te schakelen de OMS-controle op het cluster:

```golang
extClient.EnableMonitoring(context.Background(), "<Resource Group Name", "Cluster Name", hdi.ClusterMonitoringRequest {WorkspaceID: to.StringPtr("<Workspace Id>")})
```

### <a name="view-status-of-oms-monitoring"></a>De status van de OMS-controle weergeven

De status van OMS in uw cluster ophalen:

```golang
extClient.GetMonitoringStatus(context.Background(), "<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a>OMS-controle uitschakelen

OMS uitschakelen op het cluster:

```golang
extClient.DisableMonitoring(context.Background(), "<Resource Group Name", "Cluster Name")
```

## <a name="script-actions"></a>Scriptacties

HDInsight biedt een configuratie scriptacties aangeroepen functie die wordt aangeroepen aangepaste scripts voor het aanpassen van het cluster.

> [!NOTE]  
> Meer informatie over het gebruik van scriptacties vindt u hier: [HDInsight Linux gebaseerde clusters aanpassen met scriptacties](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### <a name="execute-script-actions"></a>Uitvoeren van scriptacties

U kunt scriptacties uitvoeren op een bepaald cluster als volgt te werk:

```golang
var scriptAction1 = hdi.RuntimeScriptAction{Name: to.StringPtr("<Script Name>"), URI: to.StringPtr("<URL To Script>"), Roles: <&[]string of roles>} //valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"
client.ExecuteScriptActions(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ExecuteScriptActionParameters{PersistOnSuccess: to.BoolPtr(true), ScriptActions: &[]hdi.RuntimeScriptAction{scriptAction1}}) //add more RuntimeScriptActions to the list to execute multiple scripts
```

Voor de 'Scriptactie verwijderen' en 'Lijst met vastgelegde scriptacties', moet u maken een `ScriptActionsClient`, vergelijkbaar met hoe u hebt gemaakt `ClusterClient` moet worden gebruikt voor beheerbewerkingen. Nadat u de bovenstaande sectie verificatie hebt voltooid, kunt u een `ScriptActionsClient` als volgt te werk:

```golang
scriptActionsClient := hdi.NewScriptActionsClient(SUBSCRIPTION_ID)
scriptActionsClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> De onderstaande scriptacties voorbeelden wordt ervan uitgegaan dat u hebt al geïnitialiseerd een `ScriptActionsClient` met de naam `scriptActionsClient` en stel de `Authorizer` zoals hierboven.

### <a name="delete-script-action"></a>Scriptactie verwijderen

Een opgegeven persistente scriptactie op een bepaald cluster verwijderen:

```golang
scriptActionsClient.Delete(context.Background(), "<Resource Group Name>", "<Cluster Name>", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a>Lijst met persistente scriptacties

> [!NOTE]  
> Beide `ListByCluster()` retourneert een `ScriptActionsListPage` struct. Als u de volgende pagina, kunt u bellen `Next()`. Dit kan worden herhaald totdat `ClusterListResultPage.NotDone()` retourneert `false`, zoals wordt weergegeven in het onderstaande voorbeeld.

Overzicht van alle persistente scriptacties voor het opgegeven cluster:
```golang
scriptActionsClient.ListByCluster(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Voorbeeld

```golang
page, err := scriptActionsClient.ListByCluster(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, script := range page.Values() {
        fmt.Println(*script.Name) //There are functions to get other properties of RuntimeScriptActionDetail besides Name, such as Status, Operation, StartTime, EndTime, etc. See reference documentation.
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

### <a name="list-all-scripts-execution-history"></a>Overzicht van uitvoeringsgeschiedenis voor alle scripts

Voor deze bewerking, die u wilt maken een `ScriptExecutionHistoryClient`, vergelijkbaar met hoe u hebt gemaakt `ClusterClient` moet worden gebruikt voor beheerbewerkingen. Nadat u de bovenstaande sectie verificatie hebt voltooid, kunt u een `ScriptActionsClient` als volgt te werk:

```golang
scriptExecutionHistoryClient := hdi.NewScriptExecutionHistoryClient(SUBSCRIPTION_ID)
scriptExecutionHistoryClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> De hieronder wordt ervan uitgegaan dat u hebt al geïnitialiseerd een `ScriptExecutionHistoryClient` met de naam `scriptExecutionHistoryClient` en stel de `Authorizer` zoals hierboven.

Overzicht van uitvoeringsgeschiedenis alle scripts voor het opgegeven cluster:

```golang
scriptExecutionHistoryClient.ListByCluster(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Voorbeeld

In dit voorbeeld worden alle details voor alle eerdere uitvoeringen door een script afgedrukt.

```golang
page, err := scriptExecutionHistoryClient.ListByCluster(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, script := range page.Values() {
        fmt.Println(*script.Name) //There are functions to get other properties of RuntimeScriptActionDetail besides Name, such as Status, Operation, StartTime, EndTime, etc. See reference documentation.
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

## <a name="next-steps"></a>Volgende stappen

* Verken de [GoDoc referentiemateriaal](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight). De GoDocs bieden referentiedocumentatie voor alle functies in de SDK.
