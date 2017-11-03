---
title: SAP (A) SCS Multi-SID hoge beschikbaarheid met Windows Serverfailover Clustering-instantie en bestandsshare in Azure | Microsoft Docs
description: Multi-SID hoge beschikbaarheid voor SAP-(A) SCS-exemplaar met Windows Serverfailover Clustering en bestandsshare op Azure
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: cbf18abe-41cb-44f7-bdec-966f32c89325
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9f0beae1c92c40bb89decc6ca567aed98c5e29b4
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1869038]:https://launchpad.support.sap.com/#/notes/1869038
[2287140]:https://launchpad.support.sap.com/#/notes/2287140
[2492395]:https://launchpad.support.sap.com/#/notes/2492395

[sap-official-ha-file-share-document]:https://www.sap.com/documents/2017/07/f453332f-c97c-0010-82c7-eda71af511fa.html
[s2d-in-win-2016]:https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[sofs-overview]:https://technet.microsoft.com/library/hh831349(v=ws.11).aspx
[new-in-win-2016-storage]:https://docs.microsoft.com/windows-server/storage/whats-new-in-storage

[sap-installation-guides]:http://service.sap.com/instguides
[sap-installation-guides-file-share]:https://www.sap.com/documents/2017/07/f453332f-c97c-0010-82c7-eda71af511fa.html
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md
[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ascs-ha-multi-sid-wsfc-shared-disk]:sap-ascs-ha-multi-sid-wsfc-shared-disk.md
[sap-ascs-ha-multi-sid-wsfc-shared-disk-infrast-prepare]:sap-ascs-ha-multi-sid-wsfc-shared-disk.md#25e358f8-92e5-4e8d-a1e5-df7580a39cb0
[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6001]:media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png

[sap-ha-guide-figure-8001]:./media/virtual-machines-shared-sap-high-availability-guide/8001.png
[sap-ha-guide-figure-8002]:./media/virtual-machines-shared-sap-high-availability-guide/8002.png
[sap-ha-guide-figure-8003]:./media/virtual-machines-shared-sap-high-availability-guide/8003.png
[sap-ha-guide-figure-8004]:./media/virtual-machines-shared-sap-high-availability-guide/8004.png
[sap-ha-guide-figure-8005]:./media/virtual-machines-shared-sap-high-availability-guide/8005.png
[sap-ha-guide-figure-8006]:./media/virtual-machines-shared-sap-high-availability-guide/8006.png
[sap-ha-guide-figure-8007]:./media/virtual-machines-shared-sap-high-availability-guide/8007.png
[sap-ha-guide-figure-8008]:./media/virtual-machines-shared-sap-high-availability-guide/8008.png
[sap-ha-guide-figure-8009]:./media/virtual-machines-shared-sap-high-availability-guide/8009.png
[sap-ha-guide-figure-8010]:./media/virtual-machines-shared-sap-high-availability-guide/8010.png
[sap-ha-guide-figure-8011]:./media/virtual-machines-shared-sap-high-availability-guide/8011.png
[sap-ha-guide-figure-8012]:./media/virtual-machines-shared-sap-high-availability-guide/8012.png
[sap-ha-guide-figure-8013]:./media/virtual-machines-shared-sap-high-availability-guide/8013.png
[sap-ha-guide-figure-8014]:./media/virtual-machines-shared-sap-high-availability-guide/8014.png
[sap-ha-guide-figure-8015]:./media/virtual-machines-shared-sap-high-availability-guide/8015.png
[sap-ha-guide-figure-8016]:./media/virtual-machines-shared-sap-high-availability-guide/8016.png
[sap-ha-guide-figure-8017]:./media/virtual-machines-shared-sap-high-availability-guide/8017.png
[sap-ha-guide-figure-8018]:./media/virtual-machines-shared-sap-high-availability-guide/8018.png
[sap-ha-guide-figure-8019]:./media/virtual-machines-shared-sap-high-availability-guide/8019.png
[sap-ha-guide-figure-8020]:./media/virtual-machines-shared-sap-high-availability-guide/8020.png
[sap-ha-guide-figure-8021]:./media/virtual-machines-shared-sap-high-availability-guide/8021.png
[sap-ha-guide-figure-8022]:./media/virtual-machines-shared-sap-high-availability-guide/8022.png
[sap-ha-guide-figure-8023]:./media/virtual-machines-shared-sap-high-availability-guide/8023.png
[sap-ha-guide-figure-8024]:./media/virtual-machines-shared-sap-high-availability-guide/8024.png
[sap-ha-guide-figure-8025]:./media/virtual-machines-shared-sap-high-availability-guide/8025.png


