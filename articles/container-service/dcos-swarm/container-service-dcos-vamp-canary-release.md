---
title: Kokospalm release met Vamp op Azure DC/OS-cluster | Microsoft Docs
description: Het gebruik van Vamp kokospalm release Services en toepassen van slimme verkeer filteren op een Azure Container Service DC/OS-cluster
services: container-service
author: gggina
manager: rasquill
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: 4a20091b59f2643ea71cce99c159a5075706e35d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>Kokospalm release microservices met Vamp in een Azure Container Service DC/OS-cluster

In dit scenario stelt Vamp op Azure Container Service met een DC/OS-cluster. We kokospalm release van de Vamp demoservice 'sava' en vervolgens incompatibiliteit van de service met Firefox oplossen door toe te passen smart verkeer gefilterd. 

> [!TIP] 
> In dit overzicht Vamp wordt uitgevoerd op een DC/OS-cluster, maar u kunt ook Vamp met Kubernetes gebruiken als de orchestrator.
>

## <a name="about-canary-releases-and-vamp"></a>Over Canarische vrijgegeven en Vamp


[Vrijgeven van kokospalm](https://martinfowler.com/bliki/CanaryRelease.html) is een strategie voor slimme implementatie door innovatieve organisaties zoals Netflix, Facebook en Spotify vastgesteld. Het is een benadering die logisch, klinkt omdat deze problemen vermindert veiligheid netten introduceert en verhoogt de vernieuwing. Waarom worden niet alle bedrijven met behulp van deze? Uitbreiden van een pijplijn CI/CD om op te nemen kokospalm strategieën voegt complexiteit en uitgebreide devops kennis en ervaring vereist. Dit is voldoende is voor het blokkeren van kleinere bedrijfs- en ondernemingsbeheer zowel voordat ze ook aan de slag. 

[Vamp](http://vamp.io/) brengt een open source-systeem ontworpen voor het vereenvoudigen van deze overgang en brengt Canarische functies aan uw voorkeur container scheduler. De vamp kokospalm functionaliteit zich verder uitstrekt dan implementaties op basis van een percentage. Verkeer kan worden gefilterd en splitst bij een breed scala aan de voorwaarden, bijvoorbeeld voor het doel specifieke gebruikers, IP-adresbereiken of apparaten. Vamp houdt en maatstaven voor prestaties, zodat voor automatisering op basis van echte gegevens analyseert. U kunt instellen op automatisch herstel op fouten of afzonderlijke service varianten op basis van load of latentie schalen.

## <a name="set-up-azure-container-service-with-dcos"></a>Azure Container Service met DC/OS instellen



1. [Een DC/OS-cluster implementeren](container-service-deployment.md) met één master en twee agents van standaardgrootte. 

2. [Maken van een SSH-tunnel](../container-service-connect.md) verbinding maken met de DC/OS-cluster. In dit artikel wordt ervan uitgegaan dat u tunnel aan het cluster op de lokale poort 80.


## <a name="set-up-vamp"></a>Vamp instellen

Nu dat u een actieve DC/OS-cluster hebt, kunt u Vamp installeren vanaf de DC/OS-gebruikersinterface (http://localhost:80). 

![DC/OS-webgebruikersinterface](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

De installatie wordt uitgevoerd in twee fasen:

1. **Implementeer Elasticsearch**.

2. Vervolgens **Vamp implementeren** door de DC/OS Vamp universe pakket installeert.

### <a name="deploy-elasticsearch"></a>Elasticsearch implementeren

Elasticsearch vereist vamp voor het verzamelen van de metrische gegevens en aggregatie. U kunt de [magneticio Docker installatiekopieën](https://hub.docker.com/r/magneticio/elastic/) voor het implementeren van een compatibel Vamp Elasticsearch-stack.

1. Ga in de DC/OS-gebruikersinterface naar **Services** en klik op **Service implementeren**.

2. Selecteer **JSON-modus** van de **nieuwe Service implementeren** pop-upvenster.

  ![Selecteer JSON-modus](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. In de volgende JSON plakken. Deze configuratie wordt de container met 1 GB RAM-geheugen en een eenvoudige statuscontrole op de poort Elasticsearch uitgevoerd.
  
  ```JSON
  {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
  }
  ```
  

3. Klik op **implementeren**.

  DC/OS implementeert de Elasticsearch-container. U kunt de voortgang volgen op de **Services** pagina.  

  ![e implementeren? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Vamp implementeren

Zodra Elasticsearch als rapporteert **met**, kunt u het pakket Vamp DC/OS Universe toevoegen. 

1. Ga naar **Universe** en zoek naar **vamp**. 
  ![Vamp op DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. Klik op **installeren** naast de vamp van het pakket en kies **installatie in de geavanceerde**.

3. Schuif naar beneden en voer de volgende elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`. 

  ![Voer de URL Elasticsearch](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. Klik op **Bekijk en installeer**, klikt u vervolgens op **installeren** implementatie te starten.  

  DC/OS implementeert alle vereiste Vamp onderdelen. U kunt de voortgang volgen op de **Services** pagina.
  
  ![Vamp als universe pakket implementeren](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. Zodra de implementatie is voltooid, kunt u toegang tot de gebruikersinterface Vamp:

  ![-Service op de DC/OS vamp](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Vamp gebruikersinterface](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>Uw eerste service implementeren

Nu dat Vamp actief en werkend is, een service implementeren vanuit een blauwdruk. 

In de meest eenvoudige vorm een [Vamp blauwdruk](http://vamp.io/documentation/using-vamp/blueprints/) beschrijft de eindpunten (gateways), clusters en services te implementeren. Vamp clusters gebruikt om verschillende varianten van dezelfde service te groeperen in logische groepen voor kokospalm vrijgeven of A / B-tests.  

Dit scenario wordt gebruikt voor een voorbeeldtoepassing monolithische aangeroepen [ **sava**](https://github.com/magneticio/sava), die is op versie 1.0. De monoliet wordt verpakt in een Docker-container in Docker-Hub onder de magneticio/sava:1.0.0 ligt. De app wordt normaal uitgevoerd op poort 8080, maar u wilt in dit geval onder poort 9050 weergeven. Implementeer de app via Vamp met behulp van een eenvoudige blauwdruk.

1. Ga naar **implementaties**.

2. Klik op **Add**.

3. In de volgende blauwdruk YAML te plakken. Deze blauwdruk bevat één cluster met slechts één service-variant, die we in een latere stap wijzigen:

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster to create
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. Klik op **Opslaan**. Vamp initieert de implementatie.

De implementatie wordt weergegeven op de **implementaties** pagina. Klik op de implementatie als de status wilt controleren.

![UI - sava implementeren vamp](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![de service Sava in Vamp gebruikersinterface](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

Twee gateways worden gemaakt, die worden vermeld op de **Gateways** pagina:

* een stabiele eindpunt voor toegang tot de service (poort 9050) 
* een beheerde Vamp interne gateway (meer informatie over deze gateway later). 

![UI - sava gateways vamp](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

De service sava is nu geïmplementeerd, maar u geen toegang tot deze extern omdat de Load Balancer van Azure voor het doorsturen van verkeer naar deze nog niet weet. Voor toegang tot de service, werk de netwerkconfiguratie van Azure.


## <a name="update-the-azure-network-configuration"></a>Werk de configuratie van de Azure-netwerk

Vamp de sava-service op de DC/OS-agent knooppunten, die een stabiele eindpunt op poort 9050 geïmplementeerd. Voor toegang tot de service van buiten het DC/OS-cluster, moet u de volgende wijzigingen aanbrengen aan de configuratie van de Azure-netwerk in uw implementatie van het cluster: 

1. **Configureren van de Azure Load Balancer** voor de agents (de resource met de naam **dcos-agent-lb-xxxx**) met een health test en een regel voor het doorsturen van verkeer op poort 9050 naar de sava-exemplaren. 

2. **Bijwerken van de netwerkbeveiligingsgroep** voor de openbare agents (de resource met de naam **XXXX-agent-openbare-nsg-XXXX**)-verkeer op poort 9050 toestaan.

Zie voor gedetailleerde stappen voor het voltooien van deze taken met de Azure portal [openbare toegang tot een Azure Container Service-toepassing](container-service-enable-public-access.md). Poort 9050 voor alle poortinstellingen opgeven.


Zodra u alles is gemaakt, gaat u naar de **overzicht** blade van de load balancer voor DC/OS-agent (de resource met de naam **dcos-agent-lb-xxxx**). Zoek de **openbaar IP-adres**, en het gebruik van het adres voor toegang tot sava op poort 9050.

![Azure portal - get openbaar IP-adres](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Sava](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Voer een kokospalm release

Stel dat u hebt een nieuwe versie van deze toepassing die u wilt kokospalm release in productie. U hebt het beperkte als magneticio/sava:1.1.0 en klaar bent om door te gaan. Vamp kunt u eenvoudig nieuwe services toegevoegd aan de actieve implementatie. Deze 'samengevoegde' services worden geïmplementeerd naast de bestaande services in het cluster en een gewicht van 0% wordt toegewezen. Er is geen verkeer wordt doorgestuurd naar een nieuwe, samengevoegde service totdat u de distributie van verkeer aanpassen. De schuifregelaar gewicht in de gebruikersinterface Vamp biedt u volledige controle over de distributie, waardoor het incrementele aanpassingen (kokospalm release) of een directe terugdraaien.

### <a name="merge-a-new-service-variant"></a>Een nieuwe service-variant samenvoegen

Samenvoegen van de nieuwe sava 1.1-service met de actieve implementatie:

1. Klik in de gebruikersinterface Vamp **blauwdrukken**.

2. Klik op **toevoegen** en plak in de volgende blauwdruk YAML: deze blauwdruk wordt een nieuwe service-variant (sava: 1.1.0) te implementeren binnen het bestaande cluster (sava_cluster) beschreven.

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster to update
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint to update
  ```
  
3. Klik op **Opslaan**. De blauwdruk wordt opgeslagen en weergegeven op de **blauwdrukken** pagina.

4. Open het actiemenu op de blauwdruk sava: 1.1 en klik op **samenvoegen naar**.

  ![UI - blauwdrukken vamp](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. Selecteer de **sava** implementatie en klik op **samenvoegen**.

  ![UI - samenvoegen blauwdruk aan implementatie vamp](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

Vamp implementeert de nieuwe service-variant voor sava: 1.1.0 is beschreven in de blauwdruk naast sava: 1.0.0 in de **sava_cluster** van de actieve implementatie. 

![UI - bijgewerkte sava implementatie vamp](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

De **sava_cluster-sava/webport** gateway (de clustereindpunt) wordt ook bijgewerkt, een route toe te voegen aan de zojuist geïmplementeerde sava: 1.1.0. Op dit moment geen verkeer wordt gerouteerd hier (de **gewicht** is ingesteld op 0%).

![UI - cluster gateway vamp](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Kokospalm release

Met beide versies van sava in hetzelfde cluster wordt geïmplementeerd, past u de distributie van verkeer tussen hen worden verplaatst de **gewicht** schuifregelaar.

1. Klik op ![Vamp UI - bewerken](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) naast **gewicht**.

2. De gewichtsdistributie ingesteld op 50% / 50% en klikt u op **opslaan**.

  ![UI - gateway gewicht schuifregelaar vamp](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Ga terug naar uw browser en vernieuw de pagina sava een paar keer. De toepassing sava nu schakelt u tussen een sava: 1.0 en een pagina sava: 1.1.

  ![wisselende sava1.0 en sava1.1 services](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > Deze alternatieven van de pagina geschikt is voor de 'Incognito' of 'Anonymous' modus van uw browser vanwege de caching van statische elementen.
  >

### <a name="filter-traffic"></a>Filteren van verkeer

Stel dat na implementatie dat u een incompatibiliteit in sava: 1.1.0 dat oorzaken problemen worden weergegeven in browsers Firefox gedetecteerd. U kunt Vamp binnenkomend verkeer filteren en omleiden dat alle Firefox gebruikers terug naar de bekende stabiele sava: 1.0.0 instellen. Dit filter herstelt direct de onderbreking voor Firefox gebruikers, terwijl alle andere nog steeds profiteren van de voordelen van het verbeterde sava: 1.1.0.

Maakt gebruik van vamp **voorwaarden** op het filter-verkeer tussen routes in een gateway. Het verkeer wordt eerst gefilterd en omgeleid op basis van de voorwaarden die bij elke route. Alle resterende verkeer wordt verdeeld volgens de instelling voor het gewicht van gateway.

U kunt een voorwaarde voor het filteren van alle Firefox gebruikers en ze rechtstreeks naar de oude sava: 1.0.0 maken:

1. Op sava/sava_cluster/webport **Gateways** pagina, klikt u op ![Vamp UI - bewerken](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) om toe te voegen een **voorwaarde** naar de sava/sava_cluster/sava:1.0.0/webport route. 

2. Voer de voorwaarde **gebruikersagent Firefox ==** en klik op ![Vamp UI - opslaan](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).

  Vamp voegt de voorwaarde van een standaardwaarde van 0%. Als u wilt filteren van verkeer, moet u de voorwaarde sterkte aanpassen.

3. Klik op ![Vamp UI - bewerken](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) wijzigen de **sterkte** aan de voorwaarde wordt toegepast.
 
4. Stel de **sterkte** naar 100%, klik op ![Vamp UI - opslaan](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) om op te slaan.

  Vamp verzendt nu al het verkeer die overeenkomt met de voorwaarde (alle Firefox gebruikers) voor het sava: 1.0.0.

  ![Vamp UI - voorwaarde van toepassing op gateway](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Ten slotte het gewicht van de gateway voor het verzenden van alle resterende verkeer (alle gebruikers voor niet-Firefox) naar de nieuwe sava: 1.1.0 aanpassen. Klik op ![Vamp UI - bewerken](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) naast **gewicht** en stel de gewichtsdistributie zodat 100% is omgeleid naar de sava/sava_cluster/sava:1.1.0/webport route.

  Al het verkeer niet door de voorwaarde worden gefilterd nu doorgestuurd naar de nieuwe sava: 1.1.0.

6. Het filter in te grijpen bekijken, opent u twee verschillende browsers (één Firefox en een andere browser) en toegang tot de service sava van beide. Alle Firefox aanvragen worden verzonden naar sava: 1.0.0, terwijl alle andere browsers worden doorgestuurd naar sava: 1.1.0.

  ![UI - filter verkeer vamp](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Berekening van het

Dit artikel is een korte inleiding in Vamp op een DC/OS-cluster. Om te beginnen u Vamp up verkregen en uitgevoerd op uw Azure Container Service DC/OS-cluster, een service met een blauwdruk Vamp geïmplementeerd en het heeft geopend op de blootgestelde eindpunt (gateway).

We ook op sommige andere van krachtige functies van Vamp aangeraakt: samenvoegen van een nieuwe service-variant aan de actieve implementatie en filteren van verkeer om op te lossen een bekend incompatibiliteitsprobleem incrementeel introduceren.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het beheren van Vamp acties via de [Vamp REST-API](http://vamp.io/documentation/api/api-reference/).

* Vamp automatiseringsscripts in Node.js bouwen en uitvoeren als [Vamp werkstromen](http://vamp.io/documentation/tutorials/create-a-workflow/).

* Zie aanvullende [VAMP zelfstudies](http://vamp.io/documentation/tutorials/overview/).

