---
title: "Vervangen van een knooppunt scale unit op een Azure-Stack geïntegreerd systeem | Microsoft Docs"
description: "Informatie over het vervangen van een knooppunt van de fysieke scale unit op het systeem van een Azure-Stack geïntegreerd."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: f9434689-ee66-493c-a237-5c81e528e5de
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: mabrigg
ms.openlocfilehash: f3d6215b672be0d8de8ba0d94320b1a2413eadf1
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Een knooppunt scale unit op een Azure-Stack geïntegreerd systeem vervangen

*Van toepassing op: Azure Stack geïntegreerd systemen*

Dit artikel wordt beschreven voor het algemene proces ter vervanging van een fysieke computer (ook wel een *scale unit knooppunt*) op een Azure-Stack geïntegreerd systeem. Werkelijke scale unit knooppunt vervanging stappen varieert gebaseerd op uw hardwareleverancier oorspronkelijke leveranciers (OEM). Raadpleeg uw leverancier veld FRU (replaceable unit)-documentatie voor gedetailleerde stappen die specifiek voor uw systeem zijn.

De volgende stroom diagram toont de algemene procedure FRU ter vervanging van een volledige scale unit-knooppunt.

![Stroomdiagram voor vervangen knooppunt proces](media/azure-stack-replace-node/ReplaceNodeFlow.PNG)

* Deze actie kan niet vereist op basis van de fysieke toestand van de hardware.

## <a name="review-alert-information"></a>Bekijk informatie over waarschuwingen

Als een scale unit-knooppunt niet actief is, ontvangt u alle kritieke waarschuwingen in het volgende:

- Knooppunt niet is verbonden met de netwerkcontroller
- Knooppunt dat toegankelijk is voor plaatsing van virtuele machines
- Scale unit knooppunt is offline

![Lijst met waarschuwingen voor schaaleenheid omlaag](media/azure-stack-replace-node/NodeDownAlerts.PNG)

Als u de waarschuwing 'Scale unit knooppunt is offline' opent, bevat de beschrijving van de waarschuwing het scale unit-knooppunt dat is niet toegankelijk. U kunt ook extra waarschuwingen ontvangen in de oplossing voor OEM-specifieke controle die wordt uitgevoerd op de host van de levenscyclus van hardware.

![Details van knooppunt offline waarschuwing](media/azure-stack-replace-node/NodeOffline.PNG)

## <a name="scale-unit-node-replacement-process"></a>Proces voor Scale unit knooppunt het vervangen

De volgende stappen zijn opgegeven als een overzicht van het proces scale unit knooppunt vervanging. Raadpleeg de OEM-hardwareleverancier FRU-documentatie voor gedetailleerde stappen die specifiek voor uw systeem zijn. Volg deze stappen niet zonder te verwijzen naar de OEM geleverde documentatie.

1. Gebruik de [leegmaakt](azure-stack-node-actions.md#scale-unit-node-actions) actie het scale unit-knooppunt in onderhoudsmodus te plaatsen. Deze actie kan niet vereist op basis van de fysieke toestand van de hardware.
2. Als het knooppunt wordt nog steeds ingeschakeld, gebruikt u de [uitschakelen](azure-stack-node-actions.md#scale-unit-node-actions) in te grijpen. Deze actie kan niet vereist op basis van de fysieke toestand van de hardware.
 
   > [!NOTE]
   > In het onwaarschijnlijke geval dat het uitschakelen van de actie niet werkt, gebruikt u de baseboard management controller (BMC) webinterface.

1. Vervang de fysieke computer. Dit gebeurt doorgaans door de leverancier van de OEM-hardware.
2. Gebruik de [reparatie](azure-stack-node-actions.md#scale-unit-node-actions) actie de nieuwe fysieke computer toevoegen aan de schaaleenheid.
3. Gebruik van de bevoegde eindpunt [Controleer de status van de virtuele schijf repareren](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Met nieuwe gegevensstations, een volledig herstel van de opslagtaak kan meerdere uren, afhankelijk van de systeembelasting duren en ruimte verbruikt.
4. Nadat de herstelactie is voltooid, valideert u dat alle actieve waarschuwingen automatisch is gesloten.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het vervangen van een fysieke hot verwisselbare schijf [een schijf vervangen](azure-stack-replace-disk.md). 
- Zie voor meer informatie over het vervangen van een niet hot-verwisselbare hardwareonderdeel [computerhardware](azure-stack-replace-component.md). 