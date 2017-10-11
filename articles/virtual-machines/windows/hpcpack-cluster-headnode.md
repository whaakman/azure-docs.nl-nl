---
title: Maken van een hoofdknooppunt HPC Pack in een virtuele machine in Azure | Microsoft Docs
description: Informatie over het gebruik van de Azure portal en het implementatiemodel van Resource Manager voor het maken van een Microsoft HPC Pack 2012 R2-hoofdknooppunt in een Azure VM.
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: e6a13eaf-9124-47b4-8d75-2bc4672b8f21
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: b2bb9caf82a580dc5f67ea0b0b1c2e9a46363e9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Het hoofdknooppunt van een HPC Pack-cluster maken in een Azure VM met een Marketplace-installatiekopie
Gebruik een [installatiekopie van de virtuele machine Microsoft HPC Pack 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) vanuit Azure Marketplace en de Azure-portal te maken van het hoofdknooppunt van een HPC-cluster. Deze HPC Pack VM-installatiekopie is gebaseerd op Windows Server 2012 R2 Datacenter met HPC Pack 2012 R2 Update 3 vooraf zijn geïnstalleerd. Gebruik deze hoofdknooppunt voor een bewijs van de implementatie van het concept van HPC Pack in Azure. U kunt vervolgens rekenknooppunten toevoegen aan het cluster HPC-workloads uitvoeren.

> [!TIP]
> U wordt aangeraden dat u een automatische methode voor het implementeren van een volledige HPC Pack 2012 R2-cluster in Azure met het hoofdknooppunt en rekenknooppunten. Opties zijn onder andere de [HPC Pack IaaS-implementatiescript](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) en Resource Manager-sjablonen, zoals de [cluster HPC Pack voor Windows werkbelasting](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/). Resource Manager-sjablonen zijn ook beschikbaar voor [Microsoft HPC Pack 2016 clusters](https://github.com/MsHpcPack/HPCPack2016/tree/master/newcluster-templates). 
> 
> 

## <a name="planning-considerations"></a>Aandachtspunten voor de planning
In de volgende afbeelding ziet u het HPC Pack hoofdknooppunt in Active Directory-domein in een Azure-netwerk implementeren.

![Hoofdknooppunt HPC Pack][headnode]

* **Active Directory-domein**: de HPC Pack 2012 R2 hoofdknooppunt moet worden toegevoegd aan Active Directory-domein in Azure, voordat u begint met de HPC-services op de virtuele machine. Zoals in dit artikel voor een bewijs van concept implementatie, kunt u de virtuele machine die u maakt voordat u begint de HPC-services als een domeincontroller voor het hoofdknooppunt promoveren. Er is een andere optie voor het implementeren van een afzonderlijke domeincontroller en het forest in Azure waarnaar u deelnemen aan het hoofdknooppunt VM.

* **Implementatiemodel**: voor de meeste nieuwe implementaties, wordt aangeraden dat u het Resource Manager-implementatiemodel. In dit artikel wordt ervan uitgegaan dat u dit implementatiemodel.

* **Virtuele Azure-netwerk**: wanneer u het implementatiemodel van Resource Manager gebruikt voor het implementeren van het hoofdknooppunt, u opgeven of maken van een virtuele Azure-netwerk. U gebruikt het virtuele netwerk als u wilt deelnemen aan het hoofdknooppunt aan een bestaand Active Directory-domein. U moet ook later naar een rekenknooppunt virtuele machines toevoegen aan het cluster.

## <a name="steps-to-create-the-head-node"></a>Stappen voor het maken van het hoofdknooppunt
Hieronder volgen de stappen op hoog niveau de Azure portal gebruiken om te maken van een Azure-VM voor het hoofdknooppunt HPC Pack met de Resource Manager-implementatiemodel. 

1. Als u wilt maken van een nieuw Active Directory-forest in Azure met afzonderlijke domeincontroller virtuele machines, één optie is met een [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc). Voor een eenvoudige bewijs van concept implementatie is het goed is deze stap overslaan en het hoofdknooppunt VM zelf configureren als een domeincontroller. Deze optie wordt verderop in dit artikel beschreven.
2. Op de [HPC Pack 2012 R2 op de pagina Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) in Azure Marketplace, klikt u op **virtuele Machine maken**. 
3. In de portal op de **HPC Pack 2012 R2 op Windows Server 2012 R2** pagina de **Resource Manager** implementatiemodel en klik vervolgens op **maken**.
   
    ![HPC Pack installatiekopie][marketplace]
4. Configureer de instellingen en de virtuele machine maken via de portal. Als u geen ervaring met Azure, volgt u de zelfstudie [virtuele Windows-machine maken in de Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Voor een bewijs van concept implementatie kunt meestal de standaardwaarde accepteren of aanbevolen instellingen.
   
   > [!NOTE]
   > Als u deelnemen aan het hoofdknooppunt aan een bestaand Active Directory-domein in Azure wilt, zorg er dan voor dat u het virtuele netwerk voor dat domein opgeven bij het maken van de virtuele machine.
   > 
   > 
5. Nadat u de virtuele machine maken en de virtuele machine wordt uitgevoerd, [verbinding maken met de virtuele machine](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) door extern bureaublad. 
6. De virtuele machine toevoegen aan een Active Directory-domein-forest door een van de volgende opties te kiezen:
   
   * Als u de virtuele machine hebt gemaakt in een Azure-netwerk met een bestaand domeinforest, lid worden van de virtuele machine naar het forest met standaardprogramma's voor Serverbeheer of Windows PowerShell. Start vervolgens opnieuw.
   * Als u de virtuele machine in een nieuw virtueel netwerk (zonder een bestaand domein-forest) gemaakt, kunt u vervolgens de virtuele machine als een domeincontroller promoveren. Gebruik standaard stappen om te installeren en configureren van de Active Directory Domain Services-rol op het hoofdknooppunt. Zie voor gedetailleerde stappen [een nieuw Windows Server 2012 Active Directory-Forest installeren](https://technet.microsoft.com/library/jj574166.aspx).
7. Nadat de virtuele machine wordt uitgevoerd en wordt gekoppeld aan een Active Directory-forest, start u de services HPC Pack als volgt:
   
    a. Verbinding maken met het hoofdknooppunt VM die gebruikmaakt van een domeinaccount dat lid is van de lokale groep Administrators. Bijvoorbeeld, het instellen van tijdens het maken van het hoofdknooppunt VM administrator-account gebruiken.
   
    b. Voor een hoofdknooppunt standaardconfiguratie start Windows PowerShell als beheerder en typ het volgende:
   
    ```PowerShell
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```
   
    Het kan enkele minuten duren voordat de HPC Pack services te starten.
   
    Voor extra hoofdknooppunt configuratieopties, typt u `get-help HPCHNPrepare.ps1`.

## <a name="next-steps"></a>Volgende stappen
* Nu kunt u werken met het hoofdknooppunt van het cluster HPC Pack. Bijvoorbeeld, start HPC Cluster Manager, en voltooi de [implementatie takenlijst](https://technet.microsoft.com/library/jj884141.aspx).
* Als u verhogen van het cluster compute-capaciteit wilt op aanvraag, voegt u [Azure burst knooppunten](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) in een cloudservice. 
* Probeer een werkbelasting test uit te voeren op het cluster. Zie voor een voorbeeld: de HPC Pack [instructiehandleiding](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/hpcpack-cluster-headnode/marketplace.png
