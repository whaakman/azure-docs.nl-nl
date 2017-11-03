---
title: PowerShell-script voor het Linux-HPC-cluster implementeren | Microsoft Docs
description: Voer een PowerShell-script voor het implementeren van een Linux HPC Pack 2012 R2-cluster in Azure virtuele machines
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 73041960-58d3-4ecf-9540-d7e1a612c467
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: c15dc66718a855e22f8109448cb8c8a23787b9bf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Een high performance computing (HPC)-cluster op Linux maken met het implementatiescript HPC Pack IaaS
De implementatie van HPC Pack IaaS PowerShell-script voor het implementeren van een volledige HPC Pack 2012 R2-cluster voor Linux-werkbelastingen in virtuele machines in Azure worden uitgevoerd. Het cluster bestaat uit een op die lid zijn van Active Directory hoofdknooppunt met Windows Server en Microsoft HPC Pack en rekenknooppunten die worden uitgevoerd op een van de Linux-distributies die wordt ondersteund door HPC Pack. Als u een HPC Pack cluster in Azure voor Windows werkbelasting implementeren wilt, Zie [een Windows HPC-cluster maken met het implementatiescript HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). U kunt ook een Azure Resource Manager-sjabloon gebruiken voor het implementeren van een cluster HPC Pack. Zie voor een voorbeeld [een HPC-cluster maken met Linux-rekenknooppunten](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

> [!IMPORTANT] 
> Het PowerShell-script dat wordt beschreven in dit artikel maakt een Microsoft HPC Pack 2012 R2-cluster in Azure met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> Het script dat wordt beschreven in dit artikel biedt bovendien geen ondersteuning voor HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Voorbeeld-configuratiebestand
Het volgende configuratiebestand een domeincontroller en de domein-forest maakt en implementeert een HPC Pack-cluster met één hoofdknooppunt met lokale databases en 10 Linux-rekenknooppunten. Alle cloud-services worden gemaakt rechtstreeks op de locatie Oost-Azië. De Linux-rekenknooppunten in twee cloudservices en twee storage-accounts worden gemaakt (dat wil zeggen, *MyLnxCN 0001* naar *MyLnxCN 0005* in *MyLnxCNService01* en *mylnxstorage01*, en *MyLnxCN 0006* naar *MyLnxCN 0010* in *MyLnxCNService02* en *mylnxstorage02*). De rekenknooppunten worden gemaakt van een installatiekopie van een OpenLogic CentOS versie 7.0 Linux. 

Vervangt door uw eigen waarden voor de abonnementsnaam van uw en de namen van de account en de service.

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
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Problemen oplossen
* **Fout 'Bestaat niet VNet'**. Als u het script voor de implementatie van HPC Pack IaaS voor het implementeren van meerdere clusters in Azure gelijktijdig in één abonnement hebt uitgevoerd, een of meer implementaties mislukken mogelijk met de fout ' VNet *VNet\_naam* bestaat niet ".
  Als deze fout optreedt, voert u het script voor de mislukte implementatie opnieuw uit.
* **Probleem met het openen van het Internet van de virtuele Azure-netwerk**. Als u een cluster HPC Pack met een nieuwe domeincontroller maakt met behulp van het script voor implementatie, of u handmatig een hoofdknooppunt VM naar domeincontroller promoveren, kunt u problemen die de virtuele machines in Azure virtuele netwerk verbinding met Internet ondervinden. Dit kan gebeuren als een DNS-doorstuurserver server automatisch geconfigureerd op de domeincontroller en deze server van de DNS-doorstuurserver kan niet correct worden omgezet.
  
    Dit probleem, meld u aan bij de domeincontroller en een verwijderen van de doorstuurserver configuratie-instelling omzeilen of een geldige doorstuurserver DNS-server configureren. Om dit te doen, klik in Serverbeheer op **extra** > **DNS** voor het openen van de DNS-beheer en dubbelklikt u vervolgens op **doorstuurservers**.

## <a name="next-steps"></a>Volgende stappen
* Zie [aan de slag met Linux-rekenknooppunten in een cluster HPC Pack Azure](hpcpack-cluster.md) voor informatie over ondersteunde Linux-distributies, verplaatsen van gegevens en verzenden van taken naar een cluster HPC Pack met Linux rekenknooppunten.
* Voor de zelfstudies die gebruikmaken van het script voor het maken van een cluster en een Linux HPC-werkbelasting uitvoeren, Zie:
  * [Voer NAMD met Microsoft HPC Pack op Linux-rekenknooppunten in Azure](hpcpack-cluster-namd.md)
  * [Voer OpenFOAM met Microsoft HPC Pack op Linux-rekenknooppunten in Azure](hpcpack-cluster-openfoam.md)
  * [Uitvoeren van de STER-CCM + met Microsoft HPC Pack op Linux rekenknooppunten in Azure](hpcpack-cluster-starccm.md)

