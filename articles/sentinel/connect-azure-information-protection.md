---
title: Het verzamelen van gegevens in Azure Sentinel Preview Azure Information Protection | Microsoft Docs
description: Meer informatie over het verzamelen van gegevens in Azure Sentinel Azure Information Protection.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: db87561fc8c94776335ea910f8a4c0577d5f51fc
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56993046"
---
# <a name="collect-data-from-azure-information-protection"></a>Verzamelen van gegevens van Azure Information Protection

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt de logboeken van streamen [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) in Azure Sentinel met één klik. Azure Information Protection helpt uw gegevens te beschermen, ongeacht of deze opgeslagen in de cloud of on-premises infrastructuren en beheer en hulp voor beveiligde e-mail, documenten en gevoelige gegevens die u buiten uw bedrijf delen. Verbeter de gegevensbescherming te allen tijde met Azure Information Protection van eenvoudige classificatie voor ingesloten labels en machtigingen. Wanneer u verbinding met Azure Information Protection voor Azure Sentinel, u stream alle waarschuwingen van Azure Information Protection in Azure Sentinel.


## <a name="prerequisites"></a>Vereisten

- Gebruiker met de globale beheerder, beveiligingsbeheerder of machtigingen voor beveiliging


## <a name="connect-to-azure-information-protection"></a>Verbinding maken met Azure Information Protection

Als u al Azure Information Protection hebt, zorgt ervoor dat deze [ingeschakeld op uw netwerk](https://docs.microsoft.com/azure/information-protection/activate-service).
Als Azure Information Protection wordt geïmplementeerd en ophalen van gegevens, gegevens van de waarschuwing kan eenvoudig worden gestreamd naar Azure Sentinel.


1. Selecteer in Azure Sentinel, **gegevensverzameling** en klik vervolgens op de **Azure Information Protection** tegel.

2. Ga naar de [Azure Information Protection-portal](https://portal.azure.com/?ScannerConfiguration=true&EndpointDiscovery=true#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/quickstartBlade) 

3. Onder **verbinding**, instellen van het streamen van Logboeken van Azure Information Protection voor Azure Sentinel door te klikken op [analytics configureren](https://portal.azure.com/#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/analyticsOnboardBlade)

4. Selecteer de werkruimte waarin u geïmplementeerd Sentinel van Azure. 

5. Klik op **OK**.

6. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Azure Information Protection-waarschuwingen, **InformationProtectionLogs_CL**.




## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure Information Protection Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
