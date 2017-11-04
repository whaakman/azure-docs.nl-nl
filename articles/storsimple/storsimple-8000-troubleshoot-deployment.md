---
title: StorSimple 8000 series implementatieproblemen oplossen | Microsoft Docs
description: Beschrijft hoe opsporen en oplossen van fouten die optreden bij het implementeren van StorSimple.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: d6642231bf9d012b0015902c82d2bfde3b86285c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-storsimple-device-deployment-issues"></a>StorSimple-apparaat implementatieproblemen oplossen
## <a name="overview"></a>Overzicht
Dit artikel bevat nuttige richtlijnen voor probleemoplossing voor uw Microsoft Azure StorSimple-implementatie. Hierin worden veelvoorkomende problemen, mogelijke oorzaken en aanbevolen stappen waarmee u problemen kunt oplossen die u ervaren kunt wanneer u StorSimple configureren. 

Deze informatie geldt voor zowel het fysieke StorSimple 8000 serie-apparaat en het toestel StorSimple-Cloud.

> [!NOTE]
> Apparaat configuratie gerelateerde problemen die u mogelijk geconfronteerd kunnen optreden wanneer u het apparaat voor de eerste keer implementeren of ze later, wanneer het apparaat operationele kunnen optreden. In dit artikel gaat over het oplossen van problemen bij de eerste implementatie. Voor het oplossen van een operationeel-apparaat, gaat u naar [het diagnostisch hulpprogramma gebruiken om op te lossen een operationele apparaat](storsimple-8000-diagnostics.md).

Dit artikel wordt ook de hulpprogramma's voor het oplossen van StorSimple implementaties beschreven en biedt een voorbeeld van stapsgewijze probleemoplossing.

## <a name="first-time-deployment-issues"></a>Problemen bij de eerste implementatie
Als u een probleem bij het implementeren van uw apparaat voor de eerste keer, het volgende overwegen:

