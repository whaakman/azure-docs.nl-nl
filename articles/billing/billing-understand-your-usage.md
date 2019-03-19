---
title: Uw gedetailleerde gebruik van Azure begrijpen | Microsoft Docs
description: Meer informatie over het lezen en begrijpen van de secties van uw gedetailleerde gebruik CSV voor uw Azure-abonnement
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: banders
ms.openlocfilehash: a143fc6d9dbd78ae365f943a00ac9f8492d5e51c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57897143"
---
# <a name="understand-terms-on-your-microsoft-azure-detailed-usage-charges"></a>Meer informatie over de Microsoft Azure gedetailleerde gebruikskosten 

Het gedetailleerde gebruik kosten CSV-bestand bevat dagelijkse en meter niveau gebruikskosten voor de huidige factureringsperiode. 

Als u uw gedetailleerde gebruik-bestand, raadpleegt u [over het verkrijgen van uw Azure-factuur en de dagelijkse gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md).
Het is beschikbaar in een indeling met door komma's gescheiden waarden (.csv) die u in een werkbladtoepassing openen kunt. Als er twee versies beschikbaar zijn, moet u versie 2 downloaden. Dat is de meest recente bestandsindeling.

Gebruikskosten zijn de totale **maandelijkse** kosten op een abonnement. De kosten voor het gebruik rekening geen gehouden met eventuele tegoeden of kortingen.

