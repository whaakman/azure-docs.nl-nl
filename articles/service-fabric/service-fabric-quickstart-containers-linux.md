---
title: Een Linux-containertoepassing voor Azure Service Fabric maken in Azure | Microsoft Docs
description: In deze snelstartgids maakt u een Docker-installatiekopie met uw toepassing, pusht u de installatiekopie naar een containerregister en implementeert u de container vervolgens naar een Service Fabric-cluster.
services: service-fabric
documentationcenter: linux
author: suhuruli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/11/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: b0ded0fb274f6b64935ddaba75abf23a94063120
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109549"
---
# <a name="quickstart-deploy-linux-containers-to-service-fabric"></a>Snelstartgids: Linux-containers implementeren op Service Fabric

Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u schaalbare en betrouwbare microservices en containers implementeert en beheert.

In deze Quick Start leert u hoe u Linux-containers kunt implementeren in een Service Fabric-cluster. Nadat dit is voltooid, beschikt u over een stemtoepassing die bestaat uit een Python-web-front-end en een Redis-back-end. Beide worden uitgevoerd in een Service Fabric-cluster. U leert ook hoe u failover voor een toepassing kunt gebruiken en een toepassing in uw cluster kunt schalen.

![Webpagina van stemtoepassing][quickstartpic]

In deze snelstart gebruikt u de Bash-omgeving in Azure Cloud Shell om Service Fabric CLI-opdrachten uit te voeren. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als dit de eerste keer is dat u Cloud Shell uitvoert, wordt u gevraagd uw `clouddrive`-bestandsshare in te stellen. U kunt de standaardinstellingen accepteren of een bestaande bestandsshare toevoegen. Zie [Een `clouddrive`bestandsshare instellen](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share) voor meer informatie.

## <a name="get-the-application-package"></a>Het toepassingspakket ophalen

Als u containers wilt implementeren op Service Fabric, hebt u een set manifestbestanden nodig (de toepassingsdefinitie), waarin de afzonderlijke containers en de toepassing worden beschreven.

