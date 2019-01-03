---
title: Voorbereiden van apparaten en implementaties voor de productie - Azure IoT Edge | Microsoft Docs
description: Informatie over het uitvoeren van uw Azure IoT Edge-oplossing van ontwikkeling tot productie, met inbegrip van uw apparaten instellen met de juiste certificaten en het aanbrengen van een implementatieplan voor toekomstige code-updates.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 86b33bfa0f5383ac68080e2f8f7f9a004a1364a0
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53652600"
---
# <a name="prepare-to-deploy-your-iot-edge-solution-in-production"></a>Voorbereidingen voor het implementeren van uw IoT Edge-oplossing in productie

Wanneer u klaar zijn bent voor uw IoT Edge-oplossing van ontwikkeling naar productie, zorg ervoor dat deze geconfigureerd voor continue uitvoering.

De informatie in dit artikel is niet gelijk. Als u wilt, kunt u prioriteiten elke sectie begint met lijsten die het werk te in twee secties verdelen: **belangrijk** om te voltooien voordat u doorgaat naar de productie, of **nuttig** voor u.

## <a name="device-configuration"></a>Apparaatconfiguratie

IoT Edge-apparaten kunnen van alles uit een Raspberry Pi tot een laptop met een virtuele machine op een server zijn. U hebt mogelijk toegang tot het apparaat fysiek of via een virtuele-verbinding of deze kan worden geïsoleerd gedurende langere tijd wordt opgelost. In beide gevallen die u wilt om ervoor te zorgen dat het om uit te voeren op de juiste wijze geconfigureerd. 

* **Belangrijk**
    * Productiecertificaten installeren
    * Plan voor een apparaat hebt
    * Moby gebruiken als de container-engine

* **Nuttige**
    * Kies upstream protocol

### <a name="install-production-certificates"></a>Productiecertificaten installeren

Elke IoT Edge-apparaat in de productieomgeving moet een certificaat apparaten certificaat certificeringsinstantie (CA) is geïnstalleerd. Deze CA-certificaat is vervolgens naar de IoT Edge-runtime in het bestand config.yaml gedeclareerd. Als u wilt maken, ontwikkelen en testen vereenvoudigen, de IoT Edge maakt runtime tijdelijke certificaten als er geen certificaten zijn gedefinieerd in het bestand config.yaml. Deze tijdelijke certificaten zijn echter verloopt na drie maanden en niet is beveiligd voor productiescenario's. 

Zie voor meer informatie over de rol van het CA-apparaatcertificaat, [hoe Azure IoT Edge maakt gebruik van certificaten](iot-edge-certs.md).

Zie voor meer informatie over het installeren van certificaten op een IoT Edge-apparaat en ernaar te verwijzen uit het bestand config.yaml [configureren van een IoT Edge-apparaat om te fungeren als een transparante gateway](how-to-create-transparent-gateway.md). De stappen voor het configureren van de certificaten zijn hetzelfde, of het apparaat zal worden gebruikt als een gateway of niet. Dit artikel bevat de scripts voor het genereren van certificaten voorbeeld alleen voor testdoeleinden. Gebruik deze voorbeeld-certificaten niet in de productieomgeving. 

### <a name="have-a-device-management-plan"></a>Plan voor een apparaat hebt

Voordat u een apparaat in de productieomgeving moet u weten hoe u van plan bent om toekomstige updates te beheren. Voor een IoT Edge-apparaat, de lijst van onderdelen om bij te werken kan omvatten:

* Apparaatfirmware
* Besturingssysteem-bibliotheken
* Container-engine, zoals Moby
* IoT Edge-daemon
* CA-certificaten

Zie voor stappen voor het bijwerken van de IoT Edge-daemon, [bijwerken van de IoT Edge-runtime](how-to-update-iot-edge.md). De huidige methoden voor het bijwerken van de IoT Edge-daemon is vereist voor fysieke of SSH-toegang tot de IoT Edge-apparaat. Als u er veel apparaten zijn om bij te werken, kunt u de updatestappen toevoegen aan een script of een automation-hulpprogramma, zoals Ansible gebruiken om updates op schaal.

