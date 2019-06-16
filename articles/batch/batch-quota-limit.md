---
title: Quota en limieten - Azure Batch-service | Microsoft Docs
description: Meer informatie over Azure Batch standaardquota, limieten en beperkingen en vergroot het quotum aanvragen
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/28/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: de32ae16ea4d3c52b8017f35ae5af6009ab59205
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080906"
---
# <a name="batch-service-quotas-and-limits"></a>Quota en limieten voor Batch-service

Als met andere Azure-services, er gelden beperkingen voor bepaalde resources die zijn gekoppeld aan de Batch-service. Veel van deze beperkingen zijn standaardquota die worden toegepast door Azure op het abonnement of accountniveau. Dit artikel worden de standaardwaarden, en hoe u kunt een quota aanvragen op verhoogt.

Houd er rekening mee als u deze quota ontwerp en de schaal van uw Batch-workloads. Bijvoorbeeld, als de pool niet het doelaantal rekenknooppunten dat u hebt opgegeven bereikt, u mogelijk hebt bereikt de quotumlimiet voor uw core voor uw Batch-account.

U kunt in één Batch-account meerdere Batch-workloads uitvoeren of uw workloads verdelen over Batch-accounts in hetzelfde abonnement maar in verschillende Azure-regio's.

Als u van plan bent om uit te voeren van productieworkloads in Batch, moet u mogelijk een of meer van de quota boven de standaardwaarde verhogen. Als u een quotum te verhogen wilt, kunt u openen een online [ondersteuningsverzoek](#increase-a-quota) gratis.

## <a name="resource-quotas"></a>Resourcequota

Een quotum is een tegoed is besteed, niet een capaciteit gegarandeerd. Als u grootschalige capaciteit nodig hebt, neem contact op met ondersteuning van Azure.

Houd er rekening mee dat quota niet zijn gegarandeerd ook waarden. Quota kunnen variëren op basis van wijzigingen van de gebruikersaanvraag voor een een quotumwaarde te wijzigen of de Batch-service.

[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="cores-quotas-in-user-subscription-mode"></a>Kernquota's in de gebruikersabonnementmodus

Als u een Batch-account met de pooltoewijzingsmodus is ingesteld gemaakt op **gebruikersabonnement**, quota's anders worden toegepast. In deze modus worden Batch-VM's en andere resources gemaakt rechtstreeks in uw abonnement wanneer een groep wordt gemaakt. De Azure Batch kernquota's niet van toepassing op een account gemaakt in deze modus. In plaats daarvan de quota in uw abonnement voor regionale compute-resources en andere resources worden toegepast. Meer informatie over deze quota in [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).

## <a name="pool-size-limits"></a>De maximale grootte van toepassingen

De maximale grootte van toepassingen zijn ingesteld door de Batch-service. In tegenstelling tot [resourcequota](#resource-quotas), deze waarden kunnen niet worden gewijzigd. Alleen toepassingen met de communicatie tussen knooppunten en aangepaste installatiekopieën zijn beperkingen verschillen van de standaardquota.

| **Resource** | **Maximumaantal** |
| --- | --- |
| **Rekenknooppunten in [pool communicatie tussen knooppunten is ingeschakeld](batch-mpi.md)**  ||
| Groepstoewijzingsmodus is ingesteld op batch-service | 100 |
| Groepstoewijzingsmodus is ingesteld op batch-abonnement | 80 |
| **Rekenknooppunten in [groep van toepassingen die zijn gemaakt met aangepaste VM-installatiekopie](batch-custom-images.md)** <sup>1</sup> ||
| Toegewezen knooppunten | 2000 |
| Knooppunten met lage prioriteit | 1000 |

<sup>1</sup> voor pools die niet de communicatie tussen knooppunten is ingeschakeld.

## <a name="other-limits"></a>Andere limieten

Aanvullende beperkingen instellen door de Batch-service. In tegenstelling tot [resourcequota](#resource-quotas), deze waarden kunnen niet worden gewijzigd.

| **Resource** | **Maximumaantal** |
| --- | --- |
| [Gelijktijdige taken](batch-parallel-node-tasks.md) per knooppunt | 4 x aantal kernen van knooppunt |
| [Toepassingen](batch-application-packages.md) per Batch-account | 20 |
| Toepassingspakketten per toepassing | 40 |
| Toepassingspakketten per groep | 10 |
| De maximale levensduur van taken | 180 dagen<sup>1</sup> |

<sup>1</sup> de maximale levensduur van een taak, wanneer deze wordt toegevoegd aan de taak wanneer deze is voltooid, is 180 dagen. Voltooide taken blijven voor zeven dagen. gegevens voor taken is niet voltooid binnen de maximale levensduur zijn niet toegankelijk.

## <a name="view-batch-quotas"></a>Batch-quotums weergeven

Weergeven van uw Batch-accountquota in de [Azure-portal][portal].

1. Selecteer **Batch-accounts** in de portal, schakelt u vervolgens de Batch-account dat u geïnteresseerd bent in.
1. Selecteer **quota** in het menu van de Batch-account.
1. De quota die momenteel wordt toegepast op het Batch-account weergeven

    ![Batch-accountquota][account_quotas]

## <a name="increase-a-quota"></a>Een quotum verhogen

Volg deze stappen om aan te vragen van een quotum voor uw Batch-account of uw abonnement met verhogen de [Azure-portal][portal]. Het type van de verhoging van het quotum is afhankelijk van de pooltoewijzingsmodus van uw Batch-account. Om aan te vragen een quotaverhoging, moet u de VM-reeks die u graag zou willen Verhoog het quotum voor opnemen. Wanneer de verhoging is toegepast, wordt deze toegepast op alle reeksen van virtuele machines.

### <a name="increase-cores-quota-in-batch"></a>Vergroot het quotum voor kerngeheugens in Batch 

1. Selecteer de **Help en ondersteuning** tegel op uw portal-dashboard of het vraagteken ( **?** ) in de rechterbovenhoek van de portal.
1. Selecteer **nieuwe ondersteuningsaanvraag** > **basisbeginselen**.
1. In **basisbeginselen**:
   
    a. **Type probleem** > **limieten voor Service en -abonnement (quota)**
   
    b. Selecteer uw abonnement.
   
    c. **Quotumtype** > **Batch**
      
    Selecteer **Next**.
    
1. In **Details**:
      
    a. In **vindt u informatie**, geef de locatie, quotumtype en Batch-account.
    
    ![Verhoging van batch][quota_increase]

    Quotatypen zijn onder andere:

    * **Per Batch-account**  
        Waarden die specifiek zijn voor één Batch-account, met inbegrip van toegewezen en met lage prioriteit kernen en het aantal taken en groepen.
        
    * **Per regio**  
        De waarden die gelden voor alle Batch-accounts in een regio en bevat het nummer van de Batch-accounts per regio per abonnement.

    Met lage prioriteit quotum is één waarde voor alle VM-reeksen. Als u beperkte SKU's nodig hebt, moet u **kerngeheugens met lage prioriteit** en de VM-families om aan te vragen.

    b. Selecteer een **ernst** volgens uw [impact op bedrijf][support_sev].

    Selecteer **Next**.

1. In **contactgegevens**:
   
    a. Selecteer een **voorkeursmethode voor contact op met**.
   
    b. Controleren en voer de vereiste gegevens van de contactpersoon.
   
    Selecteer **maken** aan het ondersteuningsverzoek verzendt.

Nadat u uw ondersteuningsaanvraag hebt ingediend, ondersteuning van Azure contact met u. Quotumaanvragen kunnen worden uitgevoerd binnen een paar minuten of maximaal twee werkdagen.

## <a name="related-quotas-for-vm-pools"></a>Gerelateerde quota's voor VM-pools

Batch-pools in de configuratie van de virtuele Machine automatisch geïmplementeerd in een Azure-netwerk toewijzen extra Azure-netwerkbronnen. De volgende bronnen nodig zijn voor elke 50 pool-knooppunten in een virtueel netwerk:

* Een [netwerkbeveiligingsgroep](../virtual-network/security-overview.md#network-security-groups)
* Een [openbaar IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* Een [netwerktaakverdeler](../load-balancer/load-balancer-overview.md)

Deze resources worden toegewezen in het abonnement waarin het virtuele netwerk dat is opgegeven bij het maken van de Batch-pool. De beperkingen die voor deze resources gelden, worden bepaald door de [resourcequota](../azure-subscription-service-limits.md) van het abonnement. Als u van plan grote groep implementaties in een virtueel netwerk bent, controleert u de quota van het abonnement voor deze resources. Indien nodig, een verhoging in Azure portal door te selecteren **Help en ondersteuning**.


## <a name="related-topics"></a>Verwante onderwerpen
* [Azure Batch-account via Azure portal maken](batch-account-create-portal.md)
* [Overzicht van Azure Batch-functies](batch-api-basics.md)
* [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Limieten, quota's en beperkingen voor het Azure-abonnement en de Azure-service)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: https://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
[quota_increase]: ./media/batch-quota-limit/quota-increase.png
