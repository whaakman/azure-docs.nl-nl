---
title: Oplossen van problemen met de implementatie van de StorSimple 8000-serie | Microsoft Docs
description: Beschrijft hoe u kunt vaststellen en oplossen van fouten die optreden bij het implementeren van StorSimple.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 61719d482a4db1c737bbe38277f2ac3b2d684b63
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342422"
---
# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Oplossen van problemen met de implementatie van de StorSimple-apparaat
## <a name="overview"></a>Overzicht
In dit artikel biedt nuttige richtlijnen voor probleemoplossing voor uw Microsoft Azure StorSimple-implementatie. Hierin worden veelvoorkomende problemen, mogelijke oorzaken en aanbevolen stappen voor het oplossen van problemen die u tegenkomen kunt bij het configureren van StorSimple. 

Deze informatie is van toepassing op zowel het fysieke apparaat voor StorSimple 8000-serie en StorSimple Cloud Appliance.

> [!NOTE]
> Configuratie-gerelateerde problemen met het apparaat dat u zich kan voordoen kunnen optreden wanneer u het apparaat voor de eerste keer implementeren of ze later optreden kunnen wanneer het apparaat ingeschakeld is. In dit artikel gaat over het oplossen van problemen bij de implementatie van de eerste keer in. Als u wilt een operationele apparaat gaat oplossen, gaat u naar [het hulpprogramma Diagnostics gebruiken om op te lossen een operationele apparaat](storsimple-8000-diagnostics.md).

In dit artikel wordt ook beschrijft de hulpmiddelen voor het oplossen van StorSimple-implementaties en biedt een voorbeeld van stapsgewijze probleemoplossing.

## <a name="first-time-deployment-issues"></a>Problemen bij de implementatie van de eerste keer
Als u een probleem ondervindt bij het implementeren van uw apparaat voor de eerste keer, houd rekening met het volgende:

