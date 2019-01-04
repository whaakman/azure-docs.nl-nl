---
title: Vervangen van een knooppunt van de eenheid schaal op een systeem met geïntegreerde Azure Stack | Microsoft Docs
description: Informatie over het vervangen van een knooppunt van de eenheid fysieke schaal op een geïntegreerde Azure Stack-systeem.
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
ms.date: 12/06/2018
ms.author: mabrigg
ms.openlocfilehash: 9d53aa879c39eb68597a402133a7ff16737f4f65
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53716307"
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Vervangen van een knooppunt van de eenheid schaal op een geïntegreerde Azure Stack-systeem

*Van toepassing op: Azure Stack-geïntegreerde systemen*

In dit artikel beschrijft het algemene proces ter vervanging van een fysieke computer (ook wel een knooppunt van de eenheid schalen genoemd) op een geïntegreerde Azure Stack-systeem. Werkelijke scale unit knooppunt vervanging stappen verschillen, is afhankelijk van de leverancier van de oorspronkelijke leveranciers (OEM)-hardware. Zie van uw leverancier veld FRU (replaceable unit)-documentatie voor gedetailleerde stappen die specifiek voor uw systeem zijn.

Het volgende stroomdiagram ziet u het algemene proces FRU ter vervanging van een gehele scale unit-knooppunt.

![Stroomdiagram voor vervangen knooppunt proces](media/azure-stack-replace-node/replacenodeflow.png)

* Deze actie kan niet meer nodig op basis van de fysieke voorwaarde van de hardware.

> [!Note]  
> Als het afsluiten is mislukt, wordt het aanbevolen gebruik van het clusterbesturingssysteem bewerking gevolgd door de bewerking stoppen. Zie voor meer informatie beschikbaar knooppunt operations  

## <a name="review-alert-information"></a>Bekijk informatie over waarschuwingen

Als een eenheid schaal knooppunt niet actief is, ontvangt u de volgende kritieke waarschuwingen:

- Knooppunt niet is verbonden met de netwerkcontroller
- Knooppunt niet toegankelijk is voor plaatsing van virtuele machines
- Schaal eenheid knooppunt is offline

![Lijst met waarschuwingen voor de schaaleenheid omlaag](media/azure-stack-replace-node/nodedownalerts.png)

Als u opent de **Scale unit knooppunt offline is** waarschuwing, de beschrijving van de waarschuwing bevat de scale unit-knooppunt dat is niet toegankelijk. U kunt ook extra waarschuwingen ontvangen in de oplossing voor OEM-specifieke controle die wordt uitgevoerd op de host van de levenscyclus van hardware.

![Details van knooppunt offline waarschuwing](media/azure-stack-replace-node/nodeoffline.png)

## <a name="scale-unit-node-replacement-process"></a>Proces voor Scale unit knooppunt het vervangen

De volgende stappen worden gegeven als een overzicht op hoog niveau van het proces voor scale unit knooppunt het vervangen. Raadpleeg uw OEM-hardwareleverancier FRU-documentatie voor gedetailleerde stappen die specifiek voor uw systeem zijn. Volg deze stappen niet zonder te verwijzen naar de OEM-documentatie.

1. Gebruik de **afsluiten** actie zonder problemen afsluiten het scale unit-knooppunt. Deze actie kan niet meer nodig op basis van de fysieke voorwaarde van de hardware. 

2. In het onwaarschijnlijke geval van de afsluiten actie mislukt, gebruikt u de [leegmaken](azure-stack-node-actions.md#drain) actie het scale unit-knooppunt in de onderhoudsmodus plaatsen. Deze actie kan niet meer nodig op basis van de fysieke voorwaarde van de hardware.

   > [!NOTE]  
   > In elk geval slechts één knooppunt kan worden uitgeschakeld en uitgeschakeld op hetzelfde moment zonder te verbreken van de S2D (opslagruimten Direct).

3. Als het scale unit-knooppunt in de onderhoudsmodus bevindt is, gebruikt de [stoppen](azure-stack-node-actions.md#stop) actie. Deze actie kan niet meer nodig op basis van de fysieke voorwaarde van de hardware.

   > [!NOTE]  
   > In het onwaarschijnlijke geval dat het uitschakelen van de actie niet werkt, gebruikt u de webinterface van baseboard management controller (BMC).

4. Vervang de fysieke computer. Dit wordt meestal gedaan door de leverancier van de OEM-hardware.
5. Gebruik de [herstellen](azure-stack-node-actions.md#repair) actie de nieuwe fysieke computer toevoegen aan de schaaleenheid.
6. Gebruik het eindpunt van de bevoegdheden op [Controleer de status van de virtuele schijf herstellen](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Met de nieuwe schijven, een volledig herstel van de opslagtaak kan meerdere uren, afhankelijk van het laden van het systeem duren en ruimte verbruikt.
7. Nadat de herstelactie is voltooid, valideert u dat alle actieve waarschuwingen automatisch is gesloten.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het vervangen van een fysieke schijf, terwijl het systeem wordt ingeschakeld, [een schijf vervangt](azure-stack-replace-disk.md). 
- Zie voor meer informatie over het vervangen van een hardwareonderdeel dat vereist dat het systeem wordt uitgeschakeld, [een hardwareonderdeel vervangt](azure-stack-replace-component.md).
