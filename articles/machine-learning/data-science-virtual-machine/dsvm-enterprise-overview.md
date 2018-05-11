---
title: Inleiding op de gegevens wetenschappelijke virtuele Machine op basis van Team omgevingen - Azure | Microsoft Docs
description: Patronen voor het implementeren van gegevens wetenschappelijke virtuele machine als teams bedrijfsomgeving.
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
ms.openlocfilehash: 911d6484421cc9fddad0530bf8d9ab4f01d48bf8
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Gegevens wetenschappelijke virtuele Machine op basis van Team Analytics en AI-omgeving 
De [gegevens wetenschappelijke virtuele Machine](overview.md) (DSVM) biedt een uitgebreide omgeving op de Azure-Cloud met vooraf gemaakte software voor analyses AI en gegevens. Traditioneel de DSVM is gebruikt als afzonderlijke analytics bureaublad en afzonderlijke gegevenswetenschappers productiviteit met deze gedeelde begrip van hun omgeving vooraf samengestelde analytics krijgen. Als grote analytics teams hun gegevenswetenschappers en ontwikkelaars AI hun omgeving analytics plannen, een van de terugkerende thema's is voor een gedeelde analytics ontwikkeling en experimenteren infrastructuur die wordt beheerd in overeenstemming met de IT-onderneming beleid die ook kunt u samenwerking en consistentie in de hele gegevenswetenschap / analytics teams. Een gedeelde infrastructuur kan ook IT beter gebruikmaken van de omgeving analytics. De gegevens op basis van een team wetenschap / analytics infrastructuur wordt ook wel aangeduid door sommige organisaties als 'Analytics Sandbox' waarmee de gegevenswetenschappers tot snel inzicht in gegevens, experimenten uitvoeren, hypothese valideren, voorspellende modellen maken op een veilige manier zonder enige impact op de productieomgeving en toegang hebben tot verschillende gegevensassets. 

Aangezien de DSVM op het niveau van de Azure-infrastructuur werkt, kunnen de IT-beheerders de DSVM werken in overeenstemming met de IT-beleid van de onderneming en biedt flexibiliteit bij de uitvoering van diverse delen architecturen met toegang tot gemakkelijk configureren zakelijke gegevensassets op een gecontroleerde manier. 

Deze sectie wordt beschreven bepaalde patronen en richtlijnen die kunnen worden gebruikt voor het implementeren van de DSVM als een wetenschappelijke gegevens op basis van een team infrastructuur.  De bouwstenen voor deze patronen rechtstreeks wordt gebruikgemaakt van Azure IaaS (infrastructuur als een service) en als zodanig van toepassing op alle virtuele Azure-machines. De specifieke focus van deze reeks artikelen is over het toepassen van deze mogelijkheden standaard Azure-infrastructuur voor VM wetenschappelijke gegevens. 

Enkele van de belangrijkste bouwstenen van een bedrijfsomgeving voor team analytics zijn:

* [Toepassingen automatisch geschaald van gegevens wetenschappelijke Virtual Machines](dsvm-pools.md)
* [Gemeenschappelijke identiteit en toegang tot de werkruimte uit een van de DSVMs in de groep](dsvm-common-identity.md)
* [Veilige toegang tot gegevensbronnen](dsvm-secure-access-keys.md)
* Bestuur en detectie van bedrijfs- en open gegevenssets

In deze serie artikelen, richtlijnen en aanwijzers vindt u in elk van deze aspecten. De [Azure architectuur center](https://docs.microsoft.com/en-us/azure/architecture/) voorziet in gedetailleerde architectuur van de end-to-end voor uw infrastructuur analytics.  
