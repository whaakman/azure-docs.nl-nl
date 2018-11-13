---
title: Bijwerken van apparaten naar de nieuwste versie van Azure IoT Edge | Microsoft Docs
description: Het bijwerken van IoT Edge-apparaten voor het uitvoeren van de meest recente versies van de security-daemon en de IoT Edge-runtime
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 1522d9c9bc4fda178d8571fb57cb9c94ed1044ae
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567788"
---
# <a name="update-the-iot-edge-runtime"></a>Bijwerken van de IoT Edge-runtime

Zodra de IoT Edge-service worden nieuwe versies uitgebracht, moet u uw IoT Edge-apparaten als u wilt dat de nieuwste functies en verbeteringen in de beveiliging bij te werken. In dit artikel bevat informatie over het bijwerken van uw IoT Edge-apparaten wanneer een nieuwe versie beschikbaar is. 

Twee onderdelen van een IoT Edge-apparaat moeten worden bijgewerkt als u wilt verplaatsen naar een nieuwere versie. De eerste is de daemon voor beveiliging die wordt uitgevoerd op het apparaat en de runtime wordt gestart wanneer het apparaat wordt gestart. De daemon voor beveiliging op dit moment kan alleen van het apparaat zelf worden bijgewerkt. Het tweede onderdeel is de runtime, die bestaan uit de Edge hub en de Edge agent-modules. Afhankelijk van hoe u uw implementatie structureren, kan de runtime van het apparaat of extern worden bijgewerkt. 

De nieuwste versie van Azure IoT Edge, Zie [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

## <a name="update-the-security-daemon"></a>Update de daemon voor beveiliging

De daemon van de beveiliging IoT Edge is een systeemeigen onderdeel dat moet worden bijgewerkt met behulp van de package manager op het IoT Edge-apparaat. 

Controleer de versie van de security-daemon op uw apparaat met behulp van de opdracht `iotedge version`. 

### <a name="linux-devices"></a>Linux-apparaten

Gebruik apt-get- of de juiste Pakketbeheer om bij te werken van de security-daemon op Linux-apparaten. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

### <a name="windows-devices"></a>Windows-apparaten

Gebruik het PowerShell-script verwijderen en vervolgens de daemon van de beveiliging opnieuw te installeren op Windows-apparaten. Het script voor installatie automatisch de nieuwste versie van de security-daemon worden opgehaald. U moet de verbindingsreeks voor uw apparaat opnieuw opgeven tijdens het installatieproces. 

Verwijder de daemon voor beveiliging in een PowerShell-sessie. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
UnInstall-SecurityDaemon
```

De daemon voor beveiliging, afhankelijk van of gebruikmaakt van Windows-containers of Linux-containers in uw IoT Edge-apparaat installeren. Vervang de woordgroep **\<Windows of Linux\>** met een van de container-besturingssystemen. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOS <Windows or Linux>
```

## <a name="update-the-runtime-containers"></a>Bijwerken van de runtimecontainers

De manier waarop de Edge agent en Edge hub containers te werken, is afhankelijk van of u rolling tags (zoals 1.0) of specifieke tags (zoals 1.0.2) in uw implementatie gebruiken. 

Controleer de versie van de IoT Edge-agent en Edge hub-modules die zich momenteel op uw apparaat met de opdrachten `iotedge logs edgeAgent` of `iotedge logs edgeHub`. 

  ![Versie van de container weergeven](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>Informatie over IoT Edge-tags

De Edge agent en Edge hub afbeeldingen worden gemarkeerd met de IoT Edge-versie die ze zijn gekoppeld. Er zijn twee verschillende manieren om labels te gebruiken met de runtime-installatiekopieën: 

* **Tags rolling** -alleen de eerste twee waarden van het versienummer gebruiken om op te halen van de meest recente installatiekopie die overeenkomt met de cijfers. Bijvoorbeeld wordt 1.0 bijgewerkt wanneer er sprake is van een nieuwe versie om te verwijzen naar de nieuwste versie van de 1.0.x. Als de container-runtime op uw IoT Edge-apparaat opnieuw ophaalt van de installatiekopie, worden de runtimemodules bijgewerkt naar de nieuwste versie. Deze methode wordt aangeraden voor ontwikkelingsdoeleinden. Implementaties van de Azure portal standaardwaarde voor rolling tags. 
* **Specifieke labels** -gebruik van alle drie de waarden van het versienummer expliciet in te stellen de versie van de installatiekopie. Bijvoorbeeld, 1.0.2 worden niet gewijzigd na de eerste release. Wanneer u bent klaar om te werken, kunt u een nieuwe versienummer in het manifest implementatie declareren. Deze methode wordt aangeraden voor productiedoeleinden.

### <a name="update-a-rolling-tag-image"></a>Een rolling tag installatiekopie bijwerken

Als u rolling tags in uw implementatie gebruiken (bijvoorbeeld mcr.microsoft.com/azureiotedge-hub:**1.0**) moet u de container-runtime afdwingen op uw apparaat voor het ophalen van de meest recente versie van de installatiekopie. 

De lokale versie van de installatiekopie van het verwijderen van uw IoT Edge-apparaat. 

```cmd/sh
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

U moet mogelijk gebruik van de force `-f` vlag te verwijderen van de installatiekopieën. 

De IoT Edge-service ophalen van de meest recente versies van de runtime-afbeeldingen en automatisch start deze op uw apparaat opnieuw. 

### <a name="update-a-specific-tag-image"></a>De installatiekopie van een specifieke tag bijwerken

Als u specifieke tags in uw implementatie gebruiken (bijvoorbeeld mcr.microsoft.com/azureiotedge-hub:**1.0.2**) en u hoeft alleen de code in uw manifest implementatie bijwerken en de wijzigingen toepassen op uw apparaat is. 

In de Azure-portal, de runtime-implementatie-installatiekopieën zijn gedefinieerd in de **geavanceerde instellingen voor Edge-Runtime configureren** sectie. 

[Instellingen voor geavanceerde edge-runtime configureren](./media/how-to-update-iot-edge/configure-runtime.png)

In een manifest van de implementatie JSON bijwerken van de module-afbeeldingen in de **systemModules** sectie. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.2",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.2",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de meest recente [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Blijf up-to-date met recente updates en de aankondiging in de [Internet of Things-blog](https://azure.microsoft.com/blog/topics/internet-of-things/) 