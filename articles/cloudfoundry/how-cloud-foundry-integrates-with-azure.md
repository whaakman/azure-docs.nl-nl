---
title: Hoe Cloud Foundry integreert met Azure | Microsoft Docs
description: Hierin wordt beschreven hoe Cloud Foundry can utlize Azure-services om te verbeteren de ervaring Enterprice
services: virtual-machines-linux
documentationcenter: ''
author: ningk
manager: jeconnoc
editor: ''
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: ningk
ms.openlocfilehash: 1f4afbe1849210c55c392d014449224f2fe97b04
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654943"
---
# <a name="integrate-cloud-foundry-with-azure"></a>Cloud Foundry integreren met Azure

[Foundry cloud](https://docs.cloudfoundry.org/) is een PaaS-platform uitgevoerd op IaaS-platform voor cloudproviders. Biedt consistente toepassing implementatie-ervaring via cloudproviders. Het kan bovendien ook integreren met verschillende Azure-services, met enterprise hoogwaardige HA, schaalbaarheid en kostenbesparingen met zich meebrengen.
Er zijn [6 subsystemen van Cloud Foundry](https://docs.cloudfoundry.org/concepts/architecture/), die kan worden flexibel scale online, met inbegrip van: routering, verificatie, toepassingsbeheer levenscyclus, Service-beheer, Messaging en bewaking. U kunt voor elk van de subsystemen Cloud Foundry voor het gebruik van de geadresseerde Azure-service configureren. 

![Cloud Foundry op integratie van Azure-architectuur](media/CFOnAzureEcosystem-colored.png)

## <a name="1-high-availability-and-scalability"></a>1. Hoge beschikbaarheid en schaalbaarheid
### <a name="managed-disk"></a>Beheerde schijven
Bosh maakt gebruik van Azure KPI (Cloud Provider Interface) voor het maken van de schijf en routines verwijderen. Niet-beheerde schijven worden standaard gebruikt. Hiervoor klant handmatig storage-accounts maken en vervolgens de accounts configureren in het manifest CF-bestanden. Dit komt door de beperking van het aantal schijven per storage-account.
Nu [beheerd schijf](https://azure.microsoft.com/services/managed-disks/) beschikbaar is, biedt beheerde veilige en betrouwbare schijfopslag voor virtuele machines. De klant wordt niet langer hoeft te gaan met de storage-account voor de schaal en HA. Azure rangschikt schijven automatisch. Of een nieuwe of een bestaande implementatie, wordt de KPI voor Azure maken of migreren van de beheerde schijf tijdens de implementatie van een CF afgehandeld. Met PCF 1.11 wordt ondersteund. U kunt ook de open source Cloud Foundry verkennen [schijf beheerd instructies](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/managed-disks) ter referentie. 
### <a name="availability-zone-"></a>Beschikbaarheid Zone *
Als een cloud-systeemeigen toepassingsplatform Cloud Foundry is ontworpen met [vier niveau van hoge beschikbaarheid](https://docs.pivotal.io/pivotalcf/2-1/concepts/high-availability.html). Tijdens de eerste drie niveaus van softwarefouten kunnen worden verwerkt door CF systeem zelf wordt platform fouttolerantie geleverd door cloudproviders. De hoofdonderdelen CF moeten worden beveiligd met een cloudprovider platform HA-oplossing. Dit omvat GoRouters, Diego hersenen, CF database en service tegels. Standaard [Azure Beschikbaarheidsset](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/deploy-cloudfoundry-with-availability-sets) wordt gebruikt voor fouttolerantie tussen clusters in een datacenter.
Het nieuwe goed is, [Azure beschikbaarheid Zone](https://docs.microsoft.com/azure/availability-zones/az-overview ) nu wordt vrijgegeven de fouttolerantie om een hoger niveau te brengen met lage latentie redundantie via datacenters.
Azure beschikbaarheid Zone HA bereikt door het plaatsen van een set van virtuele machines in 2 + datacenters, elke set van virtuele machines zijn redundante aan andere groepen. Als één Zone niet actief is, de andere sets zijn nog steeds actief is, geïsoleerd van de sitedatabase.
> [!NOTE] 
> Azure beschikbaarheid Zone niet aangeboden aan alle regio's nog, controleert u de meest recente [aankondiging voor een lijst met ondersteunde regio's](https://docs.microsoft.com/azure/availability-zones/az-overview). Controleer voor Open Source Cloud Foundry, [Azure beschikbaarheid Zone voor open-source Cloud Foundry richtlijnen](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/availability-zone).

## <a name="2-network-routing"></a>2. Netwerkroutering
Azure basic load balancer wordt standaard gebruikt voor inkomende aanvragen van de API-CF/apps en stuurt deze door naar de Gorouters. CF-componenten zoals Diego Brain, MySQL, invoegen kan ook de load balancer gebruiken voor het saldo van het verkeer voor HA. Azure biedt bovendien een reeks volledig beheerde load balancing-oplossingen. Als u voor TLS-beëindiging ('SSL-offload') of per toepassing HTTP/HTTPS-laag aanvraagverwerking zoeken wilt, kunt u de toepassingsgateway. Voor hoge beschikbaarheid en schaalbaarheid voor de load balancer in laag 4, kunt u standaard load balancer.
### <a name="azure-application-gateway-"></a>Azure Application Gateway *
[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) biedt verschillende laag 7 taakverdeling mogelijkheden, zoals het SSL-offloading, end-to-end SSL, Web Application Firewall, sessie cookies gebaseerde affiniteit en meer. U kunt [toepassingsgateway configureren in de Open Source Cloud Foundry](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/application-gateway). PCF, Controleer of de [PCF 2.1 releaseopmerkingen](https://docs.pivotal.io/pivotalcf/2-1/pcf-release-notes/opsmanager-rn.html#azure-application-gateway) voor Implementatiemodel test.

### <a name="azure-standard-load-balancer-"></a>Azure Standard Load Balancer *
Azure Load Balancer is een laag 4 load balancer. Wordt gebruikt voor het distribueren van het verkeer tussen de exemplaren van services in een set met gelijke taakverdeling. De standaard versie biedt [geavanceerde functies](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) boven op de standaardversie. Voorbeeld 1. De limiet voor back-end-adresgroep is verhoogd van 100 tot 1000 virtuele machines.  2. De eindpunten nu ondersteuning voor meerdere beschikbaarheidssets in plaats van één beschikbaarheidsset.  3. Aanvullende functies zoals HA-poorten, rijkere bewakingsgegevens, enzovoort. Als u naar Azure beschikbaarheid Zone overstapt, is standaard load balancer vereist. Voor een nieuwe implementatie wordt aanbevolen om te beginnen met een standaard Load Balancer van Azure. 

## <a name="3-authentication"></a>3. Verificatie 
[Foundry-gebruikersaccount en verificatie cloud](https://docs.cloudfoundry.org/concepts/architecture/uaa.html) is de centrale identity management-service voor CF en de verschillende onderdelen. [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) van Microsoft multitenant, cloud-gebaseerde directory en identity management-service is. Standaard wordt UAA voor Cloud Foundry-verificatie gebruikt. Als een geavanceerde optie ondersteuning UAA ook voor Azure AD als een externe gebruiker store. Azure AD-gebruikers hebben toegang tot de Cloud Foundry met hun identiteit LDAP, zonder Cloud Foundry-account. Volg deze stappen voor [de Azure AD configureren voor UAA in PCF](http://docs.pivotal.io/p-identity/1-6/azure/index.html).

## <a name="4-data-storage-for-cloud-foundry-runtime-system"></a>4. Gegevensopslag voor Cloud Foundry Runtime-systeem
Cloud Foundry biedt uitstekende uitbreidbaarheid voor het gebruik van Azure blobarchief of Azure MySQL/PostgreSQL-services voor opslag voor servertoepassingen runtime-systeem.
### <a name="azure-blobstore-for-cloud-foundry-cloud-controller-blobstore"></a>Azure Blobarchief voor Cloud Foundry Cloud Controller blobarchief
De Cloud Controller blobarchief is een kritieke gegevensopslag voor buildpacks, druppels, pakketten en -resourcegroepen. NFS-server wordt standaard gebruikt voor Cloud-Controller blobarchief. Om te voorkomen storingspunt, gebruikt u Azure Blob Storage als externe winkel. Bekijk de [Cloud Foundry documentatie](https://docs.cloudfoundry.org/deploying/common/cc-blobstore-config.html) voor de achtergrond en [opties in belangrijke Cloud Foundry](https://docs.pivotal.io/pivotalcf/2-0/customizing/azure.html).

### <a name="mysqlpostgresql-as-cloud-foundry-elastic-run-time-database-"></a>MySQL/PostgreSQL als Cloud Foundry elastische Database tijd uitgevoerd *
Elastische Runtime CF vereist twee belangrijke systeemdatabases:
#### <a name="ccdb"></a>CCDB 
De Cloud-Controller-database.  Cloud-Controller biedt REST-API-eindpunten voor clients toegang tot het systeem. CCDB slaat de tabellen voor organisaties, spaties, services, gebruikersrollen en meer Cloud-controller.
#### <a name="uaadb"></a>UAADB 
De database voor de verificatie- en gebruikersaccount. De gebruiker die verificatie gerelateerde worden opgeslagen gegevens, bijvoorbeeld versleuteld gebruikersnamen en wachtwoorden.

Standaard kan een lokaal systeem-database (MySQL) worden gebruikt. Voor HA en schaal, hefboomwerking Azure beheerde MySQL of PostgreSQL-services. Hier volgt de instructies van [Azure MySQL/PostgreSQL inschakelen voor CCDB, UAADB en andere systeemdatabases met Open Source Cloud Foundry](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/configure-cf-external-databases-using-azure-mysql-postgres-service).

## <a name="5-open-service-broker"></a>5. De geopende Service Broker
Azure-service broker biedt een consistente interface voor het beheren van de toepassing toegang tot Azure-services. De nieuwe [Open Service Broker voor Azure-project](https://github.com/Azure/open-service-broker-azure) biedt een enkel en eenvoudige manier om services te bieden tot toepassingen in de Cloud Foundry, OpenShift en Kubernetes. Zie de [Azure Open Service Broker voor PCF tegel](https://network.pivotal.io/products/azure-open-service-broker-pcf/) voor implementatie-instructies voor het PCF.

## <a name="6-metrics-and-logging"></a>6. Metrische gegevens en logboekregistratie
De pijp Azure Log Analytics is een onderdeel Cloud Foundry, stuurt de metrische gegevens van de [Cloud Foundry loggregator firehose](https://docs.cloudfoundry.org/loggregator/architecture.html) naar [Azure Log Analytics](https://azure.microsoft.com/services/log-analytics/). Met de pijp kunt u verzamelen, weergeven en analyseren van uw CF system status en prestaties van metrische gegevens met meerdere implementaties.
Klik op [hier](https://docs.microsoft.com/azure/cloudfoundry/cloudfoundry-oms-nozzle) voor informatie over het implementeren van de pijp Azure Log Analytics voor Open Source- en belangrijke Cloud Foundry-omgeving en vervolgens toegang tot de gegevens vanuit de Azure Log Analytics OMS-console. 
> [!NOTE]
> Vanaf PCF 2.0, BOSH health metrische gegevens voor virtuele machines worden doorgestuurd naar de Loggregator Firehose standaard en zijn geïntegreerd in de Azure Log Analytics OMS-console.

## <a name="7-cost-saving"></a>7. Kostenbesparend
### <a name="cost-saving-for-devtest-environments"></a>Kostenbesparende voor omgevingen ontwikkelen en testen
#### <a name="b-series-"></a>B-serie: *
Hoewel dit zijn vaak F en VM D-reeks voor belangrijke Cloud Foundry-productieomgeving, wordt aangeraden de nieuwe 'burstable' [B-serie](https://azure.microsoft.com/blog/introducing-b-series-our-new-burstable-vm-size/) brengt nieuwe opties. De B-serie burstable VM's zijn ideaal voor workloads die niet moet de volledige prestaties van de CPU continu, zoals webservers, kleine databases en ontwikkeling en testen van omgevingen. Deze werkbelastingen hebben doorgaans burstable prestatie-eisen. Het is $0.012 per uur (B1) in vergelijking met $0,05 per uur (F1), raadpleegt u de volledige lijst met [VM-grootten](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general) en [prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) voor meer informatie. 
#### <a name="managed-standard-disk"></a>Standard-beheerde schijven: 
Premium-schijven zijn aanbevolen voor betrouwbare prestaties in productie.  Met [beheerd schijf](https://azure.microsoft.com/services/managed-disks/), standard-opslag kan ook vergelijkbare betrouwbaarheid, met verschillende prestaties leveren. Voor de werkbelasting die geen prestatie-intensieve, zoals ontwikkelen en testen of niet-kritieke omgeving bieden beheerde standaardschijven een alternatief met lagere kosten.  
### <a name="cost-saving-in-general"></a>Opslaan in het algemeen kosten 
#### <a name="significant-vm-cost-saving-with-reserved-instances"></a>Aanzienlijke VM kostenbesparende met gereserveerde exemplaren: 
Vandaag die alle CF VM's met 'on-demand' prijzen, hoewel de omgevingen doorgaans voor onbepaalde tijd blijven worden gefactureerd. U kunt nu het reserveren van VM-capaciteit op een term 1 of 3 jaar en kortingen van 45 65% krijgen. Kortingen zijn toegepast in het factureringssysteem, zonder wijzigingen in uw omgeving. Zie voor meer informatie [hoe gereserveerde exemplaren works](https://azure.microsoft.com/pricing/reserved-vm-instances/). 
#### <a name="managed-premium-disk-with-smaller-sizes"></a>Premium-schijf met kleinere beheerd: 
Schijven ondersteuning kleinere schijf beschikken, bijvoorbeeld P4(32 GB) en P6(64 GB) voor premium en standard schijven beheerd. Als u een kleine werkbelastingen hebt, kunt u kosten opslaan bij het migreren van standaard premium-schijven aan beheerde premium-schijven.
#### <a name="utilizing-azure-first-party-services"></a>Met behulp van Azure Services voor de eerste partij: 
Profiteren van de eerste party-service van Azure verlaagt de lange termijn beheer van kosten, naast de HA en betrouwbaarheid bovenstaande secties. 

Belangrijke heeft gestart een [kleine Footprint invoegen](https://docs.pivotal.io/pivotalcf/2-0/customizing/small-footprint.html) voor PCF klanten, de onderdelen zich bevindt in slechts 4 virtuele machines, met maximaal 2500 exemplaren van een toepassing. De evaluatieversie is nu beschikbaar via [Azure-marktplaats](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

## <a name="next-steps"></a>Volgende stappen
Integratie van Azure-functies zijn eerst beschikbaar met [Open Source Cloud Foundry](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/), voordat deze beschikbaar op belangrijke Cloud Foundry is. Functies die zijn gemarkeerd met * zijn nog niet beschikbaar is via PCF. Cloud Foundry-integratie met Azure-Stack is niet ofwel in dit document behandeld.
Voor PCF ondersteuning op de functies die zijn gemarkeerd met *, of Cloud Foundry-integratie met Azure-Stack contact opnemen met uw accountmanager Pivotal en Microsoft voor de meest recente status. 

