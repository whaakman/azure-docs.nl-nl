---
title: Uw eerste Azure-microservices-app in Linux maken met behulp van Java | Microsoft Docs
description: Een Service Fabric-toepassing maken en implementeren met behulp van Java
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/02/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: e229602b4bfa72977c9b15e854d796ed09fa55d2
ms.contentlocale: nl-nl
ms.lasthandoff: 07/01/2017


---
<a id="create-your-first-service-fabric-java-application-on-linux" class="xliff"></a>

# Uw eerste Service Fabric Java-toepassing maken op Linux
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Met deze Quick Start kunt u in een paar minuten uw eerste Azure Service Fabric Java-toepassing maken in een Linux-ontwikkelomgeving.  Wanneer u klaar bent, hebt u een eenvoudige Java-app voor één service die wordt uitgevoerd op het lokale ontwikkelcluster.  

<a id="prerequisites" class="xliff"></a>

## Vereisten
Voordat u aan de slag gaat, installeert u de Service Fabric SDK en Azure CLI en stelt u een ontwikkelingscluster in uw [Linux-ontwikkelomgeving](service-fabric-get-started-linux.md) in. Als u Mac OS X gebruikt, kunt u [een Linux-ontwikkelomgeving instellen op een virtuele machine met behulp van Vagrant](service-fabric-get-started-mac.md).

