---
title: Apache Hadoop-clusters met behulp van Azure REST API - Azure maken
description: Leer hoe u HDInsight-clusters maken door het indienen van Azure Resource Manager-sjablonen op de Azure REST API.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: hrasheed
ms.openlocfilehash: 9a8c39864d05a4e2b89da7bdfc937f8d2e5dbec9
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51038414"
---
# <a name="create-apache-hadoop-clusters-using-the-azure-rest-api"></a>Apache Hadoop-clusters met behulp van de Azure REST API maken

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Informatie over het maken van een HDInsight-cluster met behulp van een Azure Resource Manager-sjabloon en de Azure REST API.

De Azure REST API kunt u beheerbewerkingen op services die worden gehost in de Azure-platform, met inbegrip van het maken van nieuwe resources zoals HDInsight-clusters.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

> [!NOTE]
> De stappen in dit document gebruiken de [curl (https://curl.haxx.se/) ](https://curl.haxx.se/) hulpprogramma om te communiceren met de Azure REST API.

## <a name="create-a-template"></a>Een sjabloon maken

Azure Resource Manager-sjablonen zijn JSON-documenten die beschrijven een **resourcegroep** en alle resources (zoals HDInsight.) Deze benadering op basis van een sjabloon kunt u de resources definiëren die u nodig hebt voor HDInsight in één sjabloon.

De volgende JSON-document is een fusie van de sjabloon en parameters-bestanden van [ https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), deze toepassing maakt een op basis van Linux-cluster met behulp van een wachtwoord om het SSH-gebruikersaccount te beveiligen.

   ```json
   {
       "properties": {
           "template": {
               "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
               "contentVersion": "1.0.0.0",
               "parameters": {
                   "clusterType": {
                       "type": "string",
                       "allowedValues": ["hadoop",
                       "hbase",
                       "storm",
                       "spark"],
                       "metadata": {
                           "description": "The type of the HDInsight cluster to create."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the HDInsight cluster to create."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                       }
                   },
                   "clusterLoginPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "sshUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to remotely access the cluster."
                       }
                   },
                   "sshPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the storage account to be created and be used as the cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "The number of nodes in the HDInsight cluster."
                       }
                   }
               },
               "variables": {
                   "defaultApiVersion": "2015-05-01-preview",
                   "clusterApiVersion": "2015-03-01-preview"
               },
               "resources": [{
                   "name": "[parameters('clusterStorageAccountName')]",
                   "type": "Microsoft.Storage/storageAccounts",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('defaultApiVersion')]",
                   "dependsOn": [],
                   "tags": {

                   },
                   "properties": {
                       "accountType": "Standard_LRS"
                   }
               },
               {
                   "name": "[parameters('clusterName')]",
                   "type": "Microsoft.HDInsight/clusters",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('clusterApiVersion')]",
                   "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                   "tags": {

                   },
                   "properties": {
                       "clusterVersion": "3.6",
                       "osType": "Linux",
                       "clusterDefinition": {
                           "kind": "[parameters('clusterType')]",
                           "configurations": {
                               "gateway": {
                                   "restAuthCredential.isEnabled": true,
                                   "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                   "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                               }
                           }
                       },
                       "storageProfile": {
                           "storageaccounts": [{
                               "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                               "isDefault": true,
                               "container": "[parameters('clusterName')]",
                               "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                           }]
                       },
                       "computeProfile": {
                           "roles": [{
                               "name": "headnode",
                               "targetInstanceCount": "2",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           },
                           {
                               "name": "workernode",
                               "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           }]
                       }
                   }
               }],
               "outputs": {
                   "cluster": {
                       "type": "object",
                       "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                   }
               }
           },
           "mode": "incremental",
           "Parameters": {
               "clusterName": {
                   "value": "newclustername"
               },
               "clusterType": {
                   "value": "hadoop"
               },
               "clusterStorageAccountName": {
                   "value": "newstoragename"
               },
               "clusterLoginUserName": {
                   "value": "admin"
               },
               "clusterLoginPassword": {
                   "value": "changeme"
               },
               "sshUserName": {
                   "value": "sshuser"
               },
               "sshPassword": {
                   "value": "changeme"
               }
           }
       }
   }
   ```

In dit voorbeeld wordt gebruikt in de stappen in dit document. Vervang het voorbeeld *waarden* in de **Parameters** sectie met de waarden voor uw cluster.

> [!IMPORTANT]
> De sjabloon maakt gebruik van het aantal worker-knooppunten (4) voor een HDInsight-cluster. Als u van plan op meer dan 32 worker-knooppunten bent, moet u de grootte van een hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB RAM-geheugen selecteren.
>
> Zie [Prijsdetails voor Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) voor meer informatie over knooppuntgrootten en de bijbehorende kosten.

## <a name="log-in-to-your-azure-subscription"></a>Aanmelden bij uw Azure-abonnement

Volg de stappen beschreven in [aan de slag met Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) en maak verbinding met uw abonnement met de `az login` opdracht.

## <a name="create-a-service-principal"></a>Een service-principal maken

> [!NOTE]
> Deze stappen zijn een verkorte versie van de *service-principal maken met een wachtwoord* sectie van de [gebruik Azure CLI voor het maken van een service-principal toegang krijgen tot bronnen](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md) document. Deze stappen maakt u een service-principal die wordt gebruikt voor verificatie bij de Azure REST API.

1. Vanaf de opdrachtregel, gebruikt u de volgende opdracht uit om uw Azure-abonnementen weer te geven.

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    In de lijst, selecteer het abonnement dat u wilt gebruiken en houd er rekening mee de **abonnements-id** en __Tenant_ID__ kolommen. Deze waarden opslaan.

2. Gebruik de volgende opdracht om te maken van een toepassing in Azure Active Directory.

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    Vervang de waarden voor de `--display-name`, `--homepage`, en `--identifier-uris` door uw eigen waarden. Een wachtwoord opgeven voor de nieuwe Active Directory-vermelding.

   > [!NOTE]
   > De `--home-page` en `--identifier-uris` waarden niet nodig hebt om te verwijzen naar een webpagina die wordt gehost op het internet. De unieke URI's, moeten ze zijn.

   De geretourneerde waarde van deze opdracht wordt de __App-ID__ voor de nieuwe toepassing. Deze waarde wordt opgeslagen.

3. Gebruik de volgende opdracht om te maken van een service-principal met behulp van de **App-ID**.

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     De geretourneerde waarde van deze opdracht wordt de __Object-ID__. Deze waarde wordt opgeslagen.

4. Toewijzen de **eigenaar** rol met de service principal via de **Object-ID** waarde. Gebruik de **abonnements-ID** u eerder hebt verkregen.

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a>Een verificatietoken ophalen

Gebruik de volgende opdracht om geen verificatietoken ophalen:

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

Stel `$TENANTID`, `$APPID`, en `$PASSWORD` op de waarden die zijn verkregen of eerder gebruikt.

Als deze aanvraag geslaagd is, ontvangt u een antwoord 200-serie en de antwoordtekst bevat een JSON-document.

Het JSON-document dat is geretourneerd door deze aanvraag bevat een element met de naam **access_token**. De waarde van **access_token** wordt gebruikt voor het verificatieaanvragen voor de REST-API.

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Het volgende gebruiken om een resourcegroep te maken.

* Stel `$SUBSCRIPTIONID` aan het abonnement-ID ontvangen tijdens het maken van de service-principal.
* Stel `$ACCESSTOKEN` aan het toegangstoken in de vorige stap hebt ontvangen.
* Vervang `DATACENTERLOCATION` met het datacenter dat u maken van de resourcegroep en resources wilt, in. Bijvoorbeeld 'Zuid-centraal VS'.
* Stel `$RESOURCEGROUPNAME` op de naam die u wilt gebruiken voor deze groep:

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

Als deze aanvraag geslaagd is, ontvangt u een antwoord 200-serie en de antwoordtekst bevat een JSON-document dat informatie bevat over de groep. De `"provisioningState"` element bevat een waarde van `"Succeeded"`.

## <a name="create-a-deployment"></a>Een implementatie maken

Gebruik de volgende opdracht voor het implementeren van de sjabloon naar de resourcegroep.

* Stel `$DEPLOYMENTNAME` op de naam die u wilt gebruiken voor deze implementatie.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [!NOTE]
> Als u de sjabloon naar een bestand hebt opgeslagen, kunt u de volgende opdracht uit in plaats van `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Als deze aanvraag geslaagd is, ontvangt u een antwoord 200-serie en de antwoordtekst bevat een JSON-document dat informatie bevat over de implementatiebewerking.

> [!IMPORTANT]
> De implementatie is verzonden, maar is niet voltooid. Het kan enige tijd duren, meestal ongeveer 15, voor de installatie is voltooid.

## <a name="check-the-status-of-a-deployment"></a>Controleer de status van een implementatie

Om te controleren of de status van de implementatie, gebruikt u de volgende opdracht:

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

Met deze opdracht retourneert een JSON-document dat informatie bevat over de implementatiebewerking. De `"provisioningState"` element bevat de status van de implementatie. Als dit element een waarde van bevat `"Succeeded"`, en vervolgens de implementatie is voltooid.

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een HDInsight-cluster hebt gemaakt, gebruikt u de volgende voor informatie over het werken met uw cluster.

### <a name="hadoop-clusters"></a>Hadoop-clusters

* [Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-clusters

* [Aan de slag met HBase in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Java-toepassingen voor HBase op HDInsight ontwikkelen](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-clusters

* [Java-topologieën ontwikkelen voor Storm op HDInsight](storm/apache-storm-develop-java-topology.md)
* [Python-onderdelen in Storm op HDInsight gebruiken](storm/apache-storm-develop-python-topology.md)
* [Topologieën met Storm op HDInsight implementeren en bewaken](storm/apache-storm-deploy-monitor-topology-linux.md)
