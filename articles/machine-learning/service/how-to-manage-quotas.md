---
title: Over het beheren en quota's voor Azure Machine Learning-service aanvragen
description: Deze gebruiksaanwijzing wordt uitgelegd van de verschillende quota voor resources voor Azure Machine Learning en het weergeven en meer quotum aanvragen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 9/24/2018
ms.openlocfilehash: 0ee0a29be6a313e09cd77051dc29bc0131b57a40
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997808"
---
# <a name="manage-and-request-quotas-for-azure-resources"></a>Beheren en quota's voor Azure-resources aanvragen

Als met andere Azure-services, er gelden beperkingen voor bepaalde resources die zijn gekoppeld aan de Azure Machine Learning-service. Deze limieten variëren van een bovengrens voor het aantal werkruimten kunt u limieten op de werkelijke onderliggende rekenresources die wordt gebruikt voor trainingen of inferentietaken uw modellen. In dit artikel geeft meer informatie over de vooraf geconfigureerde limieten voor verschillende Azure-resources voor uw abonnement en bevat ook handige koppelingen om aanvraag quotum verbeteringen voor elk type resource.

Houd er rekening mee als u deze quota ontwerp en de schaal van uw Azure ML-resources. Bijvoorbeeld, als uw cluster niet wordt bereikt het doelaantal knooppunten die u hebt opgegeven, klikt u vervolgens u mogelijk hebt een Batch AI kernen limiet bereikt voor uw abonnement. Als u van plan bent om uit te voeren van productieworkloads in Batch AI, moet u mogelijk een of meer van de quota boven de standaardwaarde verhogen. Als u de is besteed of wanneer het quotum boven de limiet standaard verhogen wilt, opent u een online ondersteuningsverzoek gratis. De limieten kunnen niet boven de maximumlimiet-waarde die wordt weergegeven in de volgende tabellen worden verhoogd. Als er geen kolom maximumlimiet is, klikt u vervolgens de resource geen instelbare limieten. 

## <a name="special-considerations"></a>Speciale overwegingen

+ Een quotum is een tegoed is besteed, niet een capaciteit gegarandeerd. Als u grootschalige capaciteit nodig hebt, neem dan contact op met ondersteuning van Azure.

+ Uw quotum wordt gedeeld door alle services, met uitzondering van Batch AI, in uw abonnement, Azure Machine Learning wordt een van beide. Zorg ervoor dat het quotagebruik berekenen van alle services bij het evalueren van uw behoeften aan capaciteit.

+ Standaardlimieten variëren per Type aanbieding, zoals gratis proefversie, betalen per gebruik en -serie, zoals Dv2, F, G, enzovoort.

## <a name="default-resource-quotas"></a>Standaard resourcequota

Hier volgt een overzicht van de quotumlimieten door verschillende resourcetypen in uw Azure-abonnement. 

> [!Important]
> Limieten kunnen worden gewijzigd. De meest recente kan altijd worden gevonden op het serviceniveau-quotum [document](https://docs.microsoft.com/azure/azure-subscription-service-limits/) voor alles van Azure.

### <a name="virtual-machines"></a>Virtuele machines 
Er geldt een limiet voor het aantal virtuele machines die u in uw services of in een zelfstandige wijze op een Azure-abonnement inrichten kunt. Deze limiet is op het regioniveau van de op het totaal aantal kerngeheugens, evenals op basis van de per-familie.

Het is belangrijk om te benadrukken dat cores van virtuele machine beschikt over regionale en regionale beperkingen per grootte reeks (Dv2, F, enz.) die afzonderlijk worden afgedwongen. Neem bijvoorbeeld een abonnement met een limiet van 30 VM-cores voor US - oost, een limiet van 30 cores voor de A-serie en een limiet van 30 cores voor de D-serie. Met dit abonnement zouden 30 A1-VM’s of 30 D1-VM’s kunnen worden geïmplementeerd, of een combinatie van deze twee typen die het totaal van 30 cores niet overschrijdt (bijvoorbeeld 10 A1-VM’s en 20 D1-VM’s).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../../includes/azure-subscription-limits-azure-resource-manager.md)]

