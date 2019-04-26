---
title: Opmerkingen bij de release van de StorSimple 8000 serie Update 2.2 | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 serie Update 2.2.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 5cf03ea8-2a0f-4552-b6dc-7ea517783d7b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 12d11cddf077d4d07732490255d44e89ddaf3217
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531051"
---
# <a name="storsimple-8000-series-update-22-release-notes"></a>Opmerkingen bij de release van de StorSimple 8000 serie Update 2.2

## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen beschrijven de nieuwe functies en de kritieke open problemen identificeren voor StorSimple 8000 serie Update 2.2. Ze bevatten ook een lijst van de StorSimple-software-updates in deze release is opgenomen.

Update 2.2 kan worden toegepast op alle StorSimple-apparaat met Release (GA) of Update 0.1 via Update 2.1. De versie van het apparaat dat is gekoppeld aan de Update 2.2 is 6.3.9600.17708.

Lees de informatie in de opmerkingen bij de release voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Update 2.2 heeft alleen software-updates. Het duurt ongeveer 1,5 tot 2 uur om deze update te installeren. 
> * Als u Update 2.1 uitvoert, wordt u aangeraden dat u Update 2.2 zo snel mogelijk van toepassing.
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk dat er een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze snel weer beschikbaar is.
> 
> 

## <a name="whats-new-in-update-22"></a>Wat is er nieuw in Update 2.2
De volgende belangrijke verbeteringen zijn aangebracht in Update 2.2.

* **Ruimte vrijmaken optimalisatie geautomatiseerde** : wanneer gegevens worden verwijderd op dun ingerichte volumes, niet-gebruikte opslag blokken hoeft te worden vrijgemaakt. Deze release is het proces voor het vrijmaken van ruimte vanuit de cloud die leidt tot de ongebruikte ruimte wordt sneller beschikbaar in vergelijking met vorige versies worden verbeterd.
* **Momentopname maken van prestatieverbeteringen** – Update 2.2 is de tijd voor het verwerken van een cloudmomentopname in bepaalde scenario's waarin grote volumes worden gebruikt en er is minimaal aan geen gegevensverloop verbeterd. Een scenario waarin voordeel van deze uitbreiding hebben veel is de archiefvolumes.
* **Hardening van ondersteuningspakket verzamelen** : Er zijn verbeteringen in de manier waarop het ondersteuningspakket wordt verzameld en geüpload in deze release. 
* **Verbeteringen van de betrouwbaarheid bijwerken** – deze release bevat oplossingen voor problemen die in een verbeterde betrouwbaarheid van de Update resulteren.

## <a name="issues-fixed-in-update-22"></a>Problemen opgelost in Update 2.2
De volgende tabel bevat een samenvatting van problemen die zijn verholpen in Updates 2.2 en 2.1.    

| Nee | Functie | Probleem | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Hostprestaties |In de eerdere versie waargenomen problemen met prestaties van de host-zijde tijdens het maken van een lokaal vastgemaakt volume en tijdens de conversie van een gelaagd volume naar een lokaal vastgemaakt volume. Deze problemen zijn opgelost in deze release waardoor leidt tot een verbetering van de hostprestaties tijdens de procedures voor het maken en conversie van volume. |Ja |Nee |
| 2 |Lokaal vastgemaakte volumes |In zeldzame gevallen, zou het systeem vastlopen wanneer ze het maken van een lokaal vastgemaakt volume. Deze fout is opgelost in deze release. |Ja |Nee |
| 3 |Tiering |Er zijn er kunnen sporadisch vastlopen wanneer de metagegevens voor de StorSimple Cloud Appliances (8010 en 8020) in de cloud gelaagde. Dit probleem is opgelost in deze release. |Nee |Ja |
| 4 |Momentopname maken |Er zijn problemen met betrekking tot het maken van incrementele momentopnamen in scenario's met grote volumes en minimale aan geen gegevensverloop. Deze problemen zijn opgelost in deze release. |Ja |Ja |
| 5 |Openstack-verificatie |Wanneer u Openstack als de cloudserviceprovider, de gebruiker zou uitvoeren in een incidentele fouten met betrekking tot de verificatie waar de JSON-parser heeft geresulteerd in een crash. Deze fout is opgelost in deze release. |Ja |Nee |
| 6 |Host-side copy |In eerdere versies van software is een incidentele fouten met betrekking tot de timing ODX gedetecteerd bij het kopiëren van de gegevens uit één volume naar een ander volume. Dit zou leiden tot een failover van de controller en het systeem kan mogelijk gaat u in de herstelmodus. Deze fout is opgelost in deze release. |Ja |Nee |
| 7 |Windows Management   Instrumentation (WMI) |In de vorige versies van software, er zijn meerdere exemplaren van de web proxy fout met de uitzondering '\<ManagementException > Fout bij het laden van Provider '. Deze fout is te wijten aan een WMI-geheugenlek en nu is opgelost. |Ja |Nee |
| 8 |Update |In sommige zeldzame gevallen, in de vorige versies van software, ontvangen de gebruiker een "CisPowershellHcsscripterror" bij het scannen of installeren van updates. Dit probleem is opgelost in deze release. |Ja |Ja |
| 9 |Ondersteuningspakket |In deze release zijn er verbeteringen in de manier waarop het ondersteuningspakket wordt verzameld en geüpload. |Ja |Ja |

