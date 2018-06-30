---
title: Upgrade van het besturingssysteem voor de SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Upgrade uitvoert voor SAP HANA in Azure (grote exemplaren)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cff9be3b074dde4a0335675663133a8df81ae62d
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114589"
---
# <a name="operating-system-upgrade"></a>Upgrade van besturingssysteem
Dit document beschrijft de details op het besturingssysteem is bijgewerkt op de grote HANA-exemplaren.

>[!NOTE]
>Het bijwerken van het besturingssysteem is de verantwoordelijkheid van klanten, ondersteuning voor Microsoft operations u kan helpen met de belangrijkste gebieden waarmee tijdens de upgrade moet u uitkijken. Voordat u van plan een upgrade bent, dient u de leverancier van uw besturingssysteem ook raadplegen.

Het operationele team van Microsoft installeren op het moment van de inrichting HLI-eenheid, het besturingssysteem. Via de tijd, moet u het besturingssysteem behouden (voorbeeld: patchen, afstemmen, upgraden enz.) op de eenheid HLI.

Voordat u belangrijke wijzigingen in het besturingssysteem (bijvoorbeeld upgraden SP1 naar SP2), moet u contact op met Microsoft Operations team door het openen van een ondersteuningsticket te raadplegen.


Zie voor de ondersteuningsmatrix van de verschillende SAP HANA-versies met de verschillende versies van Linux [SAP-notitie #2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Bekende problemen

Hier volgen enkele bekende veelvoorkomende problemen tijdens de upgrade:
- Op SKU Type II klasse SKU, worden de software-foundation-software (SFS) wordt verwijderd nadat het besturingssysteem upgraden. Nadat het besturingssysteem upgraden, moet u de compatibel SFS opnieuw installeren.
- Ethernet-stuurprogramma's voor kaarten (ENIC en FNIC) wordt teruggezet naar de oudere versie. U moet de compatibele versie van de stuurprogramma's opnieuw installeren na de upgrade.

## <a name="next-steps"></a>Volgende stappen
- Raadpleeg [back-up en herstel](hana-overview-high-availability-disaster-recovery.md) voor OS back-up van Type I SKU-klasse.
- Raadpleeg [OS back-up voor Type II SKU's](os-backup-type-ii-skus.md) voor Type II SKU-klasse.