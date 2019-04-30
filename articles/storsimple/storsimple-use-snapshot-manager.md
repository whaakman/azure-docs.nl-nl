---
title: StorSimple Snapshot Manager-gebruikersinterface | Microsoft Docs
description: Wordt de gebruikersinterface van de StorSimple Snapshot Manager beschreven en wordt uitgelegd hoe u deze gebruiken voor het beheren van back-uptaken en de back-catalogus.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: c7d91892-2881-41a2-a7a2-908dc3646493
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.custom: ''
ms.openlocfilehash: 46225e5a332e035e4d1cc256e71c4b5d8686fd47
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60845134"
---
# <a name="use-storsimple-snapshot-manager-user-interface-to-manage-backup-jobs-and-backup-catalog"></a>Gebruik StorSimple Snapshot Manager-gebruikersinterface voor het beheren van back-ups en back-catalogus

## <a name="overview"></a>Overzicht
StorSimple Snapshot Manager heeft een intuïtieve gebruikersinterface die u gebruiken kunt en back-ups beheren. Deze zelfstudie bevat een inleiding tot de gebruikersinterface en vervolgens wordt uitgelegd hoe u het gebruik van elk van de onderdelen. Zie voor een gedetailleerde beschrijving van de StorSimple Snapshot Manager [wat is StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Beschrijving van de console
Als u de gebruikersinterface, klikt u op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het consolevenster wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding.

![Deelvensters van de StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Het consolevenster heeft vijf belangrijke elementen. Klik op de koppeling voor een volledige beschrijving van elk element.

* [In de menubalk](#menu-bar) 
* [Werkbalk](#tool-bar) 
* [Deelvenster bereik](#scope-pane) 
* [Deelvenster met resultaten](#results-pane) 
* [Actiedeelvenster](#actions-pane) 

Daarnaast StorSimple Snapshot Manager ondersteunt [navigatie en een aantal snelkoppelingen op het toetsenbord](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Toegankelijkheid van de console
De gebruikersinterface van de StorSimple Snapshot Manager biedt ondersteuning voor de toegankelijkheidsfuncties van het Windows-besturingssysteem en de Microsoft Management Console (MMC), evenals enkele StorSimple Snapshot Manager-specifieke sneltoetsen toe. 

* Voor een beschrijving van de toegankelijkheidsfuncties van Windows, gaat u naar [sneltoetsen voor Windows](https://support.microsoft.com/kb/126449). 
* Voor een beschrijving van de MMC-toegankelijkheidsfuncties, gaat u naar [toegankelijkheid voor MMC 3.0](https://technet.microsoft.com/library/cc766075.aspx)
* Voor een beschrijving van de StorSimple Snapshot Manager toegankelijkheidsfuncties, gaat u naar [navigatie en snelkoppelingen op het toetsenbord](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>In de menubalk
De menubalk aan de bovenkant van het consolevenster bevat [bestand](#file-menu), [actie](#action-menu), [weergave](#view-menu), [Favorieten](#favorites-menu), [venster](#window-menu), en [Help](#help-menu) menu's.

Klik op een willekeurig item in het menu voor een overzicht van de beschikbare opdrachten die in het menu. Het volgende voorbeeld wordt de **weergave** menu is geselecteerd op de menubalk.

![Menu Beeld geselecteerd](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Menu Bestand
De **bestand** menu bevat de standaardopdrachten van Microsoft Management Console (MMC).

#### <a name="menu-access"></a>Toegang tot menu
Om weer te geven de **bestand** menu, klikt u op **bestand** op de menubalk. De volgende menu wordt weergegeven.

![Menu bestand StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft items die worden weergegeven op de **bestand** menu.

| Menu-item | Description |
|:--- |:--- |
| Nieuw |Klik op **nieuw** te maken van een nieuwe console die is gebaseerd op het StorSimple Snapshot Manager. |
| Open |Klik op **Open** om een bestaande console te openen. |
| Opslaan |Klik op **opslaan** om op te slaan van de huidige console. |
| Opslaan als |Klik op **OpslaanAls** te maken van een nieuwe, nieuwe naam exemplaar van de huidige console. Gebruik de **OpslaanAls** optie voor een weergave aanpassen en bewaar het voor later gebruik. U kunt bijvoorbeeld StorSimple Snapshot Manager-modules die naar specifieke servers verwijzen maken. |
| Toevoegen/verwijderen-module |Klik op **module toevoegen/verwijderen** toevoegen of verwijderen van modules en indelen op knooppunten in de **bereik** deelvenster. Ga voor meer informatie naar [toevoegen, verwijderen, en indelen-modules en -extensies in MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Opties |Klik op **opties** gebruikersmodi toegang en machtigingen opgeven als u wilt wijzigen op het pictogram van de console, of consolebestanden verwijderen om schijfruimte te verhogen. |
| Lijst met paden |Klik op een pad in de genummerde lijst te openen van een bestand dat u onlangs hebt geopend. |
| Afsluiten |Klik op **afsluiten** sluiten de **bestand** menu. |

### <a name="action-menu"></a>Menu voor Rapportacties
Gebruik de **actie** menu selecteren uit beschikbare acties. De items die voor u beschikbaar zijn afhankelijk van de selectie die u in de **bereik** deelvenster of **resultaten** deelvenster.

#### <a name="menu-access"></a>Toegang tot menu
Om weer te geven de **actie** menu, een van de volgende handelingen uit:

* Met de rechtermuisknop op een item in de **bereik** deelvenster of **resultaten** deelvenster.
* Selecteer een item in de **bereik** deelvenster of **resultaten** deelvenster en klik vervolgens op **actie** op de menubalk. 

Bijvoorbeeld, als u het bovenste knooppunt in de **bereik** in het deelvenster en klik vervolgens met de rechtermuisknop op of klik op **actie** in de menubalk van de volgende menu wordt weergegeven.

![Menu Actie van de StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

De **acties** deelvenster (aan de rechterkant van de console) bevat de dezelfde lijst met acties als het **actie** menu. Bovendien de **acties** deelvenster bevat de **weergave** menuopties, waarmee u kunt een aangepaste weergave maken van de **resultaten** deelvenster.

![Open het actiedeelvenster met menu Beeld](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel bevat een alfabetische lijst van acties voor StorSimple Snapshot Manager. 

* De **actie** kolom geeft een lijst van acties die u op de knooppunten en de resultaten uitvoeren kunt. 
* De **navigatie** kolom wordt uitgelegd hoe u om weer te geven de juiste **actie** menu zodat u kunt de actie selecteren. Sommige acties worden weergegeven in meerdere **actie** menu's. Selecteer voor deze acties een **navigatie** optie in de lijst met opsommingstekens. 
* De **beschrijving** kolom wordt beschreven hoe u van elke actie op de **actie** menu of het deelvenster Acties, en wordt uitgelegd wat het doet.

> [!NOTE]
> De **acties** deelvenster en **actie** menu's bevatten extra opties, zoals **weergave**, **nieuw venster hier**,  **Vernieuwen**, **lijst exporteren**, en **Help**. Deze opties zijn beschikbaar als onderdeel van de MMC-module en zijn niet specifiek voor StorSimple Snapshot Manager. De tabel bevat beschrijvingen van deze opties.
> 
> 

| Bewerking | Navigatie | Description |
|:--- |:--- |:--- |
| Verifiëren |Klik op de **apparaten** knooppunt, en met de rechtermuisknop op een apparaat in de **resultaten** deelvenster. |Klik op **verifiëren** het wachtwoord die u hebt geconfigureerd voor het apparaat invoeren. |
| Klonen |Vouw **back-catalogus**, vouw **Cloudmomentopnamen**, klikt u op een gedateerde back-up en selecteer vervolgens een volume in de **resultaten** deelvenster. |Klik op **kloon** een kopie van een cloud-momentopname maken en opslaan in een locatie die u opgeeft. |
| Een apparaat configureren |Met de rechtermuisknop op de **apparaten** knooppunt. |Klik op **configureren van een apparaat** configureren voor een apparaat of meerdere apparaten verbinding maken met de Windows-host. |
| Back-upbeleid maken |Voer een van de volgende handelingen uit:<ul><li>Met de rechtermuisknop op **back-upbeleid**.</li><li>Klik op of uitbreiden van **Volumegroepen**, en klik vervolgens met de rechtermuisknop op een volumegroep.</li><li>Klik op of uitbreiden van **back-upcatalogus**, en klik vervolgens met de rechtermuisknop op een volumegroep.</li></ul> |Klik op **back-upbeleid maken** het configureren van een geplande back-up voor een volumegroep. |
| Volume-groep maken |Voer een van de volgende handelingen uit:<ul><li>Klik op de **Volumes** knooppunt, en klik vervolgens met de rechtermuisknop op een volume in de **resultaten** deelvenster.</li><li>Met de rechtermuisknop op de **Volumegroepen** knooppunt.</li></ul> |Klik op **Volumegroep maken** volumes toewijzen aan een volumegroep. |
| Verwijderen |Klik op een knooppunt of resultaat (dit item wordt weergegeven in veel **actie** menu's en **acties** deelvensters.) |Klik op **verwijderen** verwijderen van het knooppunt of het resultaat dat u hebt geselecteerd. Wanneer het bevestigingsdialoogvenster wordt weergegeven, bevestigen of de verwijdering annuleert. |
| Details |Klik op de **apparaten** knooppunt, en klik vervolgens met de rechtermuisknop op een apparaat in de **resultaten** deelvenster. |Klik op **Details** om de configuratiedetails voor een apparaat te bekijken. |
| Bewerken |Klik op **back-upbeleid**, en klik vervolgens met de rechtermuisknop op een beleid in de **resultaten** deelvenster. |Klik op **bewerken** te wijzigen van de back-upschema voor een volumegroep. |
| Lijst exporteren |Klik op een knooppunt of resultaat (dit item wordt weergegeven op alle **actie** menu's en **acties** deelvensters.) |Klik op **lijst exporteren** voor het opslaan van een lijst in een bestand met door komma's gescheiden waarden (CSV). U kunt dit bestand vervolgens importeren in een werkbladtoepassing voor analyse. |
| Help |Klik op een knooppunt of het resultaat. (Dit item wordt weergegeven op alle **actie** menu's en **acties** deelvensters.) |Klik op **Help** online-Help openen in een nieuw browservenster. |
| Nieuw venster |Klik op een knooppunt of resultaat (dit item wordt weergegeven op alle **actie** menu's en **acties** deelvensters.) |Klik op **nieuw venster hier** om een nieuwe StorSimple Snapshot Manager-venster te openen. |
| Vernieuwen |Klik op een knooppunt of resultaat (dit item wordt weergegeven op alle **actie** menu's en **acties** deelvensters.) |Klik op **vernieuwen** om bij te werken van het momenteel weergegeven StorSimple Snapshot Manager-venster. |
| Vernieuwen van apparaat |Klik op de **apparaten** knooppunt, en met de rechtermuisknop op een apparaat in de **resultaten** deelvenster. |Klik op **apparaat vernieuwen** naar een specifieke verbonden apparaat synchroniseren met StorSimple Snapshot Manager. |
| Vernieuwen van apparaten |Met de rechtermuisknop op de **apparaten** knooppunt. |Klik op **apparaten vernieuwen** aan uw lijst met verbonden apparaten synchroniseren met StorSimple Snapshot Manager. |
| Volumes opnieuw scannen |Met de rechtermuisknop op de **Volumes** knooppunt. |Klik op **volumes opnieuw scannen** om bij te werken de lijst van volumes die wordt weergegeven in de **resultaten** deelvenster. |
| Herstellen |Vouw **back-catalogus**, vouwt u de volumegroep van een uit, vouw **lokale momentopnamen** of **Cloudmomentopnamen**, en klik vervolgens met de rechtermuisknop op een back-up. |Klik op **herstellen** ter vervanging van gegevens van de huidige volume met de gegevens van de geselecteerde back-up. |
| Back-up maken |Voer een van de volgende handelingen uit:<ul><li>Vouw **Volumegroepen**, en klik vervolgens met de rechtermuisknop op een volumegroep.</li><li>Vouw **back-upcatalogus**, en klik vervolgens met de rechtermuisknop op een volumegroep.</li></ul> |Klik op **back-ups nemen** onmiddellijk starten van een back-uptaak. |
| In-/ uitschakelen invoer weergeven |Met de rechtermuisknop op het bovenste knooppunt in de **bereik** deelvenster (de **StorSimple Snapshot Manager** knooppunt in de voorbeelden). |Klik op **in-/ uitschakelen invoer weergeven** weergeven of verbergen van de volumegroepen en gekoppelde back-ups die zijn geïmporteerd uit het dashboard van de StorSimple Device Manager. |

### <a name="view-menu"></a>Menu Beeld
Gebruik de **weergave** menu voor het maken van een aangepaste weergave van de **resultaten** deelvenster inhoud. De **weergave** menu bevat **kolommen toevoegen/verwijderen** en **aanpassen** opties.

#### <a name="menu-access"></a>Toegang tot menu
U hebt toegang tot de **weergave** in het menu van de balk of in de **acties** deelvenster.

![Menu Beeld van de StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft items die worden weergegeven op de **weergave** menu.

| Menu-item | Description |
|:--- |:--- |
| Kolommen toevoegen/verwijderen |Klik op **kolommen toevoegen/verwijderen** toevoegen of verwijderen van kolommen in de **resultaten** deelvenster. |
| Aanpassen |Klik op **aanpassen** weergeven of verbergen van items in het venster voor StorSimple Snapshot Manager-console. |

### <a name="favorites-menu"></a>Menu voor Favorieten
Gebruik de **Favorieten** menu toevoegen, verwijderen en organiseren van paginaweergaven en taken die u vaak gebruikt. 

#### <a name="menu-access"></a>Toegang tot menu
U hebt toegang tot de **Favorieten** menu op de menubalk.

![StorSimple Snapshot Manager Favorieten](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft items die worden weergegeven op de **Favorieten** menu.

| Menu-item | Description |
|:--- |:--- |
| Toevoegen aan Favorieten |Klik op **toevoegen aan Favorieten** om toe te voegen van de huidige weergave aan uw lijst met Favorieten. |
| Favorieten indelen |Klik op **Favorieten indelen** te organiseren van de inhoud van de map met Favorieten. |

### <a name="window-menu"></a>Venstermenu
Gebruik de **venster** menu toe te voegen en de volgorde van windows voor StorSimple Snapshot Manager-console.

#### <a name="menu-access"></a>Toegang tot menu
U hebt toegang tot de **venster** menu op de menubalk.

![Menu venster StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Aan de onderkant van het menu van de genummerde lijst ziet u de vensters die momenteel zijn geopend. Klik op een venster in de lijst om het venster in de voorgrond is geplaatst. 

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft de items die worden weergegeven in het menu venster.

| Menu-item | Description |
|:--- |:--- |
| Nieuw venster |Klik op **nieuw venster** om een nieuwe consolevenster (naast de bestaande venster) te openen. |
| Cascade |Klik op **Cascade** om weer te geven van de windows-console openen in een CSS-stijl. |
| Naast elkaar weergeven |Klik op **horizontaal tegel** om weer te geven van de windows-console openen in een tegel (of raster)-indeling. |
| Pictogrammen schikken |Als u meerdere console windows openen en verspreid over uw bureaublad te minimaliseren en klik vervolgens op **pictogrammen schikken** deze rangschikken in een horizontale rij aan de onderkant van het scherm. |

### <a name="help-menu"></a>Menu Help
Gebruik de **Help** menu om beschikbare online-help voor StorSimple Snapshot Manager en de MMC-module weer te geven. U kunt ook informatie bekijken over de MMC en StorSimple Snapshot Manager-software-versies die momenteel zijn geïnstalleerd op uw systeem. 

U hebt toegang tot de **Help** menu op de menubalk. U kunt ook toegang tot StorSimple Snapshot Manager help-onderwerpen uit de **acties** deelvenster.

![StorSimple Snapshot Manager Help-menu](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menu-beschrijving
De volgende tabel beschrijft items die worden weergegeven in het menu Help.

| Menu-item | Description |
|:--- |:--- |
| Help bij StorSimple Snapshot Manager |Klik op **op StorSimple Snapshot Manager Help** StorSimple Snapshot Manager help openen in een apart venster. |
| Help-onderwerpen |Klik op **Help-onderwerpen** online help van MMC openen in een apart venster. |
| TechCenter-website |Klik op **TechCenter-website** naar de startpagina van Microsoft TechNet Tech Center openen in een apart venster. |
| Over Microsoft Management Console |Klik op **over Microsoft Management Console** om te zien welke versie van de Microsoft Management Console is geïnstalleerd op uw systeem. |
| Over StorSimple Snapshot Manager |Klik op **over StorSimple Snapshot Manager** om te zien welke versie van de module is geïnstalleerd op uw systeem. |

## <a name="tool-bar"></a>Werkbalk
De werkbalk, zich onder de menubalk bevat pictogrammen voor navigatie en taak. Elk pictogram is een snelkoppeling naar een specifieke taak.

### <a name="icon-descriptions"></a>Pictogram beschrijvingen
De volgende tabel beschrijft de pictogrammen die worden weergegeven op de werkbalk. 

| Pictogram | Description |
|:--- |:--- |
| ![Pijl-links](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) |Klik op het pictogram Pijl-links om terug te keren naar de vorige pagina. |
| ![Pijl-rechts](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) |Klik op de pijl-rechts om naar de volgende pagina te gaan (als de pijl grijs wordt weergegeven, de actie is niet beschikbaar). |
| ![Pictogram omhoog](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) |Klik op het pictogram van één niveau in de consolestructuur omhoog (de **bereik** deelvenster). |
| ![Structuur van de console weergeven/verbergen](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) |Klik op het pictogram weergeven/verbergen console boomstructuur wilt weergeven of verbergen de **bereik** deelvenster. |
| ![Lijst exporteren](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) |Klik op het pictogram van de lijst exporteren om een lijst exporteren naar een CSV-bestand dat u opgeeft. |
| ![Help-pictogram](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png) |Klik op het Helppictogram om een online MMC-help-onderwerp. |
| ![Actiedeelvenster weergeven/verbergen](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) |Klik op de weergeven/verbergen **acties** pictogram in het deelvenster wilt weergeven of verbergen de **acties** deelvenster. |

## <a name="scope-pane"></a>Deelvenster bereik
De **bereik** deelvenster is het meest linkse deelvenster in de StorSimple Snapshot Manager-UI. Het bevat de structuur van de console (of knooppunt) en is het primaire navigatie-mechanisme voor StorSimple Snapshot Manager. 

### <a name="scope-pane-structure"></a>Bereik deelvenster Structuur
De **bereik** deelvenster bevat een reeks geklikt objecten (knooppunten) in een boomstructuur gerangschikt. 

![Deelvenster bereik](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

* Als u wilt uitvouwen of samenvouwen van een knooppunt, klikt u op het pijltje naast de naam van het knooppunt.
* Als u wilt weergeven van de status of de inhoud van een knooppunt, klik op de naam van het knooppunt. De informatie wordt weergegeven de **resultaten** deelvenster. 

De **bereik** deelvenster bevat de volgende knooppunten: 

* [Knooppunt apparaten](#devices-node) 
* [Volumes knooppunt](#volumes-node) 
* [Knooppunt voor volume-groepen](#volume-groups-node) 
* [Back-knooppunt van het beleid](#backup-policies-node) 
* [Back-catalogus-knooppunt](#backup-catalog-node) 
* [Taken-knooppunt](#jobs-node) 

### <a name="scope-pane-tasks"></a>Bereik deelvenster Taken
U kunt de **bereik** deelvenster om een actie op een specifiek knooppunt te voltooien. Als u wilt een taak selecteert, moet u een van de volgende handelingen:

* Met de rechtermuisknop op het knooppunt en selecteert u de taak in het menu dat wordt weergegeven.
* Klik op het knooppunt en klik vervolgens op **actie** op de menubalk. Selecteer de taak in het menu dat wordt weergegeven.
* Klik op het knooppunt en selecteer vervolgens de actie in de **acties** deelvenster.

Wanneer u een knooppunt selecteren en gebruik een van deze methoden om te zien van een lijst met taken, worden alleen personen deze acties die kunnen worden uitgevoerd op dat knooppunt weergegeven.

### <a name="devices-node"></a>Knooppunt apparaten
De **apparaten** knooppunt vertegenwoordigt de StorSimple-apparaten en het virtuele StorSimple-apparaten die zijn verbonden aan StorSimple Snapshot Manager. Dit knooppunt om te koppelen en configureren van een apparaat selecteren en importeren van de gekoppelde volumes, volumes groepen en bestaande back-ups. Meerdere apparaten kunnen worden verbonden met één host.

* Vouw het knooppunt uit, klik op de pijl naast **apparaten**.
* Als u wilt zien een menu met de beschikbare acties, met de rechtermuisknop op de **apparaten** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van geconfigureerde apparaten wilt bekijken, klikt u op **apparaten** in de **bereik** deelvenster. De lijst met apparaten, samen met informatie over elk apparaat wordt weergegeven in de **resultaten** deelvenster.

### <a name="volumes-node"></a>Volumes knooppunt
De **Volumes** knooppunt vertegenwoordigt de stations die overeenkomen met de volumes die zijn gekoppeld met de host, met inbegrip van die via iSCSI- en die zijn gedetecteerd via een apparaat wordt gedetecteerd. Gebruik dit knooppunt in de lijst met beschikbare volumes weergeven en toewijzen van afzonderlijke volumes aan volumegroepen.

* Vouw het knooppunt uit, klik op de pijl naast **Volumes**.
* Als u wilt zien een menu met de beschikbare acties, met de rechtermuisknop op de **Volumes** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van volumes wilt bekijken, klikt u op **Volumes** in de **bereik** deelvenster. De lijst van volumes, samen met informatie over elk volume wordt weergegeven in de **resultaten** deelvenster.

### <a name="volume-groups-node"></a>Knooppunt voor volume-groepen
Volumegroepen worden ook wel bekend als consistentiegroepen. Elke volumegroep is een groep van toepassingen met betrekking tot volumes waarmee u kunt zorgen voor toepassingsconsistentie tijdens de back-upbewerkingen. Gebruik de **Volumegroepen** knooppunt voor deze groepen configureren en voor het uitvoeren van interactieve back-ups of back-upschema's maken. 

* Vouw het knooppunt uit, klik op de pijl naast **Volumegroepen**.
* Als u wilt zien een menu met de beschikbare acties, met de rechtermuisknop op de **Volumegroepen** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van de volumegroepen wilt bekijken, klikt u op **Volumegroepen** in de **bereik** deelvenster. De lijst met volumegroepen, samen met informatie over elke volumegroep wordt weergegeven in de **resultaten** deelvenster.

### <a name="backup-policies-node"></a>Back-knooppunt van het beleid
Back-upbeleid zijn taakschema's voor lokale en cloud-momentopnamen. Gebruik de **back-upbeleid** knooppunt om op te geven hoe vaak een back-up wordt gemaakt en hoe lang een back-up moet worden bewaard. 

* Vouw het knooppunt uit, klik op de pijl naast **back-upbeleid**.
* Als u wilt zien een menu met de beschikbare acties, met de rechtermuisknop op de **back-upbeleid** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van de back-upbeleid wilt bekijken, klikt u op **back-upbeleid** in de **bereik** deelvenster. De lijst met back-upbeleid, samen met informatie over elk beleid wordt weergegeven in de **resultaten** deelvenster.

> [!NOTE]
> U kunt maximaal 64 back-ups behouden.


### <a name="backup-catalog-node"></a>Back-catalogus-knooppunt
De **back-upcatalogus** knooppunt bevat een lijst met lokale en externe back-ups van Azure StorSimple-volumes. Dit knooppunt is gerangschikt op volume en afzonderlijke structuren voor lokale momentopnamen voor elke groep volumecontainer bevat (de **lokale momentopname**s knooppunt) en cloud-momentopnamen (de **Cloudmomentopnamen** knooppunt). Wanneer uitgevouwen, bevat elke groep volumecontainer alle de geslaagde back-ups die zijn uitgevoerd interactief of door een geconfigureerde beleid.

* Vouw het knooppunt uit, klik op de pijl naast **back-upcatalogus**.
* Als u wilt zien een menu met de beschikbare acties, met de rechtermuisknop op de **back-upcatalogus** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van de Backup-momentopnamen wilt bekijken, klikt u op **back-catalogus** in de **bereik** deelvenster. De lijst met momentopnamen, samen met informatie over elke momentopname wordt weergegeven in de **resultaten** deelvenster.

### <a name="local-snapshots-node"></a>Momentopnamen van het lokale knooppunt
De **lokale momentopnamen** knooppunt geeft een lijst van de lokale momentopnamen voor een bepaald volume-groep. Het knooppunt bevindt zich onder de **back-upcatalogus** knooppunt in de **bereik** deelvenster. Lokale momentopnamen zijn point-in-time-kopieën van volumegegevens die zijn opgeslagen op de Azure StorSimple-apparaat. Normaal gesproken worden dit type back-up gemaakt en hersteld snel. U kunt een momentopname van een lokale kunt gebruiken, net als een lokale back-up.

* Vouw het knooppunt uit, klik op de pijl naast **lokale momentopnamen**.
* Als u wilt zien een menu met de beschikbare acties, met de rechtermuisknop op de **lokale momentopnamen** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van de lokale momentopnamen wilt bekijken, klikt u op **lokale momentopnamen** in de **bereik** deelvenster. De lijst met momentopnamen, samen met informatie over elke momentopname wordt weergegeven in de **resultaten** deelvenster.

### <a name="cloud-snapshots-node"></a>Knooppunt voor cloud-momentopnamen
De **Cloudmomentopnamen** knooppunt geeft een lijst van cloudmomentopnamen voor een bepaald volume-groep. Het knooppunt bevindt zich onder de **back-upcatalogus** knooppunt in de **bereik** deelvenster. Cloudmomentopnamen zijn point-in-time-kopieën van volumegegevens die zijn opgeslagen in de cloud. Een cloud-momentopname is gelijk aan een momentopname op een andere, externe opslagsysteem gerepliceerd. Cloudmomentopnamen zijn met name handig voor herstel na noodgevallen.

* Vouw het knooppunt uit, klik op de pijl naast **Cloudmomentopnamen**.
* Als u wilt zien een menu met de beschikbare acties, met de rechtermuisknop op de **Cloudmomentopnamen** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van cloudmomentopnamen wilt bekijken, klikt u op **Cloudmomentopnamen** in de **bereik** deelvenster. De lijst met momentopnamen, samen met informatie over elke momentopname wordt weergegeven in de **resultaten** deelvenster.

### <a name="jobs-node"></a>Taken-knooppunt
De **taken** knooppunt bevat informatie over geplande, actieve en recentelijk uitgevoerde back-uptaken. 

* Vouw het knooppunt uit, klik op de pijl naast **taken**.
* Als u wilt zien een menu met de beschikbare acties, met de rechtermuisknop op de **taken** knooppunt of met de rechtermuisknop op een van de knooppunten die worden weergegeven in de uitgevouwen weergave.
* Een lijst van geplande taken wilt bekijken, vouw de **taken** knooppunt en klik vervolgens op **geplande**. De lijst met eerder geconfigureerde taken en informatie over elke taak wordt weergegeven in de **resultaten** deelvenster. 
* Een lijst van de laatst voltooide taken wilt bekijken, vouw de **taken** knooppunt en klik vervolgens op **afgelopen 24 uur**. Een lijst met taken die in de afgelopen 24 uur zijn voltooid wordt weergegeven in de **resultaten** deelvenster. De **resultaten** deelvenster bevat ook informatie over elke voltooide taak.
* U ziet een lijst met taken die momenteel worden uitgevoerd, vouw de **taken** knooppunt en klik vervolgens op **met**. De lijst met actieve taken en informatie over elke taak wordt weergegeven in de **resultaten** deelvenster.

## <a name="results-pane"></a>Deelvenster met resultaten
De **resultaten** deelvenster is het middelste deelvenster van de StorSimple Snapshot Manager-UI. Het bevat een lijst met en gedetailleerde statusinformatie voor het knooppunt dat u hebt geselecteerd in de **bereik** deelvenster.

### <a name="example"></a>Voorbeeld
Voor het volgende voorbeeld, klikt u op de **Volumegroepen** knooppunt in de **bereik** deelvenster. De **resultaten** deelvenster geeft een lijst van volumegroepen met informatie over elke groep.

![Deelvenster met resultaten](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

U kunt de details weergegeven in de **resultaten** deelvenster: met de rechtermuisknop op een knooppunt in de **bereik** deelvenster, klikt u op **weergave**, en klik vervolgens op **kolommen toevoegen/verwijderen** .

## <a name="actions-pane"></a>Actiedeelvenster
De **acties** deelvenster is het rechter deelvenster in de StorSimple Snapshot Manager-UI. Deze bevat een menu met bewerkingen die u op het knooppunt, weergave of gegevens die u selecteert uitvoeren kunt in de **bereik** deelvenster of **resultaten** deelvenster. De **acties** deelvenster bevat dezelfde opdrachten als de **actie** menu's die beschikbaar zijn voor items in de **bereik** deelvenster en **resultaten** deelvenster. Zie voor een beschrijving van elke actie, de tabel in de **actie** sectie menu.

### <a name="examples"></a>Voorbeelden
Om te zien in het volgende voorbeeld, in de **bereik** deelvenster, vouw de **taken** knooppunt en klikt u op **geplande**. De **acties** deelvenster ziet u de beschikbare acties voor de **geplande** knooppunt.

![Voorbeeld van de geplande taken deelvenster Acties](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

Voor meer opties de **bereik** deelvenster, vouw de **taken** knooppunt, klikt u op **geplande**, en klik vervolgens op een geplande taak in de **resultaten** deelvenster. De **acties** deelvenster ziet u de beschikbare acties voor de geplande taak, zoals wordt weergegeven in het volgende voorbeeld.

![Voorbeeld van de taak acties deelvenster Acties](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Toetsenbordnavigatie en snelkoppelingen
StorSimple Snapshot Manager kunt de toegankelijkheidsfuncties van het Windows-besturingssysteem en de Microsoft Management Console (MMC). Dit omvat ook enkele functies voor Codenavigatie toetsenbord en snelkoppelingen die specifiek is voor de StorSimple Snapshot Manager zijn zoals wordt beschreven in de volgende secties.

* [Navigatie-toetsen](#keyboard-navigation-keys) 
* [Menubalk sneltoetsen](#menu-bar-shortcut-keys) 
* [Sneltoetsen in het bereik](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Navigatie-toetsen
De volgende tabel beschrijft de sleutels die u gebruiken kunt om te navigeren van de gebruikersinterface van de StorSimple Snapshot Manager. 

| Navigatie-sleutel | Bewerking |
|:--- |:--- |
| Pijltoets-omlaag |Gebruik de pijltoets-omlaag verticaal moet worden verplaatst naar het volgende item in een menu of deelvenster. |
| Enter |Druk op Enter drukken om een actie voltooien en ga daarna verder met de volgende stap. Bijvoorbeeld, drukt u op Enter om te selecteren **volgende**, **OK**, of **maken**, en ga vervolgens naar de volgende stap in een wizard. |
| Esc |Druk op ESC om een menu te sluiten of te annuleren en sluiten van een pagina. |
| F1 |Druk op F1 drukt om een help-onderwerp voor het actieve venster weer te geven. |
| F5 |Druk op F5 om te vernieuwen van een knooppunt. |
| F6 |Druk op F6 om te verplaatsen van de **bereik** deelvenster aan de **resultaten** deelvenster. |
| F10 |Druk op F10 om te gaan naar de menubalk. |
| Pijl-links |Gebruik de pijltoets-links horizontaal van een balk menuoptie verplaatsen naar de vorige optie. Wanneer u naar het vorige item op de menubalk verplaatst, wordt het menu Actie (of context) voor het vorige item weergegeven. |
| Pijl-rechts |Gebruik de toets pijl-rechts horizontaal van één optie balk verplaatsen naar de volgende. Wanneer u naar het volgende item in de menubalk verplaatst, wordt het menu Actie (of context) voor het nieuwe item weergegeven. |
| TAB-toets |Gebruik de Tab-toets te verplaatsen naar het volgende deelvenster in de console of aan de volgende selectie of tekstvak in een pagina. |
| Pijl-omhoog |Gebruik de pijltoets-omhoog verticaal moet worden verplaatst naar het vorige item in een menu of deelvenster. |

### <a name="menu-bar-shortcut-keys"></a>Menubalk sneltoetsen
De volgende tabel beschrijft de belangrijkste combinaties van de snelkoppeling voor de in de menubalk. Nadat u de snelkoppeling toetsen en het menu wordt geopend, kunt u sneltoetsen menu (de onderstreepte sleutels in het menu). Voor meer informatie over het in de menubalk, gaat u naar [in de menubalk](#menu-bar).

| Snelkoppeling | Resultaat | Menu sneltoets | Resultaat |
|:--- |:--- |:--- |:--- |
| ALT + F |Hiermee opent u de **bestand** menu. |N |Hiermee opent u een nieuw exemplaar van de console. |
|  |O |Hiermee opent u de **Systeembeheer** pagina. | |
|  |S |Hiermee slaat u de StorSimple Snapshot Manager-console. | |
|  |A |Hiermee opent u de **OpslaanAls** pagina. | |
|  |M |Hiermee opent u de **module toevoegen/verwijderen** pagina. | |
|  |P |Hiermee opent u de **opties** pagina. | |
|  |H |Hiermee opent u de online-Help. | |
| ALT + A |Hiermee opent u de **actie** menu. |I |Hiermee schakelt u de optie importeren in of uit. |
|  |W |Hiermee opent een nieuwe StorSimple Snapshot Manager-console. | |
|  |F |De StorSimple Snapshot Manager-console-updates. | |
|  |L |Hiermee opent u de **lijst exporteren** pagina. | |
|  |H |Hiermee opent u de online-Help. | |
| ALT + V |Hiermee opent u de **weergave** menu. |A |Hiermee opent u de **kolommen toevoegen/verwijderen** pagina. |
|  |U |Hiermee opent u de **weergave aanpassen** pagina. | |
| ALT + O |Hiermee opent u de **Favorieten** menu. |A |Hiermee opent u de **toevoegen aan Favorieten** pagina. |
|  |O |Hiermee opent u de **Favorieten indelen** pagina. | |
| ALT + G |Hiermee opent u de **venster** menu. |N |Hiermee opent een nieuw venster van de StorSimple Snapshot Manager. |
|  |C |Geeft alle geopende console windows in een cascading style. | |
|  |T |Geeft alle geopende console windows in een rasterpatroon. | |
|  |I |De pictogrammen in een horizontale rij aan de onderkant van het scherm. | |
| ALT + H |Hiermee opent u de **Help** menu. |H |Hiermee opent u de online-Help. |
|  |T |Hiermee opent de Microsoft TechNet Tech Center webpagina wordt weergegeven. | |
|  |A |Hiermee opent u de **over Microsoft Management Console** pagina. | |

### <a name="scope-pane-shortcut-keys"></a>Sneltoetsen in het bereik
De volgende tabellen tonen de snelkoppeling toetsencombinaties voor elk knooppunt in de **bereik** deelvenster. 

* [Sneltoetsen voor knooppunt apparaten](#devices-node-shortcut-keys)
* [Sneltoetsen voor volumes knooppunt](#volumes-node-shortcut-keys)
* [Sneltoetsen voor volume groepen-knooppunt](#volume-groups-node-shortcut-keys)
* [Back-up maken van beleid voor knooppunt sneltoetsen](#backup-policies-node-shortcut-keys)
* [Back-catalogus knooppunt-sneltoetsen](#backup-catalog-node-shortcut-keys)
* [Sneltoetsen voor taken knooppunt](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Sneltoetsen voor knooppunt apparaten
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| C |Hiermee opent u de **configureren van een apparaat** pagina. |
| D |Hiermee vernieuwt u de lijst met apparaten en de Apparaatdetails. |
| V |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Details** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de **lijst exporteren** pagina. |
| H |Hiermee opent u de online-Help. |

#### <a name="volumes-node-shortcut-keys"></a>Sneltoetsen voor volumes knooppunt
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| V |Werkt de lijst van volumes. |
| V (druk op twee keer) |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Volumes** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de **lijst exporteren** pagina. |
| H |Hiermee opent u de online-Help. |

#### <a name="volume-groups-node-shortcut-keys"></a>Sneltoetsen voor volume groepen-knooppunt
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| G |Hiermee opent u de **maken van een groep Volume** pagina. |
| V |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Volumegroepen** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de **lijst exporteren** pagina. |
| H |Hiermee opent u de online-Help. |

#### <a name="backup-policies-node-shortcut-keys"></a>Back-up maken van beleid voor knooppunt sneltoetsen
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| B |Hiermee opent u de **maken van een beleid** pagina. |
| V |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Volumegroepen** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de ** lijst exporteren ** pagina. |
| H |Hiermee opent u de online-Help. |

#### <a name="backup-catalog-node-shortcut-keys"></a>Back-catalogus knooppunt-sneltoetsen
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **Volumegroepen** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| H |Hiermee opent u de online-Help. |

#### <a name="jobs-node-shortcut-keys"></a>Sneltoetsen voor taken knooppunt
| Snelkoppeling menu | Resultaat |
|:--- |:--- |
| V |Hiermee opent u de **weergave** menu. |
| W |Hiermee opent u een nieuwe StorSimple Snapshot Manager-console gericht op de **taken** knooppunt. |
| F |De StorSimple Snapshot Manager-console-updates. |
| L |Hiermee opent u de **lijst exporteren** pagina. |
| H |Hiermee opent u de online-Help |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over het [StorSimple Snapshot Manager gebruiken om te verbinden en beheren van apparaten](storsimple-snapshot-manager-manage-devices.md).

