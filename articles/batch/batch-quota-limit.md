---
title: Quota en limieten voor Azure Batch service | Microsoft Docs
description: Meer informatie over Azure Batch standaardquota, limieten en beperkingen en verhoogt het quotum aanvragen
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3f69ed8d3a985afe07e648e7512a88b25278ced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="batch-service-quotas-and-limits"></a>Quota en limieten voor Batch-service

Zoals met andere Azure-services, moet u er limieten op bepaalde resources die zijn gekoppeld aan de Batch-service zijn. Veel van deze limieten zijn standaardquota door Azure wordt toegepast op het niveau van de account of het abonnement. In dit artikel worden de standaardinstellingen en hoe u kunt aanvragen quotum verhoogt.

Houd rekening met deze quota bij het ontwerpen en schalen van de Batch-workloads. Bijvoorbeeld, als uw pool is niet het is het doelaantal van rekenknooppunten die u hebt opgegeven bereikt, u mogelijk hebt bereikt de quotalimiet core voor uw Batch-account of een regionale VM kernen quotum voor uw abonnement.

U kunt in één Batch-account meerdere Batch-workloads uitvoeren of uw workloads verdelen over Batch-accounts in hetzelfde abonnement maar in verschillende Azure-regio's.

Als u van plan bent te productieworkloads in Batch uitvoeren, moet u wellicht een of meer van de quota boven de standaardwaarde verhogen. Als u een quotum te verhogen wilt, opent u een online [klant ondersteuningsaanvraag](#increase-a-quota) zonder kosten.

> [!NOTE]
> Een quotum is een tegoed limiet, geen garantie capaciteit. Als u grootschalige capaciteitsbehoeften hebt, neem contact op met de ondersteuning van Azure.
> 
> 

## <a name="resource-quotas"></a>Resourcequota
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a>Quota's in de gebruikersmodus-abonnement

Voor een Batch-account met de groep toewijzing modus is ingesteld op **gebruikerabonnement**, Batch-VM's en andere resources, zoals de storage-accounts, rechtstreeks in uw abonnement worden gemaakt wanneer een groep is gemaakt. Het quotum van Azure Batch kernen geldt niet voor een account dat is gemaakt in deze modus. In plaats daarvan de quota's in uw abonnement voor de regionale compute kernen en andere resources zijn toegepast. Meer informatie over deze quota in [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).

Houd rekening met dat de volgende Batch-resources (in aanvulling op compute kernen) zijn vereist voor elke 40 virtuele Linux-machines of 20 VM's van Windows bij het plannen van Resourcegebruik voor een account dat is gemaakt in de gebruikersmodus abonnement:

| Resource | Quota | Provider |
| --- | ---| --- |
| Een opslagaccount | Opslagaccounts | Microsoft.Storage |
| Een openbaar IP-adres | Openbare IP-adressen | Microsoft.Network | 
| Een virtueel netwerk | Virtuele netwerken | Microsoft.Network | 
| Een netwerkbeveiligingsgroep toevoegen | Netwerkbeveiligingsgroepen | Microsoft.Network | 
| Een virtuele-machineschaalset | Schaalsets voor virtuele machines | Microsoft.Compute | 
| Één load balancer | Taakverdelers | Microsoft.Network | 

Het quotum voor kernen op een regionaal niveau of per VM-familie moet worden ingesteld volgens de VM-grootte die vereist zijn voor uw Batch-pool of groepen:

| Quota | Provider |
| --- | ---- |
| Totaal aantal regionale kernen | Microsoft.Compute |
| … Familie kernen | Microsoft.Compute |



## <a name="other-limits"></a>Andere limieten
| **Resource** | **Maximumaantal** |
| --- | --- |
| [Gelijktijdige taken](batch-parallel-node-tasks.md) per rekenknooppunt |4 x aantal kernen voor knooppunt |
| [Toepassingen](batch-application-packages.md) per Batch-account |20 |
| Toepassingspakketten per toepassing |40 |
| Grootte van de toepassing-pakket (elk) |Ongeveer 195GB<sup>1</sup> |
| Maximale startgrootte van taak | 32768 tekens<sup>2</sup> |

<sup>1</sup> azure Storage-limiet voor maximale blob blokgrootte<br />
<sup>2</sup> bevat bronbestanden en omgevingsvariabelen

## <a name="view-batch-quotas"></a>Batch-quota's weergeven
Weergeven van uw Batch-account quota's in de [Azure-portal][portal].

1. Selecteer **Batch-accounts** in de portal, selecteer vervolgens het Batch-account dat u geïnteresseerd bent in.
2. Selecteer **eigenschappen** op het Batch-account menu blade.
3. De eigenschappenblade geeft de **quota** momenteel wordt toegepast op het Batch-account
   
    ![Quota voor batch-account][account_quotas]

Weergeven voor een Batch-account is gemaakt in de gebruikersmodus abonnement, de verwante abonnement quota in de Azure Portal.

1. Selecteer **abonnementen**, en selecteer het abonnement dat u voor het Batch-account gebruikt.

2. Op de **abonnement** blade Selecteer **gebruik + quota**.



## <a name="increase-a-quota"></a>Een quotum verhogen
Volg deze stappen om aan te vragen een quotum voor uw Batch-account of uw abonnement met verhogen de [Azure-portal][portal]. Het type van de verhoging van het quotum is afhankelijk van de modus van de toewijzing van toepassingen van uw Batch-account.

### <a name="increase-a-batch-cores-quota"></a>Een Batch kernen quotum verhogen 

Als uw Batch-account is gemaakt in **Batch-service** modus, voert u deze stappen om aan te vragen van een Batch kernen quotum verhogen:

1. Selecteer de **Help + ondersteuning** tegel op uw portal-dashboard of een vraagteken (**?**) in de rechterbovenhoek van de portal.
2. Selecteer **nieuw ondersteuningsverzoek** > **basisbeginselen**.
3. Op de **basisbeginselen** blade:
   
    a. **Type probleem** > **quotum**
   
    b. Selecteer uw abonnement.
   
    c. **Quotumtype** > **Batch**
   
    d. **Ondersteuningsplan** > **quotum support - opgenomen**
   
    Klik op **Volgende**.
4. Op de **probleem** blade:
   
    a. Selecteer een **ernst** volgens uw [bedrijfsimpact][support_sev].
   
    b. In **Details**, geef elke quota die u wilt wijzigen, de Batch-accountnaam en de nieuwe limiet.
   
    Klik op **Volgende**.
5. Op de **contactgegevens** blade:
   
    a. Selecteer een **voorkeur contactmethode**.
   
    b. Controleer en voer de vereiste contact op met details.
   
    Klik op **Maken** om het ondersteuningsverzoek in te dienen.

Nadat u uw ondersteuningsaanvraag hebt ingediend, ondersteuning van Azure contact met u op. Let op het voltooien van de aanvraag kan maximaal 2 werkdagen duren.

### <a name="increase-a-subscription-cores-quota"></a>Een abonnement kernen quotum verhogen

Als uw Batch-account is gemaakt in **gebruikerabonnement** modus en u moet extra regionale of VM-familie kernen, de aanvraag een quotum verhogen in uw abonnement. Zie voor stappen [kerngeheugenquotum voor Resource Manager verhogen aanvragen](../azure-supportability/resource-manager-core-quotas-request.md).



## <a name="related-topics"></a>Verwante onderwerpen
* [Een Azure Batch-account maken met de Azure-portal](batch-account-create-portal.md)
* [Overzicht van Azure Batch-functies](batch-api-basics.md)
* [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
