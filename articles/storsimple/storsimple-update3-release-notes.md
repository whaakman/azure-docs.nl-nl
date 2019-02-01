---
title: Opmerkingen bij de release van de StorSimple 8000 met Update 3 | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 met Update 3.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d18feba4ded3dfccb8f774112a7dc8d42b12f1d5
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55488319"
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>3 opmerkingen bij de release voor uw StorSimple 8000-apparaat bijwerken

## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen beschrijven de nieuwe functies en de kritieke open problemen voor de StorSimple 8000 met Update 3 hebt geïdentificeerd. Ze bevatten ook een lijst van de StorSimple-software-updates in deze release is opgenomen. 

Update 3 kan worden toegepast op alle StorSimple-apparaat met Release (GA) of Update 0.1 via Update 2.2. De versie van het apparaat dat is gekoppeld met Update 3 is 6.3.9600.17759.

Lees de informatie in de opmerkingen bij de release voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Update 3 is apparaatsoftware, LSI-stuurprogramma en firmware en Storport en Spaceport werkt. Het duurt ongeveer 1,5 tot 2 uur om deze update te installeren. 
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk dat er een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze snel weer beschikbaar is.
> 
> 

## <a name="whats-new-in-update-3"></a>Wat is er nieuw in Update 3
De volgende belangrijke verbeteringen en oplossingen voor problemen zijn aangebracht in Update 3.

* **Ruimte vrijmaken wijzigingen automatisch** – vanaf Update 3, de algoritmen voor het vrijmaken van ruimte uitvoeren op de stand-by-controller van het systeem leidt tot sneller worden uitgevoerd. Raadpleeg voor meer informatie over de poorten die nodig zijn voor het werken met het vrijmaken van ruimte, de [StorSimple netwerkvereisten](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).
* **Prestatieverbeteringen** – Update 3 bevat verbeterde prestaties van de lezen / schrijven naar de cloud.
* **Verbeteringen met betrekking tot migratie** : In deze release verschillende foutoplossingen en verbeteringen zijn uitgevoerd voor de functie voor migratie van apparaten uit de 5000/7000-serie 8000-serie-apparaten. Voor meer informatie over het gebruik van de migratiefunctie voor, gaat u naar [migratie van het apparaat uit de 5000/7000-serie 8000-serie apparaat](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b). 
* **Bewaking van gerelateerde oplossingen** : In deze release, fouten met betrekking tot controlegrafieken, servicedashboard en device-dashboard zijn opgelost.

## <a name="issues-fixed-in-update-3"></a>Problemen opgelost met Update 3
De volgende tabel bevat een samenvatting van problemen die zijn verholpen in Update 3.    

| Nee | Functie | Probleem | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Host-zijde gegevensmigratie |In de eerdere versie, is de StorSimple-Cloudapparaat gaat offline tijdens de migratie van een host-side gegevens. Dit probleem is opgelost in deze release. |Nee |Ja |
| 2 |Lokaal vastgemaakte volumes |In de vorige release zijn er problemen met betrekking tot i/o-fouten, volume conversie fouten en storingen gegevenspad voor lokaal vastgemaakte volumes. Deze problemen zijn hoofdmap veroorzaakt en opgelost in deze release. |Ja |Nee |
| 3 |Bewaking |Er zijn meerdere problemen met betrekking tot eenheden rapportage- en bewakingsdoeleinden evenals apparaat dashboardgrafieken waar onjuiste informatie wordt weergegeven voor lokaal vastgemaakte volumes. Deze problemen zijn opgelost in deze release. |Ja |Nee |
| 4 |I/o zware schrijfbewerkingen |Wanneer u StorSimple voor workloads met betrekking tot zware schrijfbewerkingen, de gebruiker zou uitvoeren in een onregelmatige bug waar de werkset is wordt doorverbonden naar de cloud. Deze fout is opgelost in deze release. |Ja |Ja |
| 5 |Backup |In sommige zeldzame gevallen, in de vorige versies van software, wanneer de gebruiker heeft een back-up van een externe kloon, ze zou worden uitgevoerd in de cloud-fouten en de bewerking wordt een fout. In deze release, het probleem is opgelost en de bewerking is voltooid. |Ja |Ja |
| 6 |Back-upbeleid |In sommige zeldzame gevallen, in de eerdere versies van software, er is een fout met betrekking tot het verwijderen van back-upbeleid. Dit probleem is opgelost in deze release. |Ja |Ja |

## <a name="known-issues-in-update-3"></a>Bekende problemen in Update 3
De volgende tabel bevat een overzicht van de bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen / tijdelijke oplossing | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat zijn verbroken, wat resulteert in geen quorum schijf gaat vervolgens de opslaggroep offline. Dit wordt offline blijven, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, kunt u contact op met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Onjuiste besturings-ID |Wanneer een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de installatiekopie wordt geladen vanuit het peer-knooppunt kan de besturings-ID weergegeven in eerste instantie als de peer-controller-ID. In zeldzame gevallen kan dit gedrag ook worden weergegeven na het systeem opnieuw is opgestart. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de netwerkcontroller te vervangen voltooid is. |Ja |Nee |
| 3 |Opslagaccounts |Met de Storage-service voor het verwijderen van het opslagaccount is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 4 |Failover van het apparaat |Meerdere failovers van een volumecontainer van het bronapparaat dezelfde aan verschillende doelapparaten wordt niet ondersteund. Failover van één dead apparaat naar meerdere apparaten wordt de volumecontainers van het eerste apparaat een failover geen eigenaar van gegevens maken. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u ze in de klassieke Azure portal weergeven. | |Ja |Nee |
| 5 |Installatie |Tijdens de StorSimple-Adapter voor SharePoint-installatie moet u voor een IP-adres van het apparaat in de volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 6 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de apparaat-naar-servicecommunicatie worden beïnvloed en het apparaat gaat offline. Voor ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de URL van proxy HTTP als het opgegeven protocol heeft. Zie [Configure web proxy for your device](storsimple-8000-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie. |Ja |Nee |
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
| 22 |Updates |Bij het toepassen van Update 3, de **onderhoud** pagina in de klassieke Azure portal wordt weergegeven het volgende bericht weergegeven met betrekking tot Update 2 - 'StorSimple 8000 series Update 2 omvat de mogelijkheid voor Microsoft proactief verzamelen logboek gegevens van uw apparaat wordt geactiveerd wanneer er potentiële problemen detecteren'. Dit is misleidend, betekent dit dat het apparaat met Update 2 wordt bijgewerkt. Wanneer het apparaat met Update 3 is bijgewerkt, verdwijnt dit bericht. |Dit probleem wordt opgelost in een toekomstige release. |Ja |Nee |

## <a name="controller-and-firmware-updates-in-update-3"></a>Controller en firmware-updates in Update 3
Deze release bevat LSI-stuurprogramma en firmware-updates. Zie voor meer informatie over het installeren van de LSI stuurprogramma's en firmware-updates [Update 3 installeren](storsimple-install-update-3.md) op uw StorSimple-apparaat.

## <a name="virtual-device-updates-in-update-3"></a>Updates van de virtuele apparaat in Update 3
Deze update kan niet worden toegepast op het StorSimple-Cloudapparaat (ook wel bekend als het virtuele apparaat). Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap
Meer informatie over het [Update 3 installeren](storsimple-install-update-3.md) op uw StorSimple-apparaat.

