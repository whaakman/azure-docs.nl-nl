---
title: StorSimple 8000 Series Update 3 release-opmerkingen | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 Series Update 3.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c22e3859fad55a8632d62aaf5f656081d6d662bd
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>3 release-opmerkingen voor uw StorSimple 8000 serie-apparaat bijwerken
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. De StorSimple-apparaatbeheerfuncties worden automatisch volgens schema verplaatst naar de nieuwe Azure Portal. U ontvangt hierover een e-mailbericht en een portalmelding. Dit document wordt binnenkort ook ingetrokken. Zie [FAQ: Move to Azure portal](storsimple-8000-move-azure-portal-faq.md) (Veelgestelde vragen: verplaatsing naar Azure Portal) voor vragen met betrekking tot de verplaatsing.


## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen beschrijven de nieuwe functies en problemen met de kritieke open voor StorSimple 8000 Series Update 3. Ze bevatten ook een lijst van de software-updates van het StorSimple opgenomen in deze release. 

Update 3 kan worden toegepast op een StorSimple-apparaat met Release (GA) of Update 0.1 via Update 2.2. De versie van het apparaat gekoppeld Update 3 is 6.3.9600.17759.

Controleer de informatie in de release-opmerkingen voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Update 3 is apparaatsoftware, LSI stuurprogramma en firmware en Storport en Spaceport werkt. Het duurt ongeveer 1,5 tot 2 uur deze update te installeren. 
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze is binnenkort beschikbaar.
> 
> 

## <a name="whats-new-in-update-3"></a>Wat is er nieuw in Update 3
De volgende belangrijke verbeteringen en correcties zijn aangebracht in Update 3.

* **Ruimte vrijmaken wijzigingen geautomatiseerde** – Update 3 vanaf de algoritmen voor het vrijmaken van ruimte worden uitgevoerd op de stand-by-controller van het systeem waardoor sneller kan worden uitgevoerd. Raadpleeg voor meer informatie over de poorten die nodig zijn voor het werken met het vrijmaken van ruimte, de [StorSimple netwerkvereisten](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).
* **Prestatieverbeteringen** – Update 3 heeft verbeterde prestaties van de lezen-schrijven naar de cloud.
* **Verbeteringen migratiegerelateerde** : In deze release bug verschillende oplossingen en verbeteringen zijn gedaan voor de functie van de migratie van apparaten 5000/7000-serie naar apparaten 8000 serie. Voor meer informatie over het gebruik van de migratiefunctie, gaat u naar [5000/7000-serie apparaat te migreren naar 8000 series apparaat](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b). 
* **Verwante oplossingen bewaking** - In deze release fouten die betrekking hebben op de grafieken, servicedashboard, bewaken en apparaat dashboard zijn opgelost.

## <a name="issues-fixed-in-update-3"></a>Problemen opgelost met Update 3
De volgende tabel bevat een samenvatting van problemen die zijn verholpen in Update 3.    

| Nee | Functie | Probleem | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Host-zijde gegevensmigratie |In de eerdere versie wordt is het StorSimple Cloud toestel gaat offline tijdens een gegevensmigratie van de host-zijde. Dit probleem is opgelost in deze release. |Nee |Ja |
| 2 |Lokaal vastgemaakte volumes |In de vorige release zijn er problemen met betrekking tot i/o-fouten, volume conversie storingen en fouten gegevenspad voor lokaal vastgemaakte volumes. Deze problemen zijn hoofdmap veroorzaakt en vaste in deze release. |Ja |Nee |
| 3 |Bewaking |Er zijn meerdere problemen met betrekking tot rapportage eenheden en bewaking evenals apparaat dashboard grafieken waar onjuiste informatie wordt weergegeven voor lokaal vastgemaakte volumes. Deze problemen zijn opgelost in deze release. |Ja |Nee |
| 4 |I/o zware schrijfbewerkingen |Wanneer u StorSimple voor werkbelastingen met betrekking tot zware schrijfbewerkingen, de gebruiker zou uitvoeren in een bug incidentele waar de werkset is wordt lagen in de cloud. Dit probleem wordt opgelost in deze release. |Ja |Ja |
| 5 |Back-up maken |In bepaalde zeldzame gevallen wordt in de vorige versies van software, wanneer de gebruiker heeft een back-up van een externe kloon, ze zouden worden uitgevoerd in de cloud-fouten en de bewerking zou fout optreedt. In deze release, het probleem is opgelost en de bewerking is voltooid. |Ja |Ja |
| 6 |Back-upbeleid |In bepaalde zeldzame gevallen wordt in de eerdere releases van software, er is een fout is gerelateerd aan het verwijderen van de back-upbeleid. Dit probleem is opgelost in deze release. |Ja |Ja |

