---
title: Aantekeningen toevoegen aan gegevensbronnen in Azure Data Catalog
description: Aantekeningen toevoegen aan gegevensassets in Azure Data Catalog, met inbegrip van beschrijvende namen, tags, beschrijvingen en experts markeren artikel met instructies.
services: data-catalog
author: steelanddata
ms.author: maroche
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: a6751a2cbb2a12d0d0ffe1eb0707ad4e8bc45ed8
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053551"
---
# <a name="how-to-annotate-data-sources"></a>Aantekeningen toevoegen aan gegevensbronnen
## <a name="introduction"></a>Inleiding
**Microsoft Azure Data Catalog** is een volledig beheerde cloudservice die als registratiesysteem en detectiesysteem voor zakelijke gegevensbronnen fungeert. Data Catalog is met andere woorden, alles over helpt mensen detecteren, begrijpen en met gegevensbronnen en helpt organisaties kunt u meer waarde halen uit hun bestaande gegevens. Als een gegevensbron is geregistreerd met Data Catalog, de metagegevens wordt gekopieerd en door de service geïndexeerd, maar het verhaal er niet beëindigen. Data Catalog kan gebruikers hun eigen beschrijvende metagegevens – zoals beschrijvingen en tags: ter aanvulling van de metagegevens die zijn geëxtraheerd uit de gegevensbron, en maakt de gegevensbron voor meer mensen inzichtelijk op te geven.

## <a name="annotation-and-crowdsourcing"></a>Aantekening en crowdsourcing
Iedereen heeft een advies. En dit is een goede prestaties.
Data Catalog herkent dat verschillende gebruikers verschillende perspectieven hebben voor zakelijke gegevensbronnen, en dat elk van deze perspectieven waardevol kan zijn. Houd rekening met het volgende scenario:

* De systeembeheerder weet de serviceovereenkomst voor de servers of services die als host fungeren voor de gegevensbron.
* De databasebeheerder weet de back-upschema voor elke database en de windows toegestane ETL-verwerking.
* Eigenaar van het systeem kent het proces voor gebruikers om aan te vragen van toegang tot de gegevensbron.
* De data steward weet hoe de elementen en kenmerken in de gegevensbron aan de enterprise-gegevensmodel toegewezen.
* De analist weet hoe de gegevens wordt gebruikt in de context van de bedrijfsprocessen die hij ondersteunt.

Elk van deze perspectieven is enorm waardevol en Data Catalog maakt gebruik van een crowdsourcing-methode voor metagegevens waarmee elke service worden vastgelegd en gebruikt voor een compleet beeld van de geregistreerde gegevensbronnen. Met behulp van de Data Catalog-portal, kan elke gebruiker toevoegen en bewerken van zijn of haar eigen aantekeningen, terwijl u aantekeningen die door andere gebruikers weergeven.

## <a name="different-types-of-annotations"></a>Verschillende typen aantekeningen
Data Catalog ondersteunt de volgende typen aantekeningen:

