---
title: Implementeren van een aangepaste installatiekopie voor Apparaatsimulatie - Azure | Microsoft Docs
description: In deze handleiding leert u hoe u een aangepaste Docker-installatiekopie van de Apparaatsimulatie-oplossing implementeren in Azure.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/06/2018
ms.author: dobett
ms.openlocfilehash: c1f321f452b65016c11cb66d08ebab108509cc62
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51284898"
---
# <a name="deploy-a-custom-device-simulation-docker-image"></a>Een aangepaste docker-installatiekopie voor Apparaatsimulatie implementeren

U kunt de Apparaatsimulatie-oplossing voor het toevoegen van aangepaste functies wijzigen. Bijvoorbeeld, de [serialiseren telemetrie met behulp van Protocol Buffers](iot-accelerators-device-simulation-protobuf.md) artikel leest u hoe u een aangepast apparaat toevoegen aan de oplossing die gebruikmaakt van Protocol Buffers (Protobuf) om telemetrie te verzenden. Nadat u uw wijzigingen lokaal hebt getest, wordt de volgende stap is het implementeren van uw wijzigingen naar uw instantie Apparaatsimulatie in Azure. Als u wilt deze taak hebt voltooid, moet u maken en implementeren van een Docker-installatiekopie met de gewijzigde service.

De stappen in deze procedure-naar-handleiding leert u hoe aan:

1. Een ontwikkelomgeving voorbereiden
1. Een nieuwe Docker-installatiekopie genereren
1. Apparaatsimulatie voor het gebruik van de nieuwe Docker-installatiekopie configureren
1. Een met behulp van de nieuwe simulatie uitvoeren

## <a name="prerequisites"></a>Vereisten

Voor de stappen in deze handleiding, hebt u het volgende nodig:

* Een geïmplementeerde [Apparaatsimulatie](quickstart-device-simulation-deploy.md) exemplaar.
* Docker. Download de [Docker Community Edition](https://www.docker.com/products/docker-engine#/download) voor uw platform.
* Een [Docker Hub-account](https://hub.docker.com/) waar u uw Docker-installatiekopieën kunt uploaden. Maak in uw Docker Hub-account, een openbare opslagplaats met de naam **apparaatsimulatie**.
* Een aangepast en getest [Apparaatsimulatie-oplossing](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) op uw lokale computer. Bijvoorbeeld, kunt u de oplossing [serialiseren telemetrie met behulp van Protocol Buffers](iot-accelerators-device-simulation-protobuf.md).
* Een shell die SSH kunt uitvoeren. Als u Git voor Windows hebt geïnstalleerd, kunt u de **bash** shell die deel uitmaakt van e-installatie. U kunt ook uw [Azure Cloud Shell](https://shell.azure.com/).

De instructies in dit artikel wordt ervan uitgegaan dat u gebruikmaakt van Windows. Als u een ander besturingssysteem gebruikt, moet u mogelijk enkele van de bestandspaden en opdrachten op basis van uw omgeving aanpassen.

## <a name="create-a-new-docker-image"></a>Een nieuwe Docker-installatiekopie maken

Voor het implementeren van uw eigen wijzigingen aan de Apparaatsimulatie-service, moet u de build- en -scripts in **scripts\docker** map voor het uploaden van de containers naar uw docker-hub-account

### <a name="modify-the-docker-scripts"></a>De docker-scripts wijzigen

Wijzigen van de Docker **build.cmd**, **publish.cmd**, en **run.cmd** scripts in de **scripts\docker** map met uw Docker-Hub de gegevens van de opslagplaats. Deze stappen wordt ervan uitgegaan dat u hebt gemaakt met een openbare opslagplaats met de naam **apparaatsimulatie**:

`DOCKER_IMAGE={your-docker-hub-username}/device-simulation`

Update de **docker-compose.yml** bestand als volgt:

`image: {your-docker-hub-username}/device-simulation`

### <a name="configure-the-solution-to-include-any-new-files"></a>De oplossing configureren voor alle nieuwe bestanden

Als u nieuwe bestanden die apparaat-model hebt toegevoegd, moet u ze expliciet opneemt in de oplossing. Voeg een vermelding aan de **services/services.csproj** voor elk bestand om op te nemen. Bijvoorbeeld, als u de [serialiseren telemetrie met behulp van Protocol Buffers](iot-accelerators-device-simulation-protobuf.md) instructies, de volgende vermeldingen toe te voegen:

```xml
<None Update="data\devicemodels\assettracker-01.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
<None Update="data\devicemodels\scripts\assettracker-01-state.js">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
```

### <a name="generate-new-docker-images-and-push-to-docker-hub"></a>Nieuwe Docker-installatiekopieën genereren en pushen naar Docker Hub

Publicatie van de nieuwe Docker-installatiekopie naar Docker Hub met behulp van uw docker-hub-account:

1. Open een opdrachtprompt en navigeer naar de lokale kopie van de opslagplaats van de simulatie van Apparaatbeheer.

1. Navigeer naar de **docker** map:

    ```cmd
    cd scripts\docker
    ```

1. Voer de volgende opdracht om de Docker-installatiekopie te bouwen:

    ```cmd
    build.cmd
    ```

1. Voer de volgende opdracht voor het publiceren van de Docker-installatiekopie naar uw Docker Hub-opslagplaats. Meld u aan Docker met de referenties van uw Docker Hub:

    ```cmd
    docker login
    publish.cmd
    ```

<!-- TODO fix heading levels working include -->

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="update-the-service"></a>De service bijwerken

Voor het bijwerken van de Apparaatsimulatie-container voor het gebruik van uw aangepaste installatiekopie, voert u de volgende stappen uit:

* SSH gebruiken om te verbinden met de virtuele machine die als host fungeert voor uw Apparaatsimulatie-exemplaar. Gebruik de IP-adres en het wachtwoord die u hebt genoteerd een in de vorige sectie:

    ```sh
    ssh azureuser@{your vm ip address}
    ```

* Navigeer naar de **/app** directory:

    ```sh
    cd /app
    ```

* Bewerk de **docker-compose.yml** bestand:

    ```sh
    sudo nano docker-compose.yml
    ```

    Wijzig de **installatiekopie** zodat het aangepaste **apparaatsimulatie** installatiekopie die u hebt geüpload naar uw Docker Hub-opslagplaats:

    ```yml
    image: {your-docker-hub-username}/device-simulation
    ```

    Sla uw wijzigingen op.

* Voer de volgende opdracht uit om het opnieuw opstarten van de microservices:

    ```sh
    sudo start.sh
    ```

## <a name="run-your-simulation"></a>De simulatie uitvoeren

U kunt nu een simulatie met behulp van uw aangepaste Apparaatsimulatie-oplossing uitvoeren:

1. Start uw Apparaatsimulatie web-UI van [Microsoft Azure IoT-oplossingsversnellers](https://www.azureiotsolutions.com/Accelerators#dashboard).

1. Gebruik de web-UI te configureren en een simulatie uitvoeren. Als u eerder hebt voltooid [serialiseren telemetrie met behulp van Protocol Buffers](iot-accelerators-device-simulation-protobuf.md), kunt u uw aangepaste Apparaatmodel.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe u een aangepaste Apparaatsimulatie-installatiekopie implementeert, kunt u leren hoe u [gebruiken van een bestaande IoT-hub met de oplossingsversnellers Apparaatsimulatie](iot-accelerators-device-simulation-choose-hub.md).