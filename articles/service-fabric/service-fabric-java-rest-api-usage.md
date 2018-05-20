---
title: API's van Azure Service Fabric Java-Client | Microsoft Docs
description: Genereren en het gebruik van Service Fabric-Java-client API's met behulp van REST-API-specificatie van Service Fabric-client
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
ms.openlocfilehash: bc5302b9af023b04fb6b1654ac13e9fc02e0a465
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="azure-service-fabric-java-client-apis"></a>Azure Service Fabric Java-Client-API 's

Service Fabric-client API's kunt implementeren en beheren van microservices op basis van toepassingen en containers in een Service Fabric-cluster in Azure, on-premises, op lokale ontwikkelcomputer of in andere cloud. Dit artikel wordt beschreven hoe genereren en het gebruik van Service Fabric-Java-client API's boven op de Service Fabric-client REST-API 's

## <a name="generate-the-client-code-using-autorest"></a>De clientcode genereren met AutoRest

[AutoRest](https://github.com/Azure/autorest) is een hulpprogramma dat wordt gegenereerd clientbibliotheken voor toegang tot de RESTful-web-services. Invoer voor AutoRest is een specificatie die beschrijft de REST-API met de indeling OpenAPI-specificatie. [Service Fabric-client REST-API's](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/servicefabric/data-plane) volgt u deze specificatie.

Volg de stappen voor het genereren van clientcode Service Fabric Java met het hulpprogramma AutoRest hieronder.

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

2. Installeren met NPM AutoRest.
    ```bash
    npm install -g autorest
    ```

3. Fork- en kloon [azure-rest-api-specificaties](https://github.com/Azure/azure-rest-api-specs) -opslagplaats in uw lokale machine en Ga naar de gekloonde locatie vanaf de terminal van uw machine.


4. Ga naar de locatie in de opslagplaats van uw gekloonde vermeld.
    ```bash
    cd specification\servicefabric\data-plane\Microsoft.ServiceFabric\stable\6.0
    ```

    > [!NOTE]
    > Als uw versie van het cluster niet 6.0. * gaat u naar de juiste map in de map stabiele.
    >   

5. Voer de volgende autorest-opdracht voor het genereren van de clientcode java.
    
    ```bash
    autorest --input-file= servicefabric.json --java --output-folder=[output-folder-name] --namespace=[namespace-of-generated-client]
    ```
   Hieronder ziet u een voorbeeld van het gebruik van autorest te demonstreren.
   
    ```bash
    autorest --input-file=servicefabric.json --java --output-folder=java-rest-api-code --namespace=servicefabricrest
    ```
   
   De volgende opdracht vergt ``servicefabric.json`` specificatie van het bestand als invoer en genereert de clientcode java in ``java-rest-api-     code`` map en de code in omsluit ``servicefabricrest`` naamruimte. Na deze stap vindt u twee mappen ``models``, ``implemenation`` en twee bestanden ``ServiceFabricClientAPIs.java`` en ``package-info.java`` gegenereerd in de ``java-rest-api-code`` map.


## <a name="include-and-use-the-generated-client-in-your-project"></a>Bevatten en de gegenereerde client gebruiken in uw project

1. Op de juiste wijze de gegenereerde code toevoegen aan uw project. U wordt aangeraden dat u een bibliotheek met de gegenereerde code maken en deze bibliotheek in uw project opnemen.
2. Als u een bibliotheek maakt en vervolgens de volgende afhankelijkheden in de bibliotheek-project opnemen. Als u een andere benadering neemt de afhankelijkheid op de juiste wijze.

    ```
        GroupId:  com.microsoft.rest
        Artifactid: client-runtime
        Version: 1.2.1
    ```
    Bijvoorbeeld, als u met behulp van Maven build-systeem omvatten het volgende in uw ``pom.xml`` bestand:

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
4. Gebruik het clientobject en de juiste aanroepen zoals vereist. Hier volgen enkele voorbeelden die het gebruik van clientobject aantonen. We gaan ervan uit dat het toepassingspakket is gemaakt en geüpload naar de installatiekopieopslag voordat u de onderstaande API's.
    * Een toepassing inrichten
    
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
Voor elke API vindt u vier overloads van de implementatie. Als er optionele parameters zou u vier meer variaties, met inbegrip van de volgende optionele parameters vinden. Houd bijvoorbeeld rekening met de API ``removeReplica``.
 1. **openbare void removeReplica (tekenreeks nodeName, UUID partitionId, replicaId String, Boolean forceRemove, lange time-out)**
    * Dit is de synchrone variant van de removeReplica API-aanroep
 2. **openbare ServiceFuture<Void> removeReplicaAsync (String nodeName, UUID partitionId, replicaId String, Boolean forceRemove, lang een time-out opgetreden, laatste ServiceCallback<Void> serviceCallback)**
    * Deze variant van de API-aanroep kan worden gebruikt als u wilt gebruiken van toekomstige gebaseerde asynchrone programmering en retouraanroepen gebruiken
 3. **openbare opslagtijd<Void> removeReplicaAsync (tekenreeks nodeName, UUID partitionId, tekenreeks replicaId)**
    * Deze variant van de API-aanroep kan worden gebruikt als u wilt gebruiken reactieve asynchrone programmering
 4. **openbare opslagtijd < ServiceResponse<Void>> removeReplicaWithServiceResponseAsync (tekenreeks nodeName, UUID partitionId, tekenreeks replicaId)**
    * Deze variant van de API-aanroep kan worden gebruikt als u wilt gebruiken reactieve asynchrone programmering en omgaan met ONBEWERKTE rest-antwoord

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Service Fabric REST-API's](https://docs.microsoft.com/rest/api/servicefabric/)

