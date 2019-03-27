---
title: Installatie van SAP NetWeaver hoge beschikbaarheid op een Windows-failovercluster en de gedeelde schijven voor een SAP ASCS/SCS-exemplaar in Azure | Microsoft Docs
description: Informatie over het installeren van SAP NetWeaver hoge beschikbaarheid op een Windows-failovercluster en de gedeelde schijven voor een SAP ASCS/SCS-exemplaar.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 6209bcb3-5b20-4845-aa10-1475c576659f
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1229b7f9e2a430a663a3e78bb457c03cf4a4a590
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58480580"
---
# <a name="install-sap-netweaver-ha-on-a-windows-failover-cluster-and-shared-disk-for-an-sap-ascsscs-instance-in-azure"></a>Installatie van SAP NetWeaver hoge beschikbaarheid op een Windows-failovercluster en de gedeelde schijven voor een SAP ASCS/SCS-exemplaar in Azure

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

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
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md



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

In dit artikel wordt beschreven hoe u installeren en configureren van een SAP-systeem voor hoge beschikbaarheid in Azure met behulp van een Windows Server-failovercluster en de gedeelde clusterschijf voor clustering van een SAP ASCS/SCS-exemplaar.

## <a name="prerequisites"></a>Vereisten

Voordat u de installatie begint, controleert u deze documenten:

* [Architectuurhandleiding voor: Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf][sap-high-availability-guide-wsfc-shared-disk]

* [Voorbereiden van de Azure-infrastructuur voor SAP HA met behulp van een Windows-failovercluster en een gedeelde schijf voor een SAP ASCS/SCS-exemplaar][sap-high-availability-infrastructure-wsfc-shared-disk]

We niet de DBMS-instellingen in dit artikel worden beschreven omdat instellingen variëren, afhankelijk van de DBMS-systeem die u gebruikt. Gaan we ervan uit dat problemen met de DBMS-systemen voor hoge beschikbaarheid met de functies die ondersteuning bieden voor Azure voor verschillende DBMS-leveranciers worden behandeld. Voorbeelden zijn AlwaysOn of database mirroring voor SQL Server en Oracle Data Guard voor Oracle-databases. In het scenario we in dit artikel gebruiken toevoegen niet we veel meer bescherming in het DBMS.

Er zijn geen speciale overwegingen bij verschillende DBMS-services met een geclusterde SAP ASCS of SCS-configuratie in Azure communiceren.

> [!NOTE]
> De installatieprocedures van SAP NetWeaver ABAP-systemen, Java-systemen en systemen ABAP + Java zijn bijna identiek. Het belangrijkste verschil is dat een SAP ABAP-systeem één ASCS-exemplaar. Het SAP-Java-systeem heeft een SCS-exemplaar. Het systeem SAP ABAP + Java heeft één ASCS-exemplaar en een SCS-exemplaar die wordt uitgevoerd in de groep dezelfde Microsoft failover-cluster. Eventuele verschillen installatie voor elke installatie van SAP NetWeaver stack worden expliciet vermeld. U kunt ervan uitgaan dat alle andere onderdelen zijn hetzelfde.  
>
>

## <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>SAP installeren met een hoge beschikbaarheid ASCS/SCS-exemplaar

> [!IMPORTANT]
> Zorg ervoor dat u niet uw wisselbestand op SIOS DataKeeper gespiegelde volumes plaatsen. DataKeeper biedt geen ondersteuning voor gespiegelde volumes. U kunt uw wisselbestand op de tijdelijke station D van een Azure-machine laat de standaardinstelling. Als deze nog niet is ingevuld, verplaatst u het wisselbestand van Windows op station D van uw virtuele machine van Azure.
>
>

SAP met een hoge beschikbaarheid ASCS/SCS-exemplaar installeert, moet deze taken uitvoeren:

* Maak een virtuele host-naam voor de geclusterde SAP ASCS/SCS-exemplaar.
* Installeer het eerste clusterknooppunt van SAP.
* Wijzig het SAP-profiel van de ASCS/SCS-exemplaar.
* Een testpoort toevoegen.
* Open de testpoort van Windows firewall.

### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>De naam van een virtuele host voor de geclusterde SAP ASCS/SCS-exemplaar maken

