---
title: Azure IoT Edge installeren op Windows | Microsoft Docs
description: Azure IoT Edge-installatie-instructies voor Windows 10, Windows Server en Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 27478de68cde9a097dcc160a4553839aef9a018c
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902802"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>De Azure IoT Edge-runtime installeren op Windows

De Azure IoT Edge-runtime is wat een apparaat verandert in een IoT Edge-apparaat. De runtime kan worden geïmplementeerd op apparaten als klein is als een Raspberry Pi of even groot zijn als een industrie-server. Wanneer een apparaat is geconfigureerd met de IoT Edge-runtime, kun u bedrijfslogica toe vanuit de cloud implementeren. 

Zie voor meer informatie over IoT Edge-runtime, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md).

In dit artikel vindt u de stappen voor het installeren van de Azure IoT Edge-runtime op uw Windows x64 (AMD/Intel) systeem. Windows-ondersteuning is momenteel in Preview.

>[!NOTE]
Met behulp van Linux-containers op Windows-systemen is geen aanbevolen of ondersteund productieconfiguratie voor Azure IoT Edge. Het kan echter worden gebruikt voor ontwikkeling en tests.

## <a name="prerequisites"></a>Vereisten

Gebruik deze sectie om te controleren of uw Windows-apparaat IoT Edge kan ondersteunen en voorbereidt een container-engine voordat de installatie. 

### <a name="supported-windows-versions"></a>Ondersteunde versies van Windows

Azure IoT Edge ondersteunt verschillende versies van Windows, afhankelijk van of u Windows-containers of Linux-containers uitvoert. 

De nieuwste versie van Azure IoT Edge met Windows-containers kunt uitvoeren op de volgende versies van Windows:
* Windows 10 of IoT Core met de update van oktober 2018 (build 17763)
* Windows Server 2019 (build 17763)

De meest recente versie van Azure IoT Edge met Linux-containers kunt uitvoeren op de volgende versies van Windows: 
* Windows 10 jubileumupdate (build 14393) of hoger
* Windows Server 2016 of hoger

