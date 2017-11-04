Nadat u [een Azure Container Service-cluster hebt geïmplementeerd](../articles/container-service/dcos-swarm/container-service-deployment.md), moet u mogelijk het aantal agentknooppunten wijzigen. U hebt mogelijk meer agents nodig zodat u meer containertoepassingen of -exemplaren kunt uitvoeren. 

U kunt het aantal agentknooppunten in een DC/OS-, Docker Swarm- of Kubernetes-cluster wijzigen met behulp van Azure Portal of de Azure CLI 2.0. 

## <a name="scale-with-the-azure-portal"></a>Schalen met Azure Portal

1. Browse in [Azure Portal](https://portal.azure.com) naar **Containerservices** en klik vervolgens op de containerservice die u wilt wijzigen.
2. Klik in de blade **Containerservice** op **Agents**.
3. Voer bij **Aantal VM's** het gewenste aantal agentknooppunten in.

    ![Een pool schalen in de portal](./media/container-service-scale/container-service-scale-portal.png)

4. Klik op **Opslaan** om de configuratie op te slaan.

## <a name="scale-with-the-azure-cli-20"></a>Schalen met de Azure CLI 2.0

Zorg ervoor dat de meest recente versie van Azure CLI 2.0 is [geïnstalleerd](/cli/azure/install-az-cli2) en dat u bent aangemeld bij een Azure-account (`az login`).

### <a name="see-the-current-agent-count"></a>Bekijk het huidige aantal agents
Voer de `az acs show` opdracht uit om te zien hoeveel agents zich momenteel in het cluster bevinden. Deze geeft de clusterconfiguratie weer. De volgende opdracht toont bijvoorbeeld de configuratie van de containerservice met de naam `containerservice-myACSName` in de resourcegroep `myResourceGroup`:

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

De opdracht retourneert het aantal agents in de waarde `Count` onder `AgentPoolProfiles`.

### <a name="use-the-az-acs-scale-command"></a>De opdracht az acs scale gebruiken
Als u het aantal agentknooppunten wilt wijzigen, voert u de opdracht `az acs scale` uit en geeft u de **resourcegroep**, **containerservicenaam** en het gewenste **aantal nieuwe agents** op. Door een kleiner of groter getal te gebruiken kunt u omlaag of omhoog schalen.

Voer bijvoorbeeld de volgende opdracht in als u het aantal agents in het voorgaande cluster naar 10 wilt wijzigen:

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

De Azure CLI 2.0 retourneert een JSON-tekenreeks die de nieuwe configuratie van de containerservice vertegenwoordigt, inclusief het aantal nieuwe agents.

Voer `az acs scale --help` uit voor meer opdrachtopties.

## <a name="scaling-considerations"></a>Overwegingen voor schalen

* Het aantal agentknooppunten moet tussen 1 en 100 liggen. 

* Het quotum voor kernen kan het aantal agentknooppunten in een cluster beperken.

* Schaalbewerkingen voor agentknooppunten worden toegepast op een virtuele-machineschaalset van Azure die de agentpool bevat. In een DC/OS-cluster worden alleen agentknooppunten in de persoonlijke pool geschaald door de bewerkingen in dit artikel.

* Afhankelijk van de orchestrator die u in uw cluster implementeert, kunt u het aantal exemplaren van een container die wordt uitgevoerd op het cluster afzonderlijk schalen. Gebruik in een DC/OS-cluster bijvoorbeeld de [Marathon-gebruikersinterface](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) om het aantal exemplaren van een containertoepassing te wijzigen.

* Automatisch schalen van agentknooppunten in een containerservicecluster wordt momenteel niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
* Bekijk [meer voorbeelden](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) van de toepassing van Azure CLI 2.0-opdrachten met Azure Container Service.
* Meer informatie over [DC/OS-agentpools](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) in Azure Container Service.

