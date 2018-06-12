---
title: Het herstellen van een Server in Azure-Database voor MySQL
description: Dit artikel wordt beschreven hoe u een server in Azure-Database herstelt voor MySQL met de Azure portal.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 04/01/2018
ms.openlocfilehash: 6a34bbb5eefac117775c9876f3e4a25d3dade736
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266268"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a>Het back-up en herstellen van een server in Azure-Database voor MySQL met de Azure portal

## <a name="backup-happens-automatically"></a>Back-up automatisch wordt uitgevoerd
Azure-Database voor de MySQL-servers zijn back-up periodiek Restore-functies inschakelen. Gebruik van deze functie kunt u de server en alle bijbehorende databases herstellen naar een eerdere point-in-time, op een nieuwe server.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze handleiding instructies, hebt u het volgende nodig:
- Een [Azure-Database voor MySQL-server en database](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Back-upconfiguratie instellen

Maken van de keuze tussen uw server configureren voor lokaal redundante back-ups of geografisch redundante back-ups maken van de server, in de **prijscategorie** venster.

> [!NOTE]
> Nadat een server is gemaakt, wordt het soort redundantie dat zo is, kan geografisch redundante tegenover lokaal redundant kan niet worden overgeschakeld.
>

Tijdens het maken van een server via de Azure-portal, de **prijscategorie** venster is waar u een selecteren **lokaal redundante** of **geografisch redundante** back-ups voor uw server. Dit venster is ook waar u selecteert de **bewaarperiode back-up** : hoe lang (in dagen) u wilt dat de server back-ups voor opgeslagen.

   ![Prijscategorie - back-redundantie kiezen](./media/howto-restore-server-portal/pricing-tier.png)

Zie voor meer informatie over het instellen van deze waarden tijdens het maken, de [Azure Database voor MySQL server Quick Start](quickstart-create-mysql-server-database-using-azure-portal.md).

De back-up bewaarperiode kan worden gewijzigd op een server via de volgende stappen uit:
1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Selecteer uw Azure-Database voor de MySQL-server. Hiermee opent u deze actie de **overzicht** pagina.
3. Selecteer **prijscategorie** in het menu onder **instellingen**. Met behulp van de schuifregelaar kunt u de **back-up bewaarperiode** aan uw voorkeur tussen 7 en 35 dagen.
In de onderstaande schermafbeelding is het verhoogd tot 34 dagen.
![Back-up bewaarperiode verhoogd](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Klik op **OK** de wijziging te bevestigen.

De back-up bewaarperiode bepaalt hoe ver terug in tijd die een punt in tijd terugzetbewerking kan worden opgehaald, omdat deze gebaseerd op de back-ups beschikbaar. Punt in tijd restore wordt verder beschreven in de volgende sectie. 

## <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip
Azure MySQL-Database kunt u de server terugzetten naar punt in tijd en in op een nieuwe kopie van de server. U kunt deze nieuwe server gebruiken om uw gegevens te herstellen of uw clienttoepassingen die verwijzen naar deze nieuwe server hebt.

Bijvoorbeeld, als een tabel per ongeluk is kan verwijderd op twaalf uur 's middags vandaag de dag u herstellen en de tijd net vóór twaalf uur 's middags en ophalen van de ontbrekende tabel en de gegevens van die nieuwe kopie van de server. Punt in tijd terugzetten is op de server niveau, niet op het databaseniveau van de.

De volgende stappen uit herstellen de voorbeeldserver naar een punt in tijd:
1. Selecteer uw Azure-Database voor de MySQL-server in de Azure-portal. 

2. Op de werkbalk van de server **overzicht** pagina **herstellen**.

   ![Azure-Database voor herstel van MySQL - overzicht - knop](./media/howto-restore-server-portal/2-server.png)

3. Vul het formulier terugzetten met de vereiste informatie in:

   ![Azure-Database voor de MySQL - informatie terugzetten ](./media/howto-restore-server-portal/3-restore.png)
  - **Herstelpunt**: Selecteer de point-in-time-u wilt herstellen.
  - **Doelserver**: Geef een naam op voor de nieuwe server.
  - **Locatie**: U kunt de regio niet selecteren. Het is standaard hetzelfde als de bronserver.
  - **Prijscategorie**: U kunt deze parameters niet wijzigen bij het uitvoeren van een punt in tijd te herstellen. Deze is hetzelfde als de bronserver. 

4. Klik op **OK** om de server om te zetten naar een punt in tijd te herstellen. 

5. Nadat het herstel is voltooid, zoek de nieuwe server die wordt gemaakt om te controleren of dat de gegevens is hersteld, zoals verwacht.

>[!Note]
>De nieuwe server gemaakt door herstel punt in tijd heeft als het dezelfde server admin-aanmeldingsnaam en wachtwoord dat geldig voor de bestaande server op het punt in tijd is hebt gekozen. U kunt het wachtwoord wijzigen van de nieuwe server **overzicht** pagina.

## <a name="geo-restore"></a>Geo-herstel
Als u uw server voor geografisch redundante back-ups geconfigureerd, kan een nieuwe server worden gemaakt vanuit de back-up van een bestaande server. Deze nieuwe server kan worden gemaakt in elke regio dat Azure voor MySQL-Database beschikbaar is.  

1. Selecteer de knop **Een resource maken** (+) in de linkerbovenhoek van de portal. Selecteer **Databases** > **Azure Database voor MySQL**.

   ![De optie 'Azure-Database voor MySQL'](./media/howto-restore-server-portal/2_navigate-to-mysql.png)

2. In het formulier **bron selecteren** vervolgkeuzelijst kiezen **back-up**. Met deze actie wordt een lijst met servers die geografisch redundante back-ups ingeschakeld hebben. Selecteer een van deze back-ups als de bron van de nieuwe server.
   ![Bron selecteren: Lijst van geografisch redundante back-ups en back-up](./media/howto-restore-server-portal/2-georestore.png)

   > [!NOTE]
   > Wanneer een server voor het eerst wordt gemaakt kunt deze mogelijk niet onmiddellijk beschikbaar voor de geo-herstel. Het duurt enkele uren voor de benodigde metagegevens worden ingevuld.
   >

3. Vul de rest van het formulier met uw voorkeuren. U kunt kiezen **locatie**. Na het selecteren van de locatie, kunt u **prijscategorie**. Standaard worden de parameters voor de bestaande server die vanwaar u herstelt weergegeven. U kunt klikken op **OK** zonder wijzigingen over te nemen die instellingen. Of u kunt wijzigen **Compute generatie** (indien beschikbaar in de regio u hebt gekozen), aantal **vCores**, **back-up bewaarperiode**, en **back-up Redundantie optie**. Het wijzigen van **prijscategorie** (Basic, algemeen of geoptimaliseerd voor geheugen) of **opslag** tijdens het terugzetten van de grootte wordt niet ondersteund.

>[!Note]
>De nieuwe server gemaakt door geografisch herstel heeft de dezelfde aanmeldingsnaam van server-beheerder en het wachtwoord dat is geldig voor de bestaande server op het moment dat de herstelbewerking werd gestart. Het wachtwoord kan worden gewijzigd in de nieuwe server **overzicht** pagina.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de service [back-ups](concepts-backup.md).
- Meer informatie over [bedrijfscontinuïteit](concepts-business-continuity.md) opties.
