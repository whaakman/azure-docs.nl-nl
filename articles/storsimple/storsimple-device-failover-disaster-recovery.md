---
title: StorSimple failover en herstel na noodgevallen | Microsoft Docs
description: Leer uw StorSimple-apparaat zelf, een ander fysiek apparaat of een virtueel apparaat failover.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5751598e-49c8-42b3-8121-fea5857a7d83
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2016
ms.author: alkohli
ms.openlocfilehash: bf92ffdb16b86c4033cc96ae2abb060d90f9505e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-device"></a>Failover en herstel na noodgevallen voor uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
Deze zelfstudie wordt beschreven hoe u een StorSimple-apparaat in het geval van een noodgeval failover. Een failover kunt u uw gegevens migreren vanaf een Bronapparaat in het datacenter naar het andere fysieke of zelfs een virtueel apparaat zich in hetzelfde of een andere geografische locatie. 

Herstel na noodgeval (DR) wordt beheerd via de functie van de failover-apparaat en wordt gestart vanuit de **apparaten** pagina. Deze pagina registreert alle StorSimple-apparaten die worden verbonden met uw StorSimple Manager-service. Voor elk apparaat worden de beschrijvende naam, status, ingericht en de maximale capaciteit, type en model weergegeven.

![Pagina apparaten](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

De instructies in deze zelfstudie is van toepassing op StorSimple fysieke en virtuele apparaten in alle softwareversies.

## <a name="disaster-recovery-dr-and-device-failover"></a>Noodherstel (DR) en failover-apparaat
In een (DR) noodherstelscenario, het primaire apparaat niet meer werkt. In dit geval kunt u de cloudgegevens die is gekoppeld aan het mislukte apparaat aan een ander apparaat met behulp van het primaire apparaat als de *bron* en een ander apparaat als op te geven de *doel*. U kunt een of meer volumecontainers om te migreren naar het doelapparaat selecteren. Dit proces wordt aangeduid als de *failover*. 

Tijdens de failover de volumecontainers van het bronapparaat eigendom wijzigen en worden overgedragen naar het doelapparaat. Zodra de volumecontainers eigendom wijzigen, wordt deze verwijderd van het bronapparaat. Nadat de verwijdering voltooid is, kan het doelapparaat vervolgens weer worden uitgevoerd.

De meest recente back-up is gewoonlijk na een DR gebruikt voor het herstellen van de gegevens naar het doelapparaat. Echter, als er meerdere back-upbeleid voor hetzelfde volume, vervolgens met het grootste aantal volumes een back-upbeleid opgehaald verzameld en de meest recente back-up van dit beleid wordt gebruikt om de gegevens op het doelapparaat te herstellen.

Een voorbeeld: als er twee back-upbeleid (één standaard- en een aangepaste) *defaultPol*, *customPol* met de volgende details:

* *defaultPol* : één volume *vol1*, dagelijkse begint bij 10:30 uur uitgevoerd.
* *customPol* : vier volumes *vol1*, *vol2*, *vol3*, *vol4*, dagelijkse begint bij 10:00 uur uitgevoerd.

In dit geval *customPol* wordt gebruikt als er meerdere volumes en we voor crashconsistentie prioriteren. De meest recente back-up van dit beleid wordt gebruikt om gegevens te herstellen.

## <a name="considerations-for-device-failover"></a>Overwegingen voor het apparaat failover
U kunt kiezen in het geval van een noodgeval voor failover van uw StorSimple-apparaat:

* Naar een fysiek apparaat 
* Op zichzelf
* Naar een virtueel apparaat

Voor de failover van een apparaat, houd rekening met het volgende:

* De vereisten voor herstel na Noodgevallen zijn dat alle volumes in de volumecontainers offline zijn en de volumecontainers een gekoppeld hebben cloudmomentopname. 
* De beschikbare doelapparaten voor herstel na Noodgevallen zijn apparaten die voldoende ruimte voor de geselecteerde volumecontainers hebben. 
* De apparaten die zijn verbonden met uw service, maar die niet voldoen aan de criteria van voldoende ruimte worden niet beschikbaar als doelapparaten.
* Na een DR voor een beperkte duur, de prestaties van de toegang tot gegevens kan worden beïnvloed aanzienlijk, als het apparaat moet toegang tot de gegevens vanuit de cloud en lokaal opslaan.

#### <a name="device-failover-across-software-versions"></a>Apparaat failover tussen softwareversies
Een StorSimple Manager-service in een implementatie mogelijk op meerdere apparaten, fysieke en virtuele, alle actieve verschillende software-versies. Afhankelijk van de versie van de software, de volumetypen op de apparaten mogelijk ook andere. Bijvoorbeeld, een apparaat actieve Update 2 of hoger zou gelden lokaal vastgemaakt en gelaagde volumes (met archivering wordt een subset van gelaagd). Een vooraf Update 2-apparaat aan de andere kant kan hebben lagen en archivering volumes. 

Gebruik de volgende tabel om te bepalen als u een failover naar een ander apparaat voor het uitvoeren van een ander software-versie en het gedrag van volumetypen tijdens DR.

| Failover van | Toegestaan voor het fysieke apparaat | Toegestaan voor het virtuele apparaat |
| --- | --- | --- |
| Update 2 voor het bijwerken van vooraf 1 (Release, 0,1, 0,2, 0,3) |Nee |Nee |
| Update 2 voor het bijwerken van 1 (1, 1.1, 1.2) |Ja <br></br>Als lokaal vastgemaakt of volumes of een combinatie van twee gelaagde, de volumes wordt altijd een failover uitgevoerd als lagen. |Ja<br></br>Als u lokaal vastgemaakte volumes, zijn deze dan gelaagde mislukt. |
| Update 2 voor Update 2 (latere versie) |Ja<br></br>Als u lokaal vastgemaakte of gelaagde volumes of een combinatie van twee, de volumes wordt altijd een failover uitgevoerd als het eerste volumetype; gelaagde als gelaagde en lokaal vastgemaakt als lokaal vastgemaakt. |Ja<br></br>Als u lokaal vastgemaakte volumes, zijn deze dan gelaagde mislukt. |

#### <a name="partial-failover-across-software-versions"></a>Gedeeltelijke failover tussen softwareversies
Volg deze richtlijnen als u van plan bent om uit te voeren een gedeeltelijke failover met behulp van een StorSimple-bron-apparaat met 1 vóór het bijwerken naar een doel met Update 1 of hoger. 

| Gedeeltelijke failover van | Toegestaan voor het fysieke apparaat | Toegestaan voor het virtuele apparaat |
| --- | --- | --- |
| Vooraf Update 1 (Release, 0,1, 0,2, 0,3) Update 1 of hoger |Ja, Zie hieronder voor de best practice-punt. |Ja, Zie hieronder voor de best practice-punt. |

> [!TIP]
> Er is een cloud metagegevens en gegevens indeling gewijzigd in Update 1 en latere versies. Daarom raadzaam niet een gedeeltelijke failover van vooraf Update 1 naar Update 1 of hoger. Als u een gedeeltelijke failover uitvoeren wilt, raden wij u eerst toepassen Update 1 of later op beide apparaten (bron en doel) en ga vervolgens verder met de failover. 
> 
> 

## <a name="fail-over-to-another-physical-device"></a>Failover naar een andere fysieke apparaat
De volgende stappen voor het herstellen van uw apparaat op een doelapparaat fysieke uitvoeren.

1. Controleer of dat de volumecontainer die u failover wilt uitvoeren cloudmomentopnamen is gekoppeld.
2. Op de **apparaten** pagina, klikt u op de **Volumecontainers** tabblad.
3. Selecteer een volumecontainer die u wilt een failover naar een ander apparaat. Klik op de volumecontainer lijst van volumes in deze container wilt weergeven. Selecteer een volume en klik op **Offline nemen** offline te nemen het volume. Herhaal dit proces voor de volumes in de volumecontainer.
4. Herhaal de vorige stap voor alle volumecontainers die u wilt een failover naar een ander apparaat.
5. Op de **apparaten** pagina, klikt u op **Failover**.
6. In de wizard die wordt geopend, klikt u onder **volumecontainer Kies failover**:
   
   1. Selecteer in de lijst met volumecontainers, de volumecontainers die u wilt een failover.
      **De volumecontainers met momentopnamen van de gekoppelde cloud en offline volumes worden weergegeven.**
   2. Onder **kiezen een doelapparaat** voor de volumes in de geselecteerde containers, selecteert u een doelapparaat in de vervolgkeuzelijst met beschikbare apparaten. Alleen de apparaten waarvoor de beschikbare capaciteit worden in de vervolgkeuzelijst weergegeven.
   3. Controleer ten slotte de failover-instellingen onder **bevestigen failover**. Klik op het vinkje ![vinkje](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
7. Een failover-taak gemaakt die kunnen worden bewaakt de **taken** pagina. Als de volumecontainer waarvoor u failover lokale volumes heeft, ziet u het herstellen van de afzonderlijke taken voor alle lokale volumes (en niet voor gelaagde volumes) in de container. Deze terugzetten taken kunnen geruime tijd duren om te voltooien. Is het waarschijnlijk dat de failover-taak eerder kan voltooien. Houd er rekening mee dat deze volumes beschikken over lokale garanties alleen nadat het herstellen van taken voltooid zijn. Nadat de failover is voltooid, gaat u naar de **apparaten** pagina.                                            
   
   1. Selecteer het apparaat dat is gebruikt voor het doelapparaat voor de failoverproces.
   2. Ga naar de **Volumecontainers** pagina. Volumecontainers, samen met de volumes van het oude apparaat moeten worden vermeld.

## <a name="failover-using-a-single-device"></a>Failover via één apparaat
Voer de volgende stappen uit als u slechts één apparaat hebben en moet een failover uitvoeren.

1. Momentopnamen cloud van alle volumes op uw apparaat.
2. Uw apparaat terugzetten op fabrieksinstellingen. Volg de gedetailleerde instructies in [hoe een StorSimple-apparaat opnieuw instelt fabrieksinstellingen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Uw apparaat configureren en het opnieuw registreren met StorSimple Manager-service.
4. Op de **apparaten** pagina het oude apparaat moet worden weergegeven als **Offline**. Het nieuw ingeschreven apparaat moet worden weergegeven als **Online**.
5. Voor het nieuwe apparaat, de minimale configuratie van het apparaat eerst te voltooien. 
   
   > [!IMPORTANT]
   > **Als de minimale configuratie niet eerst is voltooid, wordt uw DR mislukt als gevolg van een fout in de huidige implementatie. Dit probleem wordt opgelost in een latere release.**
   > 
   > 
6. Selecteer het oude apparaat (offline status) en klik op **Failover**. Failover voor dit apparaat in de wizard wordt weergegeven, en het doelapparaat opgeven als het nieuw ingeschreven apparaat. Raadpleeg voor gedetailleerde instructies [failover naar een ander fysiek apparaat](#fail-over-to-another-physical-device).
7. Een hersteltaak van het apparaat wordt gemaakt dat u vanaf bewaken kunt de **taken** pagina.
8. Nadat de taak is voltooid, toegang tot het nieuwe apparaat en navigeer naar de **Volumecontainers** pagina. De volumecontainers van het oude apparaat moeten nu worden gemigreerd naar het nieuwe apparaat.

## <a name="fail-over-to-a-storsimple-virtual-device"></a>Failover naar een virtueel StorSimple-apparaat
Een StorSimple-apparaat gemaakt en geconfigureerd voordat u deze procedure uitvoert, moet u hebben. Als u Update 2 wordt uitgevoerd, kunt u overwegen een virtueel apparaat 8020 voor de DR die is 64 TB en Premium-opslag gebruikt. 

Voer de volgende stappen uit voor het herstellen van het apparaat op een doel-virtuele StorSimple-apparaat.

1. Controleer of dat de volumecontainer die u failover wilt uitvoeren cloudmomentopnamen is gekoppeld.
2. Op de **apparaten** pagina, klikt u op de **Volumecontainers** tabblad.
3. Selecteer een volumecontainer die u wilt een failover naar een ander apparaat. Klik op de volumecontainer lijst van volumes in deze container wilt weergeven. Selecteer een volume en klik op **Offline nemen** offline te nemen het volume. Herhaal dit proces voor de volumes in de volumecontainer.
4. Herhaal de vorige stap voor alle volumecontainers die u wilt een failover naar een ander apparaat.
5. Op de **apparaten** pagina, klikt u op **Failover**.
6. In de wizard die wordt geopend, klikt u onder **volumecontainer Kies failover**, vult u de volgende:
   
    a. Selecteer in de lijst met volumecontainers, de volumecontainers die u wilt een failover.
   
    **De volumecontainers met momentopnamen van de gekoppelde cloud en offline volumes worden weergegeven.**
   
    b. Onder **kiest u een apparaat voor de volumes in de geselecteerde containers**, selecteer het virtuele StorSimple-apparaat in de vervolgkeuzelijst met beschikbare apparaten. **Alleen de apparaten die voldoende capaciteit hebben worden weergegeven in de vervolgkeuzelijst.**  
7. Controleer ten slotte de failover-instellingen onder **bevestigen failover**. Klik op het vinkje ![vinkje](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
8. Nadat de failover is voltooid, gaat u naar de **apparaten** pagina.
   
    a. Selecteer het virtuele StorSimple-apparaat dat is gebruikt als het doelapparaat voor de failoverproces.
   
    b. Ga naar de **Volumecontainers** pagina. Volumecontainers, samen met de volumes van het oude apparaat zou nu moeten worden vermeld.

![Video beschikbaar](./media/storsimple-device-failover-disaster-recovery/Video_icon.png) **Video beschikbaar**

Als u wilt een video die u laat zien hoe u kunt een failover via een fysiek apparaat met een virtueel apparaat in de cloud, klikt u op [hier](https://azure.microsoft.com/documentation/videos/storsimple-and-disaster-recovery/).

## <a name="failback"></a>Failback
Update 3 en hoger ondersteuning StorSimple ook voor failback. Nadat de failover voltooid is, wordt de volgende acties uitgevoerd:

* De volumecontainers die worden overgenomen worden van het bronapparaat opgeschoond.
* Een achtergrondtaak per volumecontainer (failover) wordt gestart op het bronapparaat. Als u te failback probeert terwijl de taak uitgevoerd wordt, ontvangt u een melding in die zin. U moet wachten totdat de taak is voltooid de failback starten. 
  
    De tijd voor het voltooien van de verwijdering van volumecontainers is afhankelijk van verschillende factoren zoals de hoeveelheid gegevens, de leeftijd van de gegevens, het aantal back-ups en de beschikbare netwerkbandbreedte voor de bewerking. Als u van plan bent test failovers/failbacks, raden wij volumecontainers met minder gegevens (GB) te testen. In de meeste gevallen kunt u de failback starten 24 uur na de failover voltooid is. 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
Q. **Wat gebeurt er als de DR mislukt of gedeeltelijk geslaagd is?**

A. Als de DR mislukt, wordt u aangeraden dat u het opnieuw proberen. De tweede keer ongeveer, DR weet wat alle werd uitgevoerd en wanneer het proces de eerste keer vastgelopen. Het DR-proces wordt gestart vanaf dat moment en hoger. 

Q. **Kan ik een apparaat verwijderen terwijl de apparaat-failover uitgevoerd wordt?**

A. U kunt een apparaat niet verwijderen, terwijl een DR uitgevoerd wordt. U kunt alleen het apparaat verwijderen nadat de DR voltooid is.

Q.    **Wanneer de garbagecollection begint op het bronapparaat zodat de lokale gegevens op het bronvolume is verwijderd?**

A. Garbagecollection wordt ingeschakeld op het bronapparaat pas nadat het apparaat volledig wordt opgeschoond. Het opruimen van de omvat het opruimen van objecten die zijn mislukt via van het bronapparaat zoals volumes, back-objecten (geen gegevens), volumecontainers en beleidsregels.

Q. **Wat gebeurt er als de delete-taak die is gekoppeld aan de volumecontainers in het bronapparaat mislukt?**

A.  Als de taak voor het verwijderen is mislukt, moet u de verwijdering van de volumecontainers handmatig opnieuw starten. In de **apparaten** pagina, selecteer het bronapparaat en klik op **volumecontainers**. Selecteer de volumecontainers die u niet over en in de onderkant van de pagina, klikt u op **verwijderen**. Zodra u alles hebt verwijderd de mislukte via volumecontainers op het bronapparaat, kunt u beginnen met de failback.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Herstel van zakelijke continuïteit na noodgevallen (BCDR)
Een zakelijke continuïteit (BCDR) noodherstelscenario treedt op wanneer de volledige Azure-datacenter niet meer werkt. Dit kan van invloed op uw StorSimple Manager-service en de bijbehorende StorSimple-apparaten.

Als er StorSimple-apparaten die zijn geregistreerd, net voordat een noodgeval is opgetreden, kunnen deze StorSimple-apparaten moet ondergaan fabrieksinstellingen terug te zetten. Na de sitedatabase, wordt het StorSimple-apparaat weergegeven als offline. Het StorSimple-apparaat moet worden verwijderd van de portal en fabrieksinstellingen moet worden gedaan, gevolgd door een nieuwe registratie.

## <a name="next-steps"></a>Volgende stappen
* Nadat u hebt een failover uitgevoerd, moet u mogelijk [deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-deactivate-and-delete-device.md).
* Ga voor informatie over het gebruik van de StorSimple Manager-service naar [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

