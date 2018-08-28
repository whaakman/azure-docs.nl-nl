---
title: Azure Data Lake Analytics-beleid beheren
description: Leer hoe u beleidsregels gebruiken voor het gebruik van een Data Lake Analytics-account beheren.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 0a6102d1-7554-4df2-b487-4dae9a7287b6
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: f84cb59e7d4fd7d8301d22348ca066a7f9d9e94e
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048815"
---
# <a name="manage-azure-data-lake-analytics-using-policies"></a>Azure Data Lake Analytics met behulp van beleid beheren

Met behulp van accountbeleid, kunt u bepalen hoe resources een Azure Data Lake Analytics-account worden gebruikt. Deze beleidsregels kunnen u de kosten van het gebruik van Azure Data Lake Analytics te controleren. Met dit beleid kunt u bijvoorbeeld pieken onverwachte kosten voorkomen door te beperken hoeveel AU's dat gelijktijdig gebruik van het account.

## <a name="account-level-policies"></a>Beleid op account-niveau

Dit beleid van toepassing op alle taken in een Data Lake Analytics-account.

### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Maximum aantal AU's in een Data Lake Analytics-account
Een beleid regelt het totale aantal Analytics Units (AU) uw Data Lake Analytics-account kunt gebruiken. De waarde is standaard ingesteld op 250. Bijvoorbeeld, als deze waarde is ingesteld op 250 AU's, kunt u een taak wordt uitgevoerd met 250 hebt AU's die zijn toegewezen aan of 10 taken die worden uitgevoerd met 25 elke AU's. Aanvullende taken die zijn ingediend in de wachtrij geplaatst totdat de taken die worden uitgevoerd zijn voltooid. Wanneer taken die worden uitgevoerd zijn voltooid, wordt AU's worden vrijgemaakt voor de taken in de wachtrij om uit te voeren.

Het aantal AU's voor uw Data Lake Analytics-account wijzigen:

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **Maximum AU's**, verplaats de schuifregelaar een waarde te selecteren of Voer de waarde in het tekstvak. 
4. Klik op **Opslaan**.

> [!NOTE]
> Als u meer dan de standaardwaarde (250) AU's; in de portal, klikt u op **Help + ondersteuning** een ondersteuningsaanvraag indienen. Het aantal AU's beschikbaar zijn in uw Data Lake Analytics-account kan worden verhoogd.
>

### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Maximum aantal taken dat tegelijk kan worden uitgevoerd
Een beleid bepaalt hoeveel taken kunnen worden uitgevoerd op hetzelfde moment. Deze waarde is standaard ingesteld op 20. Als uw Data Lake Analytics AU's dat beschikbaar is, worden nieuwe taken worden gepland om uit te voeren onmiddellijk totdat de waarde van dit beleid is het totale aantal actieve taken bereikt. Wanneer u het maximum aantal taken dat tegelijk kan worden uitgevoerd, volgende taken worden in de wachtrij in volgorde van prioriteit totdat een of meer taken die worden uitgevoerd (afhankelijk van beschikbaarheid Australië).

Wijzigen van het aantal taken dat tegelijk kan worden uitgevoerd:

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **Maximum aantal van een actief taken**, verplaats de schuifregelaar een waarde te selecteren of Voer de waarde in het tekstvak. 
4. Klik op **Opslaan**.

> [!NOTE]
> Als u meer dan het (20) standaardaantal taken, in de portal worden uitgevoerd wilt, klikt u op **Help + ondersteuning** een ondersteuningsaanvraag indienen. Het aantal taken die gelijktijdig kunnen worden uitgevoerd in uw Data Lake Analytics-account kan worden verhoogd.
>

