---
title: Bekende problemen en gids voor probleemoplossing | Microsoft Docs
description: Lijst met bekende problemen en een handleiding om op te lossen
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: d72f1b986671bfab1d79772ac50935918da7e681
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963267"
---
# <a name="azure-machine-learning-workbench---known-issues-and-troubleshooting-guide"></a>Azure Machine Learning Workbench - bekende problemen en problemen oplossen met 
Dit artikel helpt u bij het zoeken en corrigeer de fouten of fouten aangetroffen als onderdeel van het gebruik van de toepassing Azure Machine Learning Workbench. 

## <a name="find-the-workbench-build-number"></a>De Workbench build-nummer zoeken
Bij het communiceren met het ondersteuningsteam, is het belangrijk om het buildnummer van het Workbench-app. Op Windows, vindt u het build-nummer door te klikken op de **Help** menu en kies **over Azure ML Workbench**. Op Mac OS, u kunt klikken op de **Azure ML Workbench** menu en kies **over Azure ML Workbench**.

## <a name="machine-learning-msdn-forum"></a>Machine Learning-MSDN-Forum
We hebben een MSDN-Forum dat u vragen kunt plaatsen. Het productteam het forum wordt actief bewaakt. De URL is forum [ https://aka.ms/azureml-forum ](https://aka.ms/azureml-forum). 

## <a name="gather-diagnostics-information"></a>Verzamelen van diagnostische gegevens
Soms kan het handig zijn als u diagnostische gegevens opgeven kunt wanneer u hulp vragen. Dit is waar de logboekbestanden bevinden:

### <a name="installer-log"></a>Installer-logboekbestand
Als u probleem tijdens de installatie ondervindt, worden de logboekbestanden van het installatieprogramma hier:

```
# Windows:
%TEMP%\amlinstaller\logs\*

# macOS:
/tmp/amlinstaller/logs/*
```
U kunt een zip-up van de inhoud van deze mappen en naar ons verzenden voor diagnostische gegevens.

### <a name="workbench-desktop-app-log"></a>Workbench-desktop-app-logboek
Als u problemen ondervindt bij het aanmelden, of als het bureaublad van de Workbench vastloopt, kunt u de logboekbestanden hier vinden:
```
# Windows
%APPDATA%\AmlWorkbench

# macOS
~/Library/Application Support/AmlWorkbench
``` 
U kunt een zip-up van de inhoud van deze mappen en naar ons verzenden voor diagnostische gegevens.

### <a name="experiment-execution-log"></a>Het uitvoeringslogboek experiment
Als een bepaald script is mislukt tijdens het indienen van de desktop-app, probeer deze opnieuw indienen via de CLI met `az ml experiment submit` opdracht. Hiermee geeft u volledige foutbericht wordt weergegeven in JSON-indeling en het belangrijkste is dat deze bevat een **bewerkings-ID** waarde. Stuur ons de JSON, waaronder de **bewerkings-ID** en je kunt vaststellen. 

Als een bepaald script erin verzenden slaagt, maar in uitvoering is mislukt, moet het afdrukken van de **uitvoeren ID** om te bepalen dat bepaalde uitvoeren. U kunt u een pakket van de relevante logboekbestanden met de volgende opdracht:

```azurecli
# Create a ZIP file that contains all the diagnostics information
$ az ml experiment diagnostics -r <run_id> -t <target_name>
```

De `az ml experiment diagnostics` opdracht genereert een `diagnostics.zip` bestand in de hoofdmap van het project. Het ZIP-pakket bevat de hele projectmap in de status op het moment dat deze is uitgevoerd, plus logboekinformatie. Zorg ervoor dat gevoelige informatie die u niet opnemen wilt voor het verzenden van het bestand diagnostische gegevens verwijderen.

## <a name="send-us-a-frown-or-a-smile"></a>Stuur ons een frons (of een glimlach)

Wanneer u in Azure ML Workbench werkt, kunt u ook Stuur ons een frons (of een glimlach) door te klikken op het pictogram van de face gezichtje onderin de linkerhoek van de toepassing-shell. U kunt eventueel bevatten uw e-mailadres (zodat we kun je weer verder aan u), en/of een schermafbeelding van de huidige status. 

## <a name="known-service-limits"></a>Bekende Servicelimieten
- Maximaal toegestane grootte van de map project: 25 MB.
    >[!NOTE]
    >Deze limiet niet van toepassing op `.git`, `docs` en `outputs` mappen. Deze mapnamen zijn hoofdlettergevoelig. Als u met grote bestanden werkt, raadpleegt u [wijzigingen permanent worden gemaakt en betrekking hebben op grote bestanden](how-to-read-write-files.md).

- Maximaal toegestane uitvoeringstijd experiment: zeven dagen

- Maximale grootte van bijgehouden bestand in `outputs` map na een uitvoering: 512 MB
  - Dit betekent dat als het script wordt een bestand groter is dan 512 MB in de map voor uitvoer geproduceerd, deze is niet verzameld er. Als u met grote bestanden werkt, raadpleegt u [wijzigingen permanent worden gemaakt en betrekking hebben op grote bestanden](how-to-read-write-files.md).

- SSH-sleutels worden niet ondersteund bij het verbinden met een externe computer of een Spark-cluster via SSH. Modus van de gebruikersnaam en wachtwoord alleen wordt momenteel ondersteund.

- Bij het gebruik van HDInsight-cluster naar compute-doel, moet het gebruik van Azure blob als primaire opslag. Met behulp van Azure Data Lake Storage wordt niet ondersteund.

- Clustering teksttransformaties worden niet ondersteund op Mac.

- RevoScalePy-bibliotheek wordt alleen ondersteund in Windows en Linux (in Docker-containers). Het wordt niet ondersteund op Mac OS.

- Jupyter-Notebooks hebben een maximale grootte van 5 MB wanneer deze te openen vanaf de Workbench-app. U kunt grote notitieblokken openen vanuit de CLI met behulp van de opdracht 'az ml-notebook start' en schoon cel levert om de bestandsgrootte te reduceren.

## <a name="cant-update-workbench"></a>Workbench kan niet worden bijgewerkt
Wanneer een nieuwe update beschikbaar is, wordt de startpagina van de Workbench-app een bericht weergegeven waarin u over de nieuwe update. Hier ziet u een update-badge wordt weergegeven in de linkerbenedenhoek van de app op het belpictogram. Klik op de badge en volg de wizard installatieprogramma om de update te installeren. 

![installatiekopie bijwerken](../service/media/known-issues-and-troubleshooting-guide/update.png)

Als u de melding niet ziet, probeert u de app opnieuw te starten. Als u de melding van updates na opnieuw opstarten nog steeds niet ziet, is het mogelijk dat er enkele oorzaken.

### <a name="you-are-launching-workbench-from-a-pinned-shortcut-on-the-task-bar"></a>U bent Workbench starten vanaf een vastgemaakte snelkoppeling op de taakbalk
U kunt de update al hebt geïnstalleerd. Maar de vastgemaakte snelkoppeling nog steeds verwijst naar de oude bits op schijf. U kunt dit controleren door te bladeren naar de `%localappdata%/AmlWorkbench` map en om te zien als u de meest recente versie is geïnstalleerd en de eigenschap van de vastgemaakte snelkoppeling om te zien waar het verwijst naar. Als geverifieerd, hoeft de snelkoppeling naar de oude te verwijderen, start Workbench uit het menu Start en eventueel een nieuwe vastgemaakte snelkoppeling maken op de taakbalk.

### <a name="you-installed-workbench-using-the-install-azure-ml-workbench-link-on-a-windows-dsvm"></a>U hebt geïnstalleerd met behulp van de koppeling "Azure ML Workbench installeren" op een Windows-DSVM Workbench
Er is helaas geen snel worden opgelost op dit apparaat. U moet de volgende stappen uit om te verwijderen van de geïnstalleerde bits en download het nieuwste installatieprogramma als u wilt de Workbench vers-installatie uitvoeren: 
   - de map verwijderen `C:\Users\<Username>\AppData\Local\amlworkbench`
   - script verwijderen `C:\dsvm\tools\setup\InstallAMLFromLocal.ps1`
   - verwijderen van snelkoppeling op het bureaublad die het bovenstaande script wordt gestart
   - Download het installatieprogramma https://aka.ms/azureml-wb-msi en opnieuw installeren.

## <a name="stuck-at-checking-experimentation-account-screen-after-logging-in"></a>Vastgelopen bij 'Experimenten-account controleren' scherm na het aanmelden
Na het aanmelden, de Workbench-app mogelijk zitten op een leeg scherm met een bericht weergegeven met 'Controleren experimenten-account' met een draaiende wiel. U lost dit probleem, moet u de volgende stappen uitvoeren:
1. Afsluiten van de app.
2. Hiermee verwijdert u het volgende bestand:
  ```
  # on Windows
  %appdata%\AmlWorkbench\AmlWb.settings

  # on macOS
  ~/Library/Application Support/AmlWorkbench/AmlWb.settings
  ```
3. De app opnieuw starten.

## <a name="cant-delete-experimentation-account"></a>Kan de experimenten-Account niet verwijderen
U kunt de CLI gebruiken voor het verwijderen van een experimenten-Account, maar u moet de onderliggende werkruimten en de onderliggende projecten in deze werkruimten onderliggende eerst verwijderen. Anders is, ziet u de fout 'kan resource pas verwijderen ingesloten resources zijn verwijderd."

```azure-cli
# delete a project
$ az ml project delete -g <resource group name> -a <experimentation account name> -w <workspace name> -n <project name>

# delete a workspace 
$ az ml workspace delete -g <resource group name> -a <experimentation account name> -n <workspace name>

# delete an experimentation account
$ az ml account experimentation delete -g <resource group name> -n <experimentation account name>
```

U kunt ook de projecten en werkruimten uit in de Workbench-app verwijderen.

## <a name="cant-open-file-if-project-is-in-onedrive"></a>Kan bestand niet openen als project in OneDrive
Als u Windows 10 Fall Creators Update hebt en uw project in een lokale map die is toegewezen aan OneDrive wordt gemaakt, kunt u wellicht dat u een bestand niet in Workbench openen. Dit komt door een bug die door de Fall Creators Update die ervoor zorgt node.js-code dat wilt uitvoeren naar een OneDrive-map. De fout die snel worden opgelost door Windows update, maar tot die tijd kun, maak geen projecten in een OneDrive-map.

## <a name="file-name-too-long-on-windows"></a>Bestandsnaam te lang op Windows
Als u Workbench op Windows gebruikt, is het mogelijk uitgevoerd in de standaard maximaal 260 tekens bestand naam lengtelimiet, die kan het oppervlak als een fout 'systeem kan het opgegeven pad niet vinden'. U kunt de instelling van een registersleutel waarmee de padnaam van veel langer wijzigen. Beoordeling [in dit artikel](https://msdn.microsoft.com/library/windows/desktop/aa365247%28v=vs.85%29.aspx?#maxpath) voor meer informatie over het instellen de _MAX_PATH_ registersleutel.

## <a name="interrupt-cli-execution-output"></a>CLI-uitvoering uitvoer onderbreken
Als u een vliegende start een experimenten uitgevoerd met behulp van `az ml experiment submit` of `az ml notebook start` en u wilt onderbreken van de uitvoer: 
- Gebruik de toetscombinatie Ctrl-Break van het toetsenbord op Windows
- Gebruik Ctrl-c op Mac OS

Houd er rekening mee dat dit alleen de uitvoerstroom in het CLI-venster wordt onderbroken. Het stopt daadwerkelijk niet een taak die wordt uitgevoerd. Als u een actieve taak annuleren wilt, gebruikt u `az ml experiment cancel -r <run_id> -t <target name>` opdracht.

Op Windows-computers met een toetsenbord die geen onderbreking sleutel hebben, zijn mogelijk alternatieven Fn-B, Ctrl-Fn-B of Fn + ESC te drukken. De documentatie van de hardwareleverancier van uw voor een specifieke combinatie.

## <a name="docker-error-read-connection-refused"></a>Docker-fout ' lezen: verbinding geweigerd "
Bij het uitvoeren van op basis van een lokale Docker-container, soms ziet u mogelijk de volgende fout: 
```
Get https://registry-1.docker.io/v2/: 
dial tcp: 
lookup registry-1.docker.io on [::1]:53: read udp [::1]:49385->[::1]:53: 
read: connection refused
```

U kunt dit oplossen door het wijzigen van de Docker-DNS-Server uit `automatic` op een vaste waarde van `8.8.8.8`.

## <a name="remove-vm-execution-error-no-tty-present"></a>Fout bij uitvoering VM "geen tty aanwezig" verwijderen
Bij het uitvoeren van op basis van een Docker-container op een externe Linux-computer, kunnen er de volgende strekking weergegeven:
```
sudo: no tty present and no askpass program specified.
``` 
Dit kan gebeuren als u Azure portal gebruiken voor het wijzigen van het hoofdwachtwoord van een Ubuntu Linux-VM. 

Azure Machine Learning Workbench vereist geen wachtwoord meer sudoers toegang om uit te voeren op de externe hosts. De eenvoudigste manier om dat te doen is met _visudo_ bewerken van het volgende bestand (u kunt het bestand maken als deze niet bestaat):

```
$ sudo visudo -f /etc/sudoers
```

>[!IMPORTANT]
>Het is belangrijk om te bewerken van het bestand met de _visudo_ en niet een andere opdracht. _visudo_ syntaxis wordt automatisch gecontroleerd voor alle sudo-configuratiebestanden en fout voor het produceren van een syntactisch juiste sudo-bestand kan worden vergrendeld sudo.

Voeg de volgende regel aan het einde van het bestand:

```
username ALL=(ALL) NOPASSWD:ALL
```

Waar _gebruikersnaam_ is de naam van Azure Machine Learning Workbench wordt gebruikt om aan te melden bij uw externe host.

De regel moet worden geplaatst na #includedir "/ etc/sudoers.d", anders kunnen worden overschreven door een andere regel.

Als u een meer complexe sudo-configuratie hebt, kunt u de documentatie van sudo voor Ubuntu beschikbaar hier: https://help.ubuntu.com/community/Sudoers

De bovenstaande fout kan ook gebeuren als u niet een op basis van een Ubuntu Linux-VM in Azure als een doel voor uitvoering. We ondersteunen alleen op basis van een Ubuntu Linux-VM voor uitvoering op afstand. 

## <a name="vm-disk-is-full"></a>VM-schijf is vol
Standaard als u een nieuwe Linux-VM in Azure maken, krijgt u een schijf van 30 GB voor het besturingssysteem. Docker-engine standaard maakt gebruik van dezelfde schijf voor het binnenhalen van installatiekopieën en uitvoeren van containers. Dit kunt invullen van de besturingssysteemschijf en u een "VM schijf is vol" fout wordt weergegeven wanneer het gebeurt.

Een snelle oplossing is om te verwijderen van alle Docker-installatiekopieën die u niet meer gebruiken. De volgende Docker-opdracht doet precies dat. (Uiteraard moet SSH in de virtuele machine om de Docker-opdracht uitvoeren vanuit een bash-shell.)

```
$ docker system prune -a
```

U kunt ook een gegevensschijf toevoegen en configureren van Docker-engine voor het gebruik van de gegevensschijf voor het opslaan van installatiekopieën. Hier volgt [een gegevensschijf toevoegen](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk). U kunt vervolgens [wijzigen waar afbeeldingen worden opgeslagen in Docker](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169).

Of u kunt de besturingssysteemschijf uitbreiden en u hoeft te touch Docker engine-configuratie. Hier volgt [hoe kunt u de besturingssysteemschijf uitbreiden](https://docs.microsoft.com/azure/virtual-machines/linux/expand-disks).

```azure-cli
# Deallocate VM (stopping will not work)
$ az vm deallocate --resource-group myResourceGroup  --name myVM

# Get VM's Disc Name
az disk list --resource-group myResourceGroup --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table

# Update Disc Size using above name
$ az disk update --resource-group myResourceGroup --name myVMdisc --size-gb 250
    
# Start VM    
$ az vm start --resource-group myResourceGroup  --name myVM
```

## <a name="sharing-c-drive-on-windows"></a>Delen van station C op Windows
Als u in een lokale Docker-container op Windows worden uitgevoerd, `sharedVolumes` naar `true` in de `docker.compute` bestand onder `aml_config` uitvoering prestaties kan verbeteren. Dit vereist echter delen van C-schijf in de _Docker voor Windows hulpprogramma_. Als u niet kunnen delen van station C, kunt u het volgende proberen:

* Controleer het delen op station C met Verkenner
* Instellingen van de netwerkadapter te openen en verwijderen/opnieuw installeren 'Bestands-en printerdeling voor Microsoft-netwerken' voor vEthernet
* Docker-instellingen openen en te delen van C-station van docker-instellingen
* Wijzigingen in het Windows-wachtwoord gelden voor het delen. Open Windows Verkenner, delen van de C-schijf en voer het nieuwe wachtwoord.
* U kunt ook een firewall probleem tegenkomen wanneer bij het delen van de C-schijf met Docker. Dit [Stack Overflow post](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051) kan nuttig zijn.
* Bij het delen van station C met domeinreferenties, het delen mogelijk niet werken op netwerken waar de domeincontroller is niet bereikbaar (voor bijvoorbeeld-netwerk thuis, openbare Wi-Fi enz.). Zie voor meer informatie, [dit bericht](https://blogs.msdn.microsoft.com/stevelasker/2016/06/14/configuring-docker-for-windows-volumes/).

U kunt ook voorkomen dat het probleem met het delen, op een kleine prestaties, kosten, door in te stellen `sharedVolumne` naar `false` in de `docker.compute` bestand.

## <a name="wipe-clean-workbench-installation"></a>Schone installatie van de Workbench wissen
U nodig geen doorgaans om dit te doen. Maar in het geval u schone installatie van een wissen moet, dit zijn de stappen:

- In Windows:
  - Controleer eerst of u _toevoegen of verwijderen van programma's_ applet in de _Configuratiescherm_ verwijderen de _Azure Machine Learning Workbench_ vermelding van de toepassing.  
  - U kunt vervolgens downloaden en uitvoeren van een van de volgende scripts:
    - [Windows-opdrachtregel-script](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.cmd).
    - [Windows PowerShell-script](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_win.ps1). (Mogelijk moet u uitvoeren `Set-ExecutionPolicy Unrestricted` in een opdrachtprompt met verhoogde bevoegdheid bevoegdheid PowerShell-venster voordat u het script kunt uitvoeren.)
- In Mac OS:
  - Alleen downloaden en uitvoeren van de [macOS bash-shell-script](https://github.com/Azure/MachineLearning-Scripts/blob/master/cleanup/cleanup_mac.sh).

## <a name="azure-ml-using-a-different-python-location-than-the-azure-ml-installed-python-environment"></a>Azure ML met behulp van een andere python-locatie dan de Azure ML geïnstalleerde python-omgeving
Na een recente wijziging in Azure Machine Learning Workbench, kunnen gebruikers merken dat lokaal wordt uitgevoerd niet naar de python-omgeving niet meer door de Azure ML Workbench geïnstalleerd verwijzen kunnen. Dit kan gebeuren als de gebruiker een andere python-omgeving is geïnstalleerd op de computer en het pad 'Python' is ingesteld om te verwijzen naar die omgeving. Als u wilt gebruiken van Azure ML Workbench Python-omgeving is geïnstalleerd, als volgt te werk:
- Ga naar local.compute bestand onder de map aml_config onder de hoofdmap van uw project.
- Wijzig de variabele "pythonLocation" om te verwijzen naar het fysieke pad van Azure ML workbench geïnstalleerd python-omgeving. U kunt dit pad krijgen op twee manieren:
    - Locatie van de python Azure ML kunt u vinden op %localappdata%\AmlWorkbench\python\python.exe
    - u kunt cmd openen vanuit Azure ML Workbench, python op opdrachtprompt typt, sys.exe importeren, uitvoeren sys.executable en het pad van daaruit ophalen. 



## <a name="some-useful-docker-commands"></a>Enkele nuttige Docker-opdrachten

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
