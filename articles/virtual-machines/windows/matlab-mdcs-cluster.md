---
title: MATLAB clusters op virtuele machines | Microsoft Docs
description: Microsoft Azure virtuele machines gebruiken voor het maken van clusters MATLAB gedistribueerde Computing Server als u wilt uw rekenintensieve parallelle MATLAB workloads uitvoeren
services: virtual-machines-windows
documentationcenter: 
author: mscurrell
manager: timlt
editor: 
ms.assetid: e9980ce9-124a-41f1-b9ec-f444c8ea5c72
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Windows
ms.workload: infrastructure-services
ms.date: 05/09/2016
ms.author: markscu
ms.openlocfilehash: 177f8a61487130e718e3e6cfb779b17a3ed8ed69
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Server voor MATLAB gedistribueerde Computing clusters maken op Azure Virtual machines
Gebruik Microsoft Azure virtuele machines te maken van een of meer MATLAB gedistribueerde Computing serverclusters als u wilt uw rekenintensieve parallelle MATLAB workloads uitvoeren. Uw Server voor MATLAB gedistribueerde Computing-software installeren op een virtuele machine wilt gebruiken als een basisinstallatiekopie en een snelstartsjabloon met de Azure- of Azure PowerShell-script (beschikbaar op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) te implementeren en beheren van het cluster. Verbinding met het cluster aan uw werkbelastingen uitvoeren na de implementatie.

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Over MATLAB en MATLAB gedistribueerde Computing Server
De [MATLAB](http://www.mathworks.com/products/matlab/) platform is geoptimaliseerd voor het oplossen van problemen met de technische en wetenschappelijke. MATLAB gebruikers met grootschalige simulaties en gegevensverwerkingstaken kunnen MathWorks parallel computing producten gebruiken om te versnellen hun rekenintensieve workloads door gebruik te maken van de rekenclusters en raster services. [Parallelle berekeningen werkset](http://www.mathworks.com/products/parallel-computing/) kunnen gebruikers MATLAB parallelize toepassingen en te profiteren van multicore-processors, GPU's, en rekenclusters. [Server voor MATLAB gedistribueerde Computing](http://www.mathworks.com/products/distriben/) kunnen MATLAB gebruikers gebruikmaken van tal van computers in een rekencluster.

Met behulp van virtuele machines in Azure, kunt u MATLAB gedistribueerde Computing serverclusters waarvoor allemaal dezelfde mechanismen beschikbaar parallelle taken verzenden als lokale clusters, zoals interactieve taken, batchtaken, onafhankelijke taken en communicatie taken. Met behulp van Azure in combinatie met het platform MATLAB heeft veel voordelen ten opzichte van inrichting en met behulp van traditionele on-premises hardware: een bereik van de VM-groottes, het maken van clusters op aanvraag zodat u alleen voor de rekenresources die u gebruikt betaalt, en de de mogelijkheid voor het testen van modellen op schaal.  

## <a name="prerequisites"></a>Vereisten
* **Clientcomputer** -moet u een Windows-clientcomputer om te communiceren met Azure en het cluster MATLAB gedistribueerde Computing Server na de implementatie.
* **Azure PowerShell** -Zie [installeren en configureren van Azure PowerShell](/powershell/azure/overview) om deze te installeren op de clientcomputer.
* **Azure-abonnement** -als u geen een abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten. Voor grotere clusters kunt u een abonnement op gebruiksbasis of andere Aankoopopties.
* **vcpu's quotum** -u wellicht de vCPU quotum voor het implementeren van een grote cluster of meer dan één Server voor MATLAB gedistribueerde Computing cluster te verhogen. Een quotum te verhogen [opent u een ondersteuningsaanvraag online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zonder kosten.
* **MATLAB, parallelle berekeningen werkset en MATLAB gedistribueerde Computing Server licenties** -de scripts wordt ervan uitgegaan dat de [MathWorks gehost Licentiebeheer](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) wordt gebruikt voor alle licenties.  
* **Server voor MATLAB gedistribueerde Computing software** -wordt geïnstalleerd op een virtuele machine die wordt gebruikt als de basisinstallatiekopie VM voor het cluster virtuele machines.

## <a name="high-level-steps"></a>Hoog niveau stappen
Voor het gebruik van virtuele machines in Azure voor uw MATLAB gedistribueerde Computing serverclusters, zijn de volgende geavanceerde stappen zijn vereist. Gedetailleerde instructies vindt u in de documentatie bij de Quick Start-sjabloon en scripts op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Basis VM-installatiekopie maken**  

   * Download en installeer de Server voor MATLAB gedistribueerde Computing software op deze virtuele machine.

     > [!NOTE]
     > Dit proces kan enkele uren duren, maar u hoeft alleen te doen nadat u voor elke versie van MATLAB gebruiken.   
     >
     >
2. **Een of meer clusters maken**  

   * Het opgegeven PowerShell-script gebruiken of de Quick Start-sjabloon gebruiken voor een cluster maken van de basisinstallatiekopie van VM.   
   * Beheren van clusters met de opgegeven PowerShell-script waarmee u de lijst, onderbreken, hervatten en clusters te verwijderen.

## <a name="cluster-configurations"></a>Configuraties van clusters
Op dit moment wordt kunnen het script voor het cluster maken en de sjabloon u een topologie met één Server voor MATLAB gedistribueerde Computing maken. Als u wilt maken van een of meer extra clusters, met elk cluster met een verschillend aantal worker virtuele machines, met behulp van andere VM-grootten, enzovoort.

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB client en de cluster in Azure
De MATLAB client-knooppunt, MATLAB Taakplanner-knooppunt en Server voor MATLAB gedistribueerde Computing ' ' werkrolknooppunten zijn alle geconfigureerd als virtuele Azure-machines in een virtueel netwerk, zoals wordt weergegeven in de volgende afbeelding.


* Verbinding met extern bureaublad met het clientknooppunt voor het gebruik van het cluster. Het knooppunt voor uitvoert de client MATLAB.
* De clientknooppunt heeft een bestandsshare die toegankelijk zijn voor alle werknemers.
* MathWorks gehost Licentiebeheer wordt gebruikt voor de licentie-controles voor alle MATLAB software.
* Standaard één Server voor MATLAB gedistribueerde Computing werknemer per vCPU wordt gemaakt op de virtuele machines van de werknemer, maar u kunt een willekeurig getal opgeven.

## <a name="use-an-azure-based-cluster"></a>Gebruik een Cluster op basis van Azure
Als bij andere soorten MATLAB gedistribueerde Computing serverclusters, moet u Profielbeheer Cluster in de client MATLAB (op de client VM) gebruiken om een cluster MATLAB Taakplanner profiel te maken.

![Profiel Clusterbeheer](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor gedetailleerde instructies voor het implementeren en beheren van MATLAB gedistribueerde Computing serverclusters in Azure de [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) -opslagplaats met de sjablonen en scripts.
* Ga naar de [MathWorks site](http://www.mathworks.com/) voor gedetailleerde documentatie voor MATLAB en MATLAB gedistribueerde Computing-Server.
