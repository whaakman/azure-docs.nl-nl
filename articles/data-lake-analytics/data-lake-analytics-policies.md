---
title: Azure Data Lake Analytics-beleid beheren
description: Informatie over het beleid gebruiken voor het gebruik van een Data Lake Analytics-account beheren.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: 0a6102d1-7554-4df2-b487-4dae9a7287b6
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 2c074b1a75c5bfef07ffb90e719bd3247a474e63
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623941"
---
# <a name="manage-azure-data-lake-analytics-using-policies"></a>Azure Data Lake Analytics beheren met beleidsregels

Accountbeleid gebruikt, kunt u bepalen hoe resources een Azure Data Lake Analytics-account worden gebruikt. Dit beleid kunt u de kosten van het gebruik van Azure Data Lake Analytics beheren. Met dit beleid kunt u bijvoorbeeld onverwachte kosten pieken voorkomen door te beperken hoeveel AUs gelijktijdig gebruik van het account maken kunt.

## <a name="account-level-policies"></a>Beleid account-niveau

Deze beleidsregels van toepassing op alle taken in een Data Lake Analytics-account.

### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Maximum aantal AUs in een Data Lake Analytics-account
Een beleid bepaalt het totale aantal Analytics-eenheden (AUs) uw Data Lake Analytics-account kunt gebruiken. De waarde is standaard ingesteld op 250. Bijvoorbeeld, als deze waarde is ingesteld op 250 AUs, u kunt één actieve taak hebben met 250 AUs toegewezen aan of 10 taken uitgevoerd met 25 AUs elke. Aanvullende taken die zijn ingediend in de wachtrij staan totdat de actieve taken zijn voltooid. Wanneer taken zijn voltooid, worden de AUs vrijgemaakt voor de taken in de wachtrij om uit te voeren.

Het aantal AUs voor uw Data Lake Analytics-account wijzigen:

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **maximale AUs**, verplaats de schuifregelaar om een waarde te selecteren of Voer de waarde in het tekstvak. 
4. Klik op **Opslaan**.

> [!NOTE]
> Als u meer dan de standaardwaarde (250) AUs, in de portal klikt u op **Help + ondersteuning** ondersteuning te vragen. Het aantal AUs beschikbaar in uw Data Lake Analytics-account kan worden verhoogd.
>

### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Maximum aantal taken dat tegelijkertijd kan worden uitgevoerd
Een beleid bepaalt hoeveel taken kunnen uitvoeren op hetzelfde moment. Deze waarde is standaard ingesteld op 20. Als uw Data Lake Analytics AUs beschikbaar heeft, worden nieuwe taken worden gepland onmiddellijk totdat de waarde van dit beleid wordt bereikt door het totale aantal actieve taken uitvoeren. Wanneer u het maximum aantal taken dat tegelijkertijd kan worden uitgevoerd bereikt, latere taken in de wachtrij staan in volgorde van prioriteit als een of meer actieve taken hebt voltooid (afhankelijk van de beschikbaarheid van Australië).

Wijzigen van het aantal taken dat tegelijkertijd kan worden uitgevoerd:

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **Maximum aantal van een actief taken**, verplaats de schuifregelaar om een waarde te selecteren of Voer de waarde in het tekstvak. 
4. Klik op **Opslaan**.

> [!NOTE]
> Als u meer dan de standaardwaarde (20) aantal taken in de portal wordt uitgevoerd, klikt u op **Help + ondersteuning** ondersteuning te vragen. Het aantal taken dat tegelijkertijd kan worden uitgevoerd in uw Data Lake Analytics-account kan worden verhoogd.
>

