---
title: "Vergelijking van aangepaste installatiekopieën en formules in DevTest Labs | Microsoft Docs"
description: "Meer informatie over de verschillen tussen aangepaste installatiekopieën en formules als VM baseert zodat u kunt beslissen welke beste past bij uw omgeving."
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: v-craic
ms.openlocfilehash: 78c0255f142bd3d4b2311ac953541153b72ac12d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Vergelijking van aangepaste installatiekopieën en formules in DevTest Labs
Beide [aangepaste installatiekopieën](devtest-lab-create-template.md) en [formules](devtest-lab-manage-formulas.md) kan worden gebruikt als basis voor [gemaakt van de nieuwe virtuele machines](devtest-lab-add-vm.md). Het belangrijkste verschil tussen aangepaste installatiekopieën en formules is echter dat een aangepaste installatiekopie is gewoon een op basis van een VHD, terwijl een formule een installatiekopie op basis van een VHD is *naast* vooraf geconfigureerde instellingen - zoals VM-grootte, virtueel netwerk subnet en artefacten. Deze vooraf geconfigureerde instellingen zijn ingesteld met de standaardwaarden die kunnen worden genegeerd op het moment van de virtuele machine maken. In dit artikel worden enkele van de (professionals) voordelen en nadelen (nadelen) voor het gebruik van aangepaste installatiekopieën ten opzichte van formules.

## <a name="custom-image-pros-and-cons"></a>Aangepaste installatiekopie voor- en nadelen
Aangepaste installatiekopieën bieden een statische, niet-wijzigbaar manier om virtuele machines maken vanuit een gewenste-omgeving. 

**Professionals**

* VM-inrichting van een aangepaste installatiekopie is snelle als er niets wordt gewijzigd nadat de virtuele machine uit de afbeelding wordt ingezet. Met andere woorden, zijn er geen instellingen tijdens de aangepaste installatiekopie alleen maar een afbeelding zonder de instellingen is toegepast. 
* Virtuele machines die zijn gemaakt op basis van één aangepaste installatiekopie zijn identiek.

**Nadelen**

* Als u bijwerken van een bepaald aspect van de aangepaste installatiekopie wilt, de installatiekopie moet opnieuw worden gemaakt.  

## <a name="formula-pros-and-cons"></a>Formule voor- en nadelen
Formules bieden een dynamische manier om te maken van virtuele machines van de gewenste instellingen.

**Professionals**

* Wijzigingen in de omgeving kunnen worden vastgelegd op elk gewenst moment via artefacten. Bijvoorbeeld, als u een virtuele machine is geïnstalleerd met de meest recente bits vanuit de pipeline release of de meest recente code van uw opslagplaats aanmelden, kunt u gewoon opgeven een artefact die zich aanmeldt de meest recente code in de formule samen met een base doel of de meest recente bits geïmplementeerd afbeelding. Wanneer deze formule wordt gebruikt voor het maken van virtuele machines, zijn de meest recente bits/code geïmplementeerd/aangemeld met de virtuele machine. 
* Formules kunnen standaardinstellingen die aangepaste installatiekopieën niet - zoals instellingen voor virtuele netwerken en VM-grootten biedt definiëren. 
* De instellingen die zijn opgeslagen in een formule als standaardwaarden worden weergegeven, maar kunnen worden gewijzigd wanneer de virtuele machine wordt gemaakt. 

**Nadelen**

* Een VM maken van een formule kan langer duren dan het maken van een virtuele machine van een aangepaste installatiekopie.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten
* [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Volgende stappen
- [DevTest Labs Veelgestelde vragen](devtest-lab-faq.md)