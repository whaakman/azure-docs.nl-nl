---
title: Een HPC Pack-hoofdknooppunt in een Azure-VM maken | Microsoft Docs
description: Informatie over het gebruik van de Azure-portal en het Resource Manager-implementatiemodel te maken van een Microsoft HPC Pack 2012 R2-hoofdknooppunt in een Azure-VM.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,hpc-pack
ms.assetid: e6a13eaf-9124-47b4-8d75-2bc4672b8f21
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 70c1d95f704315ee6a481367188e2bb620916702
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428953"
---
# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Het hoofdknooppunt van een HPC Pack-cluster maken in een Azure VM met een Marketplace-installatiekopie
Gebruik een [Microsoft HPC Pack 2012 R2 virtual machine-installatiekopie](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) van de Azure Marketplace en de Azure-portal te maken van het hoofdknooppunt van een HPC-cluster. Deze HPC Pack VM-installatiekopie is gebaseerd op Windows Server 2012 R2 Datacenter met HPC Pack 2012 R2 Update 3 vooraf zijn geïnstalleerd. Gebruik deze hoofdknooppunt voor een bewijs van concept-implementatie van HPC Pack in Azure. U kunt vervolgens compute-knooppunten toevoegen aan het cluster voor het uitvoeren van HPC-workloads.

> [!TIP]
> Voor het implementeren van een volledige HPC Pack 2012 R2-cluster in Azure met het hoofdknooppunt en compute-knooppunten, raden wij aan dat u een geautomatiseerde methode. Opties zijn onder andere de [HPC Pack IaaS-implementatiescript](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) en Resource Manager-sjablonen, zoals de [HPC Pack-cluster voor Windows werkbelasting](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/). Resource Manager-sjablonen zijn ook beschikbaar voor [Microsoft HPC Pack 2016-clusters](https://github.com/MsHpcPack/HPCPack2016/tree/master/newcluster-templates). 
> 
> 

## <a name="planning-considerations"></a>Aandachtspunten voor de planning
In de volgende afbeelding ziet u het hoofdknooppunt van HPC Pack in een Active Directory-domein in een Azure-netwerk implementeren.

![Hoofdknooppunt van HPC Pack][headnode]

* **Active Directory-domein**: de HPC Pack 2012 R2 hoofdknooppunt moet worden toegevoegd aan Active Directory-domein in Azure voordat u begint met de HPC-services op de virtuele machine. Zoals in dit artikel voor een bewijs van concept-implementatie, kunt u de virtuele machine die u maakt voordat u begint met de HPC-services als een domeincontroller voor het hoofdknooppunt promoveren. Een andere optie is een afzonderlijke domeincontroller en het forest in Azure waarop u deelnemen aan het hoofdknooppunt VM implementeren.

* **Implementatiemodel**: voor de meeste nieuwe implementaties, wordt aangeraden dat u de Resource Manager-implementatiemodel. In dit artikel wordt ervan uitgegaan dat u dit implementatiemodel.

* **Azure-netwerk**: wanneer u het implementatiemodel van Resource Manager gebruikt voor het implementeren van het hoofdknooppunt, u opgeven of een Azure-netwerk maken. U kunt het virtuele netwerk gebruiken als u wilt deelnemen aan het hoofdknooppunt aan een bestaand Active Directory-domein. U moet ook later naar de compute-knooppunt VM's toevoegen aan het cluster.

## <a name="steps-to-create-the-head-node"></a>Stappen voor het maken van het hoofdknooppunt
Hieronder vindt u belangrijke stappen van de Azure-portal gebruiken om te maken van een Azure-VM voor het hoofdknooppunt van HPC Pack met behulp van de Resource Manager-implementatiemodel. 

1. Als u een nieuw Active Directory-forest in Azure maken met afzonderlijke domeincontroller virtuele machines wilt, een optie te gebruiken is een [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc). Voor een eenvoudige bewijs van concept-implementatie is het prima om deze stap overslaan en het hoofdknooppunt van virtuele machine zelf configureren als een domeincontroller. Deze optie wordt verderop in dit artikel beschreven.
1. Op de [HPC Pack 2012 R2 op Windows Server 2012 R2-pagina](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) in de Azure Marketplace, klikt u op **virtuele Machine maken**. 
1. In de portal op de **HPC Pack 2012 R2 op Windows Server 2012 R2** weergeeft, schakelt de **Resource Manager** implementatiemodel en klik vervolgens op **maken**.
   
    ![Afbeelding van HPC Pack][marketplace]
1. De portal gebruiken voor het configureren van de instellingen en de virtuele machine maken. Als u geen ervaring met Azure, volgt u de zelfstudie [maken van een Windows-machine in Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Voor een bewijs van concept-implementatie, kan gewoonlijk de standaardinstelling accepteren of aanbevolen instellingen.
   
   > [!NOTE]
   > Als u deelnemen aan het hoofdknooppunt aan een bestaand Active Directory-domein in Azure wilt, zorg ervoor dat u het virtuele netwerk voor dat domein opgeeft bij het maken van de virtuele machine.
   > 
   > 
1. Nadat u de virtuele machine maken en de virtuele machine wordt uitgevoerd, [verbinding maken met de virtuele machine](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met extern bureaublad. 
1. De virtuele machine toevoegen aan een Active Directory-domein-forest door een van de volgende opties te kiezen:
   
   * Als u de virtuele machine hebt gemaakt in een Azure-netwerk met een bestaand domein-forest, deelnemen aan de virtuele machine aan het forest met standaardprogramma's voor Serverbeheer of Windows PowerShell. Start vervolgens opnieuw.
   * Als u de virtuele machine in een nieuw virtueel netwerk (zonder een bestaand domein-forest) hebt gemaakt, kunt u vervolgens de virtuele machine als een domeincontroller promoveren. Gebruik standaard stappen om te installeren en configureren van de Active Directory Domain Services-rol op het hoofdknooppunt. Zie voor gedetailleerde stappen [installeren van een nieuwe Windows Server 2012 Active Directory-Forest](https://technet.microsoft.com/library/jj574166.aspx).
1. Nadat de virtuele machine wordt uitgevoerd en is gekoppeld aan een Active Directory-forest, start u de services HPC Pack als volgt:
   
    a. Verbinding maken met de virtuele machine met behulp van een domeinaccount dat lid is van de lokale groep Administrators van het hoofdknooppunt. Bijvoorbeeld, gebruik het beheerdersaccount dat u hebt ingesteld toen u het hoofdknooppunt VM gemaakt.
   
    b. Voor een standaardconfiguratie hoofdknooppunt, start u Windows PowerShell als beheerder en typ het volgende:
   
    ```PowerShell
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```
   
    Het kan enige tijd duren voor de HPC Pack-services te starten.
   
    Voor extra hoofdknooppunt configuratieopties, typt u `get-help HPCHNPrepare.ps1`.

## <a name="next-steps"></a>Volgende stappen
* Nu kunt u werken met het hoofdknooppunt van uw HPC Pack-cluster. Bijvoorbeeld, HPC Cluster Manager start en voltooi de [implementatie takenlijst](https://technet.microsoft.com/library/jj884141.aspx).
* Als u verhogen van het cluster compute-capaciteit wilt op aanvraag, Voeg [Azure burst knooppunten](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) in een cloudservice. 
* Probeer een werkbelasting test uitgevoerd op het cluster. Zie voor een voorbeeld: de HPC Pack [introductiehandleiding](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/hpcpack-cluster-headnode/marketplace.png
