---
title: bestand opnemen
description: bestand opnemen
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/09/2019
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: 03674a51566ab89791725d1a51c7af12ed6949e5
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952895"
---
Als u gegevens wilt lezen die aanwezig zijn in Archive Storage, moet u de laag van de blob eerst wijzigen in Hot of Cool. Dit proces wordt ook wel rehydratatie genoemd en kan uren duren om te volt ooien. We raden grote BLOB-grootten aan voor optimale rehydratatie-prestaties. Enkele kleine blobs tegelijk reactiveren kan extra tijd toevoegen. Er zijn momenteel twee herhydrate prioriteiten, hoog (preview) en standaard, die kunnen worden ingesteld via de optionele *x-MS-autohydrat-Priority-* eigenschap op een [set BLOB-laag](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) of een BLOB-bewerking [kopiëren](https://docs.microsoft.com/rest/api/storageservices/copy-blob) .

* **Standaard prioriteit**: De rehydratatie-aanvraag wordt verwerkt in de volg orde waarin deze is ontvangen en kan Maxi maal 15 uur duren.
* **Hoge prioriteit (preview-versie)** : De rehydratatie-aanvraag krijgt prioriteit boven standaard aanvragen en kan binnen 1 uur worden voltooid. Hoge prioriteit kan langer dan 1 uur duren, afhankelijk van de grootte van de BLOB en de huidige vraag. Aanvragen met een hoge prioriteit worden gegarandeerd met prioriteit voor aanvragen met een standaard prioriteit.

> [!NOTE]
> Standaard prioriteit is de standaard optie voor rehydratatie voor archiveren. Hoge prioriteit is een snellere optie waarmee meer dan de standaard prioriteit rehydratatie wordt berekend en meestal wordt gereserveerd voor gebruik in situaties waarin nood herstel van gegevens wordt uitgevoerd.

Tijdens rehydratatie kunt u aan de blob-eigenschap *archive status* zien of de laag is gewijzigd. De status is "rehydrate-pending-to-hot" of "rehydrate-pending-to-cool", afhankelijk van de doellaag. Als de bewerking is voltooid, wordt de blob-eigenschap voor archiefstatus verwijderd en geeft de blob-eigenschap *Toegangslaag* aan of de opslaglaag dynamisch of statisch is.
