---
title: Cloud App Security gegevens verbinden met Azure Sentinel preview | Microsoft Docs
description: Meer informatie over het verbinden van Cloud App Security gegevens met Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: cd9e5e27-fdd4-4717-8924-be4c1c430f23
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2019
ms.author: rkarlin
ms.openlocfilehash: 92de4120d73af70c5de013488344a15ba46513c7
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881091"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Verbinding maken met gegevens van Microsoft Cloud App Security 

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt Logboeken van [Cloud app Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) streamen naar Azure Sentinel met één klik. Met deze verbinding kunt u de waarschuwingen van Cloud App Security naar Azure Sentinel streamen. 

## <a name="prerequisites"></a>Vereisten

- Gebruiker met de machtigingen globale beheerder of beveiligings beheerder

## <a name="connect-to-cloud-app-security"></a>Verbinding maken met Cloud App Security

Als u Cloud App Security al hebt, controleert u of deze is [ingeschakeld in uw netwerk](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Als Cloud App Security is geïmplementeerd en opnamet van uw gegevens, kunnen de waarschuwings gegevens eenvoudig worden gestreamd naar Azure Sentinel.


1. Selecteer in azure Sentinel **Data connectors** en klik vervolgens op de tegel **Cloud app Security** .

2. Klik op**Verbinden**.

3. Als u het relevante schema in Log Analytics voor de Cloud App Security waarschuwingen wilt gebruiken, zoekt u naar **SecurityAlert**.


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u Microsoft Cloud App Security kunt verbinden met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
