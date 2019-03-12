---
title: Azure-abonnementslimieten en quota
description: Geeft een lijst van algemene Azure-abonnement en Servicelimieten, quotums en beperkingen. In dit artikel bevat informatie over hoe u limieten, samen met de maximumwaarden verhogen.
services: multiple
author: rothja
manager: jeffreyg
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.topic: article
ms.date: 12/10/2018
ms.author: byvinyal
ms.openlocfilehash: 21a6734ab8af2e35643f6e9b25500dad2418a8c2
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57772032"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure-abonnement en servicelimieten, quota's en beperkingen
In dit document vindt u enkele van de meest algemene Microsoft Azure-limieten, die soms ook wel quotums genoemd. Dit document niet op dit moment betrekking hebben op alle Azure-services. Na verloop van tijd, wordt de lijst worden uitgebreid en bijgewerkt in verband met betrekking hebben op meer services.

Zie voor meer informatie over Azure-prijzen, [Azure Prijsoverzicht](https://azure.microsoft.com/pricing/). U schat uw kosten met behulp van de [prijscalculator](https://azure.microsoft.com/pricing/calculator/). Tevens gaat u naar de pagina met prijsdetails voor een bepaalde service, bijvoorbeeld [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). Zie voor meer tips voor het beheer van uw kosten [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing/billing-getting-started.md).

> [!NOTE]
> Als u wilt verhogen de is besteed of wanneer het quotum boven de standaardlimiet [opent u een ondersteuningsaanvraag online klant gratis](azure-resource-manager/resource-manager-quota-errors.md). De limieten kunnen niet boven de maximumlimiet-waarde die wordt weergegeven in de volgende tabellen worden verhoogd. Als er geen kolom maximumlimiet is, is de resource geen instelbare limieten.
>
> [Gratis proefabonnementen](https://azure.microsoft.com/offers/ms-azr-0044p) niet in aanmerking komt voor is besteed of wanneer het quotum toeneemt. Als u hebt een [gratis proefabonnement](https://azure.microsoft.com/offers/ms-azr-0044p), kunt u upgraden naar een [betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) abonnement. Zie voor meer informatie, [uw gratis proefversie van Azure-abonnement upgraden naar een betalen per gebruik-abonnement](billing/billing-upgrade-azure-subscription.md) en de [gratis proefabonnement Veelgestelde vragen over](https://azure.microsoft.com/free/free-account-faq).
>

## <a name="limits-and-azure-resource-manager"></a>Limieten en Azure Resource Manager
Het is nu mogelijk om te combineren meerdere Azure-resources in een enkel Azure-resourcegroep. Wanneer u resourcegroepen, wordt één keer algemene zijn limieten op het niveau van een regionale met Azure Resource Manager beheerd. Zie voor meer informatie over Azure-resourcegroepen, [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md).

In de volgende lijst met limieten bevat een nieuwe tabel de eventuele verschillen in limieten bij het gebruik van Azure Resource Manager. Bijvoorbeeld, er is een **abonnementslimieten** tabel en een **abonnementslimieten - Azure Resource Manager** tabel. Wanneer een limiet van toepassing op beide scenario's, wordt het alleen weergegeven in de eerste tabel. Tenzij anders vermeld, gelden limieten in alle regio's.

> [!NOTE]
> Quota voor resources in Azure-resourcegroepen zijn per regio toegankelijk is voor uw abonnement, niet per abonnement als de service management-quota. We gaan vCPU-quota gebruiken als voorbeeld. Om aan te vragen een quotaverhoging met ondersteuning voor vcpu's, moet u bepalen hoeveel vcpu's die u wilt gebruiken in welke regio's. U kunt vervolgens een specifieke aanvraag voor Azure resource group vCPU-quota maken voor de bedragen en regio's die u wilt. Als u 30 vcpu's in West-Europa er voor uw toepassing gebruiken wilt, beschikt u specifiek aanvragen 30 vcpu's in West-Europa. Uw vCPU-quotum niet wordt verhoogd in andere regio--alleen West-Europa de 30-vCPU-quotum heeft.
> <!-- -->
> Als gevolg hiervan kunt u bepalen wat uw Azure-resource group-quota's voor uw workload in elke regio een moeten worden. Vervolgens aanvragen die hoeveelheid in elke regio waarin u wilt implementeren. Zie voor hulp bij het bepalen van uw huidige quota voor specifieke regio's, [probleemoplossing voor implementatieproblemen](resource-manager-common-deployment-errors.md).
>
>

## <a name="service-specific-limits"></a>Limieten voor specifieke services
* [Active Directory](#active-directory-limits)
* [API Management](#api-management-limits)
* [App Service](#app-service-limits)
* [Application Gateway](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Automation](#automation-limits)
* [Azure Cache voor Redis](#azure-cache-for-redis-limits)
* [Azure Cloud Services](#azure-cloud-services-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [Azure Database for MySQL](#azure-database-for-mysql)
* [Azure Database for PostgreSQL](#azure-database-for-postgresql)
* [Azure DNS](#azure-dns-limits)
* [Azure Firewall](#azure-firewall-limits)
* [Azure Kubernetes Service](#azure-kubernetes-service-limits)
* [Azure Maps](#azure-maps-limits)
* [Azure Monitor](#monitor-limits)
* [Azure Policy](#azure-policy-limits)
* [Azure Search](#azure-search-limits)
* [Azure SignalR Service](#azure-signalr-service-limits)
* [Een back-up maken](#backup-limits)
* [Batch](#batch-limits)
* [BizTalk Services](#biztalk-services-limits)
* [Container Instances](#container-instances-limits)
* [Container Registry](#container-registry-limits)
* [Content Delivery Network](#content-delivery-network-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [Database migratieservice](#database-migration-service-limits)
* [Event Grid](#event-grid-limits)
* [Event Hubs](#event-hubs-limits)
* [Voordeur Service](#azure-front-door-service-limits)
* [Identity Manager](#identity-manager-limits)
* [IoT Hub](#iot-hub-limits)
* [IoT Hub Device Provisioning Service](#iot-hub-device-provisioning-service-limits)
* [Key Vault](#key-vault-limits)
* [Log Analytics](#log-analytics-limits)
* [Media Services](#media-services-limits)
* [Mobile Services](#mobile-services-limits)
* [Multi-Factor Authentication](#multi-factor-authentication-limits)
* [Netwerken](#networking-limits)
* [Network Watcher](#network-watcher-limits)
* [Notification Hubs](#notification-hubs-limits)
* [Resourcegroep](#resource-group-limits)
* [Op rollen gebaseerd toegangsbeheer](#role-based-access-control-limits)
* [Scheduler](#scheduler-limits)
* [Service Bus](#service-bus-limits)
* [Site Recovery](#site-recovery-limits)
* [SQL Database](#sql-database-limits)
* [SQL Data Warehouse](#sql-data-warehouse-limits)
* [Storage](#storage-limits)
* [StorSimple System](#storsimple-system-limits)
* [Stream Analytics](#stream-analytics-limits)
* [Abonnement](#subscription-limits)
* [Traffic Manager](#traffic-manager-limits)
* [Virtuele machines](#virtual-machines-limits)
* [Virtuele-machineschaalsets](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Abonnementslimieten
#### <a name="subscription-limits---azure-service-management-classic-deployment-model"></a>Limieten voor een abonnement - Azure-servicebeheer (klassieke implementatiemodel)
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Limieten voor een abonnement - Azure Resource Manager
De volgende beperkingen gelden wanneer u Azure Resource Manager en Azure-resourcegroepen. Grenzen die nog niet hebt gewijzigd met Azure Resource Manager worden niet weergegeven. Zie de vorige tabel voor deze limieten.

Voor informatie over het Resource Manager-API lezen en schrijven van grenzen, Zie [beperking Resource Manager-aanvragen](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Grenzen van de resource
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Limieten voor virtuele Machines
#### <a name="virtual-machines-limits"></a>Limieten voor virtuele Machines
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Virtuele Machines - limieten voor Azure Resource Manager
De volgende beperkingen gelden wanneer u Azure Resource Manager en Azure-resourcegroepen. Grenzen die nog niet hebt gewijzigd met Azure Resource Manager worden niet weergegeven. Zie de vorige tabel voor deze limieten.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Limieten voor virtuele-machineschaalsets
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Container Instances-limieten
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="container-registry-limits"></a>Container Registry beperkt
De volgende tabel worden de functies en limieten van de Basic, Standard en Premium [Servicelagen](./container-registry/container-registry-skus.md).

[!INCLUDE [container-registry-limits](../includes/container-registry-limits.md)]

### <a name="azure-kubernetes-service-limits"></a>Limieten voor Azure Kubernetes Service
[!INCLUDE [container-service-limits](../includes/container-service-limits.md)]

### <a name="networking-limits"></a>Netwerklimieten
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Netwerklimieten
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Application Gateway-limieten

De volgende tabel is van toepassing op v1, v2, Standard en WAF-SKU's, tenzij anders vermeld.
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Network Watcher beperkt
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>Limieten voor Traffic Manager
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="azure-dns-limits"></a>Limieten voor Azure DNS
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

#### <a name="azure-firewall-limits"></a>Limieten voor Azure Firewall
[!INCLUDE [azure-firewall-limits](../includes/firewall-limits.md)]

#### <a name="azure-front-door-service-limits"></a>Azure voordeur Servicelimieten
[!INCLUDE [azure-front-door-service-limits](../includes/front-door-limits.md)]

### <a name="storage-limits"></a>Maximale opslag
<!--like # storage accts -->
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

Zie voor meer informatie over opslagaccountlimieten [schaalbaarheids- en prestatiedoelen voor Azure Storage](storage/common/storage-scalability-targets.md).

#### <a name="storage-resource-provider-limits"></a>Storage resource provider limieten 

[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

#### <a name="azure-blob-storage-limits"></a>Azure Blob storage-limieten
[!INCLUDE [storage-blob-scale-targets](../includes/storage-blob-scale-targets.md)]

#### <a name="azure-files-limits"></a>Limieten voor Azure bestanden
Zie voor meer informatie over limieten voor Azure Files [schaalbaarheids- en prestatiedoelen van Azure Files](storage/files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../includes/storage-files-scale-targets.md)]

#### <a name="azure-file-sync-limits"></a>Azure File Sync-limieten
[!INCLUDE [storage-sync-files-scale-targets](../includes/storage-sync-files-scale-targets.md)]

#### <a name="azure-queue-storage-limits"></a>Limieten voor Azure Queue storage
[!INCLUDE [storage-queues-scale-targets](../includes/storage-queues-scale-targets.md)]

#### <a name="azure-table-storage-limits"></a>Limieten voor Azure Table storage
[!INCLUDE [storage-tables-scale-targets](../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>Schijflimieten voor virtuele machine
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Zie voor meer informatie, [grootten van virtuele machines](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

#### <a name="managed-virtual-machine-disks"></a>Beheerde virtuele machine-schijven

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Niet-beheerde VM-schijven

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="azure-cloud-services-limits"></a>Limieten voor Azure Cloud Services
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>Limieten voor App Service
De volgende App Service-limieten zijn limieten voor Web-Apps, Mobile Apps en API Apps.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler-limieten
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Limieten voor batch
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>Limieten voor BizTalk Services
De volgende tabel bevat de limieten voor Azure BizTalk Services.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB-limieten
Azure Cosmos DB is een wereldwijde schaal-database waarin doorvoer en opslag kunnen worden geschaald voor het afhandelen van wat er in uw toepassing vereist. Als u vragen hebt over de schaal Azure Cosmos DB biedt hebt, stuurt u e-mail naar askcosmosdb@microsoft.com.

### <a name="azure-database-for-mysql"></a>Azure Database for MySQL
Zie voor Azure Database for MySQL-limieten, [beperkingen in Azure Database voor MySQL](mysql/concepts-limits.md).

### <a name="azure-database-for-postgresql"></a>Azure Database for PostgreSQL
Zie voor Azure Database for PostgreSQL-limieten, [beperkingen in Azure Database for PostgreSQL](postgresql/concepts-limits.md).

### <a name="azure-search-limits"></a>Limieten voor Azure Search
Prijscategorieën bepalen de capaciteit en de grenzen van uw search-service. Lagen zijn onder andere:

* **Gratis** multitenant-service, gedeeld met andere Azure-abonnees is bedoeld voor evaluatie en kleine ontwikkelingsprojecten.
* **Basic** biedt speciale computerbronnen voor productieworkloads op kleinere schaal, met maximaal drie replica's voor maximaal beschikbare querywerkbelastingen.
* **Standard**, waaronder S1, S2 en S3 en S3 High Density, is voor grotere productieworkloads. Meerdere niveaus bestaan binnen de Standard-laag, zodat u kunt de configuratie van een bron die het beste uw workload-profiel.

**Limieten per abonnement**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Limieten per zoekservice**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Zie voor meer informatie over limieten voor een meer gedetailleerd niveau, zoals documentgrootte, query's per seconde, sleutels, aanvragen en antwoorden, [Servicelimieten in Azure Search](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services-limieten
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="content-delivery-network-limits"></a>Content Delivery Network-limieten
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Limieten voor Mobile Services
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>Monitor-limieten
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hubs-limits"></a>Limieten voor Notification Hubs
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Event Hubs-limieten
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Service Bus-limieten
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT Hub-limieten
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="iot-hub-device-provisioning-service-limits"></a>Limieten voor IoT Hub Device Provisioning Service
[!INCLUDE [azure-iotdps-limits](../includes/iot-dps-limits.md)]

### <a name="data-factory-limits"></a>Data Factory-limieten
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Data Lake Analytics-limieten
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Data Lake Store-limieten
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="database-migration-service-limits"></a>Hiermee beperkt u database migratieservice
[!INCLUDE [database-migration-service-limits](../includes/database-migration-service-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics-limieten
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory-limieten
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="event-grid-limits"></a>Limieten Event Grid
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-maps-limits"></a>Azure Maps-limieten
[!INCLUDE [maps-limits](../includes/maps-limits.md)]

### <a name="azure-policy-limits"></a>Limieten voor Azure Policy
[!INCLUDE [policy-limits](../includes/azure-policy-limits.md)]

### <a name="storsimple-system-limits"></a>Limieten voor StorSimple-systeem
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>Hiermee beperkt u log Analytics
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Limieten voor back-up
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="azure-signalr-service-limits"></a>Limieten voor Azure SignalR Service
[!INCLUDE [signalr-service-limits](../includes/signalr-service-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery-limieten
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights-limieten
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management-limieten
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-cache-for-redis-limits"></a>Azure Cache voor Redis-limieten
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Limieten voor Key Vault
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication-limits"></a>Hiermee beperkt u multi-factor Authentication
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automation-limieten
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="identity-manager-limits"></a>Limieten voor Identity Manager
[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

### <a name="role-based-access-control-limits"></a>Op rollen gebaseerd beheer toegangsbeperkingen
[!INCLUDE [role-based-access-control-limits](../includes/role-based-access-control-limits.md)]

### <a name="sql-database-limits"></a>Limieten voor SQL-Database
Zie voor SQL Database-limieten, [SQL Database-resourcebeperkingen voor individuele databases](sql-database/sql-database-vcore-resource-limits-single-databases.md) en [SQL Database-resourcebeperkingen voor elastische pools en gepoolde databases](sql-database/sql-database-vcore-resource-limits-elastic-pools.md).

### <a name="sql-data-warehouse-limits"></a>SQL Data Warehouse-limieten
Zie voor SQL Data Warehouse-limieten, [resourcelimieten voor SQL Data Warehouse](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md).

## <a name="see-also"></a>Zie ook
- [Azure-limieten en verhoogt begrijpen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
- [Grootten voor virtuele machine en cloud-service voor Azure](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Grootten voor Azure Cloudservices](cloud-services/cloud-services-sizes-specs.md)
