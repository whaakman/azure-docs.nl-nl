---
title: StorSimple 8000 Update 0,1 release-opmerkingen | Microsoft Docs
description: Beschrijft de nieuwe functies en oplossingen, open problemen en oplossingen beschikbaar voor de oktober 2014 Microsoft Azure StorSimple versie (Update 0,1).
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: fd35e3c3-4770-460c-999d-f72ab7053a20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 4dfd3973593a94adfc15a6e15d69c697e13998af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 serie Update 0,1 release-opmerkingen – oktober 2014
## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen problemen met de kritieke open voor StorSimple 8000 serie Update 0.1 in oktober 2014 is uitgebracht. Ze bevatten ook een lijst van de StorSimple-software en -firmware-updates opgenomen in deze release. Dit is de eerste release nadat de StorSimple 8000 Series versie algemeen beschikbaar is in juli 2014 is gedaan en komt met softwareversie 6.3.9600.17312 overeen.  

U wordt aangeraden dat u zoeken en toepassen van beschikbare updates onmiddellijk nadat u het apparaat geïnstalleerd. U kunt ook inschakelen Automatische updates downloaden en installeren van updates met hoge prioriteit van Microsoft zodra ze worden vrijgegeven. Zie voor meer informatie hoe [uw StorSimple-apparaat bijwerken](storsimple-update-device.md).  

Controleer de informatie in de release-opmerkingen voordat u de updates in uw StorSimple-oplossing implementeert.  

> [!IMPORTANT]
> * Gebruik de StorSimple Manager-service en niet Windows PowerShell voor StorSimple de oktober-updates te installeren.  
> * De updates worden meestal ongeveer 3 uur duren om te voltooien.  
> * De release van oktober van StorSimple bevat geen updates op het virtuele StorSimple-apparaat. U kunt nog steeds alle beschikbare Windows-updates, inclusief recente beveiligingsupdates, toepassen, maar niet ziet u een wijziging in de versie voor het virtuele apparaat.  
> 
> 

Zorg ervoor dat de volgende vereisten wordt voldaan voordat het bijwerken van uw StorSimple-apparaat.  

