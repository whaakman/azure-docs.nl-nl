---
title: PowerShell-script voor het implementeren van Windows HPC-cluster | Microsoft Docs
description: Voer een PowerShell-script voor het implementeren van een Windows HPC Pack 2012 R2-cluster in Azure virtuele machines
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 286b2be8-2533-40df-b02a-26156b1f1133
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 85b125ab19671b61d2541af6378c95feb88bf952
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Maken van een Windows cluster-high performance computing (HPC) met het implementatiescript HPC Pack IaaS
De implementatie van HPC Pack IaaS PowerShell-script voor het implementeren van een volledige HPC Pack 2012 R2-cluster voor Windows werkbelasting in Azure virtuele machines worden uitgevoerd. Het cluster bestaat uit een op die lid zijn van Active Directory hoofdknooppunt met Windows Server en Microsoft HPC Pack en aanvullende Windows-rekenresources die u opgeeft. Als u een HPC Pack cluster in Azure voor Linux-werkbelastingen implementeren wilt, Zie [een Linux-HPC-cluster maken met het implementatiescript HPC Pack IaaS](../../linux/classic/hpcpack-cluster-powershell-script.md). U kunt ook een Azure Resource Manager-sjabloon gebruiken voor het implementeren van een cluster HPC Pack. Zie voor voorbeelden [maken van een HPC-cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) en [een HPC-cluster maken met een installatiekopie van het knooppunt aangepaste rekenservice](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

> [!IMPORTANT] 
> Het PowerShell-script dat wordt beschreven in dit artikel maakt een Microsoft HPC Pack 2012 R2-cluster in Azure met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> Het script dat wordt beschreven in dit artikel biedt bovendien geen ondersteuning voor HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Voorbeeld van de configuratiebestanden
Vervangen door uw eigen waarden voor uw abonnements-Id of naam en de namen van de account en -service in de volgende voorbeelden.

### <a name="example-1"></a>Voorbeeld 1
Het volgende configuratiebestand implementeert een HPC Pack-cluster met een hoofdknooppunt met lokale databases en vijf rekenknooppunten waarop het besturingssysteem Windows Server 2012 R2. Alle cloud-services worden gemaakt rechtstreeks op de locatie VS-West. Het hoofdknooppunt fungeert als domeincontroller in het domein-forest.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Voorbeeld 2
Het volgende configuratiebestand implementeert een cluster HPC Pack in een bestaand domeinforest. Het cluster heeft 1 hoofdknooppunt met lokale databases en 12 rekenknooppunten met de BGInfo-VM-extensie toegepast.
Automatische installatie van Windows-updates is uitgeschakeld voor alle VM's in het domein-forest. Alle cloud-services worden gemaakt rechtstreeks op de locatie Oost-Azië. De compute nodes in drie cloudservices en drie storage-accounts worden gemaakt: *MyHPCCN 0001* naar *MyHPCCN 0005* in *MyHPCCNService01* en  *mycnstorage01*; *MyHPCCN 0006* naar *MyHPCCN0010* in *MyHPCCNService02* en *mycnstorage02*; en  *MyHPCCN-0011* naar *MyHPCCN 0012* in *MyHPCCNService03* en *mycnstorage03*). De rekenknooppunten worden gemaakt vanuit een bestaande persoonlijke installatiekopie vastgelegd vanaf een rekenknooppunt. Automatisch vergroten of verkleinen-service is ingeschakeld met standaard vergroten of verkleinen intervallen.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
      </DomainController>
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Voorbeeld 3
Het volgende configuratiebestand implementeert een cluster HPC Pack in een bestaand domeinforest. Het cluster bevat één hoofdknooppunt, een databaseserver met een gegevensschijf 500 GB, twee broker knooppunten waarop het besturingssysteem Windows Server 2012 R2 en vijf rekenknooppunten waarop het besturingssysteem Windows Server 2012 R2. De cloudservice MyHPCCNService wordt gemaakt in de affiniteitsgroep *MyIBAffinityGroup*, en de cloudservices worden gemaakt in de affiniteitsgroep *MyAffinityGroup*. De HPC Job Scheduler REST API- en HPC-webportal zijn ingeschakeld op het hoofdknooppunt.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Voorbeeld 4
Het volgende configuratiebestand implementeert een cluster HPC Pack in een bestaand domeinforest. Het cluster heeft twee hoofdknooppunt met lokale databases, twee Azure knooppunt sjablonen zijn gemaakt en drie grootte normaal Azure knooppunten worden gemaakt voor Azure knooppuntsjabloon *AzureTemplate1*. Een scriptbestand wordt uitgevoerd op het hoofdknooppunt nadat het hoofdknooppunt is geconfigureerd.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Problemen oplossen
* **Fout 'Bestaat niet VNet'** -als u het script voor het implementeren van meerdere clusters in Azure gelijktijdig in één abonnement hebt uitgevoerd, een of meer implementaties mislukken mogelijk met de fout ' VNet *VNet\_naam* bestaat niet '.
  Als deze fout optreedt, moet u het script opnieuw voor de mislukte implementatie uitvoeren.
* **Probleem met het openen van het Internet van de virtuele Azure-netwerk** : als u een cluster met een nieuwe domeincontroller met behulp van het implementatiescript maakt of u handmatig een hoofdknooppunt VM naar domeincontroller promoveren, problemen kunnen verbinding maken met de Virtuele machines met het Internet. Dit probleem kan optreden als een DNS-doorstuurserver server automatisch geconfigureerd op de domeincontroller en deze server van de DNS-doorstuurserver kan niet correct worden omgezet.
  
    Dit probleem, meld u aan bij de domeincontroller en een verwijderen van de doorstuurserver configuratie-instelling omzeilen of een geldige doorstuurserver DNS-server configureren. Deze instelling configureren in Serverbeheer op **extra** >
    **DNS** voor het openen van de DNS-beheer en dubbelklikt u vervolgens op **doorstuurservers**.
* **Geen toegang tot de RDMA-netwerk van virtuele machines rekenintensieve** : als u Windows Server compute toevoegen of broker knooppunt het formaat van virtuele machines met een RDMA-compatibel zoals A8 of A9, u deze virtuele machines verbinding met het netwerk van de toepassing RDMA problemen ondervinden. Een reden dat dit probleem optreedt, is als de HpcVmDrivers-extensie is niet juist geïnstalleerd wanneer de virtuele machines worden toegevoegd aan het cluster. Bijvoorbeeld: de extensie kan blijven steken in de status van de installatie.
  
    U kunt dit probleem omzeilen, Controleer u eerst de status van de extensie in de virtuele machines. Als de extensie is niet juist is geïnstalleerd, verwijdert u de knooppunten van de HPC-cluster en voegt u de knooppunten opnieuw toe. U kunt bijvoorbeeld rekenknooppunt virtuele machines toevoegen door te voeren van het script toevoegen HpcIaaSNode.ps1 op het hoofdknooppunt.

## <a name="next-steps"></a>Volgende stappen
* Probeer een werkbelasting test uit te voeren op het cluster. Zie voor een voorbeeld: de HPC Pack [instructiehandleiding](https://technet.microsoft.com/library/jj884144).
* Zie voor een zelfstudie voor het script van de implementatie van het cluster en het uitvoeren van een HPC-werkbelasting [aan de slag met een HPC Pack-cluster in Azure uitvoeren van Excel- en SOA-werkbelastingen](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Probeer HPC Pack van hulpprogramma's voor het starten, stoppen, toevoegen en verwijderen van rekenknooppunten uit een cluster dat u maakt. Zie [rekenknooppunten beheren in een Pack HPC-cluster in Azure](hpcpack-cluster-node-manage.md).
* Als u wilt ophalen instellen om taken te verzenden naar het cluster van een lokale computer, Zie [indienen HPC-taken uit een on-premises computer naar een HPC Pack-cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

