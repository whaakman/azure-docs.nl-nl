---
title: Integratie van bestaande MongoDB-toepassing met Azure Cosmos DB API voor MongoDB en Open Service Broker for Azure (OSBA)
description: In dit artikel leert u hoe u een bestaande Java- en MongoDB-toepassing integreren met de Azure Cosmos DB-API voor MongoDB met behulp van Open Service Broker for Azure (OSBA).
services: azure-dev-spaces
author: zr-msft
manager: jeconnoc
ms.service: azure-dev-spaces
ms.topic: article
ms.date: 01/25/2019
ms.author: zarhoads
ms.custom: mvc
keywords: Cosmos DB, Open Service Broker, Open Service Broker for Azure
ms.openlocfilehash: 04b513de1d47749bb87b7aaf79839389ab4d7290
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55082515"
---
# <a name="integrate-existing-mongodb-application-with-azure-cosmos-db-api-for-mongodb-and-open-service-broker-for-azure-osba"></a>Integratie van bestaande MongoDB-toepassing met Azure Cosmos DB API voor MongoDB en Open Service Broker for Azure (OSBA)

Azure Cosmos DB is een wereldwijd gedistribueerde, multi-model databaseservice. Het biedt ook wire-protocol compatibiliteit met verschillende NoSQL APIs inclusief voor MongoDB. De Cosmos DB-API voor MongoDB kunt u het gebruik van Cosmos DB met uw bestaande MongoDB-toepassing zonder te hoeven wijzigen of implementatie van stuurprogramma's van uw toepassing-database. U kunt ook een Cosmos DB-service met behulp van Open Service Broker voor Azure inrichten.

In dit artikel nemen van een bestaande Java-toepassing die gebruikmaakt van een MongoDB-database en deze bijwerken om een Cosmos DB-database met behulp van Open Service Broker voor Azure.

## <a name="prerequisites"></a>Vereisten

Voordat u verdergaat, moet u:
    
