---
title: (AFGESCHAFT) Canary-release met Vamp op Azure DC/OS-cluster
description: Vamp canary-release-Services gebruiken en toepassen van slimme verkeer filteren op een Azure Container Service DC/OS-cluster
services: container-service
author: gggina
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: cd2eb3ba1d3207f4f210aa259e938bb42b44d37a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57535447"
---
# <a name="deprecated-canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>(AFGESCHAFT) Microservices canary-release met Vamp op een Azure Container Service DC/OS-cluster

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

In dit scenario wordt ingesteld Vamp in Azure Container Service met een DC/OS-cluster. We canary release van de Vamp demo-service "sava" en vervolgens incompatibiliteit van de service met Firefox oplossen door toe te passen slimme verkeer filteren. 

> [!TIP] 
> In dit overzicht Vamp wordt uitgevoerd op een DC/OS-cluster, maar u kunt ook Vamp met Kubernetes gebruiken als de orchestrator.
>

## <a name="about-canary-releases-and-vamp"></a>Over canary releases en Vamp


[Canary vrijgeven](https://martinfowler.com/bliki/CanaryRelease.html) is een slimme implementatie-strategie door innovatieve organisaties zoals Netflix, Facebook en Spotify vastgesteld. Het is een benadering die zinvol, omdat deze problemen vermindert, veiligheid-netten introduceert en verhoogt de innovatie. Dus waarom niet alle bedrijven gebruikt? Uitbreiden van een CI/CD-pijplijn om op te nemen canary strategieën voegt complexiteit toe en uitgebreide devops kennis en ervaring voor nodig. Dat is voldoende zijn voor het blokkeren van kleinere bedrijven en ondernemingen die zowel voordat ze zelfs aan de slag. 

[Vamp](https://vamp.io/) een open-source-systeem dat is ontworpen voor het vereenvoudigen van deze overgang en breng Canarische functies aan uw voorkeur container scheduler brengt. Canary functionaliteit van vamp gaat dan implementaties op basis van een percentage. Verkeer kan worden gefilterd en splitsen op een breed scala aan de voorwaarden, bijvoorbeeld voor richten op specifieke gebruikers, IP-adresbereiken of apparaten. Vamp worden bijgehouden en maatstaven voor prestaties, waardoor het automation op basis van echte gegevens analyseert. U kunt instellen van automatisch herstel op fouten of afzonderlijke service varianten op basis van de belasting of latentie te schalen.

## <a name="set-up-azure-container-service-with-dcos"></a>Azure Container Service met DC/OS instellen



1. [Een DC/OS-cluster implementeren](container-service-deployment.md) met één master en twee agents van standaardgrootte. 

2. [Maken van een SSH-tunnel](../container-service-connect.md) verbinding maken met de DC/OS-cluster. In dit artikel wordt ervan uitgegaan dat u tunnel naar het cluster op de lokale poort 80.


## <a name="set-up-vamp"></a>Instellen van Vamp

Nu dat u een actief DC/OS-cluster hebt, kunt u Vamp installeren vanuit de DC/OS-gebruikersinterface (http://localhost:80). 

![DC/OS-webgebruikersinterface](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

Installatie is voltooid in twee fasen:

1. **Elasticsearch implementeren**.

2. Vervolgens **Vamp implementeren** door het installeren van het pakket Vamp DC/OS-universum.

### <a name="deploy-elasticsearch"></a>Elasticsearch implementeren

Elasticsearch vereist vamp voor het verzamelen van metrische gegevens en aggregatie. U kunt de [magneticio Docker-installatiekopieën](https://hub.docker.com/r/magneticio/elastic/) naar een compatibele Vamp Elasticsearch-stack implementeren.

1. In de gebruikersinterface van DC/OS, gaat u naar **Services** en klikt u op **Service implementeren**.

2. Selecteer **JSON-modus** uit de **nieuwe Service implementeren** pop-upvenster.

  ![Selecteer JSON-modus](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. Plak de volgende JSON. Deze configuratie voert de container met 1 GB aan RAM-geheugen en een eenvoudige statuscontrole op de poort Elasticsearch.
  
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
  

3. Klik op **Implementeren**.

  DC/OS implementeert de Elasticsearch-container. U kunt de voortgang volgen op de **Services** pagina.  

  ![e implementeren? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Vamp implementeren

Als Elasticsearch als rapporteert **met**, kunt u de DC/OS-universum Vamp-pakket toevoegen. 

1. Ga naar **universum** en zoek naar de **vamp**. 
  ![Vamp op DC/OS-universum](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. Klik op **installeren** naast de vamp verpakt en kies **geavanceerde installatie**.

3. Schuif naar beneden en voer de volgende elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`. 

  ![Elasticsearch-URL invoeren](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. Klik op **Bekijk en installeer**, klikt u vervolgens op **installeren** implementatie te starten.  

  DC/OS implementeert alle vereiste Vamp-onderdelen. U kunt de voortgang volgen op de **Services** pagina.
  
  ![Vamp als universum pakket implementeren](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. Als de implementatie is voltooid, kunt u de gebruikersinterface Vamp openen:

  ![Vamp-service op DC/OS](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Vamp UI](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>Uw eerste service implementeren

Nu dat Vamp actief en werkend is een service implementeren vanuit een blauwdruk. 

In de meest eenvoudige vorm een [Vamp blauwdruk](https://vamp.io/documentation/using-vamp/blueprints/) beschrijving van de eindpunten (gateways), clusters en services te implementeren. Vamp maakt gebruik van clusters op verschillende varianten van dezelfde service te groeperen in logische groepen voor canary vrijgeven of A / B-tests.  

In dit scenario maakt gebruik van een voorbeeld van een monolithische toepassing met de naam [ **sava**](https://github.com/magneticio/sava), die versie 1.0 is. De monoliet bevindt zich in een Docker-container in Docker Hub onder magneticio/sava:1.0.0 is. De app wordt normaal uitgevoerd op poort 8080, maar u wilt om in dit geval onder poort 9050 zichtbaar te maken. Implementeer de app via Vamp met behulp van een eenvoudige blauwdruk.

1. Ga naar **implementaties**.

2. Klik op **Add**.

3. Plak de volgende blauwdruk YAML. Deze blauwdruk bevat één cluster met slechts één service-variant, waarin we wijzigingen kan in een latere stap aanbrengen:

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

De implementatie wordt weergegeven op de **implementaties** pagina. Klik op de implementatie voor het controleren van de status ervan.

![UI - sava implementeren vamp](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![Sava service in de gebruikersinterface Vamp](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

Twee gateways die zijn gemaakt, die worden weergegeven op de **Gateways** pagina:

* een stabiele eindpunt voor toegang tot de service (poort 9050) 
* een beheerde Vamp interne-gateway (deze gateway later meer). 

![UI - sava gateways vamp](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

De service sava is nu geïmplementeerd, maar u geen toegang tot het extern omdat de Azure Load Balancer voor het doorsturen van verkeer naar deze nog niet bekend is. Voor toegang tot de service, de Azure VPN-configuratie bijwerken


## <a name="update-the-azure-network-configuration"></a>Werk de configuratie van de Azure-netwerk

Vamp geïmplementeerd met de service sava op de DC/OS-agent-knooppunten om een stabiele eindpunt op poort 9050 vrij te geven. Voor toegang tot de service van buiten het DC/OS-cluster, moet u de volgende wijzigingen aanbrengen aan de configuratie van de Azure-netwerk in uw Clusterimplementatie: 

1. **Configureren van de Azure Load Balancer** voor de agents (de resource met de naam **dcos-agent-lb-xxxx**) met een statustest en een regel voor het doorsturen van verkeer op poort 9050 naar de sava-exemplaren. 

2. **Bijwerken van de netwerkbeveiligingsgroep** voor de openbare agents (de resource met de naam **XXXX-agent-openbare-nsg-XXXX**) voor verkeer op poort 9050.

Zie voor gedetailleerde stappen voor het voltooien van deze taken via Azure portal [openbare toegang tot een Azure Container Service-toepassing](container-service-enable-public-access.md). Poort 9050 voor alle poortinstellingen opgeven.


Als alles is gemaakt, gaat u naar de **overzicht** blade van de load balancer voor DC/OS-agent (de resource met de naam **dcos-agent-lb-xxxx**). Zoek de **openbaar IP-adres**, en het gebruik van het adres voor toegang tot sava op poort 9050.

![Azure portal - openbare IP-adres ophalen](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![sava](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Canary-release uitvoeren

Stel dat u hebt een nieuwe versie van deze toepassing die u wilt canary-release in productie. U hebt deze opgenomen in een container als magneticio/sava:1.1.0 en klaar bent om door te gaan. Vamp kunt u eenvoudig nieuwe services toevoegen aan de actieve implementatie. Deze 'samengevoegde' services worden geïmplementeerd naast de bestaande services in het cluster en toegewezen een gewicht van 0%. Geen verkeer wordt doorgestuurd naar een nieuwe, samengevoegde service totdat u de distributie van verkeer aanpassen. De schuifregelaar gewicht in de gebruikersinterface Vamp biedt u volledige controle over de distributie, waardoor het incrementele aanpassingen (canary-release) of een directe terugdraaien.

### <a name="merge-a-new-service-variant"></a>Een nieuwe variant van de service samenvoegen

Samenvoegen van de nieuwe sava 1.1-service met de actieve implementatie:

1. Klik in de gebruikersinterface Vamp **blauwdrukken**.

2. Klik op **toevoegen** en plak deze in de volgende YAML-blauwdruk: Deze blauwdruk wordt een nieuwe service-variant (sava: 1.1.0) te implementeren binnen het bestaande cluster (sava_cluster) beschreven.

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
  
3. Klik op **Opslaan**. De blauwdruk is opgeslagen en weergegeven op de **blauwdrukken** pagina.

4. Open het actiemenu van de blauwdruk sava: 1.1 en klikt u op **samenvoegt met**.

  ![UI - blauwdrukken vamp](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. Selecteer de **sava** implementatie en klik op **samenvoegen**.

  ![UI - samenvoegen-blauwdruk voor implementatie vamp](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

Vamp implementeert u de nieuwe service-variant voor sava: 1.1.0 is beschreven in de blauwdruk naast sava: 1.0.0 in de **sava_cluster** van de actieve implementatie. 

![UI - implementatie van bijgewerkte sava vamp](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

De **sava/sava_cluster/webport** gateway (het clustereindpunt) is ook bijgewerkt, een route toevoegen aan de zojuist geïmplementeerde sava: 1.1.0. Op dit moment geen verkeer wordt doorgestuurd hier (de **gewicht** is ingesteld op 0%).

![UI - cluster gateway vamp](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Canary-release

Aanpassen met beide versies van sava geïmplementeerd in hetzelfde cluster en de distributie van verkeer tussen beide door het verplaatsen van de **gewicht** schuifregelaar.

1. Klik op ![Vamp UI - bewerken](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) naast **gewicht**.

2. De gewichtsdistributie ingesteld op 50/50% en klikt u op **opslaan**.

  ![UI - gateway gewicht schuifregelaar vamp](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Ga terug naar uw browser en vernieuw de pagina sava nog een paar keer. De toepassing sava nu schakelt u tussen een sava: 1.0 en een sava: 1.1-pagina.

  ![wisselende sava1.0 en sava1.1 services](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > Deze alternatieven van de pagina werkt het beste met de 'Incognito' of 'Anoniem'-modus van uw browser vanwege het in cache plaatsen van statische assets.
  >

### <a name="filter-traffic"></a>Verkeer filteren

Stel dat na de implementatie die u incompatibiliteit in sava: 1.1.0 ontdekt dat oorzaken problemen in Firefox-browser worden weergegeven. U kunt Vamp naar binnenkomend verkeer filteren en aangeven dat alle gebruikers van Firefox terug naar de bekende stabiele sava: 1.0.0 instellen. Dit filter herstelt direct de onderbreking voor gebruikers van Firefox, terwijl alle andere blijft om te profiteren van de voordelen van de verbeterde sava: 1.1.0.

Maakt gebruik van vamp **voorwaarden** op het filter verkeer tussen de routes in een gateway. Verkeer wordt eerst gefilterd en worden omgeleid op basis van de voorwaarden die zijn toegepast op elke route. Alle resterende verkeer wordt verdeeld volgens de instelling van de gateway gewicht.

U kunt een voorwaarde om te filteren van alle Firefox-gebruikers en ze rechtstreeks naar de oude sava: 1.0.0 maken:

1. Op de sava/sava_cluster/webport **Gateways** pagina, klikt u op ![Vamp UI - bewerken](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) om toe te voegen een **voorwaarde** naar de sava/sava_cluster/sava:1.0.0/webport route. 

2. Voer de voorwaarde **gebruikersagent Firefox ==** en klikt u op ![Vamp UI - opslaan](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).

  Vamp voegt de voorwaarde met een standaardwaarde van 0%. Als u wilt beginnen met het filteren van verkeer, moet u de sterkte van de voorwaarde aanpassen.

3. Klik op ![Vamp UI - bewerken](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) wijzigen van de **sterkte** toegepast op de voorwaarde.
 
4. Stel de **sterkte** tot 100% en klikt u op ![Vamp UI - opslaan](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) om op te slaan.

  Vamp verzendt nu al het verkeer aan de voorwaarde (alle Firefox gebruikers) naar sava: 1.0.0.

  ![Vamp UI - voorwaarde van toepassing op gateway](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Ten slotte het gewicht van de gateway voor het verzenden van alle resterende verkeer (alle niet-Firefox gebruikers) naar de nieuwe sava: 1.1.0 aanpassen. Klik op ![Vamp UI - bewerken](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) naast **gewicht** en stel de gewichtsdistributie zodat 100% wordt omgeleid naar de sava/sava_cluster/sava:1.1.0/webport route.

  Al het verkeer niet gefilterd door de voorwaarde wordt nu omgeleid naar de nieuwe sava: 1.1.0.

6. Als u wilt het filter in actie zien, opent u twee verschillende browsers (één Firefox en een andere browser) en toegang tot de service sava van beide. Alle Firefox-aanvragen worden verzonden naar sava: 1.0.0, terwijl alle andere browsers worden doorgestuurd naar sava: 1.1.0.

  ![UI - filter verkeer vamp](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Tellen

In dit artikel is een korte inleiding over Vamp op een DC/OS-cluster. Om te beginnen, je Vamp omhoog en uitgevoerd op uw Azure Container Service DC/OS-cluster geïmplementeerd met een service met een blauwdruk Vamp en toegang tot het eindpunt van de beschikbaar gemaakte (gateway).

We ook ingaan op enkele krachtige functies van Vamp: samenvoegen van een nieuwe variant van de service aan de actieve implementatie en het incrementeel introductie, wordt het filteren van verkeer om op te lossen van een bekende compatibiliteitsproblemen.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het beheren van Vamp acties via de [REST-API Vamp](https://vamp.io/documentation/api/api-reference/).

* Vamp automatiseringsscripts in Node.js bouwen en uitvoeren als [Vamp werkstromen](https://vamp.io/documentation/using-vamp/v1.0.0/workflows/#create-a-workflow).

* Zie de aanvullende [VAMP zelfstudies](https://vamp.io/documentation/tutorials/).

