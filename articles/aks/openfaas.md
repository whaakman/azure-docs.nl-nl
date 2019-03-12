---
title: Gebruik van OpenFaaS met Azure Kubernetes Service (AKS)
description: Implementeren en gebruiken van OpenFaaS met Azure Kubernetes Service (AKS)
services: container-service
author: justindavies
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/05/2018
ms.author: juda
ms.custom: mvc
ms.openlocfilehash: 5ed6e0b21b00ede3f78a102fd004e5706ae3cea5
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57571215"
---
# <a name="using-openfaas-on-aks"></a>Met behulp van OpenFaaS in AKS

[OpenFaaS] [ open-faas] is een structuur voor het bouwen van serverloze functies door het gebruik van containers. Als een open source-project, heeft deze ervaring grootschalige acceptatie binnen de community. In dit document worden installeren en met behulp van OpenFaas op een cluster Azure Kubernetes Service (AKS).

## <a name="prerequisites"></a>Vereisten

Om de stappen in dit artikel hebt voltooid, moet u het volgende.

* Basiskennis van Kubernetes.
* Een cluster Azure Kubernetes Service (AKS) en het AKS-referenties die zijn geconfigureerd op uw ontwikkelsysteem.
* Azure CLI is geïnstalleerd op uw ontwikkelsysteem.
* GIT opdrachtregelprogramma's geïnstalleerd op uw systeem.

## <a name="add-the-openfaas-helm-chart-repo"></a>De opslagplaats van OpenFaaS helm-grafiek toevoegen

OpenFaaS onderhoudt een eigen helm-grafieken te houden met de meest recente wijzigingen.

```azurecli-interactive
helm repo add openfaas https://openfaas.github.io/faas-netes/
helm repo update
```

## <a name="deploy-openfaas"></a>OpenFaaS implementeren

Als een goede gewoonte OpenFaaS en OpenFaaS functies moeten worden opgeslagen in hun eigen Kubernetes-naamruimten.

Een naamruimte voor de functies en OpenFaaS system maken:

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml
```

Genereren van een wachtwoord voor de Portal van OpenFaaS UI en de REST-API:

```azurecli-interactive
# generate a random password
PASSWORD=$(head -c 12 /dev/urandom | shasum| cut -d' ' -f1)

kubectl -n openfaas create secret generic basic-auth \
--from-literal=basic-auth-user=admin \
--from-literal=basic-auth-password="$PASSWORD"
```

Krijgt u de waarde van het geheim met `echo $PASSWORD`.

Het wachtwoord dat we hier maakt, wordt door het helm-diagram worden gebruikt om basisverificatie inschakelen op de OpenFaaS Gateway, die wordt blootgesteld aan Internet via een cloud Load Balancer.

Een Helm-grafiek voor OpenFaaS is opgenomen in de gekloonde opslagplaats. In deze grafiek gebruiken voor het implementeren van OpenFaaS in uw AKS-cluster.

```azurecli-interactive
helm upgrade openfaas --install openfaas/openfaas \
    --namespace openfaas  \
    --set basic_auth=true \
    --set functionNamespace=openfaas-fn \
    --set serviceType=LoadBalancer
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

Een openbaar IP-adres is voor toegang tot de OpenFaaS-gateway gemaakt. Als u wilt dit IP-adres ophalen, gebruikt u de [kubectl service ophalen] [ kubectl-get] opdracht. Het duurt even voor het IP-adres moet worden toegewezen aan de service.

```console
kubectl get service -l component=gateway --namespace openfaas
```

De uitvoer.

```console
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
gateway            ClusterIP      10.0.156.194   <none>         8080/TCP         7m
gateway-external   LoadBalancer   10.0.28.18     52.186.64.52   8080:30800/TCP   7m
```

Als u wilt testen van het systeem OpenFaaS, blader naar het externe IP-adres op poort 8080, `http://52.186.64.52:8080` in dit voorbeeld. U wordt gevraagd om aan te melden. Als u wilt ophalen van uw wachtwoord, voer `echo $PASSWORD`.

![OpenFaaS UI](media/container-service-serverless/openfaas.png)

Ten slotte de CLI OpenFaaS installeren. In dit voorbeeld gebruikt u homebrew, Zie de [OpenFaaS CLI-documentatie] [ open-faas-cli] voor meer opties.

```console
brew install faas-cli
```

Stel `$OPENFAAS_URL` naar het openbare IP-adres gevonden hierboven.

Meld u aan met de Azure CLI:

