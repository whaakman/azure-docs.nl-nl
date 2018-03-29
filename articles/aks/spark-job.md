---
title: Een Apache Spark-taak uitvoeren met Azure Container Service (AKS)
description: Azure Container Service (AKS) gebruiken voor het uitvoeren van een Apache Spark-taak
services: container-service
author: lenadroid
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/15/2018
ms.author: alehall
ms.custom: mvc
ms.openlocfilehash: 3991312d7f7609bb0a206ccc0ecc67123ebec469
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="running-apache-spark-jobs-on-aks"></a>Apache Spark taken uitgevoerd op AKS

[Apache Spark] [ apache-spark] is een snelle engine voor grootschalige verwerking. Vanaf de [Spark 2.3.0 release][spark-latest-release], Apache Spark ondersteunt systeemeigen integratie met Kubernetes clusters. Azure Container Service (AKS) is een beheerde Kubernetes omgeving worden uitgevoerd in Azure. In dit document worden voorbereiden en Apache Spark taken uitgevoerd op een Azure Container Service (AKS)-cluster.

## <a name="prerequisites"></a>Vereisten

Om de stappen in dit artikel hebt voltooid, moet u het volgende.

* Basiskennis van Kubernetes en [Apache Spark][spark-quickstart].
* [Docker-Hub] [ docker-hub] -account of een [Azure Container register][acr-create].
* Azure CLI [geïnstalleerd] [ azure-cli] op uw ontwikkelsysteem.
* [JDK 8] [ java-install] op uw systeem geïnstalleerd.
* SBT ([Scala bouwen hulpprogramma][sbt-install]) op uw systeem geïnstalleerd.
* GIT opdrachtregelprogramma's op uw systeem geïnstalleerd.

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

Spark wordt gebruikt voor grootschalige gegevensverwerking en vereist dat Kubernetes knooppunten worden aangepast, zodat deze voldoen aan de vereisten van de resources Spark. We raden een minimale grootte van `Standard_D3_v2` voor uw Azure Container Service (AKS)-knooppunten.

Als u een cluster AKS die voldoet aan deze minimale aanbeveling, voer de volgende opdrachten.

Maak een resourcegroep voor het cluster.

```azurecli
az group create --name mySparkCluster --location eastus
```

Maken van het cluster AKS met knooppunten die met een grootte van `Standard_D3_v2`.

```azurecli
az aks create --resource-group mySparkCluster --name mySparkCluster --node-vm-size Standard_D3_v2
```

Verbinding maken met het cluster AKS.

```azurecli
az aks get-credentials --resource-group mySparkCluster --name mySparkCluster
```

Als u van Azure Container register (ACR) gebruikmaakt voor het opslaan van installatiekopieën van de container, configureert u verificatie tussen AKS en ACR. Zie de [ACR verificatie documentatie] [ acr-aks] voor deze stappen.

## <a name="build-the-spark-source"></a>De bron Spark bouwen

Voordat u taken uitvoert Spark op een cluster AKS, moet u voor het bouwen van de Spark broncode en deze in een installatiekopie van een container van het pakket. De Spark-bron bevat scripts die kunnen worden gebruikt om dit proces te voltooien.

Kloon de opslagplaats Spark project naar uw ontwikkelsysteem.

```bash
git clone -b branch-2.3 https://github.com/apache/spark
```

Wijzigen in de map van de gekloonde opslagplaats en het pad van de bron Spark opslaan in een variabele.

```bash
cd spark
sparkdir=$(pwd)
```

Als u meerdere JDK versies zijn geïnstalleerd, stelt `JAVA_HOME` versie 8 gebruiken voor de huidige sessie.

```bash
export JAVA_HOME=`/usr/libexec/java_home -d 64 -v "1.8*"`
```

Voer de volgende opdracht om de Spark broncode met ondersteuning voor Kubernetes samen te stellen.

```bash
./build/mvn -Pkubernetes -DskipTests clean package
```

De volgende opdrachten de installatiekopie van de container Spark maken en dit doorgeven aan een container installatiekopie-register. Vervang `registry.example.com` met de naam van het register van de container en `v1` met het label dat u wilt gebruiken. Deze waarde is de naam van routeringsregister als met behulp van Docker-Hub. Als Azure Container register (ACR) wordt gebruikt, is deze waarde de servernaam van de ACR-aanmelding.

```bash
REGISTRY_NAME=registry.example.com
REGISTRY_TAG=v1
```

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG build
```

De container-installatiekopie aan het register van de installatiekopie container push.

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG push
```

## <a name="prepare-a-spark-job"></a>Een taak Spark voorbereiden

