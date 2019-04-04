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
ms.date: 04/04/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 04/04/2019
ms.openlocfilehash: ee98aee0873796c2a06db73d3365e3ff9ef87ccf
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58915880"
---
# <a name="asdk-release-notes"></a>Opmerkingen bij de release van ASDK

In dit artikel bevat informatie over wijzigingen, correcties en bekende problemen in de Azure Stack Development Kit (ASDK). Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](../azure-stack-updates.md#determine-the-current-version).

De hoogte blijven van wat is er nieuw in de ASDK Abonneer u op de [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-1903"></a>Build 1903

De nettolading van de 1903 bevat geen een ASDK-release.

## <a name="build-11902069"></a>Build 1.1902.0.69

### <a name="new-features"></a>Nieuwe functies

- De 1902-build introduceert een nieuwe gebruikersinterface van de beheerder van Azure Stack-portal voor het maken van plannen, aanbiedingen, quota's en aanvullende plannen. Zie voor meer informatie, waaronder schermafbeeldingen, [maken, plannen, aanbiedingen en quota's](../azure-stack-create-plan.md).

- Zie voor een lijst van andere wijzigingen en verbeteringen in deze release, [in deze sectie](../azure-stack-update-1902.md#improvements) opmerkingen bij de release in de Azure Stack.

<!-- ### New features

- For a list of new features in this release, see [this section](../azure-stack-update-1902.md#new-features) of the Azure Stack release notes.

### Fixed and known issues

- For a list of issues fixed in this release, see [this section](../azure-stack-update-1902.md#fixed-issues) of the Azure Stack release notes. For a list of known issues, see [this section](../azure-stack-update-1902.md#known-issues-post-installation).
- Note that [available Azure Stack hotfixes](../azure-stack-update-1902.md#azure-stack-hotfixes) are not applicable to the Azure Stack ASDK. -->

### <a name="known-issues"></a>Bekende problemen

- Een probleem is geïdentificeerd waarbij pakketten meer dan 1450 bytes aan een interne Load Balancer (ILB) verwijderd. Het probleem wordt veroorzaakt door van de MTU-instelling op de host wordt te laag voor VXLAN encapsulated pakketten die passeren van de rol, die vanaf 1901 is verplaatst naar de host. Er zijn ten minste twee scenario's die u kunt tegenkomen waarin we dit probleem zelf manifest hebben gezien:

  - SQL-query's met SQL Always-On die achter een interne Load Balancer (ILB), en meer dan 660 bytes zijn.
  - Kubernetes-implementaties mislukken als u probeert in te schakelen van meerdere modellen.  

  Het probleem treedt op wanneer er communicatie tussen een virtuele machine en een ILB in hetzelfde virtuele netwerk, maar op verschillende subnetten. U kunt dit probleem omzeilen door het uitvoeren van de volgende opdrachten in een opdrachtprompt met verhoogde bevoegdheid op de host ASDK:

  ```shell
  netsh interface ipv4 set sub "hostnic" mtu=1660
  netsh interface ipv4 set sub "management" mtu=1660
  ```

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
