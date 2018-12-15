---
title: Data Science Virtual Machine-pools - Azure | Microsoft Docs
description: Pools van Data Science-VM's implementeren als een gedeelde bron voor een team
keywords: deep learning, AI, hulpprogramma's voor data science, virtuele machine voor datatechnologie, georuimtelijke analyses, team data science process
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: gokuma
ms.openlocfilehash: 648b0818f07aca8763fa4af01380076ae307b0a2
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408949"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Maken van een gedeelde groep van virtuele Machines voor Datatechnologie

In dit artikel wordt beschreven hoe u een gedeelde groep van virtuele Machines voor Datatechnologie (Dsvm) voor een team te gebruiken kunt maken. De voordelen van het gebruik van een gedeelde groep zijn beter Resourcegebruik, vereenvoudiging van delen en samenwerking en efficiënter beheer van resources van de DSVM. 

U kunt vele methoden en -technologieën gebruiken om een pool van Dsvm te maken. In dit artikel is gericht op groepen voor interactieve virtuele machines. Een alternatieve beheerde rekeninfrastructuur is Azure Machine Learning-Computing. Zie [compute-doelen ingesteld](../service/how-to-set-up-training-targets.md#amlcompute) voor meer informatie.

## <a name="interactive-vm-pool"></a>Interactieve VM-pool

Een pool van interactieve virtuele machines die worden gedeeld door het hele AI/data science-team kan gebruikers zich aanmelden bij een beschikbaar exemplaar van de DSVM in plaats van een toegewezen exemplaar voor elke set van gebruikers. Deze instelling kunt u betere beschikbaarheid en effectiever gebruik van resources. 

De technologie die u gebruikt om een interactieve virtuele machine-pool te maken is [Azure virtuele-machineschaalsets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). U kunt schaalsets gebruiken voor een groep identieke, taakverdeling en virtuele machines automatisch schalen maken en beheren. 

De gebruiker zich aanmeldt de hoofdpool IP- of DNS-adres. De schaal automatisch ingesteld routes de sessie op een beschikbare DSVM in de schaalset. Aangezien een dergelijke omgeving, ongeacht de virtuele machine de gebruikers willen ze bent aangemeld bij, alle exemplaren van de virtuele machine in de schaalset een gedeeld netwerk-station, zoals een Azure-bestandsshare of een NFS-share koppelt. Gedeelde werkruimte van de gebruiker wordt normaal gesproken opgeslagen op de gedeelde bestandsopslag die gekoppeld op elk van de exemplaren. 

U vindt een voorbeeld Azure Resource Manager-sjabloon maakt u een schaalset met Ubuntu-DSVM-exemplaren op [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Een voorbeeld van de [parameterbestand](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) voor de Azure Resource Manager-sjabloon is op dezelfde locatie. 

U kunt de schaalset op basis van de Azure Resource Manager-sjabloon door waarden voor de parameter-bestand op te geven in de Azure CLI maken. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
De voorgaande opdrachten wordt ervan uitgegaan dat u hebt:
* Een kopie van het parameterbestand met de waarden die zijn opgegeven voor uw exemplaar van de schaalset.
* Het aantal VM-exemplaren.
* Verwijzingen naar de Azure-bestanden delen.
* Referenties voor het opslagaccount dat wordt gekoppeld op elke virtuele machine. 

Het parameterbestand wordt lokaal verwezen in de opdrachten. U kunt ook parameters in line- of vragen om ze in uw script doorgeven.  

De voorgaande sjabloon kunt u de SSH- en de poort JupyterHub van de front-schaalset ingesteld op de back-end-pool van Ubuntu-Dsvm. Als een gebruiker aanmelden u alleen bij de virtuele machine op SSH of JupyterHub op normale wijze. Omdat de VM-exemplaren kunnen worden opgeschaald of omlaag dynamisch, elke status moet worden opgeslagen in de gekoppelde Azure-bestanden dan delen. U kunt dezelfde benadering gebruiken om een pool van Windows-Dsvm te maken. 

De [-script waarmee de Azure Files-share koppelt](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) is ook beschikbaar in de Azure-DataScienceVM-opslagplaats in GitHub. Het script koppelt u de Azure-bestandsshare op het opgegeven koppelpunt in het parameterbestand. Het script maakt ook zachte koppelingen naar het gekoppelde station in de basismap van de eerste gebruiker. Een directory gebruikersspecifieke notitieblok in de Azure Files-share is voorlopig gekoppeld aan de `$HOME/notebooks/remote` map zodat gebruikers kunnen toegang krijgen tot, uitvoeren en opslaan van hun Jupyter-notebooks. U kunt dezelfde conventie gebruiken wanneer u extra gebruikers maakt op de virtuele machine van elke gebruiker Jupyter werkruimte verwijzen naar de Azure-bestandsshare. 

Virtuele-machineschaalsets ondersteuning voor automatisch schalen. U kunt regels instellen op wanneer u extra exemplaren en wanneer u voor het schalen van exemplaren. U kunt bijvoorbeeld omlaag schalen naar nul exemplaren op te slaan op de hardwarekosten gebruik cloud wanneer de virtuele machines worden helemaal niet gebruikt. De documentatie van pagina's van virtuele-machineschaalsets bieden gedetailleerde stappen voor het [automatisch schalen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Volgende stappen

* [Een gemeenschappelijke identiteit instellen](dsvm-common-identity.md)
* [Veilig opslaan van referenties voor toegang tot cloud-bronnen](dsvm-secure-access-keys.md)















