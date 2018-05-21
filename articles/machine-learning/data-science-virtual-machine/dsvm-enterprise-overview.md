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
ms.openlocfilehash: 6a755ef4d933046377a6a25be76655b44f4bf508
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Gegevens wetenschappelijke virtuele Machine op basis van Team Analytics en AI-omgeving 
De [gegevens wetenschappelijke virtuele Machine](overview.md) (DSVM) biedt een uitgebreide omgeving op de Azure-Cloud met vooraf gemaakte software voor analyses AI en gegevens. Traditioneel de DSVM is gebruikt als afzonderlijke analytics bureaublad en afzonderlijke gegevenswetenschappers productiviteit met deze gedeelde begrip van hun omgeving vooraf samengestelde analytics krijgen. Als grote analytics teams hun gegevenswetenschappers en ontwikkelaars AI hun omgeving analytics plannen, een van de terugkerende thema's is voor een gedeelde analytics ontwikkeling en experimenteren infrastructuur die wordt beheerd in overeenstemming met de IT-onderneming beleid die ook kunt u samenwerking en consistentie in de hele gegevenswetenschap / analytics teams. Een gedeelde infrastructuur kan ook IT beter gebruikmaken van de omgeving analytics. De gegevens op basis van een team wetenschap / analytics infrastructuur wordt ook wel aangeduid door sommige organisaties als 'Analytics Sandbox' waarmee de gegevenswetenschappers tot snel inzicht in gegevens, experimenten uitvoeren, hypothese valideren, voorspellende modellen maken op een veilige manier zonder enige impact op de productieomgeving en toegang hebben tot verschillende gegevensassets. 

Aangezien de DSVM op het niveau van de Azure-infrastructuur werkt, kunnen de IT-beheerders de DSVM werken in overeenstemming met de IT-beleid van de onderneming en biedt flexibiliteit bij de uitvoering van diverse delen architecturen met toegang tot gemakkelijk configureren zakelijke gegevensassets op een gecontroleerde manier. 

Deze sectie wordt beschreven bepaalde patronen en richtlijnen die kunnen worden gebruikt voor het implementeren van de DSVM als een wetenschappelijke gegevens op basis van een team infrastructuur.  De bouwstenen voor deze patronen rechtstreeks wordt gebruikgemaakt van Azure IaaS (infrastructuur als een service) en als zodanig van toepassing op alle virtuele Azure-machines. De specifieke focus van deze reeks artikelen is over het toepassen van deze mogelijkheden standaard Azure-infrastructuur voor VM wetenschappelijke gegevens. 

Enkele van de belangrijkste bouwstenen van een bedrijfsomgeving voor team analytics zijn:

* [Toepassingen automatisch geschaald van gegevens wetenschappelijke Virtual Machines](dsvm-pools.md)
* [Gemeenschappelijke identiteit en toegang tot de werkruimte uit een van de DSVMs in de groep](dsvm-common-identity.md)
* [Veilige toegang tot gegevensbronnen](dsvm-secure-access-keys.md)


In deze serie artikelen, richtlijnen en aanwijzers vindt u in elk van deze aspecten. Natuurlijk, er zijn enkele aanvullende overwegingen en behoeften bij het implementeren van DSVM in grote ondernemingen configuraties waarop geen nog rechtstreeks in deze serie artikelen. Hier zijn enkele van de andere overwegingen en koppelingen naar algemene Azure documentatie die gemakkelijk kan worden gebruikt tijdens de uitvoering ervan op de exemplaren DSVM in uw onderneming. 

* [Netwerkbeveiliging](https://docs.microsoft.com/azure/security/azure-network-security)
* [Bewaking](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) en [Management](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Logboekregistratie en controle](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [Beleidsinstelling en afdwingen](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)
* [Anti-malware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Versleuteling](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Detectie van gegevens en toezicht](https://docs.microsoft.com/azure/data-catalog/)

De [Azure architectuur center](https://docs.microsoft.com/en-us/azure/architecture/) is ook een zeer waardevolle resource die voorziet in gedetailleerde end-to-end-architectuur en patronen voor het maken en beheren van uw cloudinfrastructuur op basis analytics. 