* Hebben een [Azure Kubernetes Service-cluster](kubernetes-walkthrough.md) gemaakt.
* Hebben [Open Service Broker for Azure geïnstalleerd en geconfigureerd op uw AKS-cluster](integrate-azure.md). 
* Hebben de [Service Catalog CLI](https://svc-cat.io/docs/install/) geïnstalleerd en geconfigureerd voor uitvoering `svcat` opdrachten.
* Een bestaande [MongoDB](https://www.mongodb.com/) database. Bijvoorbeeld, wellicht u MongoDB op uw [ontwikkelcomputer](https://docs.mongodb.com/manual/administration/install-community/) of in een [virtuele Azure-machine](../virtual-machines/linux/install-mongodb.md).
* Hebt u een manier om verbinding te maken met en het uitvoeren van query's de MongoDB-database, zoals de [mongo-shell](https://docs.mongodb.com/manual/mongo/).

## <a name="get-application-code"></a>Toepassingscode ophalen
    
In dit artikel gebruikt u de [spring muziek-voorbeeldtoepassing uit Cloud Foundry](https://github.com/cloudfoundry-samples/spring-music) ter illustratie van een toepassing die gebruikmaakt van een MongoDB-database.
    
Kloon de toepassing uit GitHub en navigeer naar de bijbehorende map:
    
```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring-music
```

## <a name="prepare-the-application-to-use-your-mongodb-database"></a>De toepassing voor het gebruik van de MongoDB-database voorbereiden

De voorbeeldtoepassing van de spring-muziek biedt veel opties voor gegevensbronnen. In dit artikel hebt configureren u deze voor het gebruik van een bestaande MongoDB-database. 

De volgende YAML toevoegen aan het einde van *src/main/resources/application.yml*. Deze toevoeging wordt gemaakt van een profiel met de naam *mongodb* en configureert u de naam van een URI en database. De URI vervangen door de verbindingsgegevens voor uw bestaande MongoDB-database. Het toevoegen van de URI, waarin een gebruikersnaam en wachtwoord, rechtstreeks naar dit bestand is voor **alleen ontwikkelactiviteiten** en **moet nooit worden toegevoegd aan versiebeheer**.

```yaml
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://user:password@serverAddress:port/musicdb"
      database: musicdb
```



Wanneer u uw toepassing te starten en deze gebruiken de *mongodb* profiel op deze verbinding maakt met de MongoDB-database en deze gebruiken voor het opslaan van gegevens van de toepassing.

Het bouwen van uw toepassing:

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```

Uw toepassing te starten en deze gebruiken de *mongodb* profiel:

```cmd
java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

Navigeer naar http://localhost:8080 in uw browser.

![Spring Music-app met standaardgegevens](media/music-app.png)

U ziet de toepassing is gevuld met enkele [standaardgegevens](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json). Communiceren met het door het verwijderen van een paar bestaande albums en enkele nieuwe maken.

U kunt controleren of uw toepassing gebruik maakt van de MongoDB-database door verbinding mee te maken en uitvoeren van query's de *musicdb* database:

```cmd
mongo serverAddress:port/musicdb -u user -p password
use musicdb
db.album.find()

{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446d"), "title" : "Nevermind", "artist" : "Nirvana", "releaseYear" : "1991", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446e"), "title" : "Pet Sounds", "artist" : "The Beach Boys", "releaseYear" : "1966", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446f"), "title" : "What's Going On", "artist" : "Marvin Gaye", "releaseYear" : "1971", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
...
```

In het voorgaande voorbeeld wordt de [mongo-shell](https://docs.mongodb.com/manual/mongo/) verbinding maken met de MongoDB-database en deze opvragen. U kunt ook controleren of dat uw wijzigingen worden doorgevoerd door uw toepassing te stoppen en te navigeren naar het in uw browser te starten. U ziet de wijzigingen die u hebt aangebracht, zijn er nog.


## <a name="create-a-cosmos-db-database"></a>Een Cosmos DB-database maken

Gebruik voor het maken van een Cosmos DB-database in Azure met behulp van Open Service Broker de `svcat provision` opdracht:

```cmd
svcat provision musicdb --class azure-cosmosdb-mongo-account --plan account  --params-json '{
  "location": "eastus",
  "resourceGroup": "MyResourceGroup",
  "ipFilters" : {
    "allowedIPRanges" : ["0.0.0.0/0"]
  }
}'
```

De voorgaande opdracht richt een Cosmos DB-database in Azure in de resourcegroep *MyResourceGroup* in de *eastus* regio. Meer informatie over *resourceGroup*, *locatie*, en andere Azure-specifieke JSON-parameters is beschikbaar in de [naslagdocumentatie voor Cosmos DB-module](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/cosmosdb.md#provision-3).

Als u wilt controleren of het inrichten van de database is voltooid, gebruikt u de opdracht `svcat get instance`:

```cmd
$ svcat get instance musicdb

   NAME     NAMESPACE              CLASS                PLAN     STATUS
+---------+-----------+------------------------------+---------+--------+
  musicdb   default     azure-cosmosdb-mongo-account   account   Ready
```

Uw database is gereed wanneer Zie *gereed* onder *STATUS*.

Als uw database is voltooid wordt ingericht, moet u de metagegevens te binden een [Kubernetes-geheim](https://kubernetes.io/docs/concepts/configuration/secret/). Andere toepassingen hebben vervolgens toegang tot die gegevens nadat deze is gebonden aan een geheim. Als u wilt de metagegevens van uw database koppelen aan een geheim, gebruikt u de `svcat bind` opdracht:

```cmd
$ svcat bind musicdb

  Name:        musicdb
  Namespace:   default
  Status:
  Secret:      musicdb
  Instance:    musicdb

Parameters:
  No parameters defined
```


## <a name="use-the-cosmos-db-database-with-your-application"></a>De Cosmos DB-database gebruiken met uw toepassing

Voor het gebruik van de Cosmos DB-database met uw toepassing, moet u weten de URI moet er verbinding mee maken. Als u deze informatie, gebruikt de `kubectl get secret` opdracht:

```cmd
$ kubectl get secret musicdb -o=jsonpath='{.data.uri}' | base64 --decode

mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
```

De voorgaande opdracht haalt de *musicdb* geheim en geeft alleen de URI. Geheimen worden opgeslagen in Base 64-indeling, zodat deze ook in de voorgaande opdracht worden gedecodeerd.

Bijwerken met behulp van de URI van de Cosmos DB-database, *src/main/resources/application.yml* om deze te gebruiken:

```yaml
...
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
      database: musicdb
```

Het bijwerken van de URI, waarin een gebruikersnaam en wachtwoord, rechtstreeks naar dit bestand is voor **alleen ontwikkelactiviteiten** en **moet nooit worden toegevoegd aan versiebeheer**.

Bouwen en uw toepassing om te beginnen met behulp van de Cosmos DB-database te starten:

```cmd
./gradlew clean assemble

java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

U ziet uw toepassing nog steeds gebruikt de *mongodb* profiel en een URI die met begint *mongodb: / /* verbinding maken met de Cosmos DB-database. De [Azure Cosmos DB-API voor MongoDB](../cosmos-db/mongodb-introduction.md) biedt deze compatibiliteit. Hierdoor kan uw toepassing om te blijven functioneren als een MongoDB-database wordt gebruikt, maar wordt deze daadwerkelijk gebruik van Cosmos DB maakt.

Navigeer naar http://localhost:8080 in uw browser. U ziet dat de gegevens die standaard is hersteld. Communiceren met het door het verwijderen van een paar bestaande albums en enkele nieuwe maken. U kunt controleren of dat uw wijzigingen worden doorgevoerd door uw toepassing te stoppen en te navigeren naar het in uw browser te starten. U ziet de wijzigingen die u hebt aangebracht, zijn er nog. De wijzigingen worden persistent gemaakt met de Cosmos DB hebt gemaakt met behulp van Open Service Broker voor Azure.


## <a name="run-your-application-on-your-aks-cluster"></a>Uw toepassing uitvoeren in uw AKS-cluster

U kunt [Azure Dev spaties](../dev-spaces/azure-dev-spaces.md) om de toepassing naar uw AKS-cluster te implementeren. Azure Dev spaties helpt u bij het genereren van artefacten, zoals Dockefiles en Helm-grafieken en implementeren en uitvoeren van een toepassing in AKS.

Om in te schakelen Azure Dev spaties in uw AKS-cluster:

```cmd
az aks enable-addons --addons http_application_routing -g MyResourceGroup -n MyAKS
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

De Azure Dev spaties hulpmiddelen gebruiken om voor te bereiden van uw toepassing om uit te voeren in AKS:

```cmd
azds prep --public
```

Met deze opdracht genereert verschillende artefacten, met inbegrip van een *grafieken /* map, namelijk uw Helm-diagram in de hoofdmap van het project. Met deze opdracht kan niet genereren een *Dockerfile* voor dit specifieke project, zodat u deze maken.

Maak een bestand in de hoofdmap van uw project met de naam *Dockerfile* met deze inhoud:

```dockerfile
FROM openjdk:8-jdk-alpine
EXPOSE 8080
WORKDIR /app
COPY build/libs/spring-music-1.0.jar .
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
```

Bovendien moet u bijwerken de *configurations.develop.build* eigenschap in *azds.yaml* naar *false*:
```yaml
...
configurations:
  develop:
    build:
      useGitIgnore: false
```

U moet ook bijwerken de *containerPort* kenmerk *8080* in *charts/spring-music/templates/deployment.yaml*:

```yaml
...
spec:
  ...
  template:
    ...
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
```

Uw toepassing in AKS implementeert:

```cmd
$ azds up

Using dev space 'default' with target 'MyAKS'
Synchronizing files...1m 18s
Installing Helm chart...5s
Waiting for container image build...23s
Building container image...
Step 1/5 : FROM openjdk:8-jdk-alpine
Step 2/5 : EXPOSE 8080
Step 3/5 : WORKDIR /app
Step 4/5 : COPY build/libs/spring-music-1.0.jar .
Step 5/5 : ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
Built container image in 21s
Waiting for container...8s
Service 'spring-music' port 'http' is available at http://spring-music.1234567890abcdef1234.eastus.aksapp.io/
Service 'spring-music' port 8080 (TCP) is available at http://localhost:57892
press Ctrl+C to detach
...
```

Navigeer naar de URL die wordt weergegeven in de logboeken. In het vorige voorbeeld, zou u *http://spring-music.1234567890abcdef1234.eastus.aksapp.io/*. 

Controleer of dat u de toepassing zien, samen met uw wijzigingen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt beschreven hoe u een bestaande toepassing met een MongoDB met Cosmos DB-API voor MongoDB bijwerken. In dit artikel wordt ook getoond over het inrichten van een Cosmos DB-service met behulp van Open Service Broker voor Azure en implementeren van de toepassing bij naar AKS met Azure Dev spaties.

Open Service Broker for Azure en Azure Dev spaties, Zie voor meer informatie over Cosmos DB:
* [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
* [Open Service Broker for Azure](https://osba.sh)
* [Ontwikkelen met Dev-opslagruimten](../dev-spaces/azure-dev-spaces.md)