* Als u een fysiek apparaat oplossen wilt, zorg ervoor dat de hardware is geïnstalleerd en geconfigureerd zoals beschreven in [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
* Controleer de vereisten voor de implementatie. Zorg ervoor dat u alle informatie die wordt beschreven in de [Configuratiecontrolelijst voor implementatie](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist).
* Lees de opmerkingen bij de Release van StorSimple om te zien als het probleem wordt beschreven. Opmerkingen bij de release bevatten oplossingen voor bekende installatieproblemen. 

Tijdens de implementatie van het apparaat problemen de meest voorkomende die gebruikers face zich voordoen wanneer ze de wizard setup uitgevoerd en wanneer ze worden geregistreerd via Windows PowerShell voor StorSimple. (U Windows PowerShell voor StorSimple om te registreren en configureren uw StorSimple-apparaat. Zie voor meer informatie over het registreren van apparaten, [stap 3: configureren en registreren van uw apparaat via Windows PowerShell voor StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

Aan de hand van de volgende secties kunnen u oplossen van problemen die optreden bij het configureren van het StorSimple-apparaat voor de eerste keer.

## <a name="first-time-setup-wizard-process"></a>Eerst setup wizard-proces
De volgende stappen geven een overzicht van het installatieproces van de wizard. Zie voor gedetailleerde informatie, [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

1. Voer de [Invoke-hcssetupwizard uit](https://technet.microsoft.com/library/dn688135.aspx) cmdlet voor het starten van de setup-wizard u door de resterende leidt stappen. 
2. Het netwerk configureren: de wizard setup kunt u netwerkinstellingen voor de DATA 0-netwerkinterface op uw StorSimple-apparaat configureren. Deze instellingen omvatten het volgende:
   * Virtuele IP-adres (VIP), subnetmasker en gateway-de [Set HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) cmdlet wordt uitgevoerd op de achtergrond. Wordt het IP-adres, subnetmasker en gateway voor de DATA 0-netwerkinterface geconfigureerd op uw StorSimple-apparaat.
   * Primaire DNS-server: de [Set HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) cmdlet wordt uitgevoerd op de achtergrond. Het configureert de DNS-instellingen voor uw StorSimple-oplossing.
   * NTP-server: de [Set HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet wordt uitgevoerd op de achtergrond. Configureert het de NTP-serverinstellingen voor uw StorSimple-oplossing.
   * Optionele webproxy – de [Set HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) cmdlet wordt uitgevoerd op de achtergrond. Dit wordt ingesteld en wordt de configuratie van de web-proxy voor uw StorSimple-oplossing.
3. Het wachtwoord instellen: de volgende stap is het instellen van het beheerderswachtwoord voor het apparaat.
   Het beheerderswachtwoord voor het apparaat wordt gebruikt voor aanmelding bij uw apparaat. Het standaardapparaatwachtwoord is **Password1**.
        
     > [!IMPORTANT]
     > Wachtwoorden zijn verzameld vóór de inschrijving, maar pas nadat u het apparaat wordt geregistreerd toegepast. Als er een fout opgetreden bij het toepassen van een wachtwoord, wordt u gevraagd het wachtwoord opnieuw opgeven totdat de vereiste wachtwoorden (die voldoen aan de complexiteitsvereisten voldoen) worden verzameld.
     
4. Registreer het apparaat: de laatste stap is het apparaat registreren bij de StorSimple Device Manager-service die wordt uitgevoerd in Microsoft Azure. De registratie, moet u [de serviceregistratiesleutel ophalen](storsimple-8000-manage-service.md#get-the-service-registration-key) vanuit Azure portal, en deze opgeven in de wizard setup. **Nadat het apparaat is geregistreerd, wordt een versleutelingssleutel voor servicegegevens wordt geleverd aan u. Zorg ervoor dat deze versleutelingssleutel op een veilige locatie omdat deze vereist is voor alle latere apparaten registreren bij de service.**

## <a name="common-errors-during-device-deployment"></a>Veelvoorkomende fouten tijdens de implementatie van apparaat
De volgende tabellen worden de algemene fouten die wanneer optreden mogelijk u:

* Configureer de vereiste netwerkinstellingen.
* De optionele web proxy-instellingen configureren.
* Het beheerderswachtwoord instellen.
* Het apparaat te registreren.

## <a name="errors-during-the-required-network-settings"></a>Fouten tijdens de vereiste netwerkinstellingen
| Nee. | Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Met deze opdracht kan alleen worden uitgevoerd op de actieve controller. |Configuratie werd uitgevoerd op de passieve controller. |Deze opdracht uitvoert vanaf de actieve controller. Zie voor meer informatie, [actieve controller op uw apparaat identificeren](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| 2 |Invoke-HcsSetupWizard: Het apparaat is niet gereed. |Er zijn problemen met de netwerkverbinding beschikbaar is op DATA 0. |Controleer de fysieke netwerkverbinding beschikbaar is op DATA 0. |
| 3 |Invoke-HcsSetupWizard: Er is een IP-adresconflict met een ander systeem op het netwerk (uitzondering van HRESULT: 0x80070263). |Het IP-adres opgegeven voor de DATA 0 is al in gebruik door een ander systeem. |Geef een nieuwe IP-adres dat niet wordt gebruikt. |
| 4 |Invoke-HcsSetupWizard: Een cluster-bron is mislukt. (Uitzondering van HRESULT: 0x800713AE). |Dubbele VIP. Het opgegeven IP-adres wordt al gebruikt. |Geef een nieuwe IP-adres dat niet wordt gebruikt. |
| 5 |Invoke-HcsSetupWizard: Ongeldig IPv4-adres. |Het IP-adres is opgegeven in een onjuiste indeling. |Controleer de indeling en geef uw IP-adres opnieuw. Zie voor meer informatie, [Ipv4-adressen][1]. |
| 6 |Invoke-HcsSetupWizard: Ongeldig IPv6-adres. |Het IP-adres is opgegeven in een onjuiste indeling. |Controleer de indeling en geef uw IP-adres opnieuw. Zie voor meer informatie, [Ipv6-adressen][2]. |
| 7 |Invoke-HcsSetupWizard: Er zijn geen eindpunten meer beschikbaar is via de eindpunttoewijzer. (Uitzondering van HRESULT: 0x800706D9) |Functie van het cluster werkt niet. |[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor de volgende stappen. |

## <a name="errors-during-the-optional-web-proxy-settings"></a>Fouten tijdens de optionele web proxy-instellingen
| Nee. | Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Ongeldige parameter (uitzondering van HRESULT: 0x80070057) |Een van de opgegeven parameters voor de proxy-instellingen is niet geldig. |De URI is niet opgegeven in de juiste indeling. Gebruik de volgende indeling: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 |Invoke-HcsSetupWizard: RPC-server niet beschikbaar (uitzondering van HRESULT: 0x800706ba) |De hoofdoorzaak is een van de volgende opties:<ol><li>Het cluster is niet maximaal.</li><li>De passieve controller niet kan communiceren met de actieve controller en de opdracht wordt uitgevoerd vanaf de passieve controller.</li></ol> |Afhankelijk van de hoofdoorzaak:<ol><li>[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om ervoor te zorgen dat het cluster actief is.</li><li>Voer de opdracht uit vanaf de actieve controller. Als u de opdracht uitvoeren vanaf de passieve controller wilt, moet u om ervoor te zorgen dat de passieve controller met de actieve controller communiceren kan. U moet [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) als deze verbinding verbroken is.</li></ol> |
| 3 |Invoke-HcsSetupWizard: RPC-aanroep is mislukt (uitzondering van HRESULT: 0x800706be) |Cluster is niet actief. |[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om ervoor te zorgen dat het cluster actief is. |
| 4 |Invoke-HcsSetupWizard: Cluster-bron is niet gevonden (uitzondering van HRESULT: 0x8007138f) |De cluster-bron is niet gevonden. Dit kan gebeuren wanneer de installatie niet juist is. |Mogelijk moet u het apparaat opnieuw instellen op de standaardfabrieksinstellingen. [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om de clusterbron van een te maken. |
| 5 |Invoke-HcsSetupWizard: Cluster-bron niet online (uitzondering van HRESULT: 0x8007138c) |Clusterresources zijn niet online. |[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor de volgende stappen. |

## <a name="errors-related-to-device-administrator-password"></a>Fouten met betrekking tot het beheerderswachtwoord voor het apparaat
Het standaard beheerderswachtwoord is **Wachtwoord1**. Dit wachtwoord verloopt na de eerste aanmelding; Daarom moet u de wizard setup gebruiken om deze te wijzigen. Wanneer u het apparaat te voor de eerste keer registreren, moet u een nieuw beheerderswachtwoord voor het apparaat opgeven. 

Zorg ervoor dat uw wachtwoord voldoet aan de volgende vereisten:

* Uw beheerderswachtwoord voor het apparaat moet tussen 8 en 15 tekens lang zijn.
* Wachtwoorden moeten 3 van de volgende tekentypen van 4 bevatten: kleine letters, hoofdletters, numerieke en speciale. 
* Uw wachtwoord mag niet gelijk zijn aan de laatste 24 wachtwoorden.

Bovendien, houd er rekening mee dat wachtwoorden verlopen elk jaar en kunnen worden gewijzigd nadat u het apparaat wordt geregistreerd. Als de registratie om welke reden dan ook mislukt, wordt de wachtwoorden die niet worden gewijzigd.

Voor meer informatie over het beheerderswachtwoord voor het apparaat, gaat u naar [de StorSimple Device Manager-service gebruiken om uw StorSimple-wachtwoord wijzigen](storsimple-8000-change-passwords.md).

U kunt een of meer van de volgende fouten tegenkomen bij het instellen van de apparaatbeheerder en StorSimple Snapshot Manager wachtwoorden.

| Nee. | Foutbericht | Aanbevolen actie |
| --- | --- | --- |
| 1 |Het wachtwoord overschrijdt de maximumlengte. |Als dit probleem zich blijft voordoen, Neem contact op met Microsoft Support voor de volgende stappen. |
| 2 |Waarschuwing: Kan het apparaat niet activeren. |Als dit probleem zich blijft voordoen, Neem contact op met Microsoft Support voor de volgende stappen.|
| 3 |De apparaatbeheerder van uw en StorSimple Snapshot Manager wachtwoorden zijn niet gewijzigd. |Als de registratie is mislukt, worden de apparaatbeheerder en StorSimple Snapshot Manager wachtwoorden worden niet gewijzigd. Hulpprogramma's voor het oplossen van StorSimple-implementaties |
| 4 |StorSimple bevat verschillende hulpprogramma's die u gebruiken kunt om op te lossen van uw StorSimple-oplossing. |Als de registratie is mislukt, worden de apparaatbeheerder en StorSimple Snapshot Manager wachtwoorden worden niet gewijzigd. Hulpprogramma's voor het oplossen van StorSimple-implementaties |
| 5 |Ondersteuning voor pakketten en logboeken van het apparaat. |Als de registratie is mislukt, worden de apparaatbeheerder en StorSimple Snapshot Manager wachtwoorden worden niet gewijzigd. Hulpprogramma's voor het oplossen van StorSimple-implementaties |
| 6 |Cmdlets die speciaal is ontworpen voor het oplossen van problemen. |Ondersteuning voor pakketten en logboeken van het apparaat beschikbaar voor het oplossen van problemen Hulpprogramma's voor het oplossen van StorSimple-implementaties |
| 7 |Een ondersteuningspakket bevat alle relevante logboeken die het team van Microsoft Support helpen kunnen bij het oplossen van problemen met het apparaat. |U kunt Windows PowerShell voor StorSimple gebruiken voor het genereren van een versleutelde ondersteuningspakket die u vervolgens met het ondersteunend personeel delen kunt. |
| 8 |De logboeken of de inhoud van het ondersteuningspakket weergeven |Windows PowerShell voor StorSimple gebruiken voor het genereren van een ondersteuningspakket met, zoals beschreven in *maken en beheren van een ondersteuningspakket*. Download de ontsleuteling script lokaal op uw clientcomputer. |
| 9 |Gebruik deze stapsgewijze procedure om te openen en ontsleutelen van het ondersteuningspakket. De logboeken van de ontsleutelde ondersteuning-pakket hebben etw/etvx-indeling. |U kunt de volgende stappen uit om deze bestanden in Windows logboeken weer te geven kunt uitvoeren: |

Voer de eventvwr opdracht op uw Windows-client. Hiermee wordt logboeken gestart.

> [!IMPORTANT]
> In de acties deelvenster, klikt u op opgeslagen logboek openen en wijst u de logboekbestanden in etvx/etw-indeling (het ondersteuningspakket). U kunt nu het bestand weergeven. Nadat u het bestand opent, kunt u met de rechtermuisknop op en sla het bestand als tekst. U kunt ook de Get-WinEvent cmdlet voor het openen van deze bestanden in Windows PowerShell.


Zie voor meer informatie, Get-WinEvent in de documentatie van Windows PowerShell-cmdlet-verwijzing. Wanneer de logboeken in Logboeken openen, kunt u zoeken naar de volgende logboeken die problemen met betrekking tot de apparaatconfiguratie bevatten:

## <a name="errors-during-device-registration"></a>hcs_pfconfig/operationeel logboek
hcs_pfconfig/Config In de logboekbestanden gezocht naar tekenreeksen met betrekking tot de cmdlets aangeroepen door de wizard setup.

| Nee. | Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| --- | --- | --- | --- |
| 1 |Zie eerst setup-wizard proces voor een lijst van deze cmdlets. | |Als u nog niet om de oorzaak van het probleem te achterhalen, kunt u Neem contact op met Microsoft Support voor de volgende stappen. Gebruik de stappen in [Maak een ondersteuningsaanvraag](storsimple-8000-contact-microsoft-support.md) wanneer u contact op met Microsoft Support voor hulp. |
| 2 |Cmdlets die beschikbaar zijn voor het oplossen van problemen Gebruik de volgende Windows PowerShell-cmdlets voor het detecteren van fouten in de basisnetwerkverbinding. | |Registreer het apparaat opnieuw met de juiste serviceregistratiesleutel. : Met deze cmdlet gebruiken voor het detecteren van de status van netwerkinterfaces.](storsimple-8000-manage-service.md#get-the-service-registration-key) |
| 3 |: Deze cmdlet gebruiken om te controleren of de netwerkverbinding binnen en buiten het netwerk. : Deze cmdlet gebruiken om te controleren of de connectiviteit van een apparaat is geregistreerd. |: Gebruik deze cmdlet voor het weergeven van de tijd op het apparaat en tijdsynchronisatie met de NTP-server dwingen. en : deze cmdlets gebruiken om de hosts te pingen de netwerkinterfaces op uw StorSimple-apparaat toestaan. |Standaard reageren de StorSimple-netwerkinterfaces niet op aanvragen van de ping. |
| 4 |: Deze cmdlet gebruiken als een hulpmiddel voor het traceren van route. |Deze pakketten verzendt naar elke router op de manier om een uiteindelijke bestemming gedurende een bepaalde periode en vervolgens worden berekend op basis van de pakketten die zijn geretourneerd door elke hop resultaten. Aangezien  ziet u de mate van pakketverlies op elke router en koppeling, u kunt achterhalen welke routers of koppelingen mogelijk veroorzaakt door problemen met het netwerk. |: Gebruik deze cmdlet om weer te geven van de lokale IP-routeringstabel. Problemen met de cmdlet Get-NetAdapter oplossen](storsimple-8000-contact-microsoft-support.md) |
| 5 |Bij het configureren van netwerkinterfaces voor de implementatie van een apparaat van de eerste keer, is de status van uw hardware niet beschikbaar in de gebruikersinterface van de StorSimple Device Manager-service omdat het apparaat nog niet is geregistreerd bij de service. | |Bovendien de hardwarestatus blade mogelijk niet altijd goed overeen met de status van het apparaat, met name als er problemen die invloed hebben op synchronisatie van de service zijn. |
| 6 |In deze situaties kunt u de  cmdlet om te bepalen van de status en de status van de netwerkinterfaces. | |Voor een overzicht van alle netwerkadapters op uw apparaat |
| 7 |Invoke-HcsSetupWizard: Is een fout opgetreden tijdens het registreren van uw apparaat. Dit kan zijn vanwege onjuiste IP-adres of de DNS-naam. Controleer uw netwerkinstellingen en probeer het opnieuw. Als het probleem zich blijft voordoen, [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md). (Fout 350050) |Zorg ervoor dat uw apparaat naar het externe netwerk kan pingen. Als u geen verbinding met extern netwerk, wordt de registratie kan mislukken vanwege de volgende fout. Deze fout kan bestaan uit een combinatie van een of meer van de volgende opties:<ul><li>Onjuiste IP</li><li>Onjuiste subnet</li><li>Ongeldige gateway</li><li>Onjuiste DNS-instellingen</li></ul> |Zie de stappen in de [voorbeeld van stapsgewijze probleemoplossing](#step-by-step-storsimple-troubleshooting-example). |
| 8 |Invoke-hcssetupwizard uit: De huidige bewerking is mislukt vanwege een interne servicefout [0x1FBE2]. Probeer het later opnieuw. Als het probleem zich blijft voordoen, neem contact op met Microsoft Support. |Dit is een algemene fout opgetreden voor alle onzichtbaar gebruikersfouten service of -agent. De meest voorkomende reden is mogelijk dat de ACS-verificatie is mislukt. Een mogelijke oorzaak van het probleem is dat er problemen met de configuratie van de NTP-server zijn en tijd op het apparaat is niet juist ingesteld. |Corrigeer de tijd (als er problemen zijn) en voer vervolgens de bewerking voor de registratie opnieuw uit. Als u de opdracht Set-HcsSystem - Timezone gebruiken om aan te passen van de tijdzone, elk woord in de tijdzone (bijvoorbeeld ' Pacific Standard Time') met een hoofdletter.  Als dit probleem zich blijft voordoen, [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor de volgende stappen. |
| 9 |Waarschuwing: Kan het apparaat niet activeren. De apparaatbeheerder van uw en StorSimple Snapshot Manager wachtwoorden zijn niet gewijzigd. |Als de registratie is mislukt, worden de apparaatbeheerder en StorSimple Snapshot Manager wachtwoorden worden niet gewijzigd. | |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Hulpprogramma's voor het oplossen van StorSimple-implementaties
StorSimple bevat verschillende hulpprogramma's die u gebruiken kunt om op te lossen van uw StorSimple-oplossing. Deze omvatten:

* Ondersteuning voor pakketten en logboeken van het apparaat.
* Cmdlets die speciaal is ontworpen voor het oplossen van problemen.

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Ondersteuning voor pakketten en logboeken van het apparaat beschikbaar voor het oplossen van problemen
Een ondersteuningspakket bevat alle relevante logboeken die het team van Microsoft Support helpen kunnen bij het oplossen van problemen met het apparaat. U kunt Windows PowerShell voor StorSimple gebruiken voor het genereren van een versleutelde ondersteuningspakket die u vervolgens met het ondersteunend personeel delen kunt.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>De logboeken of de inhoud van het ondersteuningspakket weergeven
1. Windows PowerShell voor StorSimple gebruiken voor het genereren van een ondersteuningspakket met, zoals beschreven in [maken en beheren van een ondersteuningspakket](storsimple-8000-create-manage-support-package.md).
2. Download de [ontsleuteling script](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokaal op uw clientcomputer.
3. Gebruik deze [stapsgewijze procedure](storsimple-8000-create-manage-support-package.md#edit-a-support-package) om te openen en ontsleutelen van het ondersteuningspakket.
4. De logboeken van de ontsleutelde ondersteuning-pakket hebben etw/etvx-indeling. U kunt de volgende stappen uit om deze bestanden in Windows logboeken weer te geven kunt uitvoeren:
   
   1. Voer de **eventvwr** opdracht op uw Windows-client. Hiermee wordt logboeken gestart.
   2. In de **acties** deelvenster, klikt u op **opgeslagen logboek openen** en wijst u de logboekbestanden in etvx/etw-indeling (het ondersteuningspakket). U kunt nu het bestand weergeven. Nadat u het bestand opent, kunt u met de rechtermuisknop op en sla het bestand als tekst.
      
      > [!IMPORTANT]
      > U kunt ook de **Get-WinEvent** cmdlet voor het openen van deze bestanden in Windows PowerShell. Zie voor meer informatie, [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) in de documentatie van Windows PowerShell-cmdlet-verwijzing.
     
5. Wanneer de logboeken in Logboeken openen, kunt u zoeken naar de volgende logboeken die problemen met betrekking tot de apparaatconfiguratie bevatten:
   
   * hcs_pfconfig/operationeel logboek
   * hcs_pfconfig/Config
6. In de logboekbestanden gezocht naar tekenreeksen met betrekking tot de cmdlets aangeroepen door de wizard setup. Zie [eerst setup-wizard proces](#first-time-setup-wizard-process) voor een lijst van deze cmdlets.
7. Als u nog niet om de oorzaak van het probleem te achterhalen, kunt u [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor de volgende stappen. Gebruik de stappen in [Maak een ondersteuningsaanvraag](storsimple-8000-contact-microsoft-support.md#create-a-support-request) wanneer u contact op met Microsoft Support voor hulp.

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdlets die beschikbaar zijn voor het oplossen van problemen
Gebruik de volgende Windows PowerShell-cmdlets voor het detecteren van fouten in de basisnetwerkverbinding.

* `Get-NetAdapter`: Met deze cmdlet gebruiken voor het detecteren van de status van netwerkinterfaces.
* `Test-Connection`: Deze cmdlet gebruiken om te controleren of de netwerkverbinding binnen en buiten het netwerk.
* `Test-HcsmConnection`: Deze cmdlet gebruiken om te controleren of de connectiviteit van een apparaat is geregistreerd.
* `Sync-HcsTime`: Gebruik deze cmdlet voor het weergeven van de tijd op het apparaat en tijdsynchronisatie met de NTP-server dwingen.
* `Enable-HcsPing` en `Disable-HcsPing`: deze cmdlets gebruiken om de hosts te pingen de netwerkinterfaces op uw StorSimple-apparaat toestaan. Standaard reageren de StorSimple-netwerkinterfaces niet op aanvragen van de ping.
* `Trace-HcsRoute`: Deze cmdlet gebruiken als een hulpmiddel voor het traceren van route. Deze pakketten verzendt naar elke router op de manier om een uiteindelijke bestemming gedurende een bepaalde periode en vervolgens worden berekend op basis van de pakketten die zijn geretourneerd door elke hop resultaten. Aangezien `Trace-HcsRoute` ziet u de mate van pakketverlies op elke router en koppeling, u kunt achterhalen welke routers of koppelingen mogelijk veroorzaakt door problemen met het netwerk.
* `Get-HcsRoutingTable`: Gebruik deze cmdlet om weer te geven van de lokale IP-routeringstabel.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Problemen met de cmdlet Get-NetAdapter oplossen
Bij het configureren van netwerkinterfaces voor de implementatie van een apparaat van de eerste keer, is de status van uw hardware niet beschikbaar in de gebruikersinterface van de StorSimple Device Manager-service omdat het apparaat nog niet is geregistreerd bij de service. Bovendien de **hardwarestatus** blade mogelijk niet altijd goed overeen met de status van het apparaat, met name als er problemen die invloed hebben op synchronisatie van de service zijn. In deze situaties kunt u de `Get-NetAdapter` cmdlet om te bepalen van de status en de status van de netwerkinterfaces.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Voor een overzicht van alle netwerkadapters op uw apparaat
1. Start Windows PowerShell voor StorSimple en typ vervolgens `Get-NetAdapter`. 
2. De uitvoer van de `Get-NetAdapter` cmdlet en de volgende richtlijnen om te begrijpen van de status van de netwerkinterface.
   
   * Als de interface in orde is en is ingeschakeld is, de **ifIndex** status wordt weergegeven als **van**.
   * Als de interface in orde is, maar niet fysiek zijn verbonden (via een netwerkkabel), de **ifIndex** wordt weergegeven als **uitgeschakelde**.
   * Als de interface in orde is, maar niet ingeschakeld is, de **ifIndex** status wordt weergegeven als **NotPresent**.
   * Als de interface niet bestaat, wordt deze niet weergegeven in deze lijst. De gebruikersinterface van de StorSimple Device Manager-service wordt nog steeds deze interface weergegeven in een foutstatus.

Ga voor meer informatie over het gebruik van deze cmdlet naar [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter?view=win10-ps) in de Windows PowerShell-cmdlet-verwijzing.

De volgende secties ziet u voorbeelden van uitvoer van de `Get-NetAdapter` cmdlet.

 In deze voorbeelden controller 0 is de passieve controller en is als volgt geconfigureerd:

* DATA 0, 1 van de gegevens, DATA 2 en DATA 3-netwerk interfaces aanwezig op het apparaat was.
* GEGEVENS 4 en 5 gegevens netwerkinterfacekaarten zijn niet aanwezig ze worden daarom niet weergegeven in de uitvoer.
* DATA 0 is ingeschakeld.

Controller 1 is de actieve controller en is als volgt geconfigureerd:

* DATA 0, 1 gegevens, DATA 2, DATA 3, 4 van de gegevens en gegevens 5 network interfaces aanwezig op het apparaat was.
* DATA 0 is ingeschakeld.

**Voorbeelduitvoer: controller 0**

Hier volgt de uitvoer van controller 0 (de passieve controller). GEGEVENS 1, DATA 2 en DATA 3 zijn niet verbonden. GEGEVENS 4 en 5 van de gegevens worden niet weergegeven omdat ze niet aanwezig op het apparaat zijn.

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Voorbeelduitvoer: controller 1**

Hier volgt de uitvoer van controller 1 (de actieve controller). Alleen de DATA 0-netwerkinterface op het apparaat is geconfigureerd en een werkende.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent


## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Problemen oplossen met de cmdlet Test-Connection
U kunt de `Test-Connection` cmdlet om te bepalen of uw StorSimple-apparaat verbinding met het externe netwerk maken kan. Als alle netwerkparameters, met inbegrip van de DNS-server correct zijn geconfigureerd in de installatiewizard, kunt u de `Test-Connection` cmdlet om te pingen van een bekend adres buiten het netwerk, zoals outlook.com.

Ping-opdracht naar het oplossen van problemen met de netwerkverbinding met deze cmdlet als ping is uitgeschakeld, moet u inschakelen.

Zie de volgende voorbeelden van uitvoer van de `Test-Connection` cmdlet.

> [!NOTE]
> In het eerste voorbeeld is het apparaat geconfigureerd met een onjuiste DNS-naam. In het tweede voorbeeld wordt is de DNS-server juist.

**Voorbeelduitvoer: onjuiste DNS-naam**

Er wordt geen uitvoer voor de IPV4 en IPV6-adressen, waarmee wordt aangegeven dat de DNS-server niet is omgezet in het volgende voorbeeld. Dit betekent dat er geen verbinding met het externe netwerk en een juiste DNS-server moet worden opgegeven.

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Voorbeelduitvoer: de juiste DNS**

In het volgende voorbeeld retourneert de DNS-server het IPV4-adres, waarmee wordt aangegeven dat de DNS-server correct is geconfigureerd. Hiermee bevestigt u dat er een verbinding met het externe netwerk is.

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Problemen met de cmdlet Test-HcsmConnection oplossen
Gebruik de `Test-HcsmConnection` cmdlet voor een apparaat dat al is verbonden met en geregistreerd bij uw StorSimple Device Manager-service. Deze cmdlet kunt u controleren of de netwerkverbinding tussen een geregistreerd apparaat en de bijbehorende StorSimple Device Manager-service. U kunt deze opdracht uitvoert op Windows PowerShell voor StorSimple.

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>De cmdlet Test-HcsmConnection uitvoeren
1. Zorg ervoor dat het apparaat is geregistreerd.
2. Controleer de status van het apparaat. Als het apparaat is gedeactiveerd, in de onderhoudsmodus bevinden, of offline is, ziet u mogelijk een van de volgende fouten:
   
   * ErrorCode.CiSDeviceDecommissioned – dit geeft aan dat het apparaat is gedeactiveerd.
   * ErrorCode.DeviceNotReady – dit geeft aan dat het apparaat in de onderhoudsmodus bevindt.
   * ErrorCode.DeviceNotReady – dit geeft aan dat het apparaat niet online is.
3. Controleren of de StorSimple Device Manager-service wordt uitgevoerd (Gebruik de [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) cmdlet). Als de service wordt niet uitgevoerd, ziet u mogelijk de volgende fouten:
   
   * ErrorCode.CiSApplianceAgentNotOnline
   * ErrorCode.CisPowershellScriptHcsError – dit geeft aan dat er een uitzondering opgetreden is bij het uitvoeren van Get-ClusterResource.
4. Controleer de token van de Access Control Service (ACS). Als dit een Webuitzondering genereert, is het mogelijk het resultaat van een probleem gateway, een ontbrekende proxy-verificatie, een onjuiste DNS-naam of een verificatiefout opgetreden. U ziet mogelijk de volgende fouten:
   
   * ErrorCode.CiSApplianceGateway – dit geeft aan dat een uitzondering HttpStatusCode.BadGateway: de naam resolver-service kan de hostnaam niet omzetten.
   * ErrorCode.CiSApplianceProxy – dit geeft aan dat een uitzondering HttpStatusCode.ProxyAuthenticationRequired (HTTP-statuscode 407): de client kan niet worden geverifieerd met de proxyserver.
   * ErrorCode.CiSApplianceDNSError – dit geeft aan dat een uitzondering WebExceptionStatus.NameResolutionFailure: de naam resolver-service kan de hostnaam niet omzetten.
   * ErrorCode.CiSApplianceACSError – dit geeft aan dat de service heeft een verificatiefout geretourneerd, maar er een verbinding is.
     
     Als dit een Webuitzondering niet genereren is, controleert u ErrorCode.CiSApplianceFailure. Hiermee wordt aangegeven dat het apparaat is mislukt.
5. Controleer de verbinding van de cloud-service. Als de service een Webuitzondering genereert, ziet u mogelijk de volgende fouten:
   
   * ErrorCode.CiSApplianceGateway – dit geeft aan dat een uitzondering HttpStatusCode.BadGateway: een tussenliggende proxyserver heeft een ongeldige aanvraag ontvangen van een andere proxy of van de oorspronkelijke server.
   * ErrorCode.CiSApplianceProxy – dit geeft aan dat een uitzondering HttpStatusCode.ProxyAuthenticationRequired (HTTP-statuscode 407): de client kan niet worden geverifieerd met de proxyserver.
   * ErrorCode.CiSApplianceDNSError – dit geeft aan dat een uitzondering WebExceptionStatus.NameResolutionFailure: de naam resolver-service kan de hostnaam niet omzetten.
   * ErrorCode.CiSApplianceACSError – dit geeft aan dat de service heeft een verificatiefout geretourneerd, maar er een verbinding is.
     
     Als dit een Webuitzondering niet genereren is, controleert u ErrorCode.CiSApplianceSaasServiceError. Dit duidt op een probleem met de StorSimple Device Manager-service.
6. Controleer de connectiviteit van Azure Service Bus. ErrorCode.CiSApplianceServiceBusError geeft aan dat het apparaat kan geen verbinding met de Service Bus maken.

De logboekbestanden CiSCommandletLog0Curr.errlog en CiSAgentsvc0Curr.errlog heeft meer informatie, zoals details van uitzondering.

Voor meer informatie over het gebruik van de cmdlet, gaat u naar [Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) in Windows PowerShell-referentiedocumentatie.

> [!IMPORTANT]
> U kunt deze cmdlet voor zowel de actieve als de passieve controller uitgevoerd.

Zie de volgende voorbeelden van uitvoer van de `Test-HcsmConnection` cmdlet.

**Voorbeeld van uitvoer: geregistreerd apparaat waarop StorSimple Update 3**

      Controller1>Test-HcsmConnection

      Checking device registration state  ... Success
      Device registered successfully

      Checking primary NTP server [time.windows.com] ... Success

      Checking web proxy  ... NOT SET

      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET

      Checking device online  ... Success

      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success

      Checking connectivity from device to service  ... This will take a few minutes.

      Checking connectivity from device to service  ... Success

      Checking connectivity from service to device  ... Success

      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Voorbeelduitvoer: offline apparaat** 

Dit voorbeeld is van een apparaat met de status van **Offline** in Azure portal.

     Checking device registrationstate: Success
     Device is registered successfully
     Checking connectivity from device to SaaS.. Failure

Het apparaat kan geen verbinding maken met behulp van de huidige configuratie van de web-proxy. Dit wordt mogelijk een probleem met de configuratie van uw webproxy of een probleem met de netwerkverbinding. In dit geval moet u ervoor dat de webproxy-instellingen juist zijn en uw webproxyservers online en bereikbaar zijn.

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Problemen met de cmdlet Sync-HcsTime oplossen
Deze cmdlet gebruiken om de apparaattijd weer te geven. Als de tijd op het apparaat een offset met de NTP-server heeft, kunt u deze cmdlet kunt vervolgens de tijd met een force synchroniseren met uw NTP-server gebruiken.
- Als de afstand tussen het apparaat en de NTP-server groter dan 5 minuten is, ziet u een waarschuwing. 
- Als de verschuiving van de groter is dan 15 minuten, wordt het apparaat offline gaat. U kunt deze cmdlet nog steeds gebruiken om af te dwingen een tijdsynchronisatie. 
- Echter, als de verschuiving van de groter is dan 15 uur, klikt u vervolgens u wordt pas weer force-synchronisatie die de tijd en een foutbericht wordt weergegeven.

**Voorbeeld van uitvoer: geforceerde tijdsynchronisatie met Sync-HcsTime**

     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.

     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Problemen oplossen met de cmdlets Enable-HcsPing en Disable-HcsPing
Deze cmdlets gebruiken om ervoor te zorgen dat de netwerkinterfaces op uw apparaat op ICMP-ping-aanvragen reageert. Standaard reageren de StorSimple-netwerkinterfaces niet op aanvragen van de ping. Met deze cmdlet is de eenvoudigste manier om te weten of uw apparaat online en bereikbaar is is.

**Voorbeeld van uitvoer: Enable-HcsPing en Disable-HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Problemen oplossen met de cmdlet Trace-HcsRoute
Deze cmdlet gebruiken als een hulpmiddel voor het traceren van route. Deze pakketten verzendt naar elke router op de manier om een uiteindelijke bestemming gedurende een bepaalde periode en vervolgens worden berekend op basis van de pakketten die zijn geretourneerd door elke hop resultaten. Omdat de cmdlet ziet u de mate van pakketverlies op elke router en koppeling, u kunt achterhalen welke routers of koppelingen mogelijk veroorzaakt door problemen met het netwerk.

**Voorbeeld van de uitvoer van het traceren van de route van een pakket met de Trace-HcsRoute**

     Controller0>Trace-HcsRoute -Target 10.126.174.25

     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]

     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]

     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Problemen oplossen met de cmdlet Get-HcsRoutingTable
Deze cmdlet gebruiken om de routeringstabel voor uw StorSimple-apparaat weer te geven. Een routeringstabel is een set regels die u bepalen waar de gegevenspakketten die via een netwerk IP (Internet Protocol kunt) wordt gevraagd.

De routeringstabel toont de interfaces en de gateway die de gegevens naar de opgegeven netwerken routeert. Het biedt ook de routering metrische gegevens die de besluitvormer voor het pad naar een bepaald doel te bereiken. Des te lager de routering metrische gegevens, hoe hoger de voorkeur.

Bijvoorbeeld, hebt u 2 netwerkinterfaces DATA 2 en DATA 3, verbonden met Internet. Als de routering metrische gegevens voor de DATA 2 en DATA 3 respectievelijk 15 en 261 zijn, is 2 van de gegevens met lagere routering metric de voorkeursinterface gebruikt voor het bereiken van het Internet.

Als u Update 1 op uw StorSimple-apparaat worden uitgevoerd, heeft uw DATA 0-netwerkinterface de hoogste voorkeur voor het cloudverkeer. Dit betekent dat zelfs als er andere interfaces ingeschakeld voor de cloud zijn, het cloudverkeer zou worden gerouteerd via DATA 0.

Als u de `Get-HcsRoutingTable` cmdlet zonder parameters (zoals het volgende voorbeeld wordt getoond) de cmdlet zowel IPv4 als IPv6-routeringstabel wordt uitgevoerd. U kunt ook opgeven `Get-HcsRoutingTable -IPv4` of `Get-HcsRoutingTable -IPv6` om op te halen van een relevante routeringstabel.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================

      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================

      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None

      Controller0>

## <a name="step-by-step-storsimple-troubleshooting-example"></a>Voorbeeld van stapsgewijze StorSimple-probleemoplossing
Het volgende voorbeeld ziet stapsgewijze oplossen van problemen met een StorSimple-implementatie. In het voorbeeldscenario device Registration service is mislukt met een foutbericht met de mededeling dat de instellingen van het netwerk of de DNS-naam onjuist is.

Het foutbericht dat wordt geretourneerd is:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

De fout wordt mogelijk veroorzaakt door een van de volgende:

* Installatie van de onjuiste hardware
* Defecte netwerk (s)
* Onjuist IP-adres, subnetmasker, gateway, primaire DNS-server of webproxy
* Ongeldige registratiesleutel
* Onjuiste firewall-instellingen

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Om te zoeken en los het probleem van de registratie van apparaat
1. Controleer de apparaatconfiguratie van uw: Voer op de actieve controller `Invoke-HcsSetupWizard`.
   
   > [!NOTE]
   > De wizard setup moet uitvoeren op de actieve controller. Om te controleren dat u met de actieve controller verbonden bent, bekijkt u de banner weergegeven in de seriële console. De banner geeft aan of u bent verbonden met controller 0 of 1-controller, en of de domeincontroller actief of passief is. Ga voor meer informatie naar [actieve controller op uw apparaat identificeren](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
   
2. Zorg ervoor dat het apparaat juist is bekabeld: Controleer het netwerk op het apparaat terug gegevensvlak-bekabeling. De bekabeling is specifiek voor het model. Ga voor meer informatie naar [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
   
   > [!NOTE]
   > Als u van 10 GbE-netwerkpoorten gebruikmaakt, moet u het gebruik van de opgegeven QSFP SFP-adapters en SFP-kabels. Zie voor meer informatie de [lijst kabels en switches infraroodzenders aanbevolen voor de 10 GbE-poorten](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
  
3. Controleer of de status van de netwerkinterface:
   
   * Gebruik de cmdlet Get-NetAdapter voor het detecteren van de status van de netwerkinterfaces voor de DATA 0. 
   * Als de koppeling wordt niet werkt, de **ifindex** status wordt aangegeven dat de interface is niet actief is. Vervolgens moet u om te controleren of de netwerkverbinding van de poort op het apparaat en de switch. U moet ook sprake is van slechte kabels. 
   * Als u vermoedt dat de DATA 0-poort op de actieve controller is mislukt, u dit controleren kunt door verbinding te maken met de DATA 0-poort op controller 1. U kunt dit controleren de netwerkkabel verbreken op de achterkant van het apparaat vanaf controller 0, sluit de kabel en controller 1 en voer de cmdlet Get-NetAdapter opnieuw uit.
     Als de DATA 0-op een domeincontroller poort is mislukt, [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor de volgende stappen. U moet mogelijk de controller op uw systeem vervangen.
4. Controleer of de verbinding met de switch:
   
   * Zorg ervoor dat DATA 0-netwerkinterfaces op controller 0 en controller 1 in uw primaire behuizing zich op hetzelfde subnet bevinden. 
   * Controleer de hub of een router. Normaal gesproken moet u beide controllers verbinding maken met de dezelfde hub of een router. 
   * Zorg ervoor dat de switches die u voor de verbinding gebruikt DATA 0 voor beide controllers in hetzelfde vLAN hebben.
5. Elimineer gebruikersfouten:
   
   * De wizard setup opnieuw uitvoeren (Voer **Invoke-hcssetupwizard uit**), en voer de waarden opnieuw om het ervoor te zorgen dat er geen fouten zijn. 
   * Controleer of de registratie van de sleutel die wordt gebruikt. Dezelfde registratiesleutel kan worden gebruikt op meerdere apparaten verbinden met een StorSimple Device Manager-service. Gebruik de procedure in [de serviceregistratiesleutel ophalen](storsimple-8000-manage-service.md#get-the-service-registration-key) om ervoor te zorgen dat u van de juiste registratiesleutel gebruikmaakt.
     
     > [!IMPORTANT]
     > Als u meerdere services die worden uitgevoerd hebt, moet u om ervoor te zorgen dat de registratiesleutel voor de betreffende service wordt gebruikt om het apparaat te registreren. Als u een apparaat hebt geregistreerd met de verkeerde StorSimple Device Manager-service, moet u [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor de volgende stappen. Mogelijk moet de fabrieksinstellingen van het apparaat (deze kan leiden tot verlies van gegevens) worden uitgevoerd en vervolgens verbinden met de gewenste service.
     > 
     > 
6. Gebruik de cmdlet Test-Connection om te controleren of dat er een verbinding met het externe netwerk. Ga voor meer informatie naar [problemen oplossen met de cmdlet Test-Connection](#troubleshoot-with-the-test-connection-cmdlet).
7. Controleer of firewall storingen. Als u hebt gecontroleerd of de virtuele IP-adres (VIP), subnet, gateway en DNS-instellingen juist, zijn en ziet u nog steeds problemen met de netwerkverbinding, dan is het mogelijk dat de communicatie tussen uw apparaat en het externe netwerk door uw firewall wordt geblokkeerd. U moet ervoor zorgen dat de poorten 80 en 443 beschikbaar op uw StorSimple-apparaat voor uitgaande communicatie zijn. Zie voor meer informatie, [netwerkvereisten voor uw StorSimple-apparaat](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).
8. Bekijk de logboeken. Ga naar [ondersteuning voor pakketten en logboeken van het apparaat beschikbaar voor het oplossen van](#support-packages-and-device-logs-available-for-troubleshooting).
9. Als de voorgaande stappen het probleem niet wordt opgelost [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor hulp.

## <a name="next-steps"></a>Volgende stappen
[Leer hoe u het hulpprogramma Diagnostics gebruiken om op te lossen van een StorSimple-apparaat](storsimple-8000-diagnostics.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