U configureert ook [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (aanbevolen) of [XPlat CLI](service-fabric-azure-cli.md) voor het implementeren van uw toepassing.

<a id="create-the-application" class="xliff"></a>

## De toepassing maken
Een Service Fabric-toepassing bevat een of meer services, elk met een specifieke functie met betrekking tot het leveren van de functionaliteit van de toepassing. De Service Fabric-SDK voor Linux bevat een [Yeoman](http://yeoman.io/)-generator waarmee u gemakkelijk uw eerste service kunt maken en er later meer kunt toevoegen.  U kunt ook Service Fabric-Java-toepassingen maken, ontwikkelen en implementeren met behulp van een invoegtoepassing voor Eclipse. Zie [Uw eerste Java-toepassing maken en implementeren met behulp van Eclipse](service-fabric-get-started-eclipse.md). In deze Quick Start gebruikt u Yeoman om een toepassing met één service te maken waarmee een tellerwaarde wordt opgeslagen en opgehaald.

1. Typ in een terminal ``yo azuresfjava``.
2. Geef uw toepassing een naam. 
3. Kies het type van uw eerste service en geef de service een naam. In deze zelfstudie kiest u voor een Reliable Actor Service. Zie [Service Fabric programming model overview](service-fabric-choose-framework.md) (Overzicht van het Service Fabric-programmeermodel) voor meer informatie over de andere typen services.
   ![Service Fabric Yeoman-generator voor Java][sf-yeoman]

<a id="build-the-application" class="xliff"></a>

## De toepassing bouwen
De Service Fabric Yeoman-sjablonen bevatten een bouwscript voor [Gradle](https://gradle.org/), dat u kunt gebruiken om de app via de terminal te maken. Ga als volgt te werk om de app te maken en in te pakken:

  ```bash
  cd myapp
  gradle
  ```

<a id="deploy-the-application" class="xliff"></a>

## De toepassing implementeren
Nadat de toepassing is gemaakt, kunt u deze implementeren in het lokale cluster.

<a id="using-xplat-cli" class="xliff"></a>

### Met XPlat CLI

1. Maak verbinding met het lokale cluster van Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Voer het installatiescript uit dat is opgegeven in de sjabloon om het toepassingspakket te kopiëren naar de installatiekopieopslag van het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.

    ```bash
    ./install.sh
    ```

<a id="using-azure-cli-20" class="xliff"></a>

### Met Azure CLI 2.0

De implementatie van de gemaakte toepassing werkt hetzelfde als van andere Service Fabric-toepassingen. Zie de documentatie over het [beheren van een Service Fabric-toepassing met Azure CLI](service-fabric-application-lifecycle-azure-cli-2-0.md) voor gedetailleerde instructies.

Parameters voor deze opdrachten vindt u in de gegenereerde manifesten binnen het toepassingspakket.

Nadat de toepassing is geïmplementeerd, opent u een browser en gaat u naar [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) op [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Vouw vervolgens het knooppunt **Toepassingen** uit. U ziet dat er nu een vermelding is voor uw toepassingstype en nog een voor het eerste exemplaar van dat type.

<a id="start-the-test-client-and-perform-a-failover" class="xliff"></a>

## De testclient starten en een failover uitvoeren
Actoren doen niets uit zichzelf, ze hebben een andere service of client nodig die hen berichten stuurt. De actorsjabloon bevat een eenvoudig testscript dat u kunt gebruiken om te communiceren met de actorservice.

1. Voer het script uit met behulp van het controleprogramma om de uitvoer van de actorservice te bekijken.  Via het testscript wordt de methode `setCountAsync()` op de actor aangeroepen om een teller te verhogen, wordt de methode `getCountAsync()` op de actor aangeroepen om de nieuwe tellerwaarde op te halen en wordt deze waarde weergegeven op de console.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Zoek in Service Fabric Explorer het knooppunt op dat als host fungeert voor de primaire replica voor de actorservice. In de onderstaande schermafbeelding is dit knooppunt 3. Via de primaire servicereplica worden lees- en schrijfbewerkingen verwerkt.  Wijzigingen in de servicestatus worden vervolgens gerepliceerd naar de secundaire replica's die worden uitgevoerd op knooppunt 0 en 1 in de schermafbeelding hieronder.

    ![Zoeken naar de primaire replica in Service Fabric Explorer][sfx-primary]

3. Klik in **Knooppunten** op het knooppunt dat u hebt gevonden in de vorige stap en selecteer **Deactiveren (opnieuw starten)** in het menu Acties. Met deze actie wordt het knooppunt opnieuw gestart waarop de primaire servicereplica wordt uitgevoerd en wordt een failover afgedwongen op een van de secundaire replica's die worden uitgevoerd op een ander knooppunt.  Deze secundaire replica wordt een niveau verhoogd naar primair, er wordt nog een secundaire replica gemaakt op een ander knooppunt en de primaire replica begint met het uitvoeren van lees-/ schrijfbewerkingen. Terwijl het knooppunt opnieuw wordt gestart, ziet u in de uitvoer van de testclient dat de teller blijft toenemen ondanks de failover.

<a id="add-another-service-to-the-application" class="xliff"></a>

## Nog een service toevoegen aan de toepassing
Voer de volgende stappen uit als u nog een service wilt toevoegen aan een bestaande toepassing met `yo`:
1. Stel de directory in op de hoofdmap van de bestaande toepassing.  Bijvoorbeeld `cd ~/YeomanSamples/MyApplication` als `MyApplication` de toepassing is die is gemaakt door Yeoman.
2. Voer `yo azuresfjava:AddService` uit.
3. Maak en implementeer de app, zoals in de voorgaande stappen is beschreven.

<a id="remove-the-application" class="xliff"></a>

## De toepassing verwijderen
Gebruik het uninstall-script dat is opgegeven in de sjabloon om het toepassingsexemplaar te verwijderen, de registratie van het toepassingspakket op te heffen en het toepassingspakket te verwijderen uit de installatiekopieopslag van het cluster.

```bash
./uninstall.sh
```

In Service Fabric Explorer ziet u dat de toepassing en het toepassingstype niet meer worden weergegeven in het knooppunt **Toepassingen**.

<a id="next-steps" class="xliff"></a>

## Volgende stappen
* [Uw eerste Service Fabric Java-toepassing maken op Linux met Eclipse](service-fabric-get-started-eclipse.md)
* [Meer informatie over Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Werken met Service Fabric-clusters via Azure CLI](service-fabric-azure-cli.md)
* [Problemen met implementatie oplossen](service-fabric-azure-cli.md#troubleshooting)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

<a id="related-articles" class="xliff"></a>

## Verwante artikelen:

* [Aan de slag met Service Fabric en Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Aan de slag met Service Fabric XPlat CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