* Als u een fysiek apparaat oplossen wilt, zorg ervoor dat de hardware is geïnstalleerd en geconfigureerd zoals beschreven in [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
* Controleer de vereisten voor de implementatie. Zorg ervoor dat u de informatie die wordt beschreven hebt in de [Configuratiecontrolelijst voor implementatie](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist).
* Bekijk de Release-opmerkingen voor StorSimple om te zien als het probleem wordt beschreven. De release-opmerkingen bevatten oplossingen voor bekende installatieproblemen. 

Tijdens de implementatie van het apparaat problemen de meest voorkomende die gebruikers face optreden wanneer ze de wizard setup worden uitgevoerd wanneer ze het apparaat en registreren via Windows PowerShell voor StorSimple. (U kunt Windows PowerShell voor StorSimple registreren en te configureren van uw StorSimple-apparaat. Zie voor meer informatie over apparaatregistratie [stap 3: configureren en registreren van uw apparaat via Windows PowerShell voor StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

De volgende secties kunt u problemen oplossen die u tegenkomt wanneer u het StorSimple-apparaat voor het eerst configureert.

## <a name="first-time-setup-wizard-process"></a>Wizard installatieproces eerst
De volgende stappen geven een overzicht van het installatieproces van de wizard. Zie voor gedetailleerde informatie, [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

1. Voer de [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) cmdlet start de wizard setup die u bij de resterende begeleiden zal stappen. 
2. Het netwerk configureren: de wizard setup kunt u netwerkinstellingen voor de DATA 0-netwerkinterface op uw StorSimple-apparaat configureren. Deze instellingen omvatten het volgende:
   * Virtuele IP-adres (VIP), subnetmasker en gateway – de [Set HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) cmdlet wordt uitgevoerd op de achtergrond. Configureert het de IP-adres, subnetmasker en gateway voor de DATA 0-netwerkinterface op uw StorSimple-apparaat.
   * Primaire DNS-server: de [Set HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) cmdlet wordt uitgevoerd op de achtergrond. De DNS-instellingen voor uw StorSimple-oplossing wordt geconfigureerd.
   * NTP-server: de [Set HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet wordt uitgevoerd op de achtergrond. Configureert het NTP-server-instellingen voor uw StorSimple-oplossing.
   * Optionele webproxy – de [Set HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) cmdlet wordt uitgevoerd op de achtergrond. Het wordt ingesteld en wordt de webproxyconfiguratie voor uw StorSimple-oplossing.
3. Het wachtwoord instellen: de volgende stap is het instellen van het beheerderswachtwoord van het apparaat.
   Het beheerderswachtwoord van het apparaat wordt gebruikt voor aanmelding bij uw apparaat. Het standaardapparaatwachtwoord is **Password1**.
        
     > [!IMPORTANT]
     > Wachtwoorden zijn vóór registratie worden verzameld, maar pas nadat u het apparaat wordt geregistreerd toegepast. Als er een fout is met het toepassen van een wachtwoord, wordt u gevraagd het wachtwoord opnieuw opgeven totdat de vereiste wachtwoorden (die aan de complexiteitsvereisten) worden verzameld.
     
4. Registreer het apparaat: de laatste stap is het registreren van het apparaat met de StorSimple-apparaat Manager-service worden uitgevoerd in Microsoft Azure. De registratie, moet u [de serviceregistratiesleutel ophalen](storsimple-8000-manage-service.md#get-the-service-registration-key) vanuit de Azure-portal en geef deze in de wizard setup. **Nadat het apparaat is geregistreerd, wordt er een gegevensversleutelingssleutel van service aan u geleverd. Zorg ervoor dat deze versleutelingssleutel op een veilige locatie omdat het vereiste alle latere apparaten registreren bij de service.**

## <a name="common-errors-during-device-deployment"></a>Veelvoorkomende fouten tijdens de implementatie van het apparaat
De volgende tabellen worden de algemene fouten die wanneer optreden mogelijk u:

* De vereiste netwerkinstellingen configureren.
* De optionele web proxy-instellingen configureren.
* Het beheerderswachtwoord van het apparaat instellen.
* Registreer het apparaat.

## <a name="errors-during-the-required-network-settings"></a>Fouten tijdens de vereiste netwerkinstellingen
| Nee. | Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Met deze opdracht kan alleen worden uitgevoerd op de actieve controller. |Configuratie werd in de passieve controller uitgevoerd. |Deze opdracht uitvoert vanaf de actieve controller. Zie voor meer informatie [een actieve domeincontroller op uw apparaat identificeren](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| 2 |Invoke-HcsSetupWizard: Apparaat niet gereed. |Er zijn problemen met de netwerkverbinding op DATA 0. |Controleer de fysieke netwerkverbinding op DATA 0. |
| 3 |Invoke-HcsSetupWizard: Er is een IP-adresconflict met een ander systeem in het netwerk (uitzondering van HRESULT: 0x80070263). |Het IP-adres opgegeven voor de DATA 0 is al in gebruik door een ander systeem. |Geef een nieuwe IP-adres dat niet wordt gebruikt. |
| 4 |Invoke-HcsSetupWizard: Een cluster-bron is mislukt. (Uitzondering van HRESULT: 0x800713AE). |Dubbele VIP. Het opgegeven IP-adres is al in gebruik. |Geef een nieuwe IP-adres dat niet wordt gebruikt. |
| 5 |Invoke-HcsSetupWizard: Ongeldig IPv4-adres. |Het IP-adres is opgegeven in een onjuiste indeling. |Controleer de indeling en geef uw IP-adres opnieuw. Zie voor meer informatie [IPv4-adressering][1]. |
| 6 |Invoke-HcsSetupWizard: Ongeldig IPv6-adres. |Het IP-adres is opgegeven in een onjuiste indeling. |Controleer de indeling en geef uw IP-adres opnieuw. Zie voor meer informatie [IPv6-adressering][2]. |
| 7 |Invoke-HcsSetupWizard: Er zijn geen eindpunten meer beschikbaar is via de eindpunttoewijzer. (Uitzondering van HRESULT: 0x800706D9) |De clusterfunctionaliteit werkt niet. |[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor volgende stappen. |

## <a name="errors-during-the-optional-web-proxy-settings"></a>Fouten tijdens de optionele web proxy-instellingen
| Nee. | Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Ongeldige parameter (uitzondering van HRESULT: 0x80070057) |Een van de opgegeven parameters voor de proxy-instellingen is niet geldig. |De URI is niet opgegeven in de juiste indeling. Gebruik de volgende indeling: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 |Invoke-HcsSetupWizard: RPC-server niet beschikbaar (uitzondering van HRESULT: 0x800706ba) |De hoofdoorzaak is een van de volgende opties:<ol><li>Het cluster is niet maximaal.</li><li>De passieve controller kan niet communiceren met de actieve controller en de opdracht wordt uitgevoerd vanaf een passieve-controller.</li></ol> |Afhankelijk van de hoofdoorzaak:<ol><li>[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om ervoor te zorgen dat het cluster actief is.</li><li>Voer de opdracht uit vanaf de actieve controller. Als u uitvoeren van de opdracht van de passieve controller wilt, moet u om ervoor te zorgen dat de passieve controller met de actieve controller communiceren kan. U moet [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) als deze de verbinding verbroken is.</li></ol> |
| 3 |Invoke-HcsSetupWizard: RPC-aanroep is mislukt (uitzondering van HRESULT: 0x800706be) |Cluster is niet actief. |[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) om ervoor te zorgen dat het cluster actief is. |
| 4 |Invoke-HcsSetupWizard: Clusterbron niet vinden (uitzondering van HRESULT: 0x8007138f) |De clusterbron is niet gevonden. Dit kan gebeuren wanneer de installatie niet juist is. |Mogelijk moet u het apparaat opnieuw instellen op de standaardfabrieksinstellingen. [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) clusterbron maken. |
| 5 |Invoke-HcsSetupWizard: Clusterbron niet online (uitzondering van HRESULT: 0x8007138c) |Clusterresources zijn niet online. |[Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor volgende stappen. |

## <a name="errors-related-to-device-administrator-password"></a>Fouten met betrekking tot het beheerderswachtwoord apparaat
De beheerder van het standaardapparaatwachtwoord is **Wachtwoord1**. Dit wachtwoord verloopt na de eerste keer aanmelden; Daarom moet u de wizard setup gebruiken om dit te wijzigen. U moet een nieuw apparaat beheerderswachtwoord opgeven wanneer u het apparaat voor het eerst registreren. 

Zorg ervoor dat uw wachtwoorden voldoen aan de volgende vereisten:

* Het beheerderswachtwoord van uw apparaat moet tussen 8 en 15 tekens lang zijn.
* Wachtwoorden moeten 3 van de volgende tekentypen van 4 bevatten: kleine letters, hoofdletters, numerieke en speciale. 
* Uw wachtwoord mag niet hetzelfde zijn als de laatste 24 wachtwoorden.

Bovendien, houd er rekening mee dat wachtwoorden verlopen elk jaar en kunnen pas worden gewijzigd nadat u het apparaat wordt geregistreerd. Als de registratie voor een of andere reden mislukt, wordt de wachtwoorden worden niet gewijzigd.

Voor meer informatie over het beheerderswachtwoord apparaat, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken om uw StorSimple-wachtwoord wijzigen](storsimple-8000-change-passwords.md).

U kunt een of meer van de volgende fouten kan optreden bij het instellen van de apparaatbeheerder en StorSimple Snapshot Manager wachtwoorden.

| Nee. | Foutbericht | Aanbevolen actie |
| --- | --- | --- |
| 1 |Het wachtwoord overschrijdt de maximumlengte. |Het beheerderswachtwoord van uw apparaat moet tussen 8 en 15 tekens lang zijn. |
| 2 |Het wachtwoord voldoet niet aan de vereiste lengte. |Het beheerderswachtwoord van uw apparaat moet tussen 8 en 15 tekens lang zijn.|
| 3 |Het wachtwoord moet letters bevatten. |Wachtwoorden moeten 3 van de volgende tekentypen van 4 bevatten: kleine letters, hoofdletters, numerieke en speciale. Zorg ervoor dat uw wachtwoord voldoet aan deze vereisten. |
| 4 |Het wachtwoord moet tekens bevatten. |Wachtwoorden moeten 3 van de volgende tekentypen van 4 bevatten: kleine letters, hoofdletters, numerieke en speciale. Zorg ervoor dat uw wachtwoord voldoet aan deze vereisten. |
| 5 |Het wachtwoord moet tekens bevatten. |Wachtwoorden moeten 3 van de volgende tekentypen van 4 bevatten: kleine letters, hoofdletters, numerieke en speciale. Zorg ervoor dat uw wachtwoord voldoet aan deze vereisten. |
| 6 |Het wachtwoord moet 3 van de volgende tekentypen van 4 bevatten: hoofdletters, kleine letters, numerieke en speciale. |Uw wachtwoord bevat niet de vereiste typen tekens. Zorg ervoor dat uw wachtwoord voldoet aan deze vereisten. |
| 7 |Parameter komt niet overeen met de bevestiging. |Zorg ervoor dat uw wachtwoord voldoet aan alle vereisten en dat u deze correct ingevoerd. |
| 8 |Uw wachtwoord mag niet overeenkomen met de standaardwaarde. |Is het standaardwachtwoord *Wachtwoord1*. U moet dit wachtwoord wijzigen nadat u zich aanmeldt voor het eerst. |
| 9 |Het ingevoerde wachtwoord komt niet overeen met het wachtwoord voor het apparaat. Geef het wachtwoord. |Controleer het wachtwoord en typt u het opnieuw. |

Wachtwoorden worden verzameld voordat het apparaat is geregistreerd, maar alleen na een succesvolle registratie toegepast. Het wachtwoord herstelwerkstroom vereist dat het apparaat moet worden geregistreerd.

> [!IMPORTANT]
> In het algemeen als een poging tot toepassen van een wachtwoord is mislukt, de software herhaaldelijk geprobeerd te verzamelen van het wachtwoord, totdat hij erin slaagt. In zeldzame gevallen, kan niet het wachtwoord worden toegepast. In dit geval kunt u het apparaat te registreren en doorgaan, maar de wachtwoorden worden niet gewijzigd. U kunt het beheerderswachtwoord van het apparaat wijzigen na de registratie van de Azure-portal.


U kunt het wachtwoord in de Azure portal via de service Manager voor StorSimple-apparaat opnieuw instellen. Ga voor meer informatie naar [wijzigen van het beheerderswachtwoord van het apparaat](storsimple-8000-change-passwords.md#change-the-device-administrator-password).

## <a name="errors-during-device-registration"></a>Fouten tijdens het registreren van apparaten
De StorSimple-apparaat Manager-service worden uitgevoerd in Microsoft Azure kunt u het apparaat te registreren. Er kan een of meer van de volgende problemen optreden tijdens de apparaatregistratie van het.

| Nee. | Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| --- | --- | --- | --- |
| 1 |Fout 350027: Registreer het apparaat met de StorSimple-Apparaatbeheer is mislukt. | |Wacht een paar minuten en probeer het opnieuw. Als het probleem zich blijft voordoen, [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md). |
| 2 |Fout 350013: Een fout opgetreden bij het registreren van het apparaat. Dit kan zijn vanwege onjuiste serviceregistratiesleutel. | |Registreer het apparaat opnieuw met de juiste serviceregistratiesleutel. Zie voor meer informatie [de serviceregistratiesleutel ophalen.](storsimple-8000-manage-service.md#get-the-service-registration-key) |
| 3 |Fout 350063: Verificatie Apparaatbeheer StorSimple-service is doorgegeven, maar de registratie is mislukt. Probeer de bewerking na enige tijd opnieuw. |Deze fout geeft aan dat verificatie met ACS is geslaagd, maar de register-aanroep naar de service is mislukt. Dit kan zijn veroorzaakt door een storing sporadisch netwerk. |Als het probleem zich blijft, neem voordoen [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md). |
| 4 |Fout 350049: De service kan niet worden bereikt tijdens de registratie. |Wanneer de aanroep naar de service wordt uitgevoerd, is een Webuitzondering ontvangen. In sommige gevallen kan ophalen dit opgelost door de bewerking later opnieuw proberen. |Controleer uw IP-adres en DNS-naam en probeer het opnieuw. Als het probleem zich blijft voordoen, [contact op met Microsoft Support.](storsimple-8000-contact-microsoft-support.md) |
| 5 |Fout 350031: Het apparaat is al geregistreerd. | |Er is geen actie nodig. |
| 6 |Fout 350016: Apparaatregistratie is mislukt. | |Controleer of dat de registratiesleutel juist is. |
| 7 |Invoke-HcsSetupWizard: Is een fout opgetreden tijdens het registreren van uw apparaat; Dit kan zijn veroorzaakt door onjuiste IP-adres of de DNS-naam. Controleer de netwerkinstellingen en probeer het opnieuw. Als het probleem zich blijft voordoen, [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md). (Fout 350050) |Zorg ervoor dat het apparaat het externe netwerk kan pingen. Als u geen verbinding met extern netwerk, wordt de registratie mislukken bij deze fout. Deze fout wordt mogelijk een combinatie van een of meer van de volgende opties:<ul><li>Onjuiste IP</li><li>Onjuiste subnet</li><li>Onjuiste gateway</li><li>Onjuiste DNS-instellingen</li></ul> |Zie de stappen in de [voorbeeld van stapsgewijze probleemoplossing](#step-by-step-storsimple-troubleshooting-example). |
| 8 |Invoke-HcsSetupWizard: De huidige bewerking is mislukt vanwege een interne servicefout [0x1FBE2]. Probeer het later opnieuw. Als het probleem zich blijft voordoen, neem contact op met Microsoft Support. |Dit is een algemene fout gegenereerd voor alle gebruikers onzichtbaar fouten vanuit de service of -agent. De meest voorkomende reden kan zijn dat de ACS-verificatie is mislukt. Een mogelijke oorzaak van de fout is dat er problemen met de configuratie van de NTP-server zijn en tijd op het apparaat is niet juist ingesteld. |Corrigeer de tijd (als er problemen zijn) en probeer het opnieuw registreren. Als u de opdracht Set-HcsSystem - tijdzone gebruiken om aan te passen, de tijdzone, beginhoofdletter in de tijdzone (bijvoorbeeld "Pacific (standaardtijd)").  Als dit probleem zich blijft voordoen, [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor volgende stappen. |
| 9 |Waarschuwing: Kan het apparaat niet activeren. De apparaatbeheerder van uw en StorSimple Snapshot Manager wachtwoorden zijn niet gewijzigd. |Als de registratie is mislukt, worden de apparaatbeheerder en StorSimple Snapshot Manager wachtwoorden worden niet gewijzigd. | |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Hulpprogramma's voor probleemoplossing StorSimple-implementaties
StorSimple bevat verschillende hulpprogramma's die u gebruiken kunt om op te lossen uw StorSimple-oplossing. Deze omvatten:

* Ondersteuning voor pakketten en apparaatlogboeken.
* De cmdlets is speciaal ontworpen voor het oplossen van problemen.

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Ondersteuning voor pakketten en apparaatlogboeken beschikbaar voor het oplossen van problemen
Een ondersteuningspakket bevat alle relevante logboeken die het team van Microsoft Support bij het oplossen van problemen met apparaat kunnen helpen. U kunt Windows PowerShell voor StorSimple gebruiken voor het genereren van een versleutelde ondersteuningspakket die u vervolgens met iemand van ondersteuning delen kunt.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>De logboeken of de inhoud van het ondersteuningspakket weergeven
1. Windows PowerShell voor StorSimple gebruiken voor het genereren van een ondersteuningspakket, zoals beschreven in [maken en beheren van een ondersteuningspakket](storsimple-8000-create-manage-support-package.md).
2. Download de [ontsleuteling script](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokaal op de clientcomputer.
3. Gebruik deze [stapsgewijze procedure](storsimple-8000-create-manage-support-package.md#edit-a-support-package) openen en ontsleutelen van het ondersteuningspakket.
4. De logboeken van de ontsleutelde ondersteuning-pakket hebben etw/etvx-indeling. U kunt de volgende stappen uit om deze bestanden in Windows Logboeken uitvoeren:
   
   1. Voer de **eventvwr** opdracht op uw Windows-client. Hiermee wordt logboeken gestart.
   2. In de **acties** deelvenster, klikt u op **opgeslagen logboek openen** en wijst u de logboekbestanden in etvx/etw-indeling (het ondersteuningspakket). U kunt nu het bestand bekijken. Nadat u het bestand opent, kunt u met de rechtermuisknop op en sla het bestand als tekst.
      
      > [!IMPORTANT]
      > U kunt ook de **Get-WinEvent** cmdlet bestand te openen in Windows PowerShell. Zie voor meer informatie [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) in de documentatie van Windows PowerShell-cmdlet-verwijzing.
     
5. Wanneer u de logboeken openen in Logboeken, zoekt u naar de volgende logboeken die problemen met betrekking tot de apparaatconfiguratie bevatten:
   
   * hcs_pfconfig/operationeel logboek
   * hcs_pfconfig/Config
6. In de logboekbestanden, zoeken naar tekenreeksen met betrekking tot de cmdlets aangeroepen door de wizard setup. Zie [eerst setup-wizard proces](#first-time-setup-wizard-process) voor een lijst van deze cmdlets.
7. Als u ze niet om de oorzaak van het probleem te achterhalen, kunt u [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor volgende stappen. Gebruik de stappen in [Maak een ondersteuningsaanvraag](storsimple-8000-contact-microsoft-support.md#create-a-support-request) wanneer u contact op met Microsoft Support voor hulp.

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdlets beschikbaar voor het oplossen van problemen
Gebruik de volgende Windows PowerShell-cmdlets voor het detecteren van connectiviteitsfouten.

* `Get-NetAdapter`: Gebruik deze cmdlet voor het detecteren van de status van netwerkinterfaces.
* `Test-Connection`: Gebruik deze cmdlet om te controleren van de netwerkverbinding binnen en buiten het netwerk.
* `Test-HcsmConnection`: Gebruik deze cmdlet om te controleren van de verbindingen van een apparaat is geregistreerd.
* `Sync-HcsTime`: Gebruik deze cmdlet voor het weergeven van de tijd op het apparaat en een tijdsynchronisatie met de NTP-server dwingen.
* `Enable-HcsPing`en `Disable-HcsPing`: deze cmdlets gebruiken om de hosts te pingen van de netwerkinterfaces op uw StorSimple-apparaat toestaan. Standaard reageren de StorSimple-netwerkinterfaces niet op ping-verzoeken.
* `Trace-HcsRoute`: Deze cmdlet gebruiken als een hulpmiddel voor het traceren van route. Deze pakketten verzendt naar de routers op weg naar de eindbestemming gedurende een periode en vervolgens wordt berekend op basis van de pakketten die zijn geretourneerd door elke hop resultaten. Aangezien `Trace-HcsRoute` ziet u de mate van pakketverlies op elke router en de koppeling, u kunt achterhalen welke routers of koppelingen mogelijk veroorzaakt door problemen met het netwerk.
* `Get-HcsRoutingTable`: Gebruik deze cmdlet om de IP-routeringstabel weer te geven.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Problemen oplossen met de cmdlet Get-NetAdapter
Wanneer u netwerkinterfaces voor de implementatie van een apparaat eerst configureert, is de status van de hardware niet beschikbaar in de service Manager voor StorSimple-apparaat UI omdat het apparaat nog niet is geregistreerd met de service. Bovendien de **Hardware health** blade mogelijk niet altijd goed overeen met de status van het apparaat, vooral als er problemen met synchronisatie van de service. In deze situaties kunt u de `Get-NetAdapter` cmdlet om te bepalen van de gezondheid en status van uw netwerkinterfaces.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Voor een overzicht van de netwerkadapters op uw apparaat
1. Start Windows PowerShell voor StorSimple en typ vervolgens `Get-NetAdapter`. 
2. Gebruik de uitvoer van de `Get-NetAdapter` cmdlet en de volgende richtlijnen om te begrijpen van de status van de netwerkinterface.
   
   * Als de interface in orde en is ingeschakeld is, de **ifIndex** status wordt weergegeven als **Up**.
   * Als de interface is in orde, maar niet fysiek zijn verbonden (via een netwerkkabel) de **ifIndex** wordt weergegeven als **uitgeschakelde**.
   * Als de interface in orde, maar niet ingeschakeld is, de **ifIndex** status wordt weergegeven als **NotPresent**.
   * Als de interface niet bestaat, wordt het niet weergegeven in deze lijst. De service Manager voor StorSimple-apparaat gebruikersinterface worden nog steeds voor deze interface in een foutstatus.

Ga voor meer informatie over het gebruik van deze cmdlet naar [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) in de documentatie van Windows PowerShell-cmdlet.

De volgende secties ziet u voorbeelden van uitvoer van de `Get-NetAdapter` cmdlet.

 In deze voorbeelden controller 0 is de passieve controller en is als volgt geconfigureerd:

* DATA 0, gegevens-1, DATA 2 en DATA 3 network interfaces beschikbaar op het apparaat waren.
* GEGEVENS 4 en 5 gegevens netwerkinterfacekaarten zijn niet aanwezig; ze zijn daarom niet vermeld in de uitvoer.
* DATA 0 is ingeschakeld.

Controller 1 is van de actieve controller en is als volgt geconfigureerd:

* DATA 0, gegevens-1, DATA 2, DATA 3, 4 van de gegevens en DATA 5 network interfaces beschikbaar op het apparaat waren.
* DATA 0 is ingeschakeld.

**Voorbeelduitvoer – controller 0**

Hieronder vindt de uitvoer van de controller 0 (passieve controller). GEGEVENS-1, DATA 2 en DATA 3 zijn niet verbonden. DATA 4 en 5 van de gegevens worden niet weergegeven omdat ze niet op het apparaat.

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Voorbeelduitvoer – controller 1**

Hieronder vindt de uitvoer van de controller 1 (de actieve controller). Alleen de DATA 0-netwerkinterface op het apparaat is geconfigureerd en een werkende.

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
U kunt de `Test-Connection` cmdlet om te bepalen of uw StorSimple-apparaat verbinding met het externe netwerk maken kan. Als alle netwerkparameters, met inbegrip van de DNS-server, correct zijn geconfigureerd in de wizard setup, kunt u de `Test-Connection` cmdlet te pingen van een bekende adres buiten het netwerk, zoals outlook.com.

Ping connectiviteit problemen oplossen met deze cmdlet als ping is uitgeschakeld, moet u inschakelen.

Zie de volgende voorbeelden van uitvoer van de `Test-Connection` cmdlet.

> [!NOTE]
> In het eerste voorbeeld, wordt het apparaat geconfigureerd met een onjuiste DNS-server. In het tweede voorbeeld is de DNS-server juist.

**Voorbeelduitvoer: onjuiste DNS**

Er wordt geen uitvoer voor de IPV4 en IPV6-adressen, waarmee wordt aangegeven dat de DNS-server kan niet worden omgezet in het volgende voorbeeld. Dit betekent dat er geen netwerkverbinding met het externe netwerk is en een juiste DNS-server moet worden opgegeven.

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Voorbeelduitvoer: de juiste DNS**

In het volgende voorbeeld retourneert de DNS-server de IPV4-adres, die aangeeft dat de DNS-server correct is geconfigureerd. Hiermee wordt bevestigd dat er verbinding met het externe netwerk.

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Problemen oplossen met de cmdlet Test-HcsmConnection
Gebruik de `Test-HcsmConnection` cmdlet voor een apparaat dat is al verbonden met en geregistreerd met uw StorSimple-apparaat Manager-service. Deze cmdlet kunt u controleren of de verbinding tussen een geregistreerd apparaat en de bijbehorende Apparaatbeheer StorSimple-service. U kunt deze opdracht uitvoeren op Windows PowerShell voor StorSimple.

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>De cmdlet Test-HcsmConnection uitvoeren
1. Zorg ervoor dat het apparaat is geregistreerd.
2. Controleer de status van het apparaat. Als het apparaat wordt gedeactiveerd, in de onderhoudsmodus of offline is, ziet u mogelijk een van de volgende fouten:
   
   * ErrorCode.CiSDeviceDecommissioned – dit geeft aan dat het apparaat wordt gedeactiveerd.
   * ErrorCode.DeviceNotReady – dit geeft aan dat het apparaat in de onderhoudsmodus.
   * ErrorCode.DeviceNotReady – dit geeft aan dat het apparaat niet online is.
3. Controleren of de service Manager voor StorSimple-apparaat wordt uitgevoerd (Gebruik de [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) cmdlet). Als de service niet wordt uitgevoerd, ziet u mogelijk de volgende fouten:
   
   * ErrorCode.CiSApplianceAgentNotOnline
   * ErrorCode.CisPowershellScriptHcsError – dit geeft aan dat er een uitzondering opgetreden is bij het uitvoeren van Get-ClusterResource.
4. Controleer het token Access Control Service (ACS). Als er een Webuitzondering genereert, is dit mogelijk het resultaat van een probleem gateway, een ontbrekende proxyverificatie, een onjuiste DNS-server of een verificatiefout opgetreden. U ziet de volgende fouten:
   
   * ErrorCode.CiSApplianceGateway – dit geeft een HttpStatusCode.BadGateway uitzondering: de naam resolver-service kan de hostnaam niet omzetten.
   * ErrorCode.CiSApplianceProxy – dit geeft een HttpStatusCode.ProxyAuthenticationRequired uitzondering (HTTP-statuscode 407): de client kan niet worden geverifieerd met de proxyserver.
   * ErrorCode.CiSApplianceDNSError – dit geeft een WebExceptionStatus.NameResolutionFailure uitzondering: de naam resolver-service kan de hostnaam niet omzetten.
   * ErrorCode.CiSApplianceACSError – dit geeft aan dat de service heeft een verificatiefout geretourneerd, maar er verbinding is.
     
     Als deze een Webuitzondering niet genereren is, controleert u de ErrorCode.CiSApplianceFailure. Hiermee wordt aangegeven dat het apparaat is mislukt.
5. Controleer de connectiviteit van de cloud-service. Als de service een Webuitzondering genereert, ziet u mogelijk de volgende fouten:
   
   * ErrorCode.CiSApplianceGateway – dit geeft een uitzondering HttpStatusCode.BadGateway: een tussenliggende proxy-server heeft een ongeldige aanvraag ontvangen van een andere proxy of van de oorspronkelijke server.
   * ErrorCode.CiSApplianceProxy – dit geeft een HttpStatusCode.ProxyAuthenticationRequired uitzondering (HTTP-statuscode 407): de client kan niet worden geverifieerd met de proxyserver.
   * ErrorCode.CiSApplianceDNSError – dit geeft een WebExceptionStatus.NameResolutionFailure uitzondering: de naam resolver-service kan de hostnaam niet omzetten.
   * ErrorCode.CiSApplianceACSError – dit geeft aan dat de service heeft een verificatiefout geretourneerd, maar er verbinding is.
     
     Als deze een Webuitzondering niet genereren is, controleert u de ErrorCode.CiSApplianceSaasServiceError. Dit duidt op een probleem met de service Manager voor StorSimple-apparaat.
6. Controleer de Azure Service Bus-verbindingen. ErrorCode.CiSApplianceServiceBusError geeft aan dat het apparaat kan geen verbinding met de Service Bus.

De logboekbestanden van de CiSCommandletLog0Curr.errlog en CiSAgentsvc0Curr.errlog heeft meer informatie, zoals uitzonderingsdetails.

Voor meer informatie over het gebruik van de cmdlet, gaat u naar [Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) verwijzen naar de documentatie in de Windows PowerShell.

> [!IMPORTANT]
> U kunt deze cmdlet voor zowel de actieve als de passieve controller uitvoeren.

Zie de volgende voorbeelden van uitvoer van de `Test-HcsmConnection` cmdlet.

**Voorbeeld van uitvoer – geregistreerd apparaat waarop StorSimple Update 3**

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

**Voorbeelduitvoer – offline apparaat** 

Dit voorbeeld is van een apparaat met de status van **Offline** in de Azure portal.

     Checking device registrationstate: Success
     Device is registered successfully
     Checking connectivity from device to SaaS.. Failure

Het apparaat kan geen verbinding maken met behulp van de huidige configuratie van uw webproxy. Dit kan een probleem met de webproxyconfiguratie of een probleem met de netwerkverbinding zijn. In dit geval moet u ervoor dat uw proxy-instellingen juist zijn en uw webproxyservers online en bereikbaar is zijn.

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Problemen oplossen met de cmdlet synchronisatie HcsTime
Gebruik deze cmdlet om de tijd op het apparaat weer te geven. Als de tijd op het apparaat een offset met de NTP-server heeft, kunt u deze cmdlet kunt vervolgens force-de tijd te synchroniseren met uw NTP-server gebruiken.
- Als de afstand tussen het apparaat en NTP-server groter dan 5 minuten is, ziet u een waarschuwing. 
- Als de offset is groter dan 15 minuten, wordt het apparaat offline gaat. U kunt deze cmdlet nog steeds gebruiken om af te dwingen een tijdsynchronisatie. 
- Echter, als de offset is groter dan 15 uur, vervolgens u zich niet force-synchroniseren met de tijd en een foutbericht wordt weergegeven.

**Voorbeeld van uitvoer – geforceerde tijdsynchronisatie met synchronisatie HcsTime**

     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.

     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Met de cmdlets Enable HcsPing en schakel HcsPing oplossen
Deze cmdlets gebruiken om ervoor te zorgen dat de netwerkinterfaces op het apparaat op ICMP-ping-aanvragen reageert. Standaard reageren de StorSimple-netwerkinterfaces niet op ping-verzoeken. Met deze cmdlet is de eenvoudigste manier om te weten of uw apparaat online en bereikbaar is is.

**Voorbeeld van uitvoer – Enable HcsPing en schakel HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Met de cmdlet Trace HcsRoute oplossen
Deze cmdlet gebruiken als een hulpmiddel voor het traceren van route. Deze pakketten verzendt naar de routers op weg naar de eindbestemming gedurende een periode en vervolgens wordt berekend op basis van de pakketten die zijn geretourneerd door elke hop resultaten. Omdat de cmdlet de mate van pakketverlies op elke gewenste router of koppeling toont, u kunt achterhalen welke routers of koppelingen mogelijk veroorzaakt door problemen met het netwerk.

**Voorbeeld van uitvoer waarin wordt getoond hoe u de route van een pakket met Trace HcsRoute traceren**

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
Gebruik deze cmdlet om de routeringstabel voor uw StorSimple-apparaat weer te geven. Een routeringstabel is een reeks regels om bepalen waar de gegevenspakketten die via een netwerk IP (Internet Protocol te) worden omgeleid.

De routeringstabel toont de interfaces en de gateway die voor de gegevens naar de opgegeven netwerken worden doorgestuurd. Dit biedt ook de route-metric die de besluitvormer voor het pad dat wordt gebruikt om een bepaald doel te bereiken. Hoe lager de route-metric, hoe hoger de voorkeur.

Bijvoorbeeld, als er 2 netwerkinterfaces, DATA 2 en DATA 3 verbonden met Internet. Als de routering metrische gegevens voor DATA 2 en DATA 3 15 en 261 respectievelijk, is DATA 2 met lagere routering metric de voorkeur interface gebruikt om te bereiken van het Internet.

Als u Update 1 op uw StorSimple-apparaat worden uitgevoerd, heeft uw DATA 0-netwerkinterface de hoogste voorkeur voor het verkeer van de cloud. Dit betekent dat zelfs als er andere interfaces ingeschakeld voor de cloud, het verkeer van de cloud zou worden gerouteerd via DATA 0.

Als u de `Get-HcsRoutingTable` cmdlet zonder parameters (zoals het volgende voorbeeld) de cmdlet wordt uitvoertabellen zowel IPv4 als IPv6-routering. U kunt ook opgeven `Get-HcsRoutingTable -IPv4` of `Get-HcsRoutingTable -IPv6` een relevante routeringstabel ophalen.

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
Het volgende voorbeeld ziet stapsgewijze probleemoplossing van een StorSimple-implementatie. In het voorbeeldscenario mislukt apparaatregistratie met een foutbericht dat de netwerkinstellingen of de DNS-naam onjuist is.

Het foutbericht wordt geretourneerd is:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

De fout kan worden veroorzaakt door het volgende:

* Onjuiste hardware-installatie
* Defecte netwerkinterfaces
* Onjuiste IP-adres, subnetmasker, gateway, primaire DNS-server of webproxy
* Onjuiste registratiesleutel
* Onjuiste firewall-instellingen

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Om te zoeken en los het probleem van de registratie van apparaat
1. Controleer de configuratie van uw apparaat: Voer op de actieve controller `Invoke-HcsSetupWizard`.
   
   > [!NOTE]
   > De wizard setup moet uitvoeren op de actieve controller. Om te bevestigen dat u met de actieve controller verbonden bent, bekijkt u de banner die zijn gepresenteerd in de seriële console. De banner geeft aan of u bent verbonden met de controller 0 of 1-controller, en of de domeincontroller actief of passief is. Ga voor meer informatie naar [een actieve domeincontroller op uw apparaat identificeren](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
   
2. Zorg ervoor dat het apparaat goed is bekabeld: Controleer de bekabeling op het apparaat terug luchtvervoer netwerk. De bekabeling is specifiek voor het model. Ga voor meer informatie naar [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
   
   > [!NOTE]
   > Als u van 10 GbE-netwerkpoorten gebruikmaakt, moet u de opgegeven QSFP SFP-adapters en SFP kabels gebruiken. Zie voor meer informatie de [lijst met kabels en switches infraroodzenders aanbevolen voor de 10 GbE-poorten](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
  
3. Controleer of de status van de netwerkinterface:
   
   * Gebruik de cmdlet Get-NetAdapter voor het detecteren van de status van de netwerkinterfaces voor DATA 0. 
   * Als de koppeling is niet werkt, de **ifindex** status wordt aangegeven dat de interface niet actief is. U moet vervolgens Controleer de netwerkverbinding van de poort voor het apparaat en de switch. U moet ook sprake is van slechte kabels. 
   * Als u vermoedt dat de DATA 0-poort op de actieve controller is mislukt, u dit controleren kunt door verbinding te maken met de DATA 0-poort op de controller 1. U kunt dit controleren de netwerk-kabel op de achterkant van het apparaat vanaf een controller 0, sluit de kabel en controller 1 en voer de cmdlet Get-NetAdapter opnieuw.
     Als de DATA 0-op een domeincontroller poort is mislukt, [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor volgende stappen. Mogelijk moet u de domeincontroller op uw systeem vervangen.
4. Controleer of de verbinding met de switch:
   
   * Zorg ervoor dat de DATA 0-netwerkinterfaces op de controller 0 en controller 1 in uw primaire behuizing in hetzelfde subnet zijn. 
   * Controleer de hub of een router. Normaal gesproken moet u beide domeincontrollers verbinding maken met de dezelfde hub of -router. 
   * Zorg ervoor dat de switches die u voor de verbinding gebruikt DATA 0 voor beide domeincontrollers in het hetzelfde vLAN hebben.
5. Gebruikersfouten voorkomen:
   
   * De wizard setup opnieuw uitvoeren (Voer **Invoke-HcsSetupWizard**), en voer de waarden opnieuw te controleren of er zijn geen fouten. 
   * Controleer of de registratie van de sleutel die wordt gebruikt. De dezelfde registratiesleutel kan worden gebruikt voor meerdere apparaten verbinding met een Apparaatbeheer StorSimple-service. Gebruik de procedure in [de serviceregistratiesleutel ophalen](storsimple-8000-manage-service.md#get-the-service-registration-key) om ervoor te zorgen dat u van de juiste registratiesleutel gebruikmaakt.
     
     > [!IMPORTANT]
     > Als er meerdere services die worden uitgevoerd, moet u om ervoor te zorgen dat de registratiecode voor de betreffende service wordt gebruikt voor het registreren van het apparaat. Als u een apparaat hebt geregistreerd met de verkeerde Apparaatbeheer StorSimple-service, moet u [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor volgende stappen. Wellicht moet worden uitgevoerd voor de fabrieksinstellingen van het apparaat (dit kan leiden tot verlies van gegevens) en vervolgens verbinden met de bedoelde service.
     > 
     > 
6. De cmdlet Test-Connection gebruiken om te controleren of u verbinding met het externe netwerk hebt. Ga voor meer informatie naar [problemen met de cmdlet Test-Connection](#troubleshoot-with-the-test-connection-cmdlet).
7. Controleer of de firewall storing. Als u hebt gecontroleerd of de virtuele IP-adres (VIP), subnet, gateway en DNS-instellingen juist, zijn en verbindingsproblemen nog steeds wordt weergegeven, dan is het mogelijk dat communicatie tussen het externe netwerk en het apparaat is geblokkeerd door uw firewall. U moet ervoor zorgen dat de poorten 80 en 443 op uw StorSimple-apparaat voor uitgaande communicatie beschikbaar zijn. Zie voor meer informatie [netwerkvereisten voor uw StorSimple-apparaat](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).
8. Bekijk de logboeken. Ga naar [ondersteuning voor pakketten en apparaatlogboeken beschikbaar voor het oplossen van](#support-packages-and-device-logs-available-for-troubleshooting).
9. Als de voorgaande stappen het probleem niet is opgelost [contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) voor hulp.

## <a name="next-steps"></a>Volgende stappen
[Informatie over het gebruik van het diagnostisch hulpprogramma voor het oplossen van een StorSimple-apparaat](storsimple-8000-diagnostics.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
