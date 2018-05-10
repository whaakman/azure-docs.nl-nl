---
title: OpenFaaS gebruiken met Azure Kubernetes-Service (AKS)
description: Implementeren en gebruiken van OpenFaaS met Azure Kubernetes Service (AKS)
services: container-service
author: justindavies
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/05/2018
ms.author: juda
ms.custom: mvc
ms.openlocfilehash: e26f1c298b05153736edd2b2efd0f1b27162bc3d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="using-openfaas-on-aks"></a>Met behulp van OpenFaaS op AKS

[OpenFaaS] [ open-faas] is een framework voor het bouwen van zonder Server functies boven op containers. Als een Open Source-project, heeft deze ervaring grootschalige acceptatie binnen de community. In dit document worden installeren en gebruiken van OpenFaas op een Azure Kubernetes Service (AKS)-cluster.

## <a name="prerequisites"></a>Vereisten

Om de stappen in dit artikel hebt voltooid, moet u het volgende.

* Basiskennis van Kubernetes.
* Een Azure Kubernetes Service (AKS)-cluster en AKS referenties die zijn geconfigureerd op uw ontwikkelsysteem.
* Azure CLI op uw ontwikkelsysteem geïnstalleerd.
* GIT opdrachtregelprogramma's op uw systeem geïnstalleerd.

## <a name="get-openfaas"></a>OpenFaaS ophalen

Kloon de opslagplaats OpenFaaS project naar uw ontwikkelsysteem.

```azurecli-interactive
git clone https://github.com/openfaas/faas-netes
```

Wijzigen in de map van de gekloonde opslagplaats.

```azurecli-interactive
cd faas-netes
```

## <a name="deploy-openfaas"></a>OpenFaaS implementeren

Als een goede gewoonte OpenFaaS en OpenFaaS functies moeten worden opgeslagen in de naamruimte van hun eigen Kubernetes.

Een naamruimte voor het systeem OpenFaaS maken.

```azurecli-interactive
kubectl create namespace openfaas
```

Maak een tweede naamruimte voor OpenFaaS functies.

```azurecli-interactive
kubectl create namespace openfaas-fn
```

Een grafiek Helm voor OpenFaaS is opgenomen in de gekloonde opslagplaats. Dit diagram OpenFaaS implementeren in uw cluster AKS gebruiken.

```azurecli-interactive
helm install --namespace openfaas -n openfaas \
  --set functionNamespace=openfaas-fn, \
  --set serviceType=LoadBalancer, \
  --set rbac=false chart/openfaas/
```

Uitvoer:

```
NAME:   openfaas
LAST DEPLOYED: Wed Feb 28 08:26:11 2018
NAMESPACE: openfaas
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                 DATA  AGE
prometheus-config    2     20s
alertmanager-config  1     20s

{snip}

NOTES:
To verify that openfaas has started, run:

  kubectl --namespace=openfaas get deployments -l "release=openfaas, app=openfaas"
```

Een openbaar IP-adres wordt voor toegang tot de gateway OpenFaaS gemaakt. U kunt dit IP-adres ophalen, gebruiken de [kubectl ophalen service] [ kubectl-get] opdracht. Het duurt een paar minuten voor het IP-adres moet worden toegewezen aan de service.

```console
kubectl get service -l component=gateway --namespace openfaas
```

Uitvoer.

```console
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
gateway            ClusterIP      10.0.156.194   <none>         8080/TCP         7m
gateway-external   LoadBalancer   10.0.28.18     52.186.64.52   8080:30800/TCP   7m
```

Als u wilt testen van het systeem OpenFaaS, blader naar het externe IP-adres op poort 8080, `http://52.186.64.52:8080` in dit voorbeeld.

![OpenFaaS gebruikersinterface](media/container-service-serverless/openfaas.png)

Ten slotte OpenFaaS CLI installeren. In dit voorbeeld gebruikt brew, Zie de [OpenFaaS CLI documentatie] [ open-faas-cli] voor meer opties.

```console
brew install faas-cli
```

## <a name="create-first-function"></a>Eerste-functie maken

Nu dat OpenFaaS ingeschakeld is, maakt u een functie met de portal OpenFaas.

Klik op **nieuwe functie implementeren** en zoek naar **Figlet**. Selecteer de functie Figlet en op **implementeren**.

![Figlet](media/container-service-serverless/figlet.png)

Gebruik curl aanroepen van de functie. Het IP-adres in het volgende voorbeeld met die van uw gateway OpenFaas vervangen.

