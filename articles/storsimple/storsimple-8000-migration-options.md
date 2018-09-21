---
title: Het evalueren van opties voor het migreren van gegevens uit StorSimple 5000-7000 serie | Microsoft Docs
description: Bevat een overzicht van de opties voor het migreren van gegevens uit de StorSimple 5000-7000-serie.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2018
ms.author: alkohli
ms.openlocfilehash: 5da67b5141eb61823d3e376b6f0e6b0b3895ca68
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498280"
---
# <a name="options-to-migrate-data-from-storsimple-5000-7000-series"></a>Opties voor het migreren van gegevens uit StorSimple 5000-7000 serie 

> [!IMPORTANT]
> Op 31 juli 2019 bereiken de StorSimple 5000/7000-serie het einde van de ondersteuningstatus (EOS). U wordt aangeraden dat de StorSimple 5000/7000-serie klanten naar een van de alternatieven zoals beschreven in het document migreren.

StorSimple 5000-7000 serie bereikt [einde van ondersteuning](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) in juli 2019. De klanten met een optie voor het upgraden naar andere Azure StorSimple 5000-7000-serie zijn eigen hybride services. Dit artikel beschrijft de Azure hybrid-opties die beschikbaar zijn om gegevens te migreren. 

## <a name="migration-options"></a>Opties voor de migratie

Klanten met behulp van de StorSimple hebben 5000-7000-serie de volgende twee opties:

- **Een upgrade uitvoeren naar de StorSimple 8000-serie** : upgrade uitvoeren naar de StorSimple 8000-serie en blijven op het StorSimple-platform.  Dit upgradepad moet klanten hun apparaten uit de 5000-7000 serie vervangen door een 8000-serie. De gegevens is van het apparaat uit de 5000-7000 serie gemigreerd met behulp van het hulpprogramma voor migratie. Zodra de migratie is voltooid is, blijven de apparaten van de StorSimple 8000-serie laag gegevens naar Azure Blob Storage. 

    Voor meer informatie over het migreren van gegevens met behulp van een StorSimple 8000-serie, gaat u naar [migreren van gegevens uit de StorSimple 5000-7000 serie 8000-serie apparaat](storsimple-8000-migrate-from-5000-7000.md).

