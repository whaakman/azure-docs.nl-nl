---
title: Overzicht van bedrijfscontinuïteit met Azure Database for PostgreSQL
description: Overzicht van bedrijfscontinuïteit met Azure Database for PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: b1d566ac571ddd2b2be3aff160f669e277887209
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55698216"
---
# <a name="overview-of-business-continuity-with-azure-database-for-postgresql"></a>Overzicht van bedrijfscontinuïteit met Azure Database for PostgreSQL

Dit overzicht beschrijft de mogelijkheden die Azure Database voor PostgreSQL voor bedrijfscontinuïteit en herstel na noodgevallen biedt. Meer informatie over opties voor het herstellen van storingen die kunnen leiden tot gegevensverlies of ervoor zorgen dat uw databases en toepassingen niet meer beschikbaar zijn. Meer informatie over wat er gebeurt wanneer een gebruiker of toepassing-fout is van invloed op de integriteit van gegevens, een Azure-regio een storing heeft of uw toepassing onderhoud vereist.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>Functies die u gebruiken kunt voor bedrijfscontinuïteit

Azure Database voor PostgreSQL biedt functies voor bedrijfscontinuïteit, zoals geautomatiseerde back-ups en de mogelijkheid voor gebruikers voor het starten van geo-herstel. Elke functie heeft verschillende kenmerken voor geschatte Recovery tijd (ERT) en verlies van gegevens. Wanneer u bekend bent met deze opties, kunt u kiezen tussen deze en samen gebruiken voor verschillende scenario's. Tijdens het ontwikkelen van uw plan voor bedrijfscontinuïteit, moet u inzicht in de maximaal acceptabele tijd voordat de toepassing volledig is hersteld na de storing: dit uw Recovery Time Objective (RTO is). U moet ook weten wat de maximale hoeveelheid recente gegevens updates (tijdsinterval) de toepassing kan tolereren verliezen tijdens het herstellen na de storing: dit is uw Recovery Point Objective (RPO).

De volgende tabel worden de ERT en RPO vergeleken voor de beschikbare functies:

| **Mogelijkheid** | **Basic** | **Algemeen gebruik** | **Geoptimaliseerd geheugen** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Herstel naar een bepaald tijdstip vanuit back-up | Willekeurig herstelpunt binnen de bewaarperiode liggen | Willekeurig herstelpunt binnen de bewaarperiode liggen | Willekeurig herstelpunt binnen de bewaarperiode liggen |
| Geo-herstellen vanaf back-ups via geo-replicatie | Niet ondersteund | ERT < 12 u<br/>RPO < 1 uur | ERT < 12 u<br/>RPO < 1 uur |

> [!IMPORTANT]
> Servers verwijderd **kan geen** worden hersteld. Als u de server verwijdert, worden alle databases die deel uitmaken van de server worden ook verwijderd en kunnen niet worden hersteld.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Een server herstellen na een fout van gebruiker of toepassing

Back-ups van de service kunt u een server herstellen na diverse storingen. Een gebruiker mogelijk per ongeluk bepaalde gegevens verwijderen, per ongeluk een belangrijke tabel wissen of zelfs een volledige database verwijderen. Een toepassing mogelijk per ongeluk goede gegevens overschrijven met beschadigde gegevens vanwege een toepassingsfout, enzovoort.

U kunt een punt-in-time-restore voor het maken van een kopie van uw server naar een bekend goed punt in tijd kan uitvoeren. Dit punt in tijd moet binnen de back-up bewaartermijn die u hebt geconfigureerd voor uw server. Nadat de gegevens naar de nieuwe server is hersteld, kunt u de oorspronkelijke server vervangen door de herstelde server of de benodigde gegevens van de herstelde server kopiëren naar de oorspronkelijke server.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Herstellen van een Azure regionale datacenterstoring

Hoewel zeldzaam, kan er een storing optreden in een Azure-datacenter. Wanneer een storing optreedt, veroorzaakt deze een bedrijfsonderbreking die slechts een paar minuten kan duren, maar uren kan duren.

Een optie is na afloop van de server weer online komt wanneer de storing in het datacenter afgelopen is. Dit werkt voor toepassingen die niet erg is dat de server offline halen voor een bepaalde tijd, bijvoorbeeld een ontwikkelomgeving. Wanneer het datacenter een storing heeft, weet u niet hoe lang de storing kan duren, zodat u deze optie werkt alleen als u uw server even niet nodig.

De andere optie is het gebruik van de Azure Database voor PostgreSQL van geo-restore-functie waarmee de server met behulp van geografisch redundante back-ups wordt hersteld. Deze back-ups zijn toegankelijk, zelfs wanneer de regio waarin die uw server wordt gehost in offline. U kunt herstellen vanuit deze back-ups naar elke andere regio en de server weer online brengen.

> [!IMPORTANT]
> Geo-herstel is alleen mogelijk als u de server met geografisch redundante back-upopslag hebt ingericht. Als u overschakelen van lokaal redundante naar geografisch redundante back-ups voor een bestaande server wilt, moet u een dump pg_dump van uw bestaande server met nemen en terug te zetten een nieuw gemaakte geconfigureerd met geografisch redundante back-ups.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de [automatische back-ups in Azure Database voor PostgreSQL](concepts-backup.md). 
- Meer informatie over het herstellen met behulp van [de Azure-portal](howto-restore-server-portal.md) of [de Azure CLI](howto-restore-server-cli.md).
- Meer informatie over [lezen-replica's in Azure Database for PostgreSQL](concepts-read-replicas.md).