| Aantekening | Opmerkingen |
| --- | --- |
| Beschrijvende naam |Beschrijvende namen kunnen worden opgegeven op het niveau van het asset gegevens, zodat de gegevensassets gemakkelijker te begrijpen. Beschrijvende namen zijn handig als de naam van het onderliggende object cryptisch, verkorte of anderszins niet zinvol is voor gebruikers is. |
| Beschrijving |Beschrijvingen kunnen worden opgegeven tijdens de gegevensasset en het kenmerk / kolomniveaus. Beschrijvingen van aantekeningen in vrije korte tekst waarin wordt beschreven van de gebruiker zijn perspectief op de gegevensasset of het gebruik ervan. |
| Tags (gebruiker tags) |Labels kunnen worden opgegeven tijdens de gegevensasset en het kenmerk / kolomniveaus. Gebruiker-tags zijn aangepaste labels die kunnen worden gebruikt voor het categoriseren van gegevensassets of kenmerken. |
| Tags (woordenlijst tags) |Labels kunnen worden opgegeven tijdens de gegevensasset en het kenmerk / kolomniveaus. Verklarende woordenlijst tags zijn centraal gedefinieerd Woordenlijsttermen die kunnen worden gebruikt voor het categoriseren van gegevensassets of met behulp van een algemene zakelijke taxonomie kenmerken. Zie voor meer informatie [De zakelijke woordenlijst instellen voor Governed Tagging](data-catalog-how-to-business-glossary.md) |
| Experts |Experts kunnen worden opgegeven op het niveau van de gegevens asset. Experts kunnen geven aan welke gebruikers of groepen met deskundige perspectieven van de gegevens en fungeren als contactpunten voor gebruikers die de geregistreerde gegevensbronnen detecteren en vragen hebt die niet worden beantwoord door de bestaande aantekeningen. |
| Toegang aanvragen |Aanvraag voor toegang tot informatie kan worden opgegeven op het niveau van de gegevens asset. Deze informatie is voor gebruikers die een gegevensbron die ze hebt nog geen machtigingen om toegang te detecteren. Gebruikers kunnen Voer het e-mailadres van de gebruiker of groep die toegang, de URL van het proces of het hulpprogramma die gebruikers nodig hebben verleent voor toegang, of het proces zelf als tekst kunnen invoeren. |
| Documentatie |Documentatie kan worden opgegeven op het niveau van de gegevens asset. Asset-documentatie is RTF-informatie die kan bevatten koppelingen en afbeeldingen, en die informatie niet overgedragen door middel van beschrijvingen en tags kunt bieden. |

## <a name="annotating-multiple-assets"></a>Aantekeningen maken bij meerdere assets
Bij het selecteren van meerdere gegevensassets in de portal voor Data Catalog, kunnen gebruikers aantekeningen toevoegen aan alle geselecteerde assets in één bewerking. Aantekeningen wordt toegepast op alle geselecteerde assets, zodat het eenvoudig om te selecteren en bieden een consistente beschrijving en sets met tags en experts voor gerelateerde gegevensassets.

> [!NOTE]
> -Tags en experts kunnen ook worden opgegeven bij het registreren gegevensassets met behulp van de gegevens van de Data Catalog source-hulpprogramma voor registratie.
>
>

Bij selecteren van meerdere tabellen en weergaven, alleen kolommen dat alle geselecteerde assets gemeen hebben gegevens wordt weergegeven in de Data Catalog-portal. Hiermee kunnen gebruikers voor labels en beschrijvingen voor alle kolommen met dezelfde naam voor alle geselecteerde assets.

## <a name="annotations-and-discovery"></a>Aantekeningen en detectie
Net zoals de metagegevens die zijn geëxtraheerd uit de gegevensbron tijdens de registratie wordt toegevoegd aan de search-index van Data Catalog, worden de gebruiker opgegeven metagegevens worden ook geïndexeerd. Dit betekent dat niet alleen aantekeningen maken het gemakkelijker voor gebruikers om te begrijpen van de gegevens die ze detecteren, aantekeningen ook eenvoudiger voor gebruikers met aantekeningen gegevensbronnen detecteren door te zoeken met behulp van de voorwaarden die geschikt zijn voor hen.

## <a name="summary"></a>Samenvatting
Een gegevensbron registreert met Data Catalog, die gegevens kunnen worden gedetecteerd door te kopiëren van de structurele en beschrijvende metagegevens uit de gegevensbron in de Catalog-service. Nadat een gegevensbron is geregistreerd, kunnen gebruikers aantekeningen om eenvoudiger te detecteren en te begrijpen door in de Data Catalog-portal opgeven.

## <a name="see-also"></a>Zie ook
* [Aan de slag met Azure Data Catalog](data-catalog-get-started.md) zelfstudies voor stapsgewijze informatie over hoe u aantekeningen toevoegen aan gegevensbronnen.
