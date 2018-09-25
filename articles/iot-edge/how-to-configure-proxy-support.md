---
title: Azure IoT Edge-apparaten configureren voor netwerk-proxy's | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de Azure IoT Edge-runtime en een internetgerichte IoT Edge-modules om te communiceren via een proxyserver.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 09/24/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6e6a1d2f758cabca41ac405a01de1f0d8bfd0a7b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037453"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Een IoT Edge-apparaat om te communiceren via een proxyserver configureren

IoT Edge-apparaten verzenden HTTPS-aanvragen om te communiceren met IoT Hub. Als uw apparaat is verbonden met een netwerk dat gebruikmaakt van een proxyserver, moet u de IoT Edge-runtime om te communiceren via de-server configureren. Proxy-servers kunnen ook van invloed op afzonderlijke IoT Edge-modules als ze HTTP of HTTPS-aanvragen die niet worden doorgestuurd via de Edge hub. 

Een IoT Edge-apparaat om te werken met een proxyserver configureren, volgt deze eenvoudige stappen: 

1. IoT Edge-runtime installeren op uw apparaat. 
2. Configureer de Docker-daemon en de IoT Edge-daemon op uw apparaat om de proxyserver te gebruiken.
3. Configureer de edgeAgent-eigenschappen in het bestand config.yaml op uw apparaat.
4. Omgevingsvariabelen worden ingesteld voor de IoT Edge-runtime en andere IoT-Edge modules in het manifest van de implementatie. 

## <a name="install-the-runtime"></a>De runtime installeren

Als u de IoT Edge-runtime op een Linux-apparaat installeert, configureert u de package manager om te gaan via de proxyserver voor toegang tot het installatiepakket. Bijvoorbeeld, [apt-get ingesteld voor het gebruik van een http-proxy](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy). Zodra uw pakketmanager is geconfigureerd, volg de instructies in [installeren Azure IoT Edge-runtime op Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) of [Azure IoT Edge-runtime installeren in Linux (x64)](how-to-install-iot-edge-linux.md) zoals gebruikelijk. 

Als u de IoT Edge-runtime op een Windows-apparaat installeert, moet u via de proxy-server voor toegang tot het installatiepakket. U kunt configureren van proxy-informatie in de Windows-instellingen of het opnemen van uw proxy-informatie rechtstreeks in het script voor installatie. De volgende powershell-script wordt een voorbeeld van een windows-installatie met de `-proxy` argument:

```powershell
. {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows
```

Zie voor meer informatie en installatie-opties, [installeren Azure IoT Edge-runtime op Windows voor gebruik met Windows-containers](how-to-install-iot-edge-windows-with-windows.md) of [installeren Azure IoT Edge-runtime op Windows gebruiken met Linux-containers](how-to-install-iot-edge-windows-with-linux.md).

Wanneer de IoT Edge-runtime is geïnstalleerd, gebruikt u de volgende sectie te configureren met uw proxy-informatie. 

## <a name="configure-the-daemons"></a>De daemons configureren

De Docker- en IoT Edge-daemons die worden uitgevoerd op uw IoT Edge-apparaat moeten worden geconfigureerd voor het gebruik van de proxyserver. De Docker-daemon maakt webaanvragen voor pull-containerinstallatiekopieën van container Registry. De daemon IoT Edge maakt webaanvragen om te communiceren met IoT Hub.

### <a name="docker-daemon"></a>Docker-daemon

Raadpleeg de documentatie voor Docker de Docker-daemon configureren met omgevingsvariabelen. De meeste containerregisters (inclusief DockerHub en Azure Container Registry) ondersteuning voor HTTPS-aanvragen, zodat de variabele die u moet instellen **HTTPS_PROXY**. Als u bent installatiekopieën binnenhaalt vanuit een register dat biedt geen ondersteuning voor transport layer security (TLS) en u moet instellen de **gebruikt**. 

Kies het artikel dat van toepassing op uw Docker-versie: 

