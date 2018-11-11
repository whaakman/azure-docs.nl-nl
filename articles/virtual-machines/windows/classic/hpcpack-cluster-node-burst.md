---
title: Burstknooppunten toevoegen aan een HPC Pack-cluster | Microsoft Docs
description: Meer informatie over het uitbreiden van een HPC Pack-cluster in Azure op aanvraag door het toevoegen van instanties van de werkrol die worden uitgevoerd in een cloudservice
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,hpc-pack
ms.assetid: 24b79a8a-24ad-4002-ae76-75abc9b28c83
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 7d42c026975a18c7574e4bc64ec28ab3ed0082bc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248449"
---
# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>On-demand-burst-knooppunten toevoegen aan een HPC Pack-cluster in Azure
Als u een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure, kunt u een manier om snel de capaciteit van het cluster omhoog of omlaag schalen, zonder het onderhouden van een reeks vooraf geconfigureerde virtuele machines van het rekenknooppunt. In dit artikel wordt beschreven hoe u on-demand-burst-knooppunten (werkrolinstanties die worden uitgevoerd in een cloudservice) toevoegen als rekenbronnen aan een hoofdknooppunt in Azure. 

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

![Burstknooppunten][burst]

De stappen in dit artikel kunt u snel Azure knooppunten toevoegen aan een cloud-gebaseerde HPC Pack hoofdknooppunt VM voor een test of de implementatie van proof of concept. De hoofdstappen zijn dezelfde als de stappen voor het 'burst naar Azure' om toe te voegen cloud rekencapaciteit is voor een on-premises HPC Pack-cluster. Zie voor een zelfstudie [instellen van een hybride rekencluster met Microsoft HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Zie voor gedetailleerde richtlijnen en overwegingen voor productie-implementaties, [uitbreiden naar Azure met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

## <a name="prerequisites"></a>Vereisten
* **Hoofdknooppunt van HPC Pack geïmplementeerd in een Azure VM** -kunt u een zelfstandige hoofdknooppunt VM of een die deel uitmaakt van een groter cluster. Zie voor het maken van een zelfstandige hoofdknooppunt [implementeren van een HPC Pack-hoofdknooppunt in een Azure VM](../../virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Zie voor geautomatiseerde HPC Pack-cluster-implementatieopties, [opties voor het maken en beheren van een Windows HPC-cluster in Azure met Microsoft HPC Pack](../../virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Als u de [HPC Pack IaaS-implementatiescript](hpcpack-cluster-powershell-script.md) voor het maken van het cluster in Azure, kunt u Azure burstknooppunten opnemen in uw geautomatiseerde implementatie. Zie de voorbeelden in dit artikel.
  > 
  > 
* **Azure-abonnement** : als u wilt toevoegen van Azure-knooppunten, kunt u hetzelfde abonnement gebruikt voor het implementeren van het hoofdknooppunt VM, of een ander abonnement (of abonnementen).
* **Quotum voor kerngeheugens** -moet u mogelijk vergroot het quotum voor kernen, met name als u wilt implementeren meerdere Azure-knooppunten met multicore-groottes. Om een quotum te verhogen [opent u een ondersteuningsaanvraag online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) gratis.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Stap 1: Een service in de cloud en een opslagaccount voor de Azure-knooppunten maken
De Azure portal of vergelijkbare's gebruiken om de volgende resources die nodig zijn voor het implementeren van uw Azure-knooppunten te configureren:

* Een nieuwe Azure-cloud-service (klassiek)
* Een nieuwe Azure-opslagaccount (klassiek)

> [!NOTE]
> Niet opnieuw gebruiken van een bestaande cloudservice in uw abonnement. 
> 
> 

**Overwegingen**

* Configureer een afzonderlijke cloudservice voor elke Azure-knooppunt-sjabloon die u van plan bent om te maken. U kunt echter hetzelfde opslagaccount gebruiken voor meerdere knooppunt sjablonen.
* Het is raadzaam dat u de cloudservice en het opslagaccount voor de implementatie in dezelfde Azure-regio vinden.

## <a name="step-2-configure-an-azure-management-certificate"></a>Stap 2: Een Azure-beheercertificaat configureren
Als u Azure knooppunten als rekenbronnen toevoegen, moet u een certificaat voor beheer op het hoofdknooppunt en het uploaden een overeenkomende-certificaat voor het Azure-abonnement gebruikt voor de implementatie.

Voor dit scenario kunt u de **standaard HPC Azure-Beheercertificaat** die HPC Pack installeert en configureert automatisch op het hoofdknooppunt. Dit certificaat is handig voor het testen van de toepassing en proof of concept-implementaties. Upload de C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer bestand aan het abonnement van het hoofdknooppunt VM voor het gebruik van dit certificaat. Het uploaden van het certificaat in de [Azure-portal](https://portal.azure.com):

1. Klik op **abonnementen** > *your_subscription_name*.

2. Klik op **beheercertificaten** > **uploaden**.

Zie voor meer opties voor het configureren van het beheercertificaat [scenario's voor het configureren van het Azure-Beheercertificaat voor Azure Burst-implementaties](https://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Stap 3: Azure-knooppunten aan het cluster implementeren
De stappen voor het toevoegen en Azure-knooppunten te starten in dit scenario zijn in het algemeen hetzelfde als de stappen met een on-premises hoofdknooppunt. Zie voor meer informatie de volgende secties in [stappen voor het Azure-knooppunten implementeren met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Een Azure-knooppunt-sjabloon maken
* Azure-knooppunten toevoegen aan het Windows HPC-cluster
* Start (ingericht) de Azure-knooppunten

Nadat u hebt toegevoegd en de knooppunten start, zijn ze klaar voor gebruik cluster taken uit te voeren.

Als u problemen ondervindt bij het implementeren van Azure-knooppunten, Zie [oplossen implementaties van Azure-knooppunten met Microsoft HPC Pack](https://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Volgende stappen
* Zie voor het gebruik van de grootte van een rekenintensieve exemplaar voor de burstknooppunten, de overwegingen in [High performance computing-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Als u wilt automatisch vergroten of verkleinen van de Azure computerbronnen volgens de clusterworkload, Zie [automatisch vergroten of verkleinen van Azure compute-resources in een HPC Pack-cluster](hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/hpcpack-cluster-node-burst/burst.png