Gebruik git in de Cloud Shell om een kopie van de toepassingsdefinitie te klonen. Wijzig vervolgens mappen in de `Voting`-map in de kloon.

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/Voting
```

## <a name="create-a-service-fabric-cluster"></a>Een Service Fabric-cluster maken

Voor het implementeren van de toepassing in Azure hebt u een Service Fabric-cluster nodig om de toepassing uit te voeren. Party-clusters bieden een eenvoudige manier om snel een Service Fabric-cluster te maken. Party-clusters zijn kosteloze, tijdelijke Service Fabric-clusters die worden gehost in Azure en worden uitgevoerd door het Service Fabric-team. U kunt party-clusters gebruiken om toepassingen te implementeren en meer te weten te komen over het platform. Het cluster gebruikt één zelfondertekend certificaat voor beveiliging van knooppunt-naar-knooppunt en client-naar-knooppunt.

Meld u aan en neem deel aan een [Linux-cluster](http://aka.ms/tryservicefabric). Download het PFX-certificaat naar uw computer door op de koppeling **PFX** te klikken. Klik op de koppeling **Leesmij** om het certificaatwachtwoord te vinden en instructies te krijgen over het configureren van verschillende omgevingen voor gebruik van het certificaat. Laat de pagina's **Welkom** en **Leesmij** geopend, want u hebt enkele instructies in de volgende stappen nodig.

> [!Note]
> Er zijn per uur een beperkt aantal party-clusters beschikbaar. Als er een fout optreedt wanneer u zich probeert aan te melden voor een party-cluster, kunt u een bepaalde tijd wachten en het opnieuw proberen, maar u kunt ook deze stappen in [Een Service Fabric-cluster in Azure maken](service-fabric-tutorial-create-vnet-and-linux-cluster.md) volgen om een cluster in uw abonnement te maken.
>
>Als u toch uw eigen cluster maakt, houd er dan rekening mee dat de web-front-endservice is geconfigureerd om te luisteren op poort 80 naar binnenkomend verkeer. Zorg ervoor dat de poort is geopend in het cluster. (Als u een party-cluster gebruikt, is deze poort geopend.)
>

## <a name="configure-your-environment"></a>Uw omgeving configureren

Service Fabric biedt verschillende hulpmiddelen waarmee u een cluster en de bijbehorende toepassingen kunt beheren:

- Service Fabric Explorer, een op een browser gebaseerd hulpprogramma.
- Service Fabric Command Line Interface (CLI), die wordt uitgevoerd boven op Azure CLI 2.0.
- PowerShell-opdrachten.

In deze snelstart gebruikt u de Service Fabric CLI in Cloud Shell en Service Fabric Explorer. De volgende secties laten zien hoe u met behulp van deze hulpmiddelen het certificaat installeert dat nodig is om verbinding te maken met uw beveiligd cluster.

### <a name="configure-certificate-for-the-service-fabric-cli"></a>Certificaat voor de Service Fabric CLI configureren

Om de CLI in Cloud Shell te kunnen gebruiken, moet u het PFX-certificaatbestand uploaden naar Cloud Shell en dat vervolgens gebruiken om een PEM-bestand te maken.

1. Als u het certificaat wilt uploaden naar de huidige werkmap in Cloud Shell, sleept u het PFX-certificaatbestand vanuit de map waar het op uw computer is gedownload naar uw Cloud Shell-venster.

2. Gebruik de volgende opdracht om het PFX-bestand te converteren naar een PEM-bestand. (Voor party-clusters kunt u een voor uw PFX-bestand en wachtwoord specifieke opdracht uit de instructies op de pagina **Leesmij** kopiëren.)

    ```bash
    openssl pkcs12 -in party-cluster-1486790479-client-cert.pfx -out party-cluster-1486790479-client-cert.pem -nodes -passin pass:1486790479
    ```

### <a name="configure-certificate-for-service-fabric-explorer"></a>Certificaat voor Service Fabric Explorer configureren

Als u Service Fabric Explorer wilt gebruiken, moet u het PFX-certificaatbestand dat u hebt gedownload van de Party Cluster-website importeren in uw certificaatarchief (Windows of Mac) of in de browser zelf (Ubuntu). U hebt het wachtwoord voor uw persoonlijke PFX-sleutel nodig, die te vinden is op-de pagina **Leesmij**.

Gebruik de methode die u het handigst vindt om het certificaat naar uw systeem te importeren. Bijvoorbeeld:

- Voor Windows: dubbelklik op het PFX-bestand en volg de aanwijzingen om het certificaat in uw persoonlijke archief, `Certificates - Current User\Personal\Certificates`, te installeren. U kunt ook de PowerShell-opdracht in de **Leesmij**-instructies gebruiken.
- Voor Mac: dubbelklik op het PFX-bestand en volg de aanwijzingen om het certificaat in uw Sleutelhanger te installeren.
- Voor Ubuntu: Mozilla Firefox is de standaardbrowser in Ubuntu 16.04. U kunt het certificaat importeren in Firefox door te klikken op de menuknop in de rechterbovenhoek van uw browser en te klikken op **Opties**. Op de pagina **Voorkeuren** gebruikt u het zoekvak om te zoeken naar 'certificaten'. Klik op **Certificaten bekijken**, selecteer het tabblad **Uw certificaten**, klik op **Importeren** en volg de aanwijzingen om het certificaat te importeren.

   ![Certificaat installeren in Firefox](./media/service-fabric-quickstart-containers-linux/install-cert-firefox.png)

## <a name="deploy-the-service-fabric-application"></a>De Service Fabric-toepassing implementeren

1. Maak in Cloud Shell verbinding met het Service Fabric-cluster in Azure met behulp van de CLI. Het eindpunt is het beheereindpunt voor uw cluster. U hebt het PEM-bestand in de vorige sectie gemaakt. (Voor party-clusters kunt u een voor uw PEM-bestand en beheereindpunt specifieke opdracht uit de instructies op de pagina **Leesmij** kopiëren.)

    ```bash
    sfctl cluster select --endpoint https://linh1x87d1d.westus.cloudapp.azure.com:19080 --pem party-cluster-1277863181-client-cert.pem --no-verify
    ```

2. Gebruik het installatiescript om de stemtoepassingsdefinitie te kopiëren naar het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.

    ```bash
    ./install.sh
    ```

3. Open een webbrowser en ga naar het Service Fabric Explorer-eindpunt voor uw cluster. Het eindpunt heeft de volgende indeling:  **https://\<my-azure-service-fabric-cluster-url>:19080/Explorer**, bijvoorbeeld `https://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`. </br>(Voor party-clusters kunt u het Service Fabric Explorer-eindpunt voor uw cluster vinden op de pagina **Welkom**.)

4. Als u het **toepassings**knooppunt uitvouwt, ziet u nu een vermelding voor het type stemtoepassing en het exemplaar dat u hebt gemaakt.

    ![Service Fabric Explorer][sfx]

5. Voor verbinding met de actieve container opent u een webbrowser en navigeert u naar de URL van uw cluster, bijvoorbeeld `http://linh1x87d1d.westus.cloudapp.azure.com:80`. U ziet nu de stemtoepassing in de browser.

    ![Webpagina van stemtoepassing][quickstartpic]

> [!NOTE]
> U kunt ook Service Fabric-toepassingen implementeren met Docker Compose. U zou bijvoorbeeld de volgende opdracht kunnen gebruiken om de toepassing op de cluster te implementeren en installeren met behulp van Docker Compose.
>  ```bash
> sfctl compose create --deployment-name TestApp --file-path ../docker-compose.yml
> ```

## <a name="fail-over-a-container-in-a-cluster"></a>Failover uitvoeren voor een container in een cluster

