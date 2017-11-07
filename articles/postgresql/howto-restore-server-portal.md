---
title: Het herstellen van een Server in Azure-Database voor PostgreSQL | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een server in Azure-Database herstelt voor PostgreSQL met de Azure portal.
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: 903fd2ff446e1963ab5cfcec745766188b74efcf
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Het back-up en herstellen van een server in Azure-Database voor PostgreSQL met de Azure portal

## <a name="backup-happens-automatically"></a>Back-up automatisch wordt uitgevoerd
Wanneer u Azure-Database voor PostgreSQL, wordt de database-service automatisch een back-up van de server om de 5 minuten. 

De back-ups zijn beschikbaar voor de zeven dagen bij gebruik van Basisstaffel en 35 dagen bij gebruik van de Standard-laag. Zie voor meer informatie [Azure Database voor de Servicelagen PostgreSQL](concepts-service-tiers.md)

Met deze functie voor automatische back-up kan u de server en alle bijbehorende databases herstellen naar een nieuwe server aan een eerder punt in tijd.

## <a name="restore-in-the-azure-portal"></a>Herstellen in de Azure portal
Azure PostgreSQL-Database kunt u de server weer terugzetten naar een punt in tijd en in op een nieuwe kopie van de server. U kunt deze nieuwe server gebruiken om uw gegevens te herstellen. 

Bijvoorbeeld, als een tabel per ongeluk is kan verwijderd op twaalf uur 's middags vandaag de dag u herstellen en de tijd net vóór twaalf uur 's middags en ophalen van de ontbrekende tabel en de gegevens van die nieuwe kopie van de server.

De volgende stappen uit voor het herstellen van de voorbeeldserver naar een punt in tijd:
1. Meld u aan bij de [Azure-portal](https://portal.azure.com/)
2. Ga naar uw Azure-Database voor PostgreSQL-server. Klik in de Azure-portal op **alle Resources** uit het menu links en typ de naam van de server, zoals **mypgserver 20170401**, om te zoeken naar de bestaande server. Klik op de servernaam in de zoekresultaten. De pagina **Overzicht** wordt geopend voor uw server en biedt opties voor verdere configuratie.

   ![Azure portal - zoekt u naar de server](media/postgresql-howto-restore-server-portal/1-locate.png)

3. Klik op de werkbalk van de pagina overzicht van de server, **herstellen**. De pagina voor herstel wordt geopend.

   ![Azure-Database voor herstel PostgreSQL - overzicht - knop](./media/postgresql-howto-restore-server-portal/2_server.png)

4. Vul het formulier terugzetten met de vereiste informatie in:

   ![Azure-Database voor PostgreSQL - informatie terugzetten ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - **Herstelpunt**: Selecteer een point-in-time die deze gebeurtenis treedt op voordat de server is gewijzigd.
  - **Doelserver**: Geef een nieuwe servernaam die u wilt herstellen.
  - **Locatie**: U kunt de regio niet selecteren. Het is standaard hetzelfde als de bronserver.
  - **Prijscategorie**: U kunt deze waarde niet wijzigen bij het herstellen van een server. Dit is hetzelfde als de bronserver. 

5. Klik op **OK** om de server te herstellen naar een punt in tijd te herstellen. 

6. Nadat het herstel is voltooid, zoek de nieuwe server die wordt gemaakt om te controleren of dat de gegevens is hersteld, zoals verwacht.

## <a name="next-steps"></a>Volgende stappen
- [Verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md)