Voor een meer gedetailleerde en actuele lijst van de quotalimieten, controleert u het artikel brede, door het Azure-quotum [hier](https://docs.microsoft.com/azure/azure-subscription-service-limits#subscription-limits-1).

### <a name="batch-ai-clusters"></a>Batch AI-clusters
In de Batch AI is er een standaard-quotumlimiet voor zowel het aantal kernen en het aantal clusters toegestaan per regio in een abonnement. Batch AI-quotum is gescheiden van de bovenstaande kerngeheugenquotum VM en de limieten core niet op dit moment worden gedeeld tussen de twee resourcetypen.

Beschikbare resources:
+ Toegewezen kerngeheugens per regio hebben een standaardlimiet van 10 tot 24.  Het aantal toegewezen kerngeheugens per Batch AI-abonnement kan worden verhoogd. Neem contact op met ondersteuning van Azure wilt discussiëren over meer opties.

+ Kernen per regio met lage prioriteit hebben een standaardlimiet van 10 tot 24.  Het aantal kernen per Batch AI-abonnement met lage prioriteit kan worden verhoogd. Neem contact op met ondersteuning van Azure wilt discussiëren over meer opties.

+ Clusters per regio hebben een standaardlimiet 20 en een limiet van 200. Neem contact op met ondersteuning van Azure als u wilt een verhoging boven deze limiet.

Voor een meer gedetailleerde en actuele lijst van de quotalimieten, controleert u het artikel brede, door het Azure-quotum [hier](https://docs.microsoft.com/azure/azure-subscription-service-limits#batch-ai-limits).

### <a name="container-instances"></a>Containerinstanties

Er is ook een limiet voor het aantal containerinstanties die u instellen kunt in een bepaalde periode (binnen het bereik van per uur) of in op uw volledige abonnement.

[!INCLUDE [container-instances-limits](../../../includes/container-instances-limits.md)]

Voor een meer gedetailleerde en actuele lijst van de quotalimieten, controleert u het artikel brede, door het Azure-quotum [hier](https://docs.microsoft.com/azure/azure-subscription-service-limits#container-instances-limits).

### <a name="storage"></a>Storage
Er geldt een limiet voor het aantal storage-accounts per regio ook in een bepaald abonnement. De standaardlimiet is 200 en omvat zowel Standard en Premium Storage-accounts. Als u meer dan 200 opslagaccounts in een bepaalde regio nodig hebt, moet u een aanvraag via [ondersteuning voor Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). Het Azure Storage-team controleert uw bedrijfsscenario en kan goedkeuren tot 250 opslagaccounts voor een bepaalde regio.


## <a name="find-your-quotas"></a>Uw quota's zoeken

Weergeven van uw quotum voor verschillende resources, zoals virtuele Machines, opslag, netwerk, is het eenvoudig via de Azure-portal.

1. Selecteer in het linkerdeelvenster **alle services** en selecteer vervolgens **abonnementen** onder de categorie Algemeen.

1. Selecteer het abonnement waarvan quota die u zoekt in de lijst met abonnementen.

   **Er is een voorbehoud**, specifiek voor het weergeven van de Batch AI-quotum. Zoals eerder vermeld, staat quotum los van het quotum voor compute voor uw abonnement. 
   Voor Batch AI, na het selecteren van **alle services**, zoeken naar Batch AI en open de service.

1. Onder **instellingen**, selecteer **gebruik + quota** om uw huidige quotalimieten en het gebruik weer te geven.

1. Selecteer uw abonnement om de quotumlimiet weer te geven. Vergeet niet om te filteren op de service en de regio waarin u geïnteresseerd bent.


## <a name="request-quota-increases"></a>Quotaverhogingen aanvraag

Als u wilt verhogen de is besteed of wanneer het quotum boven de standaardlimiet [opent u een ondersteuningsaanvraag online klant](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/) gratis. 

De limieten kunnen niet worden verhoogd boven de maximumlimiet-waarde die wordt weergegeven in de tabellen. Als er geen maximale limiet, hebben niet de bron instelbare limieten. [Dit](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors) artikel bevat informatie over het proces voor het vergroten van quota in meer detail.

Bij het aanvragen van een quotaverhoging, moet u de service u vraagt om te verhogen van het quotum tegen die services zoals AzureML quotum, Batch AI-quotum of quotum voor opslag kan worden te selecteren. 

> [!NOTE]
> [Gratis proefabonnementen](https://azure.microsoft.com/offers/ms-azr-0044p) komen niet in aanmerking voor is besteed of wanneer het quotum toeneemt. Als u hebt een [gratis proefabonnement](https://azure.microsoft.com/offers/ms-azr-0044p), kunt u upgraden naar een [betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) abonnement. Zie voor meer informatie, [Azure-proefabonnement upgraden naar betalen per gebruik](../../billing/billing-upgrade-azure-subscription.md) en [gratis proefabonnement Veelgestelde vragen over](https://azure.microsoft.com/free/free-account-faq).
