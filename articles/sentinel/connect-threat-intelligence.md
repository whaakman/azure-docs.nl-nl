---
title: Gegevens van bedreigingsinformatie verbinden met Azure Sentinel Preview | Microsoft Docs
description: Meer informatie over hoe u verbinding maken met gegevens van bedreigingsinformatie Sentinel van Azure.
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 56412543-5664-44c1-b026-2dbaf78a9a50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: a33b00e4e553824f1f719be2c8518487241f0a11
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60714520"
---
# <a name="connect-data-from-threat-intelligence-providers"></a>Verbinding maken met gegevens van threat intelligence providers 

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Nadat u uw gegevens naar Azure Sentinel streamen, kunt u verrijken deze met de bedreigingsinformatie feed die u in uw organisatie gebruikt. 

Om te controleren cross-waarschuwingen en regels met de waarde true bedreigingsinformatie, bijvoorbeeld als u een waarschuwing ontvangt van een specifiek IP-adres, is uw integratie threat intelligence-provider mogelijk om te laten u weten of dit IP-adres onlangs zijn schadelijke is gevonden , Maakt de integratie van azure Sentinel mogelijk [threat intelligence providers](https://aka.ms/graphsecuritytips). 

U kunt Logboeken van Threat intelligence providers streamen naar Azure Sentinel met één klik. Deze verbinding kunt u indicatoren met verschillende soorten observables zoals IP-adres, domein, URL opnemen en de hash van het bestand om te zoeken en maken van aangepaste regels in Azure Sentinel waarschuwingen.  
> [!NOTE]
> Voer aangepaste threat indicatoren in Azure Sentinel voor gebruik in regels voor waarschuwingen, dashboards en jacht scenario's door te integreren met de [Microsoft Graph Security tiIndicator](https://aka.ms/graphsecuritytiindicators) entiteit of met behulp van een [Microsoft Beveiliging van de grafiek geïntegreerd Threat Intelligence-Platform](https://aka.ms/graphsecuritytips).

## <a name="prerequisites"></a>Vereisten  

- Gebruiker met de globale beheerder of beheerder beveiligingsmachtigingen 

- Threat intelligence toepassingen geïntegreerd met Microsoft Intelligent Security Graph 

## <a name="connect-to-threat-intelligence"></a>Verbinding maken met bedreigingsinformatie 

1. Als u al een provider van threat intelligence, zorg ervoor dat u bladert u naar uw toepassing TIP en toestemming geven om te indicatoren naar Microsoft verzenden en geeft u de service als Azure Sentinel worden.  

2. Selecteer in Azure Sentinel, **gegevensconnectors** en klik vervolgens op de **Bedreigingsinformatie** tegel.

3. Klik op **Verbinden**. 

4. Zoek voor het gebruik van de relevante schema in Log Analytics voor feeds met informatie over bedreigingen, **ThreatIntelligenceIndicator**. 

 
## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u verbinding maken met uw provider Bedreigingsinformatie Sentinel van Azure. Zie de volgende artikelen voor meer informatie over Azure Sentinel.

- Als u wilt aan de slag met Azure Sentinel, moet u een abonnement op Microsoft Azure. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free/).
- Meer informatie over het [onboarding uw gegevens naar Azure Sentinel](quickstart-onboard.md), en [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