* Zorg ervoor dat beide apparaatcontrollers worden uitgevoerd voordat u op updates scannen. Als de domeincontroller niet wordt uitgevoerd, mislukt de scan. Om te bevestigen dat de controllers in orde zijn, gaat u naar **hardwarestatus** onder de **onderhoud** pagina. Als er onderdelen die **aandacht**, neem contact op met Microsoft Support voordat u verdergaat.  
* Zorg ervoor dat vaste IP-adressen voor beide Controller 0 en Controller 1 routeerbaar zijn en verbinding met het Internet maken kan als deze worden gebruikt voor het onderhoud van de updates op het apparaat. U kunt de [cmdlet Test-Connection](https://technet.microsoft.com/library/hh849808.aspx) te pingen van een bekende adres buiten het netwerk zoals outlook.com, om te controleren dat de domeincontroller verbonden met het externe netwerk is.  
* Zorg ervoor dat de vereiste uitgaande poorten beschikbaar op uw StorSimple-apparaat voor uitgaande communicatie zijn. Zie voor meer informatie de [netwerkvereisten voor uw StorSimple-apparaat](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
* Als de softwareversie apparaat ouder is dan 6.3.9600.17312 (update van oktober 2014), de poorten Data 2 en Data 3 uitschakelen als ingeschakeld voordat u de update start. Als u de Data 2 of 3 gegevens poorten ingeschakeld laat wanneer de update wordt toegepast, kan dit ertoe leiden dat uw apparaat domeincontroller om te gaan in de herstelmodus overgaat. Houd er rekening mee dat wanneer u de netwerkinterfaces uitschakelen, de gekoppelde volumes zal offline worden gehaald en de i/o's voor de duur van de update worden verstoord.  

## <a name="whats-new-in-the-october-release"></a>Wat is er nieuw in de release van oktober
Deze update bevat de volgende verbeteringen:

* U kunt nu de gebruikersinterface van de StorSimple Manager-service gebruiken voor het beheren van uw apparaat-domeincontrollers. De acties omvatten management opstarten, afsluiten, of schakel de op een domeincontroller. Ga voor meer informatie naar [StorSimple beheren apparaatcontrollers](storsimple-manage-device-controller.md).  
* U kunt de toewijzing van de WAN-bandbreedte volgens combinaties van dag van de week en tijd van de dag plannen. Hiermee kunt u beter gebruikmaken van WAN-bandbreedte tijdens de daluren. Andere bandbreedte sjablonen zijn toegestaan voor een ander volumecontainers. Ga voor meer informatie naar [beheren van uw StorSimple bandbreedte sjablonen](storsimple-manage-bandwidth-templates.md).  
* U kunt e-mailmeldingen proactief waarschuwen de (s) en andere bestaande of mogelijk toekomstige problemen kunt configureren. Ga voor meer informatie naar [configureren waarschuwingsinstellingen](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Problemen in de release van oktober opgelost
De volgende tabel bevat een samenvatting van fouten die in deze update worden verholpen.  

| Nee. | Functie | Probleem | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Netwerkinterfaces |In de vorige release zijn de netwerkinterfaces DATA 2 en DATA 3 in de software omgewisseld. Dit probleem is opgelost in deze update. Wis de instellingen en deze netwerkinterfaces uitschakelen voordat u de update installeert. U moet deze interfaces configureren na de installatie van de update. |Ja |Nee |
| 2 |Ondersteuningspakket |In de vorige release, als u de Windows PowerShell uitgevoerd **Export HcsSupportPackage** cmdlet voor het ophalen van de Baseboard Management Controller (BMC) zich aanmeldt, wordt de bewerking is mislukt met de volgende waarschuwing: 'de bewerking is gelukt Deze domeincontroller, maar op de controller peer vanwege de volgende fout(en) is mislukt. Controleer of de peer in orde is en of het huidige knooppunt kan verbinding maken met de peer." Dit probleem is nu opgelost. |Ja |Nee |
| 3 |Failover van apparaat |In de vorige release, er is een kans op inconsistenties in de gegevens als een **detecteren back-up** taak is mislukt tijdens een failover van het apparaat. Dit probleem is nu opgelost. |Ja |Nee |
| 4 |Failover van apparaat |Back-ups zijn zichtbaar in de vorige release, na een failover apparaat, maar de bijbehorende volumecontainer is niet aanwezig is op het doelapparaat. Dit probleem is nu opgelost. |Ja |Nee |
| 5 |Failover van apparaat |In de vorige release moet u er een fout is in de opsomming van back-ups cloud tijdens de Register-herstelbewerking op die tot inconsistenties in de gegevens leiden kan als er problemen met de netwerkverbinding van de cloud. |Ja |Nee |
| 6 |Firmware-update |In de vorige release is de apparaat-firmware-update-taak is mislukt, en een fout die vermeld zijn de cmdlets niet herkend en dat de update mislukt als gevolg hiervan wordt weergegeven. De controller wordt vervolgens werd in de herstelmodus overgaat. Dit probleem is nu opgelost. |Ja |Nee |
| 7 |Installeren |Fouten worden veroorzaakt door het apparaat niet wordt replicatie correct tijdens de installatie zijn vastgesteld. |Ja |Nee |
| 8 |Fabrieksinstellingen terugzetten |U kunt nu de controle van de firmware voor fabrieksinstellingen eventueel overslaan. Dit is een wijziging van de vorige release. |Ja |Nee |
| 9 |Fabrieksinstellingen terugzetten |In de vorige release tijdens het uitvoeren van een fabriek reset-cmdlet, zijn firmware versie controles alleen gemaakt voor bepaalde hardwareonderdelen. Aanvullende firmware controles zijn aangebracht nadat de eerste herstart in het proces, wat leiden opnieuw instellen van het tot kan mislukken. Deze oplossing zorgt ervoor dat alle de firmware-controles uitgevoerd wanneer de fabrieksinstellingen van cmdlet wordt uitgevoerd voordat de eerste systeem opnieuw opstarten. |Ja |Nee |
| 10 |Storage-account key draaien |De **Invoke-HcsmServiceDataEncryptionKeyChange** cmdlet gebruikt voor de opslagaccountsleutels nu draaien vraagt de gebruiker gevraagd de gegevensversleutelingssleutel van service. Dit is een wijziging van de vorige release waarin de gegevensversleutelingssleutel van service is doorgegeven als een inline-parameter. |Ja |Nee |
| 11 |Failback binnen 24 uur |Tijdens het herstel na noodgevallen, het opruimen van de op het bronapparaat niet het geval duidelijke, waardoor failback mislukken. Dit probleem is opgelost in deze release. |Ja |Nee |

## <a name="known-issues-in-the-october-release"></a>Bekende problemen in de release van oktober
De volgende tabel bevat een samenvatting van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabrieksinstellingen terugzetten |In sommige gevallen, wanneer u de fabrieksinstellingen terug te zetten, uitvoert het StorSimple-apparaat kan blijven steken en dit bericht weergegeven: **terugzetten op fabrieksinstellingen wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt, terwijl de cmdlet uitgevoerd wordt. |Druk CTRL + C niet op na het initiëren van de fabrieksinstellingen terug te zetten. Als u al in deze staat, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Fabrieksinstellingen terugzetten |Voer niet terugzetten op fabrieksinstellingen een StorSimple-apparaat is bijgewerkt van GA naar oktober 2014 release. |Deze bewerking werkt alleen als een patch is geïnstalleerd. Neem contact op met Microsoft Support op te halen dit vereist patch. |Ja |Nee |
| 3 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat niet zijn verbonden met waardoor geen quorum schijf vervolgens de opslaggroep worden offline. Het veld offline blijft, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 4 |Cloud momentopname fouten |In zeldzame gevallen wordt een cloudmomentopname van een kan mislukken met de fout **back-up maximumlimiet bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat uit hetzelfde oorspronkelijke volume dat is verwijderd. | |Ja |Ja |
| 5 |Onjuiste besturings-ID |Als een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de afbeelding is geladen vanuit het knooppunt peer de besturings-ID kan worden weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan worden dit probleem ook gezien na opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de vervangende domeincontroller voltooid is. |Ja |Nee |
| 6 |Controle van grafieken van apparaten |In de StorSimple Manager-service de bewaking grafieken apparaat werken niet als basis of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. |Wijzig de configuratie van de web-proxy voor het apparaat geregistreerd bij uw StorSimple Manager-service zodanig dat de verificatie is ingesteld op NONE. U kunt dit doen het de Windows PowerShell voor StorSimple Set-HcsWebProxy cmdlet. |Ja |Ja |
| 7 |Opslagaccounts |Het opslagaccount verwijderen met behulp van de Storage-service is een niet-ondersteund scenario. Dit leidt tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 8 |Failover van apparaat |Meerdere failovers van een volumecontainer van het bronapparaat met dezelfde aan verschillende doelapparaten wordt niet ondersteund. |Failover van één apparaat inactief naar meerdere apparaten, brengt de volumecontainers op de eerste failover apparaat is Gegevenseigendom gaan verloren. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u deze in de klassieke Azure portal bekijken. |Ja |Nee |
| 9 |Installeren |Tijdens het StorSimple-Adapter voor SharePoint-installatie moet u voor ondersteuning van een apparaat IP in volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 10 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de proxy-URL HTTP als protocol opgegeven heeft. Meer informatie over het [webproxy voor uw apparaat configureren](storsimple-configure-web-proxy.md). |Ja |Nee |
| 11 |Webproxy |Als u configureert en webproxy op een geregistreerd apparaat inschakelen, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 12 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en de hoge i/o-belasting tegenkomt, gaat u de volumes van het apparaat in een gedegradeerde status en de i/o's mislukken met een 'apparaat niet gereed'-fout. |U moet handmatig opnieuw opstarten van de Apparaatcontrollers of voer een failover apparaat deze situatie te herstellen. |Ja |Nee |

## <a name="physical-device-updates-in-the-october-release"></a>Fysiek apparaatupdates in de oktober release
Wanneer deze updates worden toegepast op een fysiek apparaat, wordt de softwareversie 6.3.9600.17312 omzetten. Tenzij anders vermeld, worden deze releaseopmerkingen zijn van toepassing op alle modellen van het StorSimple-apparaat. Zie voor meer informatie over deze updates [oktober 2014 fysieke toestel software-update voor Microsoft Azure StorSimple-apparaat](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Serial attached SCSI (SAS)-controller en firmware-updates in de oktober release
Het stuurprogramma en de firmware van de SAS-controller van uw fysieke apparaat, wordt deze versie bijgewerkt. Zie voor meer informatie over de update van de domeincontroller SAS [update van oktober 2014 voor LSI SAS-controllers in Microsoft Azure StorSimple toestel](http://support.microsoft.com/kb/2987020).   

Deze release geldt ook een cumulatieve firmware-update die zijn gericht op problemen met de hardware-onderdelen van het apparaat de betrouwbaarheid. Zie voor meer informatie over de firmware-update [oktober 2014 firmware-update voor Microsoft Azure StorSimple-apparaat](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Virtueel apparaatupdates in de oktober release
Deze release bevat geen updates voor het virtuele apparaat. Deze update zijn toegepast, wordt de versie van de software van een virtueel apparaat niet gewijzigd.

