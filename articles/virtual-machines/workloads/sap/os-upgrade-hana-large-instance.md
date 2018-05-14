---
title: Upgrade van het besturingssysteem voor de SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Upgrade uitvoert voor SAP HANA in Azure (grote exemplaren)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: timlt
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1354b67ade78a0da1578ef9d98c3b1e8edb41cd4
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="operating-system-upgrade"></a>Upgrade van besturingssysteem
Dit document beschrijft de details op het besturingssysteem is bijgewerkt op de grote HANA-exemplaren.

>[!NOTE]
>Het bijwerken van het besturingssysteem is de verantwoordelijkheid van klanten, ondersteuning voor Microsoft operations u kan helpen met de belangrijkste gebieden waarmee tijdens de upgrade moet u uitkijken. Voordat u van plan een upgrade bent, dient u de leverancier van uw besturingssysteem ook raadplegen.

Het operationele team van Microsoft installeren op het moment van de inrichting HLI-eenheid, het besturingssysteem. Via de tijd, moet u het besturingssysteem behouden (voorbeeld: patchen, afstemmen, upgraden enz.) op de eenheid HLI.

Voordat u wijzigingen in het besturingssysteem (bijvoorbeeld een OS Upgrade), primaire u **moet** Houd rekening met de volgende compatibiliteit matrix. U **moet** ook contact opnemen met Microsoft Operations team door het openen van een ondersteuningsticket te raadplegen voordat u begint met de grote besturingssysteem activiteiten zoals de upgrade.

Zie voor de ondersteuningsmatrix van de verschillende SAP HANA-versies met de verschillende versies van Linux [SAP-notitie #2235581](https://launchpad.support.sap.com/#/notes/2235581).

De volgende compatibiliteit is voor de HLIs getest. Als uw server HLI buiten de compatibiliteit matrix, moet u contact op met de ondersteuning van de Microsoft-bewerking.

## <a name="for-type-i-class-sku-category"></a>Voor het Type klasse ik SKU-categorie

| Configuratie | SUSE12 SP1 | SUSE12 SP2 | RHEL 7.2 | RHEL 7.3|
| --- | --- | --- | --- | --- |
| Server-Firmware | 3.1(2b) | 3.1(2b) | 3.1(2b) | 3.1(2b) |
| ENIC versie | 2.3.0.44 | 2.3.0.44 | 2.3.0.30 | 2.3.0.44 |
| FNIC versie | 1.6.0.34 | 1.6.0.34 | 1.6.0.27 | 1.6.0.36 |
| EDAC | Uitgeschakeld | Uitgeschakeld | Uitgeschakeld | Uitgeschakeld |
| Kernelversie | 4.4.21-69-default | 3.12.49-11-default | 3.10.0-327.el7.x86_64 | 3.10.0-693.17.1 |


## <a name="for-type-ii-class-sku-category"></a>Voor Type II klasse SKU categorie

| Configuratie | SUSE12 SP1 | SUSE12 SP2 | RHEL 7.2 | RHEL 7.3|
| --- | --- | --- | --- | --- |
| De firmwareversie RMC | 1.1.121  | 1.1.121  | 1.1.121  | 1.1.121 |
| De firmwareversie BMC | 1.0.43   | 1.0.43   | 1.0.43   | 1.0.43  |
| Softwareversie Foundation Server (SFS) | 2,16    | 2,16    | 2.14/2.16   | 2,16   |
| BIOS | 5.2.6    | 5.2.6    | 5.2.6    | 5.2.6   |
| i40e versie    | 2.0.19     | 2.0.19     | 1.5.10-k    | 1.5.10-k   |
| Kernelversie    | 3.12.49-11.1     | 4.4.21-69.1     | 3.10.0-327    | 3.10.0-693.17.1   |


## <a name="known-issues"></a>Bekende problemen

Hier volgen enkele bekende veelvoorkomende problemen tijdens de upgrade:
- Op SKU Type II klasse SKU, worden de software-foundation-software (SFS) wordt verwijderd nadat het besturingssysteem upgraden. Nadat het besturingssysteem upgraden, moet u de compatibel SFS opnieuw installeren.
- Ethernet-stuurprogramma's voor kaarten (ENIC en FNIC) wordt teruggezet naar de oudere versie. U moet de compatibele versie van de stuurprogramma's opnieuw installeren na de upgrade.

## <a name="next-steps"></a>Volgende stappen
- Raadpleeg [back-up en herstel](hana-overview-high-availability-disaster-recovery.md) voor OS back-up van Type I SKU-klasse.
- Raadpleeg [OS back-up voor Type II SKU's](os-backup-type-ii-skus.md) voor Type II SKU-klasse.