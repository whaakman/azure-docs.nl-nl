---
title: Instellen van Azure Service Fabric Linux-cluster in Windows | Microsoft Docs
description: In dit artikel bevat informatie over het instellen van Service Fabric Linux-clusters die zijn uitgevoerd op de Windows development-machines. Dit is bijzonder nuttig voor cross-platform-ontwikkeling.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: mfussell
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/20/2017
ms.author: suhuruli
ms.openlocfilehash: f21561269e90e3643ef5d8d48ee28712ee7f611c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Instellen van een Linux-Service Fabric-cluster op uw Windows-machine voor ontwikkelaars

Dit document bevat informatie over het instellen van een lokale Linux-Service Fabric-ontwikkeling op machines in Windows. Instellen van een lokale Linux-cluster is handig voor het snel testen van toepassingen voor Linux-clusters die zijn gericht, maar zijn ontwikkeld op een Windows-machine.

## <a name="prerequisites"></a>Vereisten
Service Fabric-clusters op basis van Linux niet standaard uitgevoerd in Windows. Als u wilt uitvoeren op een lokale Service Fabric-cluster, krijgt u de installatiekopie van een vooraf geconfigureerde Docker-container. Voordat u aan de slag gaat, hebt u het volgende nodig:

* Ten minste 4 GB RAM-geheugen
* Nieuwste versie van [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)
* Toegang tot Service Fabric One-box Docker-[containerinstallatiekopie](https://hub.docker.com/r/servicefabricoss/service-fabric-onebox/)

>[!TIP]
> * U kunt de stappen die worden vermeld in de officiële Docker [documentatie](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) Docker op uw Windows installeren. 
> * Wanneer u klaar bent met de installatie, controleert u of de installatie goed is verlopen door [deze stappen](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine) uit te voeren


## <a name="create-a-local-container-and-setup-service-fabric"></a>Een lokale container maken en Service Fabric configureren
Als u een lokale Docker-container wilt instellen en daarop een Service Fabric-cluster wilt uitvoeren, voert u de volgende stappen uit:

1. Haal de installatiekopie op uit de opslagplaats op de Docker-hub:

    ```powershell
    docker pull servicefabricoss/service-fabric-onebox
    ```

2. Werk de configuratie van de Docker-daemon op uw host bij met het volgende en start de Docker-daemon opnieuw op: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    De advised manier om bij te werken is - gaat u naar de Docker-pictogram > Instellingen > Daemon > geavanceerde en er bijwerken. Start vervolgens opnieuw de Docker-daemon om de wijzigingen van kracht te laten worden. 

3. Start een instantie van een Service Fabric-One-box-container met de installatiekopie:

    ```powershell
    docker run -itd -p 19080:19080 --name sfonebox servicefabricoss/service-fabric-onebox
    ```
    >[!TIP]
    > * Door een naam op te geven voor uw instantie van de container, kunt u deze op een beter leesbare manier verwerken. 
    > * Als uw toepassing op bepaalde poorten luistert, moeten deze worden opgegeven met extra -p-tags. Bijvoorbeeld: als uw toepassing luistert op poort 8080, voert u deze opdracht uit: docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox servicefabricoss/service-fabric-onebox

4. Meld u aan bij de Docker-container in interactieve ssh-modus:

    ```powershell
    docker exec -it sfonebox bash
    ```

5. Voer het installatiescript uit om de vereiste afhankelijkheden op te halen en start daarna het cluster op de container.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

6. Nadat u stap 5 is voltooid, gaat u naar ``http://localhost:19080`` van uw Windows- en u zou kunnen zien van de Service Fabric explorer. U kunt op dit moment verbinding maken met dit cluster met behulp van hulpprogramma's van uw Windows-computer voor ontwikkelaars en implementeren van toepassingen die zijn gericht voor Linux Service Fabric-clusters. 

    > [!NOTE]
    > De Eclipse-invoegtoepassing wordt momenteel niet ondersteund in Windows. 

## <a name="next-steps"></a>Volgende stappen
* Aan de slag met [Eclipse](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-eclipse)
* Bekijk andere [Java-voorbeelden](https://github.com/Azure-Samples/service-fabric-java-getting-started)


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png