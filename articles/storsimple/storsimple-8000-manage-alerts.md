---
title: Weergeven en beheren van waarschuwingen voor StorSimple 8000-apparaat | Microsoft Docs
description: Beschrijving van waarschuwing StorSimple-voorwaarden en ernst, meldingen van waarschuwingen configureren en het gebruik van de StorSimple Device Manager-service voor het beheren van waarschuwingen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: 4128ad53f30e74fb72f6256b78587cc222f4c282
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57838711"
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-storsimple-alerts"></a>De StorSimple Device Manager-service gebruiken voor het StorSimple-waarschuwingen weergeven en beheren

## <a name="overview"></a>Overzicht

De **waarschuwingen** blade in de StorSimple Device Manager-service biedt een manier om te controleren en het StorSimple-apparaat-meldingen op basis van realtime wissen. Vanaf deze blade kunt u de statusproblemen van uw StorSimple-apparaten en de algemene Microsoft Azure StorSimple-oplossing centraal te bewaken.

Deze zelfstudie beschrijft algemene voorwaarden van de waarschuwing, ernstniveau van waarschuwingen en meldingen van waarschuwingen configureren. Daarnaast bevat deze waarschuwing Naslaggids tabellen, zodat ze snel vinden van een specifieke waarschuwing en reageren op de juiste wijze.

