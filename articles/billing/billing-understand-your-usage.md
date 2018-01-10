---
title: Het gebruik van uw Azure gedetailleerde begrijpen
description: Informatie over het lezen van en inzicht in de secties van gedetailleerde gebruik van uw CSV voor uw Azure-abonnement
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: tonguyen
ms.openlocfilehash: 999f24b5a5c1bd5d259b785f9107697d2928b4eb
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="understand-terms-on-your-microsoft-azure-detailed-usage-charges"></a>Termen in uw Microsoft Azure gedetailleerde gebruikskosten begrijpen 

Het gebruik van gedetailleerde kosten CSV-bestand bevat dagelijks en meter niveau gebruikskosten voor de huidige factureringsperiode. 

Als u het bestand met gedetailleerde informatie over het gebruik, Zie [het ophalen van uw Azure facturering facturen en dagelijks gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md).
Het is beschikbaar in een indeling met door komma's gescheiden waarden (.csv) dat u in een werkbladtoepassing openen kunt. Als er twee versies beschikbaar zijn, wordt versie 2 downloaden. Dat is de meest recente bestandsindeling.

Gebruikskosten zijn het totale aantal **maandelijkse** kosten voor een abonnement. De gebruikskosten rekening geen gehouden met eventuele tegoed of kortingen.

