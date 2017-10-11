---
title: StorSimple 8000 Update 0,3 release-opmerkingen | Microsoft Docs
description: Beschrijft de nieuwe functies en oplossingen, open problemen en oplossingen beschikbaar voor de februari 2015 Microsoft Azure StorSimple versie (Update 0.3).
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: b01bfd04-f9f8-45f4-ade8-95ac2b979e6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: c059fd74854813615754e67547497b7ededbe4dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 serie Update 0,3 release-opmerkingen - februari 2015
## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen problemen met de kritieke open voor StorSimple 8000 serie Update 0.3 uitgebracht in februari 2015. Ze bevatten ook een lijst van de StorSimple-software en -firmware-updates opgenomen in deze release. Dit is de derde release nadat de StorSimple 8000 Series versie algemeen beschikbaar is in juli 2014 is gedaan.

De versie van de apparaat-software van de update van januari wordt niet door deze update worden gewijzigd. Deze blijft versie 6.3.9600.17312. U kunt bevestigen dat de update is geïnstalleerd door het controleren van de **laatst bijgewerkt** datum. Als de datum is 2-10-2015 of hoger, en vervolgens de update is geïnstalleerd.  

U wordt aangeraden dat u zoeken en toepassen van beschikbare updates onmiddellijk nadat u uw StorSimple-apparaat geïnstalleerd. U kunt ook inschakelen Automatische updates downloaden en installeren van updates met hoge prioriteit van Microsoft zodra ze worden vrijgegeven. Zie voor meer informatie [uw StorSimple-apparaat bijwerken](storsimple-update-device.md).  

Controleer de informatie in de release-opmerkingen voordat u de update in uw StorSimple-oplossing implementeert.  

> [!IMPORTANT]
> * De StorSimple Manager-service en niet Windows PowerShell voor StorSimple gebruiken voor het installeren van de update van februari.   
> * Het duurt ongeveer een uur deze update te installeren. Echter, als u de cumulatieve updates installeert, kunt het proces ongeveer 3 uur duren om te voltooien.  
> * De release februari van StorSimple bevat geen updates op het virtuele StorSimple-apparaat. U kunt nog steeds beschikbare Windows-updates toepassen op het virtuele apparaat, met inbegrip van recente beveiligingsproblemen, maar niet ziet u een wijziging in de versie voor het virtuele apparaat.  
> 
> 

Zorg ervoor dat de volgende vereisten wordt voldaan voordat het bijwerken van uw StorSimple-apparaat.  

