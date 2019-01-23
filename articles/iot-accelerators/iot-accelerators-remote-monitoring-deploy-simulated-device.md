---
title: IoT implementeren aangepaste gesimuleerde apparaten - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing ziet u hoe u aangepaste gesimuleerde apparaten in de oplossingsverbetering voor externe bewaking implementeert.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/15/2018
ms.topic: conceptual
ms.openlocfilehash: 8491acfd28524ee3bbbd06c788afb668c40c98b9
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54450565"
---
# <a name="deploy-a-new-simulated-device"></a>Een nieuw gesimuleerd apparaat implementeren

De controle op afstand en Apparaatsimulatie oplossingsversnellers beide kunnen u uw eigen gesimuleerde apparaten definiëren. Dit artikel ziet u hoe u een aangepaste Koelunit apparaattype en een nieuwe gloeilamp apparaattype implementeert in de oplossingsverbetering voor externe controle.

De stappen in dit artikel wordt ervan uitgegaan dat u hebt voltooid, de [maken en test een nieuw gesimuleerd apparaat](iot-accelerators-remote-monitoring-create-simulated-device.md) procedures begeleiden en de bestanden die de aangepaste Koelunit en nieuwe gloeilamp apparaattypen definiëren.

De stappen in deze handleiding leert u hoe aan:

1. SSH gebruiken voor toegang tot het bestandssysteem van de virtuele machine die als host fungeert voor de oplossingsverbetering voor externe controle.

1. Docker voor het laden van de apparaatmodellen uit een locatie buiten de Docker-container configureren.

1. De oplossingsversnellers bewaking op afstand met behulp van aangepast apparaat modelbestanden worden uitgevoerd.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Voor de stappen in deze handleiding, hebt u een actief Azure-abonnement nodig.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze gebruiksaanwijzing volgen, hebt u het volgende nodig:

