---
title: Het herstellen van een Server in Azure-Database voor MySQL | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een server in Azure-Database herstelt voor MySQL met de Azure portal.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 6c1c0f8a0c0e59661b70b787b551b8cfdb024cda
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Het back-up en herstellen van een server in Azure-Database voor MySQL met behulp van de Azure-portal

## <a name="backup-happens-automatically"></a>Back-up automatisch wordt uitgevoerd
Wanneer u Azure-Database voor MySQL, wordt de database-service automatisch een back-up van de service om de vijf minuten. 

De back-ups zijn beschikbaar voor de zeven dagen bij gebruik van Basisstaffel en 35 dagen bij gebruik van de Standard-laag. Zie voor meer informatie [Azure Database voor de Servicelagen MySQL](concepts-service-tiers.md)

Met deze functie voor automatische back-up kan u de server en alle bijbehorende databases herstellen naar een nieuwe server aan een eerder punt in tijd.

## <a name="restore-in-the-azure-portal"></a>Herstellen in de Azure portal
Azure MySQL-Database kunt u de server herstellen naar een bepaald punt in tijd en in op een nieuwe kopie van de server. U kunt deze nieuwe server gebruiken om uw gegevens te herstellen. 

Bijvoorbeeld, als een tabel per ongeluk is verwijderd op twaalf uur 's middags vandaag kan u herstellen naar een tijd net vóór twaalf uur 's middags en de ontbrekende tabel en gegevens ophalen van de nieuwe kopie van de server.

De volgende stappen uit voor het herstellen van de voorbeeldserver naar een bepaald punt in tijd:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/)

2. Ga naar uw Azure-Database voor de MySQL-server. Selecteer in het linkerdeelvenster **alle resources**, en selecteer vervolgens uw server uit de lijst.

3.  Klik boven aan de overzichtsblade van server **herstellen** op de werkbalk. De Restore-blade wordt geopend.
![Klik op de knop herstellen](./media/howto-restore-server-portal/click-restore-button.png)

4. Vul het formulier terugzetten met de vereiste informatie in:

- **Herstelpunt (UTC)**: met de objectkiezer datum en tijd kiezen, selecteer een punt in tijd om naar te herstellen. De opgegeven tijd is in UTC-notatie, dus u waarschijnlijk moet worden de lokale tijd geconverteerd naar UTC.
- **Herstellen naar de nieuwe server**: Geef een nieuwe naam van de server om de bestaande server naar te herstellen.
- **Locatie**: de keuze regio automatisch gevuld met de server bron regio en kan niet worden gewijzigd.
- **Prijscategorie**: de prijscategorie gekozen voor de laag wordt automatisch gevuld met de dezelfde prijscategorie als de bronserver en kan hier worden gewijzigd. 
![PITR terugzetten](./media/howto-restore-server-portal/pitr-restore.png)

5. Klik op **OK** de server te herstellen naar het opgegeven punt in tijd. 

6. Nadat het herstel is voltooid, zoekt u de nieuwe server die is gemaakt en controleer vervolgens of de databases zijn teruggezet zoals verwacht.

## <a name="next-steps"></a>Volgende stappen
- [Verbindingsbibliotheken voor Azure-Database voor MySQL](concepts-connection-libraries.md).