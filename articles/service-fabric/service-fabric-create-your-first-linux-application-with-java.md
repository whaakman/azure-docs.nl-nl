---
title: Een betrouwbare Azure Service Fabric Java-actortoepassing maken in Linux | Microsoft Docs
description: Lees hoe u een betrouwbare Service Fabric Java-actortoepassing in vijf minuten kunt maken en implementeren.
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
ms.date: 06/29/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 254f38a600ea4026120bc411368eeb01310e56b2
ms.contentlocale: nl-nl
ms.lasthandoff: 07/15/2017

---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Uw eerste betrouwbare Service Fabric Java-actortoepassing maken in Linux

Met deze snelstartgieds kunt u in slechts een paar minuten uw eerste Azure Service Fabric Java-toepassing maken in een Linux-ontwikkelomgeving.  Wanneer u klaar bent, hebt u een eenvoudige Java-toepassing voor één service die wordt uitgevoerd op het lokale ontwikkelcluster.  

## <a name="prerequisites"></a>Vereisten
Voordat u aan de slag gaat, installeert u de Service Fabric SDK en Azure CLI en stelt u een ontwikkelingscluster in uw [Linux-ontwikkelomgeving](service-fabric-get-started-linux.md) in. Als u Mac OS X gebruikt, kunt u [een Linux-ontwikkelomgeving instellen op een virtuele machine met behulp van Vagrant](service-fabric-get-started-mac.md).

U configureert ook [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (aanbevolen) of [XPlat CLI](service-fabric-azure-cli.md) voor het implementeren van uw toepassing.

## <a name="create-the-application"></a>De toepassing maken
Een Service Fabric-toepassing bevat een of meer services, elk met een specifieke functie met betrekking tot het leveren van de functionaliteit van de toepassing. De Service Fabric-SDK voor Linux bevat een [Yeoman](http://yeoman.io/)-generator waarmee u gemakkelijk uw eerste service kunt maken en er later meer kunt toevoegen.  U kunt ook Service Fabric-Java-toepassingen maken, ontwikkelen en implementeren met behulp van een invoegtoepassing voor Eclipse. Zie [Uw eerste Java-toepassing maken en implementeren met behulp van Eclipse](service-fabric-get-started-eclipse.md). In deze Quick Start gebruikt u Yeoman om een toepassing met één service te maken waarmee een tellerwaarde wordt opgeslagen en opgehaald.

1. Typ in een terminal ``yo azuresfjava``.
2. Geef uw toepassing een naam. 
3. Kies het type van uw eerste service en geef de service een naam. In deze zelfstudie kiest u voor een Reliable Actor Service. Zie [Service Fabric programming model overview](service-fabric-choose-framework.md) (Overzicht van het Service Fabric-programmeermodel) voor meer informatie over de andere typen services.
   ![Service Fabric Yeoman-generator voor Java][sf-yeoman]

## <a name="build-the-application"></a>De toepassing bouwen
De Service Fabric Yeoman-sjablonen bevatten een bouwscript voor [Gradle](https://gradle.org/), dat u kunt gebruiken om de toepassing via de terminal te maken. Ga als volgt te werk om de toepassing te maken en in te pakken:

  ```bash
  cd myapp
  gradle
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
Actoren doen niets uit zichzelf, ze hebben een andere service of client nodig die hen berichten stuurt. De actorsjabloon bevat een eenvoudig testscript dat u kunt gebruiken om te communiceren met de actorservice.

1. Voer het script uit met behulp van het controleprogramma om de uitvoer van de actorservice te bekijken.  Via het testscript wordt de methode `setCountAsync()` op de actor aangeroepen om een teller te verhogen, wordt de methode `getCountAsync()` op de actor aangeroepen om de nieuwe tellerwaarde op te halen en wordt deze waarde weergegeven op de console.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Zoek in Service Fabric Explorer het knooppunt op dat als host fungeert voor de primaire replica voor de actorservice. In de onderstaande schermafbeelding is dit knooppunt 3. Via de primaire servicereplica worden lees- en schrijfbewerkingen verwerkt.  Wijzigingen in de servicestatus worden vervolgens gerepliceerd naar de secundaire replica's die worden uitgevoerd op knooppunt 0 en 1 in de schermafbeelding hieronder.

    ![Zoeken naar de primaire replica in Service Fabric Explorer][sfx-primary]

3. Klik in **Knooppunten** op het knooppunt dat u hebt gevonden in de vorige stap en selecteer **Deactiveren (opnieuw starten)** in het menu Acties. Met deze actie wordt het knooppunt opnieuw gestart waarop de primaire servicereplica wordt uitgevoerd en wordt een failover afgedwongen op een van de secundaire replica's die worden uitgevoerd op een ander knooppunt.  Deze secundaire replica wordt een niveau verhoogd naar primair, er wordt nog een secundaire replica gemaakt op een ander knooppunt en de primaire replica begint met het uitvoeren van lees-/ schrijfbewerkingen. Terwijl het knooppunt opnieuw wordt gestart, ziet u in de uitvoer van de testclient dat de teller blijft toenemen ondanks de failover.

## <a name="remove-the-application"></a>De toepassing verwijderen
Gebruik het uninstall-script dat is opgegeven in de sjabloon om het toepassingsexemplaar te verwijderen, de registratie van het toepassingspakket op te heffen en het toepassingspakket te verwijderen uit de installatiekopieopslag van het cluster.

```bash
./uninstall.sh
```

In Service Fabric Explorer ziet u dat de toepassing en het toepassingstype niet meer worden weergegeven in het knooppunt **Toepassingen**.

## <a name="next-steps"></a>Volgende stappen
* [Uw eerste Service Fabric Java-toepassing maken op Linux met Eclipse](service-fabric-get-started-eclipse.md)
* [Meer informatie over Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Werken met Service Fabric-clusters via Azure CLI](service-fabric-azure-cli.md)
* [Problemen met implementatie oplossen](service-fabric-azure-cli.md#troubleshooting)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

## <a name="related-articles"></a>Verwante artikelen:

* [Aan de slag met Service Fabric en Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Aan de slag met Service Fabric XPlat CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

