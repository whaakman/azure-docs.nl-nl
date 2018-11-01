---
title: Een Apache Spark-taak uitvoeren met Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service (AKS) gebruiken voor het uitvoeren van een Apache Spark-taak
services: container-service
author: lenadroid
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/15/2018
ms.author: alehall
ms.custom: mvc
ms.openlocfilehash: ddaff590fd493b430a72c30dd35cb1b891b80d84
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414026"
---
# <a name="running-apache-spark-jobs-on-aks"></a>Apache Spark-taken die worden uitgevoerd in AKS

[Apache Spark] [ apache-spark] is een engine voor snelle voor grootschalige gegevensverwerking. Vanaf de [Spark 2.3.0 release][spark-latest-release], Apache Spark biedt ondersteuning voor systeemeigen integratie met Kubernetes-clusters. Azure Kubernetes Service (AKS) is een beheerde Kubernetes-omgeving worden uitgevoerd in Azure. In dit document worden voorbereidt en Apache Spark-taken uitvoert op een cluster Azure Kubernetes Service (AKS).

## <a name="prerequisites"></a>Vereisten

Om de stappen in dit artikel hebt voltooid, moet u het volgende.

* Basiskennis van Kubernetes en [Apache Spark][spark-quickstart].
* [Docker Hub] [ docker-hub] -account, of een [Azure Container Registry][acr-create].
* Azure CLI [geïnstalleerd] [ azure-cli] op uw ontwikkelsysteem.
* [JDK 8] [ java-install] op uw systeem geïnstalleerd.
* SBT ([Scala bouwen hulpprogramma][sbt-install]) op uw systeem geïnstalleerd.
* GIT opdrachtregelprogramma's geïnstalleerd op uw systeem.

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

Spark wordt gebruikt voor grootschalige gegevensverwerking en vereist dat Kubernetes-knooppunten worden aangepast, zodat deze voldoen aan de vereisten van de resources Spark. We raden aan een minimumgrootte van `Standard_D3_v2` voor uw Azure Kubernetes Service (AKS)-knooppunten.

Als u een AKS-cluster die voldoet aan deze minimale aanbeveling nodig hebt, kunt u de volgende opdrachten uitvoeren.

Maak een resourcegroep voor het cluster.

```azurecli
az group create --name mySparkCluster --location eastus
```

De AKS-cluster maken met knooppunten die van de grootte van zijn `Standard_D3_v2`.

```azurecli
az aks create --resource-group mySparkCluster --name mySparkCluster --node-vm-size Standard_D3_v2
```

Verbinding maken met het AKS-cluster.

```azurecli
az aks get-credentials --resource-group mySparkCluster --name mySparkCluster
```

Als u Azure Container Registry (ACR) gebruikt voor het opslaan van installatiekopieën van containers, wordt verificatie tussen AKS en ACR configureren. Zie de [ACR-verificatie documentatie] [ acr-aks] voor deze stappen.

## <a name="build-the-spark-source"></a>Bouw de Spark-bron

Voordat het Spark-taken op een AKS-cluster uitvoert, moet u voor het bouwen van de Spark broncode en verpakt u deze in een containerinstallatiekopie. De Spark-bron bevat scripts die kunnen worden gebruikt om dit proces te voltooien.

Kloon de opslagplaats van de Spark-project naar uw ontwikkelsysteem.

```bash
git clone -b branch-2.3 https://github.com/apache/spark
```

Wijzig in de map van de gekloonde opslagplaats en het pad van de Spark-bron in een variabele opslaan.

```bash
cd spark
sparkdir=$(pwd)
```

Als u meerdere JDK versies geïnstalleerd hebt, stelt u `JAVA_HOME` versie 8 gebruiken voor de huidige sessie.

```bash
export JAVA_HOME=`/usr/libexec/java_home -d 64 -v "1.8*"`
```

Voer de volgende opdracht uit de Spark om broncode te bouwen met ondersteuning voor Kubernetes.

```bash
./build/mvn -Pkubernetes -DskipTests clean package
```

De volgende opdrachten de installatiekopie van de Spark-container maken en deze naar een afbeelding containerregister pushen. Vervang `registry.example.com` met de naam van het containerregister en `v1` met het label dat u wilt gebruiken. Als u met behulp van Docker Hub, is deze waarde de naam van het containerregister. Als u Azure Container Registry (ACR) gebruikt, is deze waarde de naam van de ACR-aanmeldingsserver.

```bash
REGISTRY_NAME=registry.example.com
REGISTRY_TAG=v1
```

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG build
```

De containerinstallatiekopie naar het register van de container-installatiekopie pushen.

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG push
```

## <a name="prepare-a-spark-job"></a>Voorbereiden van een Spark-taak

Vervolgens bereidt u een Spark-taak. Een jar-bestand wordt gebruikt voor het opslaan van de Spark-taak en is nodig bij het uitvoeren van de `spark-submit` opdracht. De jar kan worden aangebracht toegankelijk zijn via een openbare URL of vooraf is verpakt in een containerinstallatiekopie. In dit voorbeeld wordt een voorbeeld-jar gemaakt voor het berekenen van de waarde van Pi. Deze jar is vervolgens geüpload naar Azure storage. Als u een bestaande jar hebt, kunt u vervangen door

Maak een map waar u wilt maken van het project voor een Spark-taak.

```bash
mkdir myprojects
cd myprojects
```

Maak een nieuw project voor een Scala van een sjabloon.

```bash
sbt new sbt/scala-seed.g8
```

Wanneer u hierom wordt gevraagd, voert u `SparkPi` voor naam van het project.

