---
title: Azure Virtual Machines-architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver | Microsoft Docs
description: Architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver op Azure Virtual Machines
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: gwallace
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/21/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f6c329a2b50c946e873391db431c1cd5ff30ab4f
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709071"
---
# <a name="high-availability-architecture-and-scenarios-for-sap-netweaver"></a>Architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver

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

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

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
[sap-suse-ascs-ha-anf]:high-availability-guide-suse-netapp-files.md
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

[sap-ha-bc-virtual-env-hyperv-vmware-white-paper]:https://scn.sap.com/docs/DOC-44415
[sap-ha-partner-information]:https://scn.sap.com/docs/DOC-8541
[azure-sla]:https://azure.microsoft.com/support/legal/sla/
[azure-virtual-machines-manage-availability]:https://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability
[azure-storage-redundancy]:https://azure.microsoft.com/documentation/articles/storage-redundancy/
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

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (Configuratie voor hoge beschikbaarheid van SAP multi-SID)


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


## <a name="terminology-definitions"></a>Overzicht van termen die

**Hoge beschikbaarheid**: Verwijst naar een reeks technologieën die IT onderbrekingen minimaliseren door op te geven van zakelijke continuïteit van IT-services door middel van redundante, fouttolerante of failover beveiligde onderdelen binnen de *dezelfde* Datacenter. In ons geval is het datacenter bevinden zich binnen een Azure-regio.

**Herstel na noodgevallen**: Ook verwijst naar het minimaliseren van onderbreking van de IT-services en het herstel van, maar meerdere *verschillende* datacenters die mogelijk grote afstand van elkaar. In ons geval kunnen de datacentra zich bevinden in verschillende Azure-regio's binnen dezelfde geopolitieke regio of in de locaties tot stand gebracht door u als een klant.


## <a name="overview-of-high-availability"></a>Overzicht van hoge beschikbaarheid
Hoge beschikbaarheid van SAP in Azure kan worden opgesplitst in drie typen:

