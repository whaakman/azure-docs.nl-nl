---
title: Azure IoT Edge installeren op Windows | Microsoft Docs
description: Azure IoT Edge-installatie-instructies voor Windows 10, Windows Server en Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: f67f24cab907c3fe9998704e0a0a85d5b29f60a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808862"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>De Azure IoT Edge-runtime installeren op Windows

De Azure IoT Edge-runtime is wat een apparaat verandert in een IoT Edge-apparaat. De runtime kan worden geïmplementeerd op apparaten als klein is als een Raspberry Pi of even groot zijn als een industrie-server. Wanneer een apparaat is geconfigureerd met de IoT Edge-runtime, kun u bedrijfslogica toe vanuit de cloud implementeren. 

Zie voor meer informatie over IoT Edge-runtime, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md).

In dit artikel vindt u de stappen voor het installeren van de Azure IoT Edge-runtime op uw Windows x64 (AMD/Intel) systeem met behulp van Windows-containers.

> [!NOTE]
> Een bekend probleem van de Windows-besturingssysteem wordt voorkomen dat de overgang naar de slaapstand en stand-bymodus energiestatussen bij het uitvoeren van IoT Edge-modules (Windows Nano Server-containers proces afgeschermd). Dit probleem heeft invloed op de accu van het apparaat.
>
> Als tijdelijke oplossing, gebruikt u de opdracht `Stop-Service iotedge` eventuele actieve IoT Edge-modules stoppen voordat u deze energiestatussen. 

<!--
> [!NOTE]
> Using Linux containers on Windows systems is not a recommended or supported production configuration for Azure IoT Edge. However, it can be used for development and testing purposes.
-->

Met behulp van Linux-containers op Windows-systemen is geen aanbevolen of ondersteund productieconfiguratie voor Azure IoT Edge. Het kan echter worden gebruikt voor ontwikkeling en tests. Zie voor meer informatie, [gebruik IoT Edge op Windows om uit te voeren van Linux-containers](how-to-install-iot-edge-windows-with-linux.md).

