---
title: Azure-abonnementslimieten en quota
description: Geeft een lijst van algemene Azure-abonnement en Servicelimieten, quotums en beperkingen. Dit omvat informatie over het limieten, samen met de maximumwaarden verhogen.
services: multiple
author: rothja
manager: jeffreyg
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.topic: article
ms.date: 09/11/2018
ms.author: byvinyal
ms.openlocfilehash: efaca1b9506ada02c52ad0833139d3b4e441d456
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393610"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure-abonnement en servicelimieten, quota's en beperkingen
In dit document vindt u enkele van de meest algemene Microsoft Azure-limieten, die soms ook wel quotums genoemd. Dit document niet op dit moment betrekking hebben op alle Azure-services. Na verloop van tijd, wordt de lijst worden uitgebreid en bijgewerkt in verband met betrekking hebben op meer van het platform.

Ga naar [overzicht van Azure-prijzen](https://azure.microsoft.com/pricing/) voor meer informatie over Azure-prijzen. Daar kunt u uw kosten met behulp van schat de [Prijscalculator](https://azure.microsoft.com/pricing/calculator/) of door naar de pagina met prijsdetails voor een service te gaan (bijvoorbeeld [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)). Zie voor meer tips voor het beheer van uw kosten [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](billing/billing-getting-started.md).

> [!NOTE]
> Als u wilt de is besteed of wanneer het quotum is voorgaande verhogen de **limiet standaard**, [opent u een ondersteuningsaanvraag online klant gratis](azure-resource-manager/resource-manager-quota-errors.md). De limieten kunnen niet worden verhoogd boven de **maximumlimiet** waarde die wordt weergegeven in de volgende tabellen. Als er geen **maximumlimiet** kolom en vervolgens de resource geen instelbare limieten.
>
> [Gratis proefabonnementen](https://azure.microsoft.com/offers/ms-azr-0044p) komen niet in aanmerking voor is besteed of wanneer het quotum toeneemt. Als u hebt een [gratis proefabonnement](https://azure.microsoft.com/offers/ms-azr-0044p), kunt u upgraden naar een [betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) abonnement. Zie voor meer informatie, [Azure-proefabonnement upgraden naar betalen per gebruik](billing/billing-upgrade-azure-subscription.md) en [gratis proefabonnement Veelgestelde vragen over](https://azure.microsoft.com/free/free-account-faq).
>

## <a name="limits-and-the-azure-resource-manager"></a>Limieten en de Azure Resource Manager
Het is nu mogelijk om te combineren meerdere Azure-resources in een enkel Azure-resourcegroep. Bij het gebruik van resourcegroepen, wordt één keer algemene zijn limieten op het niveau van een regionale met de Azure Resource Manager beheerd. Zie voor meer informatie over Azure-resourcegroepen [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md).

In de onderstaande grenzen, is een nieuwe tabel toegevoegd om weer te geven van eventuele verschillen in limieten bij het gebruik van Azure Resource Manager. Bijvoorbeeld, er is een **Abonnementslimieten** tabel en een **Abonnementslimieten - Azure Resource Manager** tabel. Wanneer een limiet van toepassing op beide scenario's, wordt het alleen weergegeven in de eerste tabel. Tenzij anders vermeld, gelden limieten in alle regio's.

> [!NOTE]
> Het is belangrijk om te benadrukken dat quota voor resources in Azure-resourcegroepen per regio toegankelijk is voor uw abonnement zijn, en niet per abonnement, omdat de service management-quota zijn. We gaan vCPU-quota gebruiken als voorbeeld. Als u nodig hebt om aan te vragen een quotaverhoging met ondersteuning voor vcpu's, moet u het aantal vcpu's die u wilt gebruiken in welke regio's en breng een specifieke aanvraag voor Azure-resourcegroep vCPU-quota voor de bedragen en regio's die u wilt bepalen. Als u 30 vcpu's in West-Europa er voor uw toepassing gebruiken wilt, moet u daarom specifiek 30 vcpu's in West-Europa aanvragen. Maar hebt u geen een vCPU-quotum verhogen in een andere regio--alleen West-Europa heeft de 30-vCPU-quotum.
> <!-- -->
> U kunt als gevolg hiervan vindt u het handig om te overwegen beslist wat uw Azure-resourcegroep quota's nodig hebt voor uw workload in elke regio een en bedrag in elke regio waarin u implementatie overweegt aanvragen. Zie [het oplossen van implementatieproblemen](resource-manager-common-deployment-errors.md) voor meer hulp bij het detecteren van uw huidige quota voor specifieke regio's.
>
>

## <a name="service-specific-limits"></a>Limieten voor specifieke services
* [Active Directory](#active-directory-limits)
* [API Management](#api-management-limits)
* [App Service](#app-service-limits)
* [Application Gateway](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Automatisering](#automation-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [Azure Database for MySQL](#azure-database-for-mysql)
* [Azure Database for PostgreSQL](#azure-database-for-postgresql)
* [Azure Event Grid](#azure-event-grid-limits)
* [Azure Maps](#azure-maps-limits)
* [Azure Monitor](#monitor-limits)
* [Azure Policy](#azure-policy-limits)
* [Azure Redis-cache](#azure-redis-cache-limits)
* [Een back-up maken](#backup-limits)
* [Batch](#batch-limits)
* [Batch AI](#batch-ai-limits)
* [BizTalk Services](#biztalk-services-limits)
* [CDN](#cdn-limits)
* [Cloud Services](#cloud-services-limits)
* [Container Instances](#container-instances-limits)
* [Container Registry](#container-registry-limits)
* [Kubernetes-Service](#kubernetes-service-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [Database migratieservice](#database-migration-service-limits)
* [DNS](#dns-limits)
* [Event Hubs](#event-hubs-limits)
* [Firewall van Azure](#azure-firewall-limits)
* [Voordeur](#azure-front-door-service-limits)
* [IoT Hub](#iot-hub-limits)
* [IoT Hub Device Provisioning Service](#iot-hub-device-provisioning-service-limits)
* [Key Vault](#key-vault-limits)
* [Log Analytics](#log-analytics-limits)
* [Beheerde identiteit](#managed-identity-limits)
* [Media Services](#media-services-limits)
* [Mobile Engagement](#mobile-engagement-limits)
* [Mobile Services](#mobile-services-limits)
* [Multi-Factor Authentication](#multi-factor-authentication)
* [Netwerken](#networking-limits)
* [Network Watcher](#network-watcher-limits)
* [Notification Hub-Service](#notification-hub-service-limits)
* [Resourcegroep](#resource-group-limits)
* [Op rollen gebaseerd toegangsbeheer](#role-based-access-control-limits)
* [Scheduler](#scheduler-limits)
* [Zoeken](#search-limits)
* [Service Bus](#service-bus-limits)
* [SignalR Service](#signalr-service-limits)
* [Site Recovery](#site-recovery-limits)
* [SQL Database](#sql-database-limits)
* [SQL Data Warehouse](#sql-data-warehouse-limits)
* [Storage](#storage-limits)
* [StorSimple-systeem](#storsimple-system-limits)
* [Stream Analytics](#stream-analytics-limits)
* [Abonnement](#subscription-limits)
* [Traffic Manager](#traffic-manager-limits)
* [Virtuele machines](#virtual-machines-limits)
* [Virtual Machine Scale Sets](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Abonnementslimieten
#### <a name="subscription-limits"></a>Abonnementslimieten
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Limieten voor een abonnement - Azure Resource Manager
De volgende beperkingen gelden bij het gebruik van de Azure Resource Manager en Azure-resourcegroepen. Grenzen die niet zijn gewijzigd met de Azure Resource Manager worden hieronder niet wordt vermeld. Raadpleeg de vorige tabel voor deze limieten.

Zie voor meer informatie over het verwerken van beperkingen met betrekking tot de Resource Manager-aanvragen [beperking Resource Manager-aanvragen](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Limieten voor resourcegroep
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Limieten voor virtuele Machines
#### <a name="virtual-machine-limits"></a>Limieten van de virtuele Machine
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Virtuele Machines - limieten voor Azure Resource Manager
De volgende beperkingen gelden bij het gebruik van de Azure Resource Manager en Azure-resourcegroepen. Grenzen die niet zijn gewijzigd met de Azure Resource Manager worden hieronder niet wordt vermeld. Raadpleeg de vorige tabel voor deze limieten.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Limieten voor Virtual Machine Scale Sets
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Container Instances-limieten
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="container-registry-limits"></a>Container Registry beperkt
De volgende tabel worden de functies en limieten van de Basic, Standard en Premium [Servicelagen](./container-registry/container-registry-skus.md).

[!INCLUDE [container-registry-limits](../includes/container-registry-limits.md)]

### <a name="kubernetes-service-limits"></a>Kubernetes-Servicelimieten
[!INCLUDE [container-service-limits](../includes/container-service-limits.md)]

### <a name="networking-limits"></a>Netwerklimieten
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Netwerklimieten
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Application Gateway-limieten
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Network Watcher beperkt
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>Limieten voor Traffic Manager
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-limieten
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

#### <a name="azure-firewall-limits"></a>Limieten voor Azure Firewall
[!INCLUDE [azure-firewall-limits](../includes/firewall-limits.md)]

#### <a name="azure-front-door-service-limits"></a>Azure voordeur Servicelimieten
[!INCLUDE [azure-front-door-service-limits](../includes/front-door-limits.md)]

### <a name="storage-limits"></a>Maximale opslag
<!--like # storage accts -->
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

Zie voor meer informatie over opslagaccountlimieten [Azure Storage Scalability and Performance Targets](storage/common/storage-scalability-targets.md).

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

Zie [grootten van virtuele machines](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie.

#### <a name="managed-virtual-machine-disks"></a>Beheerde virtuele machine-schijven

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Niet-beheerde VM-schijven

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="cloud-services-limits"></a>Limieten voor cloud Services
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>Limieten voor App Service
De volgende App Service-limieten zijn limieten voor Web-Apps, Mobile Apps en API Apps.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler-limieten
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Limieten voor batch
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="batch-ai-limits"></a>Batch AI-limieten
[!INCLUDE [azure-batch-ai-limits](../includes/azure-batch-ai-limits.md)]

### <a name="biztalk-services-limits"></a>Limieten voor BizTalk Services
De volgende tabel bevat de limieten voor Azure Biztalk Services.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB-limieten
Azure Cosmos DB is een wereldwijde schaal-database waarin doorvoer en opslag kunnen worden geschaald voor het afhandelen van wat er in uw toepassing vereist. Als u vragen hebt over de schaal Azure Cosmos DB biedt hebt, stuurt u e-mail naar askcosmosdb@microsoft.com.

### <a name="azure-database-for-mysql"></a>Azure Database for MySQL
Zie voor Azure Database for MySQL-limieten, [beperkingen in Azure Database voor MySQL](mysql/concepts-limits.md).

### <a name="azure-database-for-postgresql"></a>Azure Database for PostgreSQL
Zie voor Azure Database for PostgreSQL-limieten, [beperkingen in Azure Database for PostgreSQL](postgresql/concepts-limits.md).

### <a name="search-limits"></a>Limieten voor zoeken
Prijscategorieën bepalen de capaciteit en de grenzen van uw search-service. Lagen zijn onder andere:

* *Gratis* service met meerdere tenants, gedeeld met andere Azure-abonnees die bestemd zijn voor evaluatie en de ontwikkeling van kleine projecten.
* *Basic* biedt speciale computerbronnen voor productieworkloads op kleinere schaal, met maximaal drie replica's voor maximaal beschikbare querywerkbelastingen.
* *Standard (S1, S2, S3, S3 High Density)* is voor grotere werkbelastingen voor productie. Meerdere niveaus bestaan binnen de standard-laag, zodat u kunt de configuratie van een bron die het beste uw workload-profiel.

**Limieten per abonnement**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Limieten per zoekservice**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Zie voor meer informatie over limieten voor een meer gedetailleerd niveau, zoals documentgrootte, query's per seconde, sleutels, aanvragen en antwoorden, [Servicelimieten in Azure Search](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services-limieten
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN-limieten
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Limieten voor Mobile Services
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>Monitor-limieten
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Notification Hub-Servicelimieten
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

### <a name="azure-event-grid-limits"></a>Azure Event Grid-limieten
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

### <a name="signalr-service-limits"></a>Limieten voor SignalR-Service
[!INCLUDE [signalr-service-limits](../includes/signalr-service-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery-limieten
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights-limieten
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management-limieten
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Limieten voor Azure Redis-Cache
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Limieten voor Key Vault
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automation-limieten
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="managed-identity-limits"></a>Limieten voor beheerde identiteit
[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

### <a name="role-based-access-control-limits"></a>Op rollen gebaseerd beheer toegangsbeperkingen
[!INCLUDE [role-based-access-control-limits](../includes/role-based-access-control-limits.md)]

### <a name="sql-database-limits"></a>Limieten voor SQL-Database
Zie voor SQL Database-limieten, [SQL Database Resource Limits voor individuele databases](sql-database/sql-database-vcore-resource-limits-single-databases.md) en [SQL Database Resource Limits voor elastische pools en gepoolde databases](sql-database/sql-database-vcore-resource-limits-elastic-pools.md).

### <a name="sql-data-warehouse-limits"></a>SQL Data Warehouse-limieten
Zie voor SQL Data Warehouse-limieten, [Resourcelimieten voor SQL Data Warehouse](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md).

## <a name="see-also"></a>Zie ook
[Inzicht krijgen in Azure-limieten en toeneemt](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtual Machine and Cloud Service Sizes for Azure](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Groottes voor Cloudservices](cloud-services/cloud-services-sizes-specs.md)