```azurecli-interactive
curl -X POST http://52.186.64.52:8080/function/figlet -d "Hello Azure"
```

Uitvoer:

```console
 _   _      _ _            _
| | | | ___| | | ___      / \    _____   _ _ __ ___
| |_| |/ _ \ | |/ _ \    / _ \  |_  / | | | '__/ _ \
|  _  |  __/ | | (_) |  / ___ \  / /| |_| | | |  __/
|_| |_|\___|_|_|\___/  /_/   \_\/___|\__,_|_|  \___|

```

## <a name="create-second-function"></a>Tweede functie maken

Maak nu een tweede functie. In dit voorbeeld wordt geïmplementeerd met behulp van de CLI OpenFaaS en bevat een installatiekopie van een aangepaste container en ophalen van gegevens uit een Cosmos-database. Meerdere items moeten worden geconfigureerd voordat u de functie maakt.

Maak eerst een nieuwe resourcegroep voor de Cosmos-database.

```azurecli-interactive
az group create --name serverless-backing --location eastus
```

Implementeren van een CosmosDB exemplaar van het type `MongoDB`. Het exemplaar moet een unieke naam, bijwerken `openfaas-cosmos` in iets dat uniek is voor uw omgeving.

```azurecli-interactive
az cosmosdb create --resource-group serverless-backing --name openfaas-cosmos --kind MongoDB
```

De Cosmos-database-verbindingsreeks ophalen en opslaan in een variabele.

Werk de waarde voor de `--resource-group` argument voor de naam van de resourcegroep en de `--name` argument voor de naam van de Cosmos-database.

```azurecli-interactive
COSMOS=$(az cosmosdb list-connection-strings \
  --resource-group serverless-backing \
  --name openfaas-cosmos \
  --query connectionStrings[0].connectionString \
  --output tsv)
```

Nu de Cosmos-database met testgegevens vullen. Maak een bestand met de naam `plans.json` en kopieer de volgende json.

```json
{
    "name" : "two_person",
    "friendlyName" : "Two Person Plan",
    "portionSize" : "1-2 Person",
    "mealsPerWeek" : "3 Unique meals per week",
    "price" : 72,
    "description" : "Our basic plan, delivering 3 meals per week, which will feed 1-2 people.",
    "__v" : 0
}
```

Gebruik de *mongoimport* hulpprogramma voor het exemplaar CosmosDB met gegevens te laden.

Installeer zo nodig de MongoDB-hulpprogramma's. Het volgende voorbeeld installeert deze hulpprogramma's met behulp van brew, Zie de [MongoDB-documentatie] [ install-mongo] voor andere opties.

```azurecli-interactive
brew install mongodb
```

De gegevens in de database laden.

```azurecli-interactive
mongoimport --uri=$COSMOS -c plans < plans.json
```

Uitvoer:

```console
2018-02-19T14:42:14.313+0000    connected to: localhost
2018-02-19T14:42:14.918+0000    imported 1 document
```

Voer de volgende opdracht voor het maken van de functie. Werk de waarde van de `-g` argument met de gateway-adres van uw OpenFaaS.

```azurecli-interctive
faas-cli deploy -g http://52.186.64.52:8080 --image=shanepeckham/openfaascosmos --name=cosmos-query --env=NODE_ENV=$COSMOS
```

Zodra geïmplementeerd, ziet u het zojuist gemaakte OpenFaaS eindpunt voor de functie.

```console
Deployed. 202 Accepted.
URL: http://52.186.64.52:8080/function/cosmos-query
```

De functie met curl testen. Het IP-adres met het adres OpenFaaS bijwerken.

```console
curl -s http://52.186.64.52:8080/function/cosmos-query
```

Uitvoer:

```json
[{"ID":"","Name":"two_person","FriendlyName":"","PortionSize":"","MealsPerWeek":"","Price":72,"Description":"Our basic plan, delivering 3 meals per week, which will feed 1-2 people."}]
```

U kunt ook de functie binnen de OpenFaaS UI testen.

![alternatieve tekst](media/container-service-serverless/OpenFaaSUI.png)

## <a name="next-steps"></a>Volgende stappen

De standaardimplementatie van OpenFaas moet worden vergrendeld voor OpenFaaS-Gateway en functies. [De Alex Ellis blogbericht](https://blog.alexellis.io/lock-down-openfaas/) bevat meer informatie over veilige configuratie-opties.

<!-- LINKS - external -->
[install-mongo]: https://docs.mongodb.com/manual/installation/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[open-faas]: https://www.openfaas.com/
[open-faas-cli]: https://github.com/openfaas/faas-cli