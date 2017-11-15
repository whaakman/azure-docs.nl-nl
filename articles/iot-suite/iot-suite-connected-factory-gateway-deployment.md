---
title: De gateway van uw verbonden factory - Azure implementeren | Microsoft Docs
description: Het implementeren van een gateway op Windows of Linux voor verbindingen met de verbonden factory vooraf geconfigureerde oplossing.
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: dobett
ms.openlocfilehash: f6a69ecbeb09dc042eff7c1f95ee518e701b0507
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="deploy-a-gateway-on-windows-or-linux-for-the-connected-factory-preconfigured-solution"></a>Implementeer een gateway op Windows of Linux voor de verbonden factory vooraf geconfigureerde oplossing

De software die is vereist voor het implementeren van een gateway voor de verbonden factory vooraf geconfigureerde oplossing bestaat uit twee onderdelen:

* De *OPC Proxy* maakt een verbinding met IoT Hub. De *OPC Proxy* wacht tot de opdracht en controle berichten van de geïntegreerde OPC-Browser die wordt uitgevoerd in de oplossingsportal verbonden factory.
* De *OPC Publisher* maakt verbinding met bestaande lokale OPC UA-servers en telemetrieberichten van ze doorstuurt naar IoT Hub.

Beide onderdelen zijn open source en zijn beschikbaar als bron op GitHub, en als Docker-containers:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC-uitgever][lnk-publisher-github] | [OPC-uitgever][lnk-publisher-docker] |
| [OPC-Proxy][lnk-proxy-github] | [OPC-Proxy][lnk-proxy-docker] |

Er is geen openbare IP-adres of gaten in de firewall gateway zijn vereist voor beide componenten. Gebruik alleen uitgaande poorten 443 en 5671 8883 het OPC-Proxy en OPC uitgever.

