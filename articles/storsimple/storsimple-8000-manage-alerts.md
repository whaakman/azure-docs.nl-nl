---
title: Waarschuwingen voor StorSimple 8000 series apparaat bekijken en beheren | Microsoft Docs
description: Beschrijving van waarschuwing StorSimple-voorwaarden en ernst, meldingen van waarschuwingen configureren en het gebruik van de service Manager voor StorSimple-apparaat om waarschuwingen te beheren.
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
ms.workload: NA
ms.date: 08/22/2017
ms.author: alkohli
ms.openlocfilehash: b7f9a2b7eb3dbf4cc97fac9a410359e068e67eb1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-storsimple-alerts"></a>De service Manager voor StorSimple-apparaat gebruiken voor StorSimple waarschuwingen bekijken en beheren

## <a name="overview"></a>Overzicht

De **waarschuwingen** blade in de service Manager voor StorSimple-apparaat biedt een manier om te controleren en het StorSimple-apparaat – meldingen op basis van realtime wissen. In deze blade kunt u de statusproblemen van uw StorSimple-apparaten en de algehele oplossing voor Microsoft Azure StorSimple centraal controleren.

Deze zelfstudie wordt beschreven algemene waarschuwing voorwaarden, ernstniveau van waarschuwingen en meldingen van waarschuwingen configureren. Daarnaast bevat deze waarschuwing Naslaggids tabellen, waarmee u snel een specifieke waarschuwing vinden en reageren op de juiste wijze.

