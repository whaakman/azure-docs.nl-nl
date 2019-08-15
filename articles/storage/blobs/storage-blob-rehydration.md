---
title: BLOB-gegevens worden opnieuw gehydrateerd op basis van de opslaglaag
description: U kunt de blobs uit archief opslag opnieuw gebruiken zodat u toegang hebt tot de gegevens.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/07/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 2e7d56a1461dfd89a7309288aadb0ba245d0f885
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68958211"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>BLOB-gegevens worden opnieuw gehydrateerd op basis van de opslaglaag

Terwijl een BLOB zich in de Access-laag Archive bevindt, wordt deze als offline beschouwd en kan deze niet worden gelezen of gewijzigd. De meta gegevens van de BLOB blijven online en beschikbaar, zodat u de BLOB en de bijbehorende eigenschappen kunt weer geven. Het lezen en wijzigen van BLOB-gegevens is alleen beschikbaar voor online lagen, zoals warme of koud. Er zijn twee opties voor het ophalen en openen van gegevens die zijn opgeslagen in de toegangs laag voor het archief.

1. [Een gearchiveerde BLOB opnieuw gehydrateerd naar een online-laag](#rehydrate-an-archived-blob-to-an-online-tier) : een gearchiveerde BLOB opnieuw laten worden gehydrateerd naar warme of koud door de laag te wijzigen met behulp van de bewerking [BLOB-laag instellen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) .
2. [Een gearchiveerde BLOB naar een online-laag kopiëren](#copy-an-archived-blob-to-an-online-tier) : Maak een nieuwe kopie van een gearchiveerde blob met behulp van de [Kopieer bewerking BLOB](https://docs.microsoft.com/rest/api/storageservices/copy-blob) . Geef een andere blobnaam en een bestemmings laag op met de naam Hot of cool.

 Zie [Azure Blob Storage: warme, cool en archief toegangs lagen](storage-blob-storage-tiers.md)voor meer informatie over lagen.

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Een gearchiveerde BLOB opnieuw naar een online-laag gehydrateerd

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Een gearchiveerde BLOB kopiëren naar een online-laag

Als u een BLOB niet opnieuw wilt laten worden gehydrateerd, kunt u een [Kopieer BLOB](https://docs.microsoft.com/rest/api/storageservices/copy-blob) -bewerking kiezen. De oorspronkelijke BLOB blijft ongewijzigd in archief terwijl u aan de nieuwe BLOB werkt in de warme of koude laag. U kunt de optionele *x-MS-onhydrat-Priority-* eigenschap instellen op Standard of High (preview) wanneer u het kopieer proces gebruikt.

Archief-blobs kunnen alleen worden gekopieerd naar online doel lagen. Het kopiëren van een archief-BLOB naar een andere archief-BLOB wordt niet ondersteund.

Het kopiëren van een BLOB uit het archief duurt tijd. Achter de schermen wordt met de bewerking **BLOB kopiëren** tijdelijk de blob van de archief bron opnieuw gehydrateerd om een nieuwe online-Blob in de doellaag te maken. Deze nieuwe blob is pas beschikbaar als de tijdelijke rehydratatie van Archive is voltooid en de gegevens naar de nieuwe BLOB zijn geschreven.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Reactiveren-blobs uit het archief in warme of coole lagen worden in rekening gebracht als Lees bewerkingen en gegevens ophalen. Het gebruik van high priority (preview) heeft hogere kosten voor bewerkingen en het ophalen van gegevens in vergelijking met de standaard prioriteit. Rehydratatie met hoge prioriteit worden weer gegeven als een afzonderlijk regel item op uw factuur. Als een aanvraag met hoge prioriteit voor het retour neren van een archief-blob van een paar gigabytes meer dan vijf uur duurt, wordt niet het ophaal tarief met hoge prioriteit in rekening gebracht. De standaard tarieven voor ophalen zijn echter nog steeds van toepassing.

Het kopiëren van blobs van Archive in warme of coole lagen wordt in rekening gebracht als Lees bewerkingen en ophalen van gegevens. Voor het maken van de nieuwe kopie wordt een schrijf bewerking in rekening gebracht. Kosten voor vroegtijdig verwijderen zijn niet van toepassing wanneer u naar een online-BLOB kopieert, omdat de bron-BLOB ongewijzigd blijft in de laag van het archief. Kosten voor hoge prioriteit zijn van toepassing.

Blobs in de archief laag moeten mini maal 180 dagen worden opgeslagen. Als u gearchiveerde blobs verwijdert of reactiveren, worden de kosten 180 voor vroegtijdige verwijdering in rekening gebracht.

> [!NOTE]
> Zie [Azure Storage prijzen](https://azure.microsoft.com/pricing/details/storage/blobs/)voor meer informatie over de prijzen voor blok-blobs en gegevens rehydratatie. Zie [prijs informatie voor gegevens overdracht](https://azure.microsoft.com/pricing/details/data-transfers/)voor meer informatie over de kosten voor uitgaande gegevens overdracht.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Blob Storage lagen](storage-blob-storage-tiers.md)
* [Prijzen voor de dynamische, statische en archieflaag controleren in Blob Storage- en GPv2-accounts per regio](https://azure.microsoft.com/pricing/details/storage/)
* [De levens cyclus van Azure Blob-opslag beheren](storage-lifecycle-management-concepts.md)
* [Prijzen voor gegevensoverdracht controleren](https://azure.microsoft.com/pricing/details/data-transfers/)