De stappen in dit artikel ziet u het implementeren van een gateway met behulp van Docker van een [Windows](#windows-deployment) of [Linux](#linux-deployment). De gateway maakt verbinding met de verbonden factory vooraf geconfigureerde oplossing.

> [!NOTE]
> De gateway-software die wordt uitgevoerd in de Docker-container is [Azure IoT rand].

## <a name="windows-deployment"></a>Windows-implementatie

> [!NOTE]
> Als u een gateway-apparaat nog geen hebt, is het raadzaam om dat u een commerciële gateway koopt van een van de partners. Ga naar de [Azure IoT-apparaat catalogus] voor een lijst met de gatewayapparaten die compatibel zijn met de verbonden factory-oplossing. Volg de instructies die worden geleverd met het apparaat voor het instellen van de gateway. U kunt ook gebruik de volgende instructies voor het handmatig instellen van een van uw bestaande gateways.

### <a name="install-docker"></a>Docker installeren

Installeer [Docker voor Windows] op uw apparaat op basis van de Windows-gateway. Selecteer een station op de hostcomputer te delen met Docker tijdens de installatie van Windows Docker. De volgende schermafbeelding ziet het delen van het station D op uw Windows-systeem:

![Docker installeren][img-install-docker]

Maak vervolgens een map met de naam **docker** in de hoofdmap van de gedeelde schijf.
U kunt deze stap ook uitvoeren na de installatie van docker van de **instellingen** menu.

### <a name="configure-the-gateway"></a>De gateway configureren

1. U moet de **iothubowner** verbindingsreeks van uw Azure IoT Suite verbonden factory implementatie om de gateway-implementatie te vervolledigen. In de [Azure-portal], gaat u naar uw IoT-Hub in de resourcegroep gemaakt tijdens de implementatie van de verbonden factory-oplossing. Klik op **gedeeld toegangsbeleid** voor toegang tot de **iothubowner** verbindingsreeks:

    ![De verbindingsreeks IoT Hub vinden][img-hub-connection]

    Kopieer de **verbindingsreeks--primaire sleutel** waarde.

1. De gateway configureren voor uw IoT Hub met behulp van de twee gateway-modules **zodra** vanaf een opdrachtprompt met:

    `docker run -it --rm -h <ApplicationName> -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  is de naam te geven tot de uitgever van de UA OPC in de notatie **publisher.&lt; de volledig gekwalificeerde domeinnaam&gt;**. Bijvoorbeeld, als uw netwerk factory heet **myfactorynetwork.com**, wordt de **ApplicationName** waarde is **publisher.myfactorynetwork.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  is de **iothubowner** verbindingsreeks die u in de vorige stap hebt gekopieerd. Deze verbindingsreeks wordt alleen gebruikt in deze stap, u hoeft deze niet in de volgende stappen:

    U gebruikt het toegewezen D:\\docker-map (de `-v` argument) later om vast te leggen van de twee X.509-certificaten die de gateway-modules gebruiken.

### <a name="run-the-gateway"></a>De gateway uitvoeren

1. Start opnieuw op de gateway met de volgende opdrachten:

    `docker run -it --rm -h <ApplicationName> --expose 62222 -p 62222:62222 -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/shared -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Om veiligheidsredenen de twee X.509-certificaten die zijn doorgevoerd in de D:\\docker-map bevat de persoonlijke sleutel. Beperk de toegang tot deze map om de referenties (meestal **beheerders**) u kunt de Docker-container worden uitgevoerd. Met de rechtermuisknop op de D:\\docker-map, kies **eigenschappen**, klikt u vervolgens **beveiliging**, en vervolgens **bewerken**. Geef **beheerders** volledige controle en verwijder alle andere gebruikers:

    ![Machtigingen toekennen voor Docker-share][img-docker-share]

1. Controleer de netwerkverbinding. Voer de opdracht uit vanaf de opdrachtprompt `ping publisher.<your fully qualified domain name>` te pingen van uw gateway. Als de bestemming niet bereikbaar is, voeg de IP-adres en de naam van uw gateway uit naar het bestand hosts op uw gateway. Het hosts-bestand bevindt zich in de **Windows\\System32\\stuurprogramma's\\enzovoort** map.

1. Probeer vervolgens verbinding maken met de uitgever met een lokale OPC UA-client uitgevoerd op de gateway. Het OPC UA-eindpunt-URL is `opc.tcp://publisher.<your fully qualified domain name>:62222`. Als u een client OPC UA geen hebt, kunt u downloaden en gebruiken een [open source OPC UA client].

1. Wanneer u deze lokale tests hebt voltooid, bladert u naar de **verbinding maken met uw eigen OPC UA-Server** pagina in de oplossingsportal verbonden factory. Voer de publisher eindpunt-URL (`tcp://publisher.<your fully qualified domain name>:62222`) en klik op **Connect**. Ophalen van een certificaatwaarschuwing weergegeven en klik op **doorgaan.** Vervolgens krijgt u een fout die de uitgever van de webclient UA niet vertrouwt. Los deze fout, Kopieer de **UA webclient** certificaat van de **D:\\docker\\afgewezen certificaten\\certificaten** map voor de **D:\\docker\\UA toepassingen\\certificaten** map op de gateway. U hoeft niet opnieuw starten van de gateway. Herhaal deze stap. U kunt nu verbinding met de gateway vanuit de cloud en u bent klaar OPC UA servers toevoegen aan de oplossing.

### <a name="add-your-opc-ua-servers"></a>Uw OPC UA-servers toevoegen

1. Blader naar de **verbinding maken met uw eigen OPC UA-Server** pagina in de oplossingsportal verbonden factory. Volg dezelfde stappen als in de vorige sectie vertrouwensrelatie tussen de verbonden factory-portal en de OPC UA-server. Deze stap stelt u een wederzijdse vertrouwensrelatie van de certificaten van de verbonden factory-portal en de OPC UA-server en maakt een verbinding.

1. Bladeren in de structuur van de knooppunten OPC UA van uw OPC UA-server, met de rechtermuisknop op de knooppunten OPC en selecteert u **publiceren**. Voor het publiceren van om te werken op deze manier moet de OPC UA-server en de uitgever in hetzelfde netwerk. Met andere woorden, als de volledig gekwalificeerde domeinnaam van de uitgever is **publisher.mydomain.com** en vervolgens de volledig gekwalificeerde domeinnaam van de OPC UA-server, bijvoorbeeld zijn moet **myopcuaserver.mydomain.com**. Als de installatie anders is, kunt u handmatig knooppunten toevoegen aan het bestand publishesnodes.json is gevonden in de **D:\\docker** map. Het bestand publishesnodes.json is automatisch gegenereerd op de eerste geslaagde publiceren van een OPC-knooppunt.

1. Telemetrie loopt nu van het gateway-apparaat. Vindt u de telemetrie in de **Factory locaties** weergave van de portal verbonden factory onder **New Factory**.

## <a name="linux-deployment"></a>Linux-implementatie

> [!NOTE]
> Als u een gateway-apparaat nog geen hebt, is het raadzaam om dat u een commerciële gateway koopt van een van de partners. Ga naar de [Azure IoT-apparaat catalogus] voor een lijst met de gatewayapparaten die compatibel zijn met de verbonden factory-oplossing. Volg de instructies die worden geleverd met het apparaat voor het instellen van de gateway. U kunt ook gebruik de volgende instructies voor het handmatig instellen van een van uw bestaande gateways.

[Installeren van Docker] op uw Linux-gatewayapparaat.

### <a name="configure-the-gateway"></a>De gateway configureren

1. U moet de **iothubowner** verbindingsreeks van uw Azure IoT Suite verbonden factory implementatie om de gateway-implementatie te vervolledigen. In de [Azure-portal], gaat u naar uw IoT-Hub in de resourcegroep gemaakt tijdens de implementatie van de verbonden factory-oplossing. Klik op **gedeeld toegangsbeleid** voor toegang tot de **iothubowner** verbindingsreeks:

    ![De verbindingsreeks IoT Hub vinden][img-hub-connection]

    Kopieer de **verbindingsreeks--primaire sleutel** waarde.

1. De gateway configureren voor uw IoT Hub met behulp van de twee gateway-modules **zodra** van een shell met:

    `sudo docker run -it --rm -h <ApplicationName> -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/ -v /shared:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `sudo docker run --rm -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  is de naam van de OPC UA-toepassing die de gateway in de indeling wordt **publisher.&lt; de volledig gekwalificeerde domeinnaam&gt;**. Bijvoorbeeld: **publisher.microsoft.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  is de **iothubowner** verbindingsreeks die u in de vorige stap hebt gekopieerd. Deze verbindingsreeks wordt alleen gebruikt in deze stap, u hoeft deze niet in de volgende stappen:

    U gebruikt de **/ gedeelde** map (de `-v` argument) later om vast te leggen van de twee X.509-certificaten die de gateway-modules gebruiken.

### <a name="run-the-gateway"></a>De gateway uitvoeren

1. Start opnieuw op de gateway met de volgende opdrachten:

    `sudo docker run -it -h <ApplicationName> --expose 62222 -p 62222:62222 --rm -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v /shared:/shared -v /shared:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `sudo docker run -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Uit veiligheidsoverwegingen worden de twee X.509-certificaten persistent in de **/ gedeelde** map de persoonlijke sleutel bevatten. Beperk de toegang tot deze map om de referenties die u met de Docker-container uitvoeren. Stel de machtigingen voor **hoofdmap** alleen, gebruiken de `chmod` shell opdracht in de map.

1. Controleer de netwerkverbinding. Voer de opdracht van een shell `ping publisher.<your fully qualified domain name>` te pingen van uw gateway. Als de bestemming niet bereikbaar is, toevoegen het IP-adres en de naam van uw gateway aan het bestand hosts op uw gateway. Het hosts-bestand bevindt zich in de **/enzovoort** map.

1. Probeer vervolgens verbinding maken met de uitgever met een lokale OPC UA-client uitgevoerd op de gateway. Het OPC UA-eindpunt-URL is `opc.tcp://publisher.<your fully qualified domain name>:62222`. Als u een client OPC UA geen hebt, kunt u downloaden en gebruiken een [open source OPC UA client].

1. Wanneer u deze lokale tests hebt voltooid, bladert u naar de **verbinding maken met uw eigen OPC UA-Server** pagina in de oplossingsportal verbonden factory. Voer de publisher eindpunt-URL (`tcp://publisher.<your fully qualified domain name>:62222`) en klik op **Connect**. Ophalen van een certificaatwaarschuwing weergegeven en klik op **doorgaan.** Vervolgens krijgt u een fout die de uitgever van de webclient UA niet vertrouwt. Los deze fout, kopieert u de **UA webclient** certificaat van de **/gedeeld/geweigerd certificaten/certificaten** map voor de **/shared/UA-toepassingen/certificaten** map op de gateway. U hoeft niet opnieuw starten van de gateway. Herhaal deze stap. U kunt nu verbinding met de gateway vanuit de cloud en u bent klaar OPC UA servers toevoegen aan de oplossing.

### <a name="add-your-opc-ua-servers"></a>Uw OPC UA-servers toevoegen

1. Blader naar de **verbinding maken met uw eigen OPC UA-Server** pagina in de oplossingsportal verbonden factory. Volg dezelfde stappen als in de vorige sectie vertrouwensrelatie tussen de verbonden factory-portal en de OPC UA-server. Deze stap stelt u een wederzijdse vertrouwensrelatie van de certificaten van de verbonden factory-portal en de OPC UA-server en maakt een verbinding.

1. Bladeren in de structuur van de knooppunten OPC UA van uw OPC UA-server, met de rechtermuisknop op de knooppunten OPC en selecteert u **publiceren**. Voor het publiceren van om te werken op deze manier moet de OPC UA-server en de uitgever in hetzelfde netwerk. Met andere woorden, als de volledig gekwalificeerde domeinnaam van de uitgever is **publisher.mydomain.com** en vervolgens de volledig gekwalificeerde domeinnaam van de OPC UA-server, bijvoorbeeld zijn moet **myopcuaserver.mydomain.com**. Als de installatie anders is, kunt u handmatig knooppunten toevoegen aan het bestand publishesnodes.json is gevonden in de **/ gedeelde** map. De publishesnodes.json wordt automatisch gegenereerd op de eerste geslaagde publiceren van een OPC-knooppunt.

1. Telemetrie loopt nu van het gateway-apparaat. Vindt u de telemetrie in de **Factory locaties** weergave van de portal verbonden factory onder **New Factory**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de architectuur van de verbonden factory vooraf geconfigureerde oplossing, [verbonden factory oplossing walkthrough over vooraf geconfigureerde][lnk-walkthrough].

Meer informatie over de [OPC Publisher verwijzing implementatie](iot-suite-connected-factory-publisher.md).

[img-install-docker]: ./media/iot-suite-connected-factory-gateway-deployment/image1.png
[img-hub-connection]: ./media/iot-suite-connected-factory-gateway-deployment/image2.png
[img-docker-share]: ./media/iot-suite-connected-factory-gateway-deployment/image3.png

[Docker voor Windows]: https://www.docker.com/docker-windows
[Azure IoT-apparaat catalogus]: https://catalog.azureiotsuite.com/?q=opc
[Azure-portal]: http://portal.azure.com/
[open source OPC UA client]: https://github.com/OPCFoundation/UA-.NETStandardLibrary/tree/master/SampleApplications/Samples/Client.Net4
[Installeren van Docker]: https://www.docker.com/community-edition#/download
[lnk-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[Azure IoT rand]: https://github.com/Azure/iot-edge

[lnk-publisher-github]: https://github.com/Azure/iot-edge-opc-publisher
[lnk-publisher-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua
[lnk-proxy-github]: https://github.com/Azure/iot-edge-opc-proxy
[lnk-proxy-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua-proxy