Zie voor meer informatie over wat inbegrepen in de meest recente versie van IoT Edge [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

## <a name="prerequisites"></a>Vereisten

Gebruik deze sectie om te controleren of uw Windows-apparaat IoT Edge kan ondersteunen en voorbereidt een container-engine voordat de installatie. 

### <a name="supported-windows-versions"></a>Ondersteunde versies van Windows

Voor ontwikkelings- en scenario's, worden Azure IoT Edge met Windows-containers geïnstalleerd op elke versie van Windows 10 of Windows Server 2019 (build 17763) die ondersteuning biedt voor de containers-functie. Zie voor informatie over welke besturingssystemen die momenteel worden ondersteund voor productiescenario's [Azure IoT Edge ondersteunde systemen](support.md#operating-systems). 

### <a name="prepare-for-a-container-engine"></a>Voorbereiden voor een container-engine 

Azure IoT Edge is afhankelijk van een [OCI-compatibele](https://www.opencontainers.org/) container-engine. Voor productiescenario's, de Moby-engine die is opgenomen in het script voor installatie voor Windows-containers op uw Windows-apparaat te gebruiken. 

## <a name="install-iot-edge-on-a-new-device"></a>IoT Edge op een nieuw apparaat installeren

>[!NOTE]
>Azure IoT Edge-softwarepakketten zijn afhankelijk van de licentievoorwaarden die zich in de pakketten (in de map LICENSE). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

Een PowerShell-script downloadt en installeert de Azure IoT Edge-daemon voor beveiliging. De daemon security start vervolgens de eerste dag van de twee runtimemodules, de IoT Edge-agent, waarmee externe implementaties van andere modules. 

Wanneer u de IoT Edge-runtime voor de eerste keer op een apparaat installeert, moet u voor het inrichten van het apparaat een identiteit van een IoT-hub. Een enkele IoT Edge-apparaat kan worden ingericht handmatig met behulp van een apparaat-verbindingsreeks die is opgegeven door de IoT Hub. Of u de Device Provisioning Service kunt gebruiken voor het automatisch inrichten van apparaten, dit is handig wanneer er veel apparaten zijn om in te stellen. Afhankelijk van inrichting keuze, kiest u het script voor de juiste installatie. 

De volgende secties beschrijven de algemene scenario's en de parameters voor de IoT Edge-script voor installatie op een nieuw apparaat. 

### <a name="option-1-install-and-manually-provision"></a>Optie 1: Installeren en handmatig inrichten

In deze eerste optie, bieden u een **apparaatverbindingsreeks** die worden gegenereerd door de IoT Hub voor het inrichten van het apparaat. 

In dit voorbeeld ziet u een handmatige installatie met Windows-containers:

1. Als u niet hebt gedaan, een nieuwe IoT Edge-apparaat registreren en op te halen de **apparaatverbindingsreeks**. Kopieer de verbindingsreeks voor later gebruik in deze sectie. U kunt deze stap uit met de volgende hulpprogramma's kunt uitvoeren:

   * [Azure Portal](how-to-register-device-portal.md)
   * [Azure-CLI](how-to-register-device-cli.md)
   * [Visual Studio Code](how-to-register-device-vscode.md)

2. Voer PowerShell uit als beheerder.

   >[!NOTE]
   >IoT Edge, niet PowerShell (x86) installeert met behulp van een AMD64-sessie van PowerShell. Als u niet zeker weet welke Sessietype die u gebruikt, moet u de volgende opdracht uitvoeren:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. De **implementeren IoTEdge** opdracht controleert of uw Windows-machine op een ondersteunde versie is, schakelt de functie voor containers en downloadt vervolgens de moby-runtime en de IoT Edge-runtime. De opdracht standaard gebruik van Windows-containers. 

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. Op dit moment IoT Core-apparaten kunnen automatisch opnieuw opgestart. Andere apparaten met Windows 10 of Windows Server mogelijk gevraagd om opnieuw te starten. Als dit het geval is, kunt u uw apparaat opnieuw. Zodra het apparaat gereed is, opnieuw uitvoeren van PowerShell als beheerder.

5. De **initialiseren IoTEdge** opdracht configureert de IoT Edge-runtime op uw computer. De standaardinstellingen van de opdracht voor het inrichten van handmatige met Windows-containers. 

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge
   ```

6. Geef desgevraagd de apparaatverbindingsreeks die u in stap 1 hebt opgehaald. De verbindingsreeks van het apparaat wordt gekoppeld aan het fysieke apparaat met een apparaat-ID van IoT-Hub. 

   De verbindingsreeks heeft de volgende notatie en niet tussen aanhalingstekens moet worden opgenomen: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. Gebruik de stappen in [controleren of geslaagde installatie](#verify-successful-installation) om de status van IoT Edge op uw apparaat te controleren. 

Wanneer u installeert en een apparaat handmatig inrichten, kunt u aanvullende parameters te wijzigen van de installatie, waaronder:
* Direct verkeer om te gaan via een proxyserver
* Het installatieprogramma verwijzen naar een offline-adreslijst
* Een containerinstallatiekopie specifieke agent declareren en geef referenties op als het zich in een persoonlijk register

Voor meer informatie over deze installatieopties, gaat u verder voor meer informatie over [alle installatieparameters](#all-installation-parameters).

### <a name="option-2-install-and-automatically-provision"></a>Optie 2: Installeren en automatisch inrichten

In deze tweede optie, moet u het apparaat met IoT Hub Device Provisioning Service inrichten. Geef de **bereik-ID** van Device Provisioning Service-exemplaar en de **registratie-ID** van uw apparaat.

Het volgende voorbeeld ziet u een automatische installatie met Windows-containers:

1. Volg de stappen in [maken en inrichten van een gesimuleerd TPM IoT Edge-apparaat op Windows](how-to-auto-provision-simulated-device-windows.md) de Device Provisioning Service instellen en ophalen van de **bereik-ID**, TPM-apparaat simuleren en ophalen van de **Registratie-ID**, maakt u een afzonderlijke inschrijving. Nadat het apparaat is geregistreerd in uw IoT-hub, gaat u verder met deze stappen van de installatie.  

   >[!TIP]
   >Houd het venster met de TPM-simulator openen tijdens het installeren en testen. 

2. Voer PowerShell uit als beheerder.

   >[!NOTE]
   >IoT Edge, niet PowerShell (x86) installeert met behulp van een AMD64-sessie van PowerShell. Als u niet zeker weet welke Sessietype die u gebruikt, moet u de volgende opdracht uitvoeren:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. De **implementeren IoTEdge** opdracht controleert of uw Windows-machine op een ondersteunde versie is, schakelt de functie voor containers en downloadt vervolgens de moby-runtime en de IoT Edge-runtime. De opdracht standaard gebruik van Windows-containers. 

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. Op dit moment IoT Core-apparaten kunnen automatisch opnieuw opgestart. Andere apparaten met Windows 10 of Windows Server mogelijk gevraagd om opnieuw te starten. Als dit het geval is, kunt u uw apparaat opnieuw. Zodra het apparaat gereed is, opnieuw uitvoeren van PowerShell als beheerder.

6. De **initialiseren IoTEdge** opdracht configureert de IoT Edge-runtime op uw computer. De standaardinstellingen van de opdracht voor het inrichten van handmatige met Windows-containers. Gebruik de `-Dps` vlag gebruiken van de Device Provisioning Service in plaats van handmatige inrichting.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps
   ```

7. Geef desgevraagd de bereik-ID van uw Device Provisioning Service en de registratie-ID van uw apparaat, die u moet hebt opgehaald in stap 1.

8. Gebruik de stappen in [controleren of geslaagde installatie](#verify-successful-installation) om de status van IoT Edge op uw apparaat te controleren. 

Wanneer u installeert en een apparaat handmatig inrichten, kunt u aanvullende parameters te wijzigen van de installatie, waaronder:
* Direct verkeer om te gaan via een proxyserver
* Het installatieprogramma verwijzen naar een offline-adreslijst
* Een containerinstallatiekopie specifieke agent declareren en geef referenties op als het zich in een persoonlijk register

Voor meer informatie over deze installatieopties, doorgaan met het lezen van dit artikel of Ga voor meer informatie over [alle installatieparameters](#all-installation-parameters).

## <a name="offline-installation"></a>Offline-installatie

Tijdens de installatie van de twee bestanden worden gedownload: 
* Microsoft Azure IoT Edge CAB-bestand, waarin de IoT Edge security-daemon (iotedged), Moby container-engine en Moby CLI.
* Visual C++ redistributable-pakket (VC runtime) msi

U kunt een of beide van deze bestanden vooraf op het apparaat downloaden en vervolgens wijst u het script voor installatie op de map waarin de bestanden. Het installatieprogramma controleert eerst de map en downloadt vervolgens alleen onderdelen die niet zijn gevonden. Als alle bestanden offline beschikbaar zijn, kunt u met geen verbinding met internet installeren. U kunt deze functie ook gebruiken voor het installeren van een specifieke versie van de onderdelen.  

Zie voor de meest recente IoT Edge-installatiebestanden, samen met eerdere versies [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Als u wilt installeren met offline-onderdelen, gebruikt u de `-OfflineInstallationPath` parameter als onderdeel van de implementatie-IoTEdge opdracht en geeft u het absolute pad naar de map. Bijvoorbeeld:

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

U kunt ook de padparameter offline-installatie gebruiken met de opdracht Update IoTEdge, die verderop in dit artikel is geïntroduceerd. 

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Controleer de status van de IoT Edge-service. Deze moet worden weergegeven als het uitvoeren.  

```powershell
Get-Service iotedge
```

Bekijk Logboeken van de laatste 5 minuten. Als u alleen de installatie voltooid IoT Edge-runtime, ziet u mogelijk een lijst van fouten van de tijd tussen actieve **implementeren IoTEdge** en **initialiseren IoTEdge**. Deze fouten worden verwacht, omdat de service probeert te starten voordat het wordt geconfigureerd. 

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Lijst met modules. Na een nieuwe installatie, de enige module ziet u het uitvoeren is **edgeAgent**. Nadat u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md), ziet u anderen. 

```powershell
iotedge list
```

## <a name="manage-module-containers"></a>Module containers beheren

De IoT Edge-service is een container-engine die wordt uitgevoerd op uw apparaat. Wanneer u een module op een apparaat implementeert, gebruikt de IoT Edge-runtime de container-engine voor het ophalen van de container-installatiekopie uit een register in de cloud. De IoT Edge-service kunt u communiceren met uw modules en Logboeken kunt ophalen, maar soms wilt u mogelijk de container-engine gebruiken om te communiceren met de container zelf. 

Zie voor meer informatie over concepten van de module [inzicht in Azure IoT Edge-modules](iot-edge-modules.md). 

Als u Windows-containers op uw Windows IoT Edge-apparaat uitvoert, vervolgens met de installatie van de IoT Edge de engine van de container Moby opgenomen. De engine Moby is gebaseerd op de dezelfde standaarden als Docker, en is ontworpen voor het parallel uitgevoerd op dezelfde computer als Docker-bureaublad. Als u Doelcontainers die worden beheerd door de engine Moby wilt om die reden hebt u specifiek gericht op dat de engine in plaats van Docker. 

Bijvoorbeeld, als u alle Docker-installatiekopieën, gebruik de volgende opdracht:

```powershell
docker images
```

Als u alle Moby installatiekopieën, wijzig de dezelfde opdracht met een verwijzing naar de Moby-engine: 

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

De URI-engine wordt weergegeven in de uitvoer van het script voor installatie, of u kunt deze vinden in de sectie container runtime-instellingen voor het bestand config.yaml. 

![de uri moby_runtime in config.yaml](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Zie voor meer informatie over opdrachten die u kunt gebruiken om te communiceren met containers en afbeeldingen die worden uitgevoerd op uw apparaat [Docker opdrachtregelinterfaces](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="update-an-existing-installation"></a>Een bestaande installatie van update

Als u hebt al IoT Edge-runtime op een apparaat hebt geïnstalleerd en ingericht met een identiteit van de IoT Hub, kunt u de runtime bijwerken zonder dat de apparaatgegevens van uw opnieuw in te voeren. 

Zie voor meer informatie, [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).

In dit voorbeeld ziet u een installatie die verwijst naar een bestaand configuratiebestand en maakt gebruik van Windows-containers: 

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
Update-IoTEdge
```

Wanneer u IoT Edge bijwerkt, kunt u aanvullende parameters te wijzigen van de update, met inbegrip van:
* Direct verkeer om te gaan via een proxyserver of
* Het installatieprogramma verwijzen naar een offline-adreslijst 
* Opnieuw te starten zonder ernaar te vragen, indien nodig

U kunt de containerinstallatiekopie van een IoT Edge-agent met scriptparameters niet declareren omdat deze informatie is al ingesteld in het configuratiebestand van de vorige installatie. Als u wijzigen van de installatiekopie van de agent-container wilt, doen in het bestand config.yaml. 

Voor meer informatie over deze opties bijwerken, gebruikt u de opdracht `Get-Help Update-IoTEdge -full` of ze raadplegen om [alle installatieparameters](#all-installation-parameters).

## <a name="uninstall-iot-edge"></a>Verwijderen van IoT Edge

Als u de installatie van de IoT Edge van uw Windows-apparaat verwijderen wilt, gebruikt u de volgende opdracht uit vanuit een PowerShell-venster met beheerdersrechten. Met deze opdracht verwijdert u de IoT Edge-runtime, samen met uw bestaande configuratie en de gegevens van de engine Moby. 

```powershell
Uninstall-IoTEdge
```

De opdracht verwijderen IoTEdge werkt niet op Windows IoT Core. Als u wilt verwijderen IoT Edge van Windows IoT Core-apparaten, moet u uw Windows IoT Core-installatiekopie implementeren. 

Voor meer informatie over opties voor verwijderen, gebruikt u de opdracht `Get-Help Uninstall-IoTEdge -full`. 

## <a name="all-installation-parameters"></a>Alle parameters van de installatie

In de vorige secties geïntroduceerd voor algemene scenario's met voorbeelden van hoe u kunt parameters gebruiken om te wijzigen van het script voor installatie. Deze sectie bevat naslaginformatie over tabellen van de algemene parameters gebruikt om te installeren, bijwerken of verwijderen van IoT Edge. 

### <a name="deploy-iotedge"></a>Deploy-IoTEdge

De opdracht implementeren IoTEdge downloadt en implementeert het IoT Edge Security-Daemon en de bijbehorende afhankelijkheden. De implementatieopdracht accepteert deze algemene parameters, onder andere. Gebruik de opdracht voor de volledige lijst `Get-Help Deploy-IoTEdge -full`.  

| Parameter | Geaccepteerde waarden | Opmerkingen |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** of **Linux** | Als er geen besturingssysteem van de container is opgegeven, is Windows de standaardwaarde.<br><br>IoT Edge gebruikt voor Windows-containers, opgenomen in de installatie van moby-container-engine. Voor Linux-containers moet u een container-engine installeren voordat u begint de installatie. |
| **Proxy** | Proxy-URL | Deze parameter opgeeft als het apparaat via een proxyserver moet te bereiken van het internet. Zie voor meer informatie, [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Pad naar map | Als deze parameter opgenomen is, wordt het installatieprogramma de genoemde map voor de IoT Edge CAB-bestand en VC Runtime MSI-bestanden die zijn vereist voor de installatie gecontroleerd. Alle bestanden die niet is gevonden in de map worden gedownload. Als beide bestanden zich in de directory, kunt u IoT Edge kunt installeren zonder internetverbinding. Gebruik deze parameter kunt u ook een specifieke versie te gebruiken. |
| **InvokeWebRequestParameters** | Hash-tabel van de parameters en waarden | Tijdens de installatie van de verschillende webaanvragen worden gedaan. Dit veld gebruiken om in te stellen van parameters voor deze webaanvragen. Deze parameter is handig om de referenties voor de proxy-servers configureren. Zie voor meer informatie, [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md). |
| **RestartIfNeeded** | Geen | Met deze markering kan het implementatiescript de machine opnieuw te starten zonder te vragen, indien nodig. |

### <a name="initialize-iotedge"></a>Initialize-IoTEdge

De opdracht initialiseren IoTEdge configureert IoT Edge met de apparaatverbindingsreeks en operationele details. Veel van de gegevens die worden gegenereerd door deze opdracht wordt vervolgens opgeslagen in het bestand iotedge\config.yaml. De initialisatie-opdracht accepteert deze algemene parameters, onder andere. Gebruik de opdracht voor de volledige lijst `Get-Help Initialize-IoTEdge -full`. 

| Parameter | Geaccepteerde waarden | Opmerkingen |
| --------- | --------------- | -------- |
| **Handmatig** | Geen | **Overschakelen van de parameter**. Als er geen inrichtingstype is opgegeven, wordt handmatige is de standaardwaarde.<br><br>Verklaart dat u de verbindingsreeks van een apparaat voor het inrichten van het apparaat handmatig wordt opgeven |
| **Dps** | Geen | **Overschakelen van de parameter**. Als er geen inrichtingstype is opgegeven, wordt handmatige is de standaardwaarde.<br><br>Verklaart dat u een Device Provisioning Service (DPS) bereik-ID en de registratie-ID van uw apparaat om in te richten via DPS wordt verstrekt.  |
| **DeviceConnectionString** | Een verbindingsreeks vanuit een IoT Edge-apparaat is geregistreerd in een IoT-Hub, tussen enkele aanhalingstekens | **Vereiste** voor handmatige installatie. Als u een verbindingsreeks in de scriptparameters niet opgeeft, wordt u gevraagd om een tijdens de installatie. |
| **ScopeId** | Een bereik-ID van een exemplaar van Device Provisioning Service die is gekoppeld aan uw IoT-Hub. | **Vereiste** voor de DPS-installatie. Als u een bereik-ID in de scriptparameters niet opgeeft, wordt u gevraagd om een tijdens de installatie. |
| **RegistrationId** | Een registratie-ID die is gegenereerd door uw apparaat | **Vereiste** voor de DPS-installatie. Als u een registratie-ID in de scriptparameters niet opgeeft, wordt u gevraagd om een tijdens de installatie. |
| **ContainerOs** | **Windows** of **Linux** | Als er geen besturingssysteem van de container is opgegeven, is Windows de standaardwaarde.<br><br>IoT Edge gebruikt voor Windows-containers, opgenomen in de installatie van moby-container-engine. Voor Linux-containers moet u een container-engine installeren voordat u begint de installatie. |
| **InvokeWebRequestParameters** | Hash-tabel van de parameters en waarden | Tijdens de installatie van de verschillende webaanvragen worden gedaan. Dit veld gebruiken om in te stellen van parameters voor deze webaanvragen. Deze parameter is handig om de referenties voor de proxy-servers configureren. Zie voor meer informatie, [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md). |
| **AgentImage** | URI van de IoT Edge-agent-installatiekopie | Een nieuwe installatie van de IoT Edge gebruikt standaard de meest recente rolling-tag voor de installatiekopie van het IoT Edge-agent. Gebruik deze parameter om in te stellen van een specifieke tag voor de versie van de installatiekopie, of om uw eigen installatiekopie agent. Zie voor meer informatie, [inzicht in IoT Edge tags](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Gebruikersnaam** | Container registry gebruikersnaam | Gebruik deze parameter alleen als u de parameter - AgentImage ingesteld op een container in een persoonlijk register. Een gebruikersnaam opgeven met toegang tot het register. |
| **Wachtwoord** | Beveiligd-wachtwoordverificatie-tekenreeks | Gebruik deze parameter alleen als u de parameter - AgentImage ingesteld op een container in een persoonlijk register. Geef het wachtwoord voor toegang tot het register. | 

### <a name="update-iotedge"></a>Update-IoTEdge

| Parameter | Geaccepteerde waarden | Opmerkingen |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** of **Linux** | Als er geen container die besturingssysteem is opgegeven, is Windows de standaardwaarde. Voor Windows-containers, wordt een container-engine worden opgenomen in de installatie. Voor Linux-containers moet u een container-engine installeren voordat u begint de installatie. |
| **Proxy** | Proxy-URL | Deze parameter opgeeft als het apparaat via een proxyserver moet te bereiken van het internet. Zie voor meer informatie, [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Hash-tabel van de parameters en waarden | Tijdens de installatie van de verschillende webaanvragen worden gedaan. Dit veld gebruiken om in te stellen van parameters voor deze webaanvragen. Deze parameter is handig om de referenties voor de proxy-servers configureren. Zie voor meer informatie, [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Pad naar map | Als deze parameter opgenomen is, wordt het installatieprogramma de genoemde map voor de IoT Edge CAB-bestand en VC Runtime MSI-bestanden die zijn vereist voor de installatie gecontroleerd. Alle bestanden die niet is gevonden in de map worden gedownload. Als beide bestanden zich in de directory, kunt u IoT Edge kunt installeren zonder internetverbinding. Gebruik deze parameter kunt u ook een specifieke versie te gebruiken. |
| **RestartIfNeeded** | Geen | Met deze markering kan het implementatiescript de machine opnieuw te starten zonder te vragen, indien nodig. |


### <a name="uninstall-iotedge"></a>Uninstall-IoTEdge

| Parameter | Geaccepteerde waarden | Opmerkingen |
| --------- | --------------- | -------- |
| **Force** | Geen | Met deze vlag wordt het verwijderen in het geval de eerdere poging om te verwijderen mislukt is. 
| **RestartIfNeeded** | Geen | Met deze markering kan het uninstall-script dat de machine opnieuw te starten zonder te vragen, indien nodig. |


## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat dat is ingericht met de runtime geïnstalleerd hebt, kunt u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md).

Als u hebt met het IoT Edge niet goed geïnstalleerd problemen, bekijk de [probleemoplossing](troubleshoot.md) pagina.

Als u wilt een bestaande installatie bijwerken naar de nieuwste versie van IoT Edge, Zie [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).