```bash
name [Scala Seed Project]: SparkPi
```

Navigeer naar de zojuist gemaakte projectmap.

```bash
cd sparkpi
```

Voer de volgende opdrachten om toe te voegen een invoegtoepassing SBT, waardoor de verpakking van het project als een jar-bestand.

```bash
touch project/assembly.sbt
echo 'addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.6")' >> project/assembly.sbt
```

Voer deze opdrachten om te kopiëren van de voorbeeldcode in het nieuwe project en alle vereiste afhankelijkheden toevoegen.

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

Als u het project in een jar pakket, wilt u de volgende opdracht uitvoeren.

```bash
sbt assembly
```

Na geslaagde verpakking ziet u uitvoer die vergelijkbaar is met de volgende.

```bash
[info] Packaging /Users/me/myprojects/sparkpi/target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar ...
[info] Done packaging.
[success] Total time: 10 s, completed Mar 6, 2018 11:07:54 AM
```

## <a name="copy-job-to-storage"></a>Taak voor het kopiëren naar opslag

Maak een Azure storage-account en een container voor het opslaan van de jar-bestand.

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

## <a name="submit-a-spark-job"></a>Een Spark-taak verzenden

Start kube-proxy in een afzonderlijke opdrachtregel door de volgende code.

```bash
kubectl proxy
```

Ga terug naar de hoofdmap van Spark-opslagplaats.

```bash
cd $sparkdir
```

Verzenden van de taak met behulp van `spark-submit`.

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

Met deze bewerking wordt de Spark-taak, die taakstatus aan uw shellsessie gegevensstromen gestart. Terwijl de taak wordt uitgevoerd, ziet u Spark-stuurprogramma pod en executor-schillen met behulp van de kubectl schillen opdracht ophalen. Open een tweede terminalsessie om uit te voeren van deze opdrachten.

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

![Spark-UI](media/aks-spark-job/spark-ui.png)

## <a name="get-job-results-and-logs"></a>Taakresultaten en logboeken ophalen

Nadat de taak is voltooid, worden de stuurprogramma-schil bevindt zich in een status 'Voltooid'. Haal de naam van de schil met de volgende opdracht.

```bash
kubectl get pods --show-all
```

Uitvoer:

```bash
NAME                                               READY     STATUS      RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   0/1       Completed   0          1m
```

Gebruik de `kubectl logs` opdracht voor het ophalen van Logboeken van de spark-stuurprogramma-schil. De naam van de schil vervangen door de naam van de stuurprogramma-schil.

```bash
kubectl logs spark-pi-2232778d0f663768ab27edc35cb73040-driver
```

In deze logboeken ziet u het resultaat van de Spark-taak de waarde van Pi is.

```bash
Pi is roughly 3.152155760778804
```

## <a name="package-jar-with-container-image"></a>Pakket jar met containerinstallatiekopie

In het bovenstaande voorbeeld is de Spark-jar-bestand geüpload naar Azure storage. Een andere optie is om het jar-bestand in op maat gebouwde Docker-installatiekopieën.

Om dit te doen, vinden de `dockerfile` voor de installatiekopie van het Spark zich bevindt in `$sparkdir/resource-managers/kubernetes/docker/src/main/dockerfiles/spark/` directory. Voeg ben `ADD` -instructie voor de Spark-taak `jar` ergens tussen `WORKDIR` en `ENTRYPOINT` declaraties.

Bijwerken van de jar-pad naar de locatie van de `SparkPi-assembly-0.1.0-SNAPSHOT.jar` -bestand op uw ontwikkelsysteem. U kunt ook uw eigen aangepaste jar-bestand gebruiken.

```bash
WORKDIR /opt/spark/work-dir

ADD /path/to/SparkPi-assembly-0.1.0-SNAPSHOT.jar SparkPi-assembly-0.1.0-SNAPSHOT.jar

ENTRYPOINT [ "/opt/entrypoint.sh" ]
```

Bouw en push de installatiekopie met de bijgeleverde scripts van Spark.

```bash
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> build
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> push
```

Bij het uitvoeren van de taak, in plaats van die wijzen op een externe jar-URL, de `local://` schema kan worden gebruikt met het pad naar het jar-bestand in de Docker-installatiekopie.

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
> Uit Spark [documentatie][spark-docs]: "de scheduler Kubernetes is momenteel experimentele. In toekomstige versies, mogelijk zijn er wijzigingen om de configuratie, containerinstallatiekopieën en toegangspunten'.

## <a name="next-steps"></a>Volgende stappen

Bekijk de Spark-documentatie voor meer informatie.

> [!div class="nextstepaction"]
> [Spark-documentatie][spark-docs]

<!-- LINKS - external -->
[apache-spark]: https://spark.apache.org/
[docker-hub]: https://docs.docker.com/docker-hub/
[java-install]: https://aka.ms/azure-jdks
[sbt-install]: https://www.scala-sbt.org/1.0/docs/Setup.html
[spark-docs]: https://spark.apache.org/docs/latest/running-on-kubernetes.html
[spark-latest-release]: https://spark.apache.org/releases/spark-release-2-3-0.html
[spark-quickstart]: https://spark.apache.org/docs/latest/quick-start.html


<!-- LINKS - internal -->
[acr-aks]: https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks
[acr-create]: https://docs.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli
[aks-quickstart]: https://docs.microsoft.com/azure/aks/
[azure-cli]: https://docs.microsoft.com/cli/azure/?view=azure-cli-latest
[storage-account]: https://docs.microsoft.com/azure/storage/common/storage-azure-cli
