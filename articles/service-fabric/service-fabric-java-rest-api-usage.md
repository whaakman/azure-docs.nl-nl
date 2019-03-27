---
title: Azure Service Fabric Java-Client-API's | Microsoft Docs
description: Genereren en het gebruik van Service Fabric Java-client API's met behulp van REST API-specificatie van Service Fabric-client
services: service-fabric
documentationcenter: java
author: rapatchi
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/27/2017
ms.author: rapatchi
ms.openlocfilehash: a6cb5ffe6c21c6eda9eb8b7f610952e92707a6bd
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483468"
---
# <a name="azure-service-fabric-java-client-apis"></a>Azure Service Fabric Java-Client-API 's

Service Fabric client-API's kunt implementeren en beheren van microservices op basis van toepassingen en containers in een Service Fabric-cluster in Azure, on-premises, op de lokale ontwikkelcomputer of in andere cloud. In dit artikel wordt beschreven hoe u genereren en het gebruik van Service Fabric Java client-API's boven op de Service Fabric-client REST-API 's

## <a name="generate-the-client-code-using-autorest"></a>De clientcode met behulp van AutoRest genereren

[AutoRest](https://github.com/Azure/autorest) is een hulpprogramma dat-clientbibliotheken genereert voor het openen van RESTful-web-services. Invoer voor AutoRest is een specificatie die de REST-API met behulp van de OpenAPI-specificatie-indeling. [Service Fabric-client REST-API's](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/servicefabric/data-plane) volgt u deze specificatie.

Volg de stappen die hieronder worden vermeld voor het genereren van clientcode voor Service Fabric Java met behulp van het hulpprogramma AutoRest.

1. nodejs en NPM installeren op uw computer

    Als u Linux vervolgens gebruikt:
    ```bash
    sudo apt-get install npm
    sudo apt install nodejs
    ```
    Als u Mac OS X vervolgens gebruikt:
    ```bash
    brew install node
    ```

2. Installeer AutoRest met NPM.
    ```bash
    npm install -g autorest
    ```

3. Fork en kloon [azure-rest-api-specificaties](https://github.com/Azure/azure-rest-api-specs) opslagplaats in uw lokale machine en Ga naar de gekloonde locatie vanaf de terminal van uw computer.


4. Ga naar de locatie die hieronder worden vermeld in de gekloonde opslagplaats.
    ```bash
    cd specification\servicefabric\data-plane\Microsoft.ServiceFabric\stable\6.0
    ```

    > [!NOTE]
    > Als uw clusterversie niet 6.0. * gaat u naar de juiste map in de map stabiel.
    >   

5. Voer de volgende autorest-opdracht voor het genereren van de code van de java-client.
    
    ```bash
    autorest --input-file= servicefabric.json --java --output-folder=[output-folder-name] --namespace=[namespace-of-generated-client]
    ```
   Hieronder volgt een voorbeeld van het gebruik van autorest aan te tonen.
   
    ```bash
    autorest --input-file=servicefabric.json --java --output-folder=java-rest-api-code --namespace=servicefabricrest
    ```
   
   De volgende opdracht wordt ``servicefabric.json`` specificatie bestand als invoer en genereert de clientcode java in ``java-rest-api-     code`` map en de code in omsluit ``servicefabricrest`` naamruimte. Na deze stap vindt u twee mappen ``models``, ``implementation`` en twee bestanden ``ServiceFabricClientAPIs.java`` en ``package-info.java`` gegenereerd in de ``java-rest-api-code`` map.


## <a name="include-and-use-the-generated-client-in-your-project"></a>Opnemen en de gegenereerde client gebruiken in uw project

1. Voeg de gegenereerde code op de juiste wijze in uw project. U wordt aangeraden dat u een bibliotheek met behulp van de gegenereerde code maken en deze bibliotheek in uw project opnemen.
2. Als u het maken van een bibliotheek opnemen van de volgende afhankelijkheden in uw bibliotheek van project. Als u een andere benadering neemt de afhankelijkheid op de juiste wijze.

    ```
        GroupId:  com.microsoft.rest
        Artifactid: client-runtime
        Version: 1.2.1
    ```
    Bijvoorbeeld, als u met behulp van Maven build-systeem zijn onder andere het volgende in uw ``pom.xml`` bestand:

    ```xml
        <dependency>
          <groupId>com.microsoft.rest</groupId>
          <artifactId>client-runtime</artifactId>
          <version>1.2.1</version>
        </dependency>
    ```

3. Maak een RestClient met de volgende code:

    ```java
        RestClient simpleClient = new RestClient.Builder()
            .withBaseUrl("http://<cluster-ip or name:port>")
            .withResponseBuilderFactory(new ServiceResponseBuilder.Factory())
            .withSerializerAdapter(new JacksonAdapter())
            .build();
        ServiceFabricClientAPIs client = new ServiceFabricClientAPIsImpl(simpleClient);
    ```
4. Clientobject voor het gebruik en de juiste aanroepen zoals vereist. Hier volgen enkele voorbeelden die laten zien van het gebruik van clientobject. Gaan we ervan uit dat het toepassingspakket is gemaakt en geüpload naar de installatiekopieopslag voordat u de onderstaande API's.
    * Inrichten van een toepassing
    
        ```java
            ApplicationTypeImageStorePath imageStorePath = new ApplicationTypeImageStorePath();
            imageStorePath.withApplicationTypeBuildPath("<application-path-in-image-store>");
            client.provisionApplicationType(imageStorePath);
        ```
    * Een app maken

        ```java
            ApplicationDescription applicationDescription = new ApplicationDescription();
            applicationDescription.withName("<application-uri>");
            applicationDescription.withTypeName("<application-type>");
            applicationDescription.withTypeVersion("<application-version>");
            client.createApplication(applicationDescription);
        ```

## <a name="understanding-the-generated-code"></a>Wat is de gegenereerde code?
Voor elke API vindt u vier overloads van de implementatie. Als er optionele parameters zijn zou u vier meer variaties, met inbegrip van de volgende optionele parameters vinden. Zo kunt u de API ``removeReplica``.
 1. **Public void removeReplica (tekenreeks knooppuntnaam, UUID partitionId, replicaId tekenreeks, Boole forceRemove, lange time-out)**
    * Dit is de synchrone variant van de removeReplica API-aanroep
 2. **openbare ServiceFuture\<Void > removeReplicaAsync (String, knooppuntnaam, UUID partitionId, replicaId tekenreeks, Boole forceRemove, lange time-out, laatste ServiceCallback\<Void > serviceCallback)**
    * Deze variant van de API-aanroep kan worden gebruikt als u wilt gebruiken van toekomstige op basis van asynchrone programmering en retouraanroepen
 3. **openbaar zichtbaar zijn\<Void > removeReplicaAsync (tekenreeks knooppuntnaam, UUID partitionId, tekenreeks replicaId)**
    * Deze variant van de API-aanroep kan worden gebruikt als u wilt gebruiken, reactieve asynchrone programmering
 4. **openbaar zichtbaar zijn\<ServiceResponse\<Void >> removeReplicaWithServiceResponseAsync (tekenreeks knooppuntnaam, UUID partitionId, tekenreeks replicaId)**
    * Deze variant van de API-aanroep kan worden gebruikt als u wilt gebruiken, reactieve asynchrone programmering en omgaan met ONBEWERKTE rest-antwoord

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Service Fabric-REST-API's](https://docs.microsoft.com/rest/api/servicefabric/)