### <a name="use-moby-as-the-container-engine"></a>Moby gebruiken als de container-engine

Een container-engine die op een apparaat is een vereiste voor elk IoT Edge-apparaat. Alleen moby-engine wordt ondersteund in de productieomgeving. Andere container-engines, zoals Docker, werken met IoT Edge en het is om deze engines gebruiken voor ontwikkeling. De moby-engine kan opnieuw worden gedistribueerd, gebruikt in combinatie met Azure IoT Edge en Microsoft biedt onderhoud van deze engine. Andere container-engines gebruik van een IoT Edge-apparaat wordt niet ondersteund.

### <a name="choose-upstream-protocol"></a>Kies upstream protocol

Het protocol (en dus de poort die wordt gebruikt) voor de upstream-communicatie naar IoT Hub kan worden geconfigureerd voor zowel de Edge agent en Edge hub. Het standaard-protocol is AMQP, maar u mogelijk wilt wijzigen, afhankelijk van uw netwerkinstellingen. 

De twee runtimemodules beide hebben een **UpstreamProtocol** omgevingsvariabele. De geldige waarden voor de variabele zijn: 

* MQTT
* AMQP
* MQTTWS
* AMQPWS

De variabele UpstreamProtocol configureren voor de Edge agent in het bestand config.yaml op het apparaat zelf. Als uw IoT Edge-apparaat zich achter een proxyserver die blokkeert AMQP-poorten, moet u er bijvoorbeeld voor kiezen het configureren van de Edge-agent voor het gebruik van de AMQP via WebSocket (AMQPWS) voor de eerste verbinding met IoT Hub. 

Zodra uw IoT Edge-apparaat verbinding maakt, moet u gaan met het configureren van de variabele UpstreamProtocol voor beide runtimemodules in latere implementaties. Een voorbeeld van dit proces vindt u in [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md).

## <a name="deployment"></a>Implementatie

* **Nuttige**
    * Consistent zijn met upstream-protocol
    * Geheugenruimte die worden gebruikt door de Edge hub beperken
    * Gebruik geen foutopsporingsversies van module afbeeldingen

### <a name="be-consistent-with-upstream-protocol"></a>Consistent zijn met upstream-protocol

Als u de Edge agent geconfigureerd op uw IoT Edge-apparaat een ander protocol dan de standaardwaarde van AMQP gebruiken, moet u hetzelfde protocol in alle daaropvolgende implementaties declareren. Bijvoorbeeld, als uw IoT Edge-apparaat zich achter een proxyserver die blokkeert AMQP-poorten, u waarschijnlijk geconfigureerd het apparaat verbinding maakt via AMQP via WebSocket (AMQPWS). Wanneer u modules op het apparaat implementeren als u de dezelfde APQPWS-protocol voor de Edge agent en Edge hub niet configureert, wordt de standaardwaarde AMQP overschrijven de instellingen en te voorkomen dat u opnieuw verbinding te maken. 

U hoeft alleen te configureren van de omgevingsvariabele UpstreamProtocol voor de Edge agent en Edge hub-modules. Alle aanvullende modules vast welke protocol is ingesteld in de runtimemodules. 

Een voorbeeld van dit proces vindt u in [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md).

### <a name="reduce-memory-space-used-by-edge-hub"></a>Geheugenruimte die worden gebruikt door de Edge hub beperken

Als u beperkte apparaten met beperkt geheugen beschikbaar implementeert, kunt u Edge hub om te worden uitgevoerd in een meer gestroomlijnde capaciteit en gebruik minder schijfruimte configureren. Deze configuraties de prestaties van de Edge hub echter beperken, zodat de juiste balans vinden die geschikt is voor uw oplossing. 

