---
title: Azure SQL VM CLI gebruiken voor het configureren van AlwaysOn-beschikbaarheidsgroep voor SQL Server op een Azure VM
description: 'Azure CLI gebruiken voor het maken van het Windows-failovercluster, de beschikbaarheidsgroep-listener en de interne Load Balancer op een SQL Server-VM in Azure. '
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/12/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 058ed349e1aeb17dea7d550b9760082b464453f1
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57404125"
---
# <a name="use-azure-sql-vm-cli-to-configure-always-on-availability-group-for-sql-server-on-an-azure-vm"></a>Azure SQL VM CLI gebruiken voor het configureren van AlwaysOn-beschikbaarheidsgroep voor SQL Server op een Azure VM
In dit artikel wordt beschreven hoe u [Azure SQL VM CLI](https://docs.microsoft.com/mt-mt/cli/azure/ext/sqlvm-preview/sqlvm?view=azure-cli-2018-03-01-hybrid) voor het implementeren van een Windows Failover Cluster (WSFC), en SQL Server-VM's toevoegen aan het cluster, evenals de interne Load Balancer en voor een AlwaysOn-beschikbaarheidsgroep-listener maken.  De werkelijke implementatie van de AlwaysOn-beschikbaarheidsgroep is nog steeds handmatig uitgevoerd via SQL Server Management Studio (SSMS). 

## <a name="prerequisites"></a>Vereisten
Voor het automatiseren van de installatie van een AlwaysOn-beschikbaarheidsgroep met behulp van Azure SQL VM CLI, hebt u al de volgende vereisten: 
- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- Een resourcegroep met een domeincontroller. 
- Een of meer domein [virtuele machines in Azure uitgevoerd SQL Server 2016 (of hoger) Enterprise edition](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) in de *dezelfde beschikbaarheidsset of verschillende beschikbaarheidszones* die zijn [geregistreerd met de SQL-VM-resourceprovider](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).  
 
## <a name="create-storage-account-as-a-cloud-witness"></a>Storage-account maken als een cloud-witness
Het cluster moet een storage-account om te fungeren als de cloud-witness. U kunt een bestaand opslagaccount gebruiken of u kunt een nieuw opslagaccount maken. Als u een bestaand opslagaccount gebruiken wilt, gaat u verder met de volgende sectie. 

Het storage-account wordt gemaakt in het volgende codefragment: 
```cli
# Create the storage account
# example: az storage account create -n 'cloudwitness' -g SQLVM-RG -l 'West US' `
#  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true

az storage account create -n <name> -g <resource group name> -l <region ex:eastus> `
  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true
```

   >[!TIP]
   > U ziet mogelijk de fout `az sql: 'vm' is not in the 'az sql' command group` als u een verouderde versie van Azure CLI. Download de [meest recente versie van Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) om deze fout.

## <a name="define-windows-failover-cluster-metadata"></a>De metagegevens van de Windows-failovercluster definiëren
De Azure SQL VM-CLI [az sql vm-groep](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest) opdracht groep beheert de metagegevens van de Windows Failover Cluster (WSFC)-service die als host fungeert voor de beschikbaarheidsgroep. Metagegevens van clusters bevat de AD-domein, de cluster-accounts, het storage-accounts moet worden gebruikt als cloud-witness en de versie van SQL Server. Gebruik [az sql vm-groep maken](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest#az-sql-vm-group-create) voor het definiëren van de metagegevens voor de WSFC zodat wanneer de eerste virtuele machine voor SQL-Server wordt toegevoegd, het cluster is gemaakt, zoals is gedefinieerd. 

Het volgende codefragment worden de metagegevens voor het cluster gedefinieerd:
```cli
# Define the cluster metadata
# example: az sql vm group create -n Cluster -l 'West US' -g SQLVM-RG `
#  --image-offer SQL2017-WS2016 --image-sku Enterprise --domain-fqdn domain.com `
#  --operator-acc vmadmin@domain.com --bootstrap-acc vmadmin@domain.com --service-acc sqlservice@domain.com `
#  --sa-key '4Z4/i1Dn8/bpbseyWX' `
#  --storage-account 'https://cloudwitness.blob.core.windows.net/'

az sql vm group create -n <cluster name> -l <region ex:eastus> -g <resource group name> `
  --image-offer <SQL2016-WS2016 or SQL2017-WS2016> --image-sku Enterprise --domain-fqdn <FQDN ex: domain.com> `
  --operator-acc <domain account ex: testop@domain.com> --bootstrap-acc <domain account ex:bootacc@domain.com> `
  --service-acc <service account ex: testservice@domain.com> `
  --sa-key '<PublicKey>' `
  --storage-account '<ex:https://cloudwitness.blob.core.windows.net/>'
```

## <a name="add-sql-server-vms-to-cluster"></a>SQL Server-VM's aan cluster toevoegen
De eerste virtuele machine voor SQL-Server toevoegen aan het cluster maakt het cluster. De [az sql vm-naar-groep toevoegen](https://docs.microsoft.com/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-add-to-group) opdracht maakt het cluster met de naam die eerder vermeld, wordt de clusterrol geïnstalleerd op de SQL Server-VM's en voegt deze toe aan het cluster. Volgende maakt gebruik van de `az sql vm add-to-group` opdracht voegt aanvullende SQL Server-VM's toe aan het nieuwe cluster. 

Het volgende codefragment maakt het cluster en de eerste virtuele machine voor SQL-Server toegevoegd aan het: 

```cli
# Add SQL Server VMs to cluster
# example: az sql vm add-to-group -n SQLVM1 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!
# example: az sql vm add-to-group -n SQLVM2 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

az sql vm add-to-group -n <VM1 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
az sql vm add-to-group -n <VM2 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
```
Gebruik deze opdracht een andere SQL Server-VM's toevoegen aan het cluster alleen wijzigen de `-n` parameter voor de SQL Server-VM-naam. 

## <a name="create-availability-group"></a>Beschikbaarheidsgroep maken
De beschikbaarheidsgroep handmatig maken zoals u gewend bent, met behulp van [SQL Server Management Studio](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio), [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell), of [Transact-SQL](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql). 

  >[!IMPORTANT]
  > Voer **niet** een listener op dit moment niet maken omdat dit u via Azure CLI in de volgende secties doet.  

## <a name="create-internal-load-balancer"></a>Interne Load Balancer maken

De Always On (AG) beschikbaarheidsgroeplistener is een interne Azure Load Balancer (ILB) vereist. De ILB biedt een 'zwevende' IP-adres voor de AG-listener die voor een snellere failover en opnieuw verbinden zorgt. Als de SQL Server-VM's in een beschikbaarheidsgroep deel van dezelfde beschikbaarheidsset bevinden, uitmaken kunt u een Basic Load Balancer. anders moet u een Standard Load Balancer gebruiken.  **De ILB moet zich in hetzelfde vNet als de SQL Server-VM-exemplaren.** 

Het volgende codefragment wordt de interne Load Balancer:

```cli
# Create the Internal Load Balancer
# example: az network lb create --name sqlILB -g SQLVM-RG --sku Standard `
# --vnet-name SQLVMvNet --subnet default

az network lb create --name sqlILB -g <resource group name> --sku Standard `
  --vnet-name <VNet Name> --subnet <subnet name>
```

  >[!IMPORTANT]
  > De openbare IP-adresresource voor elke virtuele machine van SQL Server moet een standaard-SKU voor compatibiliteit met de Standard Load Balancer hebben. Om te bepalen van de SKU van de openbare IP-resource van de virtuele machine, gaat u naar uw **resourcegroep**, selecteer uw **openbaar IP-adres** resource voor de gewenste SQL Server-VM, en zoek de waarde onder **SKU**  van de **overzicht** deelvenster.  

## <a name="create-availability-group-listener"></a>-Listener voor beschikbaarheidsgroep maken
Nadat de beschikbaarheidsgroep handmatig is gemaakt, kunt u de listener met behulp van [az sql vm ag-listener](https://docs.microsoft.com/cli/azure/sql/vm/group/ag-listener?view=azure-cli-latest#az-sql-vm-group-ag-listener-create). 


- De **subnet resource-ID** is de waarde van `/subnets/<subnetname>` toegevoegd aan de resource-ID van de vNet-resource. Voor het identificeren van het subnet resource-ID, het volgende doen:
   1. Navigeer naar uw resourcegroep in de [Azure-portal](https://portal.azure.com). 
   1. Selecteer de vNet-resource. 
   1. Selecteer **eigenschappen** in de **instellingen** deelvenster. 
   1. Identificeren van de resource-ID voor de vNet- en toevoeg- `/subnets/<subnetname>`aan het einde van het maken van het subnet resource-ID. Bijvoorbeeld:
        - Mijn vNet-resource-ID is `/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet`.
        - De subnetnaam van mijn is `default`.
        - Mijn resource-ID van het subnet is daarom `/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default`


Het volgende codefragment wordt de beschikbaarheidsgroep-listener maken:

```cli
# Create the AG listener
# example: az sql vm group ag-listener create -n AGListener -g SQLVM-RG `
#  --ag-name SQLAG --group-name Cluster --ip-address 10.0.0.27 `
#  --load-balancer sqlilb --probe-port 59999  `
#  --subnet /subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default `
#  --sqlvms sqlvm1 sqlvm2

az sql vm group ag-listener create -n <listener name> -g <resource group name> `
  --ag-name <availability group name> --group-name <cluster name> --ip-address <ag listener IP address> `
  --load-balancer <lbname> --probe-port <Load Balancer probe port, default 59999>  `
  --subnet <subnet resource id> `
  --sqlvms <names of SQL VM’s hosting AG replicas ex: sqlvm1 sqlvm2>
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server-machine](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server-VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [Opmerkingen bij de release van SQL Server-VM](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [Switch licentiemodellen voor een SQL Server-VM](virtual-machines-windows-sql-ahb.md)
* [Overzicht van de Always On Availability Groups &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)   
* [Configuratie van een Server-exemplaar voor Always On Availability Groups &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server)   
* [Beheer van een beschikbaarheidsgroep &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server)   
* [Bewaking van beschikbaarheidsgroepen &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server)
* [Overzicht van Transact-SQL-instructies voor Always On Availability Groups &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups)   
* [Overzicht van PowerShell-Cmdlets voor Always On Availability Groups &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server)  
