---
title: Quota en limieten voor Azure Batch service | Microsoft Docs
description: Meer informatie over Azure Batch standaardquota, limieten en beperkingen en verhoogt het quotum aanvragen
services: batch
documentationcenter: 
author: v-dotren
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 210ba4a90f24ce9b0b55c4565028232c2b7fd7cc
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="batch-service-quotas-and-limits"></a>Quota en limieten voor Batch-service

Zoals met andere Azure-services, moet u er limieten op bepaalde resources die zijn gekoppeld aan de Batch-service zijn. Veel van deze limieten zijn standaardquota door Azure wordt toegepast op het niveau van de account of het abonnement. In dit artikel worden de standaardinstellingen en hoe u kunt aanvragen quotum verhoogt.

Houd in gedachten als u deze quota ontwerp en de schaal van uw Batch-workloads. Bijvoorbeeld, als uw pool niet het doelaantal rekenknooppunten die u hebt opgegeven bereiken, u mogelijk hebt bereikt de core quotalimiet voor uw Batch-account.

U kunt in één Batch-account meerdere Batch-workloads uitvoeren of uw workloads verdelen over Batch-accounts in hetzelfde abonnement maar in verschillende Azure-regio's.

Als u van plan bent te productieworkloads in Batch uitvoeren, moet u wellicht een of meer van de quota boven de standaardwaarde verhogen. Als u een quotum te verhogen wilt, opent u een online [klant ondersteuningsaanvraag](#increase-a-quota) zonder kosten.

> [!NOTE]
> Een quotum is een tegoed limiet, geen garantie capaciteit. Als u grootschalige capaciteitsbehoeften hebt, neem contact op met de ondersteuning van Azure.
> 
> 

## <a name="resource-quotas"></a>Resourcequota
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="quotas-in-user-subscription-mode"></a>Quota's in de gebruikersmodus-abonnement

Als u een Batch-account maken met toepassingen toewijzing modus is ingesteld op een eerdere versie van de Batch-API gebruikt **gebruikerabonnement**, quota anders worden toegepast. In deze modus niet meer wordt aanbevolen, worden Batch VM's en andere bronnen gemaakt rechtstreeks in uw abonnement wanneer een groep is gemaakt. Het quotum van Azure Batch kernen geldt niet voor een account dat is gemaakt in deze modus. In plaats daarvan de quota's in uw abonnement voor de regionale compute kernen en andere resources zijn toegepast. Meer informatie over deze quota in [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).

## <a name="other-limits"></a>Andere limieten
| **Resource** | **Maximumaantal** |
| --- | --- |
| [Gelijktijdige taken](batch-parallel-node-tasks.md) per rekenknooppunt |4 x aantal kernen voor knooppunt |
| [Toepassingen](batch-application-packages.md) per Batch-account |20 |
| Toepassingspakketten per toepassing |40 |
| Grootte van de toepassing-pakket (elk) |Ongeveer 195GB<sup>1</sup> |
| Maximale startgrootte van taak | 32768 tekens<sup>2</sup> |
| Taak maximale levensduur | 7 dagen<sup>3</sup> |

<sup>1</sup> azure Storage-limiet voor maximale blob blokgrootte<br />
<sup>2</sup> bevat bronbestanden en omgevingsvariabelen<br />
<sup>3</sup> de maximale levensduur van een taak uit wanneer deze wordt toegevoegd aan de taak is wanneer deze is voltooid, is 7 dagen. Voltooide taken behouden voor onbepaalde tijd; gegevens voor taken zijn niet voltooid binnen de maximale levensduur is niet toegankelijk.


## <a name="view-batch-quotas"></a>Batch-quota's weergeven
Weergeven van uw Batch-account quota's in de [Azure-portal][portal].

1. Selecteer **Batch-accounts** in de portal, selecteer vervolgens het Batch-account dat u geïnteresseerd bent in.
2. Selecteer **quota** menu voor het Batch-account.
3. De quota die momenteel wordt toegepast op het Batch-account weergeven
   
    ![Quota voor batch-account][account_quotas]



## <a name="increase-a-quota"></a>Een quotum verhogen
Volg deze stappen om aan te vragen een quotum voor uw Batch-account of uw abonnement met verhogen de [Azure-portal][portal]. Het type van de verhoging van het quotum is afhankelijk van de modus van de toewijzing van toepassingen van uw Batch-account.

### <a name="increase-a-batch-cores-quota"></a>Een Batch kernen quotum verhogen 

1. Selecteer de **Help + ondersteuning** tegel op uw portal-dashboard of een vraagteken (**?**) in de rechterbovenhoek van de portal.
2. Selecteer **nieuw ondersteuningsverzoek** > **basisbeginselen**.
3. In **basisbeginselen**:
   
    a. **Type probleem** > **quotum**
   
    b. Selecteer uw abonnement.
   
    c. **Quotumtype** > **Batch**
   
    d. **Ondersteuningsplan** > **quotum support - opgenomen**
   
    Klik op **Volgende**.
4. In **probleem**:
   
    a. Selecteer een **ernst** volgens uw [bedrijfsimpact][support_sev].
   
    b. In **Details**, geef elke quota die u wilt wijzigen, de Batch-accountnaam en de nieuwe limiet.
   
    Klik op **Volgende**.
5. In **contactgegevens**:
   
    a. Selecteer een **voorkeur contactmethode**.
   
    b. Controleer en voer de vereiste contact op met details.
   
    Klik op **Maken** om het ondersteuningsverzoek in te dienen.

Nadat u uw ondersteuningsaanvraag hebt ingediend, ondersteuning van Azure contact met u op. Let op het voltooien van de aanvraag kan maximaal 2 werkdagen duren.


## <a name="related-topics"></a>Verwante onderwerpen
* [Een Azure Batch-account maken met de Azure-portal](batch-account-create-portal.md)
* [Overzicht van Azure Batch-functies](batch-api-basics.md)
* [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