![pagina waarschuwingen](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="common-alert-conditions"></a>Algemene waarschuwingsvoorwaarden

Uw StorSimple-apparaat genereert waarschuwingen als reactie op een aantal voorwaarden. Hier volgen de meest voorkomende waarschuwingen voorwaarden:

* **Hardwareproblemen** – deze meldingen geven over de status van uw hardware. Ze laten u weten als firmware-upgrades nodig zijn, als een netwerkinterface heeft problemen of als er een probleem met een van uw schijven.
* **Problemen met de netwerkverbinding** – deze waarschuwingen zich voordoen als er problemen bij het overbrengen van gegevens. Communicatieproblemen kunnen optreden tijdens de overdracht van gegevens naar en vanuit de Azure storage-account of vanwege een gebrek aan connectiviteit tussen de apparaten en de service Manager voor StorSimple-apparaat. Communicatieproblemen zijn enkele van de moeilijkst om op te lossen omdat er zoveel foutpunten. U moet altijd eerst controleren of verbinding met het netwerk en toegang tot Internet beschikbaar zijn alvorens u aan bij de meer geavanceerde probleemoplossing. Voor hulp bij het oplossen van problemen, gaat u naar [problemen met de cmdlet Test-Connection](storsimple-troubleshoot-deployment.md).
* **Prestatieproblemen** – deze waarschuwingen worden veroorzaakt wanneer uw systeem is niet optimaal, zoals wanneer het zich onder een zware belasting.

Bovendien ziet u mogelijk waarschuwingen die betrekking hebben op beveiliging, updates of taakfouten.

## <a name="alert-severity-levels"></a>Ernstniveau van waarschuwingen

Waarschuwingen hebben verschillende niveaus, afhankelijk van het effect dat de waarschuwing situatie heeft en de noodzaak van een antwoord op de waarschuwing. De ernstniveaus zijn:

* **Kritieke** – deze waarschuwing is in reactie op een voorwaarde die van invloed op de geslaagde prestaties van uw systeem. Actie is vereist om ervoor te zorgen dat de StorSimple niet wordt onderbroken.
* **Waarschuwing** : dit probleem kritiek kan worden als niet omgezet. U moet de situatie onderzoeken en elke actie vereist om te wissen van het probleem.
* **Informatie** – deze waarschuwing bevat informatie die nuttig zijn kan bij het beheren van uw systeem en bij te houden.

## <a name="configure-alert-settings"></a>Instellingen voor waarschuwingen configureren

U kunt kiezen of u wilt ontvangen op basis van e-mailadres van de waarschuwing voorwaarden voor elk van uw StorSimple-apparaten. Bovendien kunt u andere ontvangers waarschuwingsmeldingen identificeren door te voeren hun e-mailadressen in de **andere e-mailontvangers** vak, gescheiden door puntkomma's.

> [!NOTE]
> U kunt maximaal 20 e-mailadressen per apparaat invoeren.

Nadat u e-mailmelding voor een apparaat hebt ingeschakeld, ontvangt de leden van de meldingenlijst een e-mailbericht telkens wanneer een kritieke waarschuwing optreedt. De berichten worden verzonden vanuit  *storsimple-alerts-noreply@mail.windowsazure.com*  en vindt u een voorwaarde voor de waarschuwing. Ontvangers kunnen klikken op **Unsubscribe** om zelf te verwijderen uit de lijst met e-melding.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>E-mailmeldingen van waarschuwingen voor een apparaat inschakelen
1. Ga naar uw StorSimple-apparaatbeheerservice. Selecteer in de lijst met apparaten en klikt u op het apparaat dat u wilt configureren.
2. Ga naar **instellingen** > **algemene** voor het apparaat.

   ![Waarschuwingen-blade](./media/storsimple-8000-manage-alerts/configure-alerts-email2.png)
   
2. In de **algemene instellingen** blade, gaat u naar **instellingen voor waarschuwingen** en stel het volgende:
   
   1. In de **e-mailmelding verzenden** optie **Ja**.
   2. In de **e-servicebeheerders** optie **Ja** naar de servicebeheerder en medebeheerders alle meldingen van waarschuwingen te ontvangen.
   3. In de **andere e-mailontvangers** en voer de e-mailadressen van alle andere ontvangers die de meldingen van waarschuwingen moeten ontvangen. Geef de namen in de indeling  *someone@somewhere.com* . Gebruik een puntkomma te scheiden van de e-mailadressen. U kunt maximaal 20 e-mailadressen per apparaat configureren. 
      
3. Als u wilt een test-e-mailmelding verzenden, klikt u op **testbericht verzenden**. De service Manager voor StorSimple-apparaat wordt statusberichten weergegeven als de Testmelding wordt doorgestuurd.

    ![instellingen voor waarschuwingen](./media/storsimple-8000-manage-alerts/configure-alerts-email3.png)

4. U ziet een melding wanneer de test-e-mailbericht is verzonden. 
   
    ![Waarschuwingen testen verzonden e-mailmelding](./media/storsimple-8000-manage-alerts/configure-alerts-email4.png)
   
   > [!NOTE]
   > Als de melding van de test kan niet worden verzonden, verschijnt de StorSimple-apparaat Manager-service een foutbericht weergegeven. Wacht een paar minuten en probeer het vervolgens opnieuw uw melding testbericht verzenden. 

5. Nadat u de configuratie hebt voltooid, klikt u op **opslaan**. Klik op **Ja** als u om bevestiging wordt gevraagd.

     ![Waarschuwingen testen verzonden e-mailmelding](./media/storsimple-8000-manage-alerts/configure-alerts-email5.png)

## <a name="view-and-track-alerts"></a>Waarschuwingen weergeven en bijhouden

De samenvatting blade voor Apparaatbeheer van StorSimple-service biedt u een snelle weergave op het aantal waarschuwingen op uw apparaten, gerangschikt op ernst.

![Waarschuwingen-dashboard](./media/storsimple-8000-manage-alerts/device-summary4.png)

Hiermee opent u de ernst op te klikken op de **waarschuwingen** blade. De resultaten bevatten alleen de waarschuwingen die overeenkomen met die ernst.

Te klikken op een waarschuwing in de lijst geeft u aanvullende informatie voor de waarschuwing, met inbegrip van de laatste keer dat de waarschuwing is gemeld, het aantal exemplaren van de waarschuwing op het apparaat en de aanbevolen actie voor het omzetten van de waarschuwing. Als het een hardware-waarschuwing, wordt deze ook de hardware-onderdeel te identificeren.

![Voorbeeld van de hardware-waarschuwing](./media/storsimple-8000-manage-alerts/configure-alerts-email14.png)

Als u de gegevens te verzenden naar Microsoft Support nodig hebt, kunt u de details van de waarschuwing kopiëren naar een tekstbestand. Nadat u hebt de aanbeveling gevolgd en de meldingsvoorwaarde on-premises opgelost, moet u de waarschuwing van het apparaat wissen door het selecteren van de waarschuwing in de **waarschuwingen** blade en te klikken op **wissen**. Schakel meerdere waarschuwingen, elke waarschuwing selecteert, klikt u op een van de kolommen, behalve de **waarschuwing** kolom en klik vervolgens op **wissen** nadat u hebt geselecteerd de waarschuwingen worden gewist. Houd er rekening mee dat sommige waarschuwingen automatisch gewist wanneer het probleem opgelost is of wanneer de waarschuwing wordt bijgewerkt met nieuwe informatie.

Wanneer u klikt op **wissen**, hebt u de mogelijkheid opmerkingen over de waarschuwing en de stappen die u hebt ondernomen om het probleem te verhelpen. Sommige gebeurtenissen worden gewist door het systeem als een andere gebeurtenis wordt geactiveerd met nieuwe informatie. In dat geval ziet u het volgende bericht.

![Waarschuwingsbericht voor wissen](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Sorteren en bekijk waarschuwingen

Soms is het efficiënter uitvoeren van rapporten van waarschuwingen, zodat u kunt bekijken en deze in groepen wissen. Bovendien de **waarschuwingen** blade maximaal 250 waarschuwingen kunt weergeven. Als u dat aantal waarschuwingen hebben overschreden, wordt niet alle waarschuwingen weergegeven in de standaardweergave. U kunt de volgende velden om aan te passen welke waarschuwingen worden weergegeven combineren:

* **Status** – u kunt ofwel weergeven **Active** of **uitgeschakeld** waarschuwingen. Actieve waarschuwingen zijn nog steeds op het systeem geactiveerd terwijl gewiste waarschuwingen zijn handmatig worden gewist door een beheerder of programmatisch uitgeschakeld omdat het systeem voorwaarde voor de waarschuwing met nieuwe informatie bijgewerkt.
* **Ernst** – u kunt waarschuwingen van alle niveaus (kritiek, waarschuwing, informatie) of alleen een bepaalde ernst, zoals alleen kritieke waarschuwingen weergeven.
* **Bron** – u kunt waarschuwingen van alle bronnen weergegeven of beperken van de waarschuwingen die afkomstig van de service of één of alle apparaten zijn.
* **Tijdsbereik** – door te geven de **van** en **naar** datums en tijdstempels, kunt u bekijken waarschuwingen gedurende de periode waarin u geïnteresseerd bent.

![Lijst met waarschuwingen](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="alerts-quick-reference"></a>Naslaggids voor waarschuwingen

De volgende tabellen worden enkele van de Microsoft Azure StorSimple-waarschuwingen die u tegenkomen kunt, evenals aanvullende informatie en aanbevelingen indien beschikbaar. Waarschuwingen voor StorSimple-apparaat worden onderverdeeld in een van de volgende categorieën:

* [Connectiviteitswaarschuwingen cloud](#cloud-connectivity-alerts)
* [Cluster-waarschuwingen](#cluster-alerts)
* [Disaster recovery waarschuwingen](#disaster-recovery-alerts)
* [Hardware-waarschuwingen](#hardware-alerts)
* [Waarschuwingen met betrekking tot taak](#job-failure-alerts)
* [Lokaal vastgemaakt volume waarschuwingen](#locally-pinned-volume-alerts)
* [Waarschuwingen voor netwerken](#networking-alerts)
* [Waarschuwingen over toepassingsprestaties](#performance-alerts)
* [Beveiligingswaarschuwingen](#security-alerts)
* [Ondersteuning voor pakket-waarschuwingen](#support-package-alerts)

### <a name="cloud-connectivity-alerts"></a>Connectiviteitswaarschuwingen cloud

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| De verbinding met <*cloud referentienaam*> kan niet tot stand worden gebracht. |Kan geen verbinding met de storage-account. |Het lijkt erop dat er mogelijk een probleem met uw apparaat. Voer de `Test-HcsmConnection` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat om te bepalen en corrigeer het probleem. Als de instellingen correct zijn, mogelijk het probleem met de referenties van het opslagaccount waarvoor de waarschuwing is gegeven. In dit geval gebruiken de `Test-HcsStorageAccountCredential` cmdlet om te bepalen of er zijn problemen die u kunt oplossen.<ul><li>Controleer de netwerkinstellingen.</li><li>Controleer de referenties van het opslagaccount.</li></ul> |
| We hebben een heartbeat niet ontvangen van uw apparaat voor de afgelopen <*getal*> minuten. |Kan geen verbinding met apparaat. |Het lijkt erop dat er een verbindingsprobleem met uw apparaat is. Gebruik de `Test-HcsmConnection` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat om te identificeren en los het probleem of neem contact op met uw netwerkbeheerder. |

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>StorSimple gedrag wanneer de connectiviteit van de cloud is mislukt

Wat gebeurt er als cloud-connectiviteit is mislukt voor mijn StorSimple-apparaat in productie uitgevoerd?

Als de cloud-connectiviteit is mislukt op uw StorSimple-apparaat voor productie, vervolgens afhankelijk van de status van uw apparaat kan het volgende gebeuren:

* **Voor de lokale gegevens op uw apparaat**: enige tijd zal er geen wordt onderbroken en leesbewerkingen blijven worden geleverd. Als het aantal openstaande IOs verhoogt en een limiet overschrijdt, kunnen echter gelezen gegevens worden niet gestart.

    Afhankelijk van de hoeveelheid gegevens op uw apparaat blijft de schrijfbewerkingen ook om te worden uitgevoerd voor de eerste paar uur na de onderbreking in de cloud-connectiviteit. De schrijfbewerkingen wordt vervolgens vertragen en uiteindelijk start mislukken als de cloud-connectiviteit voor enkele uren wordt onderbroken. (Er tijdelijke opslag is op het apparaat voor gegevens die moet worden gepusht naar de cloud. Dit gebied wordt leeggemaakt wanneer de gegevens worden verzonden. Als de verbinding is mislukt, gegevens in deze opslagruimte wordt niet naar de cloud gepusht, en i/o zal mislukken.)
* **Voor de gegevens in de cloud**: voor de meeste fouten voor cloud-connectiviteit, wordt een fout geretourneerd. Zodra de verbinding is hersteld, worden de IOs zonder de gebruiker hoeft te doen om het volume online hervat. In zeldzame gevallen mogelijk tussenkomst van de gebruiker moet het volume online brengen van de Azure-portal.
* **Voor cloudmomentopnamen van de Bezig**: de bewerking is geprobeerd een paar keer binnen 4-5 uur en als de verbinding is niet hersteld, mislukt de cloudmomentopnamen van de.

### <a name="cluster-alerts"></a>Cluster-waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Kan apparaat niet via <*apparaatnaam*>. |Apparaat is in de onderhoudsmodus. |Apparaat is mislukt via vanwege invoeren of het afsluiten van de onderhoudsmodus. Dit is normaal en is geen actie vereist. Nadat u deze waarschuwing hebt bevestigd, schakelt u het op de pagina waarschuwingen. |
| Kan apparaat niet via <*apparaatnaam*>. |Apparaatfirmware of de software is zojuist hebt bijgewerkt. |Er is een clusterfailover als gevolg van een update. Dit is normaal en is geen actie vereist. Nadat u deze waarschuwing hebt bevestigd, schakelt u het op de pagina waarschuwingen. |
| Kan apparaat niet via <*apparaatnaam*>. |Controller is afgesloten of opnieuw gestart. |Apparaat is via mislukt, omdat de actieve controller is afgesloten of opnieuw worden gestart door een beheerder. Er is geen actie vereist. Nadat u deze waarschuwing hebt bevestigd, schakelt u het op de pagina waarschuwingen. |
| Kan apparaat niet via <*apparaatnaam*>. |Geplande failover. |Controleer of dit is een geplande failover. Nadat u de juiste actie hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen. |
| Kan apparaat niet via <*apparaatnaam*>. |Niet-geplande failover. |StorSimple is gebouwd voor het automatisch herstellen vanuit niet-geplande failovers. Als u een groot aantal deze waarschuwingen ziet, moet u contact op met Microsoft Support. |
| Kan apparaat niet via <*apparaatnaam*>. |Andere/onbekende oorzaak. |Als u een groot aantal deze waarschuwingen ziet, moet u contact op met Microsoft Support. Nadat het probleem opgelost is, schakelt u deze waarschuwing op de pagina waarschuwingen. |
| Een service essentiële apparaat gerapporteerd als mislukt. |Gegevenspad service is mislukt. |Neem voor hulp contact op met Microsoft Support. |
| Virtueel IP-adres voor de netwerkinterface <*gegevens #*> meldt de status mislukt. |Andere/onbekende oorzaak. |Soms tijdelijke voorwaarden kunnen deze waarschuwingen veroorzaken. Als dit het geval is, wordt klikt u vervolgens deze waarschuwing automatisch gewist na enige tijd. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |
| Virtueel IP-adres voor de netwerkinterface <*gegevens #*> meldt de status mislukt. |Interfacenaam: <*gegevens #*> IP-adres <IP address> niet online worden gebracht omdat er een dubbel IP-adres op het netwerk is gedetecteerd. |Zorg ervoor dat het dubbele IP-adres uit het netwerk is verwijderd of opnieuw configureren van de interface met een ander IP-adres. |

### <a name="disaster-recovery-alerts"></a>Disaster recovery waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Herstelbewerkingen kunnen niet alle instellingen voor deze service herstellen. Apparaat-configuratiegegevens zijn in een inconsistente status voor sommige apparaten. |Een gegevensinconsistentie gevonden na het herstel na noodgevallen. |Versleutelde gegevens op de service is niet gesynchroniseerd met die op het apparaat. Het apparaat toestaan <*apparaatnaam*> van StorSimple Apparaatbeheer te openen, start het synchronisatieproces. Gebruik de Windows PowerShell-Interface voor StorSimple om uit te voeren de `Restore-HcsmEncryptedServiceData` op apparaat <*apparaatnaam*> cmdlet, biedt het oude wachtwoord als invoer voor deze cmdlet het beveiligingsprofiel herstellen. Voer vervolgens de `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet bij te werken van de gegevensversleutelingssleutel van service. Nadat u de juiste actie hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen. |

### <a name="hardware-alerts"></a>Hardware-waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Hardware-onderdeel <*onderdeel-ID*> wordt gerapporteerd als <*status*>. | |Soms tijdelijke voorwaarden kunnen deze waarschuwingen veroorzaken. Zo ja, wordt deze waarschuwing automatisch gewist na enige tijd. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |
| Passieve controller defect. |De controller voor passieve (secundair) werkt niet. |Uw apparaat functioneert, maar een van uw domeincontrollers is defect. Probeer opnieuw te starten die controller. Als het probleem niet is opgelost, moet u contact op met Microsoft Support. |

### <a name="job-failure-alerts"></a>Waarschuwingen met betrekking tot taak

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Back-up van <*volume groeps-ID bron*> is mislukt. |Back-uptaak is mislukt. |Verbindingsproblemen kunnen worden voorkomen dat de back-upbewerking is voltooid. Als er geen verbindingsproblemen zijn, is het mogelijk dat u het maximum aantal back-ups hebt bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer het opnieuw. Nadat u de juiste actie hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen. |
| Klonen van <*bron van de back-element-id's*> op <*bestemming volume serienummers*> is mislukt. |Kloon-taak is mislukt. |Vernieuw de back-lijst om te bevestigen dat de back-up nog geldig. Als de back-up geldig is, is het mogelijk dat cloud-verbindingsproblemen zijn zo wordt voorkomen dat de kopieerbewerking is voltooid. Als er geen verbindingsproblemen zijn, hebt u mogelijk de opslaglimiet bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer het opnieuw. Nadat u de juiste actie om het probleem te verhelpen hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen. |
| Herstellen van <*bron van de back-element-id's*> is mislukt. |Herstel de taak is mislukt. |Vernieuw de back-lijst om te bevestigen dat de back-up nog geldig. Als de back-up geldig is, is het mogelijk dat cloud-verbindingsproblemen zo wordt voorkomen de herstelbewerking opnieuw dat zijn uit te voltooien. Als er geen verbindingsproblemen zijn, hebt u mogelijk de opslaglimiet bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer het opnieuw. Nadat u de juiste actie om het probleem te verhelpen hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen. |

### <a name="locally-pinned-volume-alerts"></a>Lokaal vastgemaakt volume waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Het maken van lokaal volume <*volumenaam*> is mislukt. |De taak voor het maken van volume is mislukt. <*Foutbericht overeenkomt met de foutcode voor mislukte*>. |Problemen met de netwerkverbinding mogelijk verhindert de bewerking voor het maken van ruimte met succes voltooid. Lokaal vastgemaakte volumes zijn compact ingericht en het proces van het maken van de ruimte omvat lopen van gelaagde volumes in de cloud. Als er geen verbindingsproblemen zijn, u mogelijk zijn de lokale ruimte op het apparaat. Bepalen of ruimte aanwezig is op het apparaat voordat u deze bewerking. |
| Uitbreiding van de lokale volume <*volumenaam*> is mislukt. |De wijziging volume taak is mislukt vanwege <*foutbericht overeenkomt met de foutcode voor mislukte*>. |Verbindingsproblemen kunnen worden voorkomen dat het volume-uitbreiding opnieuw uit te voltooien. Lokaal vastgemaakte volumes zijn compact ingericht en het proces voor het uitbreiden van de bestaande ruimte omvat lopen van gelaagde volumes in de cloud. Als er geen verbindingsproblemen zijn, u mogelijk zijn de lokale ruimte op het apparaat. Bepalen of ruimte aanwezig is op het apparaat voordat u deze bewerking. |
| Conversie van volume <*volumenaam*> is mislukt. |De conversietaak volume converteren van het volumetype van lokaal vastgemaakt aan de lagen is mislukt. |Conversie van het volume van het type lokaal vastgemaakt aan lagen kan niet worden voltooid. Zorg ervoor dat er geen verbindingsproblemen zijn zo wordt voorkomen dat de bewerking wordt voltooid. Voor het oplossen van verbindingsproblemen problemen gaat u naar [problemen met de cmdlet Test-HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>De oorspronkelijke lokaal vastgemaakt volume is nu gemarkeerd als een gelaagd volume omdat sommige van de gegevens van de lokaal vastgemaakt volume zich in de cloud is verspreid tijdens de conversie. Het resulterende gelaagde volume wordt nog steeds lokale ruimte op het apparaat dat niet kan worden vrijgemaakt voor toekomstige lokale volumes ingenomen.<br>Los eventuele problemen met de netwerkverbinding, schakelt u de waarschuwing en dit volume converteren naar het oorspronkelijke type lokaal vastgemaakt volume om te controleren of alle van de gegevens beschikbaar worden gesteld lokaal opnieuw. |
| Conversie van volume <*volumenaam*> is mislukt. |De conversietaak volume converteren van het volumetype van lagen naar een lokaal vastgemaakt is mislukt. |Conversie van het volume van het type lagen naar lokaal vastgemaakt kan niet worden voltooid. Zorg ervoor dat er geen verbindingsproblemen zijn zo wordt voorkomen dat de bewerking wordt voltooid. Voor het oplossen van verbindingsproblemen problemen gaat u naar [problemen met de cmdlet Test-HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Het oorspronkelijke gelaagde volume worden nu gemarkeerd als een lokaal vastgemaakt volume als onderdeel van het conversieproces heeft nog steeds gegevens die zich in de cloud, terwijl de compact ingerichte ruimte op het apparaat voor dit volume niet meer kan worden vrijgemaakt voor toekomstige lokale volumes.<br>Los eventuele problemen met de netwerkverbinding, schakelt u de waarschuwing en dit volume converteren naar het oorspronkelijke type gelaagd volume om ervoor te zorgen lokale ruimte compact ingericht op het apparaat kan worden vrijgemaakt. |
| Lokale ruimte verbruik voor lokale momentopnamen van bijna <*volume groepsnaam*> |Lokale momentopnamen voor de back-upbeleid snel mogelijk tekort aan ruimte en ongeldig worden gemaakt om host schrijven fouten te voorkomen. |Veelvuldig lokale momentopnamen samen met een hoge gegevensverloop in de volumes die zijn gekoppeld aan deze groep back-upbeleid veroorzaken lokale ruimte op het apparaat te snel worden verbruikt. Verwijder eventuele lokale momentopnamen die niet langer nodig zijn. Bovendien uw lokale momentopname schema's voor het back-upbeleid minder frequente lokale momentopnamen en ervoor zorgen dat cloud regelmatig momentopnamen worden bijwerken. Als deze acties niet genomen worden, lokale ruimte voor deze momentopnamen kan snel worden verbruikt en wordt automatisch verwijderd om te zorgen dat de host schrijfbewerkingen blijven worden verwerkt. |
| Lokale momentopnamen voor <*volume groepsnaam*> zijn ongeldig gemaakt. |De lokale momentopnamen voor <*volume groepsnaam*> zijn ongeldig gemaakt en vervolgens verwijderd, omdat ze zijn van meer dan de lokale ruimte op het apparaat. |Dit niet in de toekomst herhaald, zodat de lokale momentopname schema's voor het back-upbeleid bekijken en verwijder eventuele lokale momentopnamen die niet langer nodig zijn. Veelvuldig lokale momentopnamen samen met een hoge gegevensverloop in de volumes die zijn gekoppeld aan deze groep back-upbeleid mogelijk lokale ruimte op het apparaat te snel worden verbruikt. |
| Herstellen van <*bron van de back-element-id's*> is mislukt. |De hersteltaak is mislukt. |Als u lokaal hebt vastgemaakt of een combinatie van lokaal vastgemaakte en gelaagde volumes in het back-upbeleid, vernieuw de back-lijst om te controleren of is de back-up nog geldig. Als de back-up geldig is, is het mogelijk dat cloud-verbindingsproblemen zo wordt voorkomen de herstelbewerking opnieuw dat zijn uit te voltooien. De lokaal vastgemaakte volumes die zijn als onderdeel van deze groep momentopname wordt hersteld niet beschikt over alle bijbehorende gegevens die zijn gedownload naar het apparaat en hebt u een combinatie van gelaagde en lokaal vastgemaakte volumes in deze groep momentopname, ze niet worden gesynchroniseerd met elkaar. Om te kunnen voltooien de herstelbewerking, voert u de volumes in deze groep offline op de host en probeer de herstelbewerking opnieuw. Houd er rekening mee dat eventuele wijzigingen in de volumegegevens die zijn uitgevoerd tijdens het herstelproces verloren gaan. |

### <a name="networking-alerts"></a>Waarschuwingen voor netwerken

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Kan de StorSimple-service(s) niet starten. |Gegevenspad fout |Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |
| Dubbel IP-adres voor 'Data0' gedetecteerd. | |Het systeem heeft een conflict met IP-adres '10.0.0.1' gedetecteerd. De netwerkbron 'Data0' op het apparaat  *<device1>*  offline is. Zorg ervoor dat dit IP-adres niet wordt gebruikt door een andere entiteit in dit netwerk. Voor het oplossen van problemen met netwerken, gaat u naar [problemen met de cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Neem contact op met de netwerkbeheerder voor hulp bij het oplossen van dit probleem. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |
| -(IPv4 of IPv6)-adres voor 'Data0' is offline. | |De netwerkbron 'Data0' met IP-adres '10.0.0.1'. en voorvoegsellengte '22' op het apparaat  *<device1>*  offline is. Zorg ervoor dat de switchpoorten waarmee deze interface is verbonden operationeel. Voor het oplossen van problemen met netwerken, gaat u naar [problemen met de cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
| Kan geen verbinding met de verificatieservice. |Gegevenspad fout |De URLthat wordt gebruikt voor verificatie is niet bereikbaar. Zorg ervoor dat uw firewallregels de URL-patronen die is opgegeven voor de StorSimple-apparaat bevatten. Ga voor meer informatie over URL-patronen in Azure-portal naar https://aka.ms/ss-8000-network-reqs. Als u Azure Government Cloud gebruikt, gaat u naar de URL-patronen in https://aka.ms/ss8000-gov-network-reqs.|

### <a name="performance-alerts"></a>Waarschuwingen over toepassingsprestaties

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| De belasting van het apparaat is overschreden <*drempelwaarde*>. |Langzamer dan verwacht reactietijden. |Uw apparaat rapporteert gebruik onder een zware belasting van de invoer/uitvoer. Dit kan ertoe leiden dat uw apparaat om te werken niet goed nodig. Bekijk de werkbelastingen die u hebt gekoppeld aan het apparaat en bepalen of er zijn die kan worden verplaatst naar een ander apparaat of die niet langer nodig.| Kan de StorSimple-service(s) niet starten. |Gegevenspad fout |Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |en de huidige status, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken voor het bewaken van uw apparaat](storsimple-monitor-device.md) |

### <a name="security-alerts"></a>Beveiligingswaarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Microsoft Support-sessie is gestart. |Derde support-sessie geopend. |Controleer of dat deze toegang is toegestaan. Nadat u de juiste actie hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen. |
| Wachtwoord voor <*element*> verloopt over <*tijdsduur*>. |Wachtwoord verloopt bijna is bereikt. |Wijzig uw wachtwoord voordat dit verloopt. |
| Informatie over de beveiligingsconfiguratie ontbreekt voor <*element-ID*>. | |De volumes die zijn gekoppeld aan deze volumecontainer kunnen niet worden gebruikt om uw StorSimple-configuratie te repliceren. Om ervoor te zorgen dat uw gegevens veilig worden opgeslagen, is het raadzaam dat u de volumecontainer en de volumes die zijn gekoppeld aan de volumecontainer verwijderen. Nadat u de juiste actie hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen. |
| <*aantal*> aanmeldingspogingen is mislukt voor <*element-ID*>. |Meerdere aanmeldingspogingen mislukte. |Uw apparaat wordt mogelijk aangevallen of een gemachtigde gebruiker gebruikt een onjuist wachtoord om verbinding te maken.<ul><li>Neem contact op met uw geautoriseerde gebruikers en controleer of deze pogingen zijn van een betrouwbare bron. Als u doorgaat een groot aantal mislukte aanmeldingspogingen zien, kunt u extern beheer uitschakelen en contact opnemen met uw netwerkbeheerder. Nadat u de juiste actie hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen.</li><li>Controleer of uw Snapshot Manager-exemplaren zijn geconfigureerd met het juiste wachtwoord. Nadat u de juiste actie hebt uitgevoerd, wist u deze waarschuwing op de pagina waarschuwingen.</li></ul>Ga voor meer informatie naar [wijzigen van een verlopen apparaatwachtwoord](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password). |
| Een of meer fouten opgetreden tijdens het wijzigen van de gegevensversleutelingssleutel van service. | |Er zijn fouten opgetreden tijdens het wijzigen van de gegevensversleutelingssleutel van service. Nadat u de foutvoorwaarden hebt opgelost, voert u de `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat bij te werken van de service. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. Nadat u het probleem hebt opgelost, schakelt u deze waarschuwing op de pagina waarschuwingen. |

### <a name="support-package-alerts"></a>Ondersteuning voor pakket-waarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Maken van support-pakket is mislukt. |Het pakket kan niet worden gegenereerd door StorSimple. |Probeer het opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. Nadat u het probleem hebt opgelost, schakelt u deze waarschuwing op de pagina waarschuwingen. |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple fouten en probleemoplossing van een apparaat operationele](storsimple-troubleshoot-operational-device.md).

