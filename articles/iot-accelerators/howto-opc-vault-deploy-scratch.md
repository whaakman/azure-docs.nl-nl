---
title: Over het implementeren van Azure IoT OPC UA certificate management-module helemaal | Microsoft Docs
description: Klik hier voor meer informatie over het OPC-kluis implementeert helemaal.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: a3a9d21b70f16482f05d27aa0df8d8865459aeb4
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759427"
---
# <a name="deploy-opc-vault-from-scratch"></a>OPC-kluis implementeert helemaal

Azure IoT OPC UA Certificaatbeheer, ook weten als OPC-kluis is een microservice die configureren kunt, registreren, en de levenscyclus van het certificaat voor de OPC UA-server en client-toepassingen in de cloud beheren. Dit artikel laat u het OPC-kluis implementeert helemaal.

## <a name="configuration-and-environment-variables"></a>Configuratie en omgevingsvariabelen

De serviceconfiguratie wordt opgeslagen met behulp van ASP.NET Core-configuratie-adapters, in appsettings.ini. De INI-indeling kunt voor het opslaan van waarden in een leesbare indeling, met opmerkingen.
De toepassing biedt ook ondersteuning voor invoegen omgevingsvariabelen, zoals referenties en details van netwerken. (In dit gedeelte heet oorspronkelijk TODO-configuratie en omgevingsvariabelen).

Het configuratiebestand in de opslagplaats verwijst naar bepaalde omgevingsvariabelen die moeten ten minste één keer worden gemaakt. Afhankelijk van uw besturingssysteem en de IDE zijn er verschillende manieren voor het beheren van omgevingsvariabelen:

- Voor Windows-gebruikers moet het script env-variabelen-setup.cmd worden voorbereid en slechts één keer uitgevoerd. Tijdens de uitvoering van persistent is de instellingen voor terminal-sessies en opnieuw wordt opgestart.

- Voor Linux en OS x-omgevingen moet het env-variabelen-setup-script worden uitgevoerd telkens wanneer een nieuwe console wordt geopend. Afhankelijk van het besturingssysteem en de terminal zijn er manieren om vast te leggen van de waarden wereldwijd, voor meer informatie die deze pagina's kunnen helpen:

  - https://stackoverflow.com/questions/13046624/how-to-permanently-export-a-variable-in-linux
  
  - https://stackoverflow.com/questions/135688/setting-environment-variables-in-os-x
  
  - https://help.ubuntu.com/community/EnvironmentVariables
  

- Visual Studio: Omgevingsvariabelen kan ook worden ingesteld van Visual Studio, onder de eigenschappen van het Project, selecteer in het linkerdeelvenster 'Configuratie-eigenschappen' en 'Omgeving' om te gaan naar een sectie waarin u meerdere variabelen kunt toevoegen.

- IntelliJ Rider: Omgevingsvariabelen kunnen worden ingesteld in elke configuratie uitvoeren op soortgelijke wijze aan IntelliJ IDEA https://www.jetbrains.com/help/idea/run-debug-configuration-application.html

## <a name="run-and-debug-with-visual-studio"></a>Uitvoeren en fouten opsporen met Visual Studio

Visual Studio kunt u snel openen van de toepassing zonder gebruik te maken van een opdrachtprompt zonder iets buiten de IDE configureren.

Met behulp van Visual Studio 2017 stappen:

1. Open de oplossing met behulp van de `iot-opc-gds-service.sln` bestand.

1. Wanneer de oplossing wordt geladen, met de rechtermuisknop op de `WebService` project, selecteer en gaat u naar de `Debug` sectie.

1. In dezelfde sectie, definieert u de omgevingsvariabelen die is vereist.

1. Druk op **F5**, of de **uitvoeren** pictogram. Visual Studio moet de status van de service worden weergegeven in JSON-indeling browser openen.

## <a name="run-and-debug-with-intellij-rider"></a>Uitvoeren en fouten opsporen met IntelliJ bestuurder

1. Open de oplossing met behulp van de `iot-opc-gds-service.sln` bestand.

1. Wanneer de oplossing wordt geladen, gaat u naar `Run > Edit Configurations` en maak een nieuwe `.NET Project` configuratie.

1. Selecteer het WebService-project in de configuratie.

1. Sla de instellingen op en voer de configuratie die gemaakt van de IDE-werkbalk.

1. U ziet de berichten in het venster van de IntelliJ uitvoeren met details, zoals de URL waar de webservice wordt uitgevoerd plus de service bootstrap-service zich aanmeldt.

## <a name="build-and-run-from-the-command-line"></a>Bouwen en uitvoeren vanaf de opdrachtregel

De scriptmap bevat enkele scripts voor de veelgebruikte taken:

- `build`: Alle projecten compileren en uitvoeren van de tests.

- `compile`: Alle projecten worden gecompileerd.

- `run`: De projecten compileren en uitvoeren van de service, waarbij u wordt gevraagd voor verhoogde bevoegdheden in Windows om uit te voeren van de webservice.

De scripts controleren voor de variabelen voor de installatie van de omgeving. U kunt de omgevingsvariabelen globaal instellen in uw besturingssysteem of gebruik het script 'env-variabelen instellen' in de scriptmap.

### <a name="sandbox"></a>Sandbox

De scripts wordt ervan uitgegaan dat u uw ontwikkelingsomgeving hebt geconfigureerd met .NET Core- en Docker. U kunt voorkomen dat .NET Core, installeren en alleen Docker installeren en gebruikt de opdrachtregelparameter `--in-sandbox` (of de verkorte vorm `-s`), bijvoorbeeld:

