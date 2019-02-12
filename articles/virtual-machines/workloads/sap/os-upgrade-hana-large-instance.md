---
title: Upgrade van besturingssysteem voor de SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Besturingssysteem-upgrade uitvoeren voor SAP HANA op Azure (grote instanties)
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
ms.openlocfilehash: d7d451f3831309b4755170915b35a23da8910510
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100751"
---
# <a name="operating-system-upgrade"></a>Upgrade van besturingssysteem
Dit document beschrijft de details van het besturingssysteem op de HANA grote instanties.

>[!NOTE]
>Het bijwerken van het besturingssysteem is de verantwoordelijkheid van klanten, ondersteuning voor Microsoft operations u kan helpen met de belangrijkste gebieden waarmee Kijk uit tijdens de upgrade. Voordat u van plan voor een upgrade bent, moet u de leverancier van uw besturingssysteem ook raadplegen.

Het operationele team van Microsoft installeren op het moment van de HLI eenheid inrichting, het besturingssysteem. Na verloop van de tijd die u zijn verplicht om het besturingssysteem (voorbeeld: Toepassen van patches, afstemmen en upgraden enz.) op de eenheid HLI.

Voordat u belangrijke wijzigingen in het besturingssysteem (bijvoorbeeld een upgrade uitvoert SP1 naar SP2), moet u contact op met Microsoft Operations team door het openen van een ondersteuningsticket te raadplegen.

Neem op uw ticket:

* Uw HLI abonnements-ID.
* De naam van uw server.
* Het patchniveau op dat u van plan bent om toe te passen.
* De datum die u van plan bent deze wijziging. 

Raden we u aan dat u ten minste één week voor de wenselijk upgrade datum vanwege het operationele team controleren als u een upgrade nodig op de blade van uw server met dit ticket openen.


Zie voor de ondersteuningsmatrix van de verschillende versies van de SAP HANA met de verschillende versies van Linux, [SAP Opmerking #2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Bekende problemen

Hier volgen enkele veelvoorkomende bekende problemen tijdens de upgrade:
- Voor de SKU-Type II klasse SKU, worden de software foundation software (SFS) wordt verwijderd nadat het besturingssysteem bijwerken. Nadat het besturingssysteem upgraden, moet u de compatibele SFS opnieuw installeren.
- Ethernet-stuurprogramma's voor kaarten (ENIC en FNIC) teruggedraaid naar de oudere versie. Na de upgrade moet u de compatibele versie van de stuurprogramma's installeren.

## <a name="next-steps"></a>Volgende stappen
- Raadpleeg [back-up en herstel](hana-overview-high-availability-disaster-recovery.md) voor OS back-up van de SKU-Type I-klasse.
- Raadpleeg [OS back-up voor SKU's Type II](os-backup-type-ii-skus.md) voor Type II SKU-klasse.
