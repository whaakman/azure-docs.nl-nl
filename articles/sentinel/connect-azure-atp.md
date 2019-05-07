---
title: Verbinding maken met Azure ATP-gegevens naar Azure Sentinel Preview | Microsoft Docs
description: Informatie over het verbinden met Azure ATP-gegevens Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: f0d86a62c59df5bebd34137d0903fcaa7014573d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204273"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Verbinding maken met gegevens uit Azure Advanced Threat Protection (ATP)

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



1. Selecteer in Azure Sentinel, **gegevensconnectors** en klik vervolgens op de **Azure ATP** tegel.

2. Klik op **Verbinden**.

6. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Azure ATP-meldingen, **SecurityAlert**.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure Advanced Threat Protection Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