```azurecli-interactive
export OPENFAAS_URL=http://52.186.64.52:8080
echo -n $PASSWORD | ./faas-cli login -g $OPENFAAS_URL -u admin --password-stdin
```

## <a name="create-first-function"></a>Eerste functie maken

Nu dat OpenFaaS ingeschakeld is, moet u een functie met behulp van OpenFaas portal maken.

Klik op **nieuwe functie implementeren** en zoek naar de **Figlet**. Selecteer de functie Figlet en klikt u op **implementeren**.

![Figlet](media/container-service-serverless/figlet.png)

Gebruik curl om de functie aanroepen. Vervang het IP-adres in het volgende voorbeeld door die van uw gateway OpenFaas.

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

Maak nu een tweede functie. In dit voorbeeld wordt geïmplementeerd met behulp van OpenFaaS CLI en bevat een aangepaste container-installatiekopie en het ophalen van gegevens uit een Cosmos DB. Meerdere items moeten worden geconfigureerd voordat u de functie maakt.

Maak eerst een nieuwe resourcegroep voor de Cosmos DB.

```azurecli-interactive
az group create --name serverless-backing --location eastus
```

Implementeer een CosmosDB-exemplaar van het type `MongoDB`. Het exemplaar moet een unieke naam in, bij te werken `openfaas-cosmos` in een uniek is voor uw omgeving.

```azurecli-interactive
az cosmosdb create --resource-group serverless-backing --name openfaas-cosmos --kind MongoDB
```

De Cosmos-database-verbindingsreeks ophalen en opslaan in een variabele.

Werk de waarde voor de `--resource-group` argument voor de naam van de resourcegroep, en de `--name` argument voor de naam van uw Cosmos DB.

```azurecli-interactive
COSMOS=$(az cosmosdb list-connection-strings \
  --resource-group serverless-backing \
  --name openfaas-cosmos \
  --query connectionStrings[0].connectionString \
  --output tsv)
```

Nu vullen van de Cosmos DB met testgegevens. Maak een bestand met de naam `plans.json` en kopieer de volgende json.

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

Gebruik de *mongoimport* hulpprogramma voor het laden van het cosmos DB-exemplaar met gegevens.

Installeer indien nodig, de MongoDB-hulpprogramma's. Het volgende voorbeeld installeert deze hulpprogramma's met behulp van homebrew, raadpleegt u de [MongoDB-documentatie] [ install-mongo] voor andere opties.

```azurecli-interactive
brew install mongodb
```

Laad de gegevens in de database.

```azurecli-interactive
mongoimport --uri=$COSMOS -c plans < plans.json
```

Uitvoer:

```console
2018-02-19T14:42:14.313+0000    connected to: localhost
2018-02-19T14:42:14.918+0000    imported 1 document
```

Voer de volgende opdracht om de functie te maken. Werk de waarde van de `-g` argument met de gateway-adres van uw OpenFaaS.

```azurecli-interctive
faas-cli deploy -g http://52.186.64.52:8080 --image=shanepeckham/openfaascosmos --name=cosmos-query --env=NODE_ENV=$COSMOS
```

Zodra geïmplementeerd, ziet u het zojuist gemaakte OpenFaaS-eindpunt voor de functie.

```console
Deployed. 202 Accepted.
URL: http://52.186.64.52:8080/function/cosmos-query
```

De functie met curl testen. Het IP-adres met het adres van de gateway OpenFaaS bijwerken.

```console
curl -s http://52.186.64.52:8080/function/cosmos-query
```

Uitvoer:

```json
[{"ID":"","Name":"two_person","FriendlyName":"","PortionSize":"","MealsPerWeek":"","Price":72,"Description":"Our basic plan, delivering 3 meals per week, which will feed 1-2 people."}]
```

U kunt ook de functie binnen de UI OpenFaaS testen.

![alternatieve tekst](media/container-service-serverless/OpenFaaSUI.png)

## <a name="next-steps"></a>Volgende stappen

U kunt voor meer informatie over met de workshop OpenFaaS met een reeks praktijksessies die betrekking hebben op onderwerpen, zoals het maken van uw eigen GitHub-bot, blijven gebruiken van geheimen, metrische gegevens weergeven en automatisch schalen.

<!-- LINKS - external -->
[install-mongo]: https://docs.mongodb.com/manual/installation/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[open-faas]: https://www.openfaas.com/
[open-faas-cli]: https://github.com/openfaas/faas-cli
[openfaas-workshop]: https://github.com/openfaas/workshop
