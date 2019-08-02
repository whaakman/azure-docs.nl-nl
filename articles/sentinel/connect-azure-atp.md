---
title: Azure ATP-gegevens verbinden met Azure Sentinel preview | Microsoft Docs
description: Meer informatie over het verbinden van Azure ATP-gegevens met Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2019
ms.author: rkarlin
ms.openlocfilehash: 6f41a5704c783ba8aeab7bc2e82ef731c6a257ac
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599153"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Gegevens verbinden vanuit Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.


U kunt Logboeken van [Azure Advanced Threat Protection](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) streamen naar Azure Sentinel met één klik.

## <a name="prerequisites"></a>Vereisten

- Gebruiker met de machtigingen globale beheerder of beveiligings beheerder
- U moet een preview-klant zijn van Azure ATP

## <a name="connect-to-azure-atp"></a>Verbinding maken met Azure ATP

Zorg ervoor dat de preview-versie van Azure ATP is [ingeschakeld in uw netwerk](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Als Azure ATP is geïmplementeerd en uw gegevens worden opgenomen, kunnen verdachte waarschuwingen eenvoudig worden gestreamd naar Azure Sentinel. Het kan tot 24 uur duren voordat de waarschuwingen streamen naar Azure Sentinel starten.


1. Als u Azure ATP wilt verbinden met Azure Sentinel, moet u eerst integratie tussen Azure ATP en Microsoft Cloud App Security inschakelen. Zie [integratie van Azure Advanced Threat Protection](https://docs.microsoft.com/cloud-app-security/aatp-integration)voor informatie over hoe u dit doet.

1. Selecteer in azure Sentinel **Data connectors** en klik vervolgens op de tegel **Azure ATP** .

2. Klik op**Verbinden**.

6. Als u het relevante schema in Log Analytics voor de Azure ATP-waarschuwingen wilt gebruiken, zoekt u naar **SecurityAlert**.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u Azure Advanced Threat Protection kunt verbinden met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