## <a name="known-issues-in-update-22"></a>Bekende problemen in Update 2.2
De volgende tabel bevat een overzicht van de bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen / tijdelijke oplossing | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat zijn verbroken, wat resulteert in geen quorum schijf gaat vervolgens de opslaggroep offline. Dit wordt offline blijven, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, kunt u contact op met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Onjuiste besturings-ID |Wanneer een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de installatiekopie wordt geladen vanuit het peer-knooppunt kan de besturings-ID weergegeven in eerste instantie als de peer-controller-ID. In zeldzame gevallen kan dit gedrag ook worden weergegeven na het systeem opnieuw is opgestart. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de netwerkcontroller te vervangen voltooid is. |Ja |Nee |
| 3 |Opslagaccounts |Met de Storage-service voor het verwijderen van het opslagaccount is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 4 |Failover van het apparaat |Meerdere failovers van een volumecontainer van het bronapparaat dezelfde aan verschillende doelapparaten wordt niet ondersteund. Failover van één dead apparaat naar meerdere apparaten wordt de volumecontainers van het eerste apparaat een failover geen eigenaar van gegevens maken. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u ze in de klassieke Azure portal weergeven. | |Ja |Nee |
| 5 |Installatie |Tijdens de StorSimple-Adapter voor SharePoint-installatie moet u voor een IP-adres van het apparaat in de volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 6 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de apparaat-naar-servicecommunicatie worden beïnvloed en het apparaat gaat offline. Voor ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de URL van proxy HTTP als het opgegeven protocol heeft. Zie [Configure web proxy for your device](storsimple-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie. |Ja |Nee |
| 7 |Webproxy |Als u configureren en webproxy op een geregistreerd apparaat inschakelt, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 8 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en hoge i/o-werkbelasting tegenkomt, de apparaatvolumes gaat u in een gedegradeerde status en de i/o's mislukken mogelijk vanwege een fout 'het apparaat is niet gereed'. |U moet handmatig opnieuw opstarten Apparaatcontrollers of uitvoeren van een failover van het apparaat deze situatie te herstellen. |Ja |Nee |
| 9 |Azure PowerShell |Bij het gebruik van de StorSimple-cmdlet **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - eerst 1 - Wait** het eerste object selecteren, zodat u kunt een nieuwe maken **Upopslagaccount** object, de cmdlet retourneert alle objecten. |De cmdlet als volgt tussen haakjes verpakken: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object -First 1 -Wait** |Ja |Ja |
| 10 |Migratie |Wanneer meerdere volumecontainers voor migratie worden doorgegeven, is de verwachte Aankomsttijd voor de meest recente back-up nauwkeurige alleen voor de eerste volumecontainer. Bovendien begint parallelle migratie nadat de eerste 4 back-ups in de eerste volumecontainer worden gemigreerd. |Het is raadzaam dat u een volumecontainer tegelijk migreren. |Ja |Nee |
| 11 |Migratie |Na het herstel, zijn de volumes niet toegevoegd aan het back-upbeleid of de groep van de virtuele schijf. |U moet deze volumes toevoegen aan een back-upbeleid om te kunnen maken van back-ups. |Ja |Ja |
| 12 |Migratie |Nadat de migratie voltooid is, moet de 5000/7000-serie-apparaat geen toegang tot de gemigreerde gegevenscontainers. |Het is raadzaam dat u de gemigreerde gegevenscontainers verwijderen nadat de migratie voltooid en wordt doorgevoerd is. |Ja |Nee |
| 13 |Kloon en DR |Een StorSimple-apparaat met Update 1 kan klonen of uitvoeren van herstel na noodgevallen naar een apparaat met 1 software vóór het bijwerken. |U moet het doelapparaat bijwerken op 1 om toe te staan van deze bewerkingen bijwerken |Ja |Ja |
| 14 |Migratie |Back-up van configuratie voor de migratie mislukken op een apparaat uit de 5000-7000 serie wanneer er volumegroepen met geen gekoppelde volumes. |Verwijderen van alle groepen van een leeg volume met geen gekoppelde volumes en voer vervolgens de configuratie van back-up opnieuw uit. |Ja |Nee |
| 15 |Azure PowerShell-cmdlets en lokaal vastgemaakte volumes |U kunt een lokaal vastgemaakt volume via Azure PowerShell-cmdlets kan niet maken. (Alle volumes die u via Azure PowerShell maakt worden gelaagd.) |Gebruik altijd de StorSimple Manager-service voor het configureren van lokaal vastgemaakte volumes. |Ja |Nee |
| 16 |Beschikbare schijfruimte voor het lokaal vastgemaakte volumes |Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor nieuwe volumes mogelijk niet meteen bijgewerkt. De StorSimple Manager-service-updates de lokale ruimte die beschikbaar ongeveer elk uur. |Wacht een uur voordat u probeert te maken van het nieuwe volume. |Ja |Nee |
| 17 |Lokaal vastgemaakte volumes |De hersteltaak wordt aangegeven dat de tijdelijke momentopnameback-up in de back-catalogus, maar alleen voor de duur van de hersteltaak. Bovendien het wordt aangegeven dat een groep virtuele schijf met het voorvoegsel **tmpCollection** op de **back-upbeleid** pagina, maar alleen voor de duur van de hersteltaak. |Dit probleem kan optreden als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. Als de hersteltaak alleen gelaagde volumes bevat, wordt klikt u vervolgens dit gedrag niet uitgevoerd. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 18 |Lokaal vastgemaakte volumes |Als u een hersteltaak annuleren en een failover van de controller gebeurt onmiddellijk daarna wordt de hersteltaak wordt weergegeven **mislukt** in plaats van **geannuleerd**. Als een hersteltaak mislukt en er treedt een failover van de controller op direct daarna de hersteltaak wordt weergegeven **geannuleerd** in plaats van **mislukt**. |Dit probleem kan optreden als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. Als de hersteltaak alleen gelaagde volumes bevat, wordt klikt u vervolgens dit gedrag niet uitgevoerd. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 19 |Lokaal vastgemaakte volumes |Als u een hersteltaak annuleert of als een herstelpunt is mislukt en treedt er een failover van de controller op, een extra hersteltaak wordt weergegeven op de **taken** pagina. |Dit probleem kan optreden als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. Als de hersteltaak alleen gelaagde volumes bevat, wordt klikt u vervolgens dit gedrag niet uitgevoerd. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 20 |Lokaal vastgemaakte volumes |Als u probeert te converteren van een gelaagd volume (gemaakt en gekloonde met Update 1.2 of eerder) naar een lokaal vastgemaakt volume en uw apparaat bijna vol is of er een storing in de cloud is, kunnen de clone(s) worden beschadigd. |Dit probleem treedt alleen op volumes die gemaakt en gekloonde met vooraf Update 2.1-software zijn. Dit moet een onregelmatige scenario. | | |
| 21 |Volumeconversie |De ACR's die zijn gekoppeld aan een volume, terwijl de volumeconversie van een uitgevoerd wordt, niet bijwerken (gelaagde in lokaal vastgemaakt of omgekeerd). Bijwerken van de ACR's kan leiden tot beschadigde gegevens. |Indien nodig, bijwerken van de ACR's voordat u de volumeconversie en maak geen verdere ACR updates tijdens de conversie uitgevoerd wordt. | | |

## <a name="controller-and-firmware-updates-in-update-22"></a>Controller en firmware-updates in Update 2.2
Deze release bevat alleen uit software-updates. Echter, als u een uitvoert vanaf een eerdere versie dan Update 2 update, moet u voor het installeren van stuurprogramma, Storport, Spaceport, en (in sommige gevallen) schijf firmware-updates op uw apparaat.

Zie voor meer informatie over het installeren van het stuurprogramma, Storport, Spaceport en updates van de schijffirmware [Update 2.2 installeren](storsimple-install-update-21.md) op uw StorSimple-apparaat.

## <a name="virtual-device-updates-in-update-22"></a>Updates van de virtuele apparaat in Update 2.2
Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap
Meer informatie over het [Update 2.2 installeren](storsimple-install-update-21.md) op uw StorSimple-apparaat.

