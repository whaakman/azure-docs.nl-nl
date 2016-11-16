---
title: Uw eerste Service Fabric-toepassing in Linux maken met behulp van Java | Microsoft Docs
description: Een Service Fabric-toepassing maken en implementeren met behulp van Java
services: service-fabric
documentationcenter: java
author: seanmck
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 288d504b44fd7588a03a31171da1bfb332e2429f


---
# <a name="create-your-first-azure-service-fabric-application"></a>Uw eerste Azure Service Fabric-toepassing maken
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric biedt SDK's voor het bouwen van services in Linux in zowel .NET Core als Java. In deze zelfstudie wordt behandeld hoe u een toepassing maakt voor Linux en een service bouwt met behulp van Java.

## <a name="prerequisites"></a>Vereisten
Zorg voordat u begint ervoor dat u [uw Linux-ontwikkelingsomgeving hebt ingesteld](service-fabric-get-started-linux.md). Als u Mac OS X gebruikt, kunt u [een Linux one-box omgeving instellen op een virtuele machine met behulp van Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>De toepassing maken
Een Service Fabric-toepassing kan een of meer services bevatten, elk met een specifieke functie met betrekking tot het leveren van de functionaliteit van de toepassing. De Service Fabric-SDK voor Linux bevat een [Yeoman](http://yeoman.io/)-generator waarmee u gemakkelijk uw eerste service kunt maken en er later meer kunt toevoegen. We gebruiken Yeoman om een nieuwe toepassing te maken met één service.

1. Typ in een terminal **yo azuresfjava**.
2. Geef uw toepassing een naam.
3. Kies het type van uw eerste service en geef de service een naam. In deze zelfstudie houden we het eenvoudig door een Reliable Actor Service te kiezen.
   
   ![Service Fabric Yeoman-generator voor Java][sf-yeoman]

> [!NOTE]
> Zie [Service Fabric programming model overview](service-fabric-choose-framework.md) (Overzicht van het Service Fabric-programmeermodel) voor meer informatie over de opties.
> 
> 

## <a name="build-the-application"></a>De toepassing bouwen
De Service Fabric Yeoman-sjablonen bevatten een bouwscript voor [Gradle](https://gradle.org/), dat u kunt gebruiken om de app via de terminal te maken.

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>De toepassing implementeren
Als de toepassing is gemaakt, kunt u deze kunt implementeren in het lokale cluster met behulp van de Azure CLI.

1. Maak verbinding met het lokale cluster van Service Fabric.
   
    ```bash
    azure servicefabric cluster connect
    ```
2. Gebruik het installatiescript dat is opgegeven in de sjabloon om het toepassingspakket te kopiëren naar de installatiekopieopslag van het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.
   
    ```bash
    ./install.sh
    ```
3. Open een browser en navigeer naar de Service Fabric Explorer op http://localhost:19080/Explorer (vervang localhost door het privé IP-adres van de virtuele machine als u Vagrant in Mac OS X gebruikt).
4. Vouw het knooppunt Toepassingen uit. U ziet dat er nu een vermelding is voor uw toepassingstype en nog een voor het eerste exemplaar van dat type.

## <a name="start-the-test-client-and-perform-a-failover"></a>De testclient starten en een failover uitvoeren
Actorprojecten doen niets uit zichzelf. Ze hebben een andere service of client nodig die hen berichten stuurt. De actorsjabloon bevat een eenvoudig testscript dat u kunt gebruiken om te communiceren met de actorservice.

1. Voer het script uit met behulp van het controleprogramma om de uitvoer van de actorservice te bekijken.
   
    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. Zoek in Service Fabric Explorer het knooppunt op dat als host fungeert voor de primaire replica voor de actorservice. In de onderstaande schermafbeelding is dit knooppunt 3.
   
    ![Zoeken naar de primaire replica in Service Fabric Explorer][sfx-primary]
3. Klik op het knooppunt dat u hebt gevonden in de vorige stap en selecteer vervolgens **Deactiveren (opnieuw starten)** in het menu Acties. Hiermee wordt een van de vijf knooppunten in uw lokale cluster opnieuw gestart en een failover afgedwongen op een van de secundaire replica's die worden uitgevoerd op een ander knooppunt. Let terwijl u dit doet op de uitvoer van de testclient en houd er rekening mee dat de teller blijft toenemen ondanks de failover.

## <a name="build-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Een toepassing maken en implementeren met behulp van de invoegtoepassing Eclipse Neon
Als u de Service-invoegtoepassing voor Eclipse Neon hebt geïnstalleerd, kunt u Service Fabric-toepassingen maken, bouwen en implementeren met behulp van Java.  Kies bij het installeren van Eclipse voor **Eclipse IDE voor Java-ontwikkelaars**.

### <a name="create-the-application"></a>De toepassing maken
De Service Fabric-invoegtoepassing is beschikbaar via Eclipse-uitbreidingsmogelijkheden.

1. Kies in Eclipse **File > Other > Service Fabric**. U ziet een set opties, waaronder Actors en Containers.
   
    ![Service Fabric-sjablonen in Eclipse][sf-eclipse-templates]
2. Kies in dit geval Stateless Service.
3. U wordt gevraagd het gebruik van het Service Fabric-perspectief te bevestigen, dat Eclipse optimaliseert voor gebruik met Service Fabric-projecten. Kies 'Yes'.

### <a name="deploy-the-application"></a>De toepassing implementeren
De Service Fabric-sjablonen bevatten een aantal Gradle-taken voor het maken en implementeren van toepassingen, die u via Eclipse kunt activeren.

1. Kies **Run > Run Configurations**.
2. Vouw **Gradle Project** uit en kies **ServiceFabricDeployer**.
3. Klik op **Run**.

Uw app wordt binnen enkele ogenblikken gemaakt en geïmplementeerd. U kunt de status bewaken vanuit Service Fabric Explorer.

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interactie aangaan met Service Fabric-clusters met de Azure-CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png



<!--HONumber=Nov16_HO2-->


