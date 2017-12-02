---
title: Bekende problemen en de gids voor probleemoplossing | Microsoft Docs
description: Lijst met bekende problemen en een handleiding om op te lossen
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 54038785f513e56b07f5f3fafa3dbd6d4b6e7400
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning-Workbench - bekende problemen en oplossen 
In dit artikel helpt u bij het zoeken en corrigeer de fouten of fouten dat is aangetroffen als onderdeel van het gebruik van de toepassing Azure Machine Learning-Workbench. 

## <a name="find-the-workbench-build-number"></a>Het buildnummer Workbench vinden
Om te communiceren met het ondersteuningsteam, is het belangrijk dat u het build-nummer van de Workbench-app. In Windows, kunt u het build-nummer uitzoeken door te klikken op de **Help** menu en kies **over Azure ML-Workbench**. Op Mac OS, kunt u klikken op de **Azure ML-Workbench** menu en kies **over Azure ML-Workbench**.

## <a name="machine-learning-msdn-forum"></a>Machine Learning MSDN-Forum
We hebben een MSDN-Forum dat u vragen kunt plaatsen. Het productteam bewaakt het forum actief. Het forum-URL is [https://aka.ms/azureml-forum](https://aka.ms/azureml-forum). 

## <a name="gather-diagnostics-information"></a>Diagnostische informatie verzamelen
Soms kan het handig zijn als u diagnostische gegevens opgeven kunt wanneer u hulp vragen. Hier volgt de woonplaats van de logboekbestanden:

### <a name="installer"></a>Installatieprogramma
Als u probleem tijdens de installatie, worden de logboekbestanden van het installatieprogramma hier:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
U kunt een zip-up van de inhoud van deze mappen en naar ons verzenden voor diagnostische gegevens.

### <a name="app-update"></a>App-Update 
#### <a name="no-update-notification-on-windows-desktop"></a>Er is geen melding van updates op Windows-bureaublad 
Dit probleem wordt in een toekomstige update worden verholpen. De tijdelijke oplossing is in de tussentijd om te voorkomen dat de app vanuit de snelkoppeling vastgemaakt aan de taakbalk opnieuw te starten. In plaats daarvan de app te starten via het menu Start of Start zoekbalk of de snelkoppeling op het bureaublad (indien aanwezig). 

#### <a name="no-update-notification-on-an-ubuntu-data-sciece-virtual-machine-dsvm"></a>Er is geen melding van updates op een virtuele Ubuntu gegevens Sciece virtuele Machine (DSVM)
Voer de volgende stappen uit om te downloaden van de meest recente toepassing:   
   - Verwijder de map \Users\AppData\Local\amlworkbench
   - script verwijderen`c:\dsvm\tools\setup\InstallAMLFromLocal.ps1`
   - verwijderen van snelkoppeling op het bureaublad waarmee het bovenstaande script wordt gestart
   - installeren met foutloos [https://aka.ms/azureml-wb-msi](https://aka.ms/azureml-wb-msi)

### <a name="workbench-desktop-app"></a>Workbench bureaublad-app
Als u problemen ondervindt bij het aanmelden of als het bureaublad Workbench vastloopt, vindt u hier logboekbestanden:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
U kunt een zip-up van de inhoud van deze mappen en naar ons verzenden voor diagnostische gegevens.

### <a name="experiment-execution"></a>Experiment uitvoeren
Als een bepaald script is mislukt tijdens het indienen van de bureaublad-app, probeer opnieuw te verzenden deze via het gebruik van de CLI `az ml experiment submit` opdracht. Hiermee geeft u volledig foutbericht in JSON-indeling en belangrijkste is dat deze bevat een **bewerkings-ID** waarde. Stuur ons de JSON-bestand op waaronder de **bewerkings-ID** en je kunt analyseren. 

Als een bepaald script erin slaagt verzending in uitvoering is mislukt, moet het afdrukken van de **uitvoeren ID** om te identificeren die bepaalde uitvoering. U kunt het pakket van de relevante logboekbestanden met de volgende opdracht:

```azurecli
# Create a ZIP file that contains all the diagnostics information
$ az ml experiment diagnostics -r <run_id> -t <target_name>
```

De `az ml experiment diagnostics` opdracht genereert een `diagnostics.zip` bestand in de hoofdmap van het project. Het ZIP-pakket bevat de map hele project in de status op het moment dat deze is uitgevoerd, plus informatie logboekregistratie. Zorg ervoor dat u niet opnemen wilt voor het verzenden van het bestand diagnostics vertrouwelijke gegevens te verwijderen.

## <a name="send-us-a-frown-or-a-smile"></a>Stuur ons een frons (of een glimlach)

Als u in Azure ML-Workbench werkt, kunt u ook verzenden ons een frons (of een glimlach) door te klikken op het pictogram gezichtje face onderin de linkerhoek van de toepassingsshell. U kunt eventueel bevatten uw e-mailadres (zodat we kunnen je terug), en/of een schermopname van de huidige status. 

## <a name="known-service-limits"></a>Bekende Servicelimieten
- Maximaal toegestane grootte van de map project: 25 MB.
    >[!NOTE]
    >Deze limiet niet van toepassing op `.git`, `docs` en `outputs` mappen. Deze mapnamen zijn hoofdlettergevoelig. Als u met grote bestanden werkt, raadpleegt u [behouden blijven van wijzigingen en behandelen van grote bestanden](how-to-read-write-files.md).

- Maximaal toegestane uitvoeringstijd experiment: zeven dagen

- Maximale grootte van getraceerde bestand in `outputs` map na een uitvoering: 512 MB
  - Dit betekent dat als het script een bestand groter is dan 512 MB in de map uitvoer wordt, deze is niet verzameld er. Als u met grote bestanden werkt, raadpleegt u [behouden blijven van wijzigingen en behandelen van grote bestanden](how-to-read-write-files.md).

- SSH-sleutels worden niet ondersteund bij het verbinden met een externe computer of een Spark-cluster via SSH. Modus van de gebruikersnaam en wachtwoord alleen wordt momenteel ondersteund.

- Wanneer u HDInsight-cluster als doel berekenen, moet uw organisatie gebruiken Azure blobs als primaire opslag. Met behulp van Azure Data Lake Storage wordt niet ondersteund.

- Clustering transformaties tekst worden niet ondersteund voor Mac.

- Bibliotheek RevoScalePy wordt alleen ondersteund op Windows- en Linux (in Docker-containers). Dit wordt niet ondersteund op Mac OS.

## <a name="cant-update-workbench"></a>Kan de Workbench niet bijwerken.
Wanneer een nieuwe update beschikbaar is, wordt de startpagina van de app Workbench een bericht weergegeven waarin u geïnformeerd over de nieuwe update. Hier ziet u een update-badge die zijn opgenomen in de linkerbenedenhoek van de app op het belpictogram. Klik op de badge en volg de installatiewizard om de update te installeren. 

![installatiekopie bijwerken](./media/known-issues-and-troubleshooting-guide/update.png)

Als u de melding niet ziet, probeer het opnieuw opstarten van de app. Als u de melding van updates na opnieuw opstarten niet ziet, is het mogelijk dat er enkele oorzaken.

### <a name="you-are-launching-workbench-from-a-pinned-shortcut-on-the-task-bar"></a>U bent Workbench starten vanuit een vastgemaakt snelkoppeling op de taakbalk
U kunt de update al hebt geïnstalleerd. Maar uw vastgemaakte snelkoppeling naar de oude bits op de schijf nog steeds wijst. U kunt dit controleren door te bladeren naar de `%localappdata%/AmlWorkbench` map en om te zien als u de meest recente versie geïnstalleerd er hebt en onderzoeken van de eigenschap van de vastgemaakte snelkoppeling om te zien waar het is aan te wijzen. Als geverifieerd, de oude snelkoppeling hoeft te verwijderen, Workbench starten vanuit het menu Start en eventueel een nieuwe vastgemaakt snelkoppeling maken op de taakbalk.

### <a name="you-installed-workbench-using-the-install-azure-ml-workbench-link-on-a-windows-dsvm"></a>U hebt geïnstalleerd via de koppeling 'Azure ML-Workbench installeren' aan een Windows-DSVM Workbench
Er is helaas geen eenvoudig correctie dit project. U hebt de volgende stappen uitvoeren om te verwijderen van de geïnstalleerde bits en het meest recente installatieprogramma om nieuwe-Installeer de Workbench te downloaden: 
   - de map verwijderen`C:\Users\<Username>\AppData\Local\amlworkbench`
   - script verwijderen`C:\dsvm\tools\setup\InstallAMLFromLocal.ps1`
   - verwijderen van snelkoppeling op het bureaublad waarmee het bovenstaande script wordt gestart
   - het installatieprogramma https://aka.ms/azureml-wb-msi downloaden en te installeren.

## <a name="get-stuck-at-checking-experimentation-account-screen-after-logging-in"></a>Hangen 'Experimenteren account controleren' scherm na het aanmelden
Na het aanmelden, kan de Workbench-app op een leeg scherm ophalen vastgelopen met een bericht weergegeven met 'Controleren experimenteren account' met een draaiende wheel. U lost dit probleem, moet u de volgende stappen uitvoeren:
1. Afsluiten van de app.
2. Het volgende bestand verwijderen:
  ```
  # on Windows
  %appdata%\AmlWorkbench\AmlWb.settings

  # on macOS
  ~/Library/Application Support/AmlWorkbench/AmlWb.settings
  ```
3. De app opnieuw starten.

## <a name="cant-delete-experimentation-account"></a>Experimenteren Account verwijderen niet
U kunt de CLI gebruiken om een Account experimenteren te verwijderen, maar u moet de onderliggende werkruimten en de onderliggende projecten in deze werkruimten onderliggende eerst verwijderen. Anders wordt er een fout opgetreden.

```azure-cli
# delete a project
$ az ml project delete -g <resource group name> -a <experimentation account name> -w <worksapce name> -n <project name>

# delete a workspace 
$ az ml workspace delete -g <resource group name> -a <experimentation account name> -n <worksapce name>

# delete an experimentation account
$ az ml account experimentation delete -g <resource group name> -n <experimentation account name>
```

U kunt ook de projecten en werkruimten van in de Workbench-app verwijderen.

## <a name="cant-open-file-if-project-is-in-onedrive"></a>Kan bestand niet openen als project in OneDrive
Als u Windows 10 vallen auteurs Update hebt en uw project is gemaakt in een lokale map die is toegewezen aan OneDrive, vindt u mogelijk dat u een bestand niet in de Workbench openen. Dit is vanwege een fout die door de vallen auteurs Update zorgt ervoor dat de node.js-code in een map OneDrive mislukken. De oplossingen snel worden opgelost door Windows update, maar tot die tijd, maak geen projecten in een map OneDrive.

## <a name="file-name-too-long-on-windows"></a>De bestandsnaam te lang in Windows
Als u de Workbench van Windows gebruikt, is het mogelijk uitgevoerd in de standaard maximaal 260 tekens bestand naam lengtelimiet, die surface kan als een fout 'systeem kan het opgegeven pad niet vinden'. U kunt een registersleutelinstelling waarmee veel langer pad bestandsnaam wijzigen. Bekijk [in dit artikel](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365247%28v=vs.85%29.aspx?#maxpath) voor meer informatie over het instellen van de _MAX_PATH_ registersleutel.

## <a name="docker-error-read-connection-refused"></a>Docker-fout ' lezen: verbinding geweigerd '
Wanneer u deze uitvoert op basis van een lokale Docker-container, soms mogelijk ziet u de volgende fout: 
```
Get https://registry-1.docker.io/v2/: 
dial tcp: 
lookup registry-1.docker.io on [::1]:53: read udp [::1]:49385->[::1]:53: 
read: connection refused
```

U kunt dit oplossen door het wijzigen van de Docker-DNS-Server van `automatic` op een vaste waarde `8.8.8.8`.

## <a name="remove-vm-execution-error-no-tty-present"></a>Fout bij uitvoeren van virtuele machine 'geen tty aanwezig' verwijderen
Wanneer op basis van een Docker-container op een externe Linux-machine wordt uitgevoerd, kunnen er het volgende foutbericht weergegeven:
```
sudo: no tty present and no askpass program specified.
``` 
Dit kan gebeuren als u Azure portal gebruiken voor het wijzigen van het hoofdwachtwoord van een virtuele Ubuntu Linux-machine. 

Azure Machine Learning Workbench vereist wachtwoordloze sudoers toegang worden uitgevoerd op externe hosts. De eenvoudigste manier om u te doen is met _visudo_ bewerken van het volgende bestand (u kunt het bestand maken als deze niet bestaat):

```
$ sudo visudo -f /etc/sudoers
```

>[!IMPORTANT]
>Het is belangrijk om te bewerken van het bestand met de _visudo_ en niet een andere opdracht. _visudo_ syntaxis controleert automatisch alle sudo-configuratiebestanden en mislukt voor het produceren van een bestand syntactisch juist sudoers kunt vergrendeld sudo.

Voeg de volgende regel aan het einde van het bestand:

```
username ALL=(ALL) NOPASSWD:ALL
```

Waar _gebruikersnaam_ is de naam van Azure Machine Learning Workbench wordt gebruikt voor aanmelding bij de externe host.

De regel moet worden geplaatst na #includedir ' / etc/sudoers.d ', anders kunnen worden overschreven door een andere regel.

Als u een complexere sudo-configuratie hebt, kunt u documentatie te raadplegen sudo voor Ubuntu beschikbaar hier: https://help.ubuntu.com/community/Sudoers

Deze fout kan ook gebeuren als u niet een VM op basis van Ubuntu Linux in Azure als een doel kan worden uitgevoerd. Alleen ondersteund op basis van Ubuntu Linux VM voor uitvoering op afstand. 

## <a name="vm-disk-is-full"></a>VM-schijf is vol
Standaard als u een nieuwe Linux VM in Azure maken, krijgt u een schijf van 30 GB voor het besturingssysteem. Docker-engine standaard gebruikt dezelfde schijf voor binnenhalen van afbeeldingen en actieve containers. Dit kan de besturingssysteemschijf vol en u een 'VM schijf is vol' fout ziet wanneer dit gebeurt.

Een snelle oplossing is het verwijderen van alle Docker-installatiekopieën die u niet meer gebruikt. De volgende Docker-opdracht wordt geregeld. (Natuurlijk moet SSH in de virtuele machine om de Docker-opdracht uitvoeren vanaf een bash-shell.)

```
$ docker system prune -a
```

U kunt ook een gegevensschijf toevoegen en configureren van Docker-engine voor het gebruik van de gegevensschijf voor het opslaan van afbeeldingen. Hier volgt [het toevoegen van een gegevensschijf](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk). U kunt vervolgens [wijzigen waar de installatiekopieën worden opgeslagen in Docker](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

Of u de schijf met het besturingssysteem kunt uitbreiden en u hoeft te touch configuratie van de Docker-engine. Hier volgt [hoe u de schijf met het besturingssysteem kunt uitbreiden](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk).

## <a name="sharing-c-drive-on-windows"></a>Delen van station C op Windows
Als u in een lokale Docker-container in Windows worden uitgevoerd, `sharedVolumes` naar `true` in de `docker.compute` bestand onder `aml_config` uitvoering prestaties kan verbeteren. Dit vereist echter delen van C-schijf in de _Docker voor Windows-hulpprogramma_. Als u niet C-station kunt delen bent, probeert u de volgende tips:

* Controleer het delen op station C Verkenner gebruiken
* Instellingen voor netwerkadapters openen en verwijderen/opnieuw installeren 'Bestands-en printerdeling voor Microsoft-netwerken' voor vEthernet
* Docker-instellingen openen en te delen van C-schijf van docker-instellingen
* Het Windows-wachtwoord wijzigt het delen. Open Windows Verkenner, opnieuw delen van de C-schijf en voer het nieuwe wachtwoord.
* U kunt ook firewallprobleem tegenkomen bij het delen van station C met Docker. Dit [post Stack Overflow](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) kan handig zijn.
* Bij het delen van station C met domeinreferenties, kan het delen vastlopen op netwerken waar de domeincontroller is niet bereikbaar (voor bijvoorbeeld thuisnetwerk, openbare Wi-Fi enz.). Zie voor meer informatie [deze post](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

U kunt ook het probleem met het delen, op een kleine prestaties kosten, door in te stellen vermijden `sharedVolumne` naar `false` in de `docker.compute` bestand.

## <a name="some-useful-docker-commands"></a>Een aantal handige Docker-opdrachten

Hier volgen enkele nuttige Docker-opdrachten:

```sh
# display all running containers
$ docker ps

# dislplay all containers (running or stopped)
$ docke ps -a

# display all images
$ docker images

# show Docker logs of a container
$ docker logs <container_id>

# create a new container and launch into a bash shell
$ docker run <image_id> /bin/bash

# launch into a bash shell on a running container
$ docker exec -it <container_id> /bin/bash

# stop an running container
$ docker stop <container_id>

# delete a container
$ docker rm <container_id>

# delete an image
$ docker rmi <image_id>

# delete all unussed Docker images 
$ docker system prune -a

```
