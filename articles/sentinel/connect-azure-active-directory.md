---
title: Azure AD-gegevens in Azure Sentinel Preview verzamelen | Microsoft Docs
description: Meer informatie over het verzamelen van Azure Active Directory-gegevens in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2019
ms.author: rkarlin
ms.openlocfilehash: b42fb982abb916503d530d5040641ba1dcce3c92
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58575127"
---
# <a name="collect-data-from-azure-active-directory"></a>Verzamelen van gegevens uit Azure Active Directory

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Azure Sentinel kunt u het verzamelen van gegevens uit [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) en stream het naar Azure Sentinel. U kunt naar stream [aanmelden logboeken](../active-directory/reports-monitoring/concept-sign-ins.md) en [auditlogboeken](../active-directory/reports-monitoring/concept-audit-logs.md) .

## <a name="prerequisites"></a>Vereisten

- Als u wilt aanmelden om gegevens te exporteren uit Active Directory, moet u een Azure AD P1 of P2-licentie hebben.

- Gebruiker met de globale beheerder of beveiliging beheerdersmachtigingen heeft op de tenant die u wilt de logboeken van streamen.


## <a name="connect-to-azure-ad"></a>Verbinding maken met Azure AD

1. Selecteer in Azure Sentinel, **gegevensverzameling** en klik vervolgens op de **Azure Active Directory** tegel.

2. Klik naast de logboeken die u wilt streamen naar Azure Sentinel op **Connect**.

6. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Azure AD-waarschuwingen, **SigninLogs** en **AuditLogs**.




## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure AD Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