- **Migreren naar Azure File Sync** – dit nieuwe migratieoptie kan klanten voor het opslaan van bestandsshares van hun organisatie in de Azure-bestanden. Deze bestandsshares worden vervolgens centraal voor on-premises toegang met behulp van Azure File Sync (AFS). AFS kan worden geïmplementeerd op een Windows Server-host. De migratie van de werkelijke gegevens wordt vervolgens uitgevoerd als een host worden uitgevoerd of met behulp van het hulpprogramma voor migratie.

    Voor meer informatie over het migreren van gegevens naar Azure File Sync, gaat u naar [gegevens migreren van StorSimple 5000-7000 serie naar Azure File Sync](https://aka.ms/StorSimpleMigrationAFS).

## <a name="migration---frequently-asked-questions"></a>Migratie - Veelgestelde vragen

### <a name="q-when-do-the-storsimple-5000-and-7000-series-devices-reach-end-of-service"></a>V. Wanneer de StorSimple 5000- en 7000-serie apparaten bereiken het einde van de service? 

A. StorSimple 5000-7000 serie bereiken [einde van de service](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) in juli 2019. Het einde van de service houdt in dat Microsoft niet langer ondersteuning bieden voor zowel hardware en software van deze apparaten na juli 2019. Het is raadzaam dat u start het formuleren van een abonnement nu migreren van de gegevens van uw apparaten.

### <a name="q-what-happens-to-the-data-i-have-stored-in-azure"></a>V. Wat gebeurt er met de gegevens die ik hebt opgeslagen in Azure?  

A. U kunt echter ook doorgaan met de gegevens in Azure bij het migreren naar een nieuwere service. 


### <a name="q--what-happens-to-the-data-i-have-stored-locally-on-my-storsimple-device"></a>V.  Wat gebeurt er met de gegevens die ik lokaal hebt opgeslagen op het StorSimple-apparaat? 

A. De gegevens die zich op het lokale apparaat kunnen worden gekopieerd naar de nieuwere service zoals beschreven in de migratie-documenten.

### <a name="what-happens-if-i-want-to-keep-my-storsimple-50007000-series-appliance"></a>Wat gebeurt er als ik wil mijn StorSimple 5000/7000-serie-apparaat? 

A. Terwijl de services werken blijven kunnen, is Microsoft niet langer kunnen bieden ondersteuning voor hardware en software. Migratie wordt aanbevolen voor bedrijfscontinuïteit.

### <a name="q-what-options-are-available-to-migrate-data-from-storsimple-5000-7000-series-devices"></a>V. Welke opties zijn beschikbaar voor het migreren van gegevens uit StorSimple 5000-7000 serie apparaten? 

A. StorSimple 5000-7000-serie gebruikers hebben, afhankelijk van hun scenario, de volgende opties voor de migratie. 

 - **Een upgrade uitvoeren naar de 8000-serie**: Gebruik deze optie als u wilt doorgaan met de StorSimple-platform. 
 - **Migreren naar Azure File Sync**: Gebruik deze optie als u wilt overschakelen naar de systeemeigen Azure-indeling. U kunt Azure File Sync gebruiken voor gecentraliseerd beheer van bestandsshares. 

U kunt contact opnemen met Microsoft Support om te bespreken migratieopties hier niet vermeld.

### <a name="q-is-migration-to-other-storage-solutions-supported"></a>V. Migratie op andere oplossingen voor opslag wordt ondersteund is?

A. Ja. Migratie naar andere opslagoplossingen voor via het kopiëren van de host van de gegevens wordt ondersteund.

### <a name="q-is-migration-supported-by-microsoft"></a>V. Wordt migratie ondersteund door Microsoft? 

A. Migreren van 5000 of 7000-serie is een volledig ondersteunde bewerking. Microsoft raadt in feite contact opnemen met ondersteuning, voordat u de migratie start. Migratie wordt momenteel een bewerking voor assistentie. Als u wilt migreren van gegevens uit de StorSimple 5000-7000 serie apparaat [Open een ondersteuningsticket](storsimple-8000-contact-microsoft-support.md).

### <a name="q-what-is-the-pricing-model-for-both-the-migration-options"></a>V. Wat is het prijsmodel voor zowel de opties voor de migratie?

A. Kosten van de migratie is afhankelijk van welke optie die u kiest. Tijdens de migratie zelf is gratis, als u besluit om te upgraden naar een StorSimple 8000-serie, zullen er de kosten van het apparaat. 

Op dezelfde manier als u Azure File Sync, kunnen de abonnementskosten voor de service toepassing. In elk geval wordt ook klanten hebben om te betalen lopende opslagkosten. Raadpleeg de volgende onderwerpen voor een schatting te maken: 
- [Prijzen voor StorSimple](https://azure.microsoft.com/pricing/details/storsimple/)  
- [AFS-prijzen]( https://azure.microsoft.com/pricing/details/storage/files/)

### <a name="q--how-long-does-it-take-to-complete-a-migration"></a>V.  Hoe lang duurt het om een migratie te voltooien?

A. De tijd om gegevens te migreren, is afhankelijk van de hoeveelheid gegevens en de upgrade-optie geselecteerd. 

### <a name="q-what-is-the-end-of-support-date-for-storsimple-8000-series"></a>V. Wat is de datum einde van ondersteuning voor StorSimple 8000-serie?

A. De datum einde van ondersteuning voor StorSimple 8000-serie is gepubliceerd [hier](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series).


## <a name="next-steps"></a>Volgende stappen
 - [Gegevens migreren van een StorSimple 5000-7000 serie naar een apparaat 8000-serie](storsimple-8000-migrate-from-5000-7000.md).
 - [Gegevens van een StorSimple 5000-7000-serie migreren naar Azure File Sync](storsimple-5000-7000-afs-migration.md)
