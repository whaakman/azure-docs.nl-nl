---
title: Hoe Cloud Foundry kan worden geïntegreerd met Azure | Microsoft Docs
description: Hierin wordt beschreven hoe Azure-services ter verbetering van de ervaring Enterprice kunnen gebruikmaken van Cloud Foundry
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
ms.openlocfilehash: 908b7e40c0509d7034b86985ac0775635726a6b9
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54329800"
---
# <a name="integrate-cloud-foundry-with-azure"></a>Cloud Foundry integreren met Azure

[Cloud Foundry](https://docs.cloudfoundry.org/) is een PaaS-platform uitgevoerd boven op cloudproviders IaaS-platform. Deze biedt toepassingen op een consistente implementatie-ervaring in de cloudproviders. Bovendien kan het ook integreren met verschillende Azure-services, met geavanceerde HA, schaalbaarheid en kostenbesparingen.
Er zijn [6 subsystemen van de Cloud Foundry](https://docs.cloudfoundry.org/concepts/architecture/), die kan worden flexibel schalen online, met inbegrip van: Routering, verificatie, beheer van de levenscyclus van toepassingen, Service management, berichten en bewaking. U kunt Cloud Foundry voor gebruik van geadresseerde Azure-service configureren voor elk van de subsystemen. 

![Cloud Foundry op Azure-integratie-architectuur](media/CFOnAzureEcosystem-colored.png)

## <a name="1-high-availability-and-scalability"></a>1. Hoge beschikbaarheid en schaalbaarheid
### <a name="managed-disk"></a>Managed Disk
Bosh maakt gebruik van Azure KPI (Cloud Provider Interface) voor het maken van de schijf en routines verwijderen. Standaard, worden niet-beheerde schijven gebruikt. De klant handmatig maken van de storage-accounts en vervolgens configureert u de accounts in CF manifestbestanden is vereist. Dit komt door de beperking op het aantal schijven per opslagaccount gebruikt.
Nu [Managed Disk](https://azure.microsoft.com/services/managed-disks/) beschikbaar is, biedt een beheerde schijf van beveiligde en betrouwbare opslag voor virtuele machines. De klant is niet meer nodig hebt om op te lossen met de storage-account voor schaalbaarheid en HA. Azure worden de schijven automatisch gerangschikt. Of het nu een nieuwe of een bestaande implementatie, wordt de KPI Azure het maken of de migratie van de beheerde schijf tijdens de implementatie van een CF verwerken. Met PCF 1.11 wordt ondersteund. U kunt ook de open-source Cloud Foundry verkennen [Managed Disk-richtlijnen](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/managed-disks) ter referentie. 
### <a name="availability-zone-"></a>Binnen een Beschikbaarheidszone *
Als een toepassingsplatform van cloud-native Cloud Foundry is ontworpen met [vier niveau van hoge beschikbaarheid](https://docs.pivotal.io/pivotalcf/2-1/concepts/high-availability.html). Tijdens de eerste drie niveaus van softwarefouten kunnen worden verwerkt door CF-systeem zelf wordt fouttolerantie platform wordt geleverd door cloudproviders. De hoofdonderdelen van CF moeten worden beveiligd met een van de cloudprovider platform HA-oplossing. Dit omvat GoRouters, Diego brein achter, CF-database en service-tegels. Standaard [Azure-Beschikbaarheidsset](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/deploy-cloudfoundry-with-availability-sets) wordt gebruikt voor fouttolerantie tussen clusters in een datacenter.
Het nieuwe goed is, [Azure binnen een Beschikbaarheidszone](https://docs.microsoft.com/azure/availability-zones/az-overview ) is nu vrijgegeven de fouttolerantie naar het volgende niveau te brengen met lage latentie redundantie in datacenters.
Azure-Beschikbaarheidszone HA bereikt door het plaatsen van een set van virtuele machines in 2 + datacenters, elke set van virtuele machines zijn redundante op andere sets. Als één Zone niet actief is, de andere sets zijn nog steeds live, geïsoleerd van de na noodgevallen.
> [!NOTE] 
> Azure binnen een Beschikbaarheidszone is niet beschikbaar in alle regio's nog, controleert u de meest recente [aankondiging voor een lijst van ondersteunde regio's](https://docs.microsoft.com/azure/availability-zones/az-overview). Controleer voor Open Source Cloud Foundry, [Azure binnen een Beschikbaarheidszone voor open-source Cloud Foundry richtlijnen](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/availability-zone).

## <a name="2-network-routing"></a>2. De routering
Azure basic load balancer wordt standaard gebruikt voor inkomende CF API/apps-aanvragen, stuurt deze door naar de Gorouters. CF-onderdelen, zoals Diego brein, MySQL, ERT kunt ook de load balancer gebruiken de om verkeer te verdelen voor hoge beschikbaarheid. Azure biedt bovendien een set van volledig beheerde oplossingen voor taakverdeling. Als u voor beëindiging van TLS ('SSL-offload') of per toepassing HTTP/HTTPS-laag aanvraagverwerking zoeken wilt, kunt u de Application Gateway. Voor hoge beschikbaarheid en schaalbaarheid voor taakverdeling van laag 4, kunt u de standaardversie van load balancer.
### <a name="azure-application-gateway-"></a>Azure Application Gateway *
[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) van laag 7 voor taakverdeling mogelijkheden, zoals het SSL-offloading, end-to-end SSL, Web Application Firewall, cookies gebaseerde sessieaffiniteit en meer biedt. U kunt [configureren van Application Gateway in Open Source Cloud Foundry](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/application-gateway). PCF, controleert u de [opmerkingen bij de release van PCF 2.1](https://docs.pivotal.io/pivotalcf/2-1/pcf-release-notes/opsmanager-rn.html#azure-application-gateway) voor POC-test.

### <a name="azure-standard-load-balancer-"></a>Azure Standard Load Balancer *
Azure Load Balancer is een Layer 4 load balancer. Het wordt gebruikt voor het distribueren van het verkeer tussen exemplaren van services in een load balancer-set. De standard-versie biedt [geavanceerde functies](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) boven op de basic-versie. Voorbeeld 1. De maximale limiet voor back-end-pool is verhoogd van 100 tot 1000 virtuele machines.  2. De eindpunten bieden nu ondersteuning voor meerdere beschikbaarheidssets in plaats van één beschikbaarheidsset.  3. Aanvullende functies zoals HA-poorten, uitgebreidere bewakingsgegevens, enzovoort. Als u naar Azure binnen een Beschikbaarheidszone verplaatsen wilt, is standaardversie van load balancer vereist. Voor een nieuwe implementatie raden we u om te starten met Azure Standard Load Balancer. 

## <a name="3-authentication"></a>3. Verificatie 
[Cloud Foundry-gebruikersaccount en verificatie](https://docs.cloudfoundry.org/concepts/architecture/uaa.html) is de centrale identity management-service voor CF en de verschillende onderdelen. [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is van Microsoft met meerdere tenants, cloudgebaseerde directory service- en identiteit. UAA wordt standaard gebruikt voor verificatie van de Cloud Foundry. Als een geavanceerde optie ondersteuning UAA ook voor Azure AD als een archief van de externe gebruiker. Azure AD-gebruikers hebben toegang tot Cloud Foundry met hun identiteit LDAP, zonder Cloud Foundry-account. Volg deze stappen om [configureren van de Azure AD voor UAA in PCF](http://docs.pivotal.io/p-identity/1-6/azure/index.html).

## <a name="4-data-storage-for-cloud-foundry-runtime-system"></a>4. Gegevensopslag voor Cloud Foundry Runtime-systeem
Cloud Foundry biedt uitstekende uitbreidbaarheid voor het gebruik van Azure-blobarchief of Azure MySQL/PostgreSQL-services voor opslag voor servertoepassingen runtime-systeem.
### <a name="azure-blobstore-for-cloud-foundry-cloud-controller-blobstore"></a>Azure-Blobarchief voor Cloud Foundry Cloud Controller blobarchief
De Cloud-Controller-blobarchief is een kritieke gegevensopslag voor buildpacks, druppels, pakketten en resourcegroepen. NFS-server wordt standaard gebruikt voor Cloud-Controller blobarchief. Om te voorkomen dat één storingspunt is, gebruikt u Azure Blob Storage als externe opslag. Bekijk de [documentatie voor Cloud Foundry](https://docs.cloudfoundry.org/deploying/common/cc-blobstore-config.html) voor achtergrondinformatie en [opties in Pivotal Cloud Foundry](https://docs.pivotal.io/pivotalcf/2-0/customizing/azure.html).

### <a name="mysqlpostgresql-as-cloud-foundry-elastic-run-time-database-"></a>MySQL/PostgreSQL als Cloud Foundry elastische uitvoeren tijd Database *
CF elastische Runtime zijn twee belangrijke systeemdatabases vereist:
#### <a name="ccdb"></a>CCDB 
De Cloud-Controller-database.  Cloud-Controller biedt REST API-eindpunten voor clients toegang tot het systeem. CCDB slaat de tabellen voor organisaties, spaties, services, gebruikersrollen en meer voor Cloud-controller.
#### <a name="uaadb"></a>UAADB 
De database voor de verificatie en -gebruikersaccount. De verificatie van de gebruiker die betrekking hebben opgeslagen gegevens, bijvoorbeeld versleuteld gebruikersnamen en wachtwoorden.

Standaard kan een lokaal systeem-database (MySQL) worden gebruikt. Voor hoge beschikbaarheid en schaal, maak gebruik van Azure beheerde MySQL of PostgreSQL-services. Hier volgt de instructies van [Azure MySQL/PostgreSQL inschakelen voor CCDB, UAADB en andere systeemdatabases met Open Source Cloud Foundry](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/configure-cf-external-databases-using-azure-mysql-postgres-service).

## <a name="5-open-service-broker"></a>5. Open Service Broker
Azure service broker biedt een consistente interface voor het beheren van de toepassing toegang tot Azure-services. De nieuwe [Open Service Broker for Azure-project](https://github.com/Azure/open-service-broker-azure) biedt een enkel en eenvoudige manier voor het leveren van services voor toepassingen voor de Cloud Foundry, OpenShift en Kubernetes. Zie de [Azure Open Service Broker voor PCF tegel](https://network.pivotal.io/products/azure-open-service-broker-pcf/) voor implementatie-instructies op PCF.

## <a name="6-metrics-and-logging"></a>6. Metrische gegevens en logboekregistratie
De Azure Log Analytics-pijp is een onderdeel Cloud Foundry, stuurt metrische gegevens van de [Cloud Foundry loggregator firehose](https://docs.cloudfoundry.org/loggregator/architecture.html) naar [Azure Log Analytics](https://azure.microsoft.com/services/log-analytics/). Met de Nozzle, kunt u verzamelen, weergeven en analyseren van uw CF system status en prestaties van metrische gegevens over meerdere implementaties.
Klik op [hier](https://docs.microsoft.com/azure/cloudfoundry/cloudfoundry-oms-nozzle) voor informatie over het implementeren van de Azure Log Analytics-pijp voor Open Source- en Pivotal Cloud Foundry-omgeving en vervolgens toegang tot de gegevens uit de Azure Log Analytics-console. 
> [!NOTE]
> PCF 2.0, metrische gegevens over gezondheid BOSH voor virtuele machines worden doorgestuurd naar de Loggregator Firehose standaard en zijn geïntegreerd in Azure Log Analytics-console.

## <a name="7-cost-saving"></a>7. Kosten opslaan
### <a name="cost-saving-for-devtest-environments"></a>Geld besparen voor Dev/Test-omgevingen
#### <a name="b-series-"></a>B-serie: *
Hoewel F en VM van de D-serie zijn vaak voor productie-omgeving van Pivotal Cloud Foundry, wordt aangeraden de nieuwe '' burstable '' [B-serie](https://azure.microsoft.com/blog/introducing-b-series-our-new-burstable-vm-size/) zorgt voor de nieuwe opties. De VM's met burstfunctie B-serie zijn ideaal voor workloads die niet continu de volledige prestaties van de CPU nodig hebt, zoals webservers, kleine databases en de ontwikkeling en testomgevingen. Deze werkbelastingen hebben meestal ' burstable ' prestatie-eisen. Deze $0.012/ uur (B1) in vergelijking met $0.05/ uur (F1) is, raadpleegt u de volledige lijst met [VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general) en [prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) voor meer informatie. 
#### <a name="managed-standard-disk"></a>Managed Standard Disk: 
Premium-schijven zijn aanbevolen voor betrouwbare prestaties in de productieomgeving.  Met [Managed Disk](https://azure.microsoft.com/services/managed-disks/), standard-opslag kan ook vergelijkbare betrouwbaarheid, met verschillende prestaties leveren. Voor de werkbelasting die geen prestatie-intensieve, zoals dev/Test- of niet-kritieke omgeving bieden beheerde standaardschijven een alternatief met lagere kosten.  
### <a name="cost-saving-in-general"></a>Kosten in het algemeen opslaan 
#### <a name="significant-vm-cost-saving-with-azure-reservations"></a>Belangrijke VM kosten opslaan met Azure-reserveringen: 
Vandaag die alle CF-VM's worden gefactureerd op basis van 'on-demand' prijzen, hoewel de omgevingen doorgaans voor onbepaalde tijd blijven. U kunt nu capaciteit van de virtuele machine op een term 1 of 3 jaar gereserveerd en 45 en 65% kortingen krijgen. Kortingen zijn toegepast in het factureringssysteem, zonder wijzigingen in uw omgeving. Zie voor meer informatie, [hoe Azure reserveringen werkt](https://azure.microsoft.com/pricing/reserved-vm-instances/). 
#### <a name="managed-premium-disk-with-smaller-sizes"></a>Beheerde Premium-schijf met kleinere: 
Beheerde schijven ondersteuning kleinere schijfgrootten bijvoorbeeld P4(32 GB) en P6(64 GB) voor premium en standard-schijven. Als u kleine workloads hebt, kunt u kosten kunt opslaan bij het migreren van standard premium-schijven naar beheerde premium-schijven.
#### <a name="utilizing-azure-first-party-services"></a>Met behulp van Azure Services voor de eerste partij: 
U profiteert van Azure de eigen service verlaagt de lange termijn beheer van kosten, naast de HA en -betrouwbaarheid de bovenstaande secties vermeld. 

Pivotal heeft gestart een [kleine Footprint ERT](https://docs.pivotal.io/pivotalcf/2-0/customizing/small-footprint.html) voor PCF klanten, de onderdelen zich bevindt in slechts 4 virtuele machines, met tot 2500 exemplaren van de toepassing. De proefversie is nu beschikbaar via [Azure-marktplaats](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

## <a name="next-steps"></a>Volgende stappen
Integratie van Azure-functies zijn eerst beschikbaar met [Open Source Cloud Foundry](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/), voordat deze beschikbaar op Pivotal Cloud Foundry is. Functies die zijn gemarkeerd met * zijn nog steeds niet beschikbaar is via PCF. Cloud Foundry-integratie met Azure Stack is niet de in dit document behandeld.
PCF-ondersteuning voor de functies die zijn gemarkeerd met *, of Cloud Foundry-integratie met Azure Stack, neem contact op met uw accountmanager Pivotal en Microsoft voor de meest recente status. 