Zie voor meer informatie over welke besturingssystemen worden ondersteund, [ondersteuning voor Azure IoT Edge](support.md#operating-systems). 

Zie voor meer informatie over wat inbegrepen in de meest recente versie van IoT Edge, [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

### <a name="prepare-for-a-container-engine"></a>Voorbereiden voor een container-engine 

Azure IoT Edge is afhankelijk van een [OCI-compatibele](https://www.opencontainers.org/) container-engine. Voor productiescenario's, de Moby-engine die is opgenomen in het script voor installatie voor Windows-containers op uw Windows-apparaat te gebruiken. Voor het ontwikkelen en testen, kunt u Linux-containers uitvoeren op uw Windows-apparaat, maar u moet installeren en configureren van een container-engine voor de installatie van IoT Edge. Voor beide scenario's, Zie de volgende secties voor vereisten voor het voorbereiden van het apparaat. 

#### <a name="moby-engine-for-windows-containers"></a>Moby engine voor het Windows-containers

Moby is voor Windows-apparaten met IoT Edge in productiescenario's, de enige officieel ondersteunde container-engine. Het script voor installatie installeert automatisch de Moby-engine op uw apparaat voor de installatie van IoT Edge. Voorbereiden van het apparaat door het inschakelen van de Containers-functie. 

1. Zoek in de startbalk **schakelt u Windows-onderdelen in- of uitschakelen** en open het Configuratiescherm van het besturingselement.
2. Zoek en selecteer **Containers**.
3. Selecteer **OK**. 

#### <a name="docker-for-linux-containers"></a>Docker voor Linux-containers

Als u Windows gebruikt om te ontwikkelen en testen van containers voor Linux-apparaten, kunt u [Docker voor Windows](https://www.docker.com/docker-windows) als uw container-engine. Docker kan worden geconfigureerd voor [Linux-containers gebruiken](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). U moet Docker installeren en configureren voor de installatie van IoT Edge. Linux-containers worden niet ondersteund op Windows-apparaten in productie. 

Als uw IoT Edge-apparaat een Windows-computer is, controleert u of het voldoet aan de [systeemvereisten](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) voor Hyper-V. Als het een virtuele machine is, schakelt u [geneste virtualisatie](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) in en wijst u ten minste 2 GB geheugen toe.

## <a name="install-iot-edge-on-a-new-device"></a>IoT Edge op een nieuw apparaat installeren

>[!NOTE]
>Azure IoT Edge-softwarepakketten zijn afhankelijk van de licentievoorwaarden die zich in de pakketten (in de map LICENSE). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

Een PowerShell-script downloadt en installeert de Azure IoT Edge-daemon voor beveiliging. De daemon security start vervolgens de eerste dag van de twee runtimemodules, de IoT Edge-agent, waarmee externe implementaties van andere modules. 

Wanneer u de IoT Edge-runtime voor de eerste keer op een apparaat installeert, moet u voor het inrichten van het apparaat een identiteit van een IoT-hub. Een enkele IoT Edge-apparaat kan worden ingericht handmatig met behulp van een apparaat verbindingen tekenreeks opgegeven door de IoT Hub. Of u de Device Provisioning Service kunt gebruiken voor het automatisch inrichten van apparaten, dit is handig wanneer er veel apparaten zijn om in te stellen. Afhankelijk van inrichting keuze, kiest u het script voor de juiste installatie. 

De volgende secties beschrijven de algemene scenario's en de parameters voor de IoT Edge-script voor installatie op een nieuw apparaat. 

### <a name="option-1-install-and-manually-provision"></a>Optie 1: Installeren en handmatig inrichten

In deze eerste optie, moet u een apparaat-verbindingsreeks die is gegenereerd door de IoT Hub voor het inrichten van het apparaat opgeven. 

Volg de stappen in [een nieuwe Azure IoT Edge-apparaat registreren](how-to-register-device-portal.md) op uw apparaat registreren en de verbindingsreeks op te halen. 

In dit voorbeeld ziet u een handmatige installatie met Windows-containers:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -DeviceConnectionString '<connection-string>'
```

Wanneer u installeert en een apparaat handmatig inrichten, kunt u aanvullende parameters te wijzigen van de installatie, waaronder:
* Direct verkeer om te gaan via een proxyserver
* Het installatieprogramma verwijzen naar een offline-adreslijst
* Een containerinstallatiekopie specifieke agent declareren en geef referenties op als het zich in een persoonlijk register
* De Moby CLI-installatie overslaan

Voor meer informatie over deze installatieopties, doorgaan met het lezen van dit artikel of Ga voor meer informatie over [alle installatieparameters](#all-installation-parameters).

### <a name="option-2-install-and-automatically-provision"></a>Optie 2: Installeren en automatisch inrichten

In deze tweede optie, moet u het apparaat met IoT Hub Device Provisioning Service inrichten. Geef de **bereik-ID** van Device Provisioning Service-exemplaar en de **registratie-ID** van uw apparaat.

Volg de stappen in [maken en inrichten van een gesimuleerd TPM-Edge-apparaat op Windows](how-to-auto-provision-simulated-device-windows.md) de Device Provisioning Service instellen en ophalen van de **bereik-ID**, TPM-apparaat simuleren en op te halen de  **Registratie-ID**, maakt u een afzonderlijke inschrijving. Nadat het apparaat is geregistreerd in uw IoT-Hub, gaat u verder met de installatie.  

   >[!TIP]
   >Houd het venster met de TPM-simulator openen tijdens het installeren en testen. 

Het volgende voorbeeld ziet u een automatische installatie met Windows-containers:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Dps -ContainerOs Windows -ScopeId <DPS scope ID> -RegistrationId <device registration ID>
```

Wanneer u installeert en een apparaat handmatig inrichten, kunt u aanvullende parameters te wijzigen van de installatie, waaronder:
* Direct verkeer om te gaan via een proxyserver
* Het installatieprogramma verwijzen naar een offline-adreslijst
* Een containerinstallatiekopie specifieke agent declareren en geef referenties op als het zich in een persoonlijk register
* De Moby CLI-installatie overslaan

Voor meer informatie over deze installatieopties, doorgaan met het lezen van dit artikel of Ga voor meer informatie over [alle installatieparameters](#all-installation-parameters).

## <a name="update-an-existing-installation"></a>Een bestaande installatie van update

Als u hebt al IoT Edge-runtime op een apparaat hebt geïnstalleerd en ingericht met een identiteit van de IoT Hub, kunt u het script voor een vereenvoudigde installatie gebruiken. De vlag `-ExistingConfig` verklaart dat er al een IoT Edge-configuratiebestand op het apparaat bestaat. Het configuratiebestand bevat informatie over de apparaatinstellingen voor identiteit, evenals certificaten en netwerk. U kunt deze installatieoptie gebruiken of het apparaat oorspronkelijk is ingericht handmatig of automatisch. 

Zie voor meer informatie, [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).

In dit voorbeeld ziet u een installatie die verwijst naar een bestaand configuratiebestand en maakt gebruik van Windows-containers: 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -ExistingConfig -ContainerOs Windows
```

Wanneer u IoT Edge vanaf een bestaand configuratiebestand installeren, kunt u aanvullende parameters gebruiken om te wijzigen van de installatie, met inbegrip van:
* Direct verkeer om te gaan via een proxyserver
* Het installatieprogramma verwijzen naar een offline-directory, of 
* De Moby CLI-installatie wordt overgeslagen. 

U kunt de containerinstallatiekopie van een IoT Edge-agent met scriptparameters, niet declareren, omdat deze informatie is al ingesteld in het configuratiebestand van de vorige installatie. Als u wijzigen van de installatiekopie van de agent-container wilt, doen in het bestand config.yaml. 

Voor meer informatie over deze installatieopties, doorgaan met het lezen van dit artikel of Ga voor meer informatie over [alle installatieparameters](#all-installation-parameters).

## <a name="offline-installation"></a>Offline-installatie

Tijdens de installatie van de vier bestanden worden gedownload: 
* IoT Edge security-daemon (iotedgd) zip 
* Moby engine zip
* Zip Moby CLI
* Visual C++ redistributable-pakket (VC runtime) msi

U kunt een of meer van deze bestanden vooraf op het apparaat downloaden en vervolgens wijst u het script voor installatie op de map waarin de bestanden. Het installatieprogramma controleert eerst de map en downloadt vervolgens alleen onderdelen die niet zijn gevonden. Als alle vier bestanden offline beschikbaar zijn, kunt u met geen verbinding met internet installeren. U kunt deze functie ook gebruiken voor de onderdrukking van de online versies van een of meer onderdelen.  

Zie voor de meest recente van installatiebestanden, samen met eerdere versies, [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases)

Als u wilt installeren met offline-onderdelen, gebruikt u de `-OfflineInstallationPath` parameter en geeft u het absolute pad naar de map. Bijvoorbeeld:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -DeviceConnectionString '<connection-string>' -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

## <a name="all-installation-parameters"></a>Alle parameters van de installatie

In de vorige secties geïntroduceerd voor algemene scenario's met voorbeelden van hoe u kunt parameters gebruiken om te wijzigen van het script voor installatie. Deze sectie bevat een tabel met de geldige parameters die u gebruiken kunt voor het installeren van IoT Edge. Voor meer informatie, voert u `get-help Install-SecurityDaemon -full` in een PowerShell-venster. 

IoT Edge kunt met een bestaande configuratie, de installatieopdracht installeren via deze algemene parameters: 

| Parameter | Geaccepteerde waarden | Opmerkingen |
| --------- | --------------- | -------- |
| **Handmatig** | Geen | **Overschakelen van de parameter**. Elke installatie moet worden gedeclareerd als het handmatig, DP's, of existingconfig.<br><br>Verklaart dat u de verbindingsreeks van een apparaat voor het inrichten van het apparaat handmatig wordt opgeven |
| **Dps** | Geen | **Overschakelen van de parameter**. Elke installatie moet worden gedeclareerd als het handmatig, DP's, of existingconfig.<br><br>Verklaart dat u een Device Provisioning Service (DPS) bereik-ID en de registratie-ID van uw apparaat om in te richten via DPS wordt verstrekt.  |
| **ExistingConfig** | Geen | **Overschakelen van de parameter**. Elke installatie moet worden gedeclareerd als het handmatig, DP's, of existingconfig.<br><br>Verklaart dat config.yaml al een bestand op het apparaat met de Inrichtingsgegevens bestaat. |
| **DeviceConnectionString** | Een verbindingsreeks vanuit een IoT Edge-apparaat is geregistreerd in een IoT-Hub, tussen enkele aanhalingstekens | **Vereiste** voor handmatige installatie. Als u een verbindingsreeks in de scriptparameters niet opgeeft, wordt u gevraagd om een tijdens de installatie. |
| **ScopeId** | Een bereik-ID van een exemplaar van Device Provisioning Service die is gekoppeld aan uw IoT-Hub. | **Vereiste** voor de DPS-installatie. Als u een bereik-ID in de scriptparameters niet opgeeft, wordt u gevraagd om een tijdens de installatie. |
| **RegistrationId** | Een registratie-ID die is gegenereerd door uw apparaat | **Vereiste** voor de DPS-installatie. Als u een registratie-ID in de scriptparameters niet opgeeft, wordt u gevraagd om een tijdens de installatie. |
| **ContainerOs** | **Windows** of **Linux** | Als er geen container die besturingssysteem is opgegeven, is Linux de standaardwaarde. Voor Windows-containers, wordt een container-engine worden opgenomen in de installatie. Voor Linux-containers moet u een container-engine installeren voordat u begint de installatie. Linux-containers die wordt uitgevoerd op Windows is een nuttig ontwikkelingsscenario, maar niet ondersteund in de productieomgeving. |
| **Proxy** | URL van proxy | Deze parameter opgeeft als het apparaat via een proxyserver moet te bereiken van het internet. Zie voor meer informatie, [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Hash-tabel van de parameters en waarden | Tijdens de installatie van de verschillende webaanvragen worden gedaan. Dit veld gebruiken om in te stellen van parameters voor deze webaanvragen. Deze parameter is handig om de referenties voor de proxy-servers configureren. Zie voor meer informatie, [een IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Adreslijstpad | Als deze parameter opgenomen is, wordt het installatieprogramma van de map voor de iotedged zip, Moby engine zip, zip Moby CLI en VC Runtime MSI-bestanden die zijn vereist voor de installatie gecontroleerd. Als alle vier bestanden zich in de directory, kunt u IoT Edge tijdens het installeren offline. U kunt ook deze parameter gebruiken voor de onderdrukking van de online versie van een specifiek onderdeel. |
| **AgentImage** | URI van de IoT Edge-agent-installatiekopie | Een nieuwe installatie van de IoT Edge gebruikt standaard de meest recente rolling-tag voor de installatiekopie van het IoT Edge-agent. Gebruik deze parameter om in te stellen van een specifieke tag voor de versie van de installatiekopie, of om uw eigen installatiekopie agent. Zie voor meer informatie, [inzicht in IoT Edge tags](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Gebruikersnaam** | Container registry gebruikersnaam | Gebruik deze parameter alleen als u de parameter - AgentImage ingesteld op een container in een persoonlijk register. Een gebruikersnaam opgeven met toegang tot het register. |
| **Wachtwoord** | Beveiligd-wachtwoordverificatie-tekenreeks | Gebruik deze parameter alleen als u de parameter - AgentImage ingesteld op een container in een persoonlijk register. Geef het wachtwoord voor toegang tot het register. | 
| **SkipMobyCli** | Geen | Alleen van toepassing als - ContainerOS is ingesteld op Windows. Installeer de CLI Moby (docker.exe) niet aan $MobyInstallDirectory. |

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

U kunt de status van de IoT Edge-service door te controleren: 

```powershell
Get-Service iotedge
```

Controleer de servicelogboeken van de laatste 5 minuten met behulp van:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

En lijst met modules met:

```powershell
iotedge list
```

## <a name="uninstall-iot-edge"></a>Verwijderen van IoT Edge

Als u de installatie van de IoT Edge van uw Windows-apparaat verwijderen wilt, gebruikt u de volgende opdracht uit vanuit een PowerShell-venster met beheerdersrechten. Met deze opdracht verwijdert u de IoT Edge-runtime, samen met uw bestaande configuratie en de gegevens van de engine Moby. 

```PowerShell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-SecurityDaemon -DeleteConfig -DeleteMobyDataRoot
```

Als u van plan bent om IoT Edge op uw apparaat opnieuw te installeren, laat u uit de `-DeleteConfig` en `-DeleteMobyDataRoot` parameters zodat u de daemon beveiliging met de bestaande configuratie-informatie later opnieuw kunt installeren. 

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat dat is ingericht met de runtime geïnstalleerd hebt, kunt u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md).

Als u hebt met het IoT Edge niet goed geïnstalleerd problemen, bekijk de [probleemoplossing](troubleshoot.md) pagina.

Als u wilt een bestaande installatie bijwerken naar de nieuwste versie van IoT Edge, Zie [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).
