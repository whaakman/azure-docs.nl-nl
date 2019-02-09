---
title: Opmerkingen bij de release van Microsoft Azure Stack Development Kit | Microsoft Docs
description: Verbeteringen, correcties en bekende problemen voor Azure Stack Development Kit.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 02/09/2019
ms.openlocfilehash: 35a3850cdda7f6844258439b74b29dcc20852a89
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55959592"
---
# <a name="asdk-release-notes"></a>Opmerkingen bij de release van ASDK

In dit artikel bevat informatie over wijzigingen, correcties en bekende problemen in de Azure Stack Development Kit (ASDK). Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](../azure-stack-updates.md#determine-the-current-version).

De hoogte blijven van wat is er nieuw in de ASDK Abonneer u op de [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11901095"></a>Build 1.1901.0.95

### <a name="changes"></a>Wijzigingen

Deze versie bevat de volgende verbeteringen voor Azure Stack:

- BGP- en NAT-onderdelen zijn nu geïmplementeerd op de fysieke host. Dit elimineert de noodzaak om twee openbare of zakelijke IP-adressen voor het implementeren van de ASDK en ook vereenvoudigt de implementatie.
- Azure Stack-geïntegreerde systemen back-ups nu kunnen [worden gevalideerd](asdk-validate-backup.md) met behulp van de **asdk installer.ps1** PowerShell-script.

### <a name="new-features"></a>Nieuwe functies

- Zie voor een lijst met nieuwe functies in deze release [in deze sectie](../azure-stack-update-1901.md#new-features) opmerkingen bij de release van de Azure Stack.

### <a name="fixed-and-known-issues"></a>Vaste en bekende problemen

- Zie voor een lijst met problemen in deze release worden opgelost, [in deze sectie](../azure-stack-update-1901.md#fixed-issues) opmerkingen bij de release van de Azure Stack. Zie voor een lijst van bekende problemen, [in deze sectie](../azure-stack-update-1901.md#known-issues-post-installation).
- Houd er rekening mee dat [beschikbare Azure Stack-hotfixes](../azure-stack-update-1901.md#azure-stack-hotfixes) zijn niet van toepassing op de Azure Stack-ASDK.

## <a name="build-118110101"></a>Build 1.1811.0.101

### <a name="changes"></a>Wijzigingen

Deze versie bevat de volgende verbeteringen voor Azure Stack:  

- Er is een set nieuwe minimale en aanbevolen hardware- en softwarevereisten voor de ASDK. Deze nieuwe aanbevolen specificaties zijn gedocumenteerd in [Azure Stack-implementatie planningsoverwegingen](asdk-deploy-considerations.md). Als de Azure Stack-platform heeft zich ontwikkeld, meer services zijn nu beschikbaar en andere bronnen mogelijk vereist. De specificaties van de verbeterde weerspiegelen deze herziene aanbevelingen.

### <a name="new-features"></a>Nieuwe functies

Zie voor een lijst met nieuwe functies in deze release [in deze sectie](../azure-stack-update-1811.md#new-features) opmerkingen bij de release van de Azure Stack.

### <a name="fixed-and-known-issues"></a>Vaste en bekende problemen

Zie voor een lijst met problemen in deze release worden opgelost, [in deze sectie](../azure-stack-update-1811.md#fixed-issues) opmerkingen bij de release van de Azure Stack. Zie voor een lijst van bekende problemen, [in deze sectie](../azure-stack-update-1811.md#known-issues-post-installation).

## <a name="build-11809090"></a>Build 1.1809.0.90

### <a name="new-features"></a>Nieuwe functies

Zie voor een lijst met nieuwe functies in deze release [in deze sectie](../azure-stack-update-1809.md#new-features) opmerkingen bij de release van de Azure Stack.

### <a name="fixed-issues"></a>Problemen opgelost

Zie voor een lijst met problemen in deze release worden opgelost, [in deze sectie](../azure-stack-update-1809.md#fixed-issues).

### <a name="known-issues"></a>Bekende problemen

Zie voor een lijst van bekende problemen in deze release, [in deze sectie](../azure-stack-update-1809.md#known-issues-post-installation).
