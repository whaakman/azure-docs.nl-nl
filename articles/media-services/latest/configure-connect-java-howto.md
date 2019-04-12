---
title: Verbinding maken met Azure Media Services v3 API - Java
description: Leer hoe u verbinding maken met Media Services v3 API met Java.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2019
ms.author: juliako
ms.openlocfilehash: 9177a1ae1f2939979d1f824c98b6018a83c2779f
ms.sourcegitcommit: 41015688dc94593fd9662a7f0ba0e72f044915d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502261"
---
# <a name="connect-to-media-services-v3-api---java"></a>Verbinding maken met de API van Media Services v3 - Java

Dit artikel ziet u hoe u verbinding maakt met de SDK van Azure Media Services v3 Java met behulp van de service principal-aanmelding in de methode.

In dit artikel wordt de Visual Studio-Code voor het ontwikkelen van een app gebruikt.

## <a name="prerequisites"></a>Vereisten

- Ga als volgt [Java met Visual Studio Code schrijven](https://code.visualstudio.com/docs/java/java-tutorial) te installeren:

   - JDK
   - Apache Maven
   - Java Extension Pack
- Zorg ervoor dat ingesteld `JAVA_HOME` en `PATH` omgevingsvariabelen.
- [Een Azure Media Services-account maken](create-account-cli-how-to.md). Moet u om te onthouden naam van de resourcegroep en de naam van de Media Services-account.
- Volg de stappen in de [Acces-API's](access-api-cli-how-to.md) onderwerp. Noteer de abonnements-ID, toepassings-ID (client-ID), de verificatiesleutel (geheim genoemd) en de tenant-ID die u nodig hebt in een latere stap.

Bekijk ook:

- [Java in Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Beheer van de Java-Project in VS Code](https://code.visualstudio.com/docs/java/java-project)

## <a name="create-a-maven-project"></a>Maak een Maven-project

Open een opdrachtregel-hulpprogramma en `cd` naar een map waar u het project te maken.
    
```
mvn archetype:generate -DgroupId=com.azure.ams -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Wanneer u de opdracht uitvoert de `pom.xml`, `App.java`, en andere bestanden worden gemaakt. 

## <a name="add-dependencies"></a>Afhankelijkheden toevoegen

1. Open de map waarin het project in Visual Studio Code. 
1. Zoek en open de `pom.xml`. 
1. De vereiste afhankelijkheden toevoegen. Een van beide is [com.microsoft.azure.mediaservices.v2018_07_01:azure-mgmt-media](https://search.maven.org/artifact/com.microsoft.azure.mediaservices.v2018_07_01/azure-mgmt-media/1.0.0-beta/jar).

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.mediaservices.v2018_07_01</groupId>
      <artifactId>azure-mgmt-media</artifactId>
      <version>1.0.0-beta</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.rest</groupId>
      <artifactId>client-runtime</artifactId>
      <version>1.6.5</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-client-authentication</artifactId>
      <version>1.6.5</version>
    </dependency>
    </dependency>
    ```

## <a name="connect-to-the-java-client"></a>Verbinding maken met de Java-client

1. Open de `App.java` bestand onder `src\main\java\com\azure\ams` en zorg ervoor dat het pakket is toegevoegd aan de bovenkant:

    ```java
    package com.azure.ams;
    ```
2. Voeg deze onder de pakketinstructie importeren van instructies:
   
   ```java
   import com.microsoft.azure.AzureEnvironment;
   import com.microsoft.azure.credentials.ApplicationTokenCredentials;
   import com.microsoft.azure.management.mediaservices.v2018_07_01.implementation.MediaManager;
   import com.microsoft.rest.LogLevel;
   ```
2. De volgende code toevoegen aan de belangrijkste methode van de App-klasse voor het maken van de Active Directory-referenties die u nodig hebt om aan te vragen, en stel de waarden die u hebt verkregen via [Acces-API's](access-api-cli-how-to.md):
   
   ```java
   final String clientId = "00000000-0000-0000-0000-000000000000";
   final String tenantId = "00000000-0000-0000-0000-000000000000";
   final String clientSecret = "00000000-0000-0000-0000-000000000000";
   final String subscriptionId = "00000000-0000-0000-0000-000000000000";

   try {
      ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(clientId, tenantId, clientSecret, AzureEnvironment.AZURE);
      credentials.withDefaultSubscriptionId(subscriptionId);

      MediaManager manager = MediaManager
              .configure()
              .withLogLevel(LogLevel.BODY_AND_HEADERS)
              .authenticate(credentials, credentials.defaultSubscriptionId());
      System.out.println("signed in");
   }
   catch (Exception e) {
      System.out.println("Exception encountered.");
      System.out.println(e.toString());
   }
   ```

## <a name="see-also"></a>Zie ook

- [Media Services-concepten](concepts-overview.md)
- [Java-SDK](https://aka.ms/ams-v3-java-sdk)
- [Naslaginformatie over Java](https://aka.ms/ams-v3-java-ref)
- [https://search.maven.org/](https://search.maven.org/)

## <a name="next-steps"></a>Volgende stappen

U kunt nu opnemen `import com.microsoft.azure.management.mediaservices.v2018_07_01.Asset;` en manipuleren van entiteiten te starten.<br/>
Zo krijgt u alle activa in uw account: `Observable<Asset> asyncAssets = 
                    manager.assets().listAsync(groupId, accountId).last();`
