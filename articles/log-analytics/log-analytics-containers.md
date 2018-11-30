---
title: Container Monitoring-oplossing in Azure Log Analytics | Microsoft Docs
description: De Container Monitoring solution in Log Analytics kunt u weergeven en beheren van uw Docker- en Windows containerhosts op één locatie.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 81728e7963767cd90bf2486a35fbce55043a76f7
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52633466"
---
# <a name="container-monitoring-solution-in-log-analytics"></a>Container Monitoring solution in Log Analytics

![Symbool van containers](./media/log-analytics-containers/containers-symbol.png)

Dit artikel wordt beschreven hoe u kunt instellen en gebruiken van de Container Monitoring solution in Log Analytics, dat helpt u bij het weergeven en beheren van uw Docker- en Windows containerhosts op één locatie. Docker is een software-virtualisatie-systeem gebruikt voor het maken van containers voor het automatiseren van software-implementatie naar de IT-infrastructuur.

De oplossing laat zien welke containers worden uitgevoerd, welke containerinstallatiekopie deze nu worden uitgevoerd en waarop de containers worden uitgevoerd. Hier vindt u gedetailleerde controle-informatie met opdrachten die worden gebruikt met containers. En u kunt containers oplossen weer te geven en gecentraliseerde logboeken zoeken zonder dat om Docker of Windows-hosts op afstand weer te geven. Hier vindt u containers die mogelijk veel ruis veroorzaken en in beslag nemen veel bronnen op een host. Daarnaast vindt u gecentraliseerde CPU, geheugen, opslag en gebruiks- en netwerkgegevens voor containers. Op computers waarop Windows wordt uitgevoerd, kunt u centraliseren en vergelijken van Logboeken van Windows Server, Hyper-V en Docker-containers. De oplossing biedt ondersteuning voor de volgende containerorchestrators:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift

Als u geïnteresseerd bent in het controleren van de prestaties van uw workloads die zijn geïmplementeerd in Kubernetes-omgevingen die worden gehost in Azure Kubernetes Service (AKS), raadpleegt u [Monitor Azure Kubernetes Service](../azure-monitor/insights/container-insights-overview.md). De Container Monitoring-oplossing bevat geen ondersteuning voor het controleren van dit platform.  

Het volgende diagram toont de relaties tussen verschillende hosts van de container en agents met Log Analytics.

![Diagram van containers](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements-and-supported-platforms"></a>Systeemvereisten en ondersteunde platforms

Voordat u begint, lees de volgende informatie om te controleren of u voldoet aan de vereisten.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Bewakingsoplossing voor containers, ondersteuning voor Docker-Orchestrator en OS-platform
De volgende tabel geeft een overzicht van de Docker-indeling en de bewaking van de ondersteuning van container-inventarisatie, prestaties en logboeken met Log Analytics-besturingssysteem.   

| | ACS | Linux | Windows | Container<br>Inventaris | Installatiekopie<br>Inventaris | Knooppunt<br>Inventaris | Container<br>Prestaties | Container<br>Gebeurtenis | Gebeurtenis<br>Logboek | Container<br>Logboek |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Kubernetes | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mesosphere<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Swarm | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Service<br>Fabric | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Red Hat openen<br>Shift | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(zelfstandig) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux-server<br>(zelfstandig) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |


### <a name="docker-versions-supported-on-linux"></a>Docker-versies ondersteund in Linux

- Docker 1.11-1.13
- Docker CE en EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>x64 Linux-distributies ondersteund als hosts van de container


- Ubuntu 14.04 LTS en 16.04 LTS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE 42.2 LEAP
- CentOS 7.2 en 7.3
- SLES 12
- RHEL 7.2 en 7.3
- Red Hat OpenShift Containerplatform (OCP) 3.4 en 3.5
- ACS-Mesosphere DC/OS 1.7.3-1.8.8
- ACS-Kubernetes 1.4.5-1.6
    - Kubernetes-gebeurtenissen, inventaris van Kubernetes en processen van de container worden alleen ondersteund met versie 1.4.1-45 en hoger van de Log Analytics-agent voor Linux
- ACS Docker Swarm

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)] 

### <a name="supported-windows-operating-system"></a>Ondersteunde Windows-besturingssysteem