* **Hoge beschikbaarheid van Azure-infrastructuur**: 

    Hoge beschikbaarheid kan bijvoorbeeld Reken-(VM's), netwerk, of opslag en de voordelen voor het verhogen van de beschikbaarheid van SAP-toepassingen bevatten.

* **Als u wilt bereiken met behulp van Azure-infrastructuur virtuele machine opnieuw *hogere beschikbaarheid* aan SAP-toepassingen**: 

    Als u besluit om niet te gebruiken functies zoals Windows Server Failover Clustering (WSFC) of Pacemaker op Linux, wordt Azure-VM opnieuw opstarten gebruikt. Het beveiligt SAP-systemen op basis van geplande en ongeplande uitval van de infrastructuur van Azure fysieke server en de algehele onderliggende Azure-platform.

* **Hoge beschikbaarheid van SAP**: 

    Als u wilt bereiken volledige SAP hoge beschikbaarheid van het systeem, moet u alle essentiële onderdelen van het systeem SAP beveiligen. Bijvoorbeeld:
    * Redundante SAP-toepassingsservers.
    * De unieke onderdelen. Een voorbeeld is mogelijk een single point of onderdeel van de fout (SPOF), zoals een SAP ASCS/SCS-exemplaar of een databasebeheersysteem (DBMS).

Hoge beschikbaarheid van SAP in Azure verschilt van de hoge beschikbaarheid van SAP in een on-premises fysieke of virtuele omgeving. Het volgende document [hoge beschikbaarheid van SAP NetWeaver en bedrijfscontinuïteit in virtuele omgevingen met VMware en Hyper-V op Microsoft Windows][sap-ha-bc-virtual-env-hyperv-vmware-white-paper] worden standaard SAP hoge beschikbaarheid configuraties beschreven die in een gevirtualiseerde omgevingen op Windows.

Er is geen configuratie SAP hoge beschikbaarheid sapinst geïntegreerd voor Linux, omdat er voor Windows. Zie voor meer informatie over SAP hoge beschikbaarheid on-premises voor Linux [partnergegevens voor hoge beschikbaarheid][sap-ha-partner-information].

## <a name="azure-infrastructure-high-availability"></a>Hoge beschikbaarheid van Azure-infrastructuur

### <a name="sla-for-single-instance-virtual-machines"></a>SLA voor virtuele machines van één exemplaar

Er is momenteel een één-VM-SLA van 99,9% met premium storage. Als u een idee van wat de beschikbaarheid van een enkele virtuele machine kan zijn, kunt u het product van de verschillende beschikbare bouwen [Azure Service Level Agreements][azure-sla].

De basis voor de berekening is 30 dagen per maand of 43.200 minuten. Bijvoorbeeld, een uitvaltijd 0,05% komt overeen met 21,6 minuten. Zoals gewoonlijk wordt de beschikbaarheid van de verschillende services berekend op de volgende manier:

(Beschikbaarheidservice #1/100) * (beschikbaarheidservice #2/100) * (beschikbaarheidservice #3/100) \*...

Bijvoorbeeld:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 of een algemene beschikbaarheid van 99.75%.

### <a name="multiple-instances-of-virtual-machines-in-the-same-availability-set"></a>Meerdere exemplaren van virtuele machines in dezelfde beschikbaarheidsset
Voor alle virtuele machines waarvoor twee of meer instanties zijn geïmplementeerd in dezelfde *beschikbaarheidsset*, garanderen we dat u verbinding van de virtuele machine met ten minste één exemplaar ten minste 99,95% van de tijd hebt.

Wanneer twee of meer VM's deel van dezelfde beschikbaarheidsset uitmaken, elke virtuele machine in de beschikbaarheidsset krijgt een *updatedomein* en een *foutdomein* door de onderliggende Azure-platform.

* **Updatedomeinen** garanderen dat meerdere virtuele machines niet opnieuw worden opgestart op hetzelfde moment tijdens het geplande onderhoud van een Azure-infrastructuur. Slechts één virtuele machine opnieuw wordt opgestart op een tijdstip.

* **Foutdomeinen** garanderen dat virtuele machines worden geïmplementeerd op hardware-onderdelen die geen een gemeenschappelijke voedingsbron en netwerkswitch delen. Wanneer servers, een netwerkswitch of een bron power worden onderworpen aan een niet-geplande uitvaltijd, wordt slechts één virtuele machine wordt beïnvloed.

Zie voor meer informatie, [de beschikbaarheid van Windows virtuele machines in Azure beheren][azure-virtual-machines-manage-availability].

Een beschikbaarheidsset wordt gebruikt voor het bereiken van hoge beschikbaarheid van:

* Redundante SAP-toepassingsservers.  
* Clusters met twee of meer knooppunten (bijvoorbeeld VM's) die SPOFs, zoals een SAP ASCS/SCS-exemplaar of een DBMS-systemen beveiligen.


### <a name="azure-availability-zones"></a>Azure Beschikbaarheidszones
Azure is een concepten van uitrollen [Azure Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview) in verschillende [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/). In Azure-regio's waar Beschikbaarheidszones worden aangeboden, hebben de Azure-regio's meerdere datacenters die onafhankelijk van elkaar in levering van voeding, koeling en netwerk. Reden voor het bieden van verschillende zones binnen één Azure-regio is waarmee u kunt toepassingen implementeren in twee of drie Beschikbaarheidszones aangeboden. Ervan uitgaande dat problemen in energiebronnen en/of invloed zijn op één Beschikbaarheidszone infrastructuur, is de implementatie van uw toepassing binnen een Azure-regio nog steeds volledig functioneel. Uiteindelijk verloren met sommige minder capaciteit omdat sommige virtuele machines in één zone zijn gegaan. Maar virtuele machines in de andere twee zones zijn nog steeds actief en werkend. De Azure-regio's die worden geboden door zones worden vermeld in [Azure Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview).

Beschikbaarheidszones bieden enkele aandachtspunten voor. De lijst met overwegingen, zoals:

- U kunt geen Beschikbaarheidssets van Azure binnen een Beschikbaarheidszone implementeren. U moet een Beschikbaarheidszone of een Beschikbaarheidsset kiezen als frame van de implementatie voor een virtuele machine.
- U kunt geen gebruiken de [Basic Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) failover om clusteroplossingen te maken op basis van Windows Failover Cluster-Services of Linux-Pacemaker. In plaats daarvan moet u gebruiken de [Azure Standard Load Balancer-SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)
- Azure-Beschikbaarheidszones geeft geen garanties van bepaalde afstand tussen de verschillende zones binnen één regio
- De netwerklatentie tussen verschillende Azure-Beschikbaarheidszones binnen de verschillende Azure-regio's kan afwijken van Azure-regio naar regio. Er zijn gevallen waarin u een klant kunt redelijkerwijs uitvoeren van het niveau van de SAP-toepassing geïmplementeerd in verschillende zones sinds de netwerklatentie van één zone voor de actieve DBMS-VM nog steeds acceptabel is van een impact op het proces van het bedrijf. Terwijl er klantscenario's waar de latentie tussen de actieve VM DBMS in één zone en het exemplaar van een SAP-toepassing in een virtuele machine in een andere zone worden te firewallverbinding en niet geschikt is voor de SAP-bedrijfsprocessen worden kan. Als gevolg hiervan moeten de implementatie-architecturen worden verschillende met een actief/actief-architectuur voor de toepassing of actief/passief-architectuur als te hoog is.
- Met behulp van [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/) is verplicht voor het implementeren in Azure-Beschikbaarheidszones 


### <a name="planned-and-unplanned-maintenance-of-virtual-machines"></a>Geplande en niet-gepland onderhoud van virtuele machines

Twee soorten gebeurtenissen voor Azure-platform kunnen invloed hebben op de beschikbaarheid van uw virtuele machines:

* **Gepland onderhoud** gebeurtenissen zijn periodieke updates die zijn gesteld door Microsoft de onderliggende Azure-platform. De updates verbeteren de algehele betrouwbaarheid, prestaties en beveiliging van de platform-infrastructuur waarop uw virtuele machines worden uitgevoerd.

* **Niet-gepland onderhoud** gebeurtenissen optreden wanneer de hardware of de fysieke infrastructuur die uw virtuele machine is mislukt op een bepaalde manier. Het advies inwinnen lokale netwerkproblemen, lokale schijffouten te voorkomen of andere fouten van rack-niveau. Wanneer een dergelijke fout wordt gedetecteerd, migreert het Azure-platform uw virtuele machine automatisch van de slechte fysieke server die als host fungeert voor uw virtuele machine naar een gezonde fysieke server. Dergelijke gebeurtenissen zijn zeldzaam, maar ze kunnen ook ervoor zorgen dat uw virtuele machine opnieuw op te starten.

Zie voor meer informatie, [de beschikbaarheid van Windows virtuele machines in Azure beheren][azure-virtual-machines-manage-availability].

### <a name="azure-storage-redundancy"></a>Azure Storage-redundantie
De gegevens in uw storage-account worden altijd gerepliceerd voor duurzaamheid en hoge beschikbaarheid, voldoen aan de SLA voor de Azure-opslag zelfs bij tijdelijke hardwarefouten.

Omdat Azure Storage kan drie kopieën van de gegevens standaard voldoen, is het gebruik van RAID 5 of RAID 1 over meerdere Azure-schijven is niet nodig.

Zie voor meer informatie, [Azure Storage-replicatie][azure-storage-redundancy].

### <a name="azure-managed-disks"></a>Azure Managed Disks
Beheerde schijven is een resourcetype in Azure Resource Manager die worden gebruikt in plaats van de virtuele harde schijven (VHD's) die zijn opgeslagen in Azure storage-accounts wordt aanbevolen. Beheerde schijven wordt automatisch uitgelijnd met een Azure-beschikbaarheidsset van de virtuele machine die ze zijn gekoppeld. Ze verhoogt de beschikbaarheid van uw virtuele machine en de services die erop worden uitgevoerd.

Zie voor meer informatie, [overzicht Azure Managed Disks][azure-storage-managed-disks-overview].

U wordt aangeraden dat u beheerde schijven gebruiken omdat ze de implementatie en beheer van uw virtuele machines vereenvoudigt.



## <a name="utilizing-azure-infrastructure-high-availability-to-achieve-higher-availability-of-sap-applications"></a>Met behulp van hoge beschikbaarheid van de Azure-infrastructuur om te realiseren *hogere beschikbaarheid* aan SAP-toepassingen

Als u besluit om niet te gebruiken functies zoals WSFC of Pacemaker op Linux (momenteel alleen voor SUSE Linux Enterprise Server [SLES] 12 en hoger ondersteund), wordt Azure-VM opnieuw opstarten gebruikt. Het beveiligt SAP-systemen op basis van geplande en ongeplande uitval van de infrastructuur van Azure fysieke server en de algehele onderliggende Azure-platform.

Zie voor meer informatie over deze benadering, [gebruikmaken van Azure-infrastructuur virtuele machine opnieuw opstarten voor hogere beschikbaarheid van de SAP-systeem][sap-higher-availability].

## <a name="baed0eb3-c662-4405-b114-24c10a62954e"></a> Hoge beschikbaarheid van SAP-toepassingen op Azure IaaS

Als u wilt bereiken volledige SAP hoge beschikbaarheid van het systeem, moet u alle essentiële onderdelen van het systeem SAP beveiligen. Bijvoorbeeld:
  * Redundante SAP-toepassingsservers.
  * De unieke onderdelen. Een voorbeeld is mogelijk een single point of onderdeel van de fout (SPOF), zoals een SAP ASCS/SCS-exemplaar of een databasebeheersysteem (DBMS).

De volgende secties besproken hoe voor hoge beschikbaarheid voor alle drie essentiële SAP onderdelen van het systeem.

### <a name="high-availability-architecture-for-sap-application-servers"></a>Architectuur voor hoge beschikbaarheid voor SAP-toepassingsservers

> In deze sectie is van toepassing op:
>
> ![Windows][Logo_Windows] Windows en ![Linux][Logo_Linux] Linux
>

U kunt een specifieke oplossing voor hoge beschikbaarheid doorgaans niet nodig voor de SAP-server en het dialoogvenster toepassingsexemplaren. Bereik een hoge beschikbaarheid met redundantie en u meerdere exemplaren van het dialoogvenster configureren in verschillende exemplaren van virtuele machines van Azure. U hebt ten minste twee exemplaren met SAP-toepassing geïnstalleerd in twee exemplaren van virtuele machines van Azure.

![Afbeelding 1: Hoge beschikbaarheid van SAP-toepassingsserver][sap-ha-guide-figure-2000]

_**Afbeelding 1:** Hoge beschikbaarheid van SAP-toepassingsserver_

U moet alle virtuele machines die host SAP application server-exemplaren in de dezelfde Azure-beschikbaarheidsset plaatsen. Een Azure-beschikbaarheidsset zorgt ervoor dat:

* Alle virtuele machines maken deel uit van hetzelfde updatedomein.  
    Een updatedomein zorgt ervoor dat de virtuele machines op hetzelfde moment tijdens de downtime voor gepland onderhoud niet zijn bijgewerkt.

    De basisfunctionaliteit, die is gebaseerd op een andere update en domeinen met fouten binnen een Azure-schaaleenheid, al is geïntroduceerd in de [updatedomeinen][planning-guide-3.2.2] sectie.

* Alle virtuele machines maken deel uit van hetzelfde foutdomein.  
    Een foutdomein zorgt ervoor dat virtuele machines worden geïmplementeerd, zodat er geen storingspunt is van invloed op de beschikbaarheid van alle virtuele machines.

Het aantal update- en foutdomeinen domeinen die kunnen worden gebruikt door een Azure-beschikbaarheidsset instellen in een Azure-schaaleenheid is beperkt. Als u virtuele machines aan een enkele beschikbaarheidsset toevoegen blijven, wordt twee of meer virtuele machines uiteindelijk terechtkomen in hetzelfde domein schuld of update.

Als u een paar SAP application server-exemplaren in hun toegewezen virtuele machines implementeert, ervan uitgaande dat we beschikken over vijf updatedomeinen, in de volgende afbeelding dan ontstaat. De werkelijke maximumaantal update- en foutdomeinen domeinen binnen een beschikbaarheidsset in de toekomst mogelijk wijzigen:

![Afbeelding 2: Hoge beschikbaarheid van SAP-toepassingsservers in een Azure-beschikbaarheidsset][planning-guide-figure-3000]
 _**afbeelding 2:** Hoge beschikbaarheid van SAP-toepassingsservers in een Azure-beschikbaarheidsset instellen_

Zie voor meer informatie, [de beschikbaarheid van Windows virtuele machines in Azure beheren][azure-virtual-machines-manage-availability].

Zie voor meer informatie de [Azure-beschikbaarheidssets][planning-guide-3.2.3] sectie van de virtuele Azure-machines planning en implementatie van SAP NetWeaver-document.

**Alleen niet-beheerde schijven:** Omdat de Azure storage-account een potentieel single point of failure is, is het belangrijk dat u hebt ten minste twee Azure storage-accounts, waarbij ten minste twee virtuele machines worden gedistribueerd. In een ideale instelling, wordt de schijven van elke virtuele machine waarop een exemplaar van SAP dialoogvenster wordt geïmplementeerd in een ander opslagaccount.

> [!IMPORTANT]
> Wordt aangeraden dat u Azure beheerde schijven voor uw SAP-installaties met maximale beschikbaarheid. Ze verhoogt de beschikbaarheid van uw virtuele machine en de services die erop worden uitgevoerd, omdat beheerde schijven wordt automatisch uitgelijnd met de beschikbaarheidsset van de virtuele machine die ze zijn gekoppeld.  
>

### <a name="high-availability-architecture-for-an-sap-ascsscs-instance-on-windows"></a>Architectuur voor hoge beschikbaarheid voor een SAP ASCS/SCS-exemplaar op Windows

> ![Windows][Logo_Windows] Windows
>

U kunt een WSFC-oplossing gebruiken om te beveiligen van de SAP ASCS/SCS-exemplaar. De oplossing heeft twee varianten:

* **De SAP ASCS/SCS-exemplaar met behulp van geclusterde gedeelde schijven cluster**: Zie voor meer informatie over deze architectuur [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf][sap-high-availability-guide-wsfc-shared-disk].   

* **De SAP ASCS/SCS-exemplaar met behulp van de bestandsshare cluster**: Zie voor meer informatie over deze architectuur [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van de bestandsshare][sap-high-availability-guide-wsfc-file-share].

### <a name="high-availability-architecture-for-an-sap-ascsscs-instance-on-linux"></a>Architectuur voor hoge beschikbaarheid voor een SAP ASCS/SCS-exemplaar op Linux

> ![Linux][Logo_Linux] Linux
> 
> Zie voor meer informatie over clusters van de SAP ASCS/SCS-exemplaar met behulp van de cluster-framework van SLES [hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen][sap-suse-ascs-ha]. For alternative HA architecture on SLES, which doesn't require highly available NFS see [High-availability guide for SAP NetWeaver on SUSE Linux Enterprise Server with Azure NetApp Files for SAP applications][sap-suse-ascs-ha-anf].

Zie voor meer informatie over clusters van de SAP ASCS/SCS-exemplaar met behulp van de cluster-framework van Red Hat [hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)


### <a name="sap-netweaver-multi-sid-configuration-for-a-clustered-sap-ascsscs-instance"></a>SAP NetWeaver multi-SID-configuratie voor een geclusterde SAP ASCS/SCS-exemplaar

> ![Windows][Logo_Windows] Windows
> 
> Op dit moment wordt multi-SID alleen ondersteund met WSFC. Multi-SID wordt ondersteund met de bestandsshare en gedeelde schijf.
> 
> Zie voor meer informatie over de architectuur voor hoge beschikbaarheid van multi-SID:

* [SAP ASCS/SCS-exemplaar multi-SID hoge beschikbaarheid voor Windows Server Failover Clustering en bestandsshare][sap-ascs-ha-multi-sid-wsfc-file-share]

* [SAP ASCS/SCS-exemplaar multi-SID hoge beschikbaarheid voor Windows Server Failover Clustering en gedeelde schijf][sap-ascs-ha-multi-sid-wsfc-shared-disk]

### <a name="high-availability-dbms-instance"></a>Hoge beschikbaarheid DBMS-exemplaar

De DBMS-systemen is ook een single point of neem contact op met in een SAP-systeem. U moet deze beschermen met behulp van een oplossing voor hoge beschikbaarheid. De volgende afbeelding ziet u een SQL Server AlwaysOn-oplossing voor hoge beschikbaarheid in Azure, met Windows Server Failover Clustering en de interne Azure load balancer. SQL Server AlwaysOn repliceert DBMS-gegevens en logboekbestanden bestanden met behulp van een eigen DBMS-replicatie. In dit geval moet u geen gedeelde clusterschijf die de volledige installatie vereenvoudigt.

![Afbeelding 3: Voorbeeld van een hoge beschikbaarheid SAP DBMS, met SQL Server AlwaysOn][sap-ha-guide-figure-2003]

_**Afbeelding 3:** Voorbeeld van een hoge beschikbaarheid SAP DBMS, met SQL Server AlwaysOn_

Zie de volgende artikelen voor meer informatie over clusters van SQL Server DBMS in Azure met behulp van het implementatiemodel Azure Resource Manager:

* [Een AlwaysOn-beschikbaarheidsgroep in Azure virtuele machines handmatig configureren met behulp van Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]

* [Een interne Azure load balancer configureren voor een AlwaysOn-beschikbaarheidsgroep in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

Zie voor meer informatie over clustering SAP HANA DBMS in Azure met behulp van het Azure Resource Manager-implementatiemodel, [hoge beschikbaarheid van SAP HANA op Azure virtual machines (VM's)][sap-hana-ha].