- `build --in-sandbox`: De build-taak binnen een Docker-container wordt uitgevoerd (kort formulier `build -s`).

- `compile --in-sandbox`: De taak compilatie binnen een Docker-container wordt uitgevoerd (kort formulier `compile -s`).

- `run --in-sandbox`: Start de service binnen een Docker-container (kort formulier `run -s`).

De Docker-installatiekopieën die worden gebruikt voor de sandbox worden gehost op Docker Hub [hier](https://hub.docker.com/r/azureiotpcs/code-builder-dotnet).

## <a name="package-the-application-to-a-docker-image"></a>De toepassing in een Docker-installatiekopie Inpakken

De `scripts` map bevat een docker-submap met de bestanden die nodig zijn voor het verpakken van de service in een Docker-installatiekopie:

- `Dockerfile`: Docker-installatiekopieën specificaties.
- `build`: Bouw een Docker-container en opslaan van de installatiekopie in het lokale register.
- `run`: De Docker-container uitvoeren vanuit de installatiekopie is opgeslagen in het lokale register.
- `content`: Een map met bestanden die zijn gekopieerd naar de installatiekopie, met inbegrip van het invoerpunt-script.

## <a name="azure-iot-hub-setup"></a>Installatie van de Azure IoT Hub

Voor het gebruik van de microservices, instellen van uw Azure-IoT-Hub voor ontwikkelen en integreren van tests.

Het project bevat enkele Bash-scripts waarmee u kunt met deze instellingen:

- Nieuwe IoT Hub maken: `./scripts/iothub/create-hub.sh`

- Lijst met bestaande hubs: `./scripts/iothub/list-hubs.sh`

- IoT Hub details (bijvoorbeeld sleutels) weergeven: `./scripts/iothub/show-hub.sh`

En als er meerdere Azure-abonnementen:

- Abonnementenlijst weergeven: `./scripts/iothub/list-subscriptions.sh`

- Huidige abonnement wijzigen: `./scripts/iothub/select-subscription.sh`

## <a name="development-setup"></a>Ontwikkeling instellen

### <a name="net-setup"></a>.NET-installatie

De werkstroom van het project wordt beheerd [.NET Core](https://dotnet.github.io) 1.x, die u installeren in uw omgeving wilt, zodat u kunt alle scripts uitvoeren en ervoor te zorgen dat uw IDE werkt zoals verwacht.

We bieden ook een [Java-versie](https://github.com/Azure/iot-opc-gds-service-dotnet) van het project en andere onderdelen van Azure IoT-pc's.

### <a name="ide"></a>IDE

Hier volgen enkele van de IDE's die u gebruiken kunt om te werken op Azure IoT-pc's:

- [Visual Studio](https://www.visualstudio.com)
- [Visual Studio voor Mac](https://www.visualstudio.com/vs/visual-studio-mac)
- [IntelliJ Rider](https://www.jetbrains.com/rider)
- [Visual Studio Code](https://code.visualstudio.com)

### <a name="git-setup"></a>GIT-installatie

Het project omvat een Git-hook, voor het automatiseren van enkele controles voordat een codewijziging worden geaccepteerd. U kunt handmatig de tests uitgevoerd of dat de CI-platform voor het uitvoeren van de tests. We gebruiken de volgende Git-hook automatisch alle tests voordat wijzigingen in de code wordt verzonden naar GitHub uitvoeren en de ontwikkelingswerkstroom versnellen.

Als u verwijderen van de hook op elk gewenst moment wilt, verwijdert u gewoon het bestand is geïnstalleerd onder `.git/hooks`. Ook kunt u negeren de hook vooraf doorvoeren met behulp van de `--no-verify` optie.

#### <a name="pre-commit-hook-with-sandbox"></a>Vooraf toegewezen hook met sandbox

Als u de opgenomen hooks instelt, open een Windows/Linux/MacOS-console en uitvoeren:

```
cd PROJECT-FOLDER
cd scripts/git
setup --with-sandbox
```

Met deze configuratie worden bij het inchecken van bestanden, verifieert Git dat de toepassing wordt doorgegeven voor alle tests, de build en de tests in een Docker-container die is geconfigureerd met alle ontwikkelingsvereisten voor de worden uitgevoerd.

#### <a name="pre-commit-hook-without-sandbox"></a>Vooraf toegewezen haakje zonder sandbox

> [!NOTE] 
> Vereist dat de haakje zonder sandbox [.NET Core](https://dotnet.github.io) in het systeem pad.

Als u de opgenomen hooks instelt, open een Windows/Linux/MacOS-console en uitvoeren:

```
cd PROJECT-FOLDER
cd scripts/git
setup --no-sandbox
```
Met deze configuratie worden bij het inchecken van bestanden, verifieert Git dat de toepassing wordt doorgegeven voor alle tests, de build en de tests uitvoert in uw werkstation met behulp van de hulpprogramma's in uw besturingssysteem geïnstalleerd.

Hulp bij de stijl van de project-code:

- Wanneer redelijke mate, is regels lengte beperkt tot 80 tekens voor de, coderevisies en opdrachtregel editors max.

- Codeblokken inspringing met vier spaties. Het tabblad char moeten worden vermeden.

- Tekstbestanden Unix einde van regel-indeling (LF) gebruiken.

- Afhankelijkheidsinjectie wordt beheerd met [Autofac](https://autofac.org).

- Web service-API's velden zijn CamelCased, met uitzondering van metagegevens.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over het implementeren van OPC-kluis maken, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [OPC-Twin helemaal implementeren](howto-opc-twin-deploy-modules.md)