>[!VIDEO https://www.youtube.com/embed/p13S350M2Vk]

## <a name="detailed-terms-and-descriptions-of-your-detailed-usage-file"></a>Gedetailleerde voorwaarden en beschrijvingen van het bestand met gedetailleerde informatie over het gebruik

De volgende secties worden de belangrijke termen die wordt weergegeven in versie 2 van het bestand gedetailleerde informatie over het gebruik.

### <a name="statement"></a>Verklaring

Het bovenste gedeelte van het CSV-bestand voor gedetailleerde informatie over het gebruik ziet u de services die u hebt gebruikt tijdens de maand factureringsperiode. De volgende tabel bevat de voorwaarden en beschrijvingen in deze sectie wordt weergegeven.

| Termijn | Beschrijving |
| --- | --- |
|Factureringsperiode |De factureringsperiode wanneer de meters zijn gebruikt |
|De categorie van de meter |Geeft de service op het hoogste niveau voor het gebruik |
|De subcategorie van de meter |Definieert het type van de Azure-service die invloed kan zijn op de snelheid |
|De naam van de meter |Identificeert de maateenheid voor de meter wordt gebruikt |
|De regio van de meter |Geeft de locatie van het datacenter voor bepaalde services die worden berekend op basis van locatie datacenter |
|SKU |De unieke systeem-id voor elk Azure meter identificeert |
|Eenheid |De eenheid waarmee de kosten voor de service worden berekend. Bijvoorbeeld GB, uur, 10.000 s. |
|Verbruikt aantal |Het bedrag van de meter wordt gebruikt tijdens de factureringsperiode |
|Inbegrepen hoeveelheid |Het bedrag van de meter die is opgenomen gratis in de huidige factureringsperiode |
|Overschreden hoeveelheid |Bevat het verschil tussen de verbruikt en de hoeveelheid opgenomen. U wordt gefactureerd voor dit bedrag. Voor betalen per gebruik aanbiedingen waarbij geen aantal opgenomen met de aanbieding is dit totaal hetzelfde als de hoeveelheid verbruikt. |
|Binnen toezegging |Geeft de kosten van de meter dat wordt afgetrokken van uw toezeggingsbedrag die zijn gekoppeld aan uw aanbieding 6 of 12 maanden. Kosten van de meter worden in chronologische volgorde afgetrokken. |
|Valuta |De valuta die wordt gebruikt in uw huidige factureringsperiode |
|Overschrijding |Geeft de kosten van de meter die groter is dan uw toezeggingsbedrag die zijn gekoppeld aan uw aanbieding 6 of 12 maanden |
|Toezeggingstarief |Geeft het streven tarief op basis van het totale aantal toezeggingsbedrag die zijn gekoppeld aan uw aanbieding 6 of 12 maanden |
|Tarief |De frequentie waarmee die u in rekening per factureerbare eenheid gebracht bent |
|Waarde |Geeft het resultaat van de kolom overschrijding aantal door de snelheid kolom vermenigvuldigen. Als de hoeveelheid verbruikte niet groter zijn dan de hoeveelheid opgenomen, zijn er geen kosten in deze kolom. |

### <a name="daily-usage"></a>Dagelijkse gebruik

De sectie dagelijks gebruik van het CSV-bestand bevat gebruiksdetails die invloed hebben op de facturering tarieven. De volgende tabel bevat de voorwaarden en beschrijvingen in deze sectie wordt weergegeven.

| Termijn | Beschrijving |
| --- | --- |
|Gebruiksdatum |De datum waarop de meter is gebruikt |
|De categorie van de meter |Geeft de op het hoogste niveau service waarvoor dit gebruik behoort |
|Id van de meter |De gefactureerde meter-ID die wordt gebruikt voor de prijs van facturering gebruik |
|De subcategorie van de meter |Definieert het type Azure-service die invloed kan zijn op de snelheid |
|De naam van de meter |Identificeert de maateenheid voor de meter wordt gebruikt |
|De regio van de meter |Geeft de locatie van het datacenter voor bepaalde services die worden berekend op basis van locatie datacenter |
|Eenheid |De eenheid die de meter wordt in rekening gebracht in identificeert. Bijvoorbeeld GB, uur, 10.000 s. |
|Verbruikt aantal |Het bedrag van de meter die voor die dag is verbruikt |
|Resourcelocatie |Identificeert het datacenter waarop de meter wordt uitgevoerd |
|Verbruikte service |De Azure-platform-service die u gebruikt |
|Resourcegroep |De resourcegroep waarin de geïmplementeerde meter in wordt uitgevoerd. <br/><br/>Zie voor meer informatie [Overzicht van Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
|Exemplaar-id | De id voor de meter. <br/><br/> De id bevat de naam die u voor de meter opgeeft als deze is gemaakt. Het is de naam van de bron of de volledig gekwalificeerde Resource-ID. Zie voor meer informatie [Azure Resource Manager-API](https://docs.microsoft.com/rest/api/resources/resources). |
|Tags | Code die u aan de meter toewijst. Gebruik labels facturering groeperen van records.<br/><br/>U kunt bijvoorbeeld codes gebruiken om kosten te verdelen door de afdeling die gebruikmaakt van de meter. Services die ondersteuning bieden voor het importeerbereik tags zijn virtuele machines, opslag- en netwerkservices ingericht met behulp van de [Azure Resource Manager-API](https://docs.microsoft.com/rest/api/resources/resources). Zie voor meer informatie [ordenen van uw Azure-resources met labels](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Aanvullende informatie |Service-specifieke metagegevens. Bijvoorbeeld, een type installatiekopie voor een virtuele machine. |
|Service-informatie 1 |De naam van het project dat de service tot op uw abonnement behoort. |
|Service-informatie 2 |Verouderde veld waarmee optionele servicespecifieke metagegevens wordt vastgelegd |

## <a name="how-do-i-make-sure-that-the-charges-in-my-detailed-usage-file-are-correct"></a>Hoe maak ik ervoor dat de kosten in mijn bestand gedetailleerde informatie over het gebruik correct zijn?
Als er een kosten op uw bestand voor gedetailleerde informatie over het gebruik die u graag meer details op, Zie [inzicht in uw factuur voor Microsoft Azure.](./billing-understand-your-bill.md)

## <a name="external"></a>Hoe zit het met externe servicekosten?
Externe services (ook wel bekend als Marketplace orders) worden geleverd door onafhankelijke serviceleveranciers en worden afzonderlijk gefactureerd. De kosten worden niet weergegeven op de Azure-factuur. Zie voor meer informatie, [inzicht in uw Azure externe servicekosten](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?) ophalen van uw probleem snel worden opgelost.
