---
title: Azure IoT Edge-ontwikkelomgeving | Microsoft Docs
description: Meer informatie over de ondersteunde systemen en eigen ontwikkelprogramma's waarmee dat u IoT Edge-modules maken
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/17/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 46b007cfa7156c0c7718b7f4bd4f735a58c722fc
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53797996"
---
# <a name="prepare-your-development-and-test-environment-for-iot-edge"></a>Voorbereiden van uw ontwikkel- en testomgeving voor IoT Edge

Azure IoT Edge verplaatst uw bestaande bedrijfslogica naar apparaten die aan de rand. Voorbereiden van uw toepassingen en werkbelastingen uit te voeren als [IoT Edge-modules](iot-edge-modules.md), moet u ze als containers bouwen. Dit artikel bevat informatie over het configureren van uw ontwikkelomgeving, zodat u kunt een IoT Edge-oplossing maken. Zodra u uw ontwikkelomgeving instellen hebt, klikt u vervolgens leert u hoe u [ontwikkelen van uw eigen IoT Edge-modules](module-development.md).

Er zijn ten minste twee machines rekening houden met in een IoT Edge-oplossing. Een IoT Edge-apparaat (of de apparaten) is zelf, die het IoT Edge-module wordt uitgevoerd. De andere is de ontwikkelcomputer die u gebruiken om te bouwen, testen en implementeren van modules. In dit artikel richt zich voornamelijk op de ontwikkelcomputer. Voor testdoeleinden, kunnen de twee machines hetzelfde zijn. U kunt IoT Edge uitvoeren op uw ontwikkelcomputer en modules te implementeren.

## <a name="operating-system"></a>Besturingssysteem

Azure IoT Edge wordt uitgevoerd op een specifieke set [ondersteunde besturingssystemen](support.md). Voor het ontwikkelen van IoT Edge, kunt u de meeste besturingssystemen die een engine voor container kunnen worden uitgevoerd. De container-engine is vereist op de ontwikkelcomputer voor het bouwen van uw modules als containers en ze naar een containerregister pushen. 

