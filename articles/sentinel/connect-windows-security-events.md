---
title: Gebeurtenisgegevens voor Windows-beveiliging verbinden met Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met de gegevens van de gebeurtenis van Windows Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 36d38aa82b4f0ec8d7d9ef6ebb1145b1fcc334df
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190574"
---
# <a name="connect-windows-security-events"></a>Verbinding maken met Windows-beveiligingsgebeurtenissen 

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt alle beveiligingsgebeurtenissen van de Windows-Servers die zijn verbonden met uw werkruimte Azure Sentinel streamen. Deze verbinding kunt u dashboards weergeven, aangepaste waarschuwingen maken en onderzoek verbeteren. Dit biedt u meer inzicht in het netwerk van uw organisatie en verbetert uw mogelijkheden voor beveiliging bewerking.  U kunt selecteren welke gebeurtenissen om te streamen:

- **Alle gebeurtenissen** -alle Windows-beveiligings- en AppLocker-gebeurtenissen.
- **Algemene** -een standaardset gebeurtenissen voor controledoeleinden. Een volledige gebruikersaudittrail is opgenomen in deze set. Deze verzameling bevat bijvoorbeeld zowel gebruikersaanmelding en -afmelding (gebeurtenis-ID 4634). We nemen controle van acties zoals wijzigingen, belangrijke domain controller Kerberos operations en andere gebeurtenissen die door de bedrijfstak van organisaties worden aanbevolen.

Gebeurtenissen die zeer laag volume hebben zijn opgenomen in de gemeenschappelijke set als de belangrijkste reden om te kiezen dat het over alle gebeurtenissen is dat er minder i en niet voor het filteren van specifieke gebeurtenissen.
- **Minimale** -een klein aantal gebeurtenissen die op mogelijke bedreigingen wijzen kunnen. Als deze optie inschakelt, kunt u zich niet aan een volledige audittrail hebben.  Deze set omvat alleen de gebeurtenissen die kunnen wijzen op een geslaagde inbreuk en belangrijke gebeurtenissen die een zeer lage volume hebt. Bijvoorbeeld, deze verzameling bevat geslaagde en mislukte gebruikersaanmelding (gebeurtenis-id's 4624, 4625), maar het afmelden is belangrijk voor het controleren van, maar niet zinvol is voor de detectie en relatief hoog volume heeft geen bevat. De meeste van het gegevensvolume van deze set is de aanmeldgebeurtenissen en het maken van procesgebeurtenis (gebeurtenis-ID 4688).
- **Geen** -geen beveiligings- of AppLocker-gebeurtenissen.

> [!NOTE]
> 
> - Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

De volgende lijst bevat een volledige uitsplitsing van de beveiliging en AppLocker gebeurtenis-id's voor elke set:

| Gegevenslaag | Verzamelde gebeurtenis indicatoren |
| --- | --- |
| Minimaal | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Algemeen | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

## <a name="set-up-the-windows-security-events-connector"></a>De Windows security gebeurtenissen connector instellen

Om uw Windows-beveiligingsgebeurtenissen volledig te integreren met Azure Sentinel:

1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors** en klik vervolgens op de **Windows-beveiligingsgebeurtenissen** tegel. 
1. Selecteer welke gegevenstypen die u wilt streamen.
1. Klik op **Bijwerken**.
6. Zoek voor het gebruik van de relevante schema in Log Analytics voor het Windows-beveiligingsgebeurtenissen, **SecurityEvent**.

## <a name="validate-connectivity"></a>Verbinding valideren

Het duurt ongeveer 20 minuten totdat de logboeken in Log Analytics wordt weergegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Windows-beveiligingsgebeurtenissen Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

