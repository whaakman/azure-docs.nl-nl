---
title: Het herstellen van een Server in Azure-Database voor PostgreSQL
description: Dit artikel wordt beschreven hoe u een server in Azure-Database herstelt voor PostgreSQL met de Azure portal.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 04/01/2018
ms.openlocfilehash: 0d67bf5625ee9037c5ec152c8ce8564235018e8e
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Het back-up en herstellen van een server in Azure-Database voor PostgreSQL met de Azure portal

## <a name="backup-happens-automatically"></a>Back-up automatisch wordt uitgevoerd
Azure-Database voor PostgreSQL-servers zijn back-up periodiek Restore-functies inschakelen. Gebruik van deze functie kunt u de server en alle bijbehorende databases herstellen naar een eerdere point-in-time, op een nieuwe server.

## <a name="set-backup-configuration"></a>Back-upconfiguratie instellen

Maken van de keuze tussen uw server configureren voor lokaal redundante back-ups of geografisch redundante back-ups maken van de server, in de **prijscategorie** venster.

> [!NOTE]
> Nadat een server is gemaakt, wordt het soort redundantie dat zo is, kan geografisch redundante tegenover lokaal redundant kan niet worden overgeschakeld.
>

Tijdens het maken van een server via de Azure-portal, de **prijscategorie** venster is waar u een selecteren **lokaal redundante** of **geografisch redundante** back-ups voor uw server. Dit venster is ook waar u selecteert de **bewaarperiode back-up** : hoe lang (in dagen) u wilt dat de server back-ups voor opgeslagen.

   ![Prijscategorie - back-redundantie kiezen](./media/howto-restore-server-portal/pricing-tier.png)

Zie voor meer informatie over het instellen van deze waarden tijdens het maken, de [Azure Database voor PostgreSQL server Quick Start](quickstart-create-server-database-portal.md).

De bewaarperiode voor back-up van een server kan worden gewijzigd via de volgende stappen uit:
1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Selecteer uw Azure-Database voor PostgreSQL-server. Hiermee opent u deze actie de **overzicht** pagina.
3. Selecteer **prijscategorie** in het menu onder **instellingen**. Met behulp van de schuifregelaar kunt u de **back-up bewaarperiode** aan uw voorkeur tussen 7 en 35 dagen.
In de onderstaande schermafbeelding is het verhoogd tot 34 dagen.
![Back-up bewaarperiode verhoogd](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Klik op **OK** de wijziging te bevestigen.

De back-up bewaarperiode bepaalt hoe ver terug in tijd die een punt in tijd terugzetbewerking kan worden opgehaald, omdat deze gebaseerd op de back-ups beschikbaar. Punt in tijd restore wordt verder beschreven in de volgende sectie. 

## <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip
Azure PostgreSQL-Database kunt u de server terugzetten naar punt in tijd en in op een nieuwe kopie van de server. U kunt deze nieuwe server gebruiken om uw gegevens te herstellen of uw clienttoepassingen die verwijzen naar deze nieuwe server hebt.

Bijvoorbeeld, als een tabel per ongeluk is kan verwijderd op twaalf uur 's middags vandaag de dag u herstellen en de tijd net vóór twaalf uur 's middags en ophalen van de ontbrekende tabel en de gegevens van die nieuwe kopie van de server. Punt in tijd terugzetten is op de server niveau, niet op het databaseniveau van de.

De volgende stappen uit herstellen de voorbeeldserver naar een punt in tijd:
1. Selecteer uw Azure-Database voor PostgreSQL-server in de Azure-portal. 

2. Op de werkbalk van de server **overzicht** pagina **herstellen**.

   ![Azure-Database voor herstel PostgreSQL - overzicht - knop](./media/howto-restore-server-portal/2-server.png)

3. Vul het formulier terugzetten met de vereiste informatie in:

   ![Azure-Database voor PostgreSQL - informatie terugzetten ](./media/howto-restore-server-portal/3-restore.png)
  - **Herstelpunt**: Selecteer de point-in-time-u wilt herstellen.
  - **Doelserver**: Geef een naam op voor de nieuwe server.
  - **Locatie**: U kunt de regio niet selecteren. Het is standaard hetzelfde als de bronserver.
  - **Prijscategorie**: U kunt deze parameters niet wijzigen bij het uitvoeren van een punt in tijd te herstellen. Deze is hetzelfde als de bronserver. 

4. Klik op **OK** om de server om te zetten naar een punt in tijd te herstellen. 

5. Nadat het herstel is voltooid, zoek de nieuwe server die wordt gemaakt om te controleren of dat de gegevens is hersteld, zoals verwacht.

>[!Note]
>De nieuwe server gemaakt door punt in tijd herstel heeft de dezelfde server admin-aanmeldingsnaam en wachtwoord dat geldig voor de bestaande server op het punt in tijd is hebt gekozen. U kunt het wachtwoord wijzigen van de nieuwe server **overzicht** pagina.

## <a name="geo-restore"></a>Geo-herstel
Als u uw server voor geografisch redundante back-ups geconfigureerd, kan een nieuwe server worden gemaakt vanuit de back-up van een bestaande server. Deze nieuwe server kan worden gemaakt in elke regio dat Azure-Database voor PostgreSQL beschikbaar is.  

1. Selecteer de knop **Een resource maken** (+) in de linkerbovenhoek van de portal. Selecteer **Databases** > **Azure Database for PostgreSQL**.

   ![De optie Azure Database for PostgreSQL](./media/howto-restore-server-portal/1-navigate-to-postgres.png)

2. In het formulier **bron selecteren** vervolgkeuzelijst kiezen **back-up**. Met deze actie wordt een lijst met servers die geografisch redundante back-ups ingeschakeld hebben. Selecteer een van deze back-ups als de bron van de nieuwe server.
   ![Bron selecteren: Lijst van geografisch redundante back-ups en back-up](./media/howto-restore-server-portal/2-georestore.png)

3. Vul de rest van het formulier met uw voorkeuren. U kunt kiezen **locatie**. Na het selecteren van de locatie, kunt u **prijscategorie**. Standaard worden de parameters voor de bestaande server die vanwaar u herstelt weergegeven. U kunt klikken op **OK** zonder wijzigingen over te nemen die instellingen. Of u kunt wijzigen **Compute generatie** (indien beschikbaar in de regio u hebt gekozen), aantal **vCores**, **back-up bewaarperiode**, en **back-up Redundantie optie**. Het wijzigen van **prijscategorie** (Basic, algemeen of geoptimaliseerd voor geheugen) of **opslag** tijdens het terugzetten van de grootte wordt niet ondersteund.

>[!Note]
>De nieuwe server gemaakt door geografisch herstel heeft de dezelfde aanmeldingsnaam van server-beheerder en het wachtwoord dat is geldig voor de bestaande server op het moment dat de herstelbewerking werd gestart. Het wachtwoord kan worden gewijzigd in de nieuwe server **overzicht** pagina.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de service [back-ups](concepts-backup.md).
- Meer informatie over [bedrijfscontinuïteit](concepts-business-continuity.md) opties.
