# <a name="make-a-remote-connection-to-a-kubernetes-dcos-or-docker-swarm-cluster"></a>Een externe verbinding maken met een Kubernetes-, DC/OS- of Docker Swarm-cluster
Nadat u een Azure Container Service-cluster hebt gemaakt, moet u het cluster verbinden om workloads te kunnen implementeren en beheren. In dit artikel wordt beschreven hoe u vanaf een externe computer verbinding maakt met de hoofd-VM van het cluster. 

De Kubernetes-, DC/OS- en Docker Swarm-clusters stellen lokale HTTP-eindpunten beschikbaar. Bij Kubernetes wordt dit eindpunt veilig beschikbaar gesteld op internet en kunt u het rechtstreeks openen door op een machine met een internetverbinding het opdrachtregelprogramma `kubectl` uit te voeren. 

Voor DC/OS en Docker Swarm wordt u aangeraden een SSH-tunnel (Secure Shell) te maken van uw lokale computer naar het beheersysteem van het cluster. Wanneer de tunnel is ingesteld, kunt u opdrachten uitvoeren die gebruikmaken van de HTTP-eindpunten en kunt u de webinterface van de orchestrator (indien beschikbaar) vanuit uw lokale systeem bekijken. 

## <a name="prerequisites"></a>Vereisten

