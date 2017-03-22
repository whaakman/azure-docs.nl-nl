---
title: Uw ontwikkelomgeving instellen in Mac OS X | Microsoft Docs
description: Installeer de runtime, SDK en hulpprogramma&quot;s en maak een lokaal ontwikkelcluster. Zodra u dit hebt gedaan, kunt u toepassingen bouwen in Mac OS X.
services: service-fabric
documentationcenter: java
author: saysa
manager: raunakp
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: fc73eedae7ec9664da714567f47a543e625cd023
ms.lasthandoff: 03/11/2017


---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Uw ontwikkelomgeving instellen in Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

U kunt Service Fabric-toepassingen bouwen voor uitvoering op Linux-clusters met behulp van Mac OS X. In dit artikel wordt uitgelegd hoe u uw Mac kunt instellen voor ontwikkeling.

## <a name="prerequisites"></a>Vereisten
Service Fabric wordt niet systeemeigen op OS X uitgevoerd. Als u een lokaal Service Fabric-cluster wilt uitvoeren, bieden we een vooraf geconfigureerde virtuele Ubuntu-machine met Vagrant en VirtualBox. Voordat u aan de slag gaat, hebt u het volgende nodig:

* [Vagrant (v1.8.4 of hoger)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
>  U moet wederzijds ondersteunde versies van Vagrant en VirtualBox gebruiken. Vagrant functioneert mogelijk niet goed in combinatie met een niet-ondersteunde versie van VirtualBox.
>

## <a name="create-the-local-vm"></a>De lokale virtuele machine maken
Ga als volgt te werk om de lokale VM te maken met een Service Fabric-cluster met 5-knooppunten:

1. Kloon de **Vagrantfile**-opslagplaats

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
2. Navigeer naar de lokale kloon van de opslagplaats

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. Wijzig de standaardinstellingen van de virtuele machine (optioneel)

    Standaard wordt de lokale virtuele machine als volgt geconfigureerd:

   * 3 GB toegewezen geheugen
   * Persoonlijk hostnetwerk geconfigureerd op IP 192.168.50.50 dat passthrough van verkeer van de Mac-host inschakelt

     U kunt een van deze instellingen wijzigen of een andere configuratie toevoegen aan de virtuele machine in de Vagrantfile. Zie de [Vagrant-documentatie](http://www.vagrantup.com/docs) voor de volledige lijst met configuratieopties.
4. De virtuele machine maken

    ```bash
    vagrant up
    ```

   In deze stap wordt de vooraf geconfigureerde VM-installatiekopie gedownload, lokaal opgestart en wordt er vervolgens een lokaal Service Fabric-cluster in ingesteld. Dit kan enkele minuten duren. Als de installatie is voltooid, ziet u een bericht in de uitvoer dat aangeeft dat de cluster wordt gestart.

    ![Starten van clusterinstallatie na inrichting van VM][cluster-setup-script]

5. Test of het cluster correct is ingesteld door naar Service Fabric Explorer te gaan op http://192.168.50.50:19080/Explorer (ervan uitgaande dat u het standaard IP-adres van het privénetwerk hebt gehouden).

    ![Service Fabric Explorer bekeken vanuit de host-Mac][sfx-mac]

## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>De Service Fabric-invoegtoepassing installeren voor Eclipse Neon

Service Fabric biedt een invoegtoepassing voor de **Eclipse Neon voor Java IDE** die het proces van het maken, bouwen en implementeren van Java-services kan vereenvoudigen. U kunt de installatiestappen volgen uit deze algemene [documentatie](service-fabric-get-started-eclipse.md#install-or-update-service-fabric-plugin-on-eclipse-neon) over het installeren en bijwerken van de Service Fabric Eclipse-invoegtoepassing.

## <a name="using-service-fabric-eclipse-plugin-on-mac"></a>De Service Fabric Eclipse-invoegtoepassing gebruiken op Mac-apparaten

Zorg ervoor dat u de stappen uit de [documentatie over de Service Fabric Eclipse-invoegtoepassing](service-fabric-get-started-eclipse.md) hebt gevolgd. De stappen voor het maken, bouwen en implementeren van een Service Fabric Java-toepassing met behulp van een Vagrant-gastcontainer op een Mac-host zijn vrijwel hetzelfde als in de algemene documentatie wordt beschreven, op een paar punten na. Deze punten worden hieronder beschreven.
* Omdat de Service Fabric-bibliotheken zijn vereist om de Service Fabric Java-toepassing te kunnen bouwen, moet het Eclipse-project worden gemaakt in een gedeeld pad. Standaard wordt de inhoud in het pad van de host waar ``Vagrantfile`` staat, gedeeld met het ``/vagrant``-pad voor de gast.
* Eenvoudig gezegd: als u ``Vagrantfile`` in een pad hebt staan, zoals ``~/home/john/allprojects/``, moet u het Service Fabric-project ``MyActor`` op de locatie ``~/home/john/allprojects/MyActor`` maken, en wordt het pad naar uw Eclipse-werkruimte ``~/home/john/allprojects``.

## <a name="next-steps"></a>Volgende stappen
<!-- Links -->
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Een Service Fabric-cluster maken in Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Een Service Fabric-cluster maken met behulp van de Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Inzicht krijgen in het Service Fabric-toepassingsmodel](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

