---
title: Configureren en toegang tot server-logboeken voor PostgreSQL in Azure Portal | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt configureren en toegang tot de server-Logboeken in Azure-Database voor PostgreSQL vanuit de Azure Portal.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: a2f67b21293a1a0456b27cad9043be01fdd5274a
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Configureren en de logboeken van de toegangsserver in de Azure portal

U kunt configureren, weergeven en downloaden de [Azure Database voor de serverlogboeken PostgreSQL](concepts-server-logs.md) vanuit de Azure-portal.

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- [Azure-Database voor PostgreSQL-server](quickstart-create-server-database-portal.md)

## <a name="configure-logging"></a>Logboekregistratie configureren
Toegang tot de query en de foutenlogboeken configureren. 

1. Meld u aan bij [Azure Portal](http://portal.azure.com/).

2. Selecteer uw Azure-Database voor PostgreSQL-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. 

   ![Selecteer de Server-logboeken en selecteer 'Klik hier als u wilt inschakelen...'](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Selecteer de kop **Klik hier om Logboeken inschakelen en configureren van parameters voor het vastleggen** om te zien van de parameters van de server.

5. Selecteer de **meer** uitbreidingsmodule voor een uitgebreide lijst met beschikbare parameters. 

   Zie de PostgreSQL-documentatie voor meer informatie over de definities van de parameters op [foutrapportage en de logboekregistratie](https://www.postgresql.org/docs/current/static/runtime-config-logging.html).

   ![Overzicht van de parameters voor het vastleggen. Klik op weergeven meer voor lange](./media/howto-configure-server-logs-in-portal/2-show-more.png)

6. Wijzig de parameters die u nodig hebt om aan te passen. Alle wijzigingen die u in deze sessie aanbrengt worden in paars gemarkeerd.

   Zodra u de parameters hebt gewijzigd, klikt u op **opslaan**. U kunt **negeren** uw wijzigingen. 

   ![Lange lijst met parameters met wijzigingen op te slaan of te negeren](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

7. Terug naar de lijst met Logboeken door te klikken op de **knop Sluiten** (pictogram X) op de **serverparameters** pagina.

## <a name="view-list-and-download-logs"></a>Lijst weergeven en logboeken downloaden
Zodra logboekregistratie wordt gestart, kunt u een lijst met beschikbare logboeken weergeven en downloaden in het deelvenster logboeken van de Server een afzonderlijke logboekbestanden. 

1. Open Azure Portal.

2. Selecteer uw Azure-Database voor PostgreSQL-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. De pagina bevat een overzicht van uw logboekbestanden, zoals wordt weergegeven:

   ![Logboeken van serverlijst](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > De naamgevingsconventie van het logboek is **postgresql-jjjj-mm-dd_hh0000.log**. De datum en tijd gebruikt in de bestandsnaam is de tijd wanneer het logboek is uitgegeven. De logboekbestanden roteren elke één uur of de grootte van 100 MB, afhankelijk van wat zich het eerste voordoet.

4. Indien nodig, gebruiken de **zoekvak** snel beperken aan een specifieke logboek op basis van datum/tijd. De zoekopdracht is op de naam van het logboek.

   ![Voorbeeld van de zoekopdracht op logboeknamen](./media/howto-configure-server-logs-in-portal/5-search.png)

5. Downloaden van afzonderlijke logboekbestanden met behulp van de **downloaden** knop (omlaag pijlpictogram) naast elk logboekbestand in de tabelrij, zoals wordt weergegeven:

   ![Klik op het downloadpictogram](./media/howto-configure-server-logs-in-portal/6-download.png)

## <a name="next-steps"></a>Volgende stappen
- Zie [Access Server-logboeken in CLI](howto-configure-server-logs-using-cli.md) voor informatie over het om Logboeken te downloaden via een programma.
- Meer informatie over [serverlogboeken](concepts-server-logs.md) in Azure DB voor PostgreSQL. 
- Zie de PostgreSQL-documentatie voor meer informatie over de parameterdefinities en PostgreSQL logboekregistratie op [Error Reporting en logboekregistratie](https://www.postgresql.org/docs/current/static/runtime-config-logging.html).

