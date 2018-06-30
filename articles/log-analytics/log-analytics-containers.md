---
title: Container bewaking oplossing in Azure Log Analytics | Microsoft Docs
description: De oplossing Container bewaking in Log Analytics kunt u weergeven en beheren van uw Docker- en Windows container hosts op één locatie.
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
ms.date: 04/26/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 584e7a211cde83d7785c7fa0962c004af2b76968
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128908"
---
# <a name="container-monitoring-solution-in-log-analytics"></a>Container bewaking oplossing in Log Analytics

![Containers symbool](./media/log-analytics-containers/containers-symbol.png)

Dit artikel wordt beschreven hoe u kunt instellen en gebruiken van de oplossing Container bewaking in Log Analytics, waarmee u kunt weergeven en beheren van uw Docker- en Windows container hosts op één locatie. Docker is een software-virtualisatie-systeem gebruikt voor het maken van de containers die software-implementatie op hun IT-infrastructuur te automatiseren.

De oplossing laat zien welke containers worden uitgevoerd, welke afbeelding container ze uitvoert en waar containers worden uitgevoerd. U kunt gedetailleerde controle-informatie met opdrachten gebruikt met containers weergeven. En u containers weer te geven en gecentraliseerd logboeken zoeken zonder te bekijken op afstand Windows- of Docker-hosts kunt oplossen. U vindt de containers die mogelijk veel ruis veroorzaken en verbruiken overtollige bronnen op een host. En u kunt gecentraliseerde CPU, geheugen, opslag en gebruiks- en netwerkgegevens voor containers weergeven. Op computers waarop Windows wordt uitgevoerd, kunt u centraliseren en vergelijken van Logboeken van Windows Server, Hyper-V en Docker-containers. De oplossing ondersteunt de volgende container orchestrators:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift

Als u geïnteresseerd bent in controleren van de prestaties van uw werkbelastingen geïmplementeerd op Kubernetes omgevingen gehoste op AKS (Azure Container Service), Zie [Monitor Azure Container Service](../monitoring/monitoring-container-health.md).  De oplossing voor het bewaken van de Container bevat geen ondersteuning voor het bewaken van dit platform.  

Het volgende diagram toont de relaties tussen verschillende container hosts en -agents met logboekanalyse.

