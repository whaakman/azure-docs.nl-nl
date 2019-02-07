---
title: Ondersteunde FHIR-functies in Azure - API van Azure voor FHIR
description: Dit artikel wordt uitgelegd welke functies van de FHIR-specificatie die zijn geïmplementeerd in Azure-API voor FHIR
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: a2851272d15ed2ce2c7f940cc3e429ef9f8b4446
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824038"
---
# <a name="features"></a>Functies

Azure-API voor FHIR biedt een volledig beheerde implementatie van de Microsoft FHIR-Server voor Azure. De server is een implementatie van de [FHIR](https://hl7.org/fhir) standard. Dit document geeft een lijst van de belangrijkste functies van de FHIR-Server.

## <a name="fhir-version"></a>FHIR-versie

huidige versie: `3.0.1`

## <a name="rest-api"></a>REST-API

| API                            | Ondersteund | Opmerking |
|--------------------------------|-----------|---------|
| lezen                           | Ja       |         |
| vread                          | Ja       |         |
| update                         | Ja       |         |
| werken met optimistische vergrendeling | Ja       |         |
| Update (voorwaardelijke)           | Nee        |         |
| Patch                          | Nee        |         |
| delete                         | Ja       |         |
| verwijderen (voorwaardelijke)           | Nee        |         |
| maken                         | Ja       | Ondersteuning voor zowel POST/PUT |
| (voorwaardelijke) maken           | Nee        |         |
| zoeken                         | Gedeeltelijk   | Zie hieronder |
| Mogelijkheden                   | Ja       |         |
| batch                          | Nee        |         |
| Transactie                    | Nee        |         |
| Geschiedenis                        | Ja       |         |
| Wisselgeheugengebruik                         | Gedeeltelijk   | `self` en `next` worden ondersteund |
| schakels                 | Nee        |         |

## <a name="search"></a>Search

Alle zoeken parametertypen worden ondersteund. Parameters en omgekeerde chaining zijn in een keten *niet* ondersteund.

| Het parametertype zoeken | Ondersteund | Opmerking |
|-----------------------|-----------|---------|
| Aantal                | Ja       |         |
| Datum/datum/tijd         | Ja       |         |
| String                | Ja       |         |
| Token                 | Ja       |         |
| Referentie             | Ja       |         |
| Samengestelde             | Ja       |         |
| Hoeveelheid              | Ja       | Probleem [#103](https://github.com/Microsoft/fhir-server/issues/103) |
| URI                   | Ja       |         |


| Parameters             | Ondersteund | Opmerking |
|-----------------------|-----------|---------|
|`:missing`             | Ja       |         |
|`:exact`               | Ja       |         |
|`:contains`            | Ja       |         |
|`:text`                | Ja       |         |
|`:in` (token)          | Nee        |         |
|`:below` (token)       | Nee        |         |
|`:above` (token)       | Nee        |         |
|`:not-in` (token)      | Nee        |         |
|`:[type]` (verwijzing)  | Nee        |         |
|`:below` (uri)         | Ja       |         |
|`:above` (uri)         | Nee        | Probleem [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Algemene zoekopdracht-parameter | Ondersteund | Opmerking |
|-------------------------| ----------|---------|
| `_id`                   | Ja       |         |
| `_lastUpdated`          | Ja       |         |
| `_tag`                  | Ja       |         |
| `_profile`              | Ja       |         |
| `_security`             | Ja       |         |
| `_text`                 | Nee        |         |
| `_content`              | Nee        |         |
| `_list`                 | Nee        |         |
| `_has`                  | Nee        |         |
| `_type`                 | Ja       |         |
| `_query`                | Nee        |         |

| Zoekbewerkingen       | Ondersteund | Opmerking |
|-------------------------|-----------|---------|
| `_filter`               | Nee        |         |
| `_sort`                 | Nee        |         |
| `_score`                | Nee        |         |
| `_count`                | Ja       |         |
| `_summary`              | Gedeeltelijk   | `_summary=count` wordt ondersteund |
| `_include`              | Nee        |         |
| `_revinclude`           | Nee        |         |
| `_contained`            | Nee        |         |
| `_elements`             | Nee        |         |

## <a name="persistence"></a>Persistentie

De Microsoft FHIR-Server heeft een module pluggable persistentie (Zie [ `Microsoft.Health.Fhir.Core.Features.Persistence` ](https://github.com/Microsoft/fhir-server/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

De open-source code FHIR bevat momenteel een implementatie voor [Azure Cosmos DB](../cosmos-db/index-overview.md).

Cosmos DB is een wereldwijd gedistribueerde multi-model (SQL-API, MongoDB-API, enzovoort)-database. Het ondersteunt verschillende [consistentieniveaus](../cosmos-db/consistency-levels.md). De sjabloon voor de implementatie standaard configureert de FHIR-Server met `Strong` consistentie, maar het beleid consistentie kan (in het algemeen soepele) worden gewijzigd op een aanvraag door een aanvraag uit te voeren met de `x-ms-consistency-level` aanvraagheader.

## <a name="role-based-access-control"></a>Toegangsbeheer op basis van rollen

Maakt gebruik van de Server FHIR [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) voor toegangsbeheer. Specifiek, rollen gebaseerd toegangsbeheer (RBAC) wordt afgedwongen, als de `FhirServer:Security:Enabled` configuratieparameter is ingesteld op `true`, en alle aanvragen (met uitzondering van `/metadata`) op de Server FHIR moet hebben `Authorization` aanvraagheader ingesteld op `Bearer <TOKEN>`. Het token moet een of meer rollen bevatten, zoals gedefinieerd in de `roles` claim. Een aanvraag kunnen worden als het token een rol bevat die de opgegeven actie kan op de opgegeven resource.

Op dit moment de toegestane acties voor een bepaalde rol worden toegepast *wereldwijd* op de API.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u lezen over de ondersteunde FHIR-functies in Azure-API voor FHIR. Naast een FHIR-API in Azure implementeren.
 
>[!div class="nextstepaction"]
>[Open Source-FHIR-server implementeren](fhir-oss-powershell-quickstart.md)