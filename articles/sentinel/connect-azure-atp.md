---
title: Azure ATP-gegevens in Azure Sentinel Preview verzamelen | Microsoft Docs
description: Meer informatie over het verzamelen van Azure ATP-gegevens in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2019
ms.author: rkarlin
ms.openlocfilehash: 4415e8527852955f93402abb6aa068b9dd6b1197
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992906"
---
# <a name="collect-data-from-azure-advanced-threat-protection-atp"></a>Verzamelen van gegevens uit Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.


U kunt de logboeken van streamen [Azure Advanced Threat Protection](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) in Azure Sentinel met één klik.

## <a name="prerequisites"></a>Vereisten

- Gebruiker met de globale beheerder of beheerder beveiligingsmachtigingen
- U moet de klant van een beperkte Preview-versie van Azure ATP

## <a name="connect-to-azure-atp"></a>Verbinding maken met Azure ATP

Zorg ervoor dat de Azure ATP private preview-versie is [ingeschakeld op uw netwerk](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Als Azure ATP is geïmplementeerd en ophalen van uw gegevens, de verdachte waarschuwingen kunnen eenvoudig worden gestreamd naar Azure Sentinel. Het duurt maximaal 24 uur voor de waarschuwingen om te streamen naar Azure Sentinel starten.



1. Selecteer in Azure Sentinel, **gegevensverzameling** en klik vervolgens op de **Azure ATP** tegel.

2. Klik op **Verbinden**.


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure Advanced Threat Protection Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