* Zorg ervoor dat beide apparaatcontrollers worden uitgevoerd voordat u op updates scannen. Als de domeincontroller niet wordt uitgevoerd, mislukt de scan. Om te bevestigen dat de controllers in orde zijn, gaat u naar **hardwarestatus** onder de **onderhoud** pagina. Als er onderdelen die **aandacht**, neem contact op met Microsoft Support voordat u verdergaat.
* Controleer of de vaste IP-adressen voor controller 0 en controller 1 routeerbaar zijn en verbinding kan maken met het Internet als deze worden gebruikt voor het onderhoud van de updates op het apparaat. U kunt de [cmdlet Test-Connection](https://technet.microsoft.com/library/hh849808.aspx) te pingen van een bekende adres buiten het netwerk, zoals outlook.com, om te controleren dat de domeincontroller verbonden met het externe netwerk is.
* Zorg ervoor dat de poorten 80 en 443 op uw StorSimple-apparaat voor uitgaande communicatie beschikbaar zijn. Zie voor meer informatie de [netwerkvereisten voor uw StorSimple-apparaat](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* Als de softwareversie apparaat ouder is dan 6.3.9600.17312 (update van oktober 2014), de poorten Data 2 en Data 3 uitschakelen als ingeschakeld voordat u de update start. Als u de Data 2 of 3 gegevens poorten ingeschakeld wanneer u de update toepassen, kan dit ertoe leiden dat uw apparaat domeincontroller om te gaan in de herstelmodus overgaat. Houd er rekening mee dat wanneer u de netwerkinterfaces uitschakelen, de gekoppelde volumes zal offline worden gehaald en de i/o's voor de duur van de update worden verstoord.  

## <a name="whats-new-in-the-february-release"></a>Wat is er nieuw in versie februari
Deze update bevat een oplossing voor het probleem dat is opgetreden op de fabrieksinstellingen apparaten die was zijn bijgewerkt uit de GA vrijgeven aan de release van oktober 2014. Zie voor meer informatie [problemen opgelost in deze release](#issues-fixed-in-the-february-release).   

Deze update bevat geen nieuwe functies of functionaliteit.  

## <a name="issues-fixed-in-the-february-release"></a>Problemen in de release februari opgelost
De volgende tabel worden beschreven het probleem dat met deze update is verholpen.  

| Nee. | Functie | Probleem | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Fabrieksinstellingen terugzetten |U probeert om de fabrieksinstellingen terugzetten op een apparaat die oorspronkelijk was de GA-release (versie 6.3.9600.17215) geïnstalleerd, maar is bijgewerkt naar de oktober release (versie 6.3.9600.17312). De fabrieksinstellingen van mislukt en het apparaat wordt instabiel. |Ja |Nee |

## <a name="known-issues-in-the-february-release"></a>Bekende problemen in de release februari
De volgende tabel bevat een samenvatting van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabrieksinstellingen terugzetten |In sommige gevallen, wanneer u de fabrieksinstellingen terug te zetten, uitvoert het StorSimple-apparaat kan blijven steken en dit bericht weergegeven: **terugzetten op fabrieksinstellingen wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt, terwijl de cmdlet uitgevoerd wordt. |Druk CTRL + C niet op na het initiëren van de fabrieksinstellingen terug te zetten. Als u al in deze staat, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600device zijn verbroken waardoor geen quorum schijf is vervolgens de opslaggroep offline. Het veld offline blijft, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 3 |Cloud momentopname fouten |In zeldzame gevallen wordt een cloudmomentopname van een kan mislukken met de fout **back-up maximumlimiet bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat uit hetzelfde oorspronkelijke volume dat is verwijderd. | |Ja |Ja |
| 4 |Onjuiste besturings-ID |Als een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de afbeelding is geladen vanuit het knooppunt peer de besturings-ID kan worden weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan worden dit probleem ook gezien na opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de vervangende domeincontroller voltooid is. |Ja |Nee |
| 5 |Controle van grafieken van apparaten |In de StorSimple Manager-service de bewaking grafieken apparaat werken niet als basis of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. |Wijzig de configuratie van de web-proxy voor het apparaat geregistreerd bij uw StorSimple Manager-service zodanig dat de verificatie is ingesteld op NONE. U kunt dit doen het de Windows PowerShell voor StorSimple Set-HcsWebProxy cmdlet. |Ja |Ja |
| 6 |Opslagaccounts |Het opslagaccount verwijderen met behulp van de Storage-service is een niet-ondersteund scenario. Dit leidt tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 7 |Failover van apparaat |Meerdere failovers van een volumecontainer van het bronapparaat met dezelfde aan verschillende doelapparaten wordt niet ondersteund.    Failover van één apparaat inactief naar meerdere apparaten, brengt de volumecontainers op de eerste failover apparaat is Gegevenseigendom gaan verloren. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u deze in de klassieke Azure portal bekijken. | |Ja |Nee |
| 8 |Installeren |Tijdens het StorSimple-Adapter voor SharePoint-installatie moet u voor ondersteuning van een apparaat IP in volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 9 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de proxy-URL HTTP als protocol opgegeven heeft. Meer informatie over het [webproxy voor uw apparaat configureren](storsimple-configure-web-proxy.md). |Ja |Nee |
| 10 |Webproxy |Als u configureert en webproxy op een geregistreerd apparaat inschakelen, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 11 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en de hoge i/o-belasting tegenkomt, gaat u de volumes van het apparaat in een gedegradeerde status en de i/o's mislukken met een 'apparaat niet gereed'-fout. |U moet handmatig opnieuw opstarten van de Apparaatcontrollers of voer een failover apparaat deze situatie te herstellen. |Ja |Nee |

## <a name="physical-device-updates-in-the-february-release"></a>Fysiek apparaatupdates in de februari release
Deze update corrigeert de factory opnieuw instellen probleem die is opgetreden op apparaten die was zijn bijgewerkt uit de GA vrijgeven aan de release van oktober 2014. Het bevat geen andere updates op het StorSimple-apparaat.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Serial attached SCSI (SAS)-controller en firmware-updates in de februari release
Deze release bevat geen updates op de controller serial attached SCSI (SAS) of de firmware. De stuurprogramma-update is in de oktober 2014-release.  

## <a name="virtual-device-updates-in-the-february-release"></a>Virtueel apparaatupdates in de februari release
Deze release bevat geen updates voor het virtuele apparaat. Deze update zijn toegepast, wordt de versie van de software van een virtueel apparaat niet gewijzigd.

