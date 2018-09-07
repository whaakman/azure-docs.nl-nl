---
title: Quota en limieten voor Azure Batch AI-service | Microsoft Docs
description: Meer informatie over Azure Batch AI standaardquota, limieten en beperkingen en vergroot het quotum aanvragen
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: e4b6d115aebfd96d127e0d72c0c99188bedd93e6
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057058"
---
# <a name="batch-ai-service-quotas-and-limits"></a>Batch AI-service-quota en limieten

Als met andere Azure-services, er gelden beperkingen voor bepaalde resources die zijn gekoppeld aan de Batch AI-service. In de Batch AI, deze limieten zijn standaardquota die worden toegepast op het abonnementsniveau voor elke regio waar de service is [beschikbaar](https://azure.microsoft.com/global-infrastructure/services/). Dit artikel worden de standaardwaarden, en hoe u kunt een quota aanvragen op verhoogt.

Houd er rekening mee als u deze quota ontwerp en de schaal van uw resources van Batch AI. Bijvoorbeeld, als uw cluster niet wordt bereikt het doelaantal knooppunten die u hebt opgegeven, klikt u vervolgens u mogelijk hebt een Batch AI kernen limiet bereikt voor uw abonnement.

Als u van plan bent om uit te voeren van productieworkloads in Batch AI, moet u mogelijk een of meer van de quota boven de standaardwaarde verhogen.

> [!NOTE]
> Een quotum is een tegoed is besteed, niet een capaciteit gegarandeerd. Als u grootschalige capaciteit nodig hebt, neem contact op met ondersteuning van Azure.
> 
> 

## <a name="resource-quotas"></a>Resourcequota

In de Batch AI is er een standaard-quotumlimiet voor zowel het aantal kernen en het aantal clusters toegestaan per regio in een abonnement.

| **Resource** | **Standaardlimiet** | **Maximumlimiet** |
| --- | --- | --- |
| Toegewezen kerngeheugens per regio | 10 - 24 uur per dag | N.v.t.<sup>1</sup> |
| Kernen per regio met lage prioriteit | 10 - 24 uur per dag | N.V.T.<sup>2</sup> |
| Clusters per regio | 20 | 200<sup>3</sup> |

> [!NOTE]
> Standaardlimieten variëren afhankelijk van het type abonnement die u hebt.

<sup>1</sup> het aantal toegewezen kerngeheugens per Batch AI-abonnement kan worden vergroot, maar het maximum aantal is niet opgegeven. Neem contact op met ondersteuning van Azure wilt discussiëren over meer opties.

<sup>2</sup> het aantal kernen per Batch AI-abonnement met lage prioriteit kan worden verhoogd, maar het maximum aantal is niet opgegeven. Neem contact op met ondersteuning van Azure wilt discussiëren over meer opties.

<sup>3</sup> Neem contact op met Azure-ondersteuning als u wilt een verhoging boven deze limiet.

## <a name="other-limits"></a>Andere limieten

Hieronder vindt u strikt beperkt die kunnen niet worden overschreden eenmaal is bereikt.

| **Resource** | **Maximumlimiet** |
| --- | --- |
| Maximum aantal werkruimten per resourcegroep | 800 |
| Maximale clustergrootte | 100 knooppunten |
| Maximumaantal GPU MPI verwerkt per knooppunt | 1-4 |
| Maximumaantal GPU werknemers per knooppunt | 1-4 |
| Maximale levensduur | 7 dagen<sup>1</sup> |
| Parameter Maximum servers per knooppunt | 1 |

<sup>1</sup> de maximale levensduur verwijst naar de tijd dat wordt een taak gestart die wordt uitgevoerd en wanneer deze is voltooid. Voltooide taken blijven voor onbepaalde tijd; gegevens voor taken is niet voltooid binnen de maximale levensduur is niet toegankelijk.

## <a name="view-batch-ai-quotas"></a>Batch AI-quotums weergeven

Weergeven van uw huidige quota van Batch AI-abonnement in de [Azure-portal][portal].

1. Klik in het linkerdeelvenster op **alle services**. Zoek vervolgens naar **Batch AI** en klikt u op de service te openen.
2. Klik op **gebruik + quota** in het menu van de Batch AI.
3. Selecteer uw abonnement om de quotumlimiet weer te geven.

## <a name="increase-a-batch-ai-cores-quota"></a>Verhoog het quotum voor een Batch AI-kerngeheugens

Volg deze stappen om aan te vragen van een quotum voor uw Batch AI-abonnement met verhogen de [Azure-portal][portal]. 

1. Klik in het linkerdeelvenster op **alle services**. Zoek vervolgens naar **Batch AI** en klikt u op de service te openen.
2. Klik op **nieuwe ondersteuningsaanvraag** in het menu van de Batch AI.
3. In **basisbeginselen**:
   
    a. **Type probleem** > **quotum**
   
    b. **Abonnement** > Selecteer uw abonnement.
   
    c. **Quotumtype** > **Batch AI**
   
    d. **Ondersteuningsplan** > Selecteer uw ondersteuningsplan.

    Klik op **Volgende**.
4. In **probleem**:
   
    a. Selecteer een **ernst** volgens uw [impact op bedrijf][support_sev].
   
    b. In **quotumgegevens**, geef de locatie, het quotumtype en het resourcetype. Geef de nieuwe limiet die u wilt aanvragen. Klik op **opslaan en doorgaan**.

    c. Optioneel: alle relevante bestanden uploaden met meer informatie over de reden voor verhoging.
   
    Klik op **Volgende**.
5. In **contactgegevens**:
   
    a. Selecteer een **voorkeursmethode voor contact op met**.
   
    b. Controleren en voer de vereiste gegevens van de contactpersoon.
   
    Klik op **Maken** om het ondersteuningsverzoek in te dienen.

Nadat u uw ondersteuningsaanvraag hebt ingediend, ondersteuning van Azure contact met u. De aanvraag te voltooien, kan maximaal 2 werkdagen duren.


## <a name="next-steps"></a>Volgende stappen

Bekijk de volgende artikelen voor aan de slag met Batch AI gebruiken nadat u bekend bent met de quotalimieten.

> [!div class="nextstepaction"]
> [Zelfstudie snel starten met batch AI](quickstart-tensorflow-training-cli.md)
> [Batch AI recepten](https://github.com/Azure/BatchAI/tree/master/recipes)
> [meer informatie over Batch AI-resources](resource-concepts.md)

[portal]: https://portal.azure.com
[support_sev]: http://aka.ms/supportseverity