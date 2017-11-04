---
title: Architectuur voor hoge beschikbaarheid van virtuele Machines in Azure en scenario's voor SAP NetWeaver | Microsoft Docs
description: Architectuur van hoge beschikbaarheid (HA) en scenario's voor SAP NetWeaver op virtuele Machines in Azure
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 867fe2835418a48e4e616d8137ba9fa4182c8fb7
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="high-availability-architecture-and-scenarios-for-sap-netweaver"></a>Architectuur van hoge beschikbaarheid en scenario's voor SAP NetWeaver

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-ascs-ha-multi-sid-wsfc-file-share]:sap-ascs-ha-multi-sid-wsfc-file-share.md
[sap-ascs-ha-multi-sid-wsfc-shared-disk]:sap-ascs-ha-multi-sid-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md
[sap-higher-availability]:sap-higher-availability-architecture-scenarios.md

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md

[sap-ha-bc-virtual-env-hyperv-vmware-white-paper]:http://scn.sap.com/docs/DOC-44415
[sap-ha-partner-information]:http://scn.sap.com/docs/DOC-8541
[azure-sla]:https://azure.microsoft.com/support/legal/sla/
[azure-virtual-machines-manage-availability]:http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability
[azure-storage-redundancy]:http://azure.microsoft.com/documentation/articles/storage-redundancy/
[azure-storage-managed-disks-overview]:https://docs.microsoft.com/azure/storage/storage-managed-disks-overview

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

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

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


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

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md


## <a name="definition-of-terminologies"></a>Definitie van de terminologie

De term **hoge beschikbaarheid (HA)** is gerelateerd aan een reeks technologieën waarmee IT onderbrekingen minimaliseert door zakelijke continuïteit van de IT-services door middel van redundante, fouttolerant, of failover beveiligde onderdelen in de **dezelfde** Datacenter. In ons geval binnen een Azure-regio.

**Herstel na noodgeval (DR)** is ook gericht Minimalisatie van het aantal onderbreking van de IT-services en hun herstel maar meerdere **verschillende** datacenters bevinden kilometers verwijderd honderden. In ons geval meestal tussen verschillende Azure-regio's binnen dezelfde geopolitieke regio of als tot stand gebracht door u als klant.


## <a name="overview-of-high-availability"></a>Overzicht van hoge beschikbaarheid
We kunnen de discussie over SAP hoge beschikbaarheid in Azure in drie delen gescheiden:

