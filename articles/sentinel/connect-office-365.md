---
title: Verzamelen van Office 365-gegevens in Azure Sentinel Preview | Microsoft Docs
description: Meer informatie over het verzamelen van Office 365-gegevens in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ff7c862e-2e23-4a28-bd18-f2924a30899d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/26/2019
ms.author: rkarlin
ms.openlocfilehash: ad501958a5f88c821e48a3e21f69a960160b3c8e
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574855"
---
# <a name="collect-data-from-office-365-logs"></a>Verzamelen van gegevens van Office 365-Logboeken

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt de logboeken voor controle van streamen [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) in Azure Sentinel met één klik. U kunt auditlogboeken van meerdere tenants aan één werkruimte in Azure Sentinel streamen. De Office 365 activiteit log-connector biedt inzicht in de lopende activiteiten. U krijgt informatie over verschillende gebruiker, admin, systeem, en acties en gebeurtenissen van Office 365. U kunt deze gegevens door verbinding te maken van Office 365-Logboeken in Azure Sentinel gebruiken dashboards weergeven, het maken van aangepaste waarschuwingen en het verbeteren van uw onderzoek voortzetten.


## <a name="prerequisites"></a>Vereisten

- U moet een globale beheerder of beveiligingsbeheerder voor uw tenant
- Op de computer van waaruit u aangemeld bij Azure Sentinel om de verbinding te maken is maken suretha poort 4433 openen voor webverkeer.

## <a name="connect-to-office-365"></a>Verbinding maken met Office 365

1. Selecteer in Azure Sentinel, **gegevensverzameling** en klik vervolgens op de **Office 365** tegel.

2. Als u nog niet hebt ingeschakeld, onder **verbinding** gebruiken de **inschakelen** knop de Office 365-oplossing in te schakelen. Als deze al is ingeschakeld, wordt deze aangeduid in het scherm verbinding is als al ingeschakeld.
1. Office 365 kunt u van meerdere tenants naar Azure Sentinel om gegevens te streamen. Voor elke tenant die u verbinding maken wilt met de tenant onder toevoegen **tenants verbinding met het Azure-Sentinel**. 
1. Een Active Directory-scherm wordt geopend. U wordt gevraagd om te verifiëren met een globale beheerder op elke tenant die u wilt verbinding maken met Azure Sentinel en machtigingen verlenen aan Azure Sentinel naar de logboeken te lezen. 
5. Onder Stream Office 365-activiteitenlogboeken, klikt u op **Selecteer** om te kiezen welke typen logboeken die u wilt streamen naar Azure Sentinel. Azure Sentinel ondersteunt momenteel, Exchange en SharePoint.

4. Klik op **wijzigingen toepassen**.

3. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Office 365-Logboeken, **OfficeActivity**.


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Office 365 Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