Vervolgens bereidt een Spark-taak. Een jar-bestand wordt gebruikt om de taak Spark en nodig hebt met de `spark-submit` opdracht. De jar worden aangebracht toegankelijk zijn via een openbare URL of vooraf verpakt in een installatiekopie van een container. In dit voorbeeld wordt een voorbeeld jar gemaakt voor het berekenen van de waarde van Pi. Deze jar is vervolgens naar Azure storage geüpload. Als u een bestaande jar hebt, gerust vervangen

Maak een map waarin u wilt het project voor een Spark-taak maken.

```bash
mkdir myprojects
cd myprojects
```

Maak een nieuwe Scala-project uit een sjabloon.

```bash
sbt new sbt/scala-seed.g8
```

Wanneer u wordt gevraagd, typt u `SparkPi` voor naam van het project.

```bash
name [Scala Seed Project]: SparkPi
```

Ga naar de map van het nieuwe project.

```bash
cd sparkpi
```

Voer de volgende opdrachten om toe te voegen, een invoegtoepassing SBT, waardoor het verpakken van het project als een jar-bestand.

```bash
touch project/assembly.sbt
echo 'addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.6")' >> project/assembly.sbt
```

Voer deze opdrachten de voorbeeldcode kopiëren naar het nieuwe project en alle benodigde afhankelijkheden toevoegen.

```bash
EXAMPLESDIR="src/main/scala/org/apache/spark/examples"
mkdir -p $EXAMPLESDIR
cp $sparkdir/examples/$EXAMPLESDIR/SparkPi.scala $EXAMPLESDIR/SparkPi.scala

cat <<EOT >> build.sbt
// https://mvnrepository.com/artifact/org.apache.spark/spark-sql
libraryDependencies += "org.apache.spark" %% "spark-sql" % "2.3.0" % "provided"
EOT

sed -ie 's/scalaVersion.*/scalaVersion := "2.11.11",/' build.sbt
sed -ie 's/name.*/name := "SparkPi",/' build.sbt
```

Voer de volgende opdracht om het project in een jar inpakken.

```bash
sbt assembly
```

Na de geslaagde verpakking ziet u uitvoer ziet er als volgt.

```bash
[info] Packaging /Users/me/myprojects/sparkpi/target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar ...
[info] Done packaging.
[success] Total time: 10 s, completed Mar 6, 2018 11:07:54 AM
```

## <a name="copy-job-to-storage"></a>De taak kopiëren naar de opslag

Maak een Azure-opslagaccount en container voor het opslaan van de jar-bestand.

```azurecli
RESOURCE_GROUP=sparkdemo
STORAGE_ACCT=sparkdemo$RANDOM
az group create --name $RESOURCE_GROUP --location eastus
az storage account create --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT --sku Standard_LRS
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT -o tsv`
```

Upload het jar-bestand naar de Azure storage-account met de volgende opdrachten.

```bash
CONTAINER_NAME=jars
BLOB_NAME=SparkPi-assembly-0.1.0-SNAPSHOT.jar
FILE_TO_UPLOAD=target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar

echo "Creating the container..."
az storage container create --name $CONTAINER_NAME
az storage container set-permission --name $CONTAINER_NAME --public-access blob

echo "Uploading the file..."
az storage blob upload --container-name $CONTAINER_NAME --file $FILE_TO_UPLOAD --name $BLOB_NAME

jarUrl=$(az storage blob url --container-name $CONTAINER_NAME --name $BLOB_NAME | tr -d '"')
```

Variabele `jarUrl` bevat nu het openbaar toegankelijk pad naar het jar-bestand.

## <a name="submit-a-spark-job"></a>Verzenden van een Spark-taak

Start kube-proxy in een apart opdrachtregel met de volgende code.

```bash
kubectl proxy
```

Ga terug naar de hoofdmap van Spark-opslagplaats.

```bash
cd $sparkdir
```

Verzenden de taak met `spark-submit`.

```bash
./bin/spark-submit \
  --master k8s://http://127.0.0.1:8001 \
  --deploy-mode cluster \
  --name spark-pi \
  --class org.apache.spark.examples.SparkPi \
  --conf spark.executor.instances=3 \
  --conf spark.kubernetes.container.image=$REGISTRY_NAME/spark:$REGISTRY_TAG \
  $jarUrl
```

Deze bewerking wordt de taak, Spark, die de status van de taak aan uw sessie shell streams gestart. Terwijl de taak wordt uitgevoerd, ziet u Spark stuurprogramma schil en executor gehele product met behulp van de kubectl gehele product opdracht ophalen. Open een tweede terminalsessie om deze opdrachten uitvoeren.

```console
$ kubectl get pods

