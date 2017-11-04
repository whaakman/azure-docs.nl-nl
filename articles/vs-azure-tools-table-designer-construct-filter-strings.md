---
title: Tekenreeksen voor de ontwerpfunctie voor tabellen maken | Microsoft Docs
description: Tekenreeksen voor de ontwerpfunctie voor tabellen maken
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 069224d84462b4955912ce1462a65298a5acc04a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="constructing-filter-strings-for-the-table-designer"></a>Tekenreeksen voor de ontwerpfunctie voor tabellen maken
## <a name="overview"></a>Overzicht
Om gegevens te filteren in een Azure-tabel die wordt weergegeven in de Visual Studio **tabelontwerp**, u een filtertekenreeks in te stellen en voer deze in het filterveld. De syntaxis van de filter wordt gedefinieerd door de WCF Data Services en is vergelijkbaar met een SQL WHERE-component, maar naar de tabel-service via een HTTP-aanvraag is verzonden. De **tabelontwerp** verwerkt codering voor u, zodat u wilt filteren op een waarde van de gewenste eigenschap, moet alleen voert u de naam van eigenschap, vergelijkingsoperator, waarde voor foutcriteria voor, en desgewenst Booleaanse waarde operator in het filterveld. U hoeft niet de queryoptie $filter bevatten, zoals u zou doen als u een URL in de tabel via een query zijn construeren de [Storage Services REST API-verwijzing](http://go.microsoft.com/fwlink/p/?LinkId=400447).

De WCF Data Services zijn gebaseerd op de [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Voor meer informatie over de filteroptie system-query (**$filter**), Zie de [OData URI Conventions-specificatie](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Vergelijkingsoperators
De volgende logische operators worden ondersteund voor alle eigenschaptypen:

| Logische operator | Beschrijving | Voorbeeld filtertekenreeks |
| --- | --- | --- |
| EQ |Gelijk zijn aan |Plaats-eq "Redmond" |
| gt |Groter dan |Prijs gt 20 |
| ge |Groter dan of gelijk aan |Prijs ge 10 |
| lt |Kleiner dan |Prijs lt 20 |
| RP |Kleiner dan of gelijk zijn |Prijs RP 100 |
| ne |Niet gelijk aan |Plaats ne 'Londen' |
| en |En |Prijs RP 200 en prijs gt 3.5 |
| of |of |Prijs RP 3.5 of prijs gt 200 |
| niet |niet |niet isAvailable |

De volgende regels zijn van belang bij het maken van een filtertekenreeks in:

* De logische operators gebruiken een eigenschap een waarde te vergelijken. Houd er rekening mee dat het is niet mogelijk om te vergelijken van een eigenschap aan een dynamische waarde; een-zijde van de expressie moet een constante zijn.
* Alle onderdelen van de filtertekenreeks zijn hoofdlettergevoelig.
* De constante waarde moet van hetzelfde gegevenstype als de eigenschap om het filter geldige resultaten retourneren. Zie voor meer informatie over ondersteunde eigenschaptypen [inzicht in de tabel Service Data Model](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filteren op Eigenschappen
Wanneer u op Eigenschappen van een verbindingsreeks filtert, moet u de tekenreeksconstante tussen enkele aanhalingstekens.

Het volgende voorbeeld kunt u filteren op de **PartitionKey** en **RowKey** eigenschappen; extra niet-sleutelkenmerk eigenschappen kunnen ook worden toegevoegd aan de filtertekenreeks:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

U kunt haakjes elke filterexpressie, hoewel dit niet vereist is:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Houd er rekening mee dat de tabel-service biedt geen ondersteuning voor query's met jokertekens en ze worden niet ondersteund in de ontwerpfunctie ofwel. U kunt echter met behulp van vergelijkingsoperators op het gewenste voorvoegsel vergelijken van voorvoegsels uitvoeren. Het volgende voorbeeld retourneert entiteiten met een eigenschap LastName die begint met de letter "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filteren op numerieke eigenschappen
Als u wilt filteren op een geheel getal of een getal met drijvende komma, geef het aantal zonder aanhalingstekens.

In dit voorbeeld retourneert alle entiteiten met een eigenschap Age waarvan de waarde groter dan 30 is:

    Age gt 30

In dit voorbeeld retourneert alle entiteiten met een eigenschap van een tegoed waarvan de waarde kleiner dan of gelijk zijn aan 100.25 is:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filteren op Boole-eigenschappen
Als u wilt filteren op een Booleaanse waarde, geef **true** of **false** zonder aanhalingstekens.

Het volgende voorbeeld worden alle entiteiten waarvan de eigenschap IsActive is ingesteld op **true**:

    IsActive eq true

U kunt ook deze filterexpressie zonder de logische operator schrijven. In het volgende voorbeeld wordt de tabel-service geeft ook alle entiteiten waarbij IsActive is **true**:

    IsActive

Voor alle entiteiten waarbij IsActive false is, kunt u het niet operator:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filteren op datum/tijd-eigenschappen
Als u wilt filteren op een DateTime-waarde, geef de **datetime** sleutelwoord, gevolgd door de constante datum/tijd tussen enkele aanhalingstekens. De datum/tijd-constante moet in gecombineerde UTC-notatie, zoals beschreven in [eigenschapswaarden voor datum/tijd opmaken](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Het volgende voorbeeld retourneert entiteiten waarbij de eigenschap CustomerSince is gelijk aan 10 juli 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
