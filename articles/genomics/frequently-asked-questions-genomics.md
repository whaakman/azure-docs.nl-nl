---
title: 'Microsoft Genomics: Veelgestelde vragen - Veelgestelde vragen over | Microsoft Docs'
titleSuffix: Azure
description: Antwoorden op algemene vragen klanten vragen over Microsoft Genomics.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: article
ms.date: 12/07/2017
ms.openlocfilehash: 804076fdc653622336ac3b99c15df0bc027510d9
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730141"
---
# <a name="microsoft-genomics-common-questions"></a>Microsoft Genomics: Veelgestelde vragen

In dit artikel geeft een lijst van de populairste query's die u mogelijk met betrekking tot Microsoft Genomics. Zie voor meer informatie over de service Microsoft Genomics [wat is Microsoft Genomics?](overview-what-is-genomics.md). Zie voor meer informatie over het oplossen van onze [Troubleshooting Guide](troubleshooting-guide-genomics.md). 


## <a name="what-is-the-sla-for-microsoft-genomics"></a>Wat is de SLA voor Microsoft Genomics?
We garanderen 99,9% van de time-service Microsoft Genomics is beschikbaar voor het ontvangen van de werkstroom API-aanvragen. Zie voor meer informatie, [SLA](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>Hoe wordt het gebruik van Microsoft Genomics weergegeven op mijn factuur?
Microsoft Genomics facturen op basis van het aantal gigabases dat per werkstroom is verwerkt. Zie voor meer informatie, [prijzen](https://azure.microsoft.com/pricing/details/genomics/).


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>Waar vind ik een lijst met alle mogelijke opdrachten en argumenten voor de `msgen` client?
U krijgt een volledige lijst van beschikbare opdrachten, argumenten en door te voeren `msgen help`. Als er geen verdere argumenten zijn opgegeven, wordt een lijst met beschikbare help secties, één voor elk van `submit`, `list`, `cancel`, en `status`. Als u de help voor een specifieke opdracht, typt u `msgen help command`, bijvoorbeeld `msgen help submit` geeft een lijst van alle van de opties voor het indienen.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>Wat zijn de meest gebruikte opdrachten voor het `msgen` client?
De meest gebruikte opdrachten zijn argumenten voor de `msgen` client bevatten: 

 |**Opdracht**          |  **Beschrijving van veld** |
 |:--------------------|:-------------         |
 |`list`               |Retourneert een lijst met taken die u hebt verzonden. Zie voor argumenten `msgen help list`.  |
 |`submit`             |Verzendt een verzoek van de werkstroom met de service. Zie voor argumenten `msgen help submit`.|
 |`status`             |Retourneert de status van de werkstroom die is opgegeven door `--workflow-id`. Zie ook `msgen help status`. |
 |`cancel`             |Verzendt een verzoek om te annuleren van de verwerking van de werkstroom die is opgegeven door `--workflow-id`. Zie ook `msgen help cancel`. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>Waar vind ik de waarde voor `--api-url-base`?
Ga naar Azure portal en open de pagina van uw Genomics-account. Onder de **Management** kop, kies **toegangssleutels**. U vindt er, zowel de API-URL en toegangssleutels van uw.

## <a name="where-do-i-get-the-value-for---access-key"></a>Waar vind ik de waarde voor `--access-key`?
Ga naar Azure portal en open de pagina van uw Genomics-account. Onder de **Management** kop, kies **toegangssleutels**. U vindt er, zowel de API-URL en toegangssleutels van uw.

## <a name="why-do-i-need-two-access-keys"></a>Waarom moet ik twee toegangssleutels?
U moet twee toegangssleutels in het geval u wilt bijwerken (sleutel opnieuw genereren) zonder dat gebruik van de service wordt onderbroken. U wilt bijvoorbeeld de eerste sleutel bijwerken. In dat geval kunt overstappen u alle nieuwe werkstromen naar de tweede sleutel. Klik, wacht u totdat de al actieve werkstromen met behulp van de eerste sleutel zijn voltooid. De sleutel alleen vervolgens bijwerken.

## <a name="do-you-save-my-storage-account-keys"></a>Slaat u de sleutels van mijn storage-account?
Sleutel van uw opslagaccount wordt gebruikt om te maken op korte termijn toegangstokens voor de Microsoft Genomics-service voor uw invoerbestanden lezen en schrijven van de uitvoerbestanden. De duur van het standaard-token is 48 uur. Duur van het token kan worden gewijzigd met de `-sas/--sas-duration` optie van de opdracht verzenden; de waarde is in uren.

## <a name="what-genome-references-can-i-use"></a>Welke genoom verwijst naar kan ik gebruiken?

Deze verwijzingen worden ondersteund:
 |Referentie              | Waarde van `-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (geen alt analyse) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>Hoe maak ik mijn opdrachtregelargumenten als een config-bestand? 

msgen begrijpt configuratiebestanden in de volgende indeling:
* Alle opties worden gegeven als sleutel-waardeparen met waarden van sleutels gescheiden door een dubbele punt.
Witruimte wordt genegeerd.
* Regels die beginnen met `#` worden genegeerd.
* Een opdrachtregelargument in de notatie voor lange kan worden geconverteerd naar een sleutel door de verwijdering van de toonaangevende streepjes en streepjes tussen woorden met onderstrepingstekens vervangen. Hier volgen enkele voorbeelden van de conversie:

 |Opdrachtregelargument            | Regel voor configuratie-bestand |
 |:-------------                   |:-------------                 |
 |`-u/--api-url-base https://url`  | *api_url_base:https://url*    |
 |`-k/--access-key KEY`            | *access_key:Key*              |      
 |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende bronnen om aan de slag met Microsoft Genomics:
- Aan de slag door te voeren van uw eerste werkstroom via de Microsoft Genomics-service. [Een werkstroom uitvoeren via de service Microsoft Genomics ](quickstart-run-genomics-workflow-portal.md)
- Indienen van uw eigen gegevens voor verwerking door de service Microsoft Genomics: [gekoppelde FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [meerdere FASTQ of BAM](quickstart-input-multiple.md) 

