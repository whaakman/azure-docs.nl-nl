---
title: Back-up en herstel in Azure Database voor MariaDB
description: Meer informatie over automatische back-ups en herstellen van uw Azure Database voor MariaDB-server.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: e36e718d35f652c139958388491642e98469ecaf
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548838"
---
# <a name="backup-and-restore-in-azure-database-for-mariadb"></a>Back-up en herstel in Azure Database voor MariaDB

Azure Database voor MariaDB automatisch server back-ups gemaakt en opgeslagen in lokaal redundante of geografisch redundante opslag van gebruiker die is geconfigureerd. Back-ups kunnen worden gebruikt om uw server te herstellen naar een punt-in-time. Back-up en herstel zijn een essentieel onderdeel van een strategie voor bedrijfscontinuïteit omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen.

## <a name="backups"></a>Back-ups

Azure Database voor MariaDB heeft volledige, differentiële en transactielogboekback-ups. Deze back-ups kunnen u een server te herstellen naar een punt-in-time binnen de geconfigureerde bewaarperiode voor back-up. De bewaartermijn voor back-up is zeven dagen. U kunt deze desgewenst configureren van 35 dagen. Alle back-ups zijn versleuteld met AES 256-bits versleuteling.

### <a name="backup-frequency"></a>Back-upfrequentie

Over het algemeen volledige back-ups uitgevoerd wekelijks, differentiële back-ups zich twee keer per dag en transactielogboekback-ups per vijf minuten plaatsvindt. De eerste volledige back-up is gepland, onmiddellijk nadat een server is gemaakt. De eerste back-up kan op een grote herstelde server langer duren. Het oudste herstelpunt die een nieuwe server kan worden hersteld naar is de tijd waarop de eerste volledige back-up voltooid is.

### <a name="backup-redundancy-options"></a>Redundantieopties voor back-up