### <a name="how-long-to-keep-job-metadata-and-resources"></a>Hoe lang tot behouden taak metagegevens en -bronnen 
Wanneer uw gebruikers U-SQL-taken uitvoeren, behoudt de Data Lake Analytics-service alle gerelateerde bestanden. Verwante bestanden bevatten het U-SQL-script de dll-bestanden waarnaar wordt verwezen in de U-SQL-script, gecompileerde bronnen en statistieken. De bestanden zijn in de map /system/ van het Azure Data Lake Storage-standaardaccount. Dit beleid bepaalt hoe lang deze resources worden opgeslagen voordat ze automatisch worden verwijderd (de standaardwaarde is 30 dagen). U kunt deze bestanden gebruiken voor foutopsporing en prestaties afstemmen van taken die u in de toekomst opnieuw zult uitvoeren.

Wijzigen hoe lang om metagegevens van de taak en resources te houden:

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **dagen bewaren taak voert een query**, verplaats de schuifregelaar om een waarde te selecteren of Voer de waarde in het tekstvak.  
4. Klik op **Opslaan**.

## <a name="job-level-policies"></a>Beleid Job-niveau

U kunt de maximale AUs en de maximumprioriteit individuele gebruikers (of leden van bepaalde beveiligingsgroepen) kunnen worden ingesteld voor taken die ze verzenden beheren met beleid voor taak-niveau. Dit beleid kunt u de kosten van gebruikers te beheren. U kunt ook beheer mogelijk hebt u het effect dat taken geplande hoge prioriteit productietaken die worden uitgevoerd in dezelfde Data Lake Analytics-account.

Data Lake Analytics heeft twee beleidsregels die u op het taakniveau van de instellen kunt:

* **Australië limiet is per taak**: gebruikers kunnen alleen taken die met dit aantal AUs indienen. Deze limiet is standaard hetzelfde zijn als de maximumlimiet Australië voor het account.
* **Prioriteit**: gebruikers kunnen alleen verzenden van taken met de prioriteit die lager is dan of gelijk zijn aan deze waarde. Een hoger getal geeft een lagere prioriteit. Standaard is deze limiet ingesteld op 1, die de hoogst mogelijke prioriteit is.

Er is een standaardbeleid instellen voor elke account. Het standaardbeleid is van toepassing op alle gebruikers van het account. U kunt extra beleidsregels instellen voor specifieke gebruikers en groepen. 

> [!NOTE]
> Beleidsregels voor account-niveau en op jobniveau toepassing tegelijk.
>

### <a name="add-a-policy-for-a-specific-user-or-group"></a>Een beleid voor een specifieke gebruiker of groep toevoegen

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **taak verzending limieten**, klikt u op de **beleid toevoegen** knop. Vervolgens, selecteer of Voer de volgende instellingen:
    1. **De naam van beleid COMPUTE**: Voer een beleidsnaam van het doel van het beleid.
    2. **Gebruiker of groep selecteren**: Selecteer de gebruiker of groep die dit beleid is van toepassing op.
    3. **De limiet voor de taak Australië instellen**: limiet voor de instellen Australië die voor de geselecteerde gebruiker of groep geldt.
    4. **De limiet van de prioriteit instellen**: de limiet voor de prioriteit die voor de geselecteerde gebruiker of groep geldt instellen.

4. Klik op **OK**.

5. Het nieuwe beleid wordt weergegeven in de **standaard** beleid tabel onder **taak verzending limieten**. 

### <a name="delete-or-edit-an-existing-policy"></a>Verwijder of een bestaand beleid bewerken

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **taak verzending limieten**, vinden van het beleid dat u wilt bewerken.
4.  Om te zien de **verwijderen** en **bewerken** opties in de rechterkolom van de tabel, klikt u op `...`.

## <a name="additional-resources-for-job-policies"></a>Aanvullende bronnen voor taak-beleid
* [Blogbericht beleid-overzicht](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Blogbericht beleid account-niveau](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Blogbericht beleid Job-niveau](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Data Lake Analytics met behulp van de Azure-portal](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics beheren met behulp van Azure PowerShell](data-lake-analytics-manage-use-powershell.md)

