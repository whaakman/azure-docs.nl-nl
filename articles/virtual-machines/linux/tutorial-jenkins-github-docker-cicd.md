---
title: Een pijplijn ontwikkeling maken in Azure met Jenkins | Microsoft Docs
description: Meer informatie over het maken van een Jenkins virtuele machine in Azure die ophaalt vanuit GitHub op elke doorvoer code en maakt een nieuwe Docker-container voor het uitvoeren van uw app
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d73599164589d672d6d6cde57e4a5b40774aca19
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-create-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a>Het maken van een infrastructuur voor ontwikkeling op een Linux VM in Azure met Jenkins, GitHub en Docker
U kunt een continue integratie en implementatie (CI/CD) pijplijn gebruiken voor het automatiseren van de fase bouwen en testen van de ontwikkeling van toepassingen. In deze zelfstudie maakt u een pijplijn CI/CD maken op een virtuele machine in Azure met inbegrip van hoe:

> [!div class="checklist"]
> * Maak een VM Jenkins
> * Installeren en configureren van Jenkins
> * Webhook-integratie tussen GitHub en Jenkins maken
> * Maken en activeren Jenkins build taken van GitHub doorvoeringen
> * Een Docker-installatiekopie voor uw app maken
> * Controleer of de GitHub-doorvoeracties bouwen nieuwe Docker-installatiekopie en app-updates


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.22 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-jenkins-instance"></a>Jenkins exemplaar maken
In een vorige zelfstudie over [het aanpassen van een virtuele Linux-machine op de eerste keer opstarten](tutorial-automate-vm-deployment.md), hebt u geleerd hoe u de aanpassing van de virtuele machine met cloud-init automatiseren. Deze zelfstudie wordt een bestand cloud init Jenkins en Docker wilt installeren op een virtuele machine. Jenkins is een populaire open-source automation-server die naadloos geïntegreerd met Azure zodat continue integratie (CI) en continue levering (CD). Zie voor meer zelfstudies over het gebruik van Jenkins de [Jenkins in Azure hub](https://docs.microsoft.com/azure/jenkins/).

Maak een bestand met de naam in uw huidige shell *cloud-init-jenkins.txt* en plak de volgende configuratie. Maak bijvoorbeeld het bestand in de Cloud-Shell niet op uw lokale machine. Voer `sensible-editor cloud-init-jenkins.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

Voordat u een virtuele machine maken kunt, maakt u een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupJenkins* in de *eastus* locatie:

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create). Gebruik de `--custom-data` parameter om door te geven in uw cloud-init-configuratiebestand. Geef het volledige pad naar *cloud-init-jenkins.txt* als u het bestand buiten uw werkmap aanwezig opgeslagen.

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

Het duurt enkele minuten duren voordat de virtuele machine worden gemaakt en geconfigureerd.

Gebruik zodat webverkeer bereiken van uw virtuele machine [az vm open poort](/cli/azure/vm#open-port) poort openen *8080* voor Jenkins verkeer en poort *1337* voor de Node.js-app die wordt gebruikt voor het uitvoeren van een voorbeeld-app:

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a>Jenkins configureren
Voor toegang tot uw Jenkins-exemplaar moet u het openbare IP-adres van uw virtuele machine:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Uit veiligheidsoverwegingen moet u Geef de eerste Administrator-wachtwoord die zijn opgeslagen in een tekstbestand op de virtuele machine om de Jenkins-installatie te beginnen. Gebruik het openbare IP-adres dat is verkregen in de vorige stap voor SSH met uw virtuele machine:

```bash
ssh azureuser@<publicIps>
```

Weergave de `initialAdminPassword` voor uw Jenkins installeren en dit te kopiëren:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Als het bestand is nog niet beschikbaar, wacht u een paar minuten totdat de cloud-init om de Jenkins en Docker-installatie te voltooien.

Nu open een webbrowser en Ga naar `http://<publicIps>:8080`. Voer de eerste Jenkins installatie als volgt:

- Geef de gebruikersnaam **admin**, geef dan de *initialAdminPassword* verkregen van de virtuele machine in de vorige stap.
- Selecteer **beheren Jenkins**, klikt u vervolgens **invoegtoepassingen beheren**.
- Kies **beschikbaar**, zoekt u naar *GitHub* in het tekstvak aan de bovenkant. Schakel het selectievakje voor *GitHub-invoegtoepassing*, selecteer daarna **nu downloaden en installeren na opnieuw opstarten**.
- Schakel het selectievakje voor **Jenkins opnieuw starten wanneer de installatie voltooid is en er geen taken worden uitgevoerd**, en wacht totdat de invoegtoepassing installeren proces is voltooid.


## <a name="create-github-webhook"></a>GitHub webhook maken
Voor het configureren van de integratie met GitHub, opent u de [Node.js Hallo wereld-voorbeeld-app](https://github.com/Azure-Samples/nodejs-docs-hello-world) uit de opslagplaats Azure-voorbeelden. Om te vertakken de opslagplaats op uw eigen GitHub-account, selecteert u de **Fork** knop in de rechterbovenhoek.

Maak een webhook binnen de fork die u hebt gemaakt:

- Selecteer **instellingen**, selecteer daarna **integraties & services** aan de linkerkant.
- Kies **service toevoegen**, voer dan *Jenkins* in het filtervak.
- Selecteer *Jenkins (GitHub-invoegtoepassing)*
- Voor de **Jenkins hook URL**, voer `http://<publicIps>:8080/github-webhook/`. Zorg ervoor dat u de afsluitende /
- Selecteer **service toevoegen**

![GitHub webhook toevoegen aan uw Gevorkte opslagplaats](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a>Jenkins taak maken
Om te reageren op een gebeurtenis Jenkins in GitHub zoals het vastleggen van de code hebt, kunt u een Jenkins-taak maken. 

Selecteer in uw website Jenkins **nieuwe taken maken** vanaf de startpagina:

- Voer *HelloWorld* als de taaknaam van de. Kies **Freestyle project**, selecteer daarna **OK**.
- Onder de **algemene** sectie **GitHub** project en voer uw Gevorkte repo-URL, zoals *https://github.com/iainfoulds/nodejs-docs-hello-world*
- Onder de **Source code management** sectie **Git**, Voer uw Gevorkte opslagplaats *.git* -URL, zoals *https://github.com/iainfoulds/nodejs-docs-hello-world.git*
- Onder de **bouwen Triggers** sectie **GitHub haakje trigger voor GITscm polling**.
- Onder de **bouwen** sectie **toevoegen build stap**. Selecteer **shell uitvoeren**, voer dan `echo "Testing"` in naar opdrachtvenster.
- Selecteer **opslaan** aan de onderkant van het venster met taken.


## <a name="test-github-integration"></a>Integratie van GitHub testen
Als u wilt testen op de GitHub-integratie met Jenkins, een wijziging in uw fork worden doorgevoerd. 

Terug in de GitHub-webgebruikersinterface, selecteer uw Gevorkte opslagplaats en selecteer vervolgens de **index.js** bestand. Selecteer het potloodpictogram dit bestand bewerken, zodat de regel 6 leest:

```nodejs
response.end("Hello World!");
```

Als u wilt uw wijzigingen doorvoert, selecteer de **wijzigingen doorvoeren** knop onderaan.

Een nieuwe build begint in Jenkins, onder de **bouwen geschiedenis** sectie van de linkerbenedenhoek van de pagina van de taak. Kies de koppeling van build-nummer en selecteert u **Console uitvoer** op de links grootte. U kunt de stappen Jenkins wordt als uw code wordt opgehaald uit GitHub bekijken en de build-actie levert het bericht `Testing` naar de console. Telkens wanneer een doorvoer wordt gemaakt in GitHub, de webhook gezocht naar Jenkins en activeren van een nieuwe build op deze manier.


## <a name="define-docker-build-image"></a>Afbeelding van Docker-build definiëren
Om te zien van de Node.js-app uitgevoerd op basis van uw GitHub-doorvoeracties kunnen maken van een installatiekopie Docker uitvoeren van de app. De afbeelding wordt samengesteld uit een Dockerfile die definieert het configureren van de container waarin de app wordt uitgevoerd. 

Wijzig de Jenkins werkruimte map met de naam van de taak die u in de vorige stap hebt gemaakt van de SSH-verbinding met uw virtuele machine. In dit voorbeeld is die de naam *HelloWorld*.

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

Maken van een bestand met de in deze map werkruimte met `sudo sensible-editor Dockerfile` en plak de volgende inhoud. Controleer of de hele Dockerfile correct is gekopieerd met name de eerste regel:

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

Deze Dockerfile maakt gebruik van de Node.js basisinstallatiekopie gebruik Alpine Linux, zichtbaar gemaakt poort 1337 die de Hallo wereld-app wordt uitgevoerd, klikt u vervolgens kopieert de app-bestanden en geïnitialiseerd met behulp van.


## <a name="create-jenkins-build-rules"></a>Jenkins build regels maken
In de vorige stap, moet u een eenvoudige Jenkins build regel die een bericht naar de console uitvoeren gemaakt. U kunt de build-stap voor het gebruik van onze Dockerfile en voer de app te maken.

Terug in uw exemplaar Jenkins selecteert u de taak die u in de vorige stap hebt gemaakt. Selecteer **configureren** op de linkerkant en blader omlaag naar de **bouwen** sectie:

- Verwijder de bestaande `echo "Test"` stap bouwen. Selecteer de rode x in de rechterbovenhoek van het bestaande build stap vak.
- Kies **toevoegen build stap**, selecteer daarna **shell uitvoeren**
- In de **opdracht** vak, voert de volgende Docker-opdrachten en selecteer **opslaan**:

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

De stappen van de build Docker maken een installatiekopie en de tag met de Jenkins build-nummer zodat u kunt een geschiedenis van installatiekopieën onderhouden. Eventuele bestaande containers uitvoeren van de app worden gestopt en verwijderd. Een nieuwe container vervolgens met behulp van de installatiekopie is gestart en voert u uw Node.js-app op basis van de meest recente doorvoeringen in GitHub.


## <a name="test-your-pipeline"></a>Test uw pijplijn
Overzicht van de hele pijplijn in actie bewerken de *index.js* bestand in uw GitHub-repo-Gevorkte opnieuw en selecteer **wijziging doorvoeren**. Een nieuwe taak wordt gestart met Jenkins op basis van de webhook voor GitHub. Het duurt een paar seconden de Docker-installatiekopie maken en uw app te starten in een nieuwe container.

Indien nodig, moet u het openbare IP-adres van uw virtuele machine opnieuw:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Open een webbrowser en voer `http://<publicIps>:1337`. Uw Node.js-app wordt weergegeven en reflecteert de meest recente doorvoeringen in uw GitHub-fork als volgt:

![Actieve Node.js-app](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

Maak nu een andere bewerken naar de *index.js* bestand in GitHub en de wijziging doorvoeren. Wacht een paar seconden voor de taak te voltooien in Jenkins en vernieuw vervolgens uw webbrowser als u wilt zien van de bijgewerkte versie van uw app uitgevoerd in een nieuwe container als volgt:

![Node.js-app uitgevoerd na een ander GitHub doorvoeren](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u geconfigureerd GitHub als u wilt een Jenkins build-taak uitvoeren op elke doorvoer code en vervolgens implementeert een Docker-container als uw app wilt testen. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Maak een VM Jenkins
> * Installeren en configureren van Jenkins
> * Webhook-integratie tussen GitHub en Jenkins maken
> * Maken en activeren Jenkins build taken van GitHub doorvoeringen
> * Een Docker-installatiekopie voor uw app maken
> * Controleer of de GitHub-doorvoeracties bouwen nieuwe Docker-installatiekopie en app-updates

Ga naar de volgende zelfstudie voor meer informatie over het integreren van Jenkins met Visual Studio Team Services.

> [!div class="nextstepaction"]
> [Apps implementeren met Jenkins en teamservices](tutorial-build-deploy-jenkins.md)