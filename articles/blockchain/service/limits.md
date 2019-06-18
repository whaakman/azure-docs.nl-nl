---
title: Azure Blockchain-limieten
description: Overzicht van de service en functionele limieten in Azure Blockchain-Service
services: azure-blockchain
keywords: blockchain
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 169ec7a8ef407af3f754046aa8e3b06793a7e962
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028168"
---
# <a name="limits-in-azure-blockchain-service"></a>Limieten in Azure Blockchain-Service

Azure Blockchain-Service heeft de service en functionele limieten, zoals het aantal knooppunten dat een lid kan hebben, consortium beperkingen en opslag bedragen.

## <a name="pricing-tier"></a>Prijscategorie

Maximale limieten voor transacties en validatie van knooppunten is afhankelijk van of u Azure Blockchain-Service op Basic of Standard-Prijscategorieën wilt inrichten.

| Prijscategorie | Maximaal transactie knooppunten | Maximaal validator knooppunten |
|:---|:---:|:---:|
| Basic | 10 | 1 |
| Standard | 10 | 2 |

De prijscategorie tussen Basic en Standard wijzigen nadat het lid maken wordt niet ondersteund.

## <a name="storage-capacity"></a>Opslagcapaciteit

De maximale hoeveelheid opslag die per knooppunt kan worden gebruikt voor het grootboekgegevens en Logboeken is 1 terabyte.

Grootboek en logboekbestanden opslaggrootte verkleinen wordt niet ondersteund.

## <a name="consortium-limits"></a>Consortium limieten

* **Consortium en lid moet uniek** van de namen van andere consortium en lid in de Azure Blockchain-Service.

* **Namen van lid en consortium kunnen niet worden gewijzigd**

* **Alle leden in een consortium moeten zich in dezelfde prijscategorie**

* **Alle leden die deel uitmaken van een consortium moeten zich in dezelfde regio bevinden**

    Het eerste lid gemaakt in een consortium bepaalt de regio. Uitgenodigde leden aan de consortium moeten zich bevinden in dezelfde regio als het eerste lid. Alle leden bij dezelfde regio te beperken, zorgt ervoor dat netwerk consensus geen last hebben.

* **Een consortium moet ten minste één beheerder hebben.**

    Als er slechts één beheerder in een consortium, kunnen ze zelf verwijderen uit het consortium of hun lid verwijderen totdat een andere beheerder wordt toegevoegd aan of in het consortium gepromoveerd.

* **Leden verwijderd uit het consortium kunnen niet opnieuw worden toegevoegd.**

    In plaats daarvan deze moeten opnieuw worden uitgenodigd aan het consortium toevoegen en maak een nieuw lid. Hun bestaande lid resource worden niet verwijderd als u wilt behouden historische transacties.

* **Alle leden in een consortium moeten dezelfde versie van het grootboek gebruiken**

    Zie voor meer informatie over het toepassen van patches, updates en grootboek versies die beschikbaar zijn in Azure Blockchain Service [Patching, updates en versies](ledger-versions.md).

## <a name="next-steps"></a>Volgende stappen

* [Toepassen van patches, updates en versies](ledger-versions.md)