* **Hoge beschikbaarheid van Azure-infrastructuur**, bijvoorbeeld HA van compute (VM's), netwerk, opslag, enz. en de voordelen voor de beschikbaarheid van de SAP verhogen.

* **Met behulp van Azure-infrastructuur VM opnieuw op om u te bereiken 'Hogere beschikbaarheid' van SAP-toepassingen**

  Als u besluit geen gebruik van functies zoals Windows Server Failover Clustering (WSFC)- of pacemaker heeft op Linux te, wordt Azure virtuele machine opnieuw opstarten gebruikt om te beschermen tegen de geplande en ongeplande uitval van de fysieke server Azure-infrastructuur en de algehele een SAP-systeem onderliggende Azure-platform.


* **Hoge beschikbaarheid voor SAP-toepassing**

  Als u wilt bereiken volledige SAP hoge beschikbaarheid van het systeem, moeten we alle kritieke SAP systeemonderdelen, bijvoorbeeld beveiligen:
  * Redundante **SAP-toepassingsservers**, en
  * Unieke onderdelen (bijvoorbeeld **fouttolerantie (SPOF)**) zoals
    * **SAP-(A) exemplaar SCS** en
    *  **DBMS**.


SAP hoge beschikbaarheid in Azure heeft enkele verschillen in vergelijking met SAP hoge beschikbaarheid in een on-premises fysieke of virtuele omgeving. Het volgende artikel [SAP NetWeaver hoge beschikbaarheid en zakelijke continuïteit in virtuele omgevingen met VMware en Hyper-V op Microsoft Windows] [ sap-ha-bc-virtual-env-hyperv-vmware-white-paper] beschrijft standaard SAP hoge beschikbaarheid configuraties in gevirtualiseerde omgevingen in Windows.

Er is geen sapinst geïntegreerd SAP-HA configuratie voor Linux zoals deze voor Windows bestaat. Met betrekking tot SAP HA on-premises voor Linux meer informatie vinden in [hoge beschikbaarheid partnergegevens][sap-ha-partner-information].

## <a name="azure-infrastructure-high-availability"></a>Hoge beschikbaarheid van Azure-infrastructuur

### <a name="single-instance-virtual-machine-sla"></a>Virtuele Machine SLA voor één exemplaar

Er is een enkel VM SLA van 99,9% met premium-opslag. Als u een idee hoe de beschikbaarheid van één VM als volgt uitzien, kunt u het product van de verschillende beschikbare [Azure Service Level Agreements][azure-sla].

De basis voor de berekening is 30 dagen per maand of 43.200 minuten. Daarom 0,05% uitvaltijd overeenkomt met 21,6 minuten. De beschikbaarheid van de verschillende services Vermenigvuldig zoals gebruikelijk in de volgende manier:

(Beschikbaarheidservice #1/100) * (beschikbaarheidservice #2/100) * (beschikbaarheidservice #3/100) \*...

Zoals:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 of een algemene beschikbaarheid van 99.75%.

### <a name="multiple-instances-of-virtual-machines-in-the-same-availability-set"></a>Meerdere exemplaren van virtuele Machines in dezelfde Beschikbaarheidsset
Voor alle virtuele Machines die twee of meer exemplaren die zijn geïmplementeerd in dezelfde **Beschikbaarheidsset**, wordt gegarandeerd dat u moet verbinding met virtuele Machine ten minste één exemplaar ten minste 99,95% van de tijd.

Als twee of meer virtuele machines deel van dezelfde Beschikbaarheidsset uitmaken, elke virtuele machine in de beschikbaarheidsset is toegewezen een **updatedomein** en een **foutdomein** door het onderliggende Azure-platform.

**Fault-domeinen** garandeert dat virtuele machines zijn geïmplementeerd op verschillende hardware die geen algemene power-bron- en switch delen. Wanneer niet-geplande uitvaltijd van servers, netwerkswitch of stroombron, slechts één virtuele machine wordt beïnvloed.

**Bijwerken van domeinen** wordt gegarandeerd dat verschillende virtuele machines niet op hetzelfde moment tijdens het geplande onderhoud van de Azure-infrastructuur worden opgestart, maar slechts één virtuele machine opnieuw is gestart op een tijdstip.

Zie voor meer informatie [de beschikbaarheid van Windows virtuele machines in Azure beheren][azure-virtual-machines-manage-availability].

Beschikbaarheidsset wordt gebruikt voor het bereiken van maximale beschikbaarheid van:

* Redundante SAP-toepassingsservers  

* Clusters met twee of meer knooppunten (bijvoorbeeld VM's) die SPOFs beveiligen, zoals SAP (A) SCS-exemplaar en DBMS

### <a name="virtual-machine-vm-planned-and-unplanned-maintenance"></a>Virtuele Machine (VM) geplande en niet-gepland onderhoud

Er zijn twee soorten gebeurtenissen van Azure-platform die kunnen invloed hebben op de beschikbaarheid van uw virtuele machines: gepland onderhoud en niet-gepland onderhoud.

* **Gepland onderhoud** gebeurtenissen zijn periodieke updates die zijn aangebracht door Microsoft in de onderliggende Azure-platform voor het verbeteren van de algehele betrouwbaarheid, prestaties en beveiliging van de platforminfrastructuur die op uw virtuele machines worden uitgevoerd.

* **Niet-gepland onderhoud** gebeurtenissen optreden wanneer de hardware of fysieke infrastructuur onderliggende uw virtuele machine een fout in een bepaalde manier opgetreden is. Voorbeelden hiervan zijn lokale netwerkproblemen, lokale schijfdefecten of andere defecten op rack-niveau. Wanneer een dergelijke fout wordt gedetecteerd, is de Azure-platform automatisch migratie van uw virtuele machine van de slechte fysieke server die als host fungeert voor uw virtuele machine naar een gezonde fysieke server. Dergelijke gebeurtenissen zijn zeldzaam, maar kunnen er ook toe leiden dat uw virtuele machine opnieuw moet opstarten.

Zie voor meer informatie [de beschikbaarheid van Windows virtuele machines in Azure beheren][azure-virtual-machines-manage-availability].

### <a name="azure-storage-redundancy"></a>Azure Storage redundantie
De gegevens in uw Microsoft Azure Storage-Account worden altijd gerepliceerd voor duurzaamheid en hoge beschikbaarheid, voldoen aan de SERVICEOVEREENKOMST van Azure Storage zelfs met betrekking tot tijdelijke hardwarefouten.

Omdat Azure Storage is het houden van drie afbeeldingen van de gegevens standaard, zijn RAID 5- of RAID1 over meerdere Azure-schijven niet nodig.

Zie voor meer informatie [Azure Storage-replicatie][azure-storage-redundancy].

### <a name="azure-managed-disks"></a>Azure Managed Disks
Beheerde schijven zijn een nieuwe brontype in Azure Resource Manager die kunnen worden gebruikt in plaats van VHD's die zijn opgeslagen in Azure Storage-Accounts. Beheerde schijven automatisch zijn afgestemd op de Beschikbaarheidsset van deze zijn gekoppeld aan virtuele machine en daarom verhoogt de beschikbaarheid van uw virtuele machine en de services die worden uitgevoerd op de virtuele machine.
Zie voor meer informatie [overzicht van Azure beheerd schijven][azure-storage-managed-disks-overview].

We raden je aan u beheerde schijf gebruiken, omdat ze de implementatie en beheer van uw virtuele machines vereenvoudigt.
**SAP ondersteunt momenteel alleen Premium-schijven beheerd**. Lees voor meer informatie, SAP-notitie [1928533].

## <a name="utilizing-azure-infrastructure-ha-to-achieve-sap-application-higher-availability"></a>Met behulp van Azure-infrastructuur HA "hoger-knooppunt beschikbaarheid SAP bereiken

Als u besluit geen gebruik van functies zoals Windows Server Failover Clustering (WSFC)- of pacemaker heeft op Linux te (momenteel alleen ondersteund voor SLES 12 en hoger), Azure virtuele machine opnieuw opstarten wordt gebruikt voor het beveiligen van een SAP-systeem tegen de geplande en ongeplande uitval van de Azure fysieke serverinfrastructuur en algemene onderliggende Azure-platform.

Deze aanpak meer wordt beschreven in het volgende document [met behulp van Azure-infrastructuur VM opnieuw is opgestart Achieve 'Hogere beschikbaarheid' van SAP-systeem][sap-higher-availability].

## <a name="baed0eb3-c662-4405-b114-24c10a62954e"></a>SAP-toepassing voor hoge beschikbaarheid op Azure IaaS

Als u wilt bereiken volledige SAP hoge beschikbaarheid van het systeem, moeten we alle kritieke SAP systeemonderdelen, bijvoorbeeld beveiligen:

* Redundante **SAP-toepassingsservers**, en

* Unieke onderdelen (bijvoorbeeld **fouttolerantie (SPOF)**) zoals
  * **SAP-(A) exemplaar SCS** en
  *  **DBMS**.

Wordt besproken in detail onder het bereiken van maximale beschikbaarheid voor alle drie essentiële SAP onderdelen van het systeem.

### <a name="high-availability-for-sap-application-servers"></a>Hoge beschikbaarheid voor SAP-toepassingsservers

> In dit hoofdstuk is van toepassing op beide:
>
> ![Windows][Logo_Windows] Windows en ![Linux][Logo_Linux] Linux
>

Normaal gesproken hoeft u niet een bepaalde oplossing voor hoge beschikbaarheid voor de SAP-toepassingsserver en dialoogvenster exemplaren. Een maximale beschikbaarheid realiseren door redundantie en u meerdere exemplaren van het dialoogvenster configureren in verschillende exemplaren van Azure Virtual Machines. U hebt ten minste twee SAP exemplaren van een toepassing in twee exemplaren van Azure Virtual Machines geïnstalleerd.

![Afbeelding 1: Hoge beschikbaarheid SAP-toepassingsserver][sap-ha-guide-figure-2000]

_**Afbeelding 1:** hoge beschikbaarheid SAP-toepassingsserver_

Alle virtuele machines die host SAP-toepassingsserver exemplaren in de dezelfde Azure beschikbaarheid instellen, moet u plaatsen. Een Azure beschikbaarheidsset zorgt ervoor dat:

* Alle virtuele machines deel uitmaken van dezelfde **upgradedomein**. Een upgradedomein bijvoorbeeld ervoor zorgt dat de virtuele machines op hetzelfde moment tijdens gepland onderhoud uitval zijn niet bijgewerkt.
De basisfunctionaliteit is gebaseerd op andere Upgrade en domeinen met fouten binnen een Azure-schaaleenheid al is geïntroduceerd in hoofdstuk [domeinen upgraden][planning-guide-3.2.2].

* Alle virtuele machines deel uitmaken van dezelfde **foutdomein**. Een foutdomein bijvoorbeeld ervoor zorgt dat virtuele machines zodat er geen storingspunt is van invloed op de beschikbaarheid van alle virtuele machines zijn geïmplementeerd.

Er is geen oneindig aantal probleem- en Upgrade-domeinen die kunnen worden gebruikt door een Azure-Beschikbaarheidsset binnen een Azure-schaaleenheid. Dit betekent dat als een aantal virtuele machines in een Beschikbaarheidsset, sneller of later meer dan één die VM belandt in dezelfde fout of het upgraden van domein.

Enkele SAP application server-exemplaren in hun toegewezen virtuele machines implementeert en ervan uitgaande dat wij vijf upgraden domeinen, worden de volgende afbeelding ontstaat aan het einde. De werkelijke maximum aantal fouten en update domeinen binnen een beschikbaarheidsset in de toekomst mogelijk wijzigen:

![Afbeelding 2: HA van SAP-toepassingsservers in Azure Beschikbaarheidsset][planning-guide-figure-3000]
_**afbeelding 2:** HA van SAP toepassingsservers in Azure Beschikbaarheidsset_ meer details zijn te vinden in deze documentatie: [de beschikbaarheid van virtuele machines beheren][virtual-machines-manage-availability].


Azure Beschikbaarheidssets in hoofdstuk gepresenteerd [Beschikbaarheidssets van Azure] [ planning-guide-3.2.3] van Azure Virtual Machines planning en implementatie voor SAP NetWeaver document.


**Alleen niet-beheerde schijf:** omdat de Azure storage-account een potentieel storingspunt is, het is belangrijk dat u hebt ten minste twee Azure storage-accounts, waarbij ten minste twee virtuele machines worden gedistribueerd. In een ideaal instelling, zou de schijven van elke virtuele machine die wordt uitgevoerd een SAP-dialoogvenster-exemplaar worden geïmplementeerd in een ander opslagaccount.

> [!IMPORTANT]
>
> Wordt aangeraden dat u de Azure-schijven beheerd gebruiken voor uw SAP HA-installaties, omdat ze automatisch worden uitgelijnd met de Beschikbaarheidsset van de virtuele machine ze zijn gekoppeld aan, en daarom verhoogt de beschikbaarheid van uw virtuele machine en de Services die worden uitgevoerd op de virtuele machine.  
>


### <a name="high-availability-architecture-for-the-sap-ascs-instance"></a>Architectuur van hoge beschikbaarheid voor het exemplaar van de SCS SAP (A)

### <a name="high-availability-architecture-for-the-sap-ascs-instance-on-windows"></a>Architectuur van hoge beschikbaarheid voor het exemplaar van de SCS SAP (A) in Windows


> ![Windows][Logo_Windows] Windows
>

U kunt **Windows Server Failover Clustering** (**WSFC**) oplossing voor het beveiligen van SAP (A) SCS-exemplaar. Er zijn twee varianten van oplossing:

* Clustering van het gebruik van SAP (A) SCS exemplaar **geclusterde gedeelde schijven**

   U vindt meer informatie over de HA-architectuur met geclusterde gedeelde schijven in dit document: [Clustering SAP (A) SCS exemplaar op Windows Failover Cluster met behulp van gedeelde clusterschijf][sap-high-availability-guide-wsfc-shared-disk].   

* Clustering van het gebruik van SAP (A) SCS exemplaar **bestandsshare**

  U vindt meer informatie over de HA-architectuur met bestandsshare in dit document: [Clustering SAP (A) SCS exemplaar op Windows Failover Cluster met behulp van bestandsshare][sap-high-availability-guide-wsfc-file-share].

### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Hoge beschikbaarheid voor het exemplaar van de SCS SAP (A) op Linux


> ![Linux][Logo_Linux] Linux
>

U vindt meer informatie over clustering SCS SAP (A)-exemplaar met behulp van Framework voor SUSE Linux Enterprise Server-Cluster in dit document: [hoge beschikbaarheid voor SAP NetWeaver op Azure VM's op SUSE Linux Enterprise Server voor SAP-toepassingen][sap-suse-ascs-ha].

### <a name="sap-netweaver-multi-sid-configuration-for-clustered-sap-ascs-instance"></a>SAP NetWeaver Multi-SID-configuratie voor geclusterde SAP (A) SCS-exemplaar

> ![Windows][Logo_Windows] Windows
>
>Multi-SID is momenteel alleen ondersteund bij **Windows Server Failover Cluster (WSFC)**. Multi-SID wordt ondersteund met **bestandsshare** en **gedeelde schijf**.
>

U vindt meer informatie over meerdere SID HA-architectuur in deze architectuur documenten:

* [SAP (A) SCS Multi-SID hoge beschikbaarheid voor met Windows Serverfailover Clustering-instantie en bestandsshare][sap-ascs-ha-multi-sid-wsfc-file-share]

* [SAP (A) SCS Multi-SID hoge beschikbaarheid voor met Windows Serverfailover Clustering en de gedeelde schijf][sap-ascs-ha-multi-sid-wsfc-shared-disk]

### <a name="high-availability-dbms-instance"></a>Hoge beschikbaarheid DBMS-exemplaar

De DBMS is ook een aanspreekpunt in een SAP-systeem. U moet deze beschermen met behulp van een oplossing voor hoge beschikbaarheid. Volgende afbeelding ziet u een hoge beschikbaarheid SQL Server Always On oplossing in Azure, met Windows Server Failover Clustering en de Azure interne load balancer. SQL Server Always On repliceert DBMS gegevens en logboekbestanden bestanden met behulp van een eigen DBMS-replicatie. In dit geval hoeft u niet gedeelde clusterschijf die de volledige configuratie vereenvoudigt.

![Afbeelding 3: Voorbeeld van een hoge beschikbaarheid SAP DBMS, met SQL Server Always On][sap-ha-guide-figure-2003]

_**Afbeelding 3:** voorbeeld van een hoge beschikbaarheid SAP DBMS, met SQL Server Always On_

Voor meer informatie over clustering **SQL Server-DBMS** in Azure met behulp van het Azure Resource Manager-implementatiemodel, Zie de volgende artikelen:

* [AlwaysOn-beschikbaarheidsgroep in Azure Virtual Machines handmatig configureren met behulp van Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]

* [Een Azure interne load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

Voor meer informatie over clustering **SAP HANA DBMS** in Azure met behulp van het Azure Resource Manager-implementatiemodel, raadpleegt u dit artikel:

* [Hoge beschikbaarheid van SAP HANA op Azure virtuele Machines (VM's)][sap-hana-ha]
