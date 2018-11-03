---
title: Maken van tekenreeksen voor de table designer | Microsoft Docs
description: Maken van tekenreeksen voor de table designer
services: visual-studio-online
author: ghogen
manager: douge
assetId: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/18/2016
ms.author: ghogen
ms.openlocfilehash: 1e2fd16d25d9552783d12000820cac56803fca63
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969382"
---
# <a name="constructing-filter-strings-for-the-table-designer"></a>Maken van tekenreeksen voor de Table Designer
## <a name="overview"></a>Overzicht
Om gegevens te filteren in een Azure-tabel die wordt weergegeven in de Visual Studio **Tabelontwerpfunctie**, u een filtertekenreeks samenstellen en voert u deze in het filterveld. De syntaxis van de filter wordt gedefinieerd door de WCF-gegevensservices en is vergelijkbaar met een SQL WHERE-component, maar naar de tabel-service via een HTTP-aanvraag is verzonden. De **Table Designer** verwerkt de juiste codering bij u past, dus als u wilt filteren op een gewenste eigenschapswaarde alleen u de naam van de eigenschap, vergelijkingsoperator, waarde voor waarschuwingscriteria voor, en (optioneel) Booleaanse waarde voert moet operator in het filterveld in. U hoeft niet de queryoptie $filter bevatten, zoals u zou doen als u een URL op om een query uitvoeren op de tabel via zijn maken de [naslaginformatie voor Storage Services REST API](http://go.microsoft.com/fwlink/p/?LinkId=400447).

De WCF Data Services zijn gebaseerd op de [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Voor meer informatie over de filteroptie system-query (**$filter**), Zie de [conventies voor OData-URI-specificatie](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Vergelijkingsoperators
De volgende logische operators worden ondersteund voor alle soorten:

| Logische operator | Beschrijving | Voorbeeld van de filtertekenreeks |
| --- | --- | --- |
| eq |Gelijk aan |Plaats-eq "Redmond" |
| gt |Groter dan |Prijs gt 20 |
| ge |Groter dan of gelijk aan |Prijs ge 10 |
| lt |Kleiner dan |Prijs lt 20 |
| le |Kleiner dan of gelijk zijn |Prijs le 100 |
| ne |Niet gelijk aan |Plaats ne "Londen" |
| en |En |Prijs le 200 en prijs gt 3.5 |
| of |of |Prijs le 3.5 of prijs gt 200 |
| niet |niet |niet isAvailable |

De volgende regels zijn van belang bij het maken van een filtertekenreeks:

* De logische operators gebruiken om te vergelijken van een eigenschap een waarde. Houd er rekening mee dat het is niet mogelijk om te vergelijken van een eigenschap met een dynamische waarde; een-zijde van de expressie moet een constante zijn.
* Alle onderdelen van de filtertekenreeks zijn hoofdlettergevoelig.
* Het filter retourneert alleen geldige resultaten als de constante waarde van hetzelfde gegevenstype is als de eigenschap. Zie [Understanding the Table Service Data Model](http://go.microsoft.com/fwlink/p/?LinkId=400448) (Het gegevensmodel van de tabelservice) voor meer informatie over ondersteunde eigenschapstypen.

## <a name="filtering-on-string-properties"></a>Filteren op de eigenschappen van een verbindingsreeks
Als u op Eigenschappen van een verbindingsreeks filtert, plaatst u de tekenreeksconstante tussen enkele aanhalingstekens.

Het volgende voorbeeld kunt u filteren op de **PartitionKey** en **RowKey** eigenschappen; aanvullende zonder sleutel eigenschappen kunnen ook worden toegevoegd aan de filtertekenreeks:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

U kunt tussen haakjes elke filterexpressie, maar dit niet vereist is:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Houd er rekening mee dat de Table-service biedt geen ondersteuning voor query's met jokertekens, en ze worden niet ondersteund in de ontwerpfunctie ofwel. U kunt echter met behulp van vergelijkingsoperators op het voorvoegsel van de gewenste vergelijken van voorvoegsels uitvoeren. Het volgende voorbeeld retourneert entiteiten met een LastName eigenschap die begint met de letter 'A':

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filteren op numerieke eigenschappen
Als u wilt filteren op een geheel getal of getal met drijvende komma, geeft u het aantal zonder aanhalingstekens.

In dit voorbeeld retourneert alle entiteiten met een eigenschap leeftijd is waarvan de waarde groter dan 30 is:

    Age gt 30

In dit voorbeeld retourneert alle entiteiten met een eigenschap van een tegoed waarvan de waarde kleiner dan of gelijk zijn aan 100.25 is:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filteren op Boole-eigenschappen
Als u wilt filteren op een Booleaanse waarde, geef **waar** of **false** zonder aanhalingstekens.

Het volgende voorbeeld retourneert alle entiteiten waarvan de eigenschap IsActive is ingesteld op **waar**:

    IsActive eq true

U kunt ook deze filterexpressie zonder de logische operator schrijven. In het volgende voorbeeld wordt de Table-service worden ook alle entiteiten retourneren wanneer IsActive is **waar**:

    IsActive

Als u wilt alle entiteiten retourneren wanneer IsActive is ingesteld op false, kunt u het niet operator:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filteren op datum/tijd-eigenschappen
Als u wilt filteren op een datum / tijdwaarde, geef de **datum-/** sleutelwoord, gevolgd door de constante datum/tijd tussen enkele aanhalingstekens. De datum/tijd-constante moet in gecombineerde UTC-notatie, zoals beschreven in [eigenschapswaarden voor datum/tijd opmaken](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Het volgende voorbeeld retourneert entiteiten waarbij de eigenschap CustomerSince is gelijk aan 10 juli 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
