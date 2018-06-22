---
title: Inleiding tot team gegevens wetenschappelijke virtuele machines gebaseerde omgevingen - Azure | Microsoft Docs
description: Patronen voor het implementeren van de VM van de wetenschappelijke gegevens in een bedrijfsomgeving team.
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
ms.openlocfilehash: 8486b0be1fb5e1385da3c7ad55f6410a1059df93
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309245"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Op basis van wetenschappelijke virtuele Machine team gegevensanalyse en AI-omgeving 
De [gegevens wetenschappelijke virtuele Machine](overview.md) (DSVM) biedt een uitgebreide omgeving in de Azure-platform met vooraf gedefinieerde software voor kunstmatige intelligence (AI) en gegevensanalyse. 

De DSVM is traditioneel gebruikt als een afzonderlijke analytics bureaublad. Afzonderlijke gegevenswetenschappers krijgen productiviteit met deze gedeelde begrip van hun omgeving vooraf gedefinieerde analytics. Zoals grote analytics teams hun gegevenswetenschappers en ontwikkelaars AI hun omgeving analytics plannen, is een van de terugkerende thema's een infrastructuur voor gedeelde analytics voor ontwikkeling en experimenteren. Deze infrastructuur wordt beheerd in overeenstemming met zakelijke IT-beleidsregels die ook samenwerking en consistentie teams de wetenschap/analytics gegevens vergemakkelijken. 

Een gedeelde infrastructuur kan ook IT beter gebruikmaken van de omgeving analytics. Sommige organisaties aanroepen de infrastructuur op basis van een team wetenschap/analytics een 'analytics sandbox." Hiermee kunt gegevenswetenschappers voor toegang tot verschillende gegevensassets om snel inzicht in gegevens, experimenten uitvoeren hypothesen valideren en bouwen van voorspellende modellen zonder de productie-omgeving. 

Omdat de DSVM op het niveau van de Azure-infrastructuur werkt, kunnen IT-beheerders gemakkelijk de DSVM werken in overeenstemming met de IT-beleid van de onderneming configureren. De DSVM biedt volledige flexibiliteit bij de uitvoering van diverse delen architecturen met toegang tot zakelijke gegevens activa op een gecontroleerde manier. 

Deze sectie wordt beschreven bepaalde patronen en richtlijnen die u gebruiken kunt voor het implementeren van de DSVM als een wetenschappelijke gegevens op basis van een team infrastructuur. De bouwstenen voor deze patronen afkomstig zijn van Azure-infrastructuur als een service (IaaS), zodat ze voor alle virtuele Azure-machines gelden. De focus van deze reeks artikelen is over het toepassen van deze mogelijkheden standaard Azure-infrastructuur voor VM wetenschappelijke gegevens. 

Enkele van de belangrijkste bouwstenen van een bedrijfsomgeving voor team analytics zijn:

* [Geschaald pool van gegevens wetenschappelijke Virtual Machines](dsvm-pools.md)
* [Gemeenschappelijke identiteit en toegang tot een werkruimte van een van de DSVMs in de groep](dsvm-common-identity.md)
* [Veilige toegang tot gegevensbronnen](dsvm-secure-access-keys.md)


Deze reeks artikelen biedt richtlijnen en verwijzingen voor elk van de bovenstaande items. Dekt niet de overwegingen en behoeften op het implementeren van DSVM in grote ondernemingen configuraties. Dit is een andere Azure-documentatie die u gebruiken kunt bij het DSVM exemplaren implementeren in uw onderneming: 

* [Netwerkbeveiliging](https://docs.microsoft.com/azure/security/azure-network-security)
* [Bewaking](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) en [management](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Logboekregistratie en bewaking](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Toegangsbeheer op basis van rollen](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [Beleidsinstelling en afdwingen](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)
* [Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Versleuteling](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Detectie van gegevens en toezicht](https://docs.microsoft.com/azure/data-catalog/)

De [Azure architectuur Center](https://docs.microsoft.com/en-us/azure/architecture/) biedt een gedetailleerde end-to-end-architectuur en patronen voor het maken en beheren van uw cloud-gebaseerde analytics-infrastructuur. 
