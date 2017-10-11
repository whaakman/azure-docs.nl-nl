---
title: Burst knooppunten toevoegen aan een cluster HPC Pack | Microsoft Docs
description: Meer informatie over het uitbreiden van een HPC Pack-cluster in Azure op aanvraag door toe te voegen worker rolinstanties uitgevoerd in een cloudservice
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 24b79a8a-24ad-4002-ae76-75abc9b28c83
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 9336743b92130e37b1df2992aab806696f8276aa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>On-demand 'burst' knooppunten toevoegen aan een HPC Pack cluster in Azure
Als u een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure, kunt u een manier om snel de capaciteit van het cluster omhoog of omlaag schalen, zonder het onderhouden van een reeks vooraf geconfigureerde rekenknooppunt virtuele machines. In dit artikel leest u hoe on demand 'burst' knooppunten (worker rolinstanties uitgevoerd in een cloudservice) toevoegen als de rekenresources voor een hoofdknooppunt in Azure. 

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

![Burst knooppunten][burst]

De stappen in dit artikel kunt u snel Azure knooppunten toevoegen aan een cloud-gebaseerde HPC Pack hoofdknooppunt VM voor een test of bewijs van concept implementatie. De stappen op hoog niveau zijn hetzelfde als de stappen voor een 'burst naar Azure' om toe te voegen cloud rekencapaciteit is voor een on-premises HPC Pack-cluster. Zie voor een zelfstudie [instellen van een hybride rekencluster met Microsoft HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Zie voor gedetailleerde richtlijnen en overwegingen voor productie-implementaties, [Burst naar Azure met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

## <a name="prerequisites"></a>Vereisten
* **HPC Pack hoofdknooppunt geïmplementeerd in een Azure VM** -kunt u een zelfstandige hoofdknooppunt VM of een die deel uitmaakt van een groter cluster. Zie het maken van een zelfstandige hoofdknooppunt [implementeren van een HPC Pack hoofdknooppunt in een Azure VM](../../virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Zie voor geautomatiseerde HPC Pack cluster implementatieopties [opties voor het maken en beheren van een Windows HPC-cluster in Azure met Microsoft HPC Pack](../../virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Als u de [HPC Pack IaaS-implementatiescript](hpcpack-cluster-powershell-script.md) voor het maken van het cluster in Azure, kunt u Azure burst knooppunten opnemen in uw geautomatiseerde implementatie. Zie de voorbeelden in dit artikel.
  > 
  > 
* **Azure-abonnement** - Azure knooppunten toevoegen kunt u hetzelfde abonnement gebruikt voor het implementeren van het hoofdknooppunt VM, of een ander abonnement (of abonnementen).
* **Quotum voor kernen** -moet u mogelijk verhogen van het quotum van kernen, vooral als u kiest voor het implementeren van verschillende Azure knooppunten met multicore grootten. Een quotum te verhogen [opent u een ondersteuningsaanvraag online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zonder kosten.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Stap 1: Een cloudservice en een opslagaccount voor de Azure knooppunten maken
Gebruik de klassieke Azure-portal of vergelijkbare's voor het configureren van de volgende bronnen die nodig zijn voor het implementeren van uw Azure knooppunten:

* Een nieuwe Azure-cloud-service
* Een nieuwe Azure storage-account

> [!NOTE]
> Niet opnieuw gebruiken van een bestaande cloudservice in uw abonnement. 
> 
> 

**Overwegingen**

* Configureer een afzonderlijke cloudservice voor elk Azure-knooppuntsjabloon die u wilt maken. U kunt echter hetzelfde opslagaccount gebruiken voor meerdere knooppunt sjablonen.
* Het is raadzaam om de cloudservice en het opslagaccount voor de implementatie te in dezelfde Azure-regio vinden.

## <a name="step-2-configure-an-azure-management-certificate"></a>Stap 2: Een Azure-beheercertificaat configureren
Als u Azure knooppunten toevoegen als rekenresources, moet u een beheercertificaat op het hoofdknooppunt en het uploaden van een overeenkomt met de Azure-abonnement gebruikt voor de implementatie van het certificaat.

Voor dit scenario kunt u de **standaard HPC Azure-Beheercertificaat** die HPC Pack automatisch geïnstalleerd en geconfigureerd op het hoofdknooppunt. Dit certificaat is handig voor het testen van de toepassing en bewijs van concept implementaties. Upload het bestand C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer vanaf het hoofdknooppunt van VM aan het abonnement voor het gebruik van dit certificaat. Voor het uploaden van het certificaat in de [klassieke Azure-portal](https://manage.windowsazure.com), klikt u op **instellingen** > **Beheercertificaten**.

Zie voor aanvullende opties voor het configureren van het beheercertificaat [scenario's voor het configureren van het Azure-Beheercertificaat voor Azure Burst implementaties](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Stap 3: Azure knooppunten aan het cluster implementeren
De stappen toe te voegen en start Azure knooppunten in dit scenario zijn meestal hetzelfde zijn als de stappen met het hoofdknooppunt van een lokale. Zie voor meer informatie de volgende secties in [stappen voor het implementeren van Azure knooppunten met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Een op Azure-knooppuntsjabloon maken
* Azure knooppunten toevoegen aan de Windows HPC-cluster
* Start (ingericht) de Azure-knooppunten

Na het toevoegen en starten van de knooppunten kunnen zijn ze gereed voor u gebruiken voor het cluster taken uitvoeren.

Als u problemen ondervindt bij het implementeren van Azure knooppunten, Zie [oplossen implementaties van Azure knooppunten met Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Volgende stappen
* Zie de overwegingen in voor het gebruik van een van rekenintensieve exemplaargrootte voor de knooppunten burst, [hoge prestaties compute-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Als u wilt automatisch vergroten of verkleinen van de Azure computerbronnen volgens de clusterwerkbelasting worden opgeslagen, Zie [automatisch vergroten of verkleinen van Azure-rekenresources in een cluster HPC Pack](hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/hpcpack-cluster-node-burst/burst.png
