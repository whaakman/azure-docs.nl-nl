---
title: Limieten voor Azure-abonnement en quota | Microsoft Docs
description: Geeft een lijst van algemene Azure-abonnement en Servicelimieten, quota's en beperkingen. Dit omvat informatie over het limieten samen met de maximumwaarden verhogen.
services: 
documentationcenter: 
author: rothja
manager: jeffreyg
editor: 
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: byvinyal
ms.openlocfilehash: f2c747f29d1e35b430b30d9aa4ca0bff2e78476f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure-abonnement en servicelimieten, quota's en beperkingen
Dit document vindt u enkele van de meest voorkomende Microsoft Azure limieten, quota's worden ook wel worden genoemd. Dit document betrekking niet op dit moment op alle Azure-services. Na verloop van tijd wordt de lijst uitgebreid en bijgewerkt ten aanzien van het platform.

Ga naar [overzicht van Azure prijzen](https://azure.microsoft.com/pricing/) voor meer informatie over prijzen voor Azure. Daar kunt u uw kosten met schatten de [Prijscalculator](https://azure.microsoft.com/pricing/calculator/) of via de detailpagina met prijzen voor een service (bijvoorbeeld [VM's van Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)). Zie voor meer tips voor het beheer van uw kosten [te voorkomen dat onverwachte kosten met Azure-facturering en kostenbeheer](billing/billing-getting-started.md).

> [!NOTE]
> Als u wilt de limiet of bovenstaande quotum verhogen de **standaard limiet**, [opent u een ondersteuningsaanvraag online klant kosteloos](azure-supportability/resource-manager-core-quotas-request.md). De limieten kunnen niet worden verhoogd boven de **maximumlimiet** waarde weergegeven in de volgende tabellen. Als er geen **maximumlimiet** kolom en vervolgens de resource heeft geen instelbare limieten.
>
> [Gratis proefversie van abonnementen](https://azure.microsoft.com/offers/ms-azr-0044p) zijn niet in aanmerking komen voor limiet of quotum toeneemt. Als u hebt een [gratis proefabonnement](https://azure.microsoft.com/offers/ms-azr-0044p), kunt u upgraden naar een [betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) abonnement. Zie voor meer informatie [Upgrade gratis proefversie van Azure naar betalen per gebruik](billing/billing-upgrade-azure-subscription.md) en [gratis proefabonnement Veelgestelde vragen over](https://azure.microsoft.com/free/free-account-faq).
>

## <a name="limits-and-the-azure-resource-manager"></a>Limieten en de Azure Resource Manager
Het is nu mogelijk om te combineren meerdere Azure-resources in aan één Azure-resourcegroep. Bij gebruik van resourcegroepen limieten die eenmaal globale zijn beheerd op een regionaal niveau met het Azure Resource Manager. Zie voor meer informatie over Azure-resourcegroepen [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md).

Een nieuwe tabel is in de onderstaande grenzen in overeenstemming met eventuele verschillen in limieten bij gebruik van Azure Resource Manager toegevoegd. Er is bijvoorbeeld een **abonnementen** tabel en een **abonnementen - Azure Resource Manager** tabel. Wanneer er een limiet voor beide scenario's geldt, is het alleen weergegeven in de eerste tabel. Tenzij anders vermeld, gelden limieten in alle regio's.

> [!NOTE]
> Het is belangrijk om te benadrukken dat quota's voor resources in Azure-resourcegroepen per regio toegankelijk zijn voor uw abonnement zijn, en niet per abonnement, omdat de service management-quota. Laten we vCPU quota gebruiken als voorbeeld. Als u aanvragen van een verhoging van het quotum met ondersteuning voor Vcpu alleen, moet u bepalen hoeveel vcpu's die u wilt gebruiken in welke regio's en vervolgens een specifieke aanvraag voor Azure-resourcegroep vCPU quota's voor de bedragen en regio's die u wilt maken. Als u 30 vcpu's in West-Europa gebruiken wilt voor het uitvoeren van uw toepassing bevat, moet u daarom specifiek 30 vcpu's in West-Europa aanvragen. Maar u geen een vCPU quotum verhogen in elke andere regio--alleen West-Europa heeft het quotum 30 vCPU.
> <!-- -->
> U kunt als gevolg hiervan handiger om u te overwegen beslist wat uw Azure-resourcegroep quota's nodig zijn voor uw workload in elke regio een, en dat bedrag in elke regio waarin u implementatie overweegt aanvragen. Zie [implementatieproblemen oplossen](resource-manager-common-deployment-errors.md) voor meer informatie voor het detecteren van uw huidige quota's voor specifieke regio's.
>
>

## <a name="service-specific-limits"></a>Servicespecifieke limieten
* [Active Directory](#active-directory-limits)
* [API Management](#api-management-limits)
* [App Service](#app-service-limits)
* [Application Gateway](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Automatisering](#automation-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [Azure Event Grid](#azure-event-grid-limits)
* [Azure Redis-cache](#azure-redis-cache-limits)
* [Een back-up maken](#backup-limits)
* [Batch](#batch-limits)
* [BizTalk Services](#biztalk-services-limits)
* [CDN](#cdn-limits)
* [Cloud Services](#cloud-services-limits)
* [Container Instances](#container-instances-limits)
* [Container Registry](#container-registry-limits)
* [Container Service (AKS)](#container-service-aks-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [Database-Service voor migratie](#database-migration-service-limits)
* [DNS](#dns-limits)
* [Event Hubs](#event-hubs-limits)
* [IoT Hub](#iot-hub-limits)
* [Service voor IoT Hub apparaat-inrichting](#iot-hub-device-provisioning-service-limits)
* [Key Vault](#key-vault-limits)
* [Meld u Analytics / Operational Insights](#log-analytics-limits)
* [Media Services](#media-services-limits)
* [Mobile Engagement](#mobile-engagement-limits)
* [Mobile Services](#mobile-services-limits)
* [Controle](#monitor-limits)
* [Multi-Factor Authentication](#multi-factor-authentication)
* [Netwerken](#networking-limits)
* [Network Watcher](#network-watcher-limits)
* [Notification Hub Service](#notification-hub-service-limits)
* [Resourcegroep](#resource-group-limits)
* [Scheduler](#scheduler-limits)
* [Zoeken](#search-limits)
* [Service Bus](#service-bus-limits)
* [Site Recovery](#site-recovery-limits)
* [SQL Database](#sql-database-limits)
* [SQL Data Warehouse](#sql-data-warehouse-limits)
* [Storage](#storage-limits)
* [StorSimple-systeem](#storsimple-system-limits)
* [Stream Analytics](#stream-analytics-limits)
* [Abonnement](#subscription-limits)
* [Traffic Manager](#traffic-manager-limits)
* [Virtuele machines](#virtual-machines-limits)
* [Virtuele-Machineschaalsets](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Abonnementen
#### <a name="subscription-limits"></a>Abonnementen
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Abonnement - limieten voor Azure Resource Manager
De volgende beperkingen gelden bij gebruik van de Azure Resource Manager en Azure-resourcegroepen. Limieten die niet zijn gewijzigd met de Azure Resource Manager worden hieronder niet weergegeven. Raadpleeg de vorige tabel voor deze limieten.

Zie voor meer informatie over het verwerken van limieten voor Resource Manager-verzoeken [beperking Resource Manager-aanvragen](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Limieten voor resourcegroep
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Limieten voor virtuele Machines
#### <a name="virtual-machine-limits"></a>Limieten voor de virtuele Machine
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Limieten voor virtuele Machines - Azure Resource Manager
De volgende beperkingen gelden bij gebruik van de Azure Resource Manager en Azure-resourcegroepen. Limieten die niet zijn gewijzigd met de Azure Resource Manager worden hieronder niet weergegeven. Raadpleeg de vorige tabel voor deze limieten.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Limieten voor virtuele-Machineschaalsets
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Container exemplaren limieten
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="container-registry-limits"></a>Container register limieten
De volgende tabel worden de functies en de ondergrenzen van de Basic, Standard en Premium [Servicelagen](./container-registry/container-registry-skus.md).

[!INCLUDE [container-registry-limits](../includes/container-registry-limits.md)]

### <a name="container-service-aks-limits"></a>Limieten voor container Service (AKS)
[!INCLUDE [container-service-limits](../includes/container-service-limits.md)]

### <a name="networking-limits"></a>Netwerklimieten
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Netwerklimieten
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Toepassingsgateway beperkt
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Grenzen van netwerk-Watcher
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>Traffic Manager-limieten
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-beperkingen
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Opslaglimieten
Zie voor meer informatie over opslagaccountlimieten [Azure Storage Scalability and Performance Targets](storage/common/storage-scalability-targets.md).

<!--like # storage accts -->
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

#### <a name="azure-blob-storage-limits"></a>Azure Blob storage-limieten
[!INCLUDE [storage-blob-scale-targets](../includes/storage-blob-scale-targets.md)]

#### <a name="azure-files-limits"></a>Azure bestanden limieten
Zie voor meer informatie over de limieten voor Azure Files [Azure Files schaalbaarheids- en prestatiedoelen](storage/files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../includes/storage-files-scale-targets.md)]

#### <a name="azure-file-sync-limits"></a>Azure File-Sync-limieten
[!INCLUDE [storage-sync-files-scale-targets](../includes/storage-sync-files-scale-targets.md)]

#### <a name="azure-queue-storage-limits"></a>Azure Queue storage limieten
[!INCLUDE [storage-queues-scale-targets](../includes/storage-queues-scale-targets.md)]

#### <a name="azure-table-storage-limits"></a>Azure Table storage limieten
[!INCLUDE [storage-tables-scale-targets](../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>Schijfruimtelimiet van de virtuele machine
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Zie [grootten van virtuele machines](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie.

#### <a name="managed-virtual-machine-disks"></a>Beheerde virtuele-machineschijven

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Niet-beheerde virtuele-machineschijven

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="cloud-services-limits"></a>Limieten voor cloud-Services
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>App Service-beperkingen
De volgende App Service-limieten omvatten limieten voor Web-Apps, Mobile Apps, API-Apps en Logic Apps.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler-limieten
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch-limieten
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>Limieten voor BizTalk Services
De volgende tabel bevat de limieten voor Azure Biztalk Services.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure DB Cosmos-limieten
Azure Cosmos-database is een wereldwijde schaal-database waarin doorvoer en opslag kunnen worden uitgebreid voor het afhandelen van wat er in uw toepassing vereist. Als u vragen over de schaal Azure Cosmos DB hebt biedt, stuur e-mail naar askcosmosdb@microsoft.com.

### <a name="mobile-engagement-limits"></a>Mobile Engagement-limieten
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a>Limieten voor zoeken
Prijscategorieën bepalen de capaciteit en de grenzen van uw zoekservice. Lagen zijn onder andere:

* *Gratis* multitenant-service, gedeeld met andere Azure-abonnees die zijn bestemd voor evaluatie en klein ontwikkeling projecten.
* *Basic* biedt speciale computerbronnen voor productieworkloads op kleinere schaal, met maximaal drie replica's voor query maximaal beschikbare werkbelastingen.
* *Standard (S1, S2, S3, S3 high-density)* voor productieworkloads groter wordt. Er bestaan meerdere niveaus binnen de prijscategorie standard zodat u kunt de configuratie van een bron die het meest geschikt is voor uw profiel werkbelasting.

**Limieten per abonnement**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Limieten per zoekservice**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Zie voor meer informatie over limieten voor een meer gedetailleerd niveau, zoals de documentgrootte van het, query's per seconde, sleutels, aanvragen en antwoorden, [Servicelimieten in Azure Search](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services-limieten
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN-limieten
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Limieten voor Mobile Services
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>Monitor limieten
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Notification Hub Servicelimieten
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Limieten voor Event Hubs
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Service Bus-limieten
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT Hub-limieten
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="iot-hub-device-provisioning-service-limits"></a>IoT Hub apparaat inrichten Servicelimieten
[!INCLUDE [azure-iotdps-limits](../includes/iot-dps-limits.md)]

### <a name="data-factory-limits"></a>Hiermee beperkt u Data Factory
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Data Lake Analytics-limieten
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Data Lake Store-limieten
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="database-migration-service-limits"></a>Servicelimieten voor database-migratie
[!INCLUDE [database-migration-service-limits](../includes/database-migration-service-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics-limieten
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory-limieten
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a>Azure Event raster limieten
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="storsimple-system-limits"></a>Beperkt StorSimple-systeem
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>Log Analytics beperkt
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Back-limieten
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery-limieten
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights-limieten
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Limieten voor API Management
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis-Cache-limieten
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Limieten voor Sleutelkluis
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Meervoudige verificatie
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Limieten voor Automation
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Limieten voor SQL-Database
Zie voor SQL-Database-limieten, [limieten voor SQL Database](sql-database/sql-database-resource-limits.md).

### <a name="sql-data-warehouse-limits"></a>SQL Data Warehouse-limieten
Zie voor SQL Data Warehouse-limieten, [limieten voor SQL Data Warehouse](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md).

## <a name="see-also"></a>Zie ook
[Inzicht in de Azure-limieten en toeneemt](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtual Machine and Cloud Service Sizes for Azure](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Grootten voor Cloudservices](cloud-services/cloud-services-sizes-specs.md)
