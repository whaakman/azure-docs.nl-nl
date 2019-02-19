---
title: Inleiding tot het team Data Science Virtual Machine gebaseerde omgevingen - Azure | Microsoft Docs
description: Patronen voor het implementeren van de Data Science VM in een bedrijfsomgeving team.
keywords: deep learning, AI, hulpprogramma's voor data science, virtuele machine voor datatechnologie, georuimtelijke analyses, team data science process
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 2e17ab5cfe51f3772148cc730c982671d602a79a
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56340475"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Op basis van een virtuele Machine voor Datatechnologie team gegevensanalyse en AI-omgeving 
De [Data Science Virtual Machine](overview.md) (DSVM) biedt een uitgebreide omgeving in de Azure-platform met vooraf gedefinieerde software voor kunstmatige intelligentie (AI) en gegevensanalyse. 

De DSVM is traditioneel gebruikt als een afzonderlijke analysebureaublad. Afzonderlijke data-analisten productiviteit met deze gedeelde begrip van hun analyseomgeving vooraf gemaakte. Als grote analytics teams hun data scientists en AI-ontwikkelaars hun omgevingen analytics plannen, is een van de terugkerende thema's een gedeelde analyse-infrastructuren voor ontwikkeling en experimenteren. Deze infrastructuur wordt beheerd in overeenstemming met enterprise IT beleidsregels die ook samenwerking en consistentie tussen de teams van data science en analyse mogelijk te maken. 

Een gedeelde infrastructuur gebeurtenishub IT beter gebruikmaken van de omgeving voor analyse. Sommige organisaties aanroepen de op basis van team data science en analyse-infrastructuur een "analytics sandbox." Hiermee kunt gegevenswetenschappers voor toegang tot verschillende gegevensassets om snel inzicht in gegevens, uitvoeren van experimenten, hypothesen valideren en voorspellende modellen te bouwen zonder die betrekking hebben op de productie-omgeving. 

Omdat de DSVM op het niveau van de Azure-infrastructuur werkt, kunnen IT-beheerders gemakkelijk de DSVM functioneren in overeenstemming met de IT-beleid van de onderneming configureren. De DSVM biedt volledige flexibiliteit bij de uitvoering van diverse delen architecturen met toegang tot zakelijke gegevens op een gecontroleerde manier. 

Deze sectie wordt besproken sommige patronen en richtlijnen die u gebruiken kunt voor het implementeren van de DSVM als een op basis van team data science-infrastructuur. De bouwstenen voor deze patronen zijn afkomstig van Azure-infrastructuur als een service (IaaS), zodat ze van toepassing op alle Azure-VM's. De focus van deze reeks artikelen is over het toepassen van deze mogelijkheden standaard Azure-infrastructuur naar de Data Science VM. 

Enkele van de belangrijkste bouwstenen van een bedrijfsomgeving team-analytics zijn:

* [Autoscaled pool van virtuele Machines voor Datatechnologie](dsvm-pools.md)
* [Algemene identiteit en toegang tot een werkruimte uit een van de Dsvm in de groep](dsvm-common-identity.md)
* [Beveiligde toegang tot gegevensbronnen](dsvm-secure-access-keys.md)


Deze reeks artikelen bevat richtlijnen en tips voor elk van de bovenstaande items. Ze omvatten niet de overwegingen en behoeften bij het implementeren van DSVM in grote ondernemingen configuraties. Hier ziet u andere Azure-documentatie die u gebruiken kunt tijdens het implementeren van DSVM-exemplaren in uw onderneming: 

* [Netwerkbeveiliging](https://docs.microsoft.com/azure/security/azure-network-security)
* [Bewaking](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) en [management](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Logboekregistratie en bewaking](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [Beleid instellen en afdwingen](../../governance/policy/overview.md)
* [Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Versleuteling](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Gegevensdetectie en -beheer](https://docs.microsoft.com/azure/data-catalog/)

De [Azure Architecture Center](https://docs.microsoft.com/azure/architecture/) biedt een gedetailleerde end-to-end-architectuur en patronen voor het ontwikkelen en beheren van uw cloud-gebaseerde analyse-infrastructuren. 