![Pagina met waarschuwingen](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="common-alert-conditions"></a>Algemene voorwaarden van de waarschuwing

Uw StorSimple-apparaat genereert waarschuwingen in reactie op tal van voorwaarden. Hier volgen de meest voorkomende typen waarschuwingen voorwaarden:

* **Hardwareproblemen** – deze meldingen laten u weten over de status van uw hardware. Ze laten u weten als firmware-upgrades nodig zijn, als een netwerkinterface heeft problemen of als er een probleem met een van uw schijven.
* **Problemen met de netwerkverbinding** – deze waarschuwingen gegenereerd wanneer er problemen bij het overbrengen van gegevens. Communicatieproblemen kunnen optreden tijdens de overdracht van gegevens naar en vanuit de Azure storage-account of vanwege een gebrek aan connectiviteit tussen de apparaten en de StorSimple Device Manager-service. Communicatieproblemen zijn enkele van de moeilijkst om op te lossen omdat er zoveel Point of failure. U moet altijd eerst controleren of verbinding met het netwerk en toegang tot Internet beschikbaar zijn voordat u doorgaat u aan bij de meer geavanceerde probleemoplossing. Voor hulp bij het oplossen van problemen, gaat u naar [problemen oplossen met de cmdlet Test-Connection](storsimple-8000-troubleshoot-deployment.md).
* **Prestatieproblemen** – deze waarschuwingen zijn veroorzaakt doordat het systeem is niet optimaal, zoals wanneer het zich onder een zware belasting.

Bovendien ziet u mogelijk waarschuwingen met betrekking tot beveiliging, updates of mislukte.

## <a name="alert-severity-levels"></a>Ernstniveau van waarschuwingen

Waarschuwingen hebben verschillende niveaus, afhankelijk van de impact die de waarschuwing situatie heeft en de noodzaak van een reactie op de waarschuwing. De niveaus zijn:

* **Kritieke** : deze waarschuwing wordt in reactie op een voorwaarde die invloed heeft op de geslaagde prestaties van uw systeem. Actie vereist om ervoor te zorgen dat de service niet wordt onderbroken StorSimple.
* **Waarschuwing** – dit probleem kritiek kan worden als niet is opgelost. U moet de situatie onderzoeken en elke actie die vereist is om te wissen van het probleem.
* **Informatie** : deze waarschuwing bevat informatie die nuttig zijn kan bij het bijhouden en beheren van uw systeem.

## <a name="configure-alert-settings"></a>Instellingen voor waarschuwingen configureren

U kunt kiezen of u wilt een melding per e-mail van de waarschuwing voorwaarden voor elk van uw StorSimple-apparaten. Bovendien kunt u andere ontvangers in waarschuwingsmeldingen identificeren door te voeren van hun e-mailadressen in de **andere e-mailontvangers** vak, gescheiden door puntkomma's.

> [!NOTE]
> U kunt maximaal 20 e-mailadressen per apparaat invoeren.

Nadat u e-mailmelding voor een apparaat hebt ingeschakeld, wordt een e-mailbericht telkens wanneer een kritieke waarschuwing wordt gegenereerd door leden van de meldingenlijst ontvangen. De berichten worden verzonden vanuit *storsimple-waarschuwingen-noreply\@mail.windowsazure.com* en vindt u een voorwaarde voor de waarschuwing. Ontvangers kunnen klikken op **Afmelden** om zelf te verwijderen uit de lijst met e-melding.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Aan het e-mailmeldingen van waarschuwingen voor een apparaat inschakelen
1. Ga naar uw StorSimple-apparaatbeheerservice. Selecteer in de lijst met apparaten en klikt u op het apparaat dat u wilt configureren.
2. Ga naar **instellingen** > **algemene** voor het apparaat.

   ![Waarschuwingen-blade](./media/storsimple-8000-manage-alerts/configure-alerts-email2.png)
   
2. In de **algemene instellingen** blade, Ga naar **waarschuwingsinstellingen** en stel het volgende:
   
   1. In de **e-mailmelding verzenden** veld **Ja**.
   2. In de **e-servicebeheerders** veld **Ja** naar de servicebeheerder en alle medebeheerders de waarschuwingsmeldingen moeten ontvangen.
   3. In de **andere e-mailontvangers** en voer de e-mailadressen van alle andere ontvangers die de waarschuwingsmeldingen moeten ontvangen. Geef de namen in de indeling *iemand\@somewhere.com*. Gebruik puntkomma te scheiden van de e-mailadressen. U kunt maximaal 20 e-mailadressen per apparaat configureren. 
      
3. Als u wilt een test-e-mailmelding verzenden, klikt u op **testbericht verzenden**. De StorSimple Device Manager-service wordt statusberichten weergegeven als de Testmelding wordt doorgestuurd.

    ![Waarschuwingsinstellingen](./media/storsimple-8000-manage-alerts/configure-alerts-email3.png)

4. U ziet een melding wanneer de test-e-mailbericht is verzonden. 
   
    ![Waarschuwingen test-e-mailmelding verzonden](./media/storsimple-8000-manage-alerts/configure-alerts-email4.png)
   
   > [!NOTE]
   > Als het meldingsbericht test kan niet worden verzonden, wordt de service StorSimple Device Manager een bijpassend foutbericht weergegeven. Wacht een paar minuten en probeer het vervolgens opnieuw uw test-melding verzenden. 

5. Nadat u de configuratie hebt voltooid, klikt u op **opslaan**. Klik op **Ja** als u om bevestiging wordt gevraagd.

     ![Waarschuwingen test-e-mailmelding verzonden](./media/storsimple-8000-manage-alerts/configure-alerts-email5.png)

## <a name="view-and-track-alerts"></a>Waarschuwingen weergeven en bijhouden

De overzichtsblade van de StorSimple Device Manager-service biedt u een snelle weergave op het aantal waarschuwingen op uw apparaten, gerangschikt op ernstniveau.

![Waarschuwingen-dashboard](./media/storsimple-8000-manage-alerts/device-summary4.png)

Hiermee opent u de ernst op te klikken op de **waarschuwingen** blade. De resultaten bevatten alleen de waarschuwingen die overeenkomen met die ernstniveau.

Te klikken op een waarschuwing in de lijst geeft u aanvullende informatie voor de waarschuwing, de laatste keer dat de waarschuwing is gemeld, waaronder het aantal exemplaren van de waarschuwing op het apparaat en de aanbevolen actie de waarschuwing te verhelpen. Als dit een waarschuwing voor hardware is, identificeert deze ook de hardware-onderdeel.

![Voorbeeld van de hardware-waarschuwing](./media/storsimple-8000-manage-alerts/configure-alerts-email14.png)

Als u de gegevens te verzenden naar Microsoft Support nodig hebt, kunt u de details van de waarschuwing kopiëren naar een tekstbestand. Nadat u de aanbeveling wordt gevolgd en de waarschuwingsvoorwaarde on-premises opgelost, moet u de waarschuwing van het apparaat wissen door het selecteren van de waarschuwing in de **waarschuwingen** blade en te klikken op **wissen**. Schakel meerdere waarschuwingen, elke waarschuwing selecteert, klikt u op een kolom met uitzondering van de **waarschuwing** kolom en klik vervolgens op **wissen** nadat u hebt geselecteerd de waarschuwingen worden gewist. Houd er rekening mee dat sommige waarschuwingen automatisch gewist wanneer het probleem opgelost is of wanneer het systeem de waarschuwing met nieuwe gegevens bijgewerkt.

Wanneer u klikt op **wissen**, hebt u de mogelijkheid om opmerkingen over de waarschuwing en de stappen die u hebt genomen om het probleem te verhelpen. Sommige gebeurtenissen worden verwijderd door het systeem als een andere gebeurtenis wordt geactiveerd met nieuwe gegevens. In dat geval ziet u het volgende bericht weergegeven.

![Waarschuwing wissen](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Waarschuwingen voor sorteren en controleren

Wellicht vindt u het efficiënter uitvoeren van rapporten op waarschuwingen, zodat u kunt controleren en deze in groepen wissen. Bovendien de **waarschuwingen** blade kan maximaal 250 waarschuwingen worden weergegeven. Als u dit aantal waarschuwingen hebt overschreden, wordt niet alle waarschuwingen worden weergegeven in de standaardweergave. De volgende velden om aan te passen welke waarschuwingen worden weergegeven, kunt u combineren:

* **Status** – u kunt een weergeven **Active** of **uitgeschakeld** waarschuwingen. Actieve waarschuwingen worden nog steeds op uw systeem wordt geactiveerd terwijl uitgeschakeld waarschuwingen zijn handmatig verwijderd door een beheerder of via een programma is uitgeschakeld omdat het systeem voorwaarde voor de waarschuwing met nieuwe informatie bijgewerkt.
* **Ernst** – u kunt waarschuwingen van alle niveaus (kritiek, waarschuwing, informatie), of alleen een bepaalde prioriteit, zoals alleen kritieke waarschuwingen weergeven.
* **Bron** – u kunt waarschuwingen weergeven vanaf alle bronnen of beperken van de waarschuwingen die afkomstig van de service of één of alle apparaten zijn.
* **Tijdsbereik** – door op te geven de **van** en **naar** datums en tijdstempels, u kunt kijken waarschuwingen gedurende de periode waarin u geïnteresseerd bent.

![Lijst met waarschuwingen](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="alerts-quick-reference"></a>Snelzoekgids voor waarschuwingen

De volgende tabellen worden enkele van de Microsoft Azure StorSimple-waarschuwingen die u tegenkomen kunt, evenals aanvullende informatie en aanbevelingen indien beschikbaar. StorSimple-apparaatwaarschuwingen kunnen worden onderverdeeld in een van de volgende categorieën:

* [Connectiviteitswaarschuwingen cloud](#cloud-connectivity-alerts)
* [Cluster-waarschuwingen](#cluster-alerts)
* [Disaster recovery waarschuwingen](#disaster-recovery-alerts)
* [Hardware-waarschuwingen](#hardware-alerts)
* [Waarschuwingen voor taak](#job-failure-alerts)
* [Lokaal vastgemaakt volume waarschuwingen](#locally-pinned-volume-alerts)
* [Waarschuwingen voor netwerken](#networking-alerts)
* [Waarschuwingen over toepassingsprestaties](#performance-alerts)
* [Beveiligingswaarschuwingen](#security-alerts)
* [Ondersteuning voor pakket-waarschuwingen](#support-package-alerts)

### <a name="cloud-connectivity-alerts"></a>Connectiviteitswaarschuwingen cloud

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Verbinding met de <*cloud referentienaam*> kan niet tot stand worden gebracht. |Kan geen verbinding maken met de storage-account. |Het lijkt erop dat er mogelijk een connectiviteitsprobleem met uw apparaat. Voer de `Test-HcsmConnection` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat om te bepalen en corrigeer het probleem. Als de instellingen juist zijn, wordt het probleem mogelijk met de referenties van het opslagaccount waarvoor de waarschuwing is gegeven. In dit geval gebruiken de `Test-HcsStorageAccountCredential` cmdlet om te bepalen of er zijn problemen die u kunt oplossen.<ul><li>Controleer uw netwerkinstellingen.</li><li>Controleer de referenties van uw storage-account.</li></ul> |
| Er geen heartbeat ontvangen van uw apparaat voor de afgelopen <*getal*> minuten. |Kan geen verbinding maken met apparaat. |Het lijkt erop dat er is een connectiviteitsprobleem met uw apparaat. Gebruik de `Test-HcsmConnection` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat om te identificeren en oplossen van het probleem of neem contact op met uw netwerkbeheerder. |

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>StorSimple-gedrag wanneer cloudconnectiviteit is mislukt

Wat gebeurt er als cloudconnectiviteit is mislukt voor de StorSimple-apparaat in productie?

Als cloud-connectiviteit op uw StorSimple-apparaat voor productie is mislukt, klikt u vervolgens afhankelijk van de status van uw apparaat, kan het volgende gebeuren:

* **Voor de lokale gegevens op uw apparaat**: Enige tijd zal er geen onderbreking en leesbewerkingen blijven worden geleverd. Als het aantal openstaande IO's verhoogt en een limiet overschrijdt, kunnen echter de leesbewerkingen worden niet gestart.

    Afhankelijk van de hoeveelheid gegevens op uw apparaat blijft de schrijfbewerkingen ook worden uitgevoerd voor de eerste paar uur na de onderbreking in de cloud-connectiviteit. De schrijfbewerkingen wordt vervolgens vertragen en uiteindelijk starten mislukt als de cloudconnectiviteit is onderbroken voor enkele uren. (Er is tijdelijke opslagruimte op het apparaat voor gegevens die moet worden gepusht naar de cloud. Dit gebied wordt leeggemaakt wanneer de gegevens wordt verzonden. Als de verbinding is mislukt, gegevens in dit opslaggebied worden niet gepusht naar de cloud, en i/o mislukken.)
* **Voor de gegevens in de cloud**: Voor de meeste fouten in de basisnetwerkverbinding cloud, is een fout geretourneerd. Zodra de verbinding is hersteld, worden de IOs hervat zonder dat de gebruiker op het volume online brengen. In zeldzame gevallen mogelijk tussenkomst van de gebruiker moet het volume online brengen van de Azure-portal.
* **Voor cloudmomentopnamen Bezig**: De bewerking wordt opnieuw geprobeerd een paar keer binnen de 4 en 5 uur en als de verbinding is niet hersteld, de cloudmomentopnamen mislukken.

### <a name="cluster-alerts"></a>Cluster-waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Apparaat een failover naar <*apparaatnaam*>. |Apparaat is in de onderhoudsmodus bevindt. |Apparaat een failover vanwege activeren of afsluiten van de onderhoudsmodus. Dit is normaal en geen actie te ondernemen. Nadat u deze waarschuwing hebt bevestigd, wist u deze van de pagina met waarschuwingen. |
| Apparaat een failover naar <*apparaatnaam*>. |Apparaatfirmware of software is zojuist hebt bijgewerkt. |Er is een clusterfailover vanwege een update. Dit is normaal en geen actie te ondernemen. Nadat u deze waarschuwing hebt bevestigd, wist u deze van de pagina met waarschuwingen. |
| Apparaat een failover naar <*apparaatnaam*>. |Controller heeft afgesloten of opnieuw opgestart. |Apparaat een failover omdat de actieve controller heeft afgesloten of opnieuw door een beheerder opgestart. U hoeft geen actie te ondernemen. Nadat u deze waarschuwing hebt bevestigd, wist u deze van de pagina met waarschuwingen. |
| Apparaat een failover naar <*apparaatnaam*>. |Geplande failover. |Controleer of dat dit een geplande failover. Nadat u de juiste actie hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen. |
| Apparaat een failover naar <*apparaatnaam*>. |Niet-geplande failover. |StorSimple is gebouwd voor niet-geplande failovers automatisch worden hersteld. Als u een groot aantal van deze waarschuwingen ziet, moet u contact op met Microsoft Support. |
| Apparaat een failover naar <*apparaatnaam*>. |Andere/onbekende oorzaak. |Als u een groot aantal van deze waarschuwingen ziet, moet u contact op met Microsoft Support. Nadat het probleem opgelost is, kunt u deze waarschuwing op de pagina met waarschuwingen wissen. |
| Een service essentiële apparaat gerapporteerd als mislukt. |Gegevenspad-fout-service. |Neem voor hulp contact op met Microsoft Support. |
| Virtueel IP-adres voor de netwerkinterface <*gegevens #*> status wordt gerapporteerd als mislukt. |Andere/onbekende oorzaak. |Soms tijdelijke omstandigheden kunnen leiden tot deze waarschuwingen. Als dit het geval is, wordt klikt u vervolgens deze waarschuwing automatisch gewist na enige tijd. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |
| Virtueel IP-adres voor de netwerkinterface <*gegevens #*> status wordt gerapporteerd als mislukt. |De naam van interface: <*gegevens #*> IP-adres <IP address> niet online worden gebracht omdat er een dubbel IP-adres is gedetecteerd op het netwerk. |Zorg ervoor dat het dubbele IP-adres is verwijderd uit het netwerk of de interface met een ander IP-adres configureren. |

### <a name="disaster-recovery-alerts"></a>Disaster recovery waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Herstelbewerkingen kunnen niet alle instellingen voor deze service herstellen. Apparaat-configuratiegegevens zijn in een inconsistente status voor bepaalde apparaten. |Inconsistenties in de gegevens gedetecteerd nadat het herstel na noodgevallen. |Versleutelde gegevens op de service is niet gesynchroniseerd met die op het apparaat. Toestaan dat het apparaat <*apparaatnaam*> van StorSimple Device Manager het synchronisatieproces te starten. De Windows PowerShell-Interface voor StorSimple gebruiken om uit te voeren de `Restore-HcsmEncryptedServiceData` op apparaat <*apparaatnaam*> cmdlet, biedt het oude wachtwoord als invoer voor deze cmdlet om te herstellen van het beveiligingsprofiel. Voer vervolgens de `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet voor het bijwerken van de versleutelingssleutel voor servicegegevens. Nadat u de juiste actie hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen. |

### <a name="hardware-alerts"></a>Hardware-waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Hardware-onderdeel <*onderdeel-ID*> gerapporteerd als <*status*>. | |Soms tijdelijke omstandigheden kunnen leiden tot deze waarschuwingen. Als dit het geval is, wordt deze waarschuwing automatisch gewist na enige tijd. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |
| Passieve controller functioneert niet goed. |De passieve controller (secundair) werkt niet. |Uw apparaat is operationeel, maar een van uw controllers functioneert. Probeer die controller opnieuw te starten. Als het probleem niet is opgelost, neem dan contact op met Microsoft Support. |

### <a name="job-failure-alerts"></a>Waarschuwingen voor taak

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Back-up van <*volume groeps-ID van bron*> is mislukt. |Back-uptaak is mislukt. |Problemen met de netwerkverbinding kunnen worden voorkomen dat de back-upbewerking is voltooid. Als er geen connectiviteitsproblemen zijn, is het mogelijk dat u het maximum aantal back-ups hebt bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer het opnieuw. Nadat u de juiste actie hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen. |
| Klonen van <*back-element-id's van bron*> op <*bestemming volume serienummers*> is mislukt. |Kloontaak is mislukt. |Vernieuw de back-lijst om te bevestigen dat de back-up nog geldig. Als de back-up geldig is, is het mogelijk dat problemen met de netwerkverbinding van de cloud voorkomen dat de kloonbewerking is voltooid. Als er geen connectiviteitsproblemen zijn, is het mogelijk dat u de opslaglimiet hebt bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer het opnieuw. Nadat u de passende maatregelen om het probleem te verhelpen hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen. |
| Herstellen van <*back-element-id's van bron*> is mislukt. |Terugzetten is mislukt. |Vernieuw de back-lijst om te bevestigen dat de back-up nog geldig. Als de back-up geldig is, is het mogelijk dat problemen met de netwerkverbinding van de cloud voorkomen dat de herstelbewerking is voltooid. Als er geen connectiviteitsproblemen zijn, is het mogelijk dat u de opslaglimiet hebt bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer het opnieuw. Nadat u de passende maatregelen om het probleem te verhelpen hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen. |

### <a name="locally-pinned-volume-alerts"></a>Lokaal vastgemaakt volume waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Het maken van het lokale volume <*volumenaam*> is mislukt. |De taak voor het maken van volume is mislukt. <*Foutbericht overeenkomt met de mislukte foutcode*>. |Problemen met de netwerkverbinding kunnen worden voorkomen dat de bewerking voor het maken van ruimte is voltooid. Lokaal vastgemaakte volumes worden compact ingericht en het proces voor het maken van ruimte omvat gelekt van gelaagde volumes naar de cloud. Als er geen connectiviteitsproblemen zijn, verbruikt u mogelijk de lokale ruimte op het apparaat. Bepalen of er ruimte op het apparaat bestaat voordat u deze bewerking uitvoert. |
| Uitbreiding van het lokale volume <*volumenaam*> is mislukt. |De taak van de wijziging volume is mislukt vanwege <*foutbericht overeenkomt met de mislukte foutcode*>. |Problemen met de netwerkverbinding kunnen worden voorkomen dat de volume-uitbreiding bewerking te voltooien. Lokaal vastgemaakte volumes worden compact ingericht en het proces voor het uitbreiden van de bestaande ruimte omvat gelekt van gelaagde volumes naar de cloud. Als er geen connectiviteitsproblemen zijn, verbruikt u mogelijk de lokale ruimte op het apparaat. Bepalen of er ruimte op het apparaat bestaat voordat u deze bewerking uitvoert. |
| Conversie van volume <*volumenaam*> is mislukt. |De taak volume te converteren van het volumetype van lokaal vastgemaakt aan een gelaagd is mislukt. |Conversie van het volume van het type lokaal vastgemaakt aan lagen kan niet worden voltooid. Zorg ervoor dat er geen problemen met de netwerkverbinding te voorkomen dat de bewerking niet wordt voltooid. Voor het oplossen van verbindingsproblemen problemen gaat u naar [problemen met de cmdlet Test-HcsmConnection oplossen](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>De oorspronkelijke lokaal vastgemaakt volume is nu gemarkeerd als een gelaagd volume omdat sommige van de gegevens van de lokaal vastgemaakt volume heeft zich verspreid in de cloud tijdens de conversie. Het resulterende gelaagde volume bezet nog steeds lokale ruimte op het apparaat dat niet kan worden vrijgemaakt voor toekomstige lokale volumes.<br>Los eventuele connectiviteitsproblemen, wist u de waarschuwing en dit volume converteren naar het oorspronkelijke type lokaal vastgemaakt volume om ervoor te zorgen dat alle de gegevens beschikbaar worden gesteld lokaal opnieuw. |
| Conversie van volume <*volumenaam*> is mislukt. |De taak moet worden geconverteerd van het volumetype van een gelaagd volume naar een lokaal vastgemaakt is mislukt. |Conversie van het volume van het type doorverbonden naar lokaal vastgemaakt kan niet worden voltooid. Zorg ervoor dat er geen problemen met de netwerkverbinding te voorkomen dat de bewerking niet wordt voltooid. Voor het oplossen van verbindingsproblemen problemen gaat u naar [problemen met de cmdlet Test-HcsmConnection oplossen](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Het oorspronkelijke gelaagde volume worden nu gemarkeerd als een lokaal vastgemaakt volume als onderdeel van het conversieproces heeft nog steeds gegevens die zich bevinden in de cloud, terwijl de compact ingerichte ruimte op het apparaat voor dit volume niet meer kan worden vrijgemaakt voor toekomstige lokale volumes.<br>Los eventuele connectiviteitsproblemen, wist u de waarschuwing en dit volume converteren naar het oorspronkelijke type gelaagde volume om te controleren of de lokale ruimte compact ingericht op het apparaat kan worden vrijgemaakt. |
| Ruimteverbruik voor lokale momentopnamen van <*volume groepsnaam*> |Lokale momentopnamen voor de back-upbeleid binnenkort geen schijfruimte meer mogelijk uitgevoerd en om te voorkomen dat de host-schrijffouten ongeldig worden gemaakt. |Regelmatig lokale momentopnamen samen met een hoog gegevensverloop in de volumes die zijn gekoppeld aan deze groep back-upbeleid veroorzaken lokale ruimte op het apparaat snel wordt verbruikt. Verwijder eventuele lokale momentopnamen die niet meer nodig zijn. Ook werken uw lokale momentopname schema's voor deze back-upbeleid minder frequente lokale momentopnamen en zorg ervoor dat cloudmomentopnamen regelmatig worden genomen. Als deze acties worden niet uitgevoerd, lokale ruimte voor deze momentopnamen binnenkort worden uitgeput en het systeem wordt automatisch verwijderen om ervoor te zorgen dat de host schrijfbewerkingen blijven worden verwerkt. |
| Lokale momentopnamen voor <*volume groepsnaam*> zijn ongeldig gemaakt. |De lokale momentopnamen voor <*volume groepsnaam*> zijn ongeldig gemaakt en vervolgens verwijderd, omdat ze zijn meer dan de lokale ruimte op het apparaat. |Om ervoor te zorgen dat dit niet herhaald in de toekomst, Controleer de planningen voor lokale momentopname voor deze back-upbeleid en verwijder eventuele lokale momentopnamen die niet meer nodig zijn. Regelmatig lokale momentopnamen samen met een hoog gegevensverloop in de volumes die zijn gekoppeld aan deze groep back-upbeleid kunnen leiden tot lokale ruimte op het apparaat snel wordt verbruikt. |
| Herstellen van <*back-element-id's van bron*> is mislukt. |De hersteltaak is mislukt. |Als u lokaal hebt vastgemaakt of een combinatie van lokaal vastgemaakte als gelaagde volumes in deze back-upbeleid het back-lijst om te controleren of vernieuwen wordt de back-up nog geldig is. Als de back-up geldig is, is het mogelijk dat problemen met de netwerkverbinding van de cloud voorkomen dat de herstelbewerking is voltooid. De lokaal vastgemaakte volumes die zijn als onderdeel van deze groep momentopname wordt hersteld niet beschikt over alle gegevens gedownload naar het apparaat en hebt u een combinatie van gelaagde en lokaal vastgemaakte volumes in deze groep momentopname, ze zich niet synchroon met elkaar. Als u wilt de herstelbewerking is voltooid, zet de volumes in deze groep offline op de host en voer de herstelbewerking opnieuw uit. Houd er rekening mee dat alle wijzigingen in de volumegegevens die zijn uitgevoerd tijdens het herstelproces, gaan verloren. |

### <a name="networking-alerts"></a>Waarschuwingen voor netwerken

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Kan de StorSimple-service(s) niet starten. |Gegevenspad-fout |Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |
| Dubbel IP-adres gedetecteerd voor 'Data0'. | |Er is een conflict voor het IP-adres '10.0.0.1' is gedetecteerd. De netwerkbron 'Data0' op het apparaat *<device1>* offline is. Zorg ervoor dat dit IP-adres niet wordt gebruikt door een andere entiteit in dit netwerk. Voor het oplossen van netwerkproblemen, gaat u naar [problemen met de cmdlet Get-NetAdapter oplossen](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Neem contact op met de netwerkbeheerder voor meer informatie over het oplossen van dit probleem. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |
| -(IPv4 of IPv6)-adres voor 'Data0' is offline. | |De netwerk-resource 'Data0' met de IP-adres '10.0.0.1.' en een voorvoegsellengte '22' op het apparaat *<device1>* offline is. Zorg ervoor dat de switchpoorten waarmee deze interface is verbonden, operationeel zijn. Voor het oplossen van netwerkproblemen, gaat u naar [problemen met de cmdlet Get-NetAdapter oplossen](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
| Kan geen verbinding maken met de service voor verificatie. |Gegevenspad-fout |De URLthat wordt gebruikt voor verificatie is niet bereikbaar. Zorg ervoor dat uw firewall-regels de URL-patronen die is opgegeven voor het StorSimple-apparaat bevatten. Voor meer informatie over URL-patronen in Azure portal, gaat u naar https://aka.ms/ss-8000-network-reqs. Als u Azure Government Cloud gebruikt, gaat u naar de URL-patronen in https://aka.ms/ss8000-gov-network-reqs.|

### <a name="performance-alerts"></a>Waarschuwingen over toepassingsprestaties

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties | |
|:--- |:--- |:--- | --- |
| Belasting van het apparaat heeft overschreden <*drempelwaarde*>. |Langzamer dan verwacht reactietijden. |Uw apparaat rapporteert een zware belasting voor de invoer/uitvoer. Dit kan ertoe leiden dat niet werkt, evenals het moet het apparaat. Bekijk de werkbelastingen die u hebt gekoppeld aan het apparaat en bepalen of er zijn die kan worden verplaatst naar een ander apparaat of die niet meer nodig zijn.|
| Kan de StorSimple-service(s) niet starten. |Gegevenspad-fout |Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |

### <a name="security-alerts"></a>Beveiligingswaarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Microsoft Support-sessie is gestart. |Externe toegang tot support-sessie. |Bevestig dat de toegang geautoriseerd. Nadat u de juiste actie hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen. |
| Wachtwoord voor <*element*> verloopt <*tijdsduur*>. |Wachtwoord verloopt bijna is bereikt. |Wijzig uw wachtwoord voordat dit verloopt. |
| Informatie over de beveiligingsconfiguratie ontbreekt voor <*element-ID*>. | |De volumes die zijn gekoppeld aan deze volumecontainer kunnen niet worden gebruikt om uw StorSimple-configuratie te repliceren. Om ervoor te zorgen dat uw gegevens veilig worden opgeslagen, is het raadzaam dat u verwijdert de volumecontainer en alle volumes die zijn gekoppeld aan de volumecontainer. Nadat u de juiste actie hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen. |
| <*aantal*> mislukte aanmeldingspogingen voor <*element-ID*>. |Meerdere aanmeldingspogingen mislukte. |Uw apparaat wordt mogelijk aangevallen of een gemachtigde gebruiker gebruikt een onjuist wachtoord om verbinding te maken.<ul><li>Neem contact op met uw gemachtigde gebruikers en controleer of deze pogingen zijn van een betrouwbare bron. Als u een groot aantal mislukte aanmeldingspogingen blijft, kunt u extern beheer uitschakelen en contact opnemen met uw netwerkbeheerder. Nadat u de juiste actie hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen.</li><li>Controleer of uw instanties van Snapshot Manager zijn geconfigureerd met het juiste wachtwoord. Nadat u de juiste actie hebt ondernomen, moet u deze waarschuwing op de pagina met waarschuwingen wissen.</li></ul>Ga voor meer informatie naar [wijzigen van het wachtwoord van een verlopen apparaat](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password). |
| Een of meer fouten opgetreden bij het wijzigen van de versleutelingssleutel voor servicegegevens. | |Er zijn fouten opgetreden tijdens het wijzigen van de versleutelingssleutel voor servicegegevens. Nadat u de fouten zijn opgelost, voert u de `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat bij te werken van de service. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. Nadat u het probleem hebt opgelost, kunt u deze waarschuwing op de pagina met waarschuwingen wissen. |

### <a name="support-package-alerts"></a>Ondersteuning voor pakket-waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Het ondersteuningspakket niet maken. |StorSimple kan het pakket kan niet genereren. |Probeer het opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. Nadat u het probleem hebt opgelost, kunt u deze waarschuwing op de pagina met waarschuwingen wissen. |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple fouten en het oplossen van problemen met implementatie van het apparaat](storsimple-8000-troubleshoot-deployment.md).

