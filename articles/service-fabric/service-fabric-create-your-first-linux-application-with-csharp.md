---
title: Uw eerste Azure Service Fabric-app in Linux maken met C# | Microsoft Docs
description: Informatie over het maken en implementeren van een Service Fabric-toepassing met behulp van C# en .NET Core 2.0.
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/11/2018
ms.author: subramar
ms.openlocfilehash: 7bb9b1c545d30f66a09817ba977f12666a9f41c9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60394292"
---
# <a name="create-your-first-azure-service-fabric-application"></a>Uw eerste Azure Service Fabric-toepassing maken
> [!div class="op_single_selector"]
> * [Java - Linux (Preview)](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux (Preview)](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Service Fabric biedt SDK's voor het bouwen van services in Linux in zowel .NET Core als Java. In deze zelfstudie wordt behandeld hoe u een toepassing maakt voor Linux en een service bouwt met behulp van C# op .NET Core 2.0.

## <a name="prerequisites"></a>Vereisten
Zorg voordat u begint ervoor dat u [uw Linux-ontwikkelingsomgeving hebt ingesteld](service-fabric-get-started-linux.md). Als u Mac OS X gebruikt, kunt u [een Linux one-box omgeving instellen op een virtuele machine met behulp van Vagrant](service-fabric-get-started-mac.md).

Het is dan ook een goed idee om de [Service Fabric-CLI](service-fabric-cli.md) te installeren.

### <a name="install-and-set-up-the-generators-for-c"></a>Generatoren voor C# installeren en instellen
Service Fabric biedt hulpprogramma's waarmee u vanuit een terminal Service Fabric-toepassingen kunt maken met behulp van Yeoman-sjabloongeneratoren. Volg deze stappen om de Service Fabric Yeoman-sjabloongeneratoren in te stellen voor C#:

1. nodejs en NPM installeren op uw computer

   ```bash
   curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash 
   nvm install node 
   ```
2. [Yeoman](https://yeoman.io/)-sjabloongenerator installeren op uw computer vanuit NPM

   ```bash
   npm install -g yo
   ```
3. De generator voor Service Fabric Yeoman C#-toepassingen installeren vanuit NPM

   ```bash
   npm install -g generator-azuresfcsharp
   ```

## <a name="create-the-application"></a>De toepassing maken
Een Service Fabric-toepassing kan een of meer services bevatten, elk met een specifieke functie met betrekking tot het leveren van de functionaliteit van de toepassing. De Service Fabric [Yeoman](https://yeoman.io/)-generator voor C#, die u in de laatste stap hebt geïnstalleerd, zorgt ervoor dat u gemakkelijk uw eerste service kunt maken en er later meer kunt toevoegen. We gebruiken Yeoman om een toepassing te maken met één service.

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

1. Maak verbinding met het lokale cluster van Service Fabric.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Voer het installatiescript uit dat is opgegeven in de sjabloon om het toepassingspakket te kopiëren naar de installatiekopieopslag van het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.

    ```bash
    ./install.sh
    ```

De implementatie van de gemaakte toepassing werkt hetzelfde als van andere Service Fabric-toepassingen. Zie de documentatie over het [beheren van een Service Fabric-toepassing met de Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md) voor gedetailleerde instructies.

Parameters voor deze opdrachten vindt u in de gegenereerde manifesten binnen het toepassingspakket.

Nadat de toepassing is geïmplementeerd, opent u een browser en gaat u naar [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) op [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Vouw vervolgens het knooppunt **Toepassingen** uit. U ziet dat er nu een vermelding is voor uw toepassingstype en nog een voor het eerste exemplaar van dat type.

> [!IMPORTANT]
> Voor het implementeren van de toepassing met een beveiligd Linux-cluster in Azure, moet u een certificaat voor het valideren van uw toepassing met de Service Fabric-runtime configureren. In dat geval kunt uw Reliable Services-services om te communiceren met de onderliggende Service Fabric-runtime-API's. Zie voor meer informatie, [een Reliable Services-app uit te voeren op Linux-clusters configureren](./service-fabric-configure-certificates-linux.md#configure-a-reliable-services-app-to-run-on-linux-clusters).  
>

## <a name="start-the-test-client-and-perform-a-failover"></a>De testclient starten en een failover uitvoeren
Actorprojecten doen niets uit zichzelf. Ze hebben een andere service of client nodig die hen berichten stuurt. De actorsjabloon bevat een eenvoudig testscript dat u kunt gebruiken om te communiceren met de actorservice.

1. Voer het script uit met behulp van het controleprogramma om de uitvoer van de actorservice te bekijken.

   In het geval van MAC OS X moet u de map myactorsvcTestClient kopiëren naar dezelfde locatie in de container door het uitvoeren van de volgende aanvullende opdrachten.
    
    ```bash
    docker cp  [first-four-digits-of-container-ID]:/home
    docker exec -it [first-four-digits-of-container-ID] /bin/bash
    cd /home
    ```
    
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

* [Werken met Service Fabric-clusters via de Service Fabric-CLI](service-fabric-cli.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)
* [Aan de slag met de Service Fabric-CLI](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