- Een geïmplementeerd exemplaar van de [oplossingsverbetering voor externe controle](https://www.azureiotsolutions.com/Accelerators#solutions/types/RM2).
- Een lokale **bash** shell om uit te voeren de `ssh` en `scp` opdrachten. Op Windows, een eenvoudige manier voor het installeren van **bash** is het installeren van [git](https://git-scm.com/download/win).
- De bestanden van een aangepast apparaat model, zoals beschreven in die [maken en test een nieuw gesimuleerd apparaat](iot-accelerators-remote-monitoring-create-simulated-device.md).

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="configure-docker"></a>Docker configureren

In deze sectie configureert u Docker voor het laden van de bestanden voor het model van apparaat uit de **/tmp/devicemodels** map in de virtuele machine in plaats van vanaf binnen de Docker-container. Voer de opdrachten in deze sectie in een **bash** shell op uw lokale computer:

In deze sectie configureert u Docker voor het laden van de bestanden voor het model van apparaat uit de **/tmp/devicemodels** map in de virtuele machine in plaats van vanaf binnen de Docker-container. Voer de opdrachten in deze sectie in een **bash** shell op uw lokale computer:

1. Gebruik SSH verbinding maken met de virtuele machine in Azure vanuit uw lokale computer. De volgende opdracht wordt ervan uitgegaan dat het openbare IP-adres van de virtuele machine **vm-vikxv** is **104.41.128.108** --Vervang deze waarde door het openbare IP-adres van uw virtuele machine uit de vorige sectie:

   ```sh
    ssh azureuser@104.41.128.108
    ```

    Volg de aanwijzingen voor het aanmelden bij de virtuele machine met het wachtwoord die u in de vorige sectie instelt.

1. Configureer de service van de simulatie apparaat voor het laden van de apparaatmodellen van buiten de container. De Docker-configuratiebestand voor het eerst opent:

    ```sh
    sudo nano /app/docker-compose.yml
    ```

    Ga naar de instellingen voor de **devicesimulation** container en bewerk de **volumes** instellen zoals wordt weergegeven in het volgende codefragment:

    ```yml
    devicesimulation:
      image: azureiotpcs/device-simulation-dotnet:1.0.0
      networks:
        - default_net
      depends_on:
        - storageadapter
      environment:
        - PCS_IOTHUB_CONNSTRING
        - PCS_STORAGEADAPTER_WEBSERVICE_URL=http://storageadapter:9022/v1
        - PCS_AUTH_ISSUER
        - PCS_AUTH_AUDIENCE
        - PCS_AUTH_REQUIRED
        - PCS_CORS_WHITELIST
        - PCS_APPLICATION_SECRET
      # How one could mount custom device models
      volumes:
        - /tmp/devicemodels:/app/webservice/data/devicemodels:ro
    ```

    Sla de wijzigingen op.

1. Kopieer de bestanden van de bestaande apparaatstuurprogramma-model van de container naar de nieuwe locatie. Zoek eerst de container-ID voor de container van de simulatie van apparaat:

    ```sh
    docker ps
    ```

    Kopieer de bestanden voor het model van apparaat naar de **tmp** map in de virtuele machine. De volgende opdracht wordt ervan uitgegaan dat de container-ID's c378d6878407--Vervang deze waarde door uw apparaat simulatie container-ID:

    ```sh
    docker cp c378d6878407:/app/webservice/data/devicemodels /tmp
    ```

    Houd de **bash** -venster met de SSH-sessie geopend.

1. Kopieer de bestanden van uw aangepaste apparaat model bij de virtuele machine. Voer deze opdracht uit in een andere **bash** shell op de computer waarop u uw aangepaste apparaatmodellen hebt gemaakt. Navigeer naar de lokale map met de JSON-bestanden van uw apparaat-model. De volgende opdrachten wordt ervan uitgegaan dat het openbare IP-adres van de virtuele machine is **104.41.128.108** --Vervang deze waarde door het openbare IP-adres van uw virtuele machine. Voer het wachtwoord van uw virtuele machine wanneer hierom wordt gevraagd:

    ```sh
    scp *json azureuser@104.41.128.108:/tmp/devicemodels
    cd scripts
    scp *js azureuser@104.41.128.108:/tmp/devicemodels/scripts
    ```

1. Start opnieuw op het apparaat simulatie Docker-container voor het gebruik van de nieuwe apparaatmodellen. Voer de volgende opdrachten de **bash** shell met de open SSH-sessie met de virtuele machine:

    ```sh
    sudo /app/start.sh
    ```

    Als u zien van de status van de actieve Docker-containers en hun container-id's wilt, gebruikt u de volgende opdracht uit:

    ```sh
    docker ps
    ```

    Als u zien van het logboek van de container van de simulatie van apparaat wilt, de volgende opdracht uitvoeren. De container-ID vervangen door de ID van uw apparaat simuleren-container:

    ```sh
    docker logs -f 5d3f3e78822e
    ```

## <a name="run-simulation"></a>Simulatie uitvoeren

U kunt nu uw aangepaste modellen in de oplossing voor externe controle:

1. Start uw dashboard externe controle van [Microsoft Azure IoT-oplossingsversnellers](https://www.azureiotsolutions.com/Accelerators#dashboard).

1. Gebruik de **apparaten** pagina gesimuleerde apparaten toevoegen. Wanneer u een nieuw gesimuleerd apparaat toevoegt, wordt uw nieuwe apparaatmodellen zijn beschikbaar om te kiezen.

1. Het dashboard kunt u telemetrie van apparaten weergeven en apparaatmethoden aanroepen.

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent te verder te verkennen, laat u de oplossingsversneller bewaking op afstand is geïmplementeerd.

Als u de oplossingsversneller niet meer nodig hebt, verwijderd uit de [ingericht oplossingen](https://www.azureiotsolutions.com/Accelerators#dashboard) pagina door te selecteren en vervolgens te klikken op **oplossing verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Deze handleiding hebt u geleerd u aangepaste modellen implementeren op de oplossingsverbetering voor externe controle. De voorgestelde volgende stap is te leren hoe u [een echt apparaat verbinden met uw oplossing voor externe controle](iot-accelerators-connecting-devices-node.md).
