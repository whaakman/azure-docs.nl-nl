<properties
   pageTitle="Taakverdelingscontainers in een Azure Container Service-cluster | Microsoft Azure"
   description="Verdeel taken over meerdere containers in een Azure Container Service-cluster."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>


# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Taakverdelingscontainers in een Azure Container Service-cluster

In dit artikel wordt beschreven hoe een interne load balancer in een met DC/OS beheerde Azure Container Service wordt gemaakt met Marathon-LB. Hiermee kunt u uw toepassingen horizontaal schalen. U kunt hiermee ook gebruikmaken van de clusters met openbare en persoonlijke agents door de load balancers in het openbare cluster en uw toepassingscontainers in het persoonlijke cluster te plaatsen.

## <a name="prerequisites"></a>Vereisten

[Implementeer een exemplaar van Azure Container Service](container-service-deployment.md) met orchestrator-type DC/OS en [zorg dat de client verbinding kan maken met uw cluster](container-service-connect.md). 

## <a name="load-balancing"></a>Taakverdeling

Er zijn twee taakverdelingslagen in de Container Service-cluster die we gaan bouwen: 

  1. de Azure Load Balancer biedt openbare toegangspunten (voor de eindgebruikers). Dit wordt automatisch aangeboden door de Azure Container Service en is standaard geconfigureerd om poorten 80 en 443 van 8080 open te stellen.
  2. De Marathon Load Balancer (marathon-lb) routeert binnenkomende aanvragen naar de containerinstanties die deze aanvragen bedienen. Als we de containers schalen met onze webservice, past marathon-lb zich dynamisch aan. Deze taakverdeling is geen standaardvoorziening in uw Container Service, maar is heel gemakkelijk te installeren.

## <a name="marathon-load-balancer"></a>Marathon Load Balancer

Marathon Load Balancer herconfigureert zichzelf dynamisch op basis van de containers die u hebt geïmplementeerd. De Marathon-taakverdeling past zich ook aan het verlies van een container of agent aan. Als dit gebeurt, wordt de container door Apache Mesos elders opnieuw gestart en marathon-lb wordt aangepast.

Om de Marathon Load Balancer te installeren kunt u de DC/OS web UI of de opdrachtregel gebruiken.

### <a name="install-marathon-lb-using-dc/os-web-ui"></a>Installeer Marathon-LB met DC/OS Web UI

  1. Klik op “Universe”
  2. Zoek naar “Marathon-LB”
  3. Klik op “Installeren”

![marathon-lb via the DC/OS Web Interface installeren](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dc/os-cli"></a>Marathon-LB met de DC/OS CLI installeren

Na de installatie van de DC/OS CLI en ervoor te hebben gezorgd dat u verbinding kunt maken met uw cluster, voert u de volgende opdrcht uit op uw clientcomputer:

```bash
dcos package install marathon-lb
```

Met deze opdracht wordt de load balancer automatisch geïnstalleerd in het cluster met openbare agents.

## <a name="deploy-a-load-balanced-web-application"></a>Een webtoepassing met taakverdeling implementeren

Nu we het Marathon-LB-pakket hebben, kunnen we een toepassingscontainer implementeren waarvan we de taken willen verdelen. Voor dit voorbeeld implementeren we een eenvoudige webserver met behulp van de volgende configuratie:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Stel de waarde van `HAProxy_0_VHOST` in op de FQDN van de taakverdeling voor uw agents. Dit heeft de vorm `<acsName>agents.<region>.cloudapp.azure.com`. Als u bijvoorbeeld een Container Service-cluster maakt met naam `myacs` in regio `West US`, dan krijgt de FQDN de waarde `myacsagents.westus.cloudapp.azure.com`. U kunt deze ook vinden door te zoeken naar de taakverdeling met 'agent' in de naam in de resources in de resourcegroep die u hebt gemaakt voor uw Containerservice in de [Azure Portal](https://portal.azure.com).
  * Stel servicePort in voor een poort >= 10.000. Zo wordt aangegeven dat de service in deze container wordt uitgevoerd. marathon-lb gebruikt dit om services te identificeren die moeten worden verdeeld.
  * Stel het `HAPROXY_GROUP` label op “extern”.
  * Stel `hostPort` op 0. Zo zal Marathon een willekeurige poort toewijzen.
  * Stel `instances` in op het aantal exemplaren dat u wilt maken. U kunt deze altijd omhoog en omlaag schalen.

Het is goed om te weten dat Marathon standaard in het persoonlijke cluster wordt geïmplementeerd. Dat betekent dat de bovengenoemde implementatie alleen toegankelijk is via de load balancer. Dit is gewoonlijk het gewenste gedrag.

### <a name="deploy-using-the-dc/os-web-ui"></a>Implementeren met de DC/OS Web UI

  1. Ga naar de Marathon-pagina op http://localhost/marathon (na instelling van uw [SSH-tunnel](container-service-connect.md) en klik op `Create Appliction`
  2. Klik in het dialoogvenster `New Application` op `JSON Mode` in de rechterbovenhoek
  3. Plak bovenstaande JSON in de editor
  4. Klik op `Create Appliction`

### <a name="deploy-using-the-dc/os-cli"></a>Implementeren met de DC/OS CLI

Om deze toepassing te implementeren met de DC/OS CLI moet u gewoon de bovenstaande JSON kopiëren in een bestand met de naam `hello-web.json` en uitvoeren:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure Load Balancer

Standaard stelt de Azure Load Balancer poorten 80, 8080 en 443 open. Als u een van deze drie poorten gebruikt (zoals we dit in het bovenstaande voorbeeld doen), dan hoeft u niets te doen. U moet de FQDN van de taakverdeling van uw agent kunnen bereiken en telkens u vernieuwt bereikt u één van de drie webservers volgens round-robin. Als u echter een andere poort gebruikt, dient u een round-robinregel en een probe toevoegen aan de taakverdeling voor de gebruikte poort. Dit kan vanuit de [Azure CLI](../xplat-cli-azure-resource-manager.md), met de opdrachten `azure network lb rule create` en `azure network lb probe create`. Dit kan ook met de Azure Portal.


## <a name="additional-scenarios"></a>Overige scenario's

U hebt een scenario nodig waarbij u gebruikmaakt van verschillende domeinen om over verschillende services te kunnen beschikken. Bijvoorbeeld:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Kijk hier voor op de Engelstalige website [virtual hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/). Hier vindt u een methode waarmee u domeinen aan specifieke Marathon-taakverdelingspaden kunt koppelen.

U kunt ook verschillende poorten ter beschikking stellen en deze opnieuw toewijzen aan de juiste service achter de marathon-lb. Bijvoorbeeld:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Volgende stappen

Zie de DC/OS-documentatie voor meer informatie over [marathon-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/).



<!--HONumber=Oct16_HO3-->


