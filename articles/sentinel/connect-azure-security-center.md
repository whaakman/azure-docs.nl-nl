---
title: Het verzamelen van gegevens in Azure Sentinel Preview Azure Security Center | Microsoft Docs
description: Meer informatie over het verzamelen van gegevens in Azure Sentinel Azure Security Center.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 1770d5a1fd5598972e5966ad041d35b4a9c9138d
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56993130"
---
# <a name="collect-data-from-azure-security-center"></a>Gegevens verzamelen over Azure Security Center

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.



Azure Sentinel kunt u voor het verzamelen van waarschuwingen van [Azure Security Center](../security-center/security-center-intro.md) kunt streamen naar Azure Sentinel. 

## <a name="prerequisites"></a>Vereisten

- Als u exporteren van waarschuwingen van Azure Security Center wilt, moet u een bijdrager voor het abonnement waarvan u streamen logboeken zijn.

- Hebt u de [Azure Security Center Standard-laag](../security-center/security-center-pricing.md) die worden uitgevoerd op het abonnement. Als dit niet het geval is, [uw abonnement upgraden naar standard](https://azure.microsoft.com/pricing/details/security-center/).

- U moet zich aanmelden als gebruiker die de globale beheerder of beheerdersmachtigingen heeft voor elk abonnement dat u verbinding wilt maken.


## <a name="connect-to-azure-security-center"></a>Verbinding maken met Azure Security Center

1. Selecteer in Azure Sentinel, **gegevensverzameling** en klik vervolgens op de **Azure Security Center** tegel.
1. Klik in het recht op **Connect** naast elk abonnement waarvan u wilt streamen naar Azure Sentinel waarschuwingen. Zorg ervoor dat u elk abonnement upgraden naar stream waarschuwingen naar Azure Sentinel Azure Security Center Standard-laag.

3. Klik op **Verbinden**.

4. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Azure Security Center-waarschuwingen, **SecurityEvent**.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure Security Center Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