- Windows Server 2016
- Windows 10 Anniversary Edition (Professional of Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Docker-versies ondersteund op Windows

- Docker 1.12 en 1.13
- Docker 17.03.0 en hoger

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om de oplossing te installeren en configureren.

1. Container Monitoring solution toevoegen aan uw Log Analytics-werkruimte van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen uit de galerie van oplossingen](../azure-monitor/insights/solutions.md).

2. Installeer en Docker gebruiken met een Log Analytics-agent. Op basis van uw besturingssysteem en de Docker-orchestrator, kunt u de volgende methoden gebruiken om de agent te configureren.
  - Voor zelfstandige hosts:
    - Installeren op ondersteunde Linux-besturingssystemen, en het uitvoeren van Docker en het vervolgens installeert en configureert de [Log Analytics-agent voor Linux](log-analytics-quick-collect-linux-computer.md).  
    - In CoreOS, kunt u de Log Analytics-agent voor Linux niet uitvoeren. In plaats daarvan kunt u een beperkte versie van de Log Analytics-agent voor Linux uitvoeren. Beoordeling [Linux containerhosts met inbegrip van CoreOS](#for-all-linux-container-hosts-including-coreos) of [Azure Government Linux containerhosts met inbegrip van CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) als u met containers in Azure Government-Cloud werkt.
    - De Docker-Engine en -client installeren op Windows Server 2016 en Windows 10, en verbinding maken met een agent om te verzamelen en te verzenden naar Log Analytics. Beoordeling [installeren en configureren van Windows-containerhosts](#install-and-configure-windows-container-hosts) hebt u een Windows-omgeving.
  - Voor het indelen van Docker-meerdere host:
    - Als u een Red Hat OpenShift-omgeving hebt, raadpleegt u [configureren van een Log Analytics-agent voor Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).
    - Als u een Kubernetes-cluster met behulp van de Azure Container Service:
       - Beoordeling [configureren een Log Analytics Linux-agent voor Kubernetes](#configure-an-oms-linux-agent-for-kubernetes).
       - Beoordeling [configureren van een Log Analytics-Windows-agent voor Kubernetes](#configure-an-oms-windows-agent-for-kubernetes).
       - Beoordeling [gebruik Helm om te implementeren van Log Analytics-agent op Linux Kubernetes](#use-helm-to-deploy-oms-agent-on-linux-kubernetes).
    - Als u een Azure Container Service DC/OS-cluster hebt, voor meer informatie naar [een Azure Container Service DC/OS-cluster bewaken met Log Analytics](../container-service/dcos-swarm/container-service-monitoring-oms.md).
    - Hebt u een omgeving met Docker Swarm-modus, voor meer informatie naar [configureren van een Log Analytics-agent voor Docker Swarm](#configure-an-oms-agent-for-docker-swarm).
    - Als u een Service Fabric-cluster hebt, voor meer informatie naar [bewaken van containers met Log Analytics Log Analytics](../service-fabric/service-fabric-diagnostics-oms-containers.md).

Controleer de [Docker-Engine op Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) artikel voor meer informatie over het installeren en configureren uw Docker-Engines op computers waarop Windows wordt uitgevoerd.

> [!IMPORTANT]
> Docker moet worden uitgevoerd **voordat** u installeert de [Log Analytics-agent voor Linux](log-analytics-quick-collect-linux-computer.md) op uw hosts van de container. Als u de agent al hebt geïnstalleerd voordat u Docker installeert, moet u de Log Analytics-agent voor Linux opnieuw installeren. Zie voor meer informatie over Docker, de [Docker website](https://www.docker.com).


### <a name="install-and-configure-linux-container-hosts"></a>Installeer en configureer de hosts van de Linux-container

Nadat u Docker hebt geïnstalleerd, gebruikt u de volgende instellingen voor uw containerhost configureren van de agent voor gebruik met Docker. U moet eerst uw Log Analytics-werkruimte-ID en sleutel, die u in de Azure-portal vinden kunt. Klik in uw werkruimte **Quick Start** > **Computers** om weer te geven uw **werkruimte-ID** en **primaire sleutel**.  Kopieer en plak beide in uw favoriete editor.

**Voor alle hosts door Linux-container met uitzondering van CoreOS:**

- Zie voor meer informatie en stappen voor het installeren van de Log Analytics-agent voor Linux, [Log Analytics-agent overzicht](../azure-monitor/platform/log-analytics-agent.md).

**Voor alle Linux-containerhosts met inbegrip van CoreOS:**

Start de container die u wilt bewaken. Wijzig en gebruik het volgende voorbeeld:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/containers:/var/lib/docker/containers -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

**Voor alle Azure Government Linux containerhosts met inbegrip van CoreOS:**

Start de container die u wilt bewaken. Wijzig en gebruik het volgende voorbeeld:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -v /var/lib/docker/containers:/var/lib/docker/containers -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

**Overschakelen van het gebruik van een geïnstalleerde Linux-agent op een in een container**

Als u eerder hebt gebruikt van de agent rechtstreeks-geïnstalleerd en u wilt gebruiken in plaats daarvan een agent die wordt uitgevoerd in een container, moet u eerst de Log Analytics-agent voor Linux te verwijderen. Zie [verwijderen van de Log Analytics-agent voor Linux](log-analytics-quick-collect-linux-computer.md) wilt weten hoe u de agent correct te verwijderen.  

#### <a name="configure-a-log-analytics-agent-for-docker-swarm"></a>Configureren van een Log Analytics-agent voor Docker Swarm

U kunt de Log Analytics-agent uitvoeren als een globale service op het Docker Swarm. Gebruik de volgende informatie om een Log Analytics-agent-service te maken. U moet uw Log Analytics-werkruimte-ID en primaire sleutel opgeven.

- Voer de volgende op het hoofdknooppunt.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --mount type=bind,source=/var/lib/docker/containers,destination=/var/lib/docker/containers -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

##### <a name="secure-secrets-for-docker-swarm"></a>Beveiligde geheimen voor Docker Swarm

Gebruik de volgende informatie om te maken van uw geheime informatie voor Docker Swarm, zodra het geheim voor werkruimte-ID en primaire sleutel is gemaakt.

1. Voer de volgende op het hoofdknooppunt.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Controleer of dat geheimen juist zijn gemaakt.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Voer de volgende opdracht om te koppelen van de geheimen in de beperkte Log Analytics-agent.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --mount type=bind,source=/var/lib/docker/containers,destination=/var/lib/docker/containers --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="configure-a-log-analytics-agent-for-red-hat-openshift"></a>Een Log Analytics-agent voor Red Hat OpenShift configureren
Er zijn drie manieren om toe te voegen van de Log Analytics-agent voor Red Hat OpenShift om te beginnen met het verzamelen van bewakingsgegevens van de container.

* [De Log Analytics-agent voor Linux installeren](log-analytics-quick-collect-linux-computer.md) rechtstreeks op elk knooppunt van OpenShift  
* [Log Analytics VM-extensie inschakelen](log-analytics-quick-collect-azurevm.md) op elk knooppunt OpenShift is die zich bevinden in Azure  
* De Log Analytics-agent installeren als een OpenShift-daemon-set  

In deze sectie behandelen we de stappen die nodig zijn voor het installeren van de Log Analytics-agent als een OpenShift-daemon-set.  

1. Meld u aan bij met het hoofdknooppunt van OpenShift en kopieer het yaml-bestand [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) vanuit GitHub naar het hoofdknooppunt en wijzig de waarde met de ID van uw Log Analytics-werkruimte en met uw primaire sleutel.
2. Voer de volgende opdrachten een project maken voor Log Analytics en het instellen van het gebruikersaccount.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Voor het implementeren van de daemon-set, voert u het volgende uit:

    `oc create -f ocp-omsagent.yaml`

5. Controleer of dat deze is geconfigureerd en naar behoren werkt, typt u het volgende:

    `oc describe daemonset omsagent`  

    en de uitvoer moet eruitzien als:

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

Als u gebruiken van geheimen wilt voor het beveiligen van uw Log Analytics-werkruimte-ID en primaire sleutel bij het gebruik van de Log Analytics-agent-daemon-set yaml-bestand, moet u de volgende stappen uitvoeren.

1. Meld u aan bij met het hoofdknooppunt van OpenShift en kopieer het yaml-bestand [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) en -geheim script genereren [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) vanuit GitHub.  Met dit script genereert de geheimen yaml-bestand voor Log Analytics-werkruimte-ID en primaire sleutel voor het beveiligen van uw informatie secrete.  
2. Voer de volgende opdrachten een project maken voor Log Analytics en het instellen van het gebruikersaccount. Het geheim genereren script vraagt om de ID van uw Log Analytics-werkruimte <WSID> en primaire sleutel <KEY> en na voltooiing wordt de ocp-secret.yaml-bestand gemaakt.  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Het geheime bestand door het uitvoeren van de volgende implementeren:

    `oc create -f ocp-secret.yaml`

5. Implementatie controleren door het uitvoeren van de volgende:

    `oc describe secret omsagent-secret`  

    en de uitvoer moet eruitzien als:  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. Implementeer de Log Analytics-agent-daemon-set yaml-bestand door het uitvoeren van de volgende:

    `oc create -f ocp-ds-omsagent.yaml`  

7. Implementatie controleren door het uitvoeren van de volgende:

    `oc describe ds oms`

    en de uitvoer moet eruitzien als:

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

#### <a name="configure-a-log-analytics-linux-agent-for-kubernetes"></a>Configureren van een Log Analytics Linux-agent voor Kubernetes

Voor Kubernetes, kunt u een script gebruiken om de geheimen yaml-bestand voor uw werkruimte-ID en primaire sleutel voor het installeren van de Log Analytics-agent voor Linux te genereren. Op de [Log Analytics Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) pagina, er zijn bestanden die u met of zonder uw geheime informatie gebruiken kunt.

- De standaard Log Analytics-agent voor Linux DaemonSet heeft geen geheime informatie (omsagent.yaml)
- De Log Analytics-agent voor Linux DaemonSet yaml-bestand maakt gebruik van geheime informatie (omsagent-ds-secrets.yaml) met behulp van scripts geheim genereren om de geheimen (omsagentsecret.yaml) yaml-bestand te genereren.

U kunt omsagent DaemonSets maken met of zonder geheimen.

**Standaard OMSagent DaemonSet yaml-bestand zonder geheimen**

- Voor de standaard Log Analytics-agent DaemonSet yaml-bestand, vervangt u de `<WSID>` en `<KEY>` aan uw WSID en -sleutel. Kopieer het bestand naar het hoofdknooppunt en voert de volgende opdracht uit:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

**Standaard OMSagent DaemonSet yaml-bestand met geheimen**

1. Gebruik van Log Analytics-agent DaemonSet met behulp van geheime informatie, maken de geheimen eerst.
    1. Kopieer het script en de geheime sjabloonbestand en zorg ervoor dat ze zich in dezelfde map.
        - Geheim script - geheim gen.sh genereren
        - geheime sjabloon - geheim template.yaml
    2. Voer het script, zoals in het volgende voorbeeld. Het script vraagt om de Log Analytics-werkruimte-ID en primaire sleutel en nadat u deze invoert, het script maakt u een geheim yaml-bestand, zodat u deze kunt uitvoeren.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Maak de schil geheimen door het uitvoeren van de volgende:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Om te controleren, voert u het volgende uit:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Uitvoer moet eruitzien als:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Uitvoer moet eruitzien als:

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Maken van uw omsagent-daemon-set door uit te voeren ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. Controleer of de Log Analytics-agent DaemonSet is uitgevoerd, die vergelijkbaar is met het volgende:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Bij Kubernetes wordt een script te gebruiken voor het genereren van de geheimen yaml-bestand voor de werkruimte-ID en primaire sleutel voor de Log Analytics-agent voor Linux. Gebruik de volgende voorbeeldinformatie met de [omsagent yaml-bestand](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) uw geheime gegevens kunt beveiligen.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

#### <a name="configure-a-log-analytics-windows-agent-for-kubernetes"></a>Configureren van een Log Analytics-Windows-agent voor Kubernetes
Voor Windows Kubernetes, kunt u een script gebruiken voor het genereren van de geheimen yaml-bestand voor uw werkruimte-ID en primaire sleutel voor het installeren van de Log Analytics-agent. Op de [Log Analytics Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes/windows) pagina, er zijn bestanden die u met uw geheime informatie gebruiken kunt.  U moet de Log Analytics-agent voor de hoofd- en agentknooppunten afzonderlijk installeren.  

1. Gebruik van Log Analytics-agent DaemonSet met behulp van geheime informatie op de Hoofddoelserver knooppunt, meld u aan en maakt u eerst de geheimen.
    1. Kopieer het script en de geheime sjabloonbestand en zorg ervoor dat ze zich in dezelfde map.
        - Geheim script - geheim gen.sh genereren
        - geheime sjabloon - geheim template.yaml

    2. Voer het script, zoals in het volgende voorbeeld. Het script vraagt om de Log Analytics-werkruimte-ID en primaire sleutel en nadat u deze invoert, het script maakt u een geheim yaml-bestand, zodat u deze kunt uitvoeren.   

        ```
        #> sudo bash ./secret-gen.sh
        ```
    3. Maken van uw omsagent-daemon-set door uit te voeren ``` kubectl create -f omsagentsecret.yaml ```
    4. Als u wilt controleren, voert u het volgende uit:

        ```
        root@ubuntu16-13db:~# kubectl get secrets
        ```

        Uitvoer moet eruitzien als:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Maken van uw omsagent-daemon-set door uit te voeren ```kubectl create -f ws-omsagent-de-secrets.yaml```

2. Controleer of de Log Analytics-agent DaemonSet is uitgevoerd, die vergelijkbaar is met het volgende:

    ```
    root@ubuntu16-13db:~# kubectl get deployment omsagent
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   1         1         <none>          1h
    ```

3. Volg de stappen in de sectie voor het installeren van de agent op de Worker-knooppunt, waarop wordt uitgevoerd Windows, [installeren en configureren van Windows-containerhosts](#install-and-configure-windows-container-hosts).

#### <a name="use-helm-to-deploy-log-analytics-agent-on-linux-kubernetes"></a>Helm gebruiken voor het implementeren van Log Analytics-agent op Linux Kubernetes
Voor het gebruik van helm om Log Analytics-agent op uw Linux Kubernetes-omgeving te implementeren, moet u de volgende stappen uitvoeren.

1. Maken van uw omsagent-daemon-set door uit te voeren ```helm install --name omsagent --set omsagent.secret.wsid=<WSID>,omsagent.secret.key=<KEY> stable/msoms```
2. De resultaten ziet eruit als het volgende:

    ```
    NAME:   omsagent
    LAST DEPLOYED: Tue Sep 19 20:37:46 2017
    NAMESPACE: default
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/Secret
    NAME            TYPE    DATA  AGE
    omsagent-msoms  Opaque  3     3s

    ==> v1beta1/DaemonSet
    NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE-SELECTOR  AGE
    omsagent-msoms  3        3        3      3           3          <none>         3s
    ```
3. U kunt de status van de omsagent controleren door te voeren: ```helm status "omsagent"``` en de uitvoer ziet er ongeveer als volgt:

    ```
    keiko@k8s-master-3814F33-0:~$ helm status omsagent
    LAST DEPLOYED: Tue Sep 19 20:37:46 2017
    NAMESPACE: default
    STATUS: DEPLOYED
 
    RESOURCES:
    ==> v1/Secret
    NAME            TYPE    DATA  AGE
    omsagent-msoms  Opaque  3     17m
 
    ==> v1beta1/DaemonSet
    NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE-SELECTOR  AGE
    omsagent-msoms  3        3        3      3           3          <none>         17m
    ```
Ga voor meer informatie naar [Container-oplossing Helm-diagram](https://aka.ms/omscontainerhelm).

### <a name="install-and-configure-windows-container-hosts"></a>Installeer en configureer de hosts van de Windows-container

Gebruik de informatie in de sectie installeren en configureren van de hosts van de Windows-container.

#### <a name="preparation-before-installing-windows-agents"></a>Voorbereiding voor de installatie van Windows-agents

Voordat u agents op computers waarop Windows wordt uitgevoerd installeren, moet u de Docker-service configureren. De configuratie kunt de Windows-agent of de extensie van de Log Analytics-virtuele machine het gebruik van de Docker-TCP-sockets, zodat de agents op afstand toegang krijgen de Docker-daemon tot kunnen en gegevens voor het bewaken van vast te leggen.

##### <a name="to-start-docker-and-verify-its-configuration"></a>Docker starten en controleren van de configuratie

Er zijn stappen die nodig zijn voor het instellen van TCP benoemde pipe voor Windows Server:

1. In Windows PowerShell, schakelt u pipe TCP en named pipes.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Docker configureren met het configuratiebestand voor TCP-pipe en benoemde pipe. Het configuratiebestand bevindt C:\ProgramData\docker\config\daemon.json.

    In het bestand daemon.json nodig u het volgende:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Zie voor meer informatie over de configuratie van de Docker-daemon gebruikt in combinatie met Windows-Containers, [Docker-Engine op Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


#### <a name="install-windows-agents"></a>Windows-agents installeren

Als u Windows- en Hyper-V-containers bewaken, installeert u Microsoft Monitoring Agent (MMA) op Windows-computers die containerhosts zijn. Voor computers waarop Windows wordt uitgevoerd in uw on-premises-omgeving, Zie [verbinding maken met Windows-computers naar Log Analytics](../azure-monitor/platform/agent-windows.md). Voor virtuele machines die worden uitgevoerd in Azure, verbind deze met Log Analytics met behulp van de [extensie van virtuele machine](log-analytics-quick-collect-azurevm.md).

U kunt Windows-containers die worden uitgevoerd in Service Fabric bewaken. Echter alleen [virtuele machines die worden uitgevoerd in Azure](log-analytics-quick-collect-azurevm.md) en [computers waarop Windows wordt uitgevoerd in uw on-premises omgeving](../azure-monitor/platform/agent-windows.md) worden momenteel ondersteund voor Service Fabric.

U kunt controleren of de Container Monitoring solution juist is ingesteld voor Windows. Als u wilt controleren of het managementpack downloaden goed is, zoekt *ContainerManagement.xxx*. De bestanden moeten zich in de map C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management servicepacks.


## <a name="solution-components"></a>Oplossingsonderdelen

Vanuit de Azure-portal, gaat u naar de *Oplossingengalerie* en voeg de **Container Monitoring Solution**. Als u van Windows-agents gebruikmaakt, is klikt u vervolgens de volgende managementpack geïnstalleerd op elke computer met een agent wanneer u deze oplossing toevoegt. Er zijn geen configuraties of onderhoud is vereist voor het managementpack.

- *ContainerManagement.xxx* in C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management servicepacks zijn geïnstalleerd

## <a name="container-data-collection-details"></a>Details van container gegevens verzamelen
De oplossing Container Monitoring verzamelt verschillende metrische gegevens en logboekbestanden prestatiegegevens van containerhosts en -containers met agents die u inschakelt.

Gegevens worden elke drie minuten worden verzameld door de volgende typen van de agent.

- [Log Analytics-agent voor Linux](log-analytics-quick-collect-linux-computer.md)
- [Windows-agent](../azure-monitor/platform/agent-windows.md)
- [Log Analytics VM-extensie](log-analytics-quick-collect-azurevm.md)


### <a name="container-records"></a>Container-records

De volgende tabel ziet u voorbeelden van records die zijn verzameld door de Container Monitoring-oplossing en de gegevenstypen die worden weergegeven in de resultaten van de logboekzoekopdracht.

| Gegevenstype | Het gegevenstype in zoeken in Logboeken | Velden |
| --- | --- | --- |
| Prestaties voor hosts en -containers | `Perf` | Computer, ObjectName, CounterName &#40;% processortijd, schijf leest MB, schijf schrijft MB, gebruik van geheugen (MB), netwerk ontvangen Bytes, netwerk verzenden Bytes, Processor gebruik seconde, netwerk&#41;, CounterValue, TimeGenerated, itempad, SourceSystem |
| Container-inventaris | `ContainerInventory` | TimeGenerated, de Computer, de containernaam, ContainerHostname, afbeelding, ImageTag, ContainerState, ExitCode, EnvironmentVar, opdracht, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Inventarisatie van container-installatiekopie | `ContainerImageInventory` | TimeGenerated, Computer, afbeelding, ImageTag, ImageSize, VirtualSize, die wordt uitgevoerd, is onderbroken, gestopt, is mislukt, SourceSystem, ImageID, TotalContainer |
| Container-logboek | `ContainerLog` | TimeGenerated, Computer, afbeeldings-ID, containernaam van de, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Container service-logboek | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, afbeelding, opdracht, SourceSystem, ContainerID |
| Inventaris van containerknooppunten | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Inventaris van Kubernetes | `KubePodInventory_CL` | TimeGenerated, Computer, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem |
| Containerverwerking | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Kubernetes-gebeurtenissen | `KubeEvents_CL` | TimeGenerated, Computer, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, bericht |

Labels toegevoegd aan *PodLabel* gegevenstypen zijn uw eigen aangepaste etiketten. De toegevoegde PodLabel labels weergegeven in de tabel zijn voorbeelden. Dus `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` zal verschillen in de gegevensset van uw omgeving en algemeen lijken op `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>Containers bewaken
Nadat u de oplossing ingeschakeld in de Log Analytics-portal is, de **Containers** tegel bevat overzichtsinformatie over uw containerhosts en -containers die worden uitgevoerd op hosts.


![Containers-tegel](./media/log-analytics-containers/containers-title.png)

De tegel geeft een overzicht van hoeveel containers er in de omgeving en of ze zijn mislukt, wordt uitgevoerd of gestopt.

### <a name="using-the-containers-dashboard"></a>Met behulp van de Containers-dashboard
Klik op de **Containers** tegel. Daar ziet u de weergaven die zijn geordend op:

- **Containergebeurtenissen** -ziet u de containerstatus van de en computers met mislukte containers.
- **Logboeken voor containers** -bevat een grafiek van container-logboekbestanden gegenereerd voor tijd en een lijst van computers met het hoogste aantal logboekbestanden.
- **Kubernetes-gebeurtenissen** -bevat een grafiek met Kubernetes-gebeurtenissen die worden gegenereerd voor tijd en een lijst van de redenen waarom schillen die de gebeurtenissen worden gegenereerd. *Deze gegevensset wordt alleen gebruikt in Linux-omgevingen.*
- **Kubernetes Namespace-inventaris** : bevat het nummer van naamruimten en pods en toont hun hiërarchie. *Deze gegevensset wordt alleen gebruikt in Linux-omgevingen.*
- **Inventaris van Containerknooppunten** -bevat het nummer van de orchestration-typen die zijn gebruikt op de container-knooppunten /-hosts. De computer knooppunten /-hosts worden ook weergegeven op het aantal containers. *Deze gegevensset wordt alleen gebruikt in Linux-omgevingen.*
- **Voorraad Containerinstallatiekopieën** -toont het totale aantal containerinstallatiekopieën gebruikt en het aantal afbeeldingstypen. Het aantal afbeeldingen worden ook weergegeven met het label de installatiekopie.
- **Containersstatus** -toont het totale aantal container-knooppunten/host-computers met actieve containers. Computers worden ook weergegeven door het aantal actieve hosts.
- **Containerverwerking** -ziet u een lijndiagram van processen die na verloop van tijd worden uitgevoerd container. Containers worden ook vermeld door uit te voeren opdracht/proces binnen containers. *Deze gegevensset wordt alleen gebruikt in Linux-omgevingen.*
- **CPU-prestaties van container** -ziet u een lijndiagram van het gemiddelde CPU-gebruik na verloop van tijd voor computer-knooppunten /-hosts. Ook een lijst met de computer-knooppunten/hosts op basis van Gemiddeld CPU-gebruik.
- **Prestaties van Containergeheugen** -ziet u een lijndiagram van geheugengebruik na verloop van tijd. Bevat ook gebruik gebaseerd op de instantienaam van de computergeheugen.
- **Prestaties van de computer** -ziet u lijndiagrammen van het percentage van CPU-prestaties na verloop van tijd percentage geheugenverbruik na verloop van tijd en MB aan vrije schijfruimte na verloop van tijd. U kunt de muisaanwijzer over een regel in een grafiek om meer details weer te geven.


Elk gebied van het dashboard is een visuele representatie van een zoekopdracht die wordt uitgevoerd op de verzamelde gegevens.

![Dashboard containers](./media/log-analytics-containers/containers-dash01.png)

![Dashboard containers](./media/log-analytics-containers/containers-dash02.png)

In de **containerstatus** gebied, klikt u op het bovenste gedeelte, zoals hieronder wordt weergegeven.

![Containersstatus van](./media/log-analytics-containers/containers-status.png)

Zoeken in Logboeken met de informatie over de status van uw containers wordt geopend.

![Zoeken in Logboeken voor containers](./media/log-analytics-containers/containers-log-search.png)

Hier kunt kunt u de zoekopdracht om te wijzigen om de gewenste informatie vindt dat u geïnteresseerd bent in bewerken. Zie voor meer informatie over zoekopdrachten in Logboeken, [zoekopdrachten in Logboeken in Log Analytics](log-analytics-queries.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Problemen oplossen met het vinden van een mislukte container

Log Analytics markeert een container als **mislukt** als deze is afgesloten met een afsluitcode dan nul. U kunt een overzicht van de fouten en storingen in de omgeving in de **Containers is mislukt** gebied.

### <a name="to-find-failed-containers"></a>Mislukte containers vinden
1. Klik op de **containerstatus** gebied.  
   ![Containersstatus van](./media/log-analytics-containers/containers-status.png)
2. Zoeken in Logboeken wordt geopend en wordt de status van uw containers, de volgende strekking weergegeven.  
   ![status van containers](./media/log-analytics-containers/containers-log-search.png)
3. Klik vervolgens op de geaggregeerde waarde van de mislukte containers om aanvullende informatie weer te geven. Vouw **meer weergeven** om weer te geven van de installatiekopie-ID.  
   ![mislukte containers](./media/log-analytics-containers/containers-state-failed.png)  
4. Vervolgens typt u het volgende in de zoekquery. `ContainerInventory <ImageID>` voor informatie over de afbeeldingen, zoals de grootte van installatiekopie en het aantal afbeeldingen gestopt en is mislukt.  
   ![mislukte containers](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Zoeken in Logboeken voor containergegevens
Als u problemen met een specifieke fout, kunt u om te zien waar het optreedt in uw omgeving. De volgende typen logboeken ziet u een query om de informatie te krijgen die u wilt maken.


- **ContainerImageInventory** – Gebruik dit type wanneer u probeert om gegevens geordend op de installatiekopie te zoeken en om informatie over de afbeelding zoals installatiekopie-id's of grootten weer te geven.
- **ContainerInventory** – Gebruik dit type wanneer u informatie over de containerlocatie wilt, wat hun namen zijn en wat installatiekopieën deze nu worden uitgevoerd.
- **ContainerLog** – Gebruik dit type als u wilt zoeken naar specifieke informatie in het foutenlogboek en vermeldingen.
- **ContainerNodeInventory_CL** Gebruik dit type als u de informatie over het hostknooppunt wilt/waar containers zijn die zich bevindt. Het biedt u Docker-versie, orchestration-type, opslag en netwerkgegevens.
- **ContainerProcess_CL** gebruik van dit type om snel het proces dat wordt uitgevoerd in de container.
- **ContainerServiceLog** – Gebruik dit type wanneer u probeert te auditgegevens vinden voor de Docker-daemon, zoals starten, stoppen, verwijderen of de pull-opdrachten.
- **KubeEvents_CL** gebruik van dit type om het Kubernetes-gebeurtenissen te bekijken.
- **KubePodInventory_CL** gebruik van dit type als u wilt meer informatie over de gegevens van de hiërarchie cluster.


### <a name="to-search-logs-for-container-data"></a>Om te zoeken naar Logboeken voor containergegevens
* Kies een afbeelding waarvan u weet dat onlangs is mislukt en de foutenlogboeken zoeken. Beginnen met zoeken naar een containernaam die wordt uitgevoerd die installatiekopie met een **ContainerInventory** zoeken. Zoek bijvoorbeeld naar voor `ContainerInventory | where Image == "ubuntu" and ContainerState == "Failed"`  
    ![Zoeken naar Ubuntu-containers](./media/log-analytics-containers/search-ubuntu.png)

  De naam van de container naast **naam**, en zoek naar deze logboeken. In dit voorbeeld is het `ContainerLog | where Name == "cranky_stonebreaker"`.

**Informatie over prestaties weergeven**

Wanneer u begint om query's samen te stellen, kunt u om te zien wat er mogelijk eerst. Bijvoorbeeld, als u wilt zien van alle prestatiegegevens, probeert u een uitgebreide query door te typen van de volgende query.

```
Perf
```

![prestaties van containers](./media/log-analytics-containers/containers-perf01.png)

U kunt het bereik van de prestatiegegevens die u voor een specifieke container ziet door de naam van het aan de rechterkant van de query te typen.

```
Perf <containerName>
```

De lijst met metrische gegevens voor prestaties die worden verzameld voor een afzonderlijke container wordt uitgelegd.

![prestaties van containers](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Voorbeeld van de logboekbestanden zoekquery 's
Vaak is het handig om te maken van query's beginnen met een voorbeeld of twee en vervolgens wijzigen om aan te passen in uw omgeving. Als uitgangspunt, kunt u experimenteren met de **voorbeeldquery's** gebied waarmee u meer geavanceerde query's.

![Query's voor containers](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Opslaan van logboek zoekquery 's
Opgeslagen query's is een standaardfunctie in Log Analytics. Door deze zijn opgeslagen, hebt u die u hebt gevonden nuttig bij de hand hebt voor toekomstig gebruik.

Nadat u een query die u maakt, deze door te klikken op Opslaan **Favorieten** aan de bovenkant van de pagina voor zoeken in Logboeken. En vervolgens u gemakkelijk toegankelijk is vanuit de **mijn Dashboard** pagina.

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in logboeken](log-analytics-queries.md) om gedetailleerde container gegevensrecords weer te geven.