[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

# <a name="sap-ascs-instance-multi-sid-high-availability-with-windows-server-failover-clustering-and-file-share-on-azure"></a>SAP (A) SCS Multi-SID hoge beschikbaarheid met Windows Serverfailover Clustering-instantie en bestandsshare op Azure

> ![Windows][Logo_Windows] Windows
>

In September 2016 Microsoft uitgebracht een functie waar u meerdere virtuele IP-adressen beheren met behulp van kunt een [Azure interne load balancer][load-balancer-multivip-overview]. Deze functionaliteit bestaat al in de Azure externe load balancer.

Als u een SAP-implementatie hebt, kunt u een interne load balancer maken van een Windows-clusterconfiguratie voor SAP ASC's / SCS.

In dit artikel is gericht op het verplaatsen van een enkele ASC's / SCS-installatie naar een multi-SID SAP configuratie door het installeren van extra SAP ASC's / SCS geclusterde exemplaren in een bestaande Windows Server Failover Clustering (WSFC)-cluster met **bestandsshare** . Wanneer dit proces is voltooid, kunt u een SAP multi-SID-cluster hebt geconfigureerd.

> [!NOTE]
>
> Deze functie is alleen beschikbaar in de **Azure Resource Manager** implementatiemodel.
>
>Er is een limiet aan het aantal persoonlijke front-end-IP-adressen voor elke Azure interne load balancer.
>
>Het maximum aantal SAP ASC's / SCS-exemplaren in een WSFC-cluster is gelijk aan het maximum aantal persoonlijke front-end-IP-adressen voor elke Azure interne load balancer.
>

Zie voor meer informatie over de limieten van de load balancer 'persoonlijke front-end-IP per load balancer in [netwerken limieten: Azure Resource Manager][networking-limits-azure-resource-manager].

## <a name="prerequisites"></a>Vereisten

U hebt al een WSFC-cluster dat wordt gebruikt voor geconfigureerd **één** SAP ASC's / SCS sessie gebruikt **bestandsshare**, zoals wordt weergegeven in dit diagram.

![Afbeelding 1: (A) SCS-exemplaar en SOFS geïmplementeerd in twee clusters met SAP][sap-ha-guide-figure-8007]

_**Afbeelding 1:** SAP-(A) SCS-exemplaar en SOFS geïmplementeerd in twee clusters_

> [!IMPORTANT]
> De instelling moet voldoen aan de volgende voorwaarden:
> * De SAP ASC's / SCS exemplaren moeten delen hetzelfde WSFC-cluster.
> * Elke DBMS SID moet een eigen toegewezen WSFC-cluster hebben.
> * SAP-toepassingsservers die deel uitmaken van een SAP-systeem SID moeten hebben hun eigen toegewezen virtuele machines.

## <a name="sap-ascs-multi-sid-architecture-with-file-share"></a>SAP-(A) SCS Multi-SID-architectuur met bestandsshare

Het doel is om meerdere SAP ABAP ASC's installeren of SAP Java SCS geclusterde exemplaren in de dezelfde WSFC-cluster, als geïllustreerde hier:

![Afbeelding 2: De configuratie van de SAP Multi-SID in twee clusters][sap-ha-guide-figure-8008]

_**Afbeelding 2:** SAP Multi-SID-configuratie in twee clusters_

De installatie van een extra  **&lt;SID2&gt;**  SAP-systeem is identiek aan de installatie van een <SID> systeem. Er zijn twee extra voorbereidende stappen nodig op (A) SCS-cluster, evenals op bestandsshare SOFS-cluster.

## <a name="infrastructure-preparation-for-sap-multi-sid-scenario"></a>De voorbereiding van de infrastructuur voor SAP Multi-SID-Scenario

### <a name="infrastructure-preparation-on-domain-controller"></a>Infrastructuur voorbereiden op de domeincontroller

Maken van de groep  **&lt;domein&gt;\SAP_&lt;SID2&gt;_GlobalAdmin**, bijvoorbeeld met &lt;SID2&gt; = PR2. De domeinnaam van de groep is <Domain>\SAP_PR2_GlobalAdmin

### <a name="infrastructure-preparation-on-ascs-cluster"></a>Infrastructuur voorbereiden op (A) SCS-Cluster

U moet voorbereiden van de infrastructuur op bestaande (A) cluster SCS voor een tweede SAP &lt;SID&gt;:

* De naam van een virtuele host op voor het geclusterde exemplaar van de SAP ASC's / SCS maken op de DNS-server
* Een IP-adres toevoegen aan een bestaande Azure interne load balancer met behulp van PowerShell

Deze stappen worden beschreven in [infrastructuur voorbereiden voor SAP Multi-SID Scenario][sap-ascs-ha-multi-sid-wsfc-shared-disk-infrast-prepare].


### <a name="infrastructure-preparation-on-sofs-cluster-using-existing-sap-global-host"></a>Infrastructuur voorbereiden op SOFS-Cluster met behulp van bestaande globale SAP-Host

U hebt de optie de bestaande hergebruiken  **&lt;SAPGLOBALHost&gt;**  en **Volume1** van de eerste SAP <SID1> system.

![Afbeelding 3: Multi-SID SOFS dezelfde SAP globale hostnaam][sap-ha-guide-figure-8014]

_**Afbeelding 3:** Multi-SID SOFS dezelfde SAP globale hostnaam_

> [!IMPORTANT]
>Voor het tweede **SAP &lt;SID2&gt;**  systeem, de dezelfde Volume1 en dezelfde  **&lt;SAPGlobalHost&gt;**  netwerknaam worden gebruikt.
>Als we wilt gebruiken dezelfde  **&lt;SAPGlobalHost&gt;**  netwerknaam, en we hebben opgelost **SAPMNT** sharenaam voor verschillende SAP-systemen we gebruiken de dezelfde wordengedwongen**Volume1**.
>
>Het pad voor <SID2> globale host: C:\ClusterStorage\\**Volume1**\usr\sap\<SID2 > \SYS\
>

U hebt voor het voorbereiden van de globale HOST SAP **... \SYS\..** map op het cluster SOFS voor &lt;SID2&gt; system.

Voer het volgende PowerShell-Script om voor te bereiden globale HOST voor SAP &lt;SID2&gt; exemplaar:


```PowerShell
##################
# SAP Multi-SID
##################

$SAPSID2 = "PR2"
$DomainName2 = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName2 = "$DomainName2\SAP_" + $SAPSID2 + "_GlobalAdmin"

# SAP (A)SCS cluster nodes
$ASCSCluster2Node1 = "ja1-ascs-0"
$ASCSCluster2Node2 = "ja1-ascs-1"

# Define SAP (A)SCS cluster node computer objects
$ASCSCluster2ObjectNode1 = "$DomainName2\$ASCSCluster2Node1$"
$ASCSCluster2ObjectNode2 = "$DomainName2\$ASCSCluster2Node2$"

# Create usr\sap\.. folders on CSV
$SAPGlobalFolder2 = "C:\ClusterStorage\Volume1\usr\sap\$SAPSID2\SYS"
New-Item -Path $SAPGlobalFOlder2 -ItemType Directory

# Add permissions for SAP SID2 system
Grant-SmbShareAccess -Name sapmnt -AccountName $SAPSIDGlobalAdminGroupName2, $ASCSCluster2ObjectNode1, $ASCSCluster2ObjectNode2 -AccessRight Full -Force


$UsrSAPFolder = "C:\ClusterStorage\Volume1\usr\sap\"

# Set files & folder security
$Acl = Get-Acl $UsrSAPFolder

# Add security object of SAP_<sid>_GlobalAdmin group
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($SAPSIDGlobalAdminGroupName2,"FullControl", 'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode1$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSCluster2ObjectNode1,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode2$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSCluster2ObjectNode2,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Set security
Set-Acl $UsrSAPFolder $Acl -Verbose
```

### <a name="infrastructure-preparation-on-sofs-cluster-using-different-sap-global-host"></a>Infrastructuur voorbereiden op SOFS-Cluster met behulp van verschillende SAP globale Host

U kunt de tweede SOFS configureren, bijvoorbeeld de tweede SOFS cluster-rol met  **&lt;SAPGlobalHost2&gt;**  en andere **Voulme2** voor het tweede  **&lt;SID2&gt;**.

![Afbeelding 4: Multi-SID SOFS dezelfde SAP globale hostnaam 2][sap-ha-guide-figure-8015]

_**Afbeelding 4:** Multi-SID SOFS dezelfde SAP globale hostnaam 2_

Uitvoeren van deze PowerShell-script voor het maken van de tweede SOFS-rol met &lt;SAPGlobalHost2&gt;:

```PowerShell
# Create SOFS with SAP Global Host Name 2
$SAPGlobalHostName = "sapglobal2"
Add-ClusterScaleOutFileServerRole -Name $SAPGlobalHostName
```

Maken van de tweede **Volume2**. Deze PowerShell-script uitvoeren:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName SAPPR2 -FileSystem CSVFS_ReFS -Size 5GB -ResiliencySettingName Mirror
```

![Afbeelding 5: Multi-SID SOFS dezelfde SAP globale hostnaam 2][sap-ha-guide-figure-8016]

_**Afbeelding 5:** tweede **Volume2** in Failoverclusterbeheer_

Maken van een globale SAP map voor het tweede  **&lt;SID2&gt;**  en bestandsbeveiliging ingesteld.

Deze PowerShell-script uitvoeren:

```PowerShell
# Create folder for <SID2> on second Volume2 and set file security
$SAPSID = "PR2"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# SAP (A)SCS cluster nodes
$ASCSClusterNode1 = "ascs-1"
$ASCSClusterNode2 = "ascs-2"

# Define SAP (A)SCS cluster node computer objects
$ASCSClusterObjectNode1 = "$DomainName\$ASCSClusterNode1$"
$ASCSClusterObjectNode2 = "$DomainName\$ASCSClusterNode2$"

# Create usr\sap\.. folders on CSV
$SAPGlobalFolder = "C:\ClusterStorage\Volume2\usr\sap\$SAPSID\SYS"
New-Item -Path $SAPGlobalFOlder -ItemType Directory

$UsrSAPFolder = "C:\ClusterStorage\Volume2\usr\sap\"

# Set files & folder security
$Acl = Get-Acl $UsrSAPFolder

# Add file security object of SAP_<sid>_GlobalAdmin group
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($SAPSIDGlobalAdminGroupName,"FullControl", 'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode1$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode1,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode2$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode2,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Set security
Set-Acl $UsrSAPFolder $Acl -Verbose
```

SAPMNT-bestandsshare maken op **Volume2** met  **&lt;SAPGlobalHost2&gt;**  hostnaam voor de tweede SAP  **&lt;SID2&gt;** , start u de volgende Wizard in Failoverclusterbeheer.

Klik met de rechtermuisknop op saoglobal2 SOFS clustergroep en kies "Add file share."

![Afbeelding 6: Start 'Bestandsshare toevoegen' wizard][sap-ha-guide-figure-8017]

_**Afbeelding 6:** 'Bestandsshare toevoegen' wizard starten_

![Afbeelding 7: 'SMB-Share – snel kiezen][sap-ha-guide-figure-8018]

_**Afbeelding 7:** 'SMB-Share – snel kiezen_

![Afbeelding 8: Kies sapglobalhost2 en pad op Volume2 opgeven][sap-ha-guide-figure-8019]

_**Afbeelding 8:** Kies **sapglobalhost2** en het pad op Volume2 opgeven_


![Afbeelding 9: Naam van bestandsshare ingesteld op sapmnt][sap-ha-guide-figure-8020]

_**Afbeelding 9:** Set bestandssharenaam naar **sapmnt**_

![Afbeelding 10: Alle instellingen uitschakelen][sap-ha-guide-figure-8021]

_**Afbeelding 10:** uitschakelen van alle instellingen_

Controleer of 'Volledig beheer' ingesteld op bestanden en sapmnt share voor:
* **SAP_&lt;SID&gt;_GlobalAdmin** domeingebruikersgroep
* Computerobject (A) clusterknooppunten SCS **ASC's 1$** en **ASC's 2$**

![Afbeelding 11: Zorg ervoor dat u de volledige toegang aan groep en computer gebruikersaccounts instellen][sap-ha-guide-figure-8022]

_**Afbeelding 11:** Zorg ervoor dat u de volledige toegang aan groep en computer gebruikersaccounts instellen_

![Afbeelding 12: Klik op maken][sap-ha-guide-figure-8023]

_**Afbeelding 12:** Klik op maken_

![Afbeelding 13: Afhankelijk van de tweede sapmnt sapglobal2 host en Volume2 is gemaakt][sap-ha-guide-figure-8024]

_**Afbeelding 13:** de **tweede sapmnt** gebonden aan **sapglobal2** host en **Volume2** wordt gemaakt_

## <a name="sap-netweaver-multi-sid-installation"></a>SAP NetWeaver Multi-SID-installatie

### <a name="sap-ltsid2gt-ascs-and-ers-instances-installation"></a>SAP &lt;SID2&gt; (A) SCS en Ebruikers exemplaren installatie

Volg dezelfde stappen installatie en configuratie zoals beschreven voor een SAP &lt;SID&gt;.

### <a name="install-dbms-and-sap-application-servers"></a>Installeer DBMS en SAP toepassingsservers
Installeer DBMS en SAP-toepassingsservers zoals eerder beschreven.



## <a name="next-steps"></a>Volgende stappen

* [Installatie van een exemplaar van de SCS (A) op een failovercluster waarop geen gedeelde schijven - officiële SAP-richtlijnen voor de bestandsshare HA][sap-official-ha-file-share-document]:

* [Opslagruimten Direct in WindowsServer 2016][s2d-in-win-2016]

* [Scale-Out File Server for Application Data Overview][sofs-overview]

* [Wat is er nieuw in de opslag in Windows Server 2016][new-in-win-2016-storage]
