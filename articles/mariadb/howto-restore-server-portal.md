---
title: Het herstellen van een Server in Azure Database voor MariaDB
description: In dit artikel wordt beschreven hoe u het herstellen van een server in Azure Database voor MariaDB met behulp van de Azure portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 1dc7930649a4e90c3b1c69a4f109f545f74a79f4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58081114"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mariadb-using-the-azure-portal"></a>Het back-up en herstellen van een server in Azure Database voor MariaDB met behulp van de Azure portal

## <a name="backup-happens-automatically"></a>Back-up wordt automatisch uitgevoerd
Azure Database voor MariaDB-servers zijn back-ups regelmatig op Restore-functies inschakelen. Met deze functie kan u de server en alle bijbehorende databases herstellen naar een eerder punt-in-time, op een nieuwe server.

## <a name="prerequisites"></a>Vereisten
Voor deze handleiding, hebt u het volgende nodig:
- Een [Azure Database voor MariaDB-server en database](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Configuratie van de back-up

Maken van de keuze tussen het configureren van uw server voor back-ups van lokaal redundante of geografisch redundante back-ups op de server is gemaakt, in de **prijscategorie** venster.

> [!NOTE]
> Nadat een server is gemaakt, het soort redundantie dat zo is, kan geografisch redundant en lokaal redundant, kan niet worden overgeschakeld.
>

Tijdens het maken van een server via de Azure-portal, de **prijscategorie** venster is waar u een selecteren **lokaal redundante** of **geografisch Redundant** back-ups voor uw server. In dit venster is ook hier selecteert u de **bewaarperiode voor back-up** : hoe lang (in dagen) u wilt dat de server back-ups die zijn opgeslagen voor.

   ![Prijscategorie - back-Upredundantie kiezen](./media/howto-restore-server-portal/pricing-tier.png)

Zie voor meer informatie over het instellen van deze waarden tijdens het maken, de [Azure Database voor MariaDB server Quick Start](quickstart-create-mariadb-server-database-using-azure-portal.md).

De bewaarperiode voor back-up kan worden gewijzigd op een server via de volgende stappen uit:
1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).

2. Selecteer uw Azure Database voor MariaDB-server. Deze actie opent u de **overzicht** pagina.

3. Selecteer **prijscategorie** in het menu onder **instellingen**. Met behulp van de schuifregelaar kunt u de **bewaarperiode voor back-up** aan uw voorkeur tussen 7 en 35 dagen.
In de onderstaande schermafbeelding is het verhoogd tot 35 dagen.
![Back-up bewaarperiode verhoogd](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Klik op **OK** de wijziging te bevestigen.

De bewaarperiode voor back-up bepaalt hoe ver terug in de tijd die een point-in-time-restore kan worden opgehaald, omdat deze gebaseerd op de back-ups beschikbaar. Point-in-time-restore is beschreven in de volgende sectie. 

## <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip
Azure Database voor MariaDB kunt u de server te herstellen naar een punt-in-time en in een nieuwe kopie van de server. U kunt deze nieuwe server gebruiken om uw gegevens te herstellen of hebben uw clienttoepassingen die verwijzen naar deze nieuwe server.

Bijvoorbeeld als een tabel per ongeluk is kunt verwijderd op twaalf uur 's middags vandaag, u herstellen tot het moment waarop net vóór twaalf uur 's middags en ophalen van de ontbrekende tabel en de gegevens van die nieuwe kopie van de server. Point-in-time-restore is op de server niveau, niet op het databaseniveau van de.

De volgende stappen wordt de voorbeeldserver hersteld naar een punt-in-time:
1. Selecteer uw Azure Database voor MariaDB-server in de Azure-portal. 

2. Op de werkbalk van de server **overzicht** weergeeft, schakelt **herstellen**.

   ![Azure Database voor MariaDB - overzicht - Restore-knop](./media/howto-restore-server-portal/2-server.png)

3. Vul het formulier herstellen met de vereiste gegevens in:

   ![Azure Database voor MariaDB - gegevens herstellen](./media/howto-restore-server-portal/3-restore.png)
   - **Herstelpunt**: Selecteer de point-in-time-u wilt herstellen.
   - **Doelserver**: Geef een naam voor de nieuwe server.
   - **Locatie**: U kunt de regio niet selecteren. Het is standaard hetzelfde als de bronserver.
   - **Prijscategorie**: U kunt deze parameters niet wijzigen bij het uitvoeren van een point-in-time-restore. Deze is hetzelfde als de bronserver. 

4. Klik op **OK** om de server om te herstellen naar een punt-in-time te herstellen. 

5. Nadat het herstel is voltooid, Ga naar de nieuwe server die is gemaakt om te controleren of dat de gegevens zijn hersteld zoals verwacht.

>[!Note]
>De nieuwe server die zijn gemaakt door een punt-in-time restore heeft de dezelfde serverbeheerder en het wachtwoord dat geldig voor de bestaande server op het punt-in-time is hebt gekozen. U kunt het wachtwoord wijzigen van de nieuwe server **overzicht** pagina.

## <a name="geo-restore"></a>Geo-herstel
Als u uw server voor geografisch redundante back-ups hebt geconfigureerd, kan een nieuwe server worden gemaakt van de back-up van de bestaande server. Deze nieuwe server kan worden gemaakt in andere regio's dat Azure Database voor MariaDB beschikbaar is.  

1. Selecteer de knop **Een resource maken** (+) in de linkerbovenhoek van de portal. Typ **Azure Database for MariaDB** in het zoekvak om de service te vinden.

   ![De optie "Azure Database voor MariaDB"](./media/howto-restore-server-portal/2_navigate-to-mariadb.png)

2. In het formulier **bron selecteren** vervolgkeuzelijst kiezen **back-up**. Met deze actie wordt een lijst met servers die geografisch redundante back-ups ingeschakeld hebt. Selecteer een van deze back-ups om te worden van de bron van de nieuwe server.
   ![Bron selecteren: Lijst van geografisch redundante back-ups en back-up](./media/howto-restore-server-portal/2-georestore.png)

   > [!NOTE]
   > Als u een server maakt eerst kan het niet onmiddellijk beschikbaar voor geo-herstel zijn. Het duurt een paar uur voor de metagegevens die nodig zijn om te worden ingevuld.
   >

3. Vul de rest van het formulier in met uw voorkeuren. U kunt een selecteren **locatie**. Na het selecteren van de locatie, kunt u **prijscategorie**. Standaard worden de parameters voor de bestaande server die vanwaar u herstelt weergegeven. U kunt klikken op **OK** zonder dat u wijzigingen aanbrengt die instellingen wilt overnemen. U kunt ook wijzigen **bewerking voor Compute** (indien beschikbaar in de regio u hebt gekozen), aantal **vCores**, **bewaarperiode voor back-up**, en **back-up Optie voor redundantie**. Wijzigen van **prijscategorie** (Basic, algemeen gebruik of geoptimaliseerd voor geheugen) of **opslag** grootte tijdens het terugzetten wordt niet ondersteund.

>[!Note]
>De nieuwe server die is gemaakt door geo-herstel heeft de dezelfde aanmeldnamen van serverbeheerder en het wachtwoord dat is geldig voor de bestaande server op het moment dat het herstel is gestart. Het wachtwoord kan worden gewijzigd van de nieuwe server **overzicht** pagina.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over van de service [back-ups](concepts-backup.md).
- Meer informatie over [bedrijfscontinuïteit](concepts-business-continuity.md) opties.
