---
title: Vervangen door een hardware-onderdeel op een knooppunt met Azure Stack scale unit | Microsoft Docs
description: Informatie over het vervangen van een hardware-onderdeel op een geïntegreerde Azure Stack-systeem.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.lastreviewed: 12/06/2018
ms.openlocfilehash: 1671b755b6dc5277141810d177a55bc7d6a51211
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57771664"
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Vervangen door een hardware-onderdeel op een Azure Stack scale unit-knooppunt

*Van toepassing op: Azure Stack-geïntegreerde systemen*

In dit artikel worden de algemene ter vervanging van hardwareonderdelen die niet hot-swappable zijn beschreven. Werkelijke vervanging stappen verschillen, is afhankelijk van de leverancier van de oorspronkelijke leveranciers (OEM)-hardware. Zie de FRU-documentatie (Field Replaceable Unit) van uw leverancier voor gedetailleerde stappen die specifiek zijn voor uw geïntegreerde Azure Stack-systeem.

Niet hot-swappable onderdelen zijn onder andere het volgende:

- CPU *
- Geheugen *
- Moederbord/baseboard management controller (BMC) / video kaart
- Schijf-controller/host busadapter (HBA) / backplane
- Netwerkadapter (NIC)
- Besturingssysteem schijf *
- Schijven (stations die bieden geen ondersteuning voor hot swappable, bijvoorbeeld PCI-e-invoegtoepassing kaarten) *

* Deze onderdelen hot swappable kunnen ondersteunen, maar kunnen variëren op basis van de leverancier van toepassing. Zie de OEM-leverancier FRU-documentatie voor gedetailleerde stappen.

Het volgende stroomdiagram ziet u het algemene FRU-proces op een niet hot-swappable hardwareonderdeel vervangt.

![Stroomdiagram dat toont onderdeel vervanging stroom](media/azure-stack-replace-component/replacecomponentflow.PNG)

* Deze actie kan niet meer nodig op basis van de fysieke voorwaarde van de hardware.

** Of de leverancier van de OEM-hardware voert de component vervanging en updates van die de firmware kan variëren op basis van uw contract support.

## <a name="review-alert-information"></a>Bekijk informatie over waarschuwingen

De Azure Stack-status en bewakingssysteem Volg de status van netwerkadapters en gegevensstations beheerd door opslagruimten Direct. Andere hardware-onderdelen worden niet bijgehouden. Waarschuwingen worden gegenereerd voor alle andere hardwareonderdelen in de leverancierspecifieke hardware bewakingsoplossing, die wordt uitgevoerd op de host van de levenscyclus van hardware.  

## <a name="component-replacement-process"></a>Proces voor het onderdeel vervangen

De volgende stappen bevatten een overzicht op hoog niveau van het proces voor het onderdeel vervangen. Volg deze stappen niet zonder te verwijzen naar de OEM geleverde FRU-documentatie.

1. Gebruik de actie Afsluiten op het knooppunt van de eenheid schalen zonder problemen wordt afgesloten. Deze actie kan niet meer nodig op basis van de fysieke voorwaarde van de hardware.

2. In een onwaarschijnlijke geval de afsluitactie mislukt, gebruikt u de [leegmaken](azure-stack-node-actions.md#drain) actie het scale unit-knooppunt in de onderhoudsmodus plaatsen. Deze actie kan niet meer nodig op basis van de fysieke voorwaarde van de hardware.

   > [!NOTE]  
   > In elk geval slechts één knooppunt kan worden uitgeschakeld en uitgeschakeld op hetzelfde moment zonder te verbreken van de S2D (opslagruimten Direct).

3. Als het scale unit-knooppunt in de onderhoudsmodus bevindt is, gebruikt de [uitschakelen](azure-stack-node-actions.md#scale-unit-node-actions) actie. Deze actie kan niet meer nodig op basis van de fysieke voorwaarde van de hardware.

   > [!NOTE]  
   > In het onwaarschijnlijke geval dat het uitschakelen van de actie niet werkt, gebruikt u de webinterface van baseboard management controller (BMC).

4. Vervang de beschadigde hardware-onderdeel. Of de leverancier van de OEM-hardware wordt uitgevoerd de vervanging onderdeel kan variëren op basis van uw ondersteuningscontract.  
5. De firmware bijwerken. Ga als volgt uw leverancierspecifieke firmware-updateproces met behulp van de host van de levenscyclus van hardware om te controleren of dat het vervangen van hardwareonderdeel heeft de goedgekeurde firmware toegepast. Of de leverancier van de OEM-hardware wordt uitgevoerd in deze stap kan variëren op basis van uw ondersteuningscontract.  
6. Gebruik de [herstellen](azure-stack-node-actions.md#scale-unit-node-actions) actie het knooppunt van de eenheid schaal terug te brengen in de schaaleenheid.
7. Gebruik het eindpunt van de bevoegdheden op [Controleer de status van de virtuele schijf herstellen](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Met de nieuwe schijven, een volledig herstel van de opslagtaak kan meerdere uren, afhankelijk van het laden van het systeem duren en ruimte verbruikt.
8. Nadat de herstelactie is voltooid, valideert u dat alle actieve waarschuwingen automatisch is gesloten.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het vervangen van een hot swappable fysieke schijf [een schijf vervangt](azure-stack-replace-disk.md).
- Zie voor meer informatie over het vervangen van een fysiek knooppunt [vervangen door een knooppunt van de eenheid schaal](azure-stack-replace-node.md).