## <a name="known-issues-in-update-3"></a>Bekende problemen in Update 3
De volgende tabel bevat een samenvatting van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen / tijdelijke oplossing | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat niet zijn verbonden met waardoor geen quorum schijf gaat vervolgens de opslaggroep offline. Het veld offline blijft, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Onjuiste besturings-ID |Als een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de afbeelding is geladen vanuit het knooppunt peer de besturings-ID kan worden weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan worden dit probleem ook gezien na opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de vervangende domeincontroller voltooid is. |Ja |Nee |
| 3 |Opslagaccounts |Het opslagaccount verwijderen met behulp van de Storage-service is een niet-ondersteund scenario. Dit leidt tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 4 |Failover van apparaat |Meerdere failovers van een volumecontainer van het bronapparaat met dezelfde aan verschillende doelapparaten wordt niet ondersteund. Failover van één apparaat inactief naar meerdere apparaten, brengt de volumecontainers op de eerste failover apparaat is Gegevenseigendom gaan verloren. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u deze in de klassieke Azure portal bekijken. | |Ja |Nee |
| 5 |Installatie |Tijdens het StorSimple-Adapter voor SharePoint-installatie moet u voor ondersteuning van een apparaat IP in volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 6 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de proxy-URL HTTP als protocol opgegeven heeft. Zie [Configure web proxy for your device](storsimple-8000-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie. |Ja |Nee |
| 7 |Webproxy |Als u configureert en webproxy op een geregistreerd apparaat inschakelen, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 8 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en de hoge i/o-belasting tegenkomt, gaat u de volumes van het apparaat in een gedegradeerde status en de i/o's mislukken met een 'apparaat niet gereed'-fout. |U moet handmatig opnieuw opstarten van de Apparaatcontrollers of voer een failover apparaat deze situatie te herstellen. |Ja |Nee |
| 9 |Azure PowerShell |Wanneer u de cmdlet StorSimple gebruiken **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - eerst 1 - wacht** het eerste object selecteren, zodat u kunt een nieuwe maken **VolumeContainer** object, de cmdlet retourneert alle objecten. |Inpakken van de cmdlet tussen haakjes als volgt: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - eerst 1 - wachttijd** |Ja |Ja |
| 10 |Migratie |Wanneer meerdere volumecontainers voor migratie worden doorgegeven, is de ETA voor de meest recente back-up nauwkeurig is alleen voor de eerste volumecontainer. Bovendien start parallelle migratie nadat de eerste 4 back-ups in de eerste volumecontainer zijn gemigreerd. |Het is raadzaam dat u een volumecontainer tegelijk migreren. |Ja |Nee |
| 11 |Migratie |Na het terugzetten zijn volumes niet toegevoegd aan de back-upbeleid of de groep van de virtuele schijf. |U moet deze volumes toevoegen aan een back-upbeleid om te kunnen maken van back-ups. |Ja |Ja |
| 12 |Migratie |Nadat de migratie voltooid is, moet het 5000/7000-serie apparaat geen toegang tot de gemigreerde gegevenscontainers. |Het is raadzaam dat u de gemigreerde gegevenscontainers verwijdert nadat de migratie voltooid en doorgevoerd is. |Ja |Nee |
| 13 |Kloon en Noodherstel |Een StorSimple-apparaat met Update 1 kan niet klonen of herstel na noodgevallen bij een apparaat met 1 software vóór het bijwerken uitvoeren. |U moet het doelapparaat op 1 bijwerken zodat deze bewerkingen bijwerken |Ja |Ja |
| 14 |Migratie |Configuratieback-up voor de migratie kan op een apparaat 5000 7000-serie mislukken wanneer er groepen met geen gekoppelde volumes volume zijn. |Alle lege volume-groepen die geen gekoppelde volumes verwijderen en probeer vervolgens de configuratieback-up. |Ja |Nee |
| 15 |Azure PowerShell-cmdlets en lokaal vastgemaakte volumes |U kunt een lokaal vastgemaakt volume via Azure PowerShell-cmdlets kan niet maken. (Alle volumes die u via Azure PowerShell maakt worden gelaagd.) |Gebruik altijd de StorSimple Manager-service voor het configureren van lokaal vastgemaakte volumes. |Ja |Nee |
| 16 |Beschikbare schijfruimte voor het lokaal vastgemaakte volumes |Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor de nieuwe volumes mogelijk niet meteen bijgewerkt. De StorSimple Manager-service-updates de lokale schijfruimte ongeveer om het uur. |Wacht tot een uur voordat u probeert te maken van het nieuwe volume. |Ja |Nee |
| 17 |Lokaal vastgemaakte volumes |De hersteltaak wordt de momentopname van de tijdelijke back-up in de back-upcatalogus, maar alleen voor de duur van de hersteltaak. Bovendien beschrijft de virtuele schijf wordt een groep met voorvoegsel **tmpCollection** op de **back-upbeleid** pagina, maar alleen voor de duur van de hersteltaak. |Dit kan gebeuren als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. De hersteltaak bevat alleen gelaagde volumes, wordt vervolgens dit gedrag niet uitgevoerd als. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 18 |Lokaal vastgemaakte volumes |Als u een hersteltaak annuleren en een failover controller optreedt onmiddellijk daarna de hersteltaak ziet **mislukt** in plaats van **geannuleerd**. Als een hersteltaak mislukt en een failover controller optreedt onmiddellijk daarna de hersteltaak ziet **geannuleerd** in plaats van **mislukt**. |Dit kan gebeuren als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. De hersteltaak bevat alleen gelaagde volumes, wordt vervolgens dit gedrag niet uitgevoerd als. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 19 |Lokaal vastgemaakte volumes |Als u een hersteltaak annuleert of als een terugzetbewerking uitvalt en vervolgens een controller-failover wordt uitgevoerd, een extra hersteltaak wordt weergegeven op de **taken** pagina. |Dit kan gebeuren als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. De hersteltaak bevat alleen gelaagde volumes, wordt vervolgens dit gedrag niet uitgevoerd als. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 20 |Lokaal vastgemaakte volumes |Als u probeert een gelaagd volume (gemaakt en gekloonde met Update 1.2 of eerder) converteren naar een lokaal vastgemaakt volume en uw apparaat is bijna geen schijfruimte meer of er een storing van de cloud, kan de clone(s) zijn beschadigd. |Dit probleem treedt alleen op volumes die gemaakt en gekloonde met vooraf Update 2.1-software zijn. Dit moet een incidentele scenario. | | |
| 21 |Volumeconversie |Werk de ACRs gekoppeld aan een volume, terwijl de volumeconversie van een uitgevoerd wordt niet (gelaagd naar lokaal vastgemaakt of andersom). Bijwerken van de ACRs kan leiden tot beschadiging van gegevens. |Indien nodig, werk van de ACRs voorafgaand aan de volumeconversie en maak geen verdere ACR updates terwijl de conversie uitgevoerd wordt. | | |
| 22 |Updates |Bij het toepassen van Update 3 de **onderhoud** pagina in de klassieke Azure portal verschijnt het volgende bericht betrekking hebben op Update 2 - 'StorSimple 8000 series Update 2 biedt de mogelijkheid voor Microsoft proactief verzamelen logboek gegevens van uw apparaat wanneer we potentiële problemen detecteren'. Dit is misleidend zoals betekent dit dat het apparaat met Update 2 wordt bijgewerkt. Nadat het apparaat bijgewerkt naar Update 3 succeesfully is, verdwijnt dit bericht. |Dit probleem wordt opgelost in een toekomstige release. |Ja |Nee |

## <a name="controller-and-firmware-updates-in-update-3"></a>Controller en firmware-updates in de Update 3
Deze release is LSI stuurprogramma en firmware-updates. Zie voor meer informatie over het installeren van de LSI stuurprogramma's en firmware-updates [installeren Update 3](storsimple-install-update-3.md) op uw StorSimple-apparaat.

## <a name="virtual-device-updates-in-update-3"></a>Virtueel apparaat-updates in de Update 3
Deze update kan niet worden toegepast op het StorSimple-Cloud-apparaat (ook wel bekend als het virtuele apparaat). Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap
Meer informatie over hoe [installeren Update 3](storsimple-install-update-3.md) op uw StorSimple-apparaat.

