---
title: Verzamelen van gegevens van Azure AD Identity Protection in Azure Sentinel Preview | Microsoft Docs
description: Meer informatie over het verzamelen van gegevens in Azure Sentinel Azure AD Identity Protection.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 91c870e5-2669-437f-9896-ee6c7fe1d51d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 609aced38b7e30f78d81934867196c568dcc85ca
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57239995"
---
# <a name="collect-data-from-azure-ad-identity-protection"></a>Gegevens verzamelen van Azure AD Identity Protection

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt de logboeken van streamen [Azure AD Identity Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) in Azure Sentinel naar stream waarschuwingen in Azure Sentinel dashboards weergeven, aangepaste waarschuwingen maken en onderzoek te verbeteren. Azure Active Directory Identity Protection biedt een geconsolideerde weergave op gebruikers risico lopen, risicogebeurtenissen en beveiligingsproblemen, met de mogelijkheid om risico's direct herstellen en beleid voor het automatisch herstellen van toekomstige gebeurtenissen instellen. De service is gebouwd op de ervaring van Microsoft voor het beveiligen van id's voor consumenten en krijgt enorm veel nauwkeurigheid van het signaal uit meer dan 13 miljard log aanmeldingen per dag. 


## <a name="prerequisites"></a>Vereisten

- Hebt u een [Azure Active Directory Premium P1 of P2-licentie](https://azure.microsoft.com/pricing/details/active-directory/)
- Gebruiker met de globale beheerder of beheerder beveiligingsmachtigingen


## <a name="connect-to-azure-ad-identity-protection"></a>Verbinding maken met Azure AD Identity Protection

Als u al Azure AD Identity Protection hebt, zorgt ervoor dat deze [ingeschakeld op uw netwerk](../active-directory/identity-protection/enable.md).
Als Azure AD Identity Protection is geïmplementeerd en ophalen van gegevens, gegevens van de waarschuwing kan eenvoudig worden gestreamd naar Azure Sentinel.


1. Selecteer in Azure Sentinel, **gegevensverzameling** en klik vervolgens op de **Azure AD Identity Protection** tegel.

2. Klik op **Connect** om te beginnen met het streamen van Azure AD Identity Protection-gebeurtenissen naar Azure Sentinel.


6. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Azure AD Identity Protection-waarschuwingen, **IdentityProtectionLogs_CL**.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure AD Identity Protection Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
