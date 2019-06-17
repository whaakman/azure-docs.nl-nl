---
title: Uitvoeren van query's optimaliseren met behulp van de strategie TOAST table storage in Azure Database voor PostgreSQL - Server één
description: Dit artikel wordt beschreven hoe u op het moment dat de query met het pop tabel opslagstrategie op een Azure Database voor PostgreSQL - Server één optimaliseren.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: ac1dc43a2b89bc1cc748947ec08e6ada87edbfcb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65066977"
---
# <a name="optimize-query-time-with-the-toast-table-storage-strategy"></a>Querytijd met het pop tabel-opslagstrategie optimaliseren 
In dit artikel wordt beschreven hoe u uitvoertijden van query met de opslag van grote kenmerk techniek (pop-up) tabel-opslagstrategie optimaliseren.

## <a name="toast-table-storage-strategies"></a>Strategieën voor opslag van pop-tabel
Vier verschillende strategieën worden gebruikt voor het opslaan van kolommen op de schijf die TOAST kunt gebruiken. Ze zijn verschillende combinaties tussen compressie en out-of-line-opslag. De strategie kan worden ingesteld op het niveau van het gegevenstype en op het kolomniveau van de.
- **Plain** wordt voorkomen dat de compressie- of out-of-line-opslag. Dit wordt het gebruik van enkel-byte-headers voor varlena typen. Zonder opmaak is de enige mogelijke strategie voor kolommen van de gegevenstypen die TOAST kunnen niet worden gebruikt.
- **Uitgebreide** kunnen zowel compressie als out-of-line-opslag. Uitgebreide, is de standaardinstelling voor de meeste gegevenstypen die TOAST kunt gebruiken. Compressie wordt eerst geprobeerd. Out-of-line-opslag wordt uitgevoerd als de rij nog steeds te groot is.
- **Externe** kunt out-of-line-opslag, maar geen compressie. Gebruik van externe maakt subtekenreeks bewerkingen op brede tekst en bytea kolommen sneller. Deze snelheid wordt geleverd met de nadelig worden beïnvloed en meer opslagruimte. Deze bewerkingen zijn geoptimaliseerd voor het ophalen van alleen de vereiste onderdelen van de out-of-line-waarde wanneer het niet gecomprimeerd.
- **Belangrijkste** staat compressie maar niet out-of-line-opslag. Out-of-line-opslag wordt nog steeds uitgevoerd voor dergelijke kolommen, maar alleen als laatste redmiddel. Dit treedt op wanneer er geen andere manier om de rij klein genoeg om aan te passen op een pagina.

## <a name="use-toast-table-storage-strategies"></a>Strategieën voor pop-up table storage gebruiken
Als uw query's toegang gegevenstypen die TOAST gebruiken tot kunnen, kunt u overwegen de Main-strategie in plaats van de standaardoptie voor uitgebreide te verminderen uitvoertijden van query. Belangrijkste sprake niet is van out-of-line-opslag. Als uw query's geen toegang de gegevenstypen die TOAST gebruiken tot kunnen, is het mogelijk nuttig is om de optie voor uitgebreide houden. Een groter deel van de rijen van de hoofdtabel passen in de buffercache gedeelde, die helpt de prestaties.

Als u een werkbelasting die gebruikmaakt van een schema met brede tabellen en aantal hoge tekens hebt, kunt u overwegen PostgreSQL TOAST-tabellen. Een voorbeeld van de klant-tabel had groter is dan 350 kolommen met meerdere kolommen die liep 255 tekens. Nadat deze is geconverteerd naar de tabel TOAST Main-strategie, wordt de status van hun tijd van de query benchmark verkort van 4203 seconden tot 467 seconden. Dat is een verbetering 89 procent.

## <a name="next-steps"></a>Volgende stappen
Controleer uw werkbelasting voor de vorige kenmerken. 

Raadpleeg de volgende PostgreSQL-documentatie: 
- [Hoofdstuk 68, fysieke Database-opslag](https://www.postgresql.org/docs/current/storage-toast.html) 