![Diagram van de containers](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements-and-supported-platforms"></a>Systeemvereisten en ondersteunde platforms

Voordat u begint, lees de volgende informatie om te controleren of u voldoet aan de vereisten.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Container bewakingsoplossing ondersteuning voor Docker-Orchestrator en OS-platform
De volgende tabel licht de Docker-orchestration en het besturingssysteem ondersteuning van container-inventaris, prestaties en logboeken met logboekanalyse bewaking.   

| | ACS | Linux | Windows | Container<br>Inventaris | Installatiekopie<br>Inventaris | Knooppunt<br>Inventaris | Container<br>Prestaties | Container<br>Gebeurtenis | Gebeurtenis<br>Logboek | Container<br>Logboek |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Kubernetes | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mesosphere<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Swarm | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Service<br>Fabric | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Red Hat openen<br>Shift | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(zelfstandig) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux-server<br>(zelfstandig) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |


### <a name="docker-versions-supported-on-linux"></a>Docker-versies die worden ondersteund op Linux

- Docker 1.11-1.13
- Docker CE en EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>x64 Linux-distributies als container hosts ondersteund

- Ubuntu 14.04 TNS en 16.04 TNS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE 42,2 LEAP
- CentOS 7.2 en 7.3
- SLES 12
- RHEL 7.2 en 7.3
- Red Hat OpenShift Container Platform (OCP) 3.4 en 3.5
- ACS Mesosphere DC/OS 1.7.3-1.8.8
- ACS Kubernetes 1.4.5-1.6
    - Kubernetes gebeurtenissen, Kubernetes inventaris en processen van de container worden alleen ondersteund met versie 1.4.1-45 en hoger van de OMS-Agent voor Linux
- ACS Docker Swarm

### <a name="supported-windows-operating-system"></a>Ondersteunde Windows-besturingssysteem

- Windows Server 2016
- Speciale Windows 10-editie (Professional of Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Docker-versies worden ondersteund in Windows

- Docker 1.12 en 1.13
- Docker 17.03.0 en hoger

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om de oplossing te installeren en configureren.

1. De oplossing Container bewaking toevoegen aan uw werkruimte voor logboekanalyse van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).

2. Installeren en gebruiken van Docker met een OMS-agent. Op basis van het besturingssysteem en de Docker-orchestrator, kunt u de volgende methoden voor het configureren van de agent.
  - Voor zelfstandige hosts:
    - Installeren op ondersteunde Linux-besturingssystemen en Docker uitvoeren en vervolgens installeren en configureren de [OMS-Agent voor Linux](log-analytics-agent-linux.md).  
    - Op virtuele CoreOS, kunt u de OMS-Agent niet uitvoeren voor Linux. U kunt in plaats daarvan een beperkte versie van de OMS-Agent uitvoeren voor Linux. Bekijk [Linux container hosts waaronder virtuele CoreOS](#for-all-linux-container-hosts-including-coreos) of [Azure Government Linux container hosts waaronder virtuele CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) als u met containers in Azure Government Cloud werkt.
    - Op Windows Server 2016 en Windows 10, de Docker-Engine en de client installeren en vervolgens verbinding maken met een agent voor het verzamelen van gegevens en verzenden naar Log Analytics. Bekijk [installeren en configureren van Windows-container hosts](#install-and-configure-windows-container-hosts) hebt u een Windows-omgeving.
  - Voor meerdere host-orchestration Docker:
    - Als u een Red Hat OpenShift-omgeving hebt, raadpleegt u [configureren van een OMS-agent voor Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).
    - Als u een Kubernetes-cluster met behulp van de Azure Container Service hebt:
       - Bekijk [configureren een OMS-Linux-agent voor Kubernetes](#configure-an-oms-linux-agent-for-kubernetes).
       - Bekijk [configureren van een Windows OMS-agent voor Kubernetes](#configure-an-oms-windows-agent-for-kubernetes).
       - Bekijk [Helm gebruiken voor het implementeren van OMS-Agent op Linux Kubernetes](#use-helm-to-deploy-oms-agent-on-linux-kubernetes).
    - Als u een Azure Container Service DC/OS-cluster hebt, meer informatie op [bewaken van een Azure Container Service DC/OS-cluster met Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).
    - Als u een omgeving met de modus Docker Swarm hebt, meer informatie op [configureren van een OMS-agent voor Docker Swarm](#configure-an-oms-agent-for-docker-swarm).
    - Als u een Service Fabric-cluster hebt, meer informatie op [bewaken containers met OMS Log Analytics](../service-fabric/service-fabric-diagnostics-oms-containers.md).

Controleer de [Docker-Engine op Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) artikel voor meer informatie over het installeren en configureren van uw Docker-Engines op computers waarop Windows wordt uitgevoerd.

> [!IMPORTANT]
> Docker moet worden uitgevoerd **voordat** u installeert de [OMS-Agent voor Linux](log-analytics-agent-linux.md) op de container-hosts. Als u de agent al hebt geïnstalleerd voordat u Docker installeert, moet u de OMS-Agent voor Linux opnieuw te installeren. Zie voor meer informatie over Docker de [Docker-website](https://www.docker.com).


### <a name="install-and-configure-linux-container-hosts"></a>Installeren en configureren van Linux-container hosts

Nadat u Docker hebt geïnstalleerd, gebruikt u de volgende instellingen voor de containerhost voor het configureren van de agent voor gebruik met Docker. U moet eerst uw Log Analytics-werkruimte-ID en sleutel, kunt u vinden in de Azure portal. Klik in de werkruimte op **Quick Start** > **Computers** om weer te geven uw **werkruimte-ID** en **primaire sleutel**.  Kopieer en plak beide in uw favoriete editor.

**Voor alle Linux-container hosts, virtuele CoreOS behalve:**

- Zie voor meer informatie en stapsgewijze instructies voor het installeren van de OMS-Agent voor Linux [uw Linux-Computers verbinden met Log Analytics](log-analytics-concept-hybrid.md).

**Voor alle Linux-container hosts virtuele CoreOS waaronder:**

Start de container die u wilt bewaken. Wijzigen en gebruik het volgende voorbeeld:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

**Voor alle Azure Government Linux container hosts virtuele CoreOS waaronder:**

Start de container die u wilt bewaken. Wijzigen en gebruik het volgende voorbeeld:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

**Overschakelen van het gebruik van een geïnstalleerde Linux-agent op een in een container**

Als u eerder gebruikt de agent rechtstreeks is geïnstalleerd en u wilt gebruiken in plaats daarvan een agent wordt uitgevoerd in een container, moet u eerst de OMS-Agent verwijderen voor Linux. Zie [de OMS-Agent verwijderen voor Linux](log-analytics-agent-linux.md) om te begrijpen hoe de agent met succes te verwijderen.  

#### <a name="configure-an-oms-agent-for-docker-swarm"></a>Configureer een OMS-Agent voor Docker Swarm

U kunt de OMS-Agent uitvoeren als een wereldwijde service op het Docker Swarm. Gebruik de volgende informatie om een OMS-Agent-service te maken. U moet uw Log Analytics-werkruimte-ID en de primaire sleutel opgeven.

- Voer het volgende op het hoofdknooppunt.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

##### <a name="secure-secrets-for-docker-swarm"></a>Beveiligen van geheimen voor Docker Swarm

Gebruik de volgende informatie voor het maken van uw geheime informatie voor Docker Swarm, zodra het geheim voor werkruimte-ID en de primaire sleutel is gemaakt.

1. Voer het volgende op het hoofdknooppunt.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Controleer of de geheimen op de juiste wijze zijn gemaakt.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Voer de volgende opdracht om te koppelen van de geheimen met de beperkte OMS-Agent.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="configure-an-oms-agent-for-red-hat-openshift"></a>Een OMS-Agent voor Red Hat OpenShift configureren
Er zijn drie manieren om toe te voegen de OMS-Agent voor Red Hat OpenShift naar begint met het verzamelen van bewakingsgegevens van de container.

* [Installeer de OMS-Agent voor Linux](log-analytics-agent-linux.md) rechtstreeks op elk knooppunt OpenShift  
* [Log Analytics VM-extensie inschakelen](log-analytics-azure-vm-extension.md) op elk knooppunt OpenShift is die zich in Azure  
* De OMS-Agent installeren als een OpenShift daemon-set  

In deze sectie uitgelegd hoe u de OMS-Agent installeren als een OpenShift daemon-set.  

1. Meld u bij het hoofdknooppunt OpenShift en kopieer het bestand yaml [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) vanuit GitHub op uw hoofdknooppunt en wijzig de waarde die met uw Log Analytics-werkruimte-ID en met uw primaire sleutel.
2. Voer de volgende opdrachten voor het maken van een project voor logboekanalyse en stel de gebruikersaccount.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Als u wilt implementeren de daemon-set, voert u de volgende:

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

Als u gebruiken van geheimen wilt voor het beveiligen van uw Log Analytics-werkruimte-ID en de primaire sleutel bij gebruik van de OMS-Agent daemon-set yaml-bestand, moet u de volgende stappen uitvoeren.

1. Meld u bij het hoofdknooppunt OpenShift en kopieer het bestand yaml [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) en geheim script genereren [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) vanuit GitHub.  Dit script genereert de geheimen yaml-bestand voor Log Analytics-werkruimte-ID en de primaire sleutel voor het beveiligen van uw informatie secrete.  
2. Voer de volgende opdrachten voor het maken van een project voor logboekanalyse en stel de gebruikersaccount. Het script genereren geheim vraagt om uw werkruimte-ID van Log Analytics <WSID> en primaire sleutel <KEY> en heeft voltooid, wordt het bestand ocp-secret.yaml gemaakt.  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Implementeer het geheime bestand door het uitvoeren van het volgende:

    `oc create -f ocp-secret.yaml`

5. Implementatie controleren door het uitvoeren van het volgende:

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

6. De OMS-Agent daemon-set yaml-bestand door het uitvoeren van de volgende implementeren:

    `oc create -f ocp-ds-omsagent.yaml`  

7. Implementatie controleren door het uitvoeren van het volgende:

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

#### <a name="configure-an-oms-linux-agent-for-kubernetes"></a>Een OMS-Linux-agent voor Kubernetes configureren

Voor Kubernetes gebruikt u een script om de geheimen yaml-bestand voor uw werkruimte-ID en de primaire sleutel voor het installeren van de OMS-Agent voor Linux te genereren. Op de [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) pagina, er zijn bestanden die u met of zonder uw geheime informatie gebruiken kunt.

- De OMS-Agent standaard voor Linux DaemonSet heeft geen geheime informatie (omsagent.yaml)
- De OMS-Agent voor Linux DaemonSet yaml-bestand gebruikt geheime informatie (omsagent ds secrets.yaml) met geheime generatie scripts om de geheimen yaml (omsagentsecret.yaml)-bestand te genereren.

U kunt omsagent DaemonSets maken met of zonder geheimen.

**Standaard OMSagent DaemonSet yaml-bestand zonder geheimen**

- Voor het standaard OMS-Agent DaemonSet yaml-bestand, vervangt u de `<WSID>` en `<KEY>` naar uw WSID en -sleutel. Kopieer het bestand naar uw hoofdknooppunt en voer het volgende:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

**Standaard OMSagent DaemonSet yaml-bestand met geheimen**

1. Voor het gebruik van OMS-Agent DaemonSet met behulp van de geheime gegevens van de geheimen eerst te maken.
    1. Kopieer het script en de geheime sjabloonbestand en controleer of dat ze zich op dezelfde map.
        - Geheim script - geheim gen.sh genereren
        - geheime sjabloon - geheim template.yaml
    2. Voer het script, zoals in het volgende voorbeeld. Het script vraagt om de Log Analytics-werkruimte-ID en de primaire sleutel en nadat u deze invoert, maakt het script een geheime yaml-bestand zodat u deze kunt uitvoeren.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Geheimen schil maken door het volgende:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Om te controleren, voert u de volgende:

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

    5. Maken van uw omsagent daemon-set door te voeren ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. Controleren of de DaemonSet OMS-Agent wordt uitgevoerd, vergelijkbaar met het volgende:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Voor Kubernetes, moet u een script gebruiken de geheimen yaml-bestand voor de werkruimte-ID en de primaire sleutel genereren voor de OMS-Agent voor Linux. Gebruik de volgende voorbeeldinformatie met de [omsagent yaml bestand](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) uw geheime gegevens kunt beveiligen.

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

#### <a name="configure-an-oms-windows-agent-for-kubernetes"></a>Een Windows OMS-agent voor Kubernetes configureren
Voor Windows Kubernetes gebruikt u een script om de geheimen yaml-bestand voor uw werkruimte-ID en de primaire sleutel voor het installeren van de OMS-Agent te genereren. Op de [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes/windows) pagina, er zijn bestanden die u met uw geheime informatie gebruiken kunt.  U moet de OMS-Agent voor de hoofd- en agent knooppunten afzonderlijk installeren.  

1. Gebruik van OMS-Agent DaemonSet met geheime informatie op de Master-knooppunt, aanmelden en maakt u eerst de geheimen.
    1. Kopieer het script en de geheime sjabloonbestand en controleer of dat ze zich op dezelfde map.
        - Geheim script - geheim gen.sh genereren
        - geheime sjabloon - geheim template.yaml

    2. Voer het script, zoals in het volgende voorbeeld. Het script vraagt om de OMS-werkruimte-ID en de primaire sleutel en nadat u deze invoert, maakt het script een geheime yaml-bestand zodat u deze kunt uitvoeren.   

        ```
        #> sudo bash ./secret-gen.sh
        ```
    3. Maken van uw omsagent daemon-set door te voeren ``` kubectl create -f omsagentsecret.yaml ```
    4. Als u wilt controleren, voert u de volgende:

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

    5. Maken van uw omsagent daemon-set door te voeren ```kubectl create -f ws-omsagent-de-secrets.yaml```

2. Controleren of de DaemonSet OMS-Agent wordt uitgevoerd, vergelijkbaar met het volgende:

    ```
    root@ubuntu16-13db:~# kubectl get deployment omsagent
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   1         1         <none>          1h
    ```

3. Volg de stappen in de sectie voor de installatie van de agent op het Werkrolknooppunt die Windows uitvoert, [installeren en configureren van Windows-container hosts](#install-and-configure-windows-container-hosts).

#### <a name="use-helm-to-deploy-oms-agent-on-linux-kubernetes"></a>Gebruik Helm OMS-Agent op Linux Kubernetes implementeren
Als u wilt gebruiken helm OMS-Agent op uw omgeving Linux Kubernetes implementeren, moet u de volgende stappen uitvoeren.

1. Maken van uw omsagent daemon-set door te voeren ```helm install --name omsagent --set omsagent.secret.wsid=<WSID>,omsagent.secret.key=<KEY> stable/msoms```
2. De resultaten ziet er ongeveer als volgt:

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
    NAME            TYPE    DATA  AGE
    omsagent-msoms  Opaque  3     17m
 
    ==> v1beta1/DaemonSet
    NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE-SELECTOR  AGE
    omsagent-msoms  3        3        3      3           3          <none>         17m
    ```
Voor meer informatie raadpleegt u [Container oplossing Helm grafiek](https://aka.ms/omscontainerhelm).

### <a name="install-and-configure-windows-container-hosts"></a>Installeren en configureren van Windows-container hosts

Gebruik de informatie in de sectie installeren en configureren van Windows-container-hosts.

#### <a name="preparation-before-installing-windows-agents"></a>Voorbereiding voor de installatie van Windows-agents

Voordat u agents op computers met Windows installeren, moet u de Docker-service configureren. De configuratie kan de Windows-agent of de uitbreiding van de virtuele machine Log Analytics de Docker-TCP-socket gebruiken zodat de agents toegang heeft tot de Docker-daemon op afstand en vastleggen van gegevens voor de bewaking.

##### <a name="to-start-docker-and-verify-its-configuration"></a>Docker starten en controleer of de configuratie

Er zijn stappen die nodig zijn voor het instellen van TCP benoemde pijp voor Windows Server:

1. Schakel pipe TCP en named pipe in Windows PowerShell.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Docker configureren met het configuratiebestand voor de TCP-pipe en benoemde pijp. Het configuratiebestand bevindt zich op C:\ProgramData\docker\config\daemon.json.

    In het bestand daemon.json moet u het volgende:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Zie voor meer informatie over de Docker-daemon-configuratie gebruikt met Windows Containers [Docker-Engine op Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


#### <a name="install-windows-agents"></a>Windows-agents installeren

Schakel bewaking voor het Windows- en Hyper-V-container door Microsoft Monitoring Agent (MMA) te installeren op Windows-computers die container hosts zijn. Voor computers waarop Windows wordt uitgevoerd in uw on-premises-omgeving, Zie [verbinding maken met Windows-computers met logboekanalyse](log-analytics-windows-agent.md). Voor virtuele machines worden uitgevoerd in Azure, verbind ze met logboekanalyse met behulp van de [extensie van virtuele machine](log-analytics-azure-vm-extension.md).

U kunt Windows-containers die zijn uitgevoerd op de Service Fabric bewaken. Echter alleen [virtuele machines worden uitgevoerd in Azure](log-analytics-azure-vm-extension.md) en [computers met Windows in uw on-premises omgeving](log-analytics-windows-agent.md) momenteel worden ondersteund voor Service Fabric.

U kunt controleren of de bewaking van de Container-oplossing goed is ingesteld voor Windows. Als u wilt controleren of het management pack downloaden goed is, zoekt u naar *ContainerManagement.xxx*. De bestanden zich in de map C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management servicepacks.


## <a name="solution-components"></a>Oplossingsonderdelen

Vanuit de OMS-portal gaat u naar de *galerie met oplossingen* en voeg de **Container bewaking oplossing**. Als u van Windows-agents gebruikmaakt, klikt u vervolgens de volgende management pack is geïnstalleerd op elke computer met een agent wanneer u deze oplossing toevoegt. Er is geen configuratie of het onderhoud is vereist voor het management pack.

- *ContainerManagement.xxx* in C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management servicepacks zijn geïnstalleerd

## <a name="container-data-collection-details"></a>De verzameling Gegevensdetails container
De oplossing Container bewaking verzamelt verschillende metrische gegevens en logboekbestanden prestatiegegevens van container-hosts en containers met agents die u inschakelt.

Gegevens worden elke drie minuten worden verzameld door de volgende typen van de agent.

- [OMS-Agent voor Linux](log-analytics-linux-agents.md)
- [Windows-agent](log-analytics-windows-agent.md)
- [Log Analytics VM-extensie](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a>Containerrecords

De volgende tabel ziet u voorbeelden van records die door de bewaking van de Container-oplossing en de gegevenstypen die worden weergegeven in zoekresultaten logboek worden verzameld.

| Gegevenstype | Het gegevenstype in logboek zoeken | Velden |
| --- | --- | --- |
| Prestaties voor hosts en -containers | `Perf` | Computer, objectnaam, CounterName &#40;percentage processortijd, de schijf leest MB, schijf schrijft MB geheugen gebruik MB netwerk ontvangen Bytes, netwerk verzenden Bytes Processor gebruik seconde, netwerk&#41;, tegenwaarde, TimeGenerated, itempad, SourceSystem |
| Inventarisatie van de container | `ContainerInventory` | TimeGenerated, Computer, containernaam ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, opdracht, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Container installatiekopie inventarisatie | `ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, uitvoering is onderbroken, gestopt, is mislukt, SourceSystem, ImageID, TotalContainer |
| Container-logboek | `ContainerLog` | TimeGenerated, Computer, afbeeldings-ID, containernaam LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Container service-logboek | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, opdracht, SourceSystem, ContainerID |
| Container knooppunt inventarisatie | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Kubernetes-inventarisatie | `KubePodInventory_CL` | TimeGenerated, Computer, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem |
| Container-proces | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Kubernetes gebeurtenissen | `KubeEvents_CL` | TimeGenerated, Computer, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, bericht |

Labels toegevoegd aan *PodLabel* gegevenstypen zijn uw eigen aangepaste etiketten. De toegevoegde PodLabel labels weergegeven in de tabel zijn voorbeelden. Dus `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` wordt verschillen in uw omgeving gegevensset en algemeen fungeren als `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>Containers bewaken
Nadat u de ingeschakeld in de portal Log Analytics-oplossing hebt de **Containers** tegel samenvattingsinformatie over de container-hosts en de containers die wordt uitgevoerd op hosts.


![Containers tegel](./media/log-analytics-containers/containers-title.png)

De tegel geeft een overzicht van hoeveel containers die u hebt in de omgeving en of ze klaar is mislukt, uitgevoerd of gestopt.

### <a name="using-the-containers-dashboard"></a>Met behulp van de Containers-dashboard
Klik op de **Containers** tegel. Daar ziet u weergaven onderverdeeld op basis van:

- **Gebeurtenissen van de container** -toont de status van de container en computers met mislukte containers.
- **Logboeken van de container** -bevat een grafiek met logboekbestanden container gegenereerd over tijd en een lijst met computers met het hoogste aantal logboekbestanden.
- **Kubernetes gebeurtenissen** -bevat een grafiek met Kubernetes-gebeurtenissen die gedurende een bepaalde tijd een lijst van de redenen waarom gehele product gegenereerd voor de gebeurtenissen worden gegenereerd. *Deze gegevensset wordt alleen gebruikt in omgevingen met Linux.*
- **Kubernetes Namespace inventaris** - toont het aantal naamruimten en gehele product en toont hun hiërarchie. *Deze gegevensset wordt alleen gebruikt in omgevingen met Linux.*
- **Container knooppunt inventaris** -toont het aantal orchestration typen op de container knooppunten/hosts gebruikt. De computer knooppunten/hosts staan ook door het aantal containers. *Deze gegevensset wordt alleen gebruikt in omgevingen met Linux.*
- **Container installatiekopieën inventaris** -toont het totale aantal container afbeeldingen die worden gebruikt en het aantal afbeeldingstypen. Het aantal installatiekopieën worden ook vermeld door de afbeeldingscode.
- **Status van de containers** -toont het totale aantal container knooppunten/host-computers met actieve containers. Computers worden ook vermeld door het aantal hosts uitgevoerd.
- **Container proces** -ziet u een lijndiagram van container processen uitgevoerd gedurende een periode. Containers worden ook vermeld door te voeren opdracht/proces binnen containers. *Deze gegevensset wordt alleen gebruikt in omgevingen met Linux.*
- **Container CPU-prestaties** -een lijndiagram van het gemiddelde CPU-gebruik gedurende een bepaalde periode voor computer-knooppunten/hosts bevat. Ook ziet u de computer knooppunten/hosts op basis Gemiddeld CPU-gebruik.
- **Container geheugenprestaties** -ziet u een lijndiagram geheugengebruik gedurende een bepaalde periode. Een lijst met computergeheugen gebruik op basis van de exemplaarnaam.
- **Prestaties van de computer** -worden lijndiagrammen van het percentage van de CPU-prestaties na verloop van tijd percentage geheugengebruik gedurende een bepaalde tijd, megabytes aan vrije schijfruimte voor een periode. U kunt de muisaanwijzer op elke regel in een grafiek om meer details te bekijken.


Elk onderdeel van het dashboard is een visuele representatie van een zoekopdracht die wordt uitgevoerd op de verzamelde gegevens.

![Containers dashboard](./media/log-analytics-containers/containers-dash01.png)

![Containers dashboard](./media/log-analytics-containers/containers-dash02.png)

In de **Container Status** gebied, klikt u op het bovenste gedeelte zoals hieronder wordt weergegeven.

![Status van de containers](./media/log-analytics-containers/containers-status.png)

Logboek zoeken met de informatie over de status van de containers wordt geopend.

![Meld u zoeken naar containers](./media/log-analytics-containers/containers-log-search.png)

Hier kunt kunt u de zoekopdracht om te wijzigen om te zoeken naar de specifieke informatie die u geïnteresseerd bent in bewerken. Zie voor meer informatie over logboek zoekacties [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-searches.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Problemen oplossen met een mislukte container te zoeken

Log Analytics markeert een container als **mislukt** als is afgesloten met een afsluitcode dan nul. U kunt een overzicht van de fouten en fouten in de omgeving in de **Containers mislukt** gebied.

### <a name="to-find-failed-containers"></a>Mislukte containers vinden
1. Klik op de **Container Status** gebied.  
   ![status van de containers](./media/log-analytics-containers/containers-status.png)
2. Logboek zoekopdracht wordt geopend en wordt de status van uw containers, de volgende strekking weergegeven.  
   ![status van de containers](./media/log-analytics-containers/containers-log-search.png)
3. Klik vervolgens op de toegevoegde waarde van mislukte containers naar aanvullende informatie weergeven. Vouw **meer** om weer te geven van de installatiekopie-ID.  
   ![mislukte containers](./media/log-analytics-containers/containers-state-failed.png)  
4. Typ vervolgens het volgende in de zoekopdracht. `ContainerInventory <ImageID>` voor informatie over de installatiekopie zoals afbeeldingsgrootte en aantal gestopt en mislukte afbeeldingen.  
   ![mislukte containers](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Zoeken in Logboeken voor containergegevens
Wanneer u een specifieke fout oplossen bent, kunt u zien waar het optreedt in uw omgeving. De volgende typen van de logboekbestanden kunt u query's voor het retourneren van de informatie die dat u wilt maken.


- **ContainerImageInventory** – Gebruik dit type wanneer u probeert om gegevens die zijn onderverdeeld op basis van de installatiekopie te zoeken en om informatie over de afbeelding als id's of grootten van een installatiekopie weer te geven.
- **ContainerInventory** – Gebruik dit type wanneer u informatie over de containerlocatie wilt, wat hun namen zijn en wat ze uitvoert installatiekopieën.
- **ContainerLog** – Gebruik dit type als u wilt zoeken naar specifieke informatie in het foutenlogboek en vermeldingen.
- **ContainerNodeInventory_CL** Gebruik dit type als u de informatie over het hostknooppunt wilt/containers wonen waar. Dit biedt u Docker-versie, orchestration-type, opslag en informatie over het netwerk.
- **ContainerProcess_CL** gebruik van dit type voor een snel overzicht van het proces dat wordt uitgevoerd in de container.
- **ContainerServiceLog** – Gebruik dit type wanneer u probeert te audit Audittrailinformatie vinden voor de Docker-daemon, zoals het starten, stoppen, verwijderen of pull-opdrachten.
- **KubeEvents_CL** gebruik van dit type om de Kubernetes-gebeurtenissen te bekijken.
- **KubePodInventory_CL** gebruikt dit type als u wilt weten over de clustergegevens voor de hiërarchie.


### <a name="to-search-logs-for-container-data"></a>Logboeken voor de containergegevens zoeken
* Kies een installatiekopie die u kent onlangs is mislukt en de foutenlogboeken voor het vinden. Starten door te zoeken naar een containernaam die wordt uitgevoerd die afbeelding met een **ContainerInventory** zoeken. Bijvoorbeeld zoeken naar `ContainerInventory | where Image == "ubuntu" and ContainerState == "Failed"`  
    ![Zoeken naar Ubuntu-containers](./media/log-analytics-containers/search-ubuntu.png)

  De naam van de container naast **naam**, en zoek naar deze logboeken. In dit voorbeeld is het `ContainerLog | where Name == "cranky_stonebreaker"`.

**Informatie over prestaties weergeven**

Wanneer u bent u query's maken, kunt u zien wat er mogelijk eerst. Probeer bijvoorbeeld een brede query om te zien alle prestatiegegevens, door de volgende query in te voeren.

```
Perf
```

![prestaties van de containers](./media/log-analytics-containers/containers-perf01.png)

U kunt het bereik van de prestatiegegevens die u, naar een specifieke container ziet door de naam van het aan de rechterkant van de query te typen.

```
Perf <containerName>
```

Dat geeft de lijst weer van maatstaven voor prestaties die worden verzameld voor een afzonderlijke-container.

![prestaties van de containers](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Voorbeeld logboek zoekquery 's
Vaak is het nuttig voor het bouwen van query's beginnen met een voorbeeld of twee en deze aanpassen aan uw omgeving vervolgens wijzigen. Als uitgangspunt, kunt u experimenteren met het **voorbeeldquery's** gebied waarmee u kunt meer geavanceerde query's maken.

![Containers query 's](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Zoekopdrachten opslaan logboek
Query's opslaan, is een standaardfunctie in logboekanalyse. Door deze te slaan, hebt u die u hebt gevonden nuttig bij de hand hebt voor toekomstig gebruik.

Nadat u een query die nuttig maakt, deze door te klikken op Opslaan **Favorieten** boven aan de pagina logboek zoeken. Vervolgens u gemakkelijk toegankelijk is vanuit de **mijn Dashboard** pagina.

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde gegevens voor containerrecords weer te geven.