#### <a name="dont-optimize-for-performance-on-constrained-devices"></a>Geen optimale prestaties op beperkte apparaten

De Edge hub is geoptimaliseerd voor prestaties standaard, zodat deze probeert toe te wijzen van grote hoeveelheden geheugen. Deze configuratie kan stabiliteitsproblemen veroorzaken op kleinere apparaten, zoals de Raspberry Pi. Als u apparaten met beperkte resources implementeert, kunt u om in te stellen de **OptimizeForPerformance** omgevingsvariabele **false** op de Edge hub. 

Zie voor meer informatie, [stabiliteitsproblemen voor de resource beperkte apparaten](troubleshoot.md#stability-issues-on-resource-constrained-devices).

#### <a name="disable-unused-protocols"></a>Niet-gebruikte protocollen uitschakelen

Er is een andere manier om de prestaties van Edge hub te optimaliseren en het geheugengebruik te verminderen om uit de hoofden protocol voor alle protocollen die u niet in uw oplossing te schakelen. 

Protocol koppen zijn geconfigureerd door in te stellen Booleaanse omgevingsvariabelen voor de Edge hub-module in de manifesten van uw implementatie. Zijn de drie variabelen:

* **amqpSettings__enabled**
* **mqttSettings__enabled**
* **httpSettings__enabled**

Alle drie variabelen hebben *twee onderstrepingstekens* en kan worden ingesteld op waar of ONWAAR. 

#### <a name="reduce-storage-time-for-messages"></a>Minder opslagtijd van berichten

De Edge hub-module worden berichten tijdelijk opgeslagen als ze naar IoT Hub voor een of andere reden kunnen niet worden bezorgd. U kunt configureren hoe lang de Edge hub bevat op niet-bezorgde berichten voordat om ze te laten verlopen. Als u problemen voor het geheugen op uw apparaat hebt, kunt u verlagen de **timeToLiveSecs** waarde in de moduledubbel Edge hub. 

De standaardwaarde van de parameter timeToLiveSecs is 7200 seconden, die twee uur is. 

### <a name="do-not-use-debug-versions-of-module-images"></a>Gebruik geen foutopsporingsversies van module afbeeldingen

Bij het verplaatsen van testscenario's naar productie scenario's, moet u foutopsporing configuraties van de manifesten implementatie verwijderen. Controleer of geen van de module-installatiekopieën in de manifesten implementatie hebben de  **\.debug** achtervoegsel. Als u opties om de poorten in de modules voor foutopsporing, beschikbaar te maken hebt toegevoegd en opties voor verwijderen die zijn gemaakt. 

## <a name="container-management"></a>Containerbeheer

* **Belangrijk**
    * Toegang tot uw containerregister beheren
    * Tags gebruiken voor het beheren van versies

### <a name="manage-access-to-your-container-registry"></a>Toegang tot uw containerregister beheren

Voordat u modules naar productie IoT Edge-apparaten implementeert, controleert u of toegang tot uw container registry te beheren zodat andere gebruikers geen toegang tot of wijzigingen in uw containerinstallatiekopieën aanbrengen. Een persoonlijke, niet-openbare container registry gebruiken voor het beheren van containerinstallatiekopieën. 

In de zelfstudies en andere documentatie vertelt u dezelfde container registerreferenties op uw IoT Edge-apparaat gebruiken als u op uw ontwikkelcomputer. Deze instructies zijn alleen bedoeld om u te helpen bij het instellen van test- en ontwikkelomgevingen eenvoudiger en niet in een productiescenario voor moeten worden gevolgd. Azure Container Registry raadt [verificatie met service-principals](../container-registry/container-registry-auth-service-principal.md) als toepassingen of services Haal containerinstallatiekopieën op in een geautomatiseerde of anderszins onbeheerd, zoals IoT Edge-apparaten. Een service-principal maken met alleen-lezen toegang tot uw containerregister, en die gebruikersnaam opgeven en het wachtwoord in het manifest van de implementatie.

### <a name="use-tags-to-manage-versions"></a>Tags gebruiken voor het beheren van versies

Een label is een Docker-concept kunt u onderscheid maken tussen de versies van Docker-containers. Tags zijn achtervoegsels, zoals **1.0** die gaat u aan het einde van de containeropslagplaats van een. Bijvoorbeeld, **mcr.microsoft.com/azureiotedge-agent:1.0**. Tags zijn veranderlijke en kunnen worden gewijzigd om te verwijzen naar een andere container op elk gewenst moment, zodat uw team op een overeenkomst te volgen bij het bijwerken van uw module installatiekopieën vooruit moet overeenkomen. 

Labels helpen u om af te dwingen van updates op uw IoT Edge-apparaten. Wanneer u een bijgewerkte versie van een module naar het containerregister hebt gepusht, verhogen de tag. Push vervolgens een nieuwe implementatie naar uw apparaten met de tag verhoogd. De container-engine herkent de tag verhoogd als een nieuwe versie en de meest recente moduleversie naar uw apparaat wordt opgehaald. 

Zie voor een voorbeeld van een overeenkomst tag [bijwerken van de IoT Edge-runtime](how-to-update-iot-edge.md#understand-iot-edge-tags) voor meer informatie over hoe IoT Edge rolling tags en specifieke tags gebruikt voor het bijhouden van versies. 

## <a name="networking"></a>Netwerken

* **Nuttige**
    * Uitgaand/inkomende configuratie controleren
    * Lijst met toegestane adressen verbindingen
    * Communicatie via een proxy configureren

### <a name="review-outboundinbound-configuration"></a>Uitgaand/inkomende configuratie controleren

Communicatiekanalen tussen Azure IoT Hub en IoT Edge worden altijd geconfigureerd om uitgaande. Voor de meeste scenario's voor IoT Edge zijn alleen voor de drie verbindingen nodig. De container-engine moet verbinding maken met het containerregister (of registers) waarin de module-installatiekopieën. IoT Edge-runtime moet verbinding maken met IoT Hub voor het ophalen van informatie over de configuratie van apparaten en voor het verzenden van berichten en telemetrie. En als u automatische inrichting, het IoT Edge-daemon moet verbinding maken met de Device Provisioning Service. Zie voor meer informatie, [Firewall en configuratieregels poort](troubleshoot.md#firewall-and-port-configuration-rules-for-iot-edge-deployment).

### <a name="whitelist-connections"></a>Lijst met toegestane adressen verbindingen

Wanneer de VPN-configuratie is vereist dat u expliciet verbindingen van de goedgekeurde lijst van IoT Edge-apparaten gemaakt, controleert u de volgende lijst van IoT Edge-onderdelen:

* **IoT Edge agent** een permanente verbinding via AMQP/MQTT naar IoT-Hub, mogelijk wordt geopend via WebSockets. 
* **IoT Edge hub** mogelijk meerdere MQTT-verbindingen naar IoT Hub, of één permanente AMQP-verbinding opent via WebSockets. 
* **IoT Edge-daemon** doet onregelmatige HTTPS-aanroepen naar IoT Hub. 

In alle drie gevallen moet de DNS-naam kan overeenkomen met het patroon \*.azure-devices.net. 

Bovendien de **Container engine** containerregisters opgeroepen via HTTPS. Als u wilt de containerinstallatiekopieën van de IoT Edge-runtime ophalen, is de DNS-naam mcr.microsoft.com. De container-engine maakt verbinding met andere registers zoals geconfigureerd in de implementatie. 

Deze controlelijst is een beginpunt voor firewall-regels:

   | URL (\* = jokertekens) | Uitgaande TCP-poorten | Gebruik |
   | ----- | ----- | ----- |
   | MCR.Microsoft.com  | 443 | Microsoft-containerregister |
   | Global.Azure-apparaten-provisioning.net  | 443 | DPS-toegang (optioneel) |
   | \*. azurecr.io | 443 | Persoonlijke en 3e partij container Registry |
   | \*.blob.core.windows.net | 443 | Het downloaden van de installatiekopie van delta 's | 
   | \*.Azure-devices.net | 5671, 8883, 443 | IoT Hub-toegang |
   | \*. docker.io  | 443 | Docker-toegang (optioneel) |

### <a name="configure-communication-through-a-proxy"></a>Communicatie via een proxy configureren

Als uw apparaten worden geïmplementeerd op een netwerk dat gebruikmaakt van een proxyserver, moeten ze kunnen communiceren via de proxy om IoT-Hub en containerregisters te bereiken. Zie voor meer informatie, [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md).

## <a name="solution-management"></a>Voor oplossingsbeheer

* **Nuttige**
    * Instellen van Logboeken en diagnostische gegevens
    * U kunt tests en CI/CD-pijplijnen

### <a name="set-up-logs-and-diagnostics"></a>Instellen van Logboeken en diagnostische gegevens

Op Linux gebruikt de IoT Edge-daemon journald als het standaard-stuurprogramma voor logboekregistratie. U kunt het opdrachtregelhulpprogramma `journalctl` query uitvoeren op de daemon-Logboeken. Op Windows gebruikt de IoT Edge-daemon PowerShell diagnostische gegevens. Gebruik `Get-WinEvent` naar Logboeken vanuit de daemon voor query's. IoT Edge-modules gebruiken de JSON-stuurprogramma voor logboekregistratie, dit de standaardinstelling voor Docker is.  

Wanneer u een IoT Edge-implementatie test, kunt u uw apparaten op te halen van Logboeken en oplossen van meestal openen. In een implementatiescenario, hebt u mogelijk geen die optie. Houd rekening met hoe u gegevens verzamelen over uw apparaten in productie gaat. Een optie is het gebruik van een logboekregistratiemodule die u verzamelt gegevens van de andere modules en verzendt dit naar de cloud. Een voorbeeld van een logboekregistratiemodule is [logspout loganalytics](https://github.com/veyalla/logspout-loganalytics), of u kunt ze zelf kunt ontwerpen. 

Als u bang over Logboeken te groot is op een apparaat resource beperkt bent, hebt u een aantal opties voor het gebruik van geheugen verminderen. 

* U kunt de grootte van alle docker-logboekbestanden in de Docker-daemon zelf specifiek beperken. Voor Linux, configureert u de daemon op `/etc/docker/daemon.json`. Voor Windows, `C:\ProgramData\docker\confige\daemon.json`. 
* Als u de grootte van het logboekbestand voor elke container aanpassen wilt, kunt u dit doen in de CreateOptions van elke module. 
* Docker voor het beheren van logboeken automatisch door de instelling journald als de standaard stuurprogramma logboekregistratie voor Docker configureren. 
* Oude logboeken van uw apparaat periodiek verwijderen door het installeren van een logrotate-hulpprogramma voor Docker. Gebruik de specificatie van het volgende bestand: 

   ```
   /var/lib/docker/containers/*/*-json.log{
       copytruncate
       daily
       rotate7
       delaycompress
       compress
       notifempty
       missingok
   }
   ```

### <a name="consider-tests-and-cicd-pipelines"></a>U kunt tests en CI/CD-pijplijnen

Voor de meest efficiënte implementatiescenario van de IoT Edge kunt u uw productie-implementatie integreren in uw test- en CI/CD-pijplijnen. Azure IoT Edge biedt ondersteuning voor meerdere CI/CD-platformen, waaronder Azure DevOps. Zie voor meer informatie, [continue integratie en continue implementatie voor Azure IoT Edge](how-to-ci-cd.md).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [automatische implementatie van IoT Edge](module-deployment-monitoring.md).
* Zie hoe IoT Edge ondersteunt [continue integratie en continue implementatie](how-to-ci-cd.md).