* Een Kubernetes-, DC/OS- of Docker Swarm-cluster, [geïmplementeerd in Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).
* SSH RSA-bestand met persoonlijke sleutel die overeenkomt met de openbare sleutel die tijdens de implementatie is toegevoegd aan het cluster. Met deze opdrachten wordt ervan uitgegaan dat de persoonlijke SSH-sleutel zich bevindt in `$HOME/.ssh/id_rsa` op uw computer. Zie deze instructies voor [macOS en Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) of [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) voor meer informatie. Als de SSH-verbinding niet werkt, moet u mogelijk [uw SSH-sleutels opnieuw instellen](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

## <a name="connect-to-a-kubernetes-cluster"></a>Verbinding maken met een Kubernetes-cluster

Volg deze stappen om `kubectl` op uw computer te installeren en te configureren.

> [!NOTE] 
> Op Linux of macOS moet u de opdrachten in deze sectie mogelijk uitvoeren met `sudo`.
> 

### <a name="install-kubectl"></a>Kubectl installeren
Eén manier om dit hulpprogramma te installeren, is met de Azure CLI 2.0-opdracht `az acs kubernetes install-cli`. Als u deze opdracht wilt uitvoeren, moet de meest recente versie van Azure CLI 2.0 zijn [geïnstalleerd](/cli/azure/install-az-cli2) en moet u zijn aangemeld bij een Azure-account (`az login`).

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

U kunt de nieuwste `kubectl`-client ook rechtstreeks downloaden vanaf de [Kubernetes-releasepagina](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md). Zie [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/) (kubectl installeren en instellen) voor meer informatie.

### <a name="download-cluster-credentials"></a>Clusterreferenties downloaden
Nadat `kubectl` is geïnstalleerd, kopieert u de clusterreferenties naar uw machine. Eén manier om de referenties te verkrijgen, is met de opdracht `az acs kubernetes get-credentials`. Geef de naam van de resourcegroep en de naam van de containerserviceresource als volgt door:

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Hiermee worden de clusterreferenties gedownload in `$HOME/.kube/config`, waar `kubectl` ze verwacht.

U kunt `scp` ook gebruiken om het bestand veilig te kopiëren van `$HOME/.kube/config` op de hoofd-VM naar uw lokale machine. Bijvoorbeeld:

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Als u met Windows werkt, kunt u gebruikmaken van Bash op Ubuntu in Windows, de PuTTy-client voor het veilig kopiëren van bestanden of een vergelijkbaar hulpprogramma.

### <a name="use-kubectl"></a>Kubectl gebruiken

Zodra `kubectl` is geconfigureerd, test u de verbinding door de knooppunten in uw cluster weer te geven:

```bash
kubectl get nodes
```

U kunt andere `kubectl`-opdrachten proberen. Zo zou u bijvoorbeeld het Kubernetes Dashboard kunnen weergeven. Daartoe voert u eerst een proxy uit naar de API-server van Kubernetes:

```bash
kubectl proxy
```

De gebruikersinterface van Kubernetes is nu beschikbaar via `http://localhost:8001/ui`.

Raadpleeg de [Snelstartgids van Kubernetes](http://kubernetes.io/docs/user-guide/quick-start/) voor meer informatie.

## <a name="connect-to-a-dcos-or-swarm-cluster"></a>Verbinding maken met een DC/OS- of Swarm-cluster

Als u de CD/OS- en Docker Swarm-clusters wilt gebruiken die door Azure Container Service zijn geïmplementeerd, volgt u deze instructies om een SSH-tunnel (Secure Shell) vanuit uw lokale Linux-, macOS- of Windows-systeem te maken. 

> [!NOTE]
> Deze instructies zijn gericht op tunneling van TCP-verkeer via SSH. U kunt ook een interactieve SSH-sessie starten met een van de interne clusterbeheersystemen, maar dit wordt niet aanbevolen. Wanneer er rechtstreeks in een intern systeem wordt gewerkt, ontstaat het risico dat er onbedoeld configuratiewijzigingen worden doorgevoerd.  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a>Een SSH-tunnel maken in Linux of macOS
Het eerste wat u doet wanneer u een SSH-tunnel in Linux of macOS maakt, is het lokaliseren van de openbare DNS-naam van de masters met gelijke taakverdeling. Volg deze stappen:


1. Blader in [Azure Portal](https://portal.azure.com) naar de resourcegroep die uw containerservicecluster bevat. Vouw de resourcegroep uit zodat elke resource wordt weergegeven. 

2. Klik op de resource van de **Containerservice** en vervolgens op **Overzicht**. Onder **Essentials** wordt de **Hoofd-FQDN** van het cluster weergegeven. Bewaar deze naam voor later gebruik. 

    ![Openbare DNS-naam](./media/container-service-connect/pubdns.png)

    U kunt ook de opdracht `az acs show` voor de containerservice uitvoeren. Zoek naar de eigenschap **Master Profile:fqdn** in de uitvoer van de opdracht.

3. Open nu een shell en voer de opdracht `ssh` uit door de volgende waarden op te geven: 

    **LOCAL_PORT** is de TCP-poort aan de servicezijde van de tunnel waarmee verbinding moet worden gemaakt. Voor Swarm stelt u deze in op 2375. Voor DC/OS stelt u deze in op 80. 
    **REMOTE_PORT** is de naam van het eindpunt dat u beschikbaar wilt maken. Voor Swarm gebruikt u poort 2375. Gebruik poort 80 voor DC/OS.  
    **USERNAME** de naam is van de gebruiker die is opgegeven tijdens de implementatie van het cluster.  
    **DNSPREFIX** het DNS-voorvoegsel is dat is opgegeven tijdens de implementatie van het cluster.  
    **REGION** de regio is waarin uw resourcegroep zich bevindt.  
    **PATH_TO_PRIVATE_KEY** [OPTIONEEL] is het pad naar de persoonlijke sleutel die overeenkomt met de openbare sleutel die u hebt opgegeven bij het maken van het cluster. Gebruik deze optie met de vlag `-i`.

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > De poort voor de SSH-verbinding is 2200 en niet de standaardpoort 22. In een cluster met meerdere hoofd-VM's is dit de verbindingspoort naar de eerste hoofd-VM.
  > 

  De opdracht retourneert geen uitvoer.

Zie de voorbeelden voor DC/OS en Swarm in de volgende secties.    

### <a name="dcos-tunnel"></a>DC/OS-tunnel
Als u een tunnel voor DC/OS-eindpunten wilt openen, voert u een opdracht als deze uit:

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> Zorg ervoor dat u geen ander lokaal proces hebt dat een binding heeft met poort 80. U kunt indien nodig een andere lokale poort dan poort 80 opgeven, bijvoorbeeld poort 8080. Sommige web-UI-koppelingen werken echter niet wanneer u deze poort gebruikt.
>

Nu hebt u vanaf de lokale computer via de volgende URL's toegang tot de DC/OS-eindpunten (ervan uitgaande dat de lokale poort 80 is):

* DC/OS: `http://localhost:80/`
* Marathon: `http://localhost:80/marathon`
* Mesos: `http://localhost:80/mesos`

U kunt ook de REST API's voor elke toepassing via deze tunnel bereiken.

### <a name="swarm-tunnel"></a>Swarm-tunnel
Wanneer u een tunnel naar het Swarm-eindpunt wilt openen, voert u een opdracht als deze uit:

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> Zorg ervoor dat u geen ander lokaal proces hebt dat een binding heeft met poort 2375. Als u de Docker-daemon bijvoorbeeld lokaal uitvoert, wordt deze standaard ingesteld op het gebruik van poort 2375. U kunt indien nodig een andere lokale poort dan poort 2375 opgeven.
>

U hebt nu toegang tot het Docker Swarm-cluster met behulp van de Docker-opdrachtregelinterface (Docker CLI) op uw lokale systeem. Zie [Docker installeren](https://docs.docker.com/engine/installation/) voor installatie-instructies.

Stel uw DOCKER_HOST-omgevingsvariabele in op de lokale poort die u hebt geconfigureerd voor de tunnel. 

```bash
export DOCKER_HOST=:2375
```

Voer Docker-opdrachten uit die tunnelen naar het Docker Swarm-cluster. Bijvoorbeeld:

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Een SSH-tunnel maken in Windows
Er zijn meerdere opties voor het maken van SSH-tunnels in Windows. Als u Bash uitvoert in Ubuntu of Windows of een vergelijkbaar hulpprogramma, kunt u de instructies voor SSH-tunneling eerder in dit artikel voor macOS en Linux volgen. In deze sectie wordt beschreven hoe u PuTTY gebruikt om de tunnel te maken als een alternatief in Windows.

1. [Download PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) naar uw Windows-systeem.

2. Voer de toepassing uit.

3. Voer een hostnaam in die bestaat uit de naam van de cluserbeheerdergebruiker en de openbare DNS-naam van de eerste master in het cluster. De **hostnaam** ziet er ongeveer zo uit: `azureuser@PublicDNSName`. Voer 2200 in bij **Poort**.

    ![PuTTY-configuratie 1](./media/container-service-connect/putty1.png)

4. Selecteer **SSH > Verificatie**. Voeg een pad aan uw persoonlijke-sleutelbestand (.ppk-indeling) toe voor verificatie. U kunt een hulpprogramma als [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) gebruiken om dit bestand te genereren vanuit de SSH-sleutel waarmee het cluster is gemaakt.

    ![PuTTY-configuratie 2](./media/container-service-connect/putty2.png)

5. Selecteer **SSH > Tunnels** en configureer de volgende doorgestuurde poorten:

    * **Bronpoort:** gebruik 80 voor DC/OS of 2375 voor Swarm.
    * **Doelpoort:** gebruik localhost:80 voor DC/OS of localhost:2375 voor Swarm.

    Het volgende voorbeeld is geconfigureerd voor DC/OS, maar ziet er ongeveer hetzelfde uit voor Docker Swarm.

    > [!NOTE]
    > Poort 80 mag niet in gebruik zijn wanneer u deze tunnel maakt.
    > 

    ![PuTTY-configuratie 3](./media/container-service-connect/putty3.png)

6. Wanneer u klaar bent, klikt u op **Sessie > Opslaan** om de verbindingsconfiguratie op te slaan.

7. Klik op **Openen** als u verbinding wilt maken met de PuTTY-sessie. Wanneer u verbinding maakt, kunt u de poortconfiguratie in het PuTTY-gebeurtenislogboek zien.

    ![PuTTY-gebeurtenislogboek](./media/container-service-connect/putty4.png)

Nadat u de tunnel voor DC/OS hebt geconfigureerd, hebt u toegang tot het gerelateerde eindpunt op:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Nadat u de tunnel voor Docker Swarm hebt geconfigureerd, opent u de Windows-instellingen om een systeemomgevingsvariabele te configureren met de naam `DOCKER_HOST` en de waarde `:2375`. Daarna kunt u het Swarm-cluster openen via de Docker CLI.

## <a name="next-steps"></a>Volgende stappen
Containers in het cluster implementeren en beheren:

* [Werken met de Azure Container Service en Kubernetes](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [Werken met de Azure Container Service en DC/OS](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [Werken met de Azure Container Service en Docker Swarm](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

