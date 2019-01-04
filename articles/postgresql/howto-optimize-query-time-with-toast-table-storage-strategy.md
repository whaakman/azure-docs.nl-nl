---
title: Querytijd in Azure Database voor PostgreSQL-server met behulp van de pop-up tabel opslagstrategie optimaliseren
description: Dit artikel wordt beschreven hoe u op het moment dat de query met het pop tabel opslagstrategie in een Azure Database for PostgreSQL-server te optimaliseren.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 1fb818a65e26f969f72131b0f5265f3efdd36bb6
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542196"
---
# <a name="optimizing-query-time-with-toast-table-storage-strategy"></a>Querytijd met het pop-tabel opslagstrategie optimaliseren 
In dit artikel wordt beschreven hoe u query keer met het pop-tabel opslagstrategie optimaliseren.

## <a name="toast-table-storage-strategies"></a>Strategieën voor opslag van pop-tabel
Er zijn vier verschillende strategieën voor het opslaan van pop-kunnen kolommen op schijf voor verschillende combinaties tussen compressie en out-of-line-opslag. De strategie kan worden ingesteld op het niveau van het gegevenstype en op het kolomniveau van de.
- **Plain** wordt voorkomen dat de compressie of out-of-line-opslag; bovendien wordt het gebruik van enkel-byte-headers voor varlena typen. **Plain** is de enige mogelijke strategie voor kolommen van niet-pop-kunnen gegevenstypen.
- **Uitgebreide** kunnen zowel compressie als out-of-line-opslag. **Uitgebreide** is de standaardinstelling voor de meeste toast-kunnen gegevenstypen. Compressie worden geprobeerd eerste vervolgens out-of-line-opslag als de rij nog steeds te groot is.
- **Externe** kunt out-of-line-opslag, maar geen compressie. Het gebruik van **externe** maakt subtekenreeks bewerkingen op breed tekst en bytea kolommen sneller op de nadelig worden beïnvloed en meer opslagruimte, omdat deze bewerkingen zijn geoptimaliseerd voor het ophalen van alleen de vereiste onderdelen van de out-of-line waarde wanneer het niet gecomprimeerd.
- **Belangrijkste** staat compressie maar niet out-of-line-opslag. Out-of-line-opslag wordt nog steeds uitgevoerd voor dergelijke kolommen, maar alleen als laatste redmiddel wanneer er geen andere manier om de rij klein genoeg om aan te passen op een pagina.

## <a name="using-toast-table-storage-strategies"></a>Met behulp van pop-up table storage strategieën
Als uw query's kunnen toast gegevenstypen, kunt u overwegen **Main** in plaats van de standaard **Extended** optie om te keren query verminderen. **Belangrijkste** belet niet out-of-line-opslag. Anderzijds, als uw query's geen toegang toast-kunnen gegevenstypen tot, het kan zinvol zijn om te houden de **Extended** optie. Dus past een groter deel van de rijen van de hoofdtabel in de buffercache gedeelde, waardoor de prestaties.

Als u een workload met behulp van een schema met brede tabellen en aantal hoge tekens hebt, kunt u overwegen PostgreSQL TOAST-tabellen. Een voorbeeld van de klant-tabel had groter is dan 350 kolommen met meerdere kolommen spanning van 255 tekens. De tijd van de query benchmark verminderd van 4203 seconden tot 467 seconden, een verbetering 89 procent, na het converteren van de pop-strategie van **Main**.

## <a name="next-steps"></a>Volgende stappen
Controleer uw werkbelasting voor de bovenstaande kenmerken. 

Raadpleeg de volgende PostgreSQL-documentatie: [Hoofdstuk 68, fysieke Database-opslag](https://www.postgresql.org/docs/current/storage-toast.html) 