Azure Database voor MariaDB biedt de flexibiliteit om te kiezen tussen lokaal redundante of geografisch redundante back-upopslag in de lagen algemeen gebruik en geoptimaliseerd voor geheugen. Wanneer de back-ups worden opgeslagen in geografisch redundante back-upopslag, ze worden niet alleen opgeslagen in de regio waarin uw server wordt gehost, maar worden ook gerepliceerd naar een [gekoppeld Datacenter](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Dit biedt betere bescherming en de mogelijkheid om te herstellen van uw server in een andere regio in het geval van een noodgeval. De Basic-laag biedt alleen lokaal redundante back-upopslag.

> [!IMPORTANT]
> Configureren van lokaal redundante of geografisch redundante opslag voor back-up is alleen toegestaan tijdens de server maken. Zodra de server is ingericht, kunt u de back-upopslag-optie voor redundantie niet wijzigen.

### <a name="backup-storage-cost"></a>Kosten voor back-upopslag

Azure Database voor MariaDB biedt maximaal 100% van uw ingerichte serveropslag als back-upopslag zonder extra kosten. Dit is meestal geschikt is voor een back-upretentie van zeven dagen. Eventuele extra back-upopslag gebruikt, wordt in rekening gebracht in GB-maand.

Bijvoorbeeld, als u een server met 250 GB hebt ingericht, hebt u 250 GB aan back-upopslag zonder extra kosten. Upopslag groter dan 250 GB, wordt in rekening gebracht.

Voor meer informatie over de kosten voor back-upopslag, gaat u naar de [MariaDB pagina met prijzen](https://azure.microsoft.com/pricing/details/mariadb/).

## <a name="restore"></a>Herstellen

In Azure Database voor MariaDB maakt uitvoeren van een herstelpunt een nieuwe server van de back-ups van de oorspronkelijke server.

Er zijn twee soorten herstel beschikbaar:

- **Point-in-time restore** is beschikbaar met een van beide opties voor back-upredundantie en maakt u een nieuwe server in dezelfde regio als de oorspronkelijke server.
- **Geo-herstel** is alleen beschikbaar als u uw server voor geografisch redundante opslag hebt geconfigureerd en Hiermee kunt u uw server te herstellen naar een andere regio.

De geschatte tijd van het herstel is afhankelijk van diverse factoren, waaronder de grootte van de database, de transactielogboekgrootte bandbreedte van het netwerk en het totale aantal databases herstellen in dezelfde regio op hetzelfde moment. De hersteltijd is meestal minder dan 12 uur.

> [!IMPORTANT]
> Servers verwijderd **kan geen** worden hersteld. Als u de server verwijdert, worden alle databases die deel uitmaken van de server worden ook verwijderd en kunnen niet worden hersteld.

### <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip

Onafhankelijk van uw back-upredundantie-optie, kunt u herstellen naar een bepaald tijdstip uitvoeren binnen de retentieperiode van uw back-up. Een nieuwe server is gemaakt in dezelfde Azure-regio als de oorspronkelijke server. Deze is gemaakt met de configuratie van de oorspronkelijke server voor de prijscategorie, bewerkingen, aantal vCores, opslaggrootte bewaarperiode voor back- en back-up redundantieoptie voor compute.

Point-in-time-restore is handig in meerdere scenario's. Bijvoorbeeld, wanneer een gebruiker worden gegevens per ongeluk verwijderd, wordt een belangrijke tabel of de database, of als een toepassing per ongeluk goede gegevens met beschadigde gegevens vanwege een toepassingsfout overschrijven.

Mogelijk moet u de volgende transactielogboekback-up moeten worden genomen voordat u naar een tijdstip in de laatste vijf minuten herstellen kunt wachten.

### <a name="geo-restore"></a>Geo-herstel

Als u uw server voor geografisch redundante back-ups hebt geconfigureerd, kunt u een server herstellen naar een andere Azure-regio waar de service beschikbaar is. Geo-restore is de standaardoptie voor herstel wanneer uw server is niet beschikbaar vanwege een incident in de regio waar de server wordt gehost. Als een grootschalige incident in een regio, resulteert in niet-beschikbaarheid van uw databasetoepassing, kunt u een server uit de geografisch redundante back-ups herstellen naar een server in een andere regio. Er is een vertraging tussen wanneer een back-up is gemaakt en wanneer deze worden gerepliceerd naar andere regio. Deze vertraging mag maximaal een uur, dus als er zich een noodgeval voordoet, kan er van één uur gegevens verloren gaan.

Tijdens de geo-restore zijn de serverconfiguraties die kunnen worden gewijzigd bewerking voor compute, vCore, bewaarperiode voor back- en redundantieopties voor back-up. Wisselende prijscategorie (Basic, algemeen gebruik of geoptimaliseerd voor geheugen) of opslaggrootte tijdens de geo-restore wordt niet ondersteund.

### <a name="perform-post-restore-tasks"></a>Voer taken na herstellen

Na het terugzetten van beide herstelmechanismen, u moet de volgende taken uitvoeren om uw gebruikers en toepassingen weer actief:

- Als de nieuwe server is bedoeld om de oorspronkelijke server, clients en clienttoepassingen naar de nieuwe server omleiden
- Zorg ervoor dat de juiste firewallregels op serverniveau worden ingesteld voor gebruikers verbinding kunnen maken
- Zorg ervoor dat de juiste aanmeldingen en machtigingen op databaseniveau is voldaan
- Waarschuwingen configureren, indien van toepassing

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over bedrijfscontinuïteit, de [overzicht voor zakelijke continuïteit](concepts-business-continuity.md).
- Als u wilt herstellen naar een eerder tijdstip met behulp van de Azure-portal, Zie [database herstellen naar een eerder tijdstip met behulp van de Azure-portal](howto-restore-server-portal.md).
 
<!--
- To restore to a point in time using Azure CLI, see [restore database to a point in time using CLI](howto-restore-server-cli.md).-->