1. Maak een DNS-vermelding voor de virtuele host-naam van de ASCS/SCS-exemplaar in de Windows DNS-beheer.

   > [!IMPORTANT]
   > Het IP-adres dat u aan de virtuele host-naam van de ASCS/SCS-exemplaar toewijst moet hetzelfde zijn als het IP-adres dat u hebt toegewezen aan Azure Load Balancer (\<SID\>- lb-ascs).  
   >
   >

   Het IP-adres van de virtuele SAP ASCS/SCS-hostnaam (pr1-ascs-sap) is hetzelfde als het IP-adres van Azure Load Balancer (lb-pr1-ascs).

   ![Afbeelding 1: De DNS-vermelding voor de virtuele naam van SAP ASCS/SCS cluster en de TCP/IP-adres definiëren][sap-ha-guide-figure-3046]

   _**Afbeelding 1:** De DNS-vermelding voor de virtuele naam van SAP ASCS/SCS cluster en de TCP/IP-adres definiëren_

2. Voor het definiëren van het IP-adres dat toegewezen aan de virtuele host-naam, selecteer **DNS-beheer** > **domein**.

   ![Afbeelding 2: Nieuwe naam van de virtuele en TCP/IP-adres voor SAP ASCS/SCS-clusterconfiguratie][sap-ha-guide-figure-3047]

   _**Afbeelding 2:** Nieuwe naam van de virtuele en TCP/IP-adres voor SAP ASCS/SCS-clusterconfiguratie_

### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Het eerste clusterknooppunt van SAP installeren

1. Uitvoeren van de eerste optie van cluster-knooppunt op clusterknooppunt A. Bijvoorbeeld: op de pr1-ascs-0 * host.
2. Om te voorkomen dat de standaard-poorten voor de interne Azure load balancer, selecteren:

   * **ABAP system**: **ASCS** exemplaarnummer **00**
   * **Java-systeem**: **SCS** exemplaarnummer **01**
   * **ABAP + Java system**: **ASCS** exemplaarnummer **00** en **SCS** exemplaarnummer **01**

   Voor het gebruik van cijfers dan 00 exemplaar voor de ABAP ASCS-exemplaar en 01 voor de Java-SCS-exemplaar, moet u eerst de interne Azure load balancer standaard load balancer-regels wijzigen. Zie voor meer informatie, [wijzigen van de ASCS/SCS standaard load balancer-regels voor de interne Azure load balancer][sap-ha-guide-8.9].

De volgende paar taken worden niet beschreven in de documentatie van de standaard SAP-installatie.

> [!NOTE]
> De SAP-installatie-documentatie wordt beschreven hoe u het eerste ASCS/SCS-clusterknooppunt installeren.
>
>

### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Wijzig het SAP-profiel van de ASCS/SCS-exemplaar

Voeg eerst een nieuwe profiel-parameter toe. De profiel-parameter voorkomt dat verbindingen tussen SAP processen en de server in de wachtrij plaatsen af te sluiten wanneer ze niet actief is te lang zijn. We het probleem scenario in vermelden [registervermeldingen op beide clusterknooppunten van de SAP ASCS/SCS-exemplaar toe te voegen][sap-ha-guide-8.11]. In deze sectie introduceren we ook twee wijzigingen aan enkele eenvoudige parameters van de TCP/IP-verbinding. In een tweede stap, moet u om in te stellen van de server in de wachtrij plaatsen voor het verzenden van een `keep_alive` geven zodat de verbindingen niet van de interne Azure load balancer niet-actieve drempelwaarde bereikt.

De SAP-profiel van de ASCS/SCS-exemplaar wijzigen:

1. Deze parameter profiel toevoegen aan het profiel SAP ASCS/SCS-exemplaar:

   ```
   enque/encni/set_so_keepalive = true
   ```
   In ons voorbeeld is het pad:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

   Bijvoorbeeld, voor de SAP-SCS-exemplaar profiel en het bijbehorende pad:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2. De wijzigingen wilt toepassen, door het SAP ASCS/SCS-exemplaar opnieuw te starten.

### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Een testpoort toevoegen

De interne load balancer-test-functionaliteit gebruiken om de configuratie van het hele cluster werken met Azure Load Balancer. De interne Azure load balancer verdeelt meestal de inkomende belasting gelijk verdelen tussen deelnemende virtuele machines.

 Echter, dat werkt niet in sommige configuraties van clusters op omdat er slechts één exemplaar actief is. Het andere exemplaar is passieve en een van de werkbelasting kan accepteren. Een test-functionaliteit helpt bij de interne Azure load balancer werken alleen met een actief exemplaar wijst. De interne load balancer kan met de functionaliteit test detecteren welke instanties actief zijn en vervolgens de doelinstantie van alleen de met de werkbelasting.

