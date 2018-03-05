---
title: "Overzicht van zakelijke continuïteit met Azure-Database voor MySQL"
description: "Overzicht van zakelijke continuïteit met Azure-Database voor MySQL."
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: MySQL
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 7961da65abacf6058b4b0ece4e9cbe7b8d1ef254
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="overview-of-business-continuity-with-azure-database-for-mysql"></a>Overzicht van zakelijke continuïteit met Azure-Database voor MySQL

Dit overzicht beschrijft de mogelijkheden die Azure-Database voor MySQL voor bedrijfscontinuïteit en noodherstel biedt. Meer informatie over opties voor het herstellen van verstoren gebeurtenissen die kunnen leiden tot gegevensverlies van of ervoor zorgen dat uw database en de toepassing niet meer beschikbaar. Meer informatie over wat te doen wanneer een gebruiker of toepassing de fout is van invloed op de integriteit van gegevens, een Azure-regio een storing is of uw toepassing onderhoud vereist.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>Functies die u gebruiken kunt voor bedrijfscontinuïteit

Azure MySQL-Database biedt zakelijke continuïteit functies die zijn onder andere automatische back-ups en de mogelijkheid voor gebruikers voor het initiëren van geo-herstel. Elk heeft verschillende kenmerken voor geschatte herstel tijd (invoegen) en mogelijk gegevensverlies. Zodra u deze opties begrijpt, kunt u kiezen tussen deze en ze samen gebruiken voor verschillende scenario's. Tijdens het ontwikkelen van uw bedrijfscontinuïteitsplan, moet u de maximale toegestane tijd voordat de toepassing volledig hersteld na de gebeurtenis verstoren - dit uw herstel tijd Objective (RTO is) begrijpen. U moet ook de maximale hoeveelheid recente gegevens begrijpen updates (tijdsinterval) de toepassing kan tolereren verliezen tijdens het herstellen van na de gebeurtenis verstoren - dit is uw Recovery Point Objective (RPO).

De volgende tabel vergelijkt de invoegen en RPO voor de beschikbare functies:

| **Mogelijkheid tot** | **Basic** | **Voor algemene doeleinden** | **Geoptimaliseerd geheugen** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Herstel naar een bepaald tijdstip vanuit back-up | Een herstelpunt binnen de bewaarperiode | Een herstelpunt binnen de bewaarperiode | Een herstelpunt binnen de bewaarperiode |
| Geo-herstel van back-ups geo-replicatie | Niet ondersteund | Invoegen < 12 uur<br/>RPO < 1 uur | Invoegen < 12 uur<br/>RPO < 1 uur |

> [!IMPORTANT]
> Als u de server verwijdert, worden alle databases die deel uitmaken van de server worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Een server herstellen na een fout van gebruiker of toepassing

U kunt back-ups van de service gebruiken voor het herstellen van een server uit diverse gebeurtenissen op verstoren. Een gebruiker mogelijk per ongeluk verwijdert sommige gegevens, onbedoeld verwijderen van een belangrijke tabel of zelfs een volledige database verwijderen. Een toepassing mogelijk per ongeluk de onbeschadigde gegevens overschrijven met beschadigde gegevens als gevolg van een toepassing defect, enzovoort.

U kunt een punt-in-time-herstel uitvoeren voor het maken van een kopie van uw server naar een bekende goede punt in tijd. Dit punt in tijd moet binnen de Backup-bewaartermijn die u voor uw server hebt geconfigureerd. Nadat de gegevens is hersteld naar de nieuwe server, kunt u de oorspronkelijke server vervangen door de herstelde server of kopieer de benodigde gegevens van de herstelde server in de oorspronkelijke server.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Herstellen van een storing van Azure regionale data center

Hoewel zeldzaam, kan er een storing optreden in een Azure-datacenter. Wanneer er een storing optreedt, wordt er een onderbreking van de bedrijven die mogelijk slechts een paar minuten laatste, maar uur kan duren.

Een mogelijkheid is om te wachten op de server weer online te krijgen als het datacentrum op. Dit werkt voor toepassingen die toestaan van de server offline zijn gedurende een bepaalde tijd, bijvoorbeeld een ontwikkelomgeving. Wanneer Datacenter een storing heeft, weet u niet hoe lang de storing kan duren, zodat u deze optie werkt alleen als u uw server voor een tijdje niet nodig.

De andere mogelijkheid is het gebruik van de Azure-Database voor de MySQL geo-herstel functie die de server met geografisch redundante back-ups worden hersteld. Deze back-ups zijn toegankelijk, zelfs wanneer de regio die wordt gehost door de server offline is. U kunt deze back-ups terugzetten naar een andere regio en de server weer online brengen.

> [!IMPORTANT]
> Geo-herstel is alleen mogelijk als u de server met geografisch redundante back-upopslag ingericht.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de geautomatiseerde back-ups, [back-ups in Azure-Database voor MySQL](concepts-backup.md).
- Als u wilt herstellen naar een punt in tijd met de Azure portal, Zie [database herstellen naar een tijdstip met de Azure portal](howto-restore-server-portal.md).
- Als u wilt herstellen naar een punt in tijd met Azure CLI, Zie [-database herstellen naar een punt in tijd met CLI](howto-restore-server-cli.md).