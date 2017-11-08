---
title: StorSimple 8000 Series Update 2.2 release-opmerkingen | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 Series Update 2.2.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5cf03ea8-2a0f-4552-b6dc-7ea517783d7b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 06c14bdd24dd24a98b3838a2ba73b657ce56785a
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 Series Update 2.2 release-opmerkingen
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen beschrijven de nieuwe functies en problemen met de kritieke open voor StorSimple 8000 Series Update 2.2. Ze bevatten ook een lijst van de software-updates van het StorSimple opgenomen in deze release.

Update 2.2 kan worden toegepast op een StorSimple-apparaat met Release (GA) of Update 0.1 via Update 2.1. De versie van het apparaat gekoppeld Update 2.2 is 6.3.9600.17708.

Controleer de informatie in de release-opmerkingen voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * 2.2-update heeft alleen software-updates. Het duurt ongeveer 1,5 tot 2 uur deze update te installeren. 
> * Als u Update 2.1 worden uitgevoerd, raden wij aan dat u de Update 2.2 zo snel mogelijk toepassen.
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze is binnenkort beschikbaar.
> 
> 

## <a name="whats-new-in-update-22"></a>Wat is er nieuw in Update 2.2
De volgende belangrijke verbeteringen zijn aangebracht in Update 2.2.

* **Ruimte vrijmaken optimalisatie geautomatiseerde** – wanneer gegevens worden verwijderd op dun ingerichte volumes, niet-gebruikte opslag blokken moeten worden vrijgemaakt. Deze release is het proces voor het vrijmaken van ruimte uit de cloud waardoor de ongebruikte ruimte wordt sneller beschikbaar in vergelijking met vorige versies worden verbeterd.
* **Prestatieverbeteringen momentopname** – Update 2.2 de tijd om een momentopname in bepaalde scenario's waarin grote volumes worden gebruikt en er is minimaal aan geen gegevensverloop cloud is verbeterd. Een scenario dat u van deze uitbreiding profiteren wilt is de archief-volumes.
* **Beperken van ondersteuningspakket verzamelen** : Er zijn verbeteringen in de manier waarop het pakket ondersteuning wordt verzameld en geüpload in deze release. 
* **Bijwerken van verbeteringen van de betrouwbaarheid** – deze release bevat oplossingen voor problemen die in een verbeterde betrouwbaarheid van de Update resulteren.

## <a name="issues-fixed-in-update-22"></a>Problemen in Update 2.2 opgelost
De volgende tabel bevat een samenvatting van problemen die Updates 2.2 en 2.1 zijn opgelost.    

| Nee | Functie | Probleem | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Host-prestaties |Host-zijde prestatieproblemen zijn in de eerdere versie wordt waargenomen tijdens het maken van een lokaal vastgemaakt volume en tijdens de conversie van een gelaagd volume naar een lokaal vastgemaakt volume. Deze problemen zijn opgelost in deze release waardoor wat leidt tot een verbetering van de prestaties van de host tijdens de volume-procedures voor het maken en de conversie. |Ja |Nee |
| 2 |Lokaal vastgemaakte volumes |In zeldzame gevallen, zou het systeem vastlopen bij het maken van een lokaal vastgemaakt volume. Deze fout is verholpen in deze release. |Ja |Nee |
| 3 |lagen |Er zijn sporadisch crashes wanneer de metagegevens voor de StorSimple Cloud toestellen (8010 en 8020) naar de cloud lagen. Dit probleem is opgelost in deze release. |Nee |Ja |
| 4 |Het maken van momentopnamen |Er zijn problemen met betrekking tot het maken van incrementele momentopnamen in scenario's met grote volumes en minimale naar geen gegevensverloop. Deze problemen zijn opgelost in deze release. |Ja |Ja |
| 5 |Openstack verificatie |Wanneer u Openstack als de cloudserviceprovider, de gebruiker zou uitvoeren in een incidentele fout verband houden met de verificatie waarbij de JSON-parser resulteerde in een crash. Dit probleem wordt opgelost in deze release. |Ja |Nee |
| 6 |Host-zijde kopiëren |In eerdere versies van software is een incidentele oplossingen die betrekking hebben op het tijdstip ODX gezien wanneer u de gegevens van één volume kopieert naar een ander volume. Dit resulteert in een failover van de domeincontroller en het systeem kan mogelijk gaat u in de herstelmodus overgaat. Dit probleem wordt opgelost in deze release. |Ja |Nee |
| 7 |Windows Management Instrumentation (WMI) |In de vorige versies van software, er zijn meerdere exemplaren van web proxy mislukt met de uitzondering '<ManagementException> fout bij het laden van Provider '. Deze fout is toegekend aan een WMI-geheugenlek en is nu opgelost. |Ja |Nee |
| 8 |Update |In bepaalde zeldzame gevallen wordt in de vorige versies van software, ontvangen de gebruiker een 'CisPowershellHcsscripterror' wanneer u probeert te scannen of updates installeren. Dit probleem is opgelost in deze release. |Ja |Ja |
| 9 |Ondersteuningspakket |In deze release zijn er verbeteringen in de manier waarop het pakket ondersteuning wordt verzameld en geüpload. |Ja |Ja |

## <a name="known-issues-in-update-22"></a>Bekende problemen in Update 2.2
De volgende tabel bevat een samenvatting van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen / tijdelijke oplossing | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat niet zijn verbonden met waardoor geen quorum schijf gaat vervolgens de opslaggroep offline. Het veld offline blijft, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Onjuiste besturings-ID |Als een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de afbeelding is geladen vanuit het knooppunt peer de besturings-ID kan worden weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan worden dit probleem ook gezien na opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de vervangende domeincontroller voltooid is. |Ja |Nee |
| 3 |Opslagaccounts |Het opslagaccount verwijderen met behulp van de Storage-service is een niet-ondersteund scenario. Dit leidt tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 4 |Failover van apparaat |Meerdere failovers van een volumecontainer van het bronapparaat met dezelfde aan verschillende doelapparaten wordt niet ondersteund. Failover van één apparaat inactief naar meerdere apparaten, brengt de volumecontainers op de eerste failover apparaat is Gegevenseigendom gaan verloren. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u deze in de klassieke Azure portal bekijken. | |Ja |Nee |
| 5 |Installeren |Tijdens het StorSimple-Adapter voor SharePoint-installatie moet u voor ondersteuning van een apparaat IP in volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 6 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de proxy-URL HTTP als protocol opgegeven heeft. Zie [Configure web proxy for your device](storsimple-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie. |Ja |Nee |
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

## <a name="controller-and-firmware-updates-in-update-22"></a>Controller en firmware-updates in de Update 2.2
Deze release heeft alleen uit software-updates. Echter, als u vanuit een eerdere versie dan Update 2 bijwerken wilt, u moet installeren stuurprogramma Storport, Spaceport, en (in sommige gevallen) schijf firmware-updates op uw apparaat.

Zie voor meer informatie over het installeren van het stuurprogramma, Storport, Spaceport en firmware-updates schijf [installeren Update 2.2](storsimple-install-update-21.md) op uw StorSimple-apparaat.

## <a name="virtual-device-updates-in-update-22"></a>Virtueel apparaat-updates in de Update 2.2
Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap
Meer informatie over hoe [installeren Update 2.2](storsimple-install-update-21.md) op uw StorSimple-apparaat.