>[!VIDEO https://www.youtube.com/embed/p13S350M2Vk]

## <a name="detailed-terms-and-descriptions-of-your-detailed-usage-file"></a>Gedetailleerde voorwaarden en beschrijvingen van het bestand met gedetailleerde informatie over gebruik

De volgende secties beschrijven de belangrijke termen die in versie 2 van het bestand gedetailleerd gebruik worden weergegeven.

### <a name="statement"></a>Verklaring

Het bovenste gedeelte van het gedetailleerde gebruik CSV-bestand ziet u de services die u hebt gebruikt tijdens de factureringsperiode van de maand. De volgende tabel bevat de voorwaarden en -beschrijvingen weergegeven in deze sectie.

| Termijn | Description |
| --- | --- |
|Factureringsperiode |De factureringsperiode wanneer de meters zijn gebruikt |
|De categorie van de meter |Identificeert de service op het hoogste niveau voor het gebruik |
|De subcategorie van de meter |Bepaalt het type Azure-service die invloed kan zijn op de snelheid |
|De naam van de meter |Identificeert de maateenheid voor de meter wordt gebruikt |
|De regio van de meter |Geeft de locatie van de datacenterlocatie voor bepaalde services waarbij de prijs gebaseerd op de datacenterlocatie |
|SKU |De unieke systeem-id voor elke Azure-meter |
|Eenheid |De eenheid waarmee de kosten voor de service worden berekend. Bijvoorbeeld GB, uren, 10.000 s. |
|Verbruikt aantal |Het bedrag van de meter gebruikt tijdens de factureringsperiode |
|Inbegrepen hoeveelheid |Het bedrag van de meter die is opgenomen gratis inbegrepen in uw huidige factureringsperiode. |
|Overschreden hoeveelheid |Bevat het verschil tussen de verbruikte hoeveelheid en het aantal opgenomen. U wordt gefactureerd voor dit bedrag. Bij betalen per gebruik-aanbiedingen waarbij er geen hoeveelheid opgenomen in de aanbieding is dit totaal hetzelfde als de verbruikte hoeveelheid. |
|Binnen toezegging |Geeft de meter-kosten die worden afgetrokken van de toezeggingshoeveelheid die is gekoppeld aan uw aanbieding 6 of 12 maanden. Meter-kosten worden afgetrokken in chronologische volgorde. |
|Valuta |De valuta die wordt gebruikt in uw huidige factureringsperiode. |
|Overschrijding |De kosten voor meter die groter zijn dan de toezeggingshoeveelheid die is gekoppeld aan uw aanbieding 6 of 12 maanden |
|Toezeggingstarief |Geeft het toezeggingstarief op basis van de totale toezeggingshoeveelheid die is gekoppeld aan uw aanbieding 6 of 12 maanden |
|Tarief |Het tarief dat u kosten in per factureerbare eenheid gebracht rekening |
|Value |Toont het resultaat van het vermenigvuldigen van de hoeveelheid overschrijding kolom met de kolom tarief. Als de verbruikte hoeveelheid niet hoger is dan het aantal opgenomen, zijn er geen kosten in deze kolom. |

### <a name="daily-usage"></a>Dagelijks gebruik

De sectie dagelijks gebruik van het CSV-bestand bevat informatie over het gebruik die invloed hebben op de facturering-tarieven. De volgende tabel bevat de voorwaarden en -beschrijvingen weergegeven in deze sectie.

| Termijn | Description |
| --- | --- |
|Gebruiksdatum |De datum waarop de meter is gebruikt |
|De categorie van de meter |Geeft de op het hoogste niveau service waartoe dit gebruik behoort |
|Id van de meter |De gefactureerde meter-ID die wordt gebruikt voor prijsfacturering wordt gehanteerd |
|De subcategorie van de meter |Definieert het type Azure-service die invloed kan zijn op de snelheid |
|De naam van de meter |Identificeert de maateenheid voor de meter wordt gebruikt |
|De regio van de meter |Geeft de locatie van de datacenterlocatie voor bepaalde services waarbij de prijs gebaseerd op de datacenterlocatie |
|Eenheid |De eenheid waarmee de meter wordt in rekening gebracht in. Bijvoorbeeld GB, uren, 10.000 s. |
|Verbruikt aantal |Het bedrag van de meter die is verbruikt voor die dag |
|Resourcelocatie |Identificeert het datacenter waar de meter wordt uitgevoerd |
|Verbruikte service |De Azure-platform-service die u gebruikt |
|Resourcegroep |De resourcegroep waarin de geïmplementeerde meter wordt uitgevoerd in. <br/><br/>Zie voor meer informatie [Overzicht van Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
|Exemplaar-id | De id voor de meter. <br/><br/> De id bevat de naam die u voor de meter opgeeft wanneer deze is gemaakt. Het is de naam van de resource of de volledig gekwalificeerde Resource-ID. Zie voor meer informatie, [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources). |
|Tags | Code die u aan de meter toewijst. Gebruik tags facturering om records te groeperen.<br/><br/>U kunt bijvoorbeeld tags gebruiken om kosten te verdelen van de afdeling die gebruikmaakt van de meter. Services die ondersteuning bieden voor uitgeven van tags zijn virtuele machines, opslag- en netwerkservices die zijn ingericht met behulp van de [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources). Zie voor meer informatie, [ordenen van uw Azure-resources met tags](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Aanvullende informatie |Servicespecifieke metagegevens. Bijvoorbeeld, een type installatiekopie voor een virtuele machine. |
|Service-informatie 1 |De naam van het project die op uw abonnement deel uitmaakt van de service |
|Service-informatie 2 |Ouder veld met optionele servicespecifieke metagegevens |

## <a name="how-do-i-make-sure-that-the-charges-in-my-detailed-usage-file-are-correct"></a>Hoe ik ervoor zorgen dat de kosten in rekening gebracht in mijn bestand gedetailleerd gebruik correct zijn?
Als er een kosten in rekening gebracht op uw gedetailleerde gebruik-bestand dat u wilt meer informatie over, Zie [meer informatie over uw factuur voor Microsoft Azure.](./billing-understand-your-bill.md)

## <a name="external"></a>Hoe zit het met externe servicekosten?
Externe services (ook wel bekend als een Marketplace-bestellingen) worden geleverd door een onafhankelijke serviceleveranciers en worden afzonderlijk gefactureerd. De kosten worden niet weergegeven op de Azure-factuur. Zie voor meer informatie, [meer informatie over uw Azure externe servicekosten](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).
