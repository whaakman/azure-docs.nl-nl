---
title: Gegevens wetenschappelijke virtuele Machine Pools - Azure | Microsoft Docs
description: Pools van gegevens wetenschappelijke VM implementeren als een gedeelde bron voor het team
keywords: deep leren, AI en hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, georuimtelijke analytics, team gegevens wetenschappelijke processen
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 625154378fecbd4d45904fa5669adb866fc9487c
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="creating-a-shared-pool-of-data-science-virtual-machines"></a>Maken van een gedeelde groep van gegevens wetenschappelijke Virtual Machines

Dit artikel wordt beschreven hoe een gedeelde groep van virtuele Machines (gegevens wetenschappelijke DSVM) kan worden gemaakt voor gebruik door het team. Het voordeel van het gebruik van een gedeelde groep is beter brongebruik te vergemakkelijken delen en samenwerking en waardoor IT effectiever DSVM resources beheren. 

Er zijn veel manieren en andere technologieën die kunnen worden gebruikt om een pool van DSVM te maken.  Hier volgen de belangrijkste scenario's:

* Groep voor batchverwerking
* Groep voor interactieve virtuele machines

## <a name="batch-processing-pool"></a>Batchverwerking van toepassingen
Als u wilt dat voor het instellen van een pool van DSVM hoofdzakelijk uit te voeren taken in een batch offline, dan kunt u [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) service of [Azure Batch](https://docs.microsoft.com/azure/batch/). 

### <a name="azure-batch-ai"></a>Azure Batch AI
Ubuntu-versie van de DSVM wordt ondersteund als een van de afbeeldingen in Azure Batch AI. In de Azure CLI of de Python SDK waar u de Azure Batch AI-cluster maakt, kunt u de ```image``` parameter en wordt ingesteld op ```UbuntuDSVM```. U kunt kiezen welk type verwerking knooppunten - exemplaren op basis van GPU tegenover CPU alleen instanties, het aantal CPU's, het geheugen van een [ruime keuze van VM-instanties](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) beschikbaar zijn op Azure. Wanneer u de installatiekopie van het Ubuntu DSVM in Batch AI met op basis van GPU knooppunten zijn alle benodigde GPU stuurprogramma's en frameworks grondige learning vooraf zijn veel tijd besparen bij het voorbereiden van de batch-knooppunten. Zelfs als u op een Ubuntu DSVM interactief ontwikkelt, ziet u dat de knooppunten Batch AI precies de dezelfde installatie en configuratie van de omgeving zijn. Tijdens het maken van een Batch AI-cluster maakt u gewoonlijk ook een bestandsshare die is gekoppeld door alle knooppunten en wordt gebruikt voor invoer en uitvoer van gegevens en opslaan van de taakcode batch / scripts. 

Zodra uw Batch-AI-cluster is gemaakt, kunt u dezelfde CLI of Python SDK voor het verzenden van taken worden uitgevoerd. U betaalt alleen voor de tijd die wordt gebruikt voor het uitvoeren van de batchtaken. 

#### <a name="more-information"></a>Meer informatie
* Stapsgewijze handleiding voor het gebruik van [Azure CLI](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) voor het beheren van Batch AI
* Stapsgewijze handleiding voor het gebruik van [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) voor het beheren van Batch AI
* [Batch-AI recepten](https://github.com/Azure/BatchAI) zijn beschikbaar op het gebruik van verschillende AI/deep learning frameworks met Batch AI te demonstreren.

## <a name="interactive-vm-pool"></a>Interactieve VM-groep

Een pool van interactieve DSVMs die worden gedeeld door de hele AI / gegevens wetenschappelijke team kan gebruikers zich aanmelden bij een beschikbaar exemplaar van de DSVM in plaats van een speciaal exemplaar voor elke set van gebruikers. Dit helpt met betere beschikbaarheid en effectiever gebruik van bronnen. 

De technologie gebruikt voor het maken van een interactieve toepassingen van de VM is de [Azure virtuele-Machineschaalsets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) (VMSS), waarmee u een groep identieke, taakverdeling en automatisch schalen virtuele machines maken en beheren. De gebruiker zich aanmeldt in de IP- of DNS-adres van de belangrijkste pool. De schaal automatisch ingesteld routes de sessie een DSVM beschikbaar in de Schaalset. Omdat de gebruiker wilt soortgelijke omgeving ongeacht de VM ze aan te melden bij, koppelt u alle exemplaren van de virtuele machine in de Schaalset een gedeeld netwerkstation zoals een Azure-bestanden of een NFS-share. De gebruiker gedeelde werkruimte wordt normaal gesproken op de gedeelde bestandsopslag dat gekoppeld is op elk van de exemplaren. 

Voorbeeld Azure Resource Manager-sjabloon die een VM-Schaalset met Ubuntu DSVMs exemplaren gemaakt vindt u op de [github](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Een voorbeeld van de Azure Resource Manager-sjabloon [parameterbestand](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) is ook beschikbaar in dezelfde locatie. 

U kunt de VM-schaalset instellen van de Azure Resource Manager-sjabloon door te geven geschikte waarden voor de parameter-bestand met Azure CLI kunt maken. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
De bovenstaande opdrachten wordt ervan uitgegaan dat u hebt een kopie van de parameterbestand met de opgegeven waarden voor uw exemplaar van het VM-schaalaanpassingsset, het aantal VM-instanties, verwijzingen naar uw Azure-bestanden samen met de referenties voor het opslagaccount dat zal worden gekoppeld op elke virtuele machine. De parameter-bestand wordt lokaal verwezen in de bovenstaande opdracht. U kunt ook doorgeven parameters inline of vragen om ze in uw script.  

De bovenstaande sjabloon Hiermee de SSH- en de poort Jupyterhub via de frontend VM-Schaalset ingesteld op de back-endpool van Ubuntu DSVMs.  Als een gebruiker aanmelden u alleen bij VM op SSH of JupyterHub op de normale wijze. Omdat de VM-exemplaren kunnen worden geschaald omhoog of omlaag dynamisch, geen status moet worden opgeslagen in de gekoppelde Azure-bestanden kunt delen. Dezelfde manier kan worden gebruikt om een pool van Windows DSVMs te maken. 

De [script dat de Azure-bestanden koppelt](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) is ook beschikbaar in de Azure DataScienceVM Github. Naast het koppelen van de Azure-bestanden op het opgegeven koppelpunt in het parameterbestand, maakt deze ook aanvullende zachte koppelingen naar het gekoppelde station in de basismap van de eerste gebruiker en een gebruikersspecifieke notebook-map in de gedeelde bestanden met Azure is voorlopig gekoppeld aan ```$HOME/notebooks/remote``` directory zodat gebruiker toegang tot, uitvoeren en hun Jupyter-notebooks opslaan.  De dezelfde overeenkomst kan worden gebruikt wanneer u extra gebruikers maakt op de virtuele machine om te verwijzen naar Jupyter-werkruimte van elke gebruiker met de gedeelde bestanden met Azure. 

Azure VM-Schaalset ingesteld ondersteuning voor automatisch schalen waarin u regels kunt instellen als extra exemplaren maken en onder wat omstandigheden in te perken exemplaren waaronder u deze naar beneden op nul exemplaren op te slaan op brengt cloud hardwarekosten voor gebruik als de virtuele machines helemaal niet worden gebruikt . De documentatie van pagina's van VM-schaalsets bieden gedetailleerde stappen voor het [automatische schaling](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Volgende stappen

* [Algemene identiteit instellen](dsvm-common-identity.md)
* [Veilig opslaan van referenties voor toegang tot cloud-bronnen](dsvm-secure-access-keys.md)