Service Fabric zorgt ervoor dat containerexemplaren automatisch worden verplaatst naar andere knooppunten in het cluster, mocht er een fout optreden. U kunt een knooppunt voor containers ook handmatig leegmaken en vervolgens probleemloos verplaatsen naar andere knooppunten in het cluster. Service Fabric biedt verschillende manieren voor het schalen van uw services. In de volgende stappen gebruikt u Service Fabric Explorer.

Doe het volgende om een failover uit te voeren voor de front-endcontainer:

1. Open Service Fabric Explorer in uw cluster, bijvoorbeeld `https://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`.
2. Klik op het knooppunt **fabric:/Voting/azurevotefront** in de structuurweergave en vouw het partitieknooppunt uit (aangegeven door een GUID). Let op: de naam van het knooppunt in de structuurweergave laat zien op welke knooppunten de container momenteel wordt uitgevoerd, bijvoorbeeld `_nodetype_4`.
3. Vouw het knooppunt **Knooppunten** in de structuurweergave uit. Klik op het weglatingsteken (drie punten) naast het knooppunt waarop de container wordt uitgevoerd.
4. Kies **Opnieuw starten** om dit knooppunt opnieuw te starten en bevestig deze actie. Door het opnieuw starten wordt voor de container een failover uitgevoerd naar een ander knooppunt in het cluster.

    ![Weergave van het knooppunt in Service Fabric Explorer][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Toepassingen en services voor schalen in een cluster

Service Fabric-services kunnen eenvoudig worden geschaald in een cluster om plaats te bieden aan de belasting voor de services. U schaalt een service door het aantal exemplaren te wijzigen dat wordt uitgevoerd in het cluster.

Voer de volgende stappen uit om de web-front-endservice te schalen:

1. Open Service Fabric Explorer in het cluster, bijvoorbeeld `https://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Klik op het weglatingsteken (drie punten) naast het knooppunt **fabric:/Voting/azurevotefront** in de structuurweergave en kies **Service schalen**.

    ![Starten van het schalen door Service Fabric Explorer][containersquickstartscale]

    U kunt er nu voor kiezen om het aantal exemplaren van de web-front-endservice te schalen.

3. Wijzig het aantal in **2** en klik op **Service schalen**.
4. Klik op het knooppunt **fabric:/Voting/azurevotefront** in de structuurweergave en vouw het partitieknooppunt uit (aangegeven door een GUID).

    ![Het schalen van Service Fabric Explorer is voltooid][containersquickstartscaledone]

    U ziet nu dat de service twee exemplaren heeft. In de structuurweergave ziet u op welke knooppunten de exemplaren worden uitgevoerd.

Met deze eenvoudige beheertaak hebt u het aantal beschikbare resources voor het verwerken van gebruikersbelasting voor de front-endservice verdubbeld. Het is belangrijk te weten dat u niet meerdere exemplaren van een service nodig hebt om ervoor te zorgen dat deze op betrouwbare wijze wordt uitgevoerd. Als de service mislukt, wordt in Service Fabric een nieuw exemplaar van de service uitgevoerd in het cluster.

## <a name="clean-up-resources"></a>Resources opschonen

1. Gebruik het uninstall-script (uninstall.sh) dat is opgegeven in de sjabloon om het toepassingsexemplaar te verwijderen uit het cluster en de registratie van het toepassingstype op te heffen. Omdat het enige tijd duurt voordat het script het exemplaar heeft verwijderd, moet u het installatiescript niet onmiddellijk na dit script uitvoeren. U kunt Service Fabric Explorer gebruiken om te bepalen wanneer het exemplaar is verwijderd en de registratie van het toepassingstype ongedaan is gemaakt.

    ```bash
    ./uninstall.sh
    ```

2. Als u klaar bent met het cluster, kunt u het certificaat verwijderen uit het certificaatarchief. Bijvoorbeeld:
   - Op Windows: gebruik de [MMC-module Certificaten](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in). Selecteer **Mijn gebruikersaccount** wanneer u de module toevoegt. Navigeer naar `Certificates - Current User\Personal\Certificates` en verwijder het certificaat.
   - Op Mac: gebruik de app Sleutelhanger.
   - Op Ubuntu: volg de stappen die u hebt gebruikt om certificaten weer te geven en verwijder het certificaat.

3. Als u Cloud Shell niet wilt blijven gebruiken, kunt u het daaraan gekoppelde opslagaccount verwijderen om verdere kosten te voorkomen. Sluit uw Cloud Shell-sessie. Klik in Azure Portal op het opslagaccount dat gekoppeld is aan Cloud Shell en klik op **Verwijderen** bovenaan de pagina en reageer op de prompts.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Linux-containertoepassing geïmplementeerd in een Service Fabric-cluster in Azure, een failover uitgevoerd op de toepassing en de toepassing geschaald in het cluster. Meer informatie over het werken met Linux-containers in Service Fabric vindt u in de zelfstudie voor Linux-containerapps.

> [!div class="nextstepaction"]
> [Een Linux-container-app maken](./service-fabric-tutorial-create-container-images.md)

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png
