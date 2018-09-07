---
title: Gegevens in lagen en extensie knooppunten voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Gegevens in lagen en extensie-knooppunten voor SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 87c2cb3e373b76685fca09eb0cfeefdc9216df77
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028192"
---
# <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>SAP HANA-gegevens in lagen en extensie-knooppunten gebruiken

SAP biedt ondersteuning voor een gegevensmodel warmtemeting voor SAP BW van verschillende versies van SAP NetWeaver en SAP BW/4HANA. Raadpleeg het document SAP voor meer informatie over het gegevensmodel voor cloudlagen [SAP BW/4HANA en SAP BW op HANA met SAP HANA-extensie knooppunten](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
Met HANA grote instantie kunt u de optie-1-configuratie van SAP HANA-extensie knooppunten, zoals wordt beschreven in de veelgestelde vragen en SAP blog-documenten. Optie 2-configuraties kunnen worden ingesteld met de volgende HANA grote instantie SKU's: S72m, S192, S192m, S384 en S384m. 

Wanneer u de documentatie bekijkt, het voordeel mogelijk niet meer zichtbaar onmiddellijk. Maar wanneer u de richtlijnen van de grootte SAP bekijkt, ziet u een voordeel met behulp van de optie-1 en de optie-2 SAP HANA-extensie-knooppunten. Hier volgen enkele voorbeelden:

- Richtlijnen voor SAP HANA-grootte is meestal de dubbele hoeveelheid gegevensvolume als geheugen vereist. Wanneer u uw SAP HANA-instantie met de dynamische gegevens uitvoert, hebt u slechts 50% of minder van het geheugen gevuld met gegevens. De rest van het geheugen worden in het ideale geval bewaard voor SAP HANA zijn werk doet.
- Dat betekent dat in een HANA grote instantie S192-eenheid met 2 TB aan geheugen, met een SAP BW-database, hoeft u slechts 1 TB als gegevensvolume.
- Als u een extra knooppunt van SAP HANA-extensie van de optie-1, ook een S192 HANA grote instantie SKU, dit biedt u een extra capaciteit van 2 TB voor gegevensvolume. In de optie-2-configuratie krijgt u een extra 4 TB voor warme gegevensvolume. Vergeleken met het knooppunt ' hot ', kan de volledige geheugencapaciteit van het knooppunt 'warme' extensie worden gebruikt voor het opslaan van gegevens voor de optie-1. Dubbele het geheugen kan worden gebruikt voor gegevensvolume in optie 2 SAP HANA-extensieconfiguratie in het knooppunt.
- U uiteindelijk een capaciteit van 3 TB voor uw gegevens en een ' hot ' naar warm-verhouding van 1:2 voor optie 1. U hebt 5 TB aan gegevens en een 1:4-verhouding met de configuratie voor de uitbreiding van de optie-2-knooppunt.

Hoe hoger het gegevensvolume in vergelijking met het geheugen, hoe hoger de kans op zijn dat de warme gegevens die u daarvoor is opgeslagen op de schijfopslag.

**Volgende stappen**
- Raadpleeg [SAP HANA (grote instanties)-architectuur op Azure](hana-architecture.md)