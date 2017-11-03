---
title: Gebruikersinterface voor StorSimple Snapshot Manager | Microsoft Docs
description: Beschrijft de gebruikersinterface van het StorSimple Snapshot Manager en wordt uitgelegd hoe u deze voor het beheren van back-uptaken en de back-catalogus.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: c7d91892-2881-41a2-a7a2-908dc3646493
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.custom: 
ms.openlocfilehash: b48c507e38eb7cadff56259f617e336e4efe5708
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-user-interface-to-manage-backup-jobs-and-backup-catalog"></a>Gebruik StorSimple Snapshot Manager-gebruikersinterface voor het beheren van back-ups en back-upcatalogus

## <a name="overview"></a>Overzicht
De StorSimple Snapshot Manager heeft een intuïtieve gebruikersinterface waarmee u kunt uitvoeren en beheren van back-ups. Deze zelfstudie bevat een inleiding tot de gebruikersinterface en vervolgens wordt uitgelegd hoe u elk van de onderdelen. Zie voor een gedetailleerde beschrijving van het StorSimple Snapshot Manager [wat is er StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Beschrijving van de console
Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad de gebruikersinterface. Het consolevenster weergegeven, zoals wordt weergegeven in de volgende afbeelding.

![Deelvensters van de StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Het consolevenster heeft vijf belangrijke elementen. Klik op de koppeling voor een volledige beschrijving van elk element.

* [Menubalk](#menu-bar) 
* [Werkbalk](#tool-bar) 
* [Deelvenster bereik](#scope-pane) 
* [Deelvenster met resultaten](#results-pane) 
* [Actiedeelvenster](#actions-pane) 

Daarnaast StorSimple Snapshot Manager ondersteunt [toetsenbord navigatie en een aantal snelkoppelingen](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Toegankelijkheid van de console
De gebruikersinterface van de StorSimple Snapshot Manager ondersteunt de toegankelijkheidsfuncties van Windows-besturingssysteem en de Microsoft Management Console (MMC), evenals enkele sneltoetsen StorSimple Snapshot Manager-specifieke. 

* Voor een beschrijving van de toegankelijkheidsfuncties van Windows, gaat u naar [sneltoetsen voor Windows](https://support.microsoft.com/kb/126449). 
* Voor een beschrijving van de toegankelijkheidsfuncties van MMC, gaat u naar [toegankelijkheid voor MMC 3.0](https://technet.microsoft.com/library/cc766075.aspx)
* Voor een beschrijving van de toegankelijkheidsfuncties van StorSimple Snapshot Manager, gaat u naar [toetsenbord navigatie en snelkoppelingen](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Menubalk
De menubalk boven aan het consolevenster bevat [bestand](#file-menu), [actie](#action-menu), [weergave](#view-menu), [Favorieten](#favorites-menu), [venster](#window-menu), en [Help](#help-menu) menu's.

Klik op elk item in de menubalk voor een overzicht van de beschikbare opdrachten op dat menu. Het volgende voorbeeld wordt de **weergave** menu is geselecteerd in het menu.

![Menu Beeld geselecteerd](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Menu Bestand
De **bestand** menu bevat de standaardopdrachten van Microsoft Management Console (MMC).

#### <a name="menu-access"></a>Toegang tot menu
Om weer te geven de **bestand** menu, klikt u op **bestand** op de menubalk. Het volgende menu wordt weergegeven.

![Menu bestand StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft items die worden weergegeven op de **bestand** menu.

| Menu-item | Beschrijving |
|:--- |:--- |
| Nieuw |Klik op **nieuw** voor het maken van een nieuwe gebaseerd op het StorSimple Snapshot Manager-console. |
| Open |Klik op **openen** om een bestaande console te openen. |
| Opslaan |Klik op **opslaan** om op te slaan van de huidige console. |
| Opslaan als |Klik op **OpslaanAls** een nieuwe, nieuwe naam exemplaar maken van de huidige console. Gebruik de **OpslaanAls** optie voor een weergave aanpassen en opslaan voor later gebruik. U kunt bijvoorbeeld StorSimple Snapshot Manager-modules die naar specifieke servers verwijzen maken. |
| Software-module |Klik op **module toevoegen/verwijderen** toevoegen of verwijderen van modules en indelen op knooppunten in de **bereik** deelvenster. Ga voor meer informatie naar [toevoegen, verwijderen, en organiseren-modules en -extensies in MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Opties |Klik op **opties** gebruikersmodi toegang en machtigingen geven om te wijzigen op het pictogram van de console, of verwijder consolebestanden voor een verhoging van de beschikbare schijfruimte. |
| Lijst met paden |Klik op een pad in de genummerde lijst te openen van een bestand dat u onlangs geopend. |
| Afsluiten |Klik op **afsluiten** sluiten de **bestand** menu. |

### <a name="action-menu"></a>Actiemenu
Gebruik de **actie** menu kiezen uit de beschikbare acties. De items die voor u beschikbaar, is afhankelijk van de selectie die u aanbrengt in de **bereik** deelvenster of **resultaten** deelvenster.

#### <a name="menu-access"></a>Toegang tot menu
Om weer te geven de **actie** menu, een van de volgende handelingen uit:

* Met de rechtermuisknop op een item in de **bereik** deelvenster of **resultaten** deelvenster.
* Selecteer een item in de **bereik** deelvenster of **resultaten** deelvenster en klik vervolgens op **actie** op de menubalk. 

Bijvoorbeeld, als u het bovenste knooppunt in de **bereik** deelvenster en klik vervolgens met de rechtermuisknop of **actie** in de menubalk van de volgende menu wordt weergegeven.

![Menu Actie StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

De **acties** deelvenster (aan de rechterkant van de console) bevat dezelfde lijst met acties als de **actie** menu. Bovendien de **acties** deelvenster bevat de **weergave** menuopties waarmee u kunt het maken van een aangepaste weergave van de **resultaten** deelvenster.

![Open het actiedeelvenster met menu Beeld](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel bevat een alfabetische lijst van acties voor StorSimple Snapshot Manager. 

* De **actie** kolom geeft een lijst van acties die u op de knooppunten en de resultaten uitvoeren kunt. 
* De **navigatie** kolom wordt uitgelegd hoe u de juiste **actie** menu zodat u de actie kunt selecteren. Sommige acties worden weergegeven in meerdere **actie** menu's. Selecteer voor deze acties een **navigatie** optie uit de lijst met opsommingstekens. 
* De **beschrijving** kolom wordt beschreven hoe u elke actie te gebruiken in de **actie** menu of in het deelvenster Acties, en wordt uitgelegd wat het doet.

> [!NOTE]
> De **acties** deelvenster en **actie** menu's bevatten extra opties, zoals **weergave**, **nieuw venster vanaf hier**,  **Vernieuwen**, **lijst exporteren**, en **Help**. Deze opties zijn beschikbaar als onderdeel van de MMC-module en zijn niet specifiek voor StorSimple Snapshot Manager. De tabel bevat beschrijvingen van deze opties.
> 
> 

| Actie | Navigatie | Beschrijving |
|:--- |:--- |:--- |
| Verifiëren |Klik op de **apparaten** knooppunt met de rechtermuisknop op een apparaat in de **resultaten** deelvenster. |Klik op **verifiëren** het wachtwoord die u hebt geconfigureerd voor het apparaat invoeren. |
| Kloon |Vouw **back-upcatalogus**, vouw **Cloudmomentopnamen**, klikt u op een datum back-up en selecteer vervolgens een volume in de **resultaten** deelvenster. |Klik op **kloon** een kopie van een cloudmomentopname maken en deze opslaan op een locatie die u opgeeft. |
| Een apparaat configureren |Met de rechtermuisknop op de **apparaten** knooppunt. |Klik op **configureren van een apparaat** voor het configureren van een apparaat met één of meerdere apparaten verbinding maken met de Windows-host. |
| Back-upbeleid maken |Een van de volgende handelingen uit:<ul><li>Met de rechtermuisknop op **back-upbeleid**.</li><li>Klik op of vouw **Volume groepen**, en klik vervolgens met de rechtermuisknop op een volume-groep.</li><li>Klik of vouw **back-upcatalogus**, en klik vervolgens met de rechtermuisknop op een volume-groep.</li></ul> |Klik op **back-upbeleid maken** voor het configureren van een geplande back-up voor een groep volume. |
| Volume-groep maken |Een van de volgende handelingen uit:<ul><li>Klik op de **Volumes** knooppunt en klik vervolgens met de rechtermuisknop op een volume in de **resultaten** deelvenster.</li><li>Met de rechtermuisknop op de **Volume groepen** knooppunt.</li></ul> |Klik op **Create Volume Group** volumes toewijzen aan een groep volume. |
| Verwijderen |Klik op een knooppunt of het resultaat (dit item wordt weergegeven op veel **actie** menu's en **acties** deelvensters.) |Klik op **verwijderen** verwijderen van het knooppunt of het resultaat dat u hebt geselecteerd. Wanneer het bevestigingsdialoogvenster wordt weergegeven, Controleer of de verwijdering annuleert. |
| Details |Klik op de **apparaten** knooppunt en klik vervolgens met de rechtermuisknop op een apparaat in de **resultaten** deelvenster. |Klik op **Details** voor de configuratie-informatie voor een apparaat. |
| Bewerken |Klik op **back-upbeleid**, en klik vervolgens met de rechtermuisknop op een beleid in de **resultaten** deelvenster. |Klik op **bewerken** wijzigen van het back-upschema voor een groep volume. |
| Lijst exporteren |Klik op een knooppunt of het resultaat (dit item wordt weergegeven op alle **actie** menu's en **acties** deelvensters.) |Klik op **lijst exporteren** voor het opslaan van een lijst in een bestand met door komma's gescheiden waarden (CSV). U kunt dit bestand vervolgens importeren in een werkbladtoepassing voor analyse. |
| Help |Klik op een knooppunt of het resultaat. (Dit item wordt weergegeven op alle **actie** menu's en **acties** deelvensters.) |Klik op **Help** online-Help openen in een apart browservenster. |
| Nieuw venster |Klik op een knooppunt of het resultaat (dit item wordt weergegeven op alle **actie** menu's en **acties** deelvensters.) |Klik op **nieuw venster vanaf hier** om een nieuwe StorSimple Snapshot Manager-venster te openen. |
| Vernieuwen |Klik op een knooppunt of het resultaat (dit item wordt weergegeven op alle **actie** menu's en **acties** deelvensters.) |Klik op **vernieuwen** bijwerken van het StorSimple Snapshot Manager-venster weergegeven. |
| Apparaat vernieuwen |Klik op de **apparaten** knooppunt met de rechtermuisknop op een apparaat in de **resultaten** deelvenster. |Klik op **apparaat vernieuwen** specifieke aangesloten apparaat synchroniseren met StorSimple Snapshot Manager. |
| Vernieuwen van apparaten |Met de rechtermuisknop op de **apparaten** knooppunt. |Klik op **apparaten vernieuwen** uw lijst met verbonden apparaten synchroniseren met StorSimple Snapshot Manager. |
| Volumes opnieuw scannen |Met de rechtermuisknop op de **Volumes** knooppunt. |Klik op **volumes opnieuw scannen** bijwerken van de lijst van volumes die wordt weergegeven in de **resultaten** deelvenster. |
| Herstellen |Vouw **back-upcatalogus**, vouw een volume-groep uit, vouw **lokale momentopnamen** of **Cloudmomentopnamen**, en klik vervolgens met de rechtermuisknop op een back-up. |Klik op **herstellen** ter vervanging van de huidige gegevens over volume met de gegevens van de geselecteerde back-up. |
| Back-up maken |Een van de volgende handelingen uit:<ul><li>Vouw **Volume groepen**, en klik vervolgens met de rechtermuisknop op een volume-groep.</li><li>Vouw **back-upcatalogus**, en klik vervolgens met de rechtermuisknop op een volume-groep.</li></ul> |Klik op **duren back-** onmiddellijk starten van een back-uptaak. |
| Wisselknop invoer weergeven |Met de rechtermuisknop op het bovenste knooppunt in de **bereik** deelvenster (de **StorSimple Snapshot Manager** knooppunt in de voorbeelden). |Klik op **wisselknop invoer weergave** weergeven of verbergen van de volume-groepen en de gekoppelde back-ups die zijn geïmporteerd uit het servicedashboard Manager voor StorSimple-apparaat. |

### <a name="view-menu"></a>Menu Beeld
Gebruik de **weergave** menu voor het maken van een aangepaste weergave van de **resultaten** deelvenster inhoud. De **weergave** menu bevat **kolommen toevoegen/verwijderen** en **aanpassen** opties.

#### <a name="menu-access"></a>Toegang tot menu
U hebt toegang tot de **weergave** in het menu balk of in de **acties** deelvenster.

![Menu Beeld StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft items die worden weergegeven op de **weergave** menu.

| Menu-item | Beschrijving |
|:--- |:--- |
| Kolommen toevoegen/verwijderen |Klik op **kolommen toevoegen/verwijderen** toevoegen of verwijderen van kolommen in de **resultaten** deelvenster. |
| Aanpassen |Klik op **aanpassen** weergeven of verbergen van items in het consolevenster StorSimple Snapshot Manager-. |

### <a name="favorites-menu"></a>Favorieten
Gebruik de **Favorieten** menu wilt toevoegen, verwijderen en het ordenen van paginaweergaven en taken die u vaak gebruikt. 

#### <a name="menu-access"></a>Toegang tot menu
U hebt toegang tot de **Favorieten** menu in het menu.

![Menu StorSimple Snapshot Manager Favorieten](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft items die worden weergegeven op de **Favorieten** menu.

| Menu-item | Beschrijving |
|:--- |:--- |
| Toevoegen aan Favorieten |Klik op **toevoegen aan Favorieten** de huidige weergave toevoegen aan de lijst met Favorieten. |
| Favorieten indelen |Klik op **Favorieten indelen** te organiseren van de inhoud van de map van uw Favorieten. |

### <a name="window-menu"></a>Venstermenu
Gebruik de **venster** menu toe te voegen en het rangschikken van windows voor StorSimple Snapshot Manager-console.

#### <a name="menu-access"></a>Toegang tot menu
U hebt toegang tot de **venster** menu in het menu.

![Menu StorSimple Snapshot Manager-venster](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

De genummerde lijst onderaan in het menu ziet u de vensters die momenteel zijn geopend. Klik op elk venster in de lijst om te zorgen dat het venster in de voorgrond is geplaatst. 

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft de items die worden weergegeven in het menu venster.

| Menu-item | Beschrijving |
|:--- |:--- |
| Nieuw venster |Klik op **nieuw venster** openen van een nieuw consolevenster (naast het bestaande venster). |
| CASCADE |Klik op **Cascade** om weer te geven van de open consolevensters in een CSS-stijl. |
| Schikken |Klik op **tegel horizontaal** om weer te geven van de windows console openen in een tegel (of het raster)-indeling. |
| Pictogrammen schikken |Als er meerdere console windows openen en verspreid over uw bureaublad te minimaliseren en klik vervolgens op **schikken Pictogrammen schikken** deze rangschikken in een horizontale rij aan de onderkant van het scherm. |

### <a name="help-menu"></a>Menu Help
Gebruik de **Help** menu beschikbaar online-help voor StorSimple Snapshot Manager en MMC weergeven. U kunt ook informatie over de MMC en StorSimple Snapshot Manager software-versies die momenteel zijn geïnstalleerd op uw systeem weergeven. 

U hebt toegang tot de **Help** menu in het menu. U kunt ook toegang tot StorSimple Snapshot Manager help-onderwerpen in de **acties** deelvenster.

![StorSimple Snapshot Manager Help-menu](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft de items die worden weergegeven in het menu Help.

| Menu-item | Beschrijving |
|:--- |:--- |
| Help bij StorSimple Snapshot Manager |Klik op **StorSimple Snapshot Manager Help** StorSimple Snapshot Manager help openen in een apart venster. |
| Help-onderwerpen |Klik op **Help-onderwerpen** online help van MMC openen in een apart venster. |
| TechCenter-website |Klik op **TechCenter-website** de startpagina van Microsoft TechNet Tech Center openen in een apart venster. |
| Over Microsoft Management Console |Klik op **over Microsoft Management Console** om te zien welke versie van de Microsoft Management Console is geïnstalleerd op uw systeem. |
| StorSimple Snapshot Manager |Klik op **StorSimple Snapshot Manager** om te zien welke versie van de module is geïnstalleerd op uw systeem. |

## <a name="tool-bar"></a>Werkbalk
De werkbalk zich onder de menubalk bevat pictogrammen voor navigatie en taak. Elk pictogram is een snelkoppeling naar een specifieke taak.

### <a name="icon-descriptions"></a>Pictogram beschrijvingen
De volgende tabel beschrijft de pictogrammen op de werkbalk. 

| Pictogram | Beschrijving |
|:--- |:--- |
| ![Pijl-links](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) |Klik op het pictogram Pijl-links om terug te keren naar de vorige pagina. |
| ![Pijl-rechts](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) |Klik op de pijl naar rechts om naar de volgende pagina te gaan (als de pijl grijs wordt weergegeven, de actie is niet beschikbaar). |
| ![Pictogram omhoog](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) |Klik op het pictogram omhoog gaat één niveau in de consolestructuur (de **bereik** deelvenster). |
| ![Structuur van de console weergeven/verbergen](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) |Klik op het pictogram weergeven/verbergen console structuur wilt weergeven of verbergen de **bereik** deelvenster. |
| ![Lijst exporteren](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) |Klik op het pictogram van de lijst exporteren om een lijst exporteren naar een CSV-bestand dat u opgeeft. |
| ![Help-pictogram](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png) |Klik op het Helppictogram om een online MMC-help-onderwerp. |
| ![Actiedeelvenster weergeven/verbergen](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) |Klik op de weergeven/verbergen **acties** pictogram in het deelvenster weergeven of verbergen de **acties** deelvenster. |

## <a name="scope-pane"></a>Deelvenster bereik
De **bereik** deelvenster is het linkerdeelvenster in de StorSimple Snapshot Manager-UI. Het bevat de structuur van de console (of een knooppunt) en is het primaire navigatie-mechanisme voor StorSimple Snapshot Manager. 

### <a name="scope-pane-structure"></a>Bereik deelvenster Structuur
De **bereik** deelvenster bevat een reeks klikbaar objecten (knooppunten) in een boomstructuur gerangschikt. 

![Deelvenster bereik](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

* Als u wilt uitvouwen of samenvouwen van een knooppunt, klikt u op het pijltje naast de naam van het knooppunt.
* De status of de inhoud van een knooppunt, klikt u op naam van het knooppunt. De informatie wordt weergegeven in de **resultaten** deelvenster. 

De **bereik** deelvenster bevat de volgende knooppunten: 

* [Knooppunt apparaten](#devices-node) 
* [Volumes knooppunt](#volumes-node) 
* [Knooppunt voor volume-groepen](#volume-groups-node) 
* [Back-knooppunt van het beleid](#backup-policies-node) 
* [Back-knooppunt van de catalogus](#backup-catalog-node) 
* [Taken knooppunt](#jobs-node) 

### <a name="scope-pane-tasks"></a>Bereik deelvenster Taken
U kunt de **bereik** deelvenster om een actie op een specifiek knooppunt te voltooien. Selecteer een taak, een van de volgende te doen:

* Met de rechtermuisknop op het knooppunt en selecteer vervolgens de taak in het menu dat verschijnt.
* Klik op het knooppunt en klik vervolgens op **actie** op de menubalk. Selecteer de taak in het menu dat verschijnt.
* Klik op het knooppunt en selecteer vervolgens de bewerking in de **acties** deelvenster.

Wanneer u een knooppunt selecteert en een van deze methoden te gebruiken om te zien van een lijst met taken, worden alleen acties die kunnen worden uitgevoerd op dat knooppunt worden weergegeven.

### <a name="devices-node"></a>Knooppunt apparaten
De **apparaten** knooppunt vertegenwoordigt de StorSimple-apparaten en het virtuele StorSimple-apparaten die zijn verbonden aan StorSimple Snapshot Manager. Selecteer dit knooppunt om te verbinden en configureert u een apparaat en de gekoppelde volumes, volumes, groepen en bestaande back-ups importeren. Meerdere apparaten kunnen worden aangesloten op één host.

* Vouw het knooppunt uit, klik op de pijl naast **apparaten**.
* Een menu te zien van de beschikbare acties, met de rechtermuisknop op de **apparaten** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van geconfigureerde apparaten wilt bekijken, klikt u op **apparaten** in de **bereik** deelvenster. De lijst met apparaten, evenals informatie over elk apparaat wordt weergegeven in de **resultaten** deelvenster.

### <a name="volumes-node"></a>Volumes knooppunt
De **Volumes** knooppunt vertegenwoordigt de stations die overeenkomen met de volumes die zijn gekoppeld met de host, met inbegrip van die via iSCSI- en die zijn gedetecteerd via een apparaat gedetecteerd. Gebruik dit knooppunt in de lijst met beschikbare volumes weergeven en afzonderlijke volumes toewijzen aan groepen volume.

* Vouw het knooppunt uit, klik op de pijl naast **Volumes**.
* Een menu te zien van de beschikbare acties, met de rechtermuisknop op de **Volumes** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van volumes wilt bekijken, klikt u op **Volumes** in de **bereik** deelvenster. De lijst van volumes, samen met informatie over elk volume wordt weergegeven in de **resultaten** deelvenster.

### <a name="volume-groups-node"></a>Knooppunt voor volume-groepen
Volume-groepen zijn ook wel consistentiecontrole groepen. Elke groep volume is een groep van toepassing-gerelateerde volumes die helpt consistentie te garanderen toepassing tijdens de back-upbewerkingen. Gebruik de **Volume groepen** knooppunt voor het configureren van deze groepen en interactieve back-ups uitvoeren of maken van back-upschema's. 

* Vouw het knooppunt uit, klik op de pijl naast **Volume groepen**.
* Een menu te zien van de beschikbare acties, met de rechtermuisknop op de **Volume groepen** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van volume-groepen wilt bekijken, klikt u op **Volume groepen** in de **bereik** deelvenster. De lijst met groepen van volume, samen met informatie over elke groep volume wordt weergegeven in de **resultaten** deelvenster.

### <a name="backup-policies-node"></a>Back-knooppunt van het beleid
Back-upbeleid zijn taakschema's voor lokale en cloud worden opgeslagen. Gebruik de **back-upbeleid** knooppunt om op te geven hoe vaak een back-up wordt gemaakt en hoe lang een back-up moet worden bewaard. 

* Vouw het knooppunt uit, klik op de pijl naast **back-upbeleid**.
* Een menu te zien van de beschikbare acties, met de rechtermuisknop op de **back-upbeleid** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Klik op een overzicht van back-upbeleid **back-upbeleid** in de **bereik** deelvenster. De lijst met back-upbeleid, samen met informatie over elk beleid wordt weergegeven in de **resultaten** deelvenster.

> [!NOTE]
> U kunt maximaal 64 back-ups behouden.


### <a name="backup-catalog-node"></a>Back-knooppunt van de catalogus
De **back-upcatalogus** knooppunt bevat lijsten met lokale en externe back-ups van Azure StorSimple-volumes. Dit knooppunt is geordend op het volume-groep, en elke groep volumecontainer afzonderlijke structuren voor lokale momentopnamen bevat (de **lokale momentopname**s knooppunt) en in de cloud momentopnamen (de **Cloudmomentopnamen** knooppunt). Wanneer u uitgevouwen, wordt elke groep volumecontainer alle geslaagde back-ups die zijn uitgevoerd, interactief of door een geconfigureerde beleid.

* Vouw het knooppunt uit, klik op de pijl naast **back-upcatalogus**.
* Een menu te zien van de beschikbare acties, met de rechtermuisknop op de **back-upcatalogus** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Klik op een overzicht van back-upmomentopnamen **back-upcatalogus** in de **bereik** deelvenster. De lijst van momentopnamen, samen met informatie over elke momentopname wordt weergegeven in de **resultaten** deelvenster.

### <a name="local-snapshots-node"></a>Lokale momentopnamen knooppunt
De **lokale momentopnamen** knooppunt een lijst met lokale momentopnamen voor een specifieke volume-groep. Het knooppunt bevindt zich onder de **back-upcatalogus** knooppunt in de **bereik** deelvenster. Lokale momentopnamen zijn punt in tijd kopieën van volumegegevens die zijn opgeslagen op het Azure StorSimple-apparaat. Normaal gesproken worden deze type back-up gemaakt en snel worden hersteld. Net als een lokale back-up, kunt u een lokale momentopname gebruiken.

* Vouw het knooppunt uit, klik op de pijl naast **lokale momentopnamen**.
* Een menu te zien van de beschikbare acties, met de rechtermuisknop op de **lokale momentopnamen** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van lokale momentopnamen wilt bekijken, klikt u op **lokale momentopnamen** in de **bereik** deelvenster. De lijst van momentopnamen, samen met informatie over elke momentopname wordt weergegeven in de **resultaten** deelvenster.

### <a name="cloud-snapshots-node"></a>Cloud momentopnamen knooppunt
De **Cloudmomentopnamen** knooppunt cloudmomentopnamen voor een groep specifieke volume bevat. Het knooppunt bevindt zich onder de **back-upcatalogus** knooppunt in de **bereik** deelvenster. Cloudmomentopnamen zijn punt in tijd kopieën van volumegegevens die zijn opgeslagen in de cloud. Een cloudmomentopname is gelijk aan een momentopname op een andere, externe opslagsysteem gerepliceerd. Cloudmomentopnamen zijn bijzonder nuttig voor herstel na noodgevallen.

* Vouw het knooppunt uit, klik op de pijl naast **Cloudmomentopnamen**.
* Een menu te zien van de beschikbare acties, met de rechtermuisknop op de **Cloudmomentopnamen** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Klik op een overzicht van cloudmomentopnamen **Cloudmomentopnamen** in de **bereik** deelvenster. De lijst van momentopnamen, samen met informatie over elke momentopname wordt weergegeven in de **resultaten** deelvenster.

### <a name="jobs-node"></a>Taken knooppunt
De **taken** knooppunt bevat informatie over de geplande uitgevoerd en recentelijk uitgevoerde back-uptaken. 

* Vouw het knooppunt uit, klik op de pijl naast **taken**.
* Een menu te zien van de beschikbare acties, met de rechtermuisknop op de **taken** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van geplande taken wilt bekijken, vouwt u de **taken** knooppunt en klik vervolgens op **geplande**. De lijst van eerder geconfigureerde taken en informatie over elke taak wordt weergegeven in de **resultaten** deelvenster. 
* Een lijst van de laatst voltooide taken wilt bekijken, vouwt u de **taken** knooppunt en klik vervolgens op **afgelopen 24 uur**. Een lijst met taken die in de afgelopen 24 uur zijn voltooid wordt weergegeven in de **resultaten** deelvenster. De **resultaten** deelvenster bevat ook informatie over elke voltooide taak.
* U ziet een lijst met taken die momenteel worden uitgevoerd, vouw de **taken** knooppunt en klik vervolgens op **met**. De lijst met actieve taken en informatie over elke taak wordt weergegeven in de **resultaten** deelvenster.

## <a name="results-pane"></a>Deelvenster met resultaten
De **resultaten** deelvenster is het middelste deelvenster van de StorSimple Snapshot Manager-UI. Het bevat een lijst met en gedetailleerde statusinformatie voor het knooppunt dat u hebt geselecteerd in de **bereik** deelvenster.

### <a name="example"></a>Voorbeeld
Als u wilt het volgende voorbeeld ziet, klikt u op de **Volume groepen** knooppunt in de **bereik** deelvenster. De **resultaten** deelvenster geeft een lijst met groepen met informatie over elke groep volume.

![Deelvenster met resultaten](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

U kunt de details weergegeven in de **resultaten** deelvenster: met de rechtermuisknop op een knooppunt in de **bereik** deelvenster, klikt u op **weergave**, en klik vervolgens op **kolommen toevoegen/verwijderen** .

## <a name="actions-pane"></a>Actiedeelvenster
De **acties** deelvenster is het rechter deelvenster in het StorSimple Snapshot Manager-UI. Het bevat een menu van bewerkingen die u op het knooppunt, weergave of gegevens die u selecteert uitvoeren kunt in de **bereik** deelvenster of **resultaten** deelvenster. De **acties** deelvenster bevat dezelfde opdrachten als de **actie** menu's die beschikbaar zijn voor de items in de **bereik** deelvenster en **resultaten** deelvenster. Voor een beschrijving van elke actie, Zie de tabel in de **actie** sectie menu.

### <a name="examples"></a>Voorbeelden
In het volgende voorbeeld zien de **bereik** deelvenster, vouw de **taken** knooppunt en klik op **geplande**. De **acties** deelvenster ziet u de beschikbare acties voor de **geplande** knooppunt.

![Voorbeeld van de geplande taken deelvenster Acties](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

Voor meer opties de **bereik** deelvenster, vouw de **taken** knooppunt, klik op **geplande**, en klik vervolgens op een geplande taak in de **resultaten** deelvenster. De **acties** deelvenster ziet u de beschikbare acties voor de geplande taak, zoals wordt weergegeven in het volgende voorbeeld.

![Voorbeeld van de taak acties deelvenster Acties](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Toetsenbordnavigatie en snelkoppelingen
StorSimple Snapshot Manager kunt de toegankelijkheidsfuncties van het Windows-besturingssysteem en de Microsoft Management Console (MMC). Dit omvat ook sommige toetsenbord navigatiefuncties en snelkoppelingen die specifiek voor StorSimple Snapshot Manager zijn, zoals beschreven in de volgende secties.

* [Toetsen voor toetsenbordnavigatie](#keyboard-navigation-keys) 
* [Menubalk sneltoetsen](#menu-bar-shortcut-keys) 
* [Sneltoetsen in het bereik](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Toetsen voor toetsenbordnavigatie
De volgende tabel beschrijft de sleutels die u gebruiken kunt om te navigeren in de gebruikersinterface van de StorSimple Snapshot Manager. 

| Navigatie-sleutel | Actie |
|:--- |:--- |
| Pijltoets-omlaag |Gebruik de pijltoets-omlaag verticaal moet worden verplaatst naar het volgende item in een menu of deelvenster. |
| Voer |Druk op Enter drukken om een actie voltooid en gaat u verder met de volgende stap. Bijvoorbeeld, u kunt op ENTER drukken om te selecteren **volgende**, **OK**, of **maken**, en gaat u naar de volgende stap in een wizard. |
| ESC |Druk op ESC om een menu te sluiten of te annuleren en een pagina te sluiten. |
| F1 |Druk op F1 om weer te geven van een help-onderwerp voor het actieve venster. |
| F5 |Druk op F5 voor vernieuwen van een knooppunt. |
| F6 |Druk op F6 voor het verplaatsen van de **bereik** deelvenster naar de **resultaten** deelvenster. |
| F10 |Druk op F10 om naar de menubalk te gaan. |
| Pijl-links |Gebruik de pijltoets-links horizontaal van menuoptie voor een balk verplaatsen naar de vorige optie. Wanneer u naar het vorige item op de menubalk verplaatst, wordt het menu Actie (of context) voor het vorige item weergegeven. |
| Pijl-rechts |Gebruik de pijltoets-rechts horizontaal van één optie balk verplaatsen naar de volgende. Wanneer u naar het volgende item op de menubalk verplaatst, wordt het menu Actie (of context) voor het nieuwe item weergegeven. |
| TAB-toets |Gebruik de Tab-toets om naar het volgende deelvenster op de console of naar het volgende selectie of tekst in een pagina te verplaatsen. |
| Pijl-omhoog |Gebruik de pijltoets-omhoog verticaal moet worden verplaatst naar het vorige item in een menu of deelvenster. |

### <a name="menu-bar-shortcut-keys"></a>Menubalk sneltoetsen
De volgende tabel beschrijft de sneltoetsen voor de menubalk. Nadat u de snelkoppeling toetsen en het menu wordt geopend, kunt u sneltoetsen voor menuopdrachten (onderstreepte sleutels in het menu). Voor meer informatie over de menubalk, gaat u naar [menubalk](#menu-bar).

| Snelkoppeling | Resultaat | Menu sneltoets | Resultaat |
|:--- |:--- |:--- |:--- |
| ALT + F |Hiermee opent u de **bestand** menu. |N |Hiermee opent u een nieuw exemplaar van de console. |
|  |O |Hiermee opent u de **Systeembeheer** pagina. | |
|  |S |Hiermee slaat u het StorSimple Snapshot Manager-console. | |
|  |A |Hiermee opent u de **OpslaanAls** pagina. | |
|  |M |Hiermee opent u de **module toevoegen/verwijderen** pagina. | |
|  |P |Hiermee opent u de **opties** pagina. | |
|  |H |Hiermee opent u een online-Help. | |
| ALT + A |Hiermee opent u de **actie** menu. |I |Hiermee schakelt u de optie importeren weergeven in- of uitschakelen. |
|  |W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console. | |
|  |F |De StorSimple Snapshot Manager-console-updates. | |
|  |L |Hiermee opent u de **lijst exporteren** pagina. | |
|  |H |Hiermee opent u een online-Help. | |
| ALT + V |Hiermee opent u de **weergave** menu. |A |Hiermee opent u de **kolommen toevoegen/verwijderen** pagina. |
|  |U |Hiermee opent u de **weergave aanpassen** pagina. | |
| ALT + O |Hiermee opent u de **Favorieten** menu. |A |Hiermee opent u de **toevoegen aan Favorieten** pagina. |
|  |O |Hiermee opent u de **Favorieten indelen** pagina. | |
| ALT + W |Hiermee opent u de **venster** menu. |N |Hiermee opent u een ander StorSimple Snapshot Manager-venster. |
|  |C |Geeft alle open consolevensters in een CSS-stijl. | |
|  |T |Geeft alle open consolevensters weer in een rasterpatroon. | |
|  |I |Pictogrammen in een horizontale rij aan de onderkant van het scherm. | |
| ALT + H |Hiermee opent u de **Help** menu. |H |Hiermee opent u een online-Help. |
|  |T |Hiermee opent u de webpagina van Microsoft TechNet Tech Center. | |
|  |A |Hiermee opent u de **over Microsoft Management Console** pagina. | |

### <a name="scope-pane-shortcut-keys"></a>Sneltoetsen in het bereik
De volgende tabellen tonen de snelkoppeling toetsencombinaties voor elk knooppunt in de **bereik** deelvenster. 

* [Sneltoetsen voor knooppunt apparaten](#devices-node-shortcut-keys)
* [Sneltoetsen voor volumes knooppunt](#volumes-node-shortcut-keys)
* [Sneltoetsen voor volume groepen knooppunt](#volume-groups-node-shortcut-keys)
* [Back-up maken van beleid knooppunt sneltoetsen](#backup-policies-node-shortcut-keys)
* [Back-up van catalogus knooppunt sneltoetsen](#backup-catalog-node-shortcut-keys)
* [Sneltoetsen voor taken knooppunt](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Sneltoetsen voor knooppunt apparaten
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| C |Hiermee opent u de **configureren van een apparaat** pagina. |
| D |Hiermee vernieuwt u de lijst met apparaten en Apparaatdetails. |
| V |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Details** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de **lijst exporteren** pagina. |
| H |Hiermee opent u een online-Help. |

#### <a name="volumes-node-shortcut-keys"></a>Sneltoetsen voor volumes knooppunt
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| V |De lijst van volumes bijgewerkt. |
| V (druk tweemaal) |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Volumes** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de **lijst exporteren** pagina. |
| H |Hiermee opent u een online-Help. |

#### <a name="volume-groups-node-shortcut-keys"></a>Sneltoetsen voor volume groepen knooppunt
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| G |Hiermee opent u de **maakt u een groep Volume** pagina. |
| V |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Volume groepen** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de **lijst exporteren** pagina. |
| H |Hiermee opent u een online-Help. |

#### <a name="backup-policies-node-shortcut-keys"></a>Back-up maken van beleid knooppunt sneltoetsen
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| B |Hiermee opent u de **een beleid maken** pagina. |
| V |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Volume groepen** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de ** lijst exporteren ** pagina. |
| H |Hiermee opent u een online-Help. |

#### <a name="backup-catalog-node-shortcut-keys"></a>Back-up van catalogus knooppunt sneltoetsen
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Volume groepen** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| H |Hiermee opent u een online-Help. |

#### <a name="jobs-node-shortcut-keys"></a>Sneltoetsen voor taken knooppunt
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| V |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **taken** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de **lijst exporteren** pagina. |
| H |Hiermee opent u de online-Help |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het verbinding maken en beheren van apparaten](storsimple-snapshot-manager-manage-devices.md).

