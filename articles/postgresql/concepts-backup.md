---
title: Back-up en herstel in de Azure-Database voor PostgreSQL
description: Meer informatie over automatische back-ups en terugzetten van uw Azure-Database voor PostgreSQL-server.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 0f7ec38d2c271ebaa15e681a71eb32be7151921f
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29693359"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql"></a>Back-up en herstel in de Azure-Database voor PostgreSQL

Azure PostgreSQL-Database maakt back-ups server en slaat ze op in de gebruiker geconfigureerde lokaal redundante of geografisch redundante opslag automatisch. Back-ups kunnen worden gebruikt voor uw server herstellen naar een punt in tijd. Back-up en herstelbewerkingen zijn een essentieel onderdeel van een strategie voor zakelijke continuïteit omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen.

## <a name="backups"></a>Back-ups

Azure PostgreSQL-Database heeft volledige en differentiële transactielogboekback-ups. Deze back-ups kunnen u een server naar een punt in tijd binnen de geconfigureerde bewaartermijn voor back-up herstellen. De bewaartermijn voor back-up is zeven dagen. U kunt deze desgewenst configureren tot 35 dagen. Alle back-ups zijn versleuteld met AES 256-bits versleuteling.

### <a name="backup-frequency"></a>Back-upfrequentie

In het algemeen volledige back-up wekelijks, differentiële back-ups twee keer per dag, en plaatsvinden transactielogboekback-ups om de vijf minuten. De eerste volledige back-up is gepland onmiddellijk nadat een server is gemaakt. De eerste back-up kan op een grote herstelde server langer duren. Het eerste punt in tijd die een nieuwe server kan worden hersteld is de tijd waarop de eerste volledige back-up voltooid is.

### <a name="backup-redundancy-options"></a>Redundantieopties voor van de back-up

Azure PostgreSQL-Database biedt de flexibiliteit om te kiezen tussen lokaal redundante of geografisch redundante back-upopslag in de categorieën algemene doeleinden en geoptimaliseerd voor geheugen. Wanneer de back-ups worden opgeslagen in een geografisch redundante back-upopslag, ze worden niet alleen opgeslagen in de regio waarin uw server wordt gehost, maar ook worden gerepliceerd naar een [gekoppeld Datacenter](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Dit biedt betere beveiliging en uw server in een andere regio herstellen in geval van een noodgeval. De laag Basic biedt alleen lokaal redundante opslag van de back-up.

> [!IMPORTANT]
> Configureren van lokaal redundante of geografisch redundante opslag voor back-up is alleen toegestaan tijdens server maken. Zodra de server is geconfigureerd, kunt u de back-upopslag redundantie-optie niet wijzigen.

### <a name="backup-storage-cost"></a>Back-up opslagkosten

Azure PostgreSQL-Database biedt tot wel 100% van uw serveropslag ingerichte als back-upopslag zonder extra kosten. Dit is meestal geschikt is voor een back-up bewaren van zeven dagen. Alle aanvullende back-opslag is in rekening gebracht per GB maand.

Bijvoorbeeld, als u een server met 250 GB hebt ingericht, hebt u 250 GB aan back-upopslag zonder extra kosten. Opslag die groter zijn dan 250 GB in rekening wordt gebracht.

## <a name="restore"></a>Herstellen

In Azure-Database voor PostgreSQL maakt uitvoeren van een herstelpunt een nieuwe server vanaf de oorspronkelijke server back-ups.

Er zijn twee typen van terugzetten:

- **Punt in tijd terugzetten** is beschikbaar in eender welke optie back-redundantie en maakt u een nieuwe server in dezelfde regio bevinden als de oorspronkelijke server.
- **Geo-restore** is alleen beschikbaar als u uw server voor geografisch redundante opslag hebt geconfigureerd en Hiermee kunt u uw server herstellen naar een andere regio.

De geschatte tijd van het herstel is afhankelijk van verschillende factoren, waaronder de grootte van de database, de grootte van het transactielogboek, de netwerkbandbreedte en het totale aantal databases herstellen in dezelfde regio op hetzelfde moment. De hersteltijd is meestal minder dan 12 uur.

> [!IMPORTANT]
> Als u de server verwijdert, worden alle databases die deel uitmaken van de server worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.

### <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip

Onafhankelijk van de redundantie van de back-optie kunt u een terugzetbewerking naar een willekeurig punt in tijd uitvoeren binnen de bewaarperiode van uw back-up. Een nieuwe server wordt gemaakt in dezelfde Azure-regio als de oorspronkelijke server. Deze is gemaakt met de oorspronkelijke configuratie van de server voor de prijscategorie, compute genereren, aantal vCores, opslaggrootte, back-up bewaarperiode en redundantie van de back-optie.

Punt in tijd terugzetten is nuttig in scenario's met meerdere. Bijvoorbeeld, wanneer een gebruiker per ongeluk gegevens verwijdert, verwijdert een belangrijke tabel of de database, of als een toepassing per ongeluk de onbeschadigde gegevens wordt overschreven door ongeldige gegevens als gevolg van een toepassing defect.

Mogelijk moet u wachten op de volgende transactielogboek moeten worden genomen voordat u naar een punt in tijd binnen de laatste vijf minuten herstellen kunt.

### <a name="geo-restore"></a>Geo-herstel

Als u uw server voor geografisch redundante back-ups hebt geconfigureerd, kunt u een server herstellen naar een andere Azure-regio waar de service beschikbaar is. Geo-herstel is de standaardoptie voor herstel wanneer de server is niet beschikbaar vanwege een incident in de regio waar de server wordt gehost. Als een grootschalige incident in een regio, worden niet beschikbaar zijn van uw databasetoepassing, kunt u een server uit de geografisch redundante back-ups herstellen naar een server in een andere regio. Er is een vertraging tussen wanneer een back-up is gemaakt en wanneer deze worden gerepliceerd naar andere regio. Deze vertraging mag maximaal een uur, dus als er een ramp optreedt, kan er van één uur gegevens verloren gaan.

### <a name="perform-post-restore-tasks"></a>Voer taken na herstellen

Na het terugzetten van een mechanisme voor herstel, u moet de volgende taken uitvoeren om gebruikers en toepassingen weer actief:

- Als de nieuwe server is bedoeld om de oorspronkelijke server vervangen, omleiden clients en clienttoepassingen naar de nieuwe server
- Zorg ervoor dat de juiste serverniveau firewallregels zijn voor gebruikers verbinding kunnen maken
- Zorg ervoor dat de juiste aanmeldingen en niveau databasemachtigingen is voldaan
- Waarschuwingen configureren, indien van toepassing

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over zakelijke continuïteit, de [business continuity overview](concepts-business-continuity.md).
- Als u wilt herstellen naar een punt in tijd met de Azure portal, Zie [database herstellen naar een tijdstip met de Azure portal](howto-restore-server-portal.md).
- Als u wilt herstellen naar een punt in tijd met Azure CLI, Zie [-database herstellen naar een punt in tijd met CLI](howto-restore-server-cli.md).