Een testpoort toevoegen:

1. Controleer de huidige **ProbePort** waarde door het uitvoeren van de volgende PowerShell-opdracht:

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
   ```

   De opdracht uit in een van de virtuele machines worden uitgevoerd in de configuratie van het cluster.

2. Een testpoort definiëren. Het standaardpoortnummer voor de test is 0. In ons voorbeeld gebruiken we de testpoort 62000.

   ![Afbeelding 3: De testpoort van de cluster-configuratie is 0 standaard][sap-ha-guide-figure-3048]

   _**Afbeelding 3:** De standaardpoort cluster configuration test is 0_

   Het poortnummer dat is gedefinieerd in SAP Azure Resource Manager-sjablonen. U kunt het poortnummer in PowerShell.

   Een nieuwe ProbePort waarde instellen voor de SAP \<SID\> cluster-IP-bron, voer het volgende PowerShell-script voor het bijwerken van de PowerShell-variabelen voor uw omgeving:

   ```powershell
   $SAPSID = "PR1"      # SAP <SID>
   $ProbePort = 62000   # ProbePort of the Azure internal load balancer

   Clear-Host
   $SAPClusterRoleName = "SAP $SAPSID"
   $SAPIPresourceName = "SAP $SAPSID IP"
   $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
   $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
   $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
   $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
   $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
   $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
   $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

   $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

   Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
   Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

   Write-Host
   Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
   Write-Host
   Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
   Write-Host

   $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

   Write-Host

   $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

   if($ActivateChanges -eq "yes"){
   Write-Host
   Write-Host "Activating changes..." -ForegroundColor Cyan

   Write-Host
   write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
   Stop-ClusterResource -Name $SAPIPresourceName
   sleep 5

   Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
   Start-ClusterGroup -Name $SAPClusterRoleName

   Write-Host "New ProbePort parameter is active." -ForegroundColor Green
   Write-Host

   Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
   Write-Host
   Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
   }else
   {
   Write-Host "Changes are not activated."
   }
   ```

   Nadat u de SAP brengt \<SID\> cluster online rol, Controleer **ProbePort** is ingesteld op de nieuwe waarde.

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

   ```
   Nadat het script wordt uitgevoerd, wordt u gevraagd om opnieuw te starten van de SAP-clustergroep voor het activeren van de wijzigingen.

   ![Afbeelding 4: De cluster testpoort na het instellen van de nieuwe waarde][sap-ha-guide-figure-3049]

   _**Afbeelding 4:** De cluster testpoort na het instellen van de nieuwe waarde_

### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open de Windows firewall-test-poort

