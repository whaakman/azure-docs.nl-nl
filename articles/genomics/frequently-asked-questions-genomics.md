---
title: 'Microsoft Genomics: Veelgestelde vragen | Microsoft Docs'
titleSuffix: Azure
description: Antwoorden op algemene vragen klanten vragen over Microsoft Genomics.
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 12/07/2017
ms.openlocfilehash: 2077eeb5177b07c458476ae900f81b72e35f0dc3
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="microsoft-genomics-common-questions"></a>Microsoft Genomics: Veelgestelde vragen

Dit artikel worden de bovenste query's die moet u wellicht hebben betrekking op Microsoft Genomics. Zie voor meer informatie over de service Microsoft Genomics [wat is Microsoft Genomics?](overview-what-is-genomics.md) 


## <a name="what-is-the-sla-for-microsoft-genomics"></a>Wat is de SLA voor Microsoft Genomics?
Wordt gegarandeerd dat 99,9% van de time-service voor Microsoft Genomics beschikbaar zijn voor de workflow-API-aanvragen worden ontvangen. Zie voor meer informatie [SLA](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>Hoe wordt het gebruik van Microsoft Genomics weergegeven op mijn factuur?
Microsoft Genomics facturen op basis van het aantal gigabases per werkstroom verwerkt. Zie voor meer informatie [prijzen](https://azure.microsoft.com/pricing/details/genomics/).


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>Waar vind ik een lijst met alle mogelijke opdrachten en argumenten voor de `msgen` client?
U krijgt een volledige lijst met beschikbare opdrachten, argumenten en door te voeren `msgen help`. Als er geen verdere argumenten zijn opgegeven, wordt er wordt een lijst met beschikbare help weergegeven secties, één voor elk van `submit`, `list`, `cancel`, en `status`. Als u de help voor een specifieke opdracht, typt u `msgen help command`, bijvoorbeeld `msgen help submit` geeft een lijst van alle opties voor het indienen.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>Wat zijn de meest gebruikte opdrachten voor de `msgen` client?
De meest gebruikte opdrachten zijn argumenten voor de `msgen` client bevatten: 

 |**Opdracht**          |  **Beschrijving van veld** |
 |:--------------------|:-------------         |
 |`list`               |Retourneert een lijst met taken die u hebt verzonden. Zie voor argumenten `msgen help list`.  |
 |`submit`             |Verzendt een aanvraag voor de werkstroom naar de service. Zie voor argumenten `msgen help submit`.|
 |`status`             |De status van de werkstroom die is opgegeven door `--workflow-id`. Zie ook `msgen help status`. |
 |`cancel`             |Verzendt een aanvraag te annuleren verwerking van de werkstroom die is opgegeven door `--workflow-id`. Zie ook `msgen help cancel`. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>Waar vind ik de waarde voor `--api-url-base`?
Ga naar Azure-portal en open uw accountpagina genomica. Onder de **Management** kopje kiezen **toegangssleutels**. U zowel de API-URL en toegangssleutels van uw vinden.

## <a name="where-do-i-get-the-value-for---access-key"></a>Waar vind ik de waarde voor `--access-key`?
Ga naar Azure-portal en open uw accountpagina genomica. Onder de **Management** kopje kiezen **toegangssleutels**. U zowel de API-URL en toegangssleutels van uw vinden.

## <a name="why-do-i-need-two-access-keys"></a>Waarom moet ik twee toegangssleutels?
U moet twee toegangssleutels voor het geval u wilt bijwerken (opnieuw genereren) zonder dat gebruik van de service wordt onderbroken. U wilt bijvoorbeeld de eerste sleutel bijwerken. In dat geval overschakelen u alle nieuwe werkstromen op de tweede sleutel. Vervolgens wacht u totdat de al actieve werkstromen met behulp van de eerste sleutel zijn voltooid. De sleutel alleen dan bijwerken.

## <a name="do-you-save-my-storage-account-keys"></a>Slaat u Mijn opslagsleutels voor account?
Sleutel van uw opslagaccount wordt gebruikt voor de korte termijn toegangstokens voor de service Microsoft Genomics uw invoerbestanden lezen en schrijven van de uitvoerbestanden maken. De duur van het standaard-token is 48 uur. Duur van het token kan worden gewijzigd met de `-sas/--sas-duration` optie van de opdracht indienen; de waarde is in uren.

## <a name="what-genome-references-can-i-use"></a>Welke genetisch materiaal verwijst naar kan ik gebruiken?

Deze referenties worden ondersteund:
 |Naslaginformatie              | Waarde van`-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (geen alt analyse) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>Hoe maak ik mijn opdrachtregelargumenten als een configuratiebestand 

msgen begrijpt configuratiebestanden in de volgende indeling:
* Alle opties worden opgegeven als sleutel-waardeparen met waarden van sleutels wordt gescheiden door een dubbele punt.
Witruimte wordt genegeerd.
* Regels die beginnen met `#` worden genegeerd.
* Een opdrachtregelargument in lange notatie kan worden geconverteerd naar een sleutel door het verwijderen van oude de toonaangevende streepjes en streepjes tussen woorden met onderstrepingstekens vervangen. Hier volgen enkele voorbeelden conversie:

 |Opdrachtregelargument            | Regel voor configuratie-bestand |
 |:-------------                   |:-------------                 |
 |`-u/--api-url-base https://url`  | *api_url_base:https://URL*    |
 |`-k/--access-key KEY`            | *access_key:Key*              |      
 |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende bronnen voor aan de slag met Microsoft Genomics:
- Aan de slag door het uitvoeren van uw eerste werkstroom via de service Microsoft Genomics. [Een werkstroom uitvoeren via de service Microsoft Genomics](quickstart-run-genomics-workflow-portal.md)
- Verzenden van uw eigen gegevens voor verwerking door de service Microsoft Genomics: [gekoppeld FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [meerdere FASTQ of BAM](quickstart-input-multiple.md) 