* [Docker](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
* [Docker voor Windows](https://docs.docker.com/docker-for-windows/#proxies)

### <a name="iot-edge-daemon"></a>IoT Edge-daemon

De IoT Edge-daemon is geconfigureerd in een soortgelijke manier als in de Docker-daemon. HTTPS gebruikt voor alle aanvragen die IoT Edge naar IoT Hub verzendt. Gebruik de volgende stappen uit om in te stellen van een omgevingsvariabele voor de service, op basis van uw besturingssysteem. 

#### <a name="linux"></a>Linux

Een editor openen in de terminal om te configureren van de IoT Edge-daemon. 

```bash
sudo systemctl edit iotedge
```

Voer de volgende tekst vervangen  **\<proxy-URL >** uw proxyserveradres en de poort. Vervolgens opslaan en sluiten. 

```text
[Service]
Environment="https_proxy=<proxy URL>"
```

De servicemanager om op te halen de nieuwe configuratie voor iotedge vernieuwen.

```bash
sudo systemctl daemon-reload
```

Start opnieuw op IoT Edge om de wijzigingen te laten treden.

```bash
sudo systemctl restart iotedge
```

Controleer of de omgevingsvariabele is gemaakt, en de nieuwe configuratie is geladen. 

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

Open een PowerShell-venster als beheerder en voer de volgende opdracht naar het register bewerken met de nieuwe omgevingsvariabele. Vervang  **\<proxy-url >** uw proxyserveradres en de poort. 

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

Start opnieuw op IoT Edge om de wijzigingen te laten treden.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-edge-agent"></a>De Edge agent configureren

De Edge agent is de eerste module te starten op elk IoT Edge-apparaat. Deze gestart voor het eerst op basis van de informatie in de IoT Edge config.yaml-bestand. De Edge agent maakt vervolgens verbinding met IoT Hub om op te halen van implementatie-manifesten die welke andere modules moeten worden geïmplementeerd op het apparaat.

Open het bestand config.yaml op uw IoT Edge-apparaat. Op Linux-systemen, dit bestand bevindt zich in **/etc/iotedge/config.yaml**. Op Windows-systemen, dit bestand bevindt zich in **C:\ProgramData\iotedge\config.yaml**. Het configuratiebestand is beveiligd, moet u de Administrator-bevoegdheden om deze te openen. Op Linux-systemen, betekent dit dat met behulp van de `sudo` opdracht voor het openen van het bestand in uw favoriete teksteditor. Op Windows betekent dat een teksteditor zoals Kladblok als administrator uitvoeren en vervolgens het bestand te openen. 

Zoek in het bestand config.yaml de **Edge Agent-module spec** sectie. De definitie van de Edge agent bevat een **env** parameter waar u omgevingsvariabelen kunt toevoegen. 

![edgeAgent definitie](./media/how-to-configure-proxy-support/edgeagent-unedited.png)

Verwijder de accolades die tijdelijke aanduidingen voor de parameter env zijn, en voeg de nieuwe variabele op een nieuwe regel toe. Houd er rekening mee dat streepjes in YAML twee spaties zijn. 

```yaml
https_proxy: "<proxy URL>"
```

IoT Edge-runtime gebruikt AMQP standaard om te communiceren met IoT Hub. Sommige proxyservers blokkeren AMQP poorten. Als dat het geval is, klikt u vervolgens moet u ook het configureren van edgeAgent voor het gebruik van AMQP via WebSocket. Een tweede omgevingsvariabele toevoegen.

```yaml
UpstreamProtocol: "AmqpWs"
```

![edgeAgent definitie met omgevingsvariabelen](./media/how-to-configure-proxy-support/edgeagent-edited.png)

Sla de wijzigingen in config.yaml en sluit de editor. Start opnieuw op IoT Edge om de wijzigingen te laten treden. 

* Linux: 

   ```bash
   sudo systemctl restart iotedge
   ```

* Windows:

   ```powershell
   Restart-Service iotedge
   ```

## <a name="configure-deployment-manifests"></a>Manifesten implementatie configureren  

Nadat uw IoT Edge-apparaat is geconfigureerd om te werken met uw proxy-server, moet u ook de omgevingsvariabelen in alle toekomstige implementatie manifesten declareren. De twee runtimemodules, edgeAgent en edgeHub, moet u altijd de proxyserver geconfigureerd voor communicatie met IoT Hub onderhouden hebben. U kunt elk IoT Edge-module om te communiceren via een proxyserver configureren, maar het is niet nodig voor modules die hun via doorsturen edgeHub of die alleen communiceren met andere modules op het apparaat. 

U kunt implementatie manifesten met behulp van de Azure portal of handmatig door het bewerken van een JSON-bestand maken. 

### <a name="azure-portal"></a>Azure Portal

Wanneer u gebruikt de **modules instellen** wizard om implementaties te maken voor IoT Edge-apparaten, elke module heeft een **omgevingsvariabelen** sectie die u gebruiken kunt om proxy serververbindingen te configureren. 

Selecteer voor het configureren van de Edge agent en Edge hub-modules, **geavanceerde instellingen voor Edge-Runtime configureren** op de eerste stap van de wizard. 

![Geavanceerde runtime-instellingen voor Edge configureren](./media/how-to-configure-proxy-support/configure-runtime.png)

Voeg de **https_proxy** omgevingsvariabele op de Edge agent- en moduledefinities voor Edge hub. Als u de **UpstreamProtocol** omgevingsvariabele in het bestand config.yaml op uw IoT Edge-apparaat toevoegen die aan de definitie van de Edge agent-module te. 

![Omgevingsvariabelen instellen](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

Alle andere modules die u aan een manifest van de implementatie toevoegt volgen hetzelfde patroon. Op de pagina waar u de modulenaam van de en de installatiekopie instellen, moet u er een sectie van de variabelen voor de omgeving is.

### <a name="json-deployment-manifest-files"></a>JSON-implementatie-manifestbestanden

Als u implementaties voor IoT Edge-apparaten die gebruikmaken van de sjablonen in Visual Studio Code of door het handmatig maken van JSON-bestanden maakt, kunt u de omgevingsvariabelen rechtstreeks naar de definitie van elke module kunt toevoegen. 

Gebruik de volgende JSON-indeling: 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

Met de omgevingsvariabelen die is opgenomen, moet de definitie van de module edgeHub volgt uitzien:

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": ""
    },
    "env": {
        "https_proxy": {
            "value": "https://proxy.example.com:3128"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Als u de **UpstreamProtocol** omgevingsvariabele in het bestand confige.yaml op uw IoT Edge-apparaat toevoegen die aan de definitie van de Edge agent-module te. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de functies van de [IoT Edge-runtime](iot-edge-runtime.md).

Installatie en configuratie van fouten met [veelvoorkomende problemen en oplossingen voor Azure IoT Edge](troubleshoot.md)

