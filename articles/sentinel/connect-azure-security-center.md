---
title: Azure Security Center-gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Informatie over het verbinden met Azure Security Center gegevens Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: bd6fe027da8c16157c7fce6c348e49c0a6b73ea3
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59490668"
---
# <a name="connect-data-from-azure-security-center"></a>Verbinding maken met gegevens van Azure Security Center

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.



Azure Sentinel kunt u verbinding maken van waarschuwingen van [Azure Security Center](../security-center/security-center-intro.md) kunt streamen naar Azure Sentinel. 

## <a name="prerequisites"></a>Vereisten

- Als u exporteren van waarschuwingen van Azure Security Center wilt, moet u een bijdrager voor het abonnement waarvan u streamen logboeken zijn.

- Hebt u de [Azure Security Center Standard-laag](../security-center/security-center-pricing.md) die worden uitgevoerd op het abonnement. Als dit niet het geval is, [uw abonnement upgraden naar standard](https://azure.microsoft.com/pricing/details/security-center/).

- U moet zich aanmelden als gebruiker die de globale beheerder of beheerdersmachtigingen heeft voor elk abonnement dat u verbinding wilt maken.


## <a name="connect-to-azure-security-center"></a>Verbinding maken met Azure Security Center

1. Selecteer in Azure Sentinel, **gegevensconnectors** en klik vervolgens op de **Azure Security Center** tegel.
1. Klik in het recht op **Connect** naast elk abonnement waarvan u wilt streamen naar Azure Sentinel waarschuwingen. Zorg ervoor dat u elk abonnement upgraden naar stream waarschuwingen naar Azure Sentinel Azure Security Center Standard-laag.

3. Klik op **Verbinden**.

4. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Azure Security Center-waarschuwingen, **SecurityEvent**.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure Security Center Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
