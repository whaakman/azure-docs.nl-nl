---
title: Gegevens van toepassingen wetenschappelijke Virtual Machine - Azure | Microsoft Docs
description: Pools van gegevens wetenschappelijke virtuele machines implementeren als een gedeelde bron voor een team
keywords: deep leren, AI en hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, georuimtelijke analytics, team gegevens wetenschappelijke processen
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 0740ff7542d066442146b8e80e188ad5ba49a2b5
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309395"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Een gedeelde groep van gegevens wetenschappelijke virtuele Machines maken

Dit artikel wordt beschreven hoe u een gedeelde groep van gegevens wetenschappelijke Virtual Machines (DSVMs) voor een team gebruik kunt maken. De voordelen van het gebruik van een gedeelde groep zijn beter brongebruik vergemakkelijking van delen en samenwerking en effectiever beheer van DSVM resources. 

U kunt veel methoden en technologieën gebruiken om een pool van DSVMs te maken. Dit artikel is gericht op de groepen voor batchverwerking en interactieve virtuele machines.

## <a name="batch-processing-pool"></a>Batchverwerking van toepassingen
Als u een pool van DSVMs hoofdzakelijk uit te voeren taken in een batch offline instellen wilt, kunt u de [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) of [Azure Batch](https://docs.microsoft.com/azure/batch/) service. Dit artikel is gericht op Azure Batch AI.

Ubuntu-versie van de DSVM wordt ondersteund als een van de afbeeldingen in Azure Batch AI. In de Azure CLI of de SDK voor Python, waar u de Azure Batch AI-cluster maakt, kunt u de `image` parameter en wordt ingesteld op `UbuntuDSVM`. U kunt kiezen welk type verwerking knooppunten: exemplaren op basis van GPU versus instanties van alleen-CPU, aantal processors en geheugen van een [ruime keuze van VM-instanties](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) beschikbaar zijn op Azure. 

Wanneer u de installatiekopie van het Ubuntu DSVM in Batch AI met knooppunten op basis van GPU gebruikt, worden de benodigde GPU-stuurprogramma's en frameworks learning diepe vooraf is geïnstalleerd. De voorinstallatie bespaart u veel tijd bij het voorbereiden van de batch-knooppunten. Zelfs als u op een Ubuntu DSVM interactief ontwikkelt, zult u merken dat de knooppunten Batch AI precies de dezelfde installatie en configuratie van de omgeving zijn. 

Wanneer u een Batch AI-cluster maakt, maakt u gewoonlijk ook een bestandsshare die is gekoppeld door alle knooppunten. De bestandsshare wordt gebruikt voor invoer en uitvoer van gegevens, evenals de code batch taak/scripts opslaan. 

Nadat u een Batch AI-cluster maakt, kunt u dezelfde CLI of Python SDK voor het verzenden van taken worden uitgevoerd. U betaalt voor alleen de tijd die wordt gebruikt voor het uitvoeren van de batchtaken. 

Zie voor meer informatie:
* Stapsgewijze handleiding voor het gebruik van [Azure CLI](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) Batch AI beheren
* Stapsgewijze handleiding voor het gebruik van [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) Batch AI beheren
* [Batch-AI recepten](https://github.com/Azure/BatchAI) die laten zien hoe u verschillende AI en deep frameworks met Batch AI leren gebruiken

## <a name="interactive-vm-pool"></a>Interactieve VM-groep

Een pool van interactieve virtuele machines die worden gedeeld door het hele AI/gegevens wetenschappelijke team kunnen gebruikers zich aanmelden met een beschikbaar exemplaar van de DSVM in plaats van een speciaal exemplaar voor elke set van gebruikers. Deze instelling kunt u betere beschikbaarheid en effectiever gebruik van bronnen. 

De technologie die u gebruikt om een interactieve VM-pool te maken is [virtuele Azure-machine-schaalsets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). U kunt schaalsets maken en beheren van een groep identieke, taakverdeling en virtuele machines automatisch schalen. 

De gebruiker zich aanmeldt van de belangrijkste groep IP- of DNS-adres. De schaal automatisch ingesteld routes de sessie een DSVM beschikbaar in de schaalset. Omdat een soortgelijke omgeving ongeacht de virtuele machine wilt dat gebruikers ze bent aangemeld bij, alle exemplaren van de virtuele machine in de schaalset een station gedeeld netwerk, zoals een Azure-bestanden-share of een NFS-share koppelt. De gebruiker gedeelde werkruimte wordt normaal gesproken op de gedeelde bestandsopslag dat gekoppeld is op elk van de exemplaren. 

U vindt een voorbeeldsjabloon voor Azure Resource Manager waarmee een schaal en Ubuntu DSVM exemplaren instelt voor [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Een voorbeeld van de [parameterbestand](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) voor de Azure Resource Manager-sjabloon is op dezelfde locatie bevinden. 

U kunt de schaal van de Azure Resource Manager-sjabloon is ingesteld door het opgeven van waarden voor de parameter-bestand in Azure CLI kunt maken. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
De voorgaande opdrachten wordt ervan uitgegaan dat u hebt:
* Een kopie van de parameter-bestand met de opgegeven waarden voor uw exemplaar van de schaal is ingesteld.
* Het aantal VM-exemplaren.
* Verwijzingen naar de Azure-bestanden delen.
* Referenties voor het opslagaccount dat zal worden gekoppeld op elke virtuele machine. 

De parameter-bestand wordt lokaal verwezen in de opdrachten. U kunt ook parameters in line- of vragen om ze in uw script doorgeven.  

De voorgaande sjabloon kunt u de SSH- en de JupyterHub-poort van de front-schaal ingesteld op de back-end-pool van Ubuntu DSVMs. Als een gebruiker aanmelden u alleen bij de virtuele machine op SSH of JupyterHub op normale wijze. Omdat de VM-exemplaren kunnen worden geschaald omhoog of omlaag dynamisch, geen status moet worden opgeslagen in de gekoppelde Azure-bestanden kunt delen. Dezelfde manier kunt u een pool maken met Windows DSVMs. 

De [script dat de bestanden van de Azure-share koppelt](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) is ook beschikbaar in de Azure-DataScienceVM-opslagplaats in GitHub. Het script koppelt de share Azure-bestanden op het opgegeven koppelpunt in het parameterbestand. Het script maakt ook zachte koppelingen naar het gekoppelde station in de basismap van de eerste gebruiker. Een directory gebruikersspecifieke notebook binnen de share Azure Files is voorlopig gekoppeld aan de `$HOME/notebooks/remote` map zodat gebruikers kunnen toegang krijgen tot, uitvoeren en hun Jupyter-notebooks opslaan. U kunt de dezelfde overeenkomst gebruiken wanneer u extra gebruikers maakt op de virtuele machine van de gebruiker Jupyter werkruimte verwijzen naar de share Azure-bestanden. 

Virtuele-machineschaalsets ondersteuning voor automatisch schalen. U kunt regels instellen op wanneer extra exemplaren maken en wanneer schalen exemplaren. U kunt bijvoorbeeld omlaag schalen op nul-exemplaren op te slaan op de hardwarekosten gebruik cloud wanneer de virtuele machines niet helemaal worden gebruikt. De documentatie van pagina's van virtuele-machineschaalsets bieden gedetailleerde stappen voor het [automatisch schalen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Volgende stappen

* [Instellen van een algemene identiteit](dsvm-common-identity.md)
* [Veilig opslaan van referenties voor toegang tot cloud-bronnen](dsvm-secure-access-keys.md)















