---
title: Vergelijking van aangepaste installatiekopieën en formules in DevTest Labs | Microsoft Docs
description: Meer informatie over de verschillen tussen aangepaste installatiekopieën en formules as VM bases zodat u kunt beslissen welke beste tegemoetkomt aan uw omgeving.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: ae7556eda817b9eb7be84f9d4a23ea91d3d5440d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64680304"
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Vergelijking van aangepaste installatiekopieën en formules in DevTest Labs
Beide [aangepaste installatiekopieën](devtest-lab-create-template.md) en [formules](devtest-lab-manage-formulas.md) kan worden gebruikt als basis voor [nieuwe VM's gemaakt](devtest-lab-add-vm.md). Het belangrijkste verschil tussen aangepaste installatiekopieën en formules is echter dat een aangepaste installatiekopie gewoon een installatiekopie op basis van een VHD is, terwijl een formule een installatiekopie op basis van een VHD wordt *naast* vooraf geconfigureerde instellingen - zoals VM-grootte, virtueel netwerk subnet, en artefacten. Deze vooraf gedefinieerde instellingen worden ingesteld met de standaardwaarden die kunnen worden overschreven op het moment van de VM wordt gemaakt. In dit artikel worden enkele van de voordelen (professionals) en nadelen (nadelen) uitgelegd voor het gebruik van aangepaste installatiekopieën met behulp van formules.

## <a name="custom-image-pros-and-cons"></a>Aangepaste installatiekopie voor- en nadelen
Aangepaste installatiekopieën bieden een statisch, onveranderbare manier om virtuele machines maken van een gewenste omgeving. 

**Pros**

* VM-inrichting van een aangepaste installatiekopie is snel als er niets verandert nadat de virtuele machine van de installatiekopie wordt genomen. Met andere woorden, zijn er geen instellingen om toe te passen als de aangepaste installatiekopie alleen een installatiekopie zonder de instellingen is. 
* Virtuele machines die zijn gemaakt op basis van een aangepaste installatiekopie van één zijn identiek.

**Nadelen**

* Als u nodig hebt om bij te werken van een bepaald aspect van de aangepaste installatiekopie, kan de installatiekopie moet worden gemaakt.  

## <a name="formula-pros-and-cons"></a>Formule voor- en nadelen
Formules bieden een dynamische manier om virtuele machines maken van de gewenste instellingen.

**Pros**

* Wijzigingen in de omgeving kunnen worden vastgelegd in een handomdraai via artefacten. Bijvoorbeeld, als u een virtuele machine met de meest recente materialen vanuit uw release-pijplijn geïnstalleerd of de meest recente code van uw opslagplaats aanmelden, kunt u gewoon opgeven een artefact die implementeert u de meest recente materialen of waarmee de meest recente code in de formule samen met een doel-basis afbeelding. Wanneer deze formule wordt gebruikt voor het maken van virtuele machines, zijn de meest recente bits/code geïmplementeerd/aangemeld bij de virtuele machine. 
* Formules kunnen standaardinstellingen die aangepaste installatiekopieën niet - zoals VM-grootten en virtuele-netwerkinstellingen biedt definiëren. 
* De instellingen die zijn opgeslagen in een formule als standaardwaarden worden weergegeven, maar kunnen worden gewijzigd wanneer de virtuele machine wordt gemaakt. 

**Nadelen**

* Het maken van een virtuele machine van een formule, duurt langer dan het maken van een virtuele machine van een aangepaste installatiekopie.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Gerelateerde blogberichten
* [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Volgende stappen
- [Veelgestelde vragen DevTest Labs](devtest-lab-faq.md)