### <a name="how-long-to-keep-job-metadata-and-resources"></a>Hoe lang tot metagegevens van de taak behouden en bronnen 
Wanneer uw gebruikers U-SQL-taken uitvoert, worden alle gerelateerde bestanden bewaard in de Data Lake Analytics-service. Gerelateerde bestanden bevatten de U-SQL-script, de dll-bestanden waarnaar wordt verwezen in de U-SQL-script, gecompileerde, resources en statistieken. De bestanden zich in de map /system/ van het Azure Data Lake Storage-standaardaccount. Dit beleid wordt bepaald hoe lang deze resources worden opgeslagen voordat ze automatisch worden verwijderd (de standaardwaarde is 30 dagen). U kunt deze bestanden gebruiken voor foutopsporing en prestatieafstemming van taken die u in de toekomst opnieuw gaat uitvoeren.

Wijzigen hoe lang om metagegevens van taak en resources te houden:

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **dagen te bewaren Taakquery's**, verplaats de schuifregelaar een waarde te selecteren of Voer de waarde in het tekstvak.  
4. Klik op **Opslaan**.

## <a name="job-level-policies"></a>Beleid op taak-niveau

Taak-niveau implementeert, kunt u bepalen het maximumaantal AU's en de maximale prioriteit die afzonderlijke gebruikers (of leden van specifieke beveiligingsgroepen) kunnen instellen voor taken die ze aanbieden. Dit beleid kunt u de kosten in rekening gebracht door gebruikers te beheren. U kunt ook het effect dat taken geplande kan hebben op hoge prioriteit productietaken die worden uitgevoerd in hetzelfde Data Lake Analytics-account beheren.

Data Lake Analytics heeft twee beleidsregels die u op het taakniveau van de instellen kunt:

* **AU-limiet per taak**: gebruikers kunnen alleen taken die met dit aantal AU's worden verzonden. Deze limiet is standaard hetzelfde als de maximale AU-limiet voor het account.
* **Prioriteit**: gebruikers kunnen alleen taken die een prioriteit die lager is dan of gelijk zijn aan deze waarde indienen. Hoe hoger de waarde geeft aan dat een lagere prioriteit. Standaard is deze limiet ingesteld op 1, dit is de hoogste prioriteit mogelijk.

Er is een standaardbeleid instellen voor elke account. Het standaardbeleid is van toepassing op alle gebruikers van het account. U kunt extra beleidsregels instellen voor specifieke gebruikers en groepen. 

> [!NOTE]
> Beleidsregels voor accountniveau en taakniveau zijn van toepassing tegelijk.
>

### <a name="add-a-policy-for-a-specific-user-or-group"></a>Een beleid voor een specifieke gebruiker of groep toevoegen

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **limieten voor het verzenden van taak**, klikt u op de **beleid toevoegen** knop. Vervolgens selecteert u of Voer de volgende instellingen:
    1. **Beleidsnaam COMPUTE**: Voer een beleidsnaam eraan te herinneren dat u van het doel van het beleid.
    2. **Selecteer gebruiker of groep**: Selecteer de gebruiker of groep die dit beleid is van toepassing op.
    3. **De AU-limiet voor de taak instellen**: Stel de AU-limiet die van toepassing op de geselecteerde gebruiker of groep.
    4. **De limiet van de prioriteit instellen**: de limiet voor de prioriteit die van toepassing op de geselecteerde gebruiker of groep instellen.

4. Klik op **OK**.

5. Het nieuwe beleid wordt weergegeven in de **standaard** beleid tabel onder **limieten voor het verzenden van taak**. 

### <a name="delete-or-edit-an-existing-policy"></a>Verwijderen of een bestaande beleidsregel bewerken

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **limieten voor het verzenden van taak**, vinden van het beleid dat u wilt bewerken.
4.  Om te zien de **verwijderen** en **bewerken** opties in de meest rechtse kolom van de tabel, klikt u op `...`.

## <a name="additional-resources-for-job-policies"></a>Aanvullende bronnen voor taak-beleid
* [Beleid overzicht blogbericht](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Blogbericht accountniveau beleid](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Blogbericht taakniveau beleid](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Data Lake Analytics met behulp van Azure portal](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics beheren met behulp van Azure PowerShell](data-lake-analytics-manage-use-powershell.md)