NAME                                               READY     STATUS     RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   1/1       Running    0          16s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-1   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-2   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-3   0/1       Init:0/1   0          4s
```

Terwijl de taak wordt uitgevoerd, kunt u ook toegang tot de gebruikersinterface van Spark. In de tweede terminalsessie en gebruik de `kubectl port-forward` opdracht toegang bieden tot Spark-gebruikersinterface.

```bash
kubectl port-forward spark-pi-2232778d0f663768ab27edc35cb73040-driver 4040:4040
```

Voor toegang tot Spark-gebruikersinterface, opent u het adres `127.0.0.1:4040` in een browser.

![Spark gebruikersinterface](media/aks-spark-job/spark-ui.png)

## <a name="get-job-results-and-logs"></a>Resultaten van de taak en logboeken ophalen

Nadat de taak is voltooid, worden de stuurprogramma-schil in een status 'Voltooid'. De naam van de schil met de volgende opdracht worden opgehaald.

```bash
kubectl get pods --show-all
```

Uitvoer:

```bash
NAME                                               READY     STATUS      RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   0/1       Completed   0          1m
```

Gebruik de `kubectl logs` opdracht logboeken van de spark stuurprogramma schil ophalen. De naam van de schil vervangen door de naam van de stuurprogramma-schil.

```bash
kubectl logs spark-pi-2232778d0f663768ab27edc35cb73040-driver
```

In deze logboeken ziet u het resultaat van de taak, Spark, die de waarde van Pi.

```bash
Pi is roughly 3.152155760778804
```

## <a name="package-jar-with-container-image"></a>De jar pakket met de installatiekopie van de container

In het bovenstaande voorbeeld is het Spark jar-bestand geüpload naar Azure-opslag. Een andere optie is naar het jar-bestand van het pakket op maat gemaakte Docker afbeeldingen.

Om dit te doen, vinden de `dockerfile` voor de installatiekopie van het Spark op `$sparkdir/resource-managers/kubernetes/docker/src/main/dockerfiles/spark/` directory. Voeg ben `ADD` -instructie voor de taak Spark `jar` ergens tussen `WORKDIR` en `ENTRYPOINT` declaraties.

Het pad van de jar bijwerken naar de locatie van de `SparkPi-assembly-0.1.0-SNAPSHOT.jar` -bestand op uw ontwikkelsysteem. U kunt ook uw eigen aangepaste jar-bestand.

```bash
WORKDIR /opt/spark/work-dir

ADD /path/to/SparkPi-assembly-0.1.0-SNAPSHOT.jar SparkPi-assembly-0.1.0-SNAPSHOT.jar

ENTRYPOINT [ "/opt/entrypoint.sh" ]
```

Bouw en push van de installatiekopie met de opgenomen Spark-scripts.

```bash
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> build
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> push
```

Wanneer de taak, in plaats van die wijzen op een externe jar-URL, de `local://` schema kan worden gebruikt met het pad naar het jar-bestand in de Docker-installatiekopie.

```bash
./bin/spark-submit \
    --master k8s://https://<k8s-apiserver-host>:<k8s-apiserver-port> \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=3 \
    --conf spark.kubernetes.container.image=<spark-image> \
    local:///opt/spark/work-dir/<your-jar-name>.jar
```

> [!WARNING]
> Van Spark [documentatie][spark-docs]: "de planner Kubernetes is momenteel experimentele. In toekomstige versies mogelijk zijn er wijzigingen rond configuratie, installatiekopieën van de container en toegangspunten'.

## <a name="next-steps"></a>Volgende stappen

Uitchecken Spark-documentatie voor meer informatie.

> [!div class="nextstepaction"]
> [Spark-documentatie][spark-docs]

<!-- LINKS - external -->
[apache-spark]: https://spark.apache.org/
[docker-hub]: https://docs.docker.com/docker-hub/
[java-install]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[sbt-install]: https://www.scala-sbt.org/1.0/docs/Setup.html
[spark-docs]: https://spark.apache.org/docs/latest/running-on-kubernetes.html
[spark-latest-release]: https://spark.apache.org/releases/spark-release-2-3-0.html
[spark-quickstart]: https://spark.apache.org/docs/latest/quick-start.html


<!-- LINKS - internal -->
[acr-aks]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-auth-aks
[acr-create]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-azure-cli
[aks-quickstart]: https://docs.microsoft.com/en-us/azure/aks/
[azure-cli]: https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest
[storage-account]: https://docs.microsoft.com/en-us/azure/storage/common/storage-azure-cli