Als uw ontwikkelcomputer Azure IoT Edge kan niet worden uitgevoerd, gaat u verder in dit artikel voor meer informatie over [testprogramma's](#testing-tools) die u helpen bij test en lokaal fouten opsporen. 

Het besturingssysteem van uw ontwikkelcomputer beschikt niet over zodat deze overeenkomt met het besturingssysteem van uw IoT Edge-apparaat. Het besturingssysteem van de container moet consistent zijn tussen de computer van de ontwikkeling en IoT Edge-apparaat. U kunt bijvoorbeeld ontwikkelen van modules op een Windows-machine en deze implementeren in een Linux-apparaat. De Windows-machine moet worden uitgevoerd van Linux-containers voor het bouwen van de modules voor het Linux-apparaat. 

## <a name="container-engine"></a>Container-engine

Het centrale concept van IoT Edge is dat u op afstand uw bedrijfs- en cloud-logica op apparaten implementeren kunt met het verpakken in containers. Als u wilt maken van containers, moet u een container-engine op uw ontwikkelcomputer. 

De enige ondersteunde container-engine voor IoT Edge-apparaten in productie is Moby. Een compatibel is met het Open-Container-initiatief, zoals Docker, container-engine is echter wel geschikt voor het maken van IoT Edge module-installatiekopieën. 

## <a name="development-tools"></a>Ontwikkelhulpprogramma’s

Zowel Visual Studio en Visual Studio Code hebt aanvullende extensies voor helpt bij het ontwikkelen van IoT Edge-oplossingen. Deze uitbreidingen bieden taalspecifieke sjablonen om u te helpen bij het maken en implementeren van nieuwe IoT Edge-scenario's. De Azure IoT Edge-uitbreidingen voor Visual Studio en Visual Studio Code kunt u code, bouwen, implementeren en fouten opsporen in uw IoT Edge-oplossingen. U kunt een complete IoT Edge-oplossing met meerdere modules maken en een manifest implementatiesjabloon de extensies automatisch bijgewerkt met de toevoeging van elke nieuwe module. U kunt ook uw IoT-apparaten in Visual Studio of Visual Studio Code met de extensies beheren. Modules implementeert op een apparaat, de status controleren en weergeven van berichten binnenkomen bij IoT Hub. Beide extensies gebruiken de [IoT EdgeHub ontwikkeltool](#iot-edgehub-dev-tool) lokaal uitvoeren en foutopsporing van modules op uw ontwikkelcomputer ook inschakelen. 

Als u liever voor het ontwikkelen met andere editors of vanuit de CLI, biedt het hulpprogramma voor het ontwikkelen van Azure IoT Edge opdrachten die u kunt ontwikkelen en vanaf de opdrachtregel testen. U kunt nieuwe IoT-Edge-scenario's maken, maken van installatiekopieën van de module, modules in een simulator uitvoeren en bewaken van berichten naar IoT Hub. 

### <a name="visual-studio-code-extension"></a>Visual Studio Code-extensie

De Azure IoT Edge-extensie voor Visual Studio Code biedt IoT Edge module-sjablonen die zijn gebouwd op het programmeren van verschillende talen, waaronder C, C#, Java, Node.js, en Python, evenals Azure functions in C#. 

Zie voor meer informatie en om te downloaden, [Azure IoT Edge voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Naast de IoT Edge-extensies merkt u misschien het handig zijn voor het installeren van extra extensies voor het ontwikkelen van. Bijvoorbeeld, kunt u [Docker-ondersteuning voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) voor het beheren van uw afbeeldingen, containers en registers. Bovendien hebben alle ondersteunde talen van het primaire uitbreidingen voor Visual Studio-Code waarmee u kunt tijdens het ontwikkelen van modules. 

#### <a name="prerequisites"></a>Vereisten

De module-sjablonen voor sommige talen en services zijn vereisten die nodig zijn voor het bouwen van de projectmappen op een ontwikkelcomputer met Visual Studio Code.

| Module-sjabloon | Vereiste |
| --------------- | ------------ |
| Azure Functions | [.NET core SDK 2.1](https://www.microsoft.com/net/download) |
| C | [Git](https://git-scm.com/) |
| C# | [.NET core SDK 2.1](https://www.microsoft.com/net/download) |
| Java | <ul><li>[Java SE Development Kit 10](https://aka.ms/azure-jdks) <li> [Stel de omgevingsvariabele JAVA_HOME](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) <li> [Maven](https://maven.apache.org/)</ul> |
| Node.js | <ul><li>[Node.js](https://nodejs.org/) <li> [Yeoman](https://www.npmjs.com/package/yo) <li> [Generator voor Azure IoT Edge Node.js-module](https://www.npmjs.com/package/generator-azure-iot-edge-module)</ul> |
| Python |<ul><li> [Python](https://www.python.org/downloads/) <li> [PIP](https://pip.pypa.io/en/stable/installing/#installation) <li> [Cookiecutteru](https://cookiecutter.readthedocs.io/en/latest/installation.html) <li> [Git](https://git-scm.com/) </ul> |

### <a name="visual-studio-2017-extension"></a>Visual Studio 2017-extensie

De Azure IoT Edge-tools voor Visual Studio bieden een IoT Edge module-sjabloon die is gebouwd op C#. 

Zie voor meer informatie en om te downloaden, [Azure IoT Edge-Tools voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools).

### <a name="iot-edge-dev-tool"></a>Hulpprogramma voor het ontwikkelen van IoT Edge

Het hulpprogramma voor het ontwikkelen van Azure IoT Edge vereenvoudigt de ontwikkeling van IoT Edge met mogelijkheden voor het opdrachtregelprogramma. Dit hulpprogramma biedt CLI-opdrachten voor het ontwikkelen, fouten opsporen en modules testen. Het hulpprogramma voor het ontwikkelen van IoT Edge werkt met uw ontwikkelsysteem, ongeacht of u hebt de afhankelijkheden handmatig geïnstalleerd op uw computer of met behulp van de IoT Edge-dev-container. 

Zie voor meer informatie en aan de slag [IoT Edge dev hulpprogramma wiki](https://github.com/Azure/iotedgedev/wiki).

## <a name="testing-tools"></a>Testprogramma's

Er bestaan verschillende testhulpprogramma's om u te helpen bij het simuleren van IoT Edge-apparaten of efficiënter fouten opsporen in modules. De volgende tabel ziet u een op hoog niveau vergelijking tussen de hulpprogramma's en vervolgens afzonderlijke secties elk hulpprogramma meer in het bijzonder beschrijven. 

Alleen de IoT Edge-runtime voor productie-implementaties wordt ondersteund, maar de volgende hulpprogramma's kunnen u om te simuleren of eenvoudig IoT Edge-apparaten voor ontwikkelings- en testdoeleinden maken. Deze hulpprogramma's niet sluiten elkaar wederzijds uit, maar kunnen samenwerken voor een complete ontwikkel-ervaring. 

| Hulpprogramma | Ook wel bekend als | Ondersteunde platforms | Ideaal voor |
| ---- | ------------- | ------------------- | --------- |
| Hulpprogramma voor het ontwikkelen van IoT EdgeHub  | iotedgehubdev | Windows, Linux, Mac OS | Een apparaat om op te sporen modules simuleren. |
| IoT Edge dev-container | Microsoft/iotedgedev | Windows, Linux, Mac OS | Ontwikkelen zonder de installatie van afhankelijkheden. |
| IoT Edge-runtime in een container | iotedgec | Windows, Linux, MacOS, ARM | Testen op een apparaat dat de runtime wordt mogelijk niet ondersteund. |
| IoT Edge-apparaatcontainer | toolboc/azure-iot-edge-apparaat-container | Windows, Linux, MacOS, ARM | Het testen van een scenario met veel IoT Edge-apparaten op schaal. |

### <a name="iot-edgehub-dev-tool"></a>Hulpprogramma voor het ontwikkelen van IoT EdgeHub

De Azure IoT EdgeHub dev-hulpprogramma biedt een lokale ontwikkeling en foutopsporing. Het hulpprogramma helpt begin IoT Edge-modules zonder de IoT Edge-runtime zodat u maken kunt, ontwikkelen, testen, uitvoeren en fouten opsporen in IoT Edge-modules en lokaal oplossingen. U hoeft niet te installatiekopieën pushen naar een containerregister en implementeer ze op een apparaat voor het testen.

Het hulpprogramma voor het ontwikkelen van IoT EdgeHub is ontworpen voor gebruik in combinatie met de extensies voor Visual Studio en Visual Studio Code, evenals met het hulpprogramma voor het ontwikkelen van IoT Edge. Het biedt ondersteuning voor inner loop-ontwikkeling en testen van de buitenste lus, dus kan worden geïntegreerd met de DevOps-hulpprogramma's te. 

Zie voor meer informatie en voor het installeren van [Azure IoT EdgeHub ontwikkeltool](https://pypi.org/project/iotedgehubdev/).

### <a name="iot-edge-dev-container"></a>IoT Edge dev-container

De Azure IoT Edge-dev-container is een Docker-container met de afhankelijkheden die u nodig hebt voor het ontwikkelen van IoT Edge. Deze container maakt het gemakkelijk om te beginnen met welke taal u wilt ontwikkelen, met inbegrip van C#, Python, Node.js en Java. U wilt installeren, is een container-engine, zoals Docker of Moby, voor het ophalen van de container naar uw ontwikkelmachine. 

Zie voor meer informatie, [Azure IoT Edge dev container](https://hub.docker.com/r/microsoft/iotedgedev/).

### <a name="iot-edge-runtime-in-a-container"></a>IoT Edge-runtime in een container

IoT Edge-runtime in een container biedt een volledige runtime die met de apparaatverbindingsreeks als een omgevingsvariabele. Deze container kunt u voor het testen van IoT Edge-modules en scenario's op een systeem dat mogelijk geen ondersteuning voor de runtime systeemeigen, zoals Mac OS. Alle modules die u implementeert buiten de runtime-container gestart. Als u wilt dat de runtime en alle geïmplementeerde modules bestaan in dezelfde container, kunt u de container IoT Edge-apparaat in plaats daarvan in.

Zie voor meer informatie, [Azure IoT Edge wordt uitgevoerd in een container](https://github.com/Azure/iotedgedev/tree/master/docker/runtime).

### <a name="iot-edge-device-container"></a>IoT Edge-apparaatcontainer

De container IoT Edge-apparaat is een complete IoT Edge-apparaat, worden klaar om te worden gestart op een machine met een container-engine. De apparaatcontainer bevat de IoT Edge-runtime en een container-engine zelf. Elk exemplaar van de container is een volledig functionele zelf provisioning IoT Edge-apparaat. De apparaatcontainer ondersteunt foutopsporing op afstand van modules, mits er een netwerkroute naar de module is. De apparaatcontainer is handig voor het snel maken van grote aantallen IoT Edge-apparaten voor het testen van scenario's op schaal of DevOps-pijplijnen. Het ondersteunt ook implementatie naar continue kubernetes via helm. 

Zie voor meer informatie, [Azure IoT Edge-apparaatcontainer](https://github.com/toolboc/azure-iot-edge-device-container).

## <a name="devops-tools"></a>DevOps-hulpprogramma’s

Wanneer u klaar om oplossingen voor uitgebreide productiescenario's op schaal te ontwikkelen bent, profiteer van moderne DevOps-principes, met inbegrip van automatisering, controle en gestroomlijnde software engineering-processen. IoT Edge is extensies voor de ondersteuning van DevOps-hulpprogramma's zoals Azure DevOps, Azure DevOps Projects en Jenkins. Als u wilt aanpassen aan een bestaande pijplijn of gebruik een andere DevOps-hulpprogramma, zoals CircleCI of TravisCI, kunt u dit doen met de CLI-functies opgenomen in het hulpprogramma voor het ontwikkelen van IoT Edge.

Zie voor meer informatie, instructies en voorbeelden, de volgende pagina's:
* [Continue integratie en continue implementatie voor Azure IoT Edge](how-to-ci-cd.md)
* [Een CI/CD-pijplijn maken voor IoT Edge met Azure DevOps Projects](how-to-devops-project.md)
* [Azure IoT Edge Jenkins-invoegtoepassing](https://plugins.jenkins.io/azure-iot-edge)
* [IoT Edge DevOps GitHub-opslagplaats](https://github.com/toolboc/IoTEdge-DevOps)
