---
title: Apparaten configureren voor netwerk-proxy's - Azure IoT Edge | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de Azure IoT Edge-runtime en een internetgerichte IoT Edge-modules om te communiceren via een proxyserver.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 1c0da1a768b894f543b9089643622c31d6a8758d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66730151"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Een IoT Edge-apparaat om te communiceren via een proxyserver configureren

IoT Edge-apparaten verzenden HTTPS-aanvragen om te communiceren met IoT Hub. Als uw apparaat is verbonden met een netwerk dat gebruikmaakt van een proxyserver, moet u de IoT Edge-runtime om te communiceren via de-server configureren. Proxy-servers kunnen ook van invloed op afzonderlijke IoT Edge-modules als ze HTTP of HTTPS-aanvragen die niet worden doorgestuurd via de IoT Edge hub. 

Dit artikel helpt bij de volgende vier stappen voor het configureren en vervolgens beheren een IoT Edge-apparaat achter een proxyserver: 

1. **IoT Edge-runtime installeren op uw apparaat.**

   De IoT Edge-installatiescripts pull-pakketten en bestanden van het internet, zodat het apparaat moet communiceren via de proxy-server om deze aanvragen te maken. Zie voor gedetailleerde stappen de [installeren van de runtime via een proxy](#install-the-runtime-through-a-proxy) sectie van dit artikel. Voor Windows-apparaten, biedt het script voor installatie ook een [Offline-installatie](how-to-install-iot-edge-windows.md#offline-installation) optie. 

   Deze stap is een eenmalige proces dat wordt uitgevoerd op het IoT Edge-apparaat als u deze eerst van instelt. De dezelfde verbindingen zijn ook vereist wanneer u de IoT Edge-runtime bijwerken. 

2. **Configureer de Docker-daemon en de IoT Edge-daemon op uw apparaat.**

   IoT Edge maakt gebruik van twee daemons op het apparaat, die beide nodig hebt om webaanvragen via de proxy-server te maken. De IoT Edge-daemon is verantwoordelijk voor communicatie met IoT Hub. De daemon Moby is verantwoordelijk voor het containerbeheer van de, dus communiceert met container Registry. Zie voor gedetailleerde stappen de [configureren de daemons](#configure-the-daemons) sectie van dit artikel. 

   Deze stap is een eenmalige proces dat wordt uitgevoerd op het IoT Edge-apparaat als u deze eerst van instelt.

3. **Configureer de eigenschappen van de IoT Edge-agent in het bestand config.yaml op uw apparaat.**

   De IoT Edge-daemon Start de module edgeAgent in eerste instantie, maar vervolgens de edgeAgent-module is zelf verantwoordelijk voor het ophalen van het manifest van de implementatie van IoT-Hub en starten van de modules. Voor de IoT Edge-agent op de eerste verbinding maken met IoT Hub, de edgeAgent module omgevingsvariabelen configureren handmatig op het apparaat zelf. U kunt de module edgeAgent op afstand configureren na de eerste verbinding. Zie voor gedetailleerde stappen de [configureren van de IoT Edge-agent](#configure-the-iot-edge-agent) sectie van dit artikel.

   Deze stap is een eenmalige proces dat wordt uitgevoerd op het IoT Edge-apparaat als u deze eerst van instelt.

4. **Voor alle toekomstige module-implementaties, omgevingsvariabelen voor elke module die communiceren via de proxy worden ingesteld.**

   Zodra uw IoT Edge-apparaat is ingesteld en met IoT Hub via de proxy-server verbonden, moet u de verbinding in alle toekomstige module implementaties behouden. Zie voor gedetailleerde stappen de [configureren implementatie manifesten](#configure-deployment-manifests) sectie van dit artikel. 

   Deze stap is een continu proces op afstand worden uitgevoerd zodat elke nieuwe update van de module of implementatie van het apparaat de mogelijkheid om te communiceren via de proxy-server onderhoudt. 

## <a name="know-your-proxy-url"></a>De URL van uw proxy weet

Voordat u begint met een van de stappen in dit artikel, moet u de URL van uw proxy weet.

Proxy-URL's worden de volgende indeling: **protocol**://**proxy_host**:**proxy_poort**.

* De **protocol** via HTTP of HTTPS is. De Docker-daemon-protocol een van beide, afhankelijk van uw container registry-instellingen kunt gebruiken, maar de IoT Edge-daemon en runtime containers moeten altijd gebruik van HTTPS.

* De **proxy_host** is een adres voor de proxyserver. Als uw proxyserver verificatie is vereist, kunt u uw referenties opgeven als onderdeel van de proxyhost met de volgende indeling: **gebruiker**:**wachtwoord**\@**proxy_host** .

* De **proxy_poort** is de netwerkpoort waarop de proxy op netwerkverkeer reageert.

## <a name="install-the-runtime-through-a-proxy"></a>Installeer de runtime via een proxy

Of uw IoT Edge-apparaat op Windows of Linux wordt uitgevoerd, moet u toegang tot de van installatiepakketten via de proxy-server. Volg de stappen voor het installeren van de IoT Edge-runtime via een proxyserver, afhankelijk van uw besturingssysteem. 

### <a name="linux"></a>Linux

Als u de IoT Edge-runtime op een Linux-apparaat installeert, configureert u de package manager om te gaan via de proxyserver voor toegang tot het installatiepakket. Bijvoorbeeld, [apt-get ingesteld voor het gebruik van een http-proxy](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy). Zodra uw pakketmanager is geconfigureerd, volg de instructies in [installeren Azure IoT Edge-runtime op Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) of [Azure IoT Edge-runtime installeren in Linux (x64)](how-to-install-iot-edge-linux.md) zoals gebruikelijk.

### <a name="windows"></a>Windows

Als u de IoT Edge-runtime op een Windows-apparaat installeert, moet u twee keer doorlopen van de proxyserver. De eerste verbinding downloadt het script installer-bestand en de tweede verbinding is tijdens de installatie om de benodigde onderdelen te downloaden. U kunt configureren van proxy-informatie in de Windows-instellingen of het opnemen van uw proxy-informatie rechtstreeks in de PowerShell-opdrachten. 

De volgende stappen laten zien van een voorbeeld van een windows-installatie met de `-proxy` argument:

1. De opdracht Invoke-WebRequest moet proxy-informatie voor toegang tot het installatiescript. De opdracht implementeren IoTEdge moet vervolgens de proxy-informatie voor het downloaden van de installatiebestanden. 

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Deploy-IoTEdge -proxy <proxy URL>
   ```

2. De opdracht initialiseren IoTEdge hoeft niet te gaan via de proxyserver, zodat de tweede stap alleen proxy-informatie voor Invoke-WebRequest vereist.

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Initialize-IoTEdge

If you have complicated credentials for the proxy server that can't be included in the URL, use the `-ProxyCredential` parameter within `-InvokeWebRequestParameters`. For example,

```powershell
$proxyCredential = (Get-Credential).GetNetworkCredential()
. {Invoke-WebRequest -proxy <proxy URL> -ProxyCredential $proxyCredential -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -InvokeWebRequestParameters @{ '-Proxy' = '<proxy URL>'; '-ProxyCredential' = $proxyCredential }
```

Zie voor meer informatie over proxyparameters [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest). Zie voor meer informatie over de installatieopties voor Windows, met inbegrip van offline-installatie, [installeren Azure IoT Edge-runtime op Windows](how-to-install-iot-edge-windows.md).

## <a name="configure-the-daemons"></a>De daemons configureren

IoT Edge is afhankelijk van twee daemons die worden uitgevoerd op het IoT Edge-apparaat. De daemon Moby maakt webaanvragen voor pull-containerinstallatiekopieën van container Registry. De daemon IoT Edge maakt webaanvragen om te communiceren met IoT Hub.

Zowel de Moby en de IoT Edge-daemons moeten worden geconfigureerd voor het gebruik van de proxyserver voor de functionaliteit voor actieve apparaten. Deze stap vindt plaats op het IoT Edge-apparaat tijdens de initiële installatie. 

### <a name="moby-daemon"></a>Moby daemon

Omdat Moby is gebouwd op Docker, Raadpleeg de documentatie Docker de daemon Moby configureren met omgevingsvariabelen. De meeste containerregisters (inclusief DockerHub en Azure Container Registry) ondersteuning voor HTTPS-aanvragen, zodat de parameter die u moet instellen is **HTTPS_PROXY**. Als u bent installatiekopieën binnenhaalt vanuit een register dat biedt geen ondersteuning voor transport layer security (TLS), dan moet u instellen de **gebruikt** parameter. 

Kies het artikel dat van toepassing op het besturingssysteem van uw IoT Edge-apparaat: 

* [Docker-daemon op Linux configureren](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
    * De daemon Moby op Linux-apparaten blijft de naam van Docker.
* [Docker-daemon op Windows configureren](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon#proxy-configuration)
    * De daemon Moby op Windows-apparaten wordt iotedge moby genoemd. De namen zijn verschillend, omdat het is mogelijk op zowel de Docker-Desktop- en de Moby parallel uitgevoerd op een Windows-apparaat. 

### <a name="iot-edge-daemon"></a>IoT Edge-daemon

De IoT Edge-daemon is geconfigureerd in een soortgelijke manier als in de daemon Moby. Gebruik de volgende stappen uit om in te stellen van een omgevingsvariabele voor de service, op basis van uw besturingssysteem. 

De daemon IoT Edge maakt altijd gebruik van HTTPS om aanvragen te verzenden naar IoT Hub.

#### <a name="linux"></a>Linux

Een editor openen in de terminal om te configureren van de IoT Edge-daemon. 

```bash
sudo systemctl edit iotedge
```

Voer de volgende tekst vervangen  **\<proxy-URL >** uw proxyserveradres en de poort. Vervolgens opslaan en sluiten. 

```ini
[Service]
Environment="https_proxy=<proxy URL>"
```

Vernieuwen van de servicemanager om op te halen de nieuwe configuratie voor IoT Edge.

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

## <a name="configure-the-iot-edge-agent"></a>De IoT Edge-agent configureren

De IoT Edge-agent is de eerste module te starten op elk IoT Edge-apparaat. Deze gestart voor het eerst op basis van de informatie in de IoT Edge config.yaml-bestand. De IoT Edge-agent maakt vervolgens verbinding met IoT Hub om op te halen van implementatie-manifesten die welke andere modules moeten worden geïmplementeerd op het apparaat.

Deze stap vindt plaats één keer op het IoT Edge-apparaat tijdens de initiële installatie. 

1. Open het bestand config.yaml op uw IoT Edge-apparaat. Op Linux-systemen, dit bestand bevindt zich in **/etc/iotedge/config.yaml**. Op Windows-systemen, dit bestand bevindt zich in **C:\ProgramData\iotedge\config.yaml**. Het configuratiebestand is beveiligd, moet u de Administrator-bevoegdheden om deze te openen. Op Linux-systemen, gebruikt u de `sudo` opdracht voor het openen van het bestand in uw favoriete teksteditor. Open een teksteditor zoals Kladblok als administrator op Windows, en open vervolgens het bestand. 

2. Zoek in het bestand config.yaml de **Edge Agent-module spec** sectie. De definitie van de IoT Edge-agent bevat een **env** parameter waar u omgevingsvariabelen kunt toevoegen. 

3. Verwijder de accolades die tijdelijke aanduidingen voor de parameter env zijn, en voeg de nieuwe variabele op een nieuwe regel toe. Houd er rekening mee dat streepjes in YAML twee spaties zijn. 

   ```yaml
   https_proxy: "<proxy URL>"
   ```

4. IoT Edge-runtime gebruikt AMQP standaard om te communiceren met IoT Hub. Sommige proxyservers blokkeren AMQP poorten. Als dat het geval is, klikt u vervolgens moet u ook het configureren van edgeAgent voor het gebruik van AMQP via WebSocket. Een tweede omgevingsvariabele toevoegen.

   ```yaml
   UpstreamProtocol: "AmqpWs"
   ```

   ![edgeAgent definitie met omgevingsvariabelen](./media/how-to-configure-proxy-support/edgeagent-edited.png)

5. Sla de wijzigingen in config.yaml en sluit de editor. Start opnieuw op IoT Edge om de wijzigingen te laten treden. 

   * Linux: 

      ```bash
      sudo systemctl restart iotedge
      ```

   * Windows:

      ```powershell
      Restart-Service iotedge
      ```

## <a name="configure-deployment-manifests"></a>Manifesten implementatie configureren  

Nadat uw IoT Edge-apparaat is geconfigureerd om te werken met uw proxy-server, moet u daarmee doorgaan om aan te geven van de omgevingsvariabelen in de toekomst implementatie manifesten. U kunt bewerken implementatie manifesten mogelijk maken via de wizard voor Azure portal of door te bewerken van een implementatie manifest van de JSON-bestand. 

Configureer altijd twee runtimemodules, edgeAgent en edgeHub, om te communiceren via de proxy-server, zodat ze een verbinding met IoT Hub kunnen onderhouden. Als u de proxy-informatie uit de module edgeAgent verwijdert, is de enige manier om de verbinding opnieuw tot stand door het bewerken van het bestand config.yaml op het apparaat, zoals beschreven in de vorige sectie. 

Andere IoT Edge-modules die verbinding met internet maken moeten worden geconfigureerd om te communiceren via de proxy-server. Modules die hun via doorsturen edgeHub of die alleen communiceren met andere modules op het apparaat hoeft echter niet de gegevens van de proxy-server. 

Deze stap is continue gedurende de levensduur van het IoT Edge-apparaat. 

### <a name="azure-portal"></a>Azure Portal

Wanneer u gebruikt de **modules instellen** wizard om implementaties te maken voor IoT Edge-apparaten, elke module heeft een **omgevingsvariabelen** sectie die u gebruiken kunt om proxy serververbindingen te configureren. 

Selecteer voor het configureren van de IoT Edge-agent en de IoT Edge hub-modules, **geavanceerde instellingen voor Edge-Runtime configureren** op de eerste stap van de wizard. 

![Geavanceerde runtime-instellingen voor Edge configureren](./media/how-to-configure-proxy-support/configure-runtime.png)

Voeg de **https_proxy** omgevingsvariabele naar de IoT Edge agent- en moduledefinities voor IoT Edge hub. Als u de **UpstreamProtocol** omgevingsvariabele in het bestand config.yaml op uw IoT Edge-apparaat toevoegen die aan de definitie van de IoT Edge agent-module te. 

![Set https_proxy-omgevingsvariabele](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

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

Als u de **UpstreamProtocol** omgevingsvariabele in het bestand confige.yaml op uw IoT Edge-apparaat toevoegen die aan de definitie van de IoT Edge agent-module te. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de functies van de [IoT Edge-runtime](iot-edge-runtime.md).

Installatie en configuratie van fouten met [veelvoorkomende problemen en oplossingen voor Azure IoT Edge](troubleshoot.md)