Open een Windows firewall-testpoort op beide knooppunten van het cluster. Gebruik het volgende script om een Windows firewall-testpoort te openen. De PowerShell-variabelen voor uw omgeving bijwerken.

  ```powershell
  $ProbePort = 62000   # ProbePort of the Azure internal load balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

**ProbePort** is ingesteld op **62000**. Nu u toegang hebt tot de bestandsshare \\\ascsha-clsap\sapmnt met andere hosts, zoals van ascsha DBA.

## <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Het database-exemplaar installeren

Volg het proces dat wordt beschreven in de documentatie van de SAP-installatie voor het installeren van de database-instantie.

## <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Het tweede clusterknooppunt installeren

Volg de stappen die worden beschreven in de SAP-installatiehandleiding voor het installeren van het tweede cluster.

## <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Wijzig het starttype van de SAP INGEN Windows service-exemplaar

Wijzig het starttype van de SAP INGEN Windows-service naar **automatisch (vertraagd starten)** op beide knooppunten van het cluster.

![Afbeelding 5: Wijzig het type van de service voor het exemplaar van de SAP-klanten in vertraagd automatisch][sap-ha-guide-figure-3050]

_**Afbeelding 5:** Wijzig het type van de service voor het exemplaar van de SAP-klanten in vertraagd automatisch_

## <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> De primaire SAP-toepassingsserver installeren

Het exemplaar van de primaire toepassing Server (PAS) installeren \<SID\>-di-0 op de virtuele machine die u hebt opgegeven voor het hosten van de Pa's. Er zijn geen afhankelijkheden op Azure. Er zijn geen DataKeeper-specifieke instellingen.

## <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> De aanvullende SAP-toepassingsserver installeren

Installeer een SAP extra Application Server (AAS) op alle virtuele machines die u hebt opgegeven voor het hosten van een exemplaar van de SAP-toepassingsserver. Bijvoorbeeld: op \<SID\>-di-1 tot en met \<SID\>- di -&lt;n&gt;.

> [!NOTE]
> De installatie van een hoge beschikbaarheid SAP NetWeaver-systeem wordt voltooid. Vervolgens gaat u verder met het testen van failover.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> De failover-SAP ASCS/SCS-exemplaar en SIOS replicatie testen
Het is gemakkelijk om te testen en een failover-SAP ASCS/SCS-exemplaar en SIOS schijfreplicatie bewaken met behulp van Failoverclusterbeheer en de SIOS DataKeeper Management en configuratie-hulpprogramma.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS-exemplaar wordt uitgevoerd op een clusterknooppunt A

De clustergroep SAP PR1 wordt uitgevoerd op een clusterknooppunt A. Bijvoorbeeld: op pr1-ascs-0. De gedeelde schijf S, die deel van de clustergroep SAP PR1 uitmaakt, toewijzen aan clusterknooppunt A. De ASCS/SCS-exemplaar maakt ook gebruik van schijf S. 

![Afbeelding 6: Failover Cluster Manager: De SAP \<SID\> clustergroep wordt uitgevoerd op een clusterknooppunt A][sap-ha-guide-figure-5000]

_**Afbeelding 6:** Failover Cluster Manager: De SAP \<SID\> clustergroep wordt uitgevoerd op een clusterknooppunt A_

In het hulpprogramma SIOS DataKeeper beheer en de configuratie kunt u zien dat de gegevens van de gedeelde schijf is asynchroon worden gerepliceerd vanaf de bron volume station S op clusterknooppunt A naar het doel volume station S op clusterknooppunt B. Er worden bijvoorbeeld gerepliceerd van pr1-ascs-0 [10.0.0.40] naar pr1-ascs-1 [10.0.0.41].

![Afbeelding 7: Repliceren in SIOS DataKeeper, het lokale volume van clusterknooppunt A naar B-clusterknooppunt][sap-ha-guide-figure-5001]

_**Afbeelding 7:** Repliceren in SIOS DataKeeper, het lokale volume van clusterknooppunt A naar B-clusterknooppunt_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover van knooppunt A naar B-knooppunt

1. Kies een van deze opties om een failover van de SAP- \<SID\> clustergroep van het clusterknooppunt een clusterknooppunt B:
   - Failover-clusterbeheer  
   - Failover-Cluster PowerShell

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPClusterGroup = "SAP $SAPSID"
   Move-ClusterGroup -Name $SAPClusterGroup

   ```
2. Start opnieuw op een van de cluster-knooppunt in de Windows-gastbesturingssysteem. Hiermee initieert een automatische failover van de SAP \<SID\> clustergroep van knooppunt A naar knooppunt B.  
3. Start opnieuw op een van de cluster-knooppunt vanuit Azure portal. Hiermee initieert een automatische failover van de SAP \<SID\> clustergroep van knooppunt A naar knooppunt B.  
4. Start opnieuw op cluster-knooppunt A met behulp van Azure PowerShell. Hiermee initieert een automatische failover van de SAP \<SID\> clustergroep van knooppunt A naar knooppunt B.

   Na een failover van de SAP \<SID\> clustergroep wordt uitgevoerd op een clusterknooppunt B. Deze wordt bijvoorbeeld uitgevoerd op pr1-ascs-1.

   ![Afbeelding 8: In Failoverclusterbeheer, de SAP \<SID\> clustergroep wordt uitgevoerd op een clusterknooppunt B][sap-ha-guide-figure-5002]

   _**Afbeelding 8**: In Failoverclusterbeheer, de SAP \<SID\> clustergroep wordt uitgevoerd op een clusterknooppunt B_

   De gedeelde schijf is nu gekoppeld op cluster knooppunt B. SIOS DataKeeper is die gegevens van de bron volume station S op clusterknooppunt B naar volume doelstation S op clusterknooppunt A. Er wordt bijvoorbeeld repliceren van pr1-ascs-1 [10.0.0.41] naar pr1-ascs-0 [10.0.0.40].

   ![Afbeelding 9: SIOS DataKeeper worden gerepliceerd van het lokale volume van het clusterknooppunt B aan het cluster een knooppunt][sap-ha-guide-figure-5003]

   _**Afbeelding 9:** SIOS DataKeeper worden gerepliceerd van het lokale volume van het clusterknooppunt B aan het cluster een knooppunt_
