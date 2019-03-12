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
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: bb9e5ba960251f728e14106ab1c586e1d3ef373f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57538643"
---
# <a name="asdk-release-notes"></a>Opmerkingen bij de release van ASDK

In dit artikel bevat informatie over wijzigingen, correcties en bekende problemen in de Azure Stack Development Kit (ASDK). Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](../azure-stack-updates.md#determine-the-current-version).

De hoogte blijven van wat is er nieuw in de ASDK Abonneer u op de [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11902069"></a>Build 1.1902.0.69

### <a name="changes"></a>Wijzigingen

- De 1902-build introduceert een nieuwe gebruikersinterface van de beheerder van Azure Stack-portal voor het maken van plannen, aanbiedingen, quota's en aanvullende plannen. Zie voor meer informatie, waaronder schermafbeeldingen, [maken, plannen, aanbiedingen en quota's](../azure-stack-create-plan.md).

<!-- ### New features

- For a list of new features in this release, see [this section](../azure-stack-update-1902.md#new-features) of the Azure Stack release notes.

### Fixed and known issues

- For a list of issues fixed in this release, see [this section](../azure-stack-update-1902.md#fixed-issues) of the Azure Stack release notes. For a list of known issues, see [this section](../azure-stack-update-1902.md#known-issues-post-installation).
- Note that [available Azure Stack hotfixes](../azure-stack-update-1902.md#azure-stack-hotfixes) are not applicable to the Azure Stack ASDK. -->

## <a name="build-11901095"></a>Build 1.1901.0.95

Zie de [belangrijke informatie in de opmerkingen bij de release van de Azure Stack](../azure-stack-update-1901.md#build-reference).

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
