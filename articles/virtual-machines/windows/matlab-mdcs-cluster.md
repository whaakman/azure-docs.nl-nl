---
title: MATLAB-clusters op virtuele machines | Microsoft Docs
description: Microsoft Azure virtual machines gebruiken om te maken van clusters MATLAB gedistribueerde Computing-Server als u wilt uw rekenintensieve parallelle MATLAB-workloads uitvoeren
services: virtual-machines-windows
documentationcenter: ''
author: mscurrell
manager: jeconnoc
editor: ''
ms.assetid: e9980ce9-124a-41f1-b9ec-f444c8ea5c72
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Windows
ms.workload: infrastructure-services
ms.date: 05/09/2016
ms.author: markscu
ms.openlocfilehash: fd5ae375dff80c8b1179d2fd73566d07c5861e4a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58000339"
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Server voor MATLAB gedistribueerde Computing-clusters maken in Azure-VM 's
Microsoft Azure virtual machines gebruiken om een of meer MATLAB gedistribueerde Computing Server-clusters voor het uitvoeren van uw rekenintensieve parallelle MATLAB-workloads te maken. Uw Server voor MATLAB gedistribueerde Computing-software installeren op een virtuele machine wilt gebruiken als basisinstallatiekopie en een Azure-quickstart-sjabloon of Azure PowerShell-script (beschikbaar op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) te implementeren en beheren van het cluster. Verbinding maken met het cluster aan uw workloads worden uitgevoerd na de implementatie.

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Over MATLAB en MATLAB gedistribueerde Computing-Server
De [MATLAB](https://www.mathworks.com/products/matlab/) platform is geoptimaliseerd voor het oplossen van problemen met technische en wetenschappelijke. MATLAB-gebruikers met grootschalige simulaties en gegevensverwerkingstaken kunnen MathWorks voor parallelle berekeningen producten om hun rekenintensieve workloads sneller door te profiteren van de rekenclusters en raster services te gebruiken. [Parallelle Computing werkset](https://www.mathworks.com/products/parallel-computing/) MATLAB gebruikers kunnen toepassingen parallel en te profiteren van meerdere kernen processors, GPU's, en rekenclusters. [Server voor MATLAB gedistribueerde Computing](https://www.mathworks.com/products/distriben/) kunnen MATLAB gebruikers gebruikmaken van veel computers in een rekencluster.

Met behulp van virtuele machines van Azure, kunt u MATLAB gedistribueerde Computing-serverclusters die hebben allemaal dezelfde methoden beschikbaar om in te dienen parallel werk als clusters van on-premises, zoals interactieve taken, batch-taken, onafhankelijke taken en communicatie taken. Met behulp van Azure in combinatie met het platform MATLAB heeft veel voordelen ten opzichte van inrichting en met behulp van traditionele on-premises hardware: een bereik van de VM-grootten, het maken van clusters op aanvraag, zodat u alleen voor de compute-resources die u gebruikt betaalt, en de de mogelijkheid om modellen in de test.  

## <a name="prerequisites"></a>Vereisten
* **Clientcomputer** -, moet u een Windows-clientcomputer om te communiceren met Azure en de MATLAB gedistribueerde Computing Server-cluster na de implementatie.
* **Azure PowerShell** -Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) ze deze willen installeren op uw clientcomputer.
* **Azure-abonnement** -als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten. Voor grotere clusters, kunt u een abonnement op gebruiksbasis of andere Aankoopopties.
* **Vcpu-quotum** -moet u mogelijk het vCPU-quotum voor het implementeren van een groot cluster of meer dan één Server voor MATLAB gedistribueerde Computing cluster verhogen. Om een quotum te verhogen [opent u een ondersteuningsaanvraag online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) gratis.
* **MATLAB, parallelle Computing-werkset en MATLAB gedistribueerde Computing Server-licenties** -scripts wordt ervan uitgegaan dat de [MathWorks gehost License Manager](https://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) wordt gebruikt voor alle licenties.  
* **MATLAB gedistribueerde Computing-serversoftware** -wordt geïnstalleerd op een virtuele machine die wordt gebruikt als de basis VM-installatiekopie voor de cluster-VM's.

## <a name="high-level-steps"></a>Hoog niveau stappen
Voor het gebruik van virtuele machines van Azure voor uw Server voor MATLAB gedistribueerde Computing-clusters, zijn de volgende geavanceerde stappen zijn vereist. Gedetailleerde instructies vindt u in de documentatie bij de quickstart-sjabloon en scripts op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Basis VM-installatiekopie maken**  

   * Download en installeer MATLAB gedistribueerde Computing Server-software op deze virtuele machine.

     > [!NOTE]
     > Dit proces kan enkele uren duren, maar u hoeft alleen te doen wanneer u voor elke versie van MATLAB gebruiken.   
     >
     >
2. **Een of meer clusters maken**  

   * De opgegeven PowerShell-script gebruiken of de quickstart-sjabloon gebruiken voor het maken van een cluster van de basisinstallatiekopie voor de virtuele machine.   
   * Beheren van clusters met behulp van het opgegeven PowerShell-script waarmee u de lijst, onderbreken, hervatten en clusters verwijderen.

## <a name="cluster-configurations"></a>Configuraties van clusters
Op dit moment kunnen de cluster-script voor het maken en de sjabloon u een topologie met één Server voor MATLAB gedistribueerde Computing maken. Als u wilt, een of meer extra clusters maken met een cluster met een verschillend aantal worker-VM's, met verschillende VM-grootten, enzovoort.

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB-client en -cluster in Azure
De MATLAB client-knooppunt, MATLAB Taakplanner-knooppunt en Server voor MATLAB gedistribueerde Computing "werknemer" knooppunten zijn alle geconfigureerd als virtuele Azure-machines in een virtueel netwerk, zoals wordt weergegeven in de volgende afbeelding.


* Voor het gebruik van het cluster, verbinding maken met extern bureaublad naar de clientknooppunt. De client-knooppunt uitvoert de MATLAB-client.
* De client-knooppunt heeft een bestandsshare die kan worden geopend door alle werknemers.
* MathWorks gehost License Manager wordt gebruikt voor de licentie-controles voor alle MATLAB-software.
* Standaard één Server voor MATLAB gedistribueerde Computing werknemer per vCPU wordt gemaakt op de virtuele machines van de werknemer, maar u kunt een willekeurig getal opgeven.

## <a name="use-an-azure-based-cluster"></a>Een Cluster op basis van Azure gebruiken
Als bij andere soorten MATLAB gedistribueerde Computing Server-clusters, moet u de profiel-Clustermanager in de MATLAB-client (op de client virtuele machine) gebruiken om een Taakplanner MATLAB-cluster-profiel te maken.

![Profiel Clusterbeheer](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor gedetailleerde instructies voor het implementeren en beheren van de Server voor MATLAB gedistribueerde Computing-clusters in Azure, de [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) -opslagplaats met de sjablonen en scripts.
* Ga naar de [MathWorks site](https://www.mathworks.com/) voor gedetailleerde informatie voor MATLAB en MATLAB gedistribueerde Computing-Server.
