---
title: Uw eerste Azure-microservices-app in Linux maken met behulp van C# | Microsoft Docs
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
ms.date: 8/9/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 4baf144cc28eeff0ab8f8b60e837f8a2bad903af
ms.contentlocale: nl-nl
ms.lasthandoff: 07/01/2017

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

U configureert ook [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (aanbevolen) of [XPlat CLI](service-fabric-azure-cli.md) voor het implementeren van uw toepassing.

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

Nadat de toepassing is gemaakt, kunt u deze implementeren in het lokale cluster.

### <a name="using-xplat-cli"></a>Met XPlat CLI

1. Maak verbinding met het lokale cluster van Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Voer het installatiescript uit dat is opgegeven in de sjabloon om het toepassingspakket te kopiëren naar de installatiekopieopslag van het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.

    ```bash
    ./install.sh
    ```

### <a name="using-azure-cli-20"></a>Met Azure CLI 2.0

De implementatie van de gemaakte toepassing werkt hetzelfde als van andere Service Fabric-toepassingen. Zie de documentatie over het [beheren van een Service Fabric-toepassing met Azure CLI](service-fabric-application-lifecycle-azure-cli-2-0.md) voor gedetailleerde instructies.

Parameters voor deze opdrachten vindt u in de gegenereerde manifesten binnen het toepassingspakket.

Nadat de toepassing is geïmplementeerd, opent u een browser en gaat u naar [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) op [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Vouw vervolgens het knooppunt **Toepassingen** uit. U ziet dat er nu een vermelding is voor uw toepassingstype en nog een voor het eerste exemplaar van dat type.

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

## <a name="migrating-from-projectjson-to-csproj"></a>Migreren van project.json naar .csproj
1. Als u dotnet migrate uitvoert in de hoofdmap van het project, worden alle project.json gemigreerd naar de indeling csproj.
2. Werk de projectverwijzingen in de projectbestanden op basis hiervan bij naar csproj-bestanden.
3. Werk de projectbestandsnamen bij naar csproj-bestanden in build.sh.

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interactie aangaan met Service Fabric-clusters met de Azure-CLI](service-fabric-azure-cli.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

## <a name="related-articles"></a>Verwante artikelen:

* [Aan de slag met Service Fabric en Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Aan de slag met Service Fabric XPlat CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png

