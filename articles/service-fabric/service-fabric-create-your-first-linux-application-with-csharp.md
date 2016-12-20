---
title: Uw eerste Service Fabric-toepassing in Linux maken met behulp van C# | Microsoft Docs
description: Een Service Fabric-toepassing maken en implementeren met behulp van C#
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2016
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: f31c8ab3d2b4fd871c92ac0e7d04bc8d5ab86830
ms.openlocfilehash: 046cc1286d894e28ed4d560c7c0b815f582e1e77


---
# <a name="create-your-first-azure-service-fabric-application"></a>Uw eerste Azure Service Fabric-toepassing maken
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric biedt SDK's voor het bouwen van services in Linux in zowel .NET Core als Java. In deze zelfstudie wordt behandeld hoe u een toepassing maakt voor Linux en een service bouwt met behulp van C# (.NET Core).

## <a name="prerequisites"></a>Vereisten
Zorg voordat u begint ervoor dat u [uw Linux-ontwikkelingsomgeving hebt ingesteld](service-fabric-get-started-linux.md). Als u Mac OS X gebruikt, kunt u [een Linux one-box omgeving instellen op een virtuele machine met behulp van Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>De toepassing maken
Een Service Fabric-toepassing kan een of meer services bevatten, elk met een specifieke functie met betrekking tot het leveren van de functionaliteit van de toepassing. De Service Fabric-SDK voor Linux bevat een [Yeoman](http://yeoman.io/)-generator waarmee u gemakkelijk uw eerste service kunt maken en er later meer kunt toevoegen. We gebruiken Yeoman om een toepassing te maken met één service.

1. Typ in een terminal de volgende opdracht om te beginnen met de opbouw van de sjabloon: `yo azuresfcsharp`
2. Geef uw toepassing een naam.
3. Kies het type van uw eerste service en geef de service een naam. In deze zelfstudie kiezen we voor een Reliable Actor Service.
   
   ![Service Fabric Yeoman-generator voor C#][sf-yeoman]

> [!NOTE]
> Zie [Service Fabric programming model overview](service-fabric-choose-framework.md) (Overzicht van het Service Fabric-programmeermodel) voor meer informatie over de opties.
> 
> 

## <a name="build-the-application"></a>De toepassing bouwen
De Service Fabric Yeoman-sjablonen bevatten een bouwscript dat u kunt gebruiken om de app via de terminal te maken (na het navigeren naar de toepassingsmap).

  ```sh
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>De toepassing implementeren
Als de toepassing is gemaakt, kunt u deze kunt implementeren in het lokale cluster met behulp van de Azure CLI.

1. Maak verbinding met het lokale cluster van Service Fabric.
   
    ```sh
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
3. Klik op het knooppunt dat u hebt gevonden in de vorige stap en selecteer vervolgens **Deactiveren (opnieuw starten)** in het menu Acties. Deze actie start één knooppunt in het lokale cluster opnieuw op om een failover af te dwingen naar een secundaire replica die wordt uitgevoerd op een ander knooppunt. Let terwijl u deze actie uitvoert op de uitvoer van de testclient en houd er rekening mee dat de teller blijft toenemen ondanks de failover.



## <a name="adding-more-services-to-an-existing-application"></a>Meer services toevoegen aan een bestaande toepassing

Voer de volgende stappen uit als u nog een service wilt toevoegen aan een toepassing die al is gemaakt met `yo`: 
1. Stel de directory in op de hoofdmap van de bestaande toepassing.  Bijvoorbeeld `cd ~/YeomanSamples/MyApplication` als `MyApplication` de toepassing is die is gemaakt door Yeoman.
2. Voer `yo azuresfcsharp:AddService` uit.




## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interactie aangaan met Service Fabric-clusters met de Azure-CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png



<!--HONumber=Dec16_HO1-->


