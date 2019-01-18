---
title: Instellen van Azure Service Fabric Linux-cluster in Windows | Microsoft Docs
description: In dit artikel bevat informatie over het instellen van Service Fabric Linux-clusters die worden uitgevoerd op Windows development-machines. Dit is vooral handig voor cross platformontwikkeling.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: mfussell
editor: ''
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/20/2017
ms.author: suhuruli
ms.openlocfilehash: 26acb3a2a0cefdca74d2c761ccddf89e18aa909a
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54388495"
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Instellen van een Linux Service Fabric-cluster op uw Windows-machine voor ontwikkelaars

In dit document bevat informatie over het instellen van een lokaal Service Fabric voor Linux op virtuele machines ontwikkeling van Windows. Instellen van een lokaal cluster voor Linux is handig voor het snel testen van toepassingen voor Linux-clusters die zijn gericht, maar zijn ontwikkeld op een Windows-machine.

## <a name="prerequisites"></a>Vereisten
Service Fabric-clusters op basis van Linux, niet systeemeigen worden uitgevoerd op Windows. Voor het uitvoeren van een lokaal Service Fabric-cluster, krijgt u een vooraf geconfigureerde Docker-containerinstallatiekopie. Voordat u aan de slag gaat, hebt u het volgende nodig:

* Ten minste 4 GB RAM-geheugen
* Nieuwste versie van [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)

>[!TIP]
> * U kunt de stappen die worden vermeld in de officiële Docker [documentatie](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) om Docker te installeren op uw Windows. 
> * Wanneer u klaar bent met de installatie, controleert u of de installatie goed is verlopen door [deze stappen](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine) uit te voeren


## <a name="create-a-local-container-and-setup-service-fabric"></a>Een lokale container maken en Service Fabric configureren
Als u wilt een lokale Docker-container instellen en daarop een service fabric-cluster die erop worden uitgevoerd, moet u de volgende stappen uitvoeren in PowerShell:


1. Werk de configuratie van de Docker-daemon op uw host bij met het volgende en start de Docker-daemon opnieuw op: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    De aanbevolen manier om bij te werken is: Ga naar Docker-pictogram > Instellingen > Daemon > Geavanceerd en werk deze. Start vervolgens opnieuw op de Docker-daemon voor de wijzigingen worden doorgevoerd. 

2. Maak in een nieuwe map een bestand met de naam `Dockerfile` om uw Service Fabric Image te maken:

    ```dockerfile
    FROM microsoft/service-fabric-onebox
    WORKDIR /home/ClusterDeployer
    RUN ./setup.sh
    #Generate the local
    RUN locale-gen en_US.UTF-8
    #Set environment variables
    ENV LANG=en_US.UTF-8
    ENV LANGUAGE=en_US:en
    ENV LC_ALL=en_US.UTF-8
    EXPOSE 19080 19000 80 443
    #Start SSH before running the cluster
    CMD /etc/init.d/ssh start && ./run.sh
    ```

    >[!NOTE]
    >U kunt het bestand aanpassen zodat aanvullende programma's of afhankelijkheden aan uw container kunnen worden toegevoegd.
    >Bijvoorbeeld: het toevoegen van `RUN apt-get install nodejs -y` biedt ondersteuning voor `nodejs`-toepassingen als uitvoerbare gastbestanden.
    
    >[!TIP]
    > Standaard wordt hierdoor de installatiekopie met de nieuwste versie van Service Fabric opgehaald. Ga naar de [Docker Hub](https://hub.docker.com/r/microsoft/service-fabric-onebox/)-pagina als u een bepaalde revisie wilt ophalen

3. Als u uw herbruikbare installatiekopie wilt bouwen vanaf `Dockerfile`, opent u een terminal en gaat u met de opdracht `cd` naar de map met uw `Dockerfile`. Voer vervolgende de volgende opdracht uit:

    ```powershell 
    docker build -t mysfcluster .
    ```
    
    >[!NOTE]
    >Deze bewerking kan enige tijd duren, maar hoeft slechts eenmaal te worden uitgevoerd.

4. Nu kunt u snel een lokale kopie van Service Fabric starten zodra u deze nodig hebt. Voer hiertoe de volgende opdracht uit:

    ```powershell 
    docker run --name sftestcluster -d -v //var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mysfcluster
    ```

    >[!TIP]
    >Door een naam op te geven voor uw instantie van de container, kunt u deze op een beter leesbare manier verwerken. 
    >
    >Als uw toepassing op bepaalde poorten luistert, moeten de poorten worden opgegeven met behulp van aanvullende `-p` labels. Bijvoorbeeld, als uw toepassing op poort 8080 luistert, voeg dan de volgende `-p` tag toe:
    >
    >`docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox microsoft/service-fabric-onebox`
    >

5. Het duurt even voor het cluster is gestart. U kunt logboeken bekijken via de volgende opdracht of naar het dashboard gaan om de status van het cluster, [http://localhost:19080](http://localhost:19080), te bekijken:

    ```powershell 
    docker logs sftestcluster
    ```

6. Nadat stap 5 is voltooid, gaat u naar ``http://localhost:19080`` van uw Windows en u zou kunnen om te zien van de Service Fabric explorer. U kunt op dit moment verbinding maken met dit cluster met behulp van hulpprogramma's van uw Windows-machine voor ontwikkelaars en implementeren van toepassingen die zijn gericht voor Linux Service Fabric-clusters. 

    > [!NOTE]
    > De Eclipse-invoegtoepassing wordt momenteel niet ondersteund in Windows. 

7. Wanneer u klaar bent, stoppen en opschonen de container met de volgende opdracht:

    ```powershell 
    docker rm -f sftestcluster
    ```

### <a name="known-limitations"></a>Bekende beperkingen 
 
 Hier volgen bekende beperkingen van het uitvoeren van het lokale cluster in een container voor Mac-computers: 
 
 * DNS-service kan niet worden uitgevoerd en wordt niet ondersteund [Probleem #132](https://github.com/Microsoft/service-fabric/issues/132)

## <a name="next-steps"></a>Volgende stappen
* Aan de slag met [Eclipse](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-eclipse)
* Bekijk andere [Java-voorbeelden](https://github.com/Azure-Samples/service-fabric-java-getting-started)


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png