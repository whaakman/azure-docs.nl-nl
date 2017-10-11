---
title: Microsoft Azure StorSimple virtuele matrix waarschuwingen bekijken en beheren | Microsoft Docs
description: Beschrijft virtuele StorSimple-matrix waarschuwingsvoorwaarden en ernst en het gebruik van de StorSimple Manager-service om waarschuwingen te beheren.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 97ee25a1-0ec3-4883-9a0a-54b722598462
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3062c7c060c3730c5edef3ada16f3a5077cf5558
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-storsimple-device-manager-to-manage-alerts-for-the-storsimple-virtual-array"></a>Het StorSimple-Apparaatbeheer gebruiken voor het beheren van waarschuwingen voor de virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

De waarschuwingsfunctie in de service Manager voor StorSimple-apparaat biedt een manier om te controleren en schakel waarschuwingen over de real-time op basis van een virtueel StorSimple-matrices. U kunt de waarschuwingen op de **Serviceoverzicht** blade voor het bewaken van de statusproblemen van uw virtuele StorSimple-matrices en de algehele Microsoft Azure StorSimple-oplossing.

Deze zelfstudie wordt beschreven hoe waarschuwingsmeldingen, algemene voorwaarden voor waarschuwing, ernstniveau van waarschuwingen configureren, weergeven en waarschuwingen bijhouden. Daarnaast bevat deze waarschuwing Naslaggids tabellen, waarmee u snel een specifieke waarschuwing vinden en reageren op de juiste wijze.

![pagina waarschuwingen](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Instellingen voor waarschuwingen configureren

U kunt kiezen of u wilt ontvangen op basis van e-mailadres van de waarschuwing voorwaarden voor elk van uw virtuele StorSimple-matrices. Bovendien kunt u andere ontvangers waarschuwingsmeldingen identificeren door te voeren hun e-mailadressen in de **extra e-mailontvangers** vak, gescheiden door puntkomma's.

> [!NOTE]
> U kunt maximaal 20 e-mailadressen per virtuele matrix.


Nadat u een e-mailmelding voor een virtuele-matrix ingeschakeld, ontvangt de leden van de meldingenlijst een e-mailbericht telkens wanneer een kritieke waarschuwing optreedt. De berichten worden verzonden vanuit  *storsimple-alerts-noreply@mail.windowsazure.com*  en vindt u een voorwaarde voor de waarschuwing. Ontvangers kunnen klikken op **Unsubscribe** om zelf te verwijderen uit de lijst met e-melding.

#### <a name="to-enable-email-notification-for-alerts"></a>E-mailmeldingen voor waarschuwingen inschakelen

1. Ga op uw StorSimple-apparaat Manager-service en in de **Management** sectie, selecteert en op **apparaten**. Selecteer in de lijst met apparaten weergegeven, en klikt u op uw apparaat.
   
    ![instellingen voor waarschuwingen](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Hiermee opent u de **instellingen** blade. In de **apparaatinstellingen** sectie **algemene**. Hiermee opent u de **algemene instellingen** blade.
   
    ![configuratie voor meldingen van waarschuwingen](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. In de **algemene instellingen** blade, gaat u naar **instellingen voor waarschuwingen** sectie en stel het volgende:
   
   1. In de **Activeer e-mailmelding** optie **Ja**.
   2. In de **e-servicebeheerders** optie **Ja** als u wilt laten de servicebeheerder en medebeheerders alle van de meldingen waarschuwingsmeldingen ontvangen.
   3. In de **extra e-mailontvangers** en voer de e-mailadressen van alle andere ontvangers die de meldingen van waarschuwingen moeten ontvangen. Geef de namen in de indeling  *someone@somewhere.com* . Gebruik een puntkomma te scheiden van de e-mailadressen. U kunt maximaal 20 e-mailadressen per virtuele apparaat configureren.
      
       ![configuratie voor meldingen van waarschuwingen](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. Als u wilt een test-e-mailmelding verzenden, klikt u op **testbericht verzenden**. De service Manager voor StorSimple-apparaat wordt statusberichten weergegeven als de Testmelding wordt doorgestuurd.
      
       ![Waarschuwingen testen verzonden e-mailmelding](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Als de melding van de test kan niet worden verzonden, wordt de service Manager voor StorSimple-apparaat een toepasselijk bericht weergegeven. Klik op **OK**wacht een paar minuten en probeer het vervolgens opnieuw uw melding testbericht verzenden.
      > 
      > 
   5. Klik onder aan de pagina op **opslaan** aan uw configuratie op te slaan. Klik op **Ja** als u om bevestiging wordt gevraagd.
      
      ![Waarschuwingen testen verzonden e-mailmelding](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Algemene waarschuwingsvoorwaarden

Uw virtuele StorSimple-matrix genereert waarschuwingen als reactie op een aantal voorwaarden. Hier volgen de meest voorkomende waarschuwingen voorwaarden:

* **Problemen met de netwerkverbinding** – deze waarschuwingen zich voordoen als er problemen bij het overbrengen van gegevens. Communicatieproblemen kunnen optreden tijdens de overdracht van gegevens naar en vanuit de Azure storage-account of vanwege een gebrek aan connectiviteit tussen virtuele apparaten en de service Manager voor StorSimple-apparaat. Communicatieproblemen zijn enkele van de moeilijkst om op te lossen omdat er zoveel foutpunten. U moet altijd eerst controleren of verbinding met het netwerk en toegang tot Internet beschikbaar zijn alvorens u aan bij de meer geavanceerde probleemoplossing. Ga voor informatie over de poorten en firewall-instellingen naar [virtuele StorSimple-matrix systeemvereisten](storsimple-ova-system-requirements.md). Voor hulp bij het oplossen van problemen, gaat u naar [problemen met de cmdlet Test-Connection](storsimple-troubleshoot-deployment.md).
* **Prestatieproblemen** – deze waarschuwingen worden veroorzaakt wanneer uw systeem is niet optimaal, zoals wanneer het zich onder een zware belasting.

Bovendien ziet u mogelijk waarschuwingen die betrekking hebben op beveiliging, updates of taakfouten.

## <a name="alert-severity-levels"></a>Ernstniveau van waarschuwingen

Waarschuwingen hebben verschillende niveaus, afhankelijk van het effect dat de waarschuwing situatie heeft en de noodzaak van een antwoord op de waarschuwing. De ernstniveaus zijn:

* **Kritieke** – deze waarschuwing is in reactie op een voorwaarde die van invloed op de geslaagde prestaties van uw systeem. Actie is vereist om ervoor te zorgen dat de StorSimple niet wordt onderbroken.
* **Waarschuwing** : dit probleem kritiek kan worden als niet omgezet. U moet de situatie onderzoeken en elke actie vereist om te wissen van het probleem.
* **Informatie** – deze waarschuwing bevat informatie die nuttig zijn kan bij het beheren van uw systeem en bij te houden.

## <a name="view-and-track-alerts"></a>Waarschuwingen weergeven en bijhouden

De samenvatting blade voor Apparaatbeheer van StorSimple-service biedt u een snelle weergave op het aantal waarschuwingen op uw virtuele apparaten, gerangschikt op ernst.

![Waarschuwingen-dashboard](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Klik op het ernstniveau openen de **waarschuwingen** blade. De resultaten bevatten alleen de waarschuwingen die overeenkomen met die ernst.

![Rapport waarschuwingen binnen het bereik van Waarschuwingstype](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Klik op een waarschuwing in de lijst voor aanvullende informatie voor de waarschuwing, met inbegrip van de laatste keer dat de waarschuwing is gemeld, het aantal exemplaren van de waarschuwing op het apparaat en de aanbevolen actie voor het omzetten van de waarschuwing.

![Lijst met waarschuwingen en details](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Als u de gegevens te verzenden naar Microsoft Support nodig hebt, kunt u de details van de waarschuwing kopiëren naar een tekstbestand. Nadat u hebt de aanbeveling gevolgd en de meldingsvoorwaarde on-premises opgelost, moet u de waarschuwing in de lijst wissen. Selecteer de waarschuwing in de lijst en klik vervolgens op **wissen**. Schakel meerdere waarschuwingen, elke waarschuwing selecteert, klikt u op een van de kolommen, behalve de **waarschuwing** kolom en klik vervolgens op **wissen** nadat u hebt geselecteerd de waarschuwingen worden gewist.

Wanneer u klikt op **wissen**, hebt u de mogelijkheid opmerkingen over de waarschuwing en de stappen die u hebt ondernomen om het probleem te verhelpen. 

![Waarschuwing opmerkingen](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Sommige gebeurtenissen worden gewist door het systeem als een andere gebeurtenis wordt geactiveerd met nieuwe informatie. 

## <a name="sort-and-review-alerts"></a>Sorteren en bekijk waarschuwingen

De **waarschuwingen** blade maximaal 250 waarschuwingen kunt weergeven. Als u dat aantal waarschuwingen hebben overschreden, wordt niet alle waarschuwingen weergegeven in de standaardweergave. U kunt de volgende velden om aan te passen welke waarschuwingen worden weergegeven combineren:

* **Status** – u kunt ofwel weergeven **Active** of **uitgeschakeld** waarschuwingen. Actieve waarschuwingen zijn nog steeds op het systeem geactiveerd terwijl gewiste waarschuwingen zijn handmatig worden gewist door een beheerder of programmatisch uitgeschakeld omdat het systeem voorwaarde voor de waarschuwing met nieuwe informatie bijgewerkt.
* **Ernst** – u kunt waarschuwingen van alle niveaus (kritiek, waarschuwing, informatie) of alleen een bepaalde ernst, zoals alleen kritieke waarschuwingen weergeven.
* **Bron** – u kunt waarschuwingen van alle bronnen weergegeven of beperken van de waarschuwingen die afkomstig van de service of een of alle virtuele apparaten zijn.
* **Tijdsbereik** – door te geven de **van** en **naar** datums en tijdstempels, kunt u bekijken waarschuwingen gedurende de periode waarin u geïnteresseerd bent.

## <a name="alerts-quick-reference"></a>Naslaggids voor waarschuwingen

De volgende tabellen worden enkele van de StorSimple-waarschuwingen die u tegenkomen kunt, evenals aanvullende informatie en aanbevelingen indien beschikbaar. Virtuele StorSimple-matrix waarschuwingen kunnen worden onderverdeeld in een van de volgende categorieën:

* [Connectiviteitswaarschuwingen cloud](#cloud-connectivity-alerts)
* [Configuratiewaarschuwingen](#configuration-alerts)
* [Waarschuwingen met betrekking tot taak](#job-failure-alerts)
* [Waarschuwingen over toepassingsprestaties](#performance-alerts)
* [Beveiligingswaarschuwingen](#security-alerts)
* [Waarschuwingen bijwerken](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Connectiviteitswaarschuwingen cloud

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Apparaat  *<device name>*  niet is verbonden met de cloud. |Het apparaat met de naam kan geen verbinding met de cloud. |Kan geen verbinding maken met de cloud. Dit kan worden veroorzaakt door een van de volgende:<ul><li>Er is mogelijk een probleem met de instellingen op uw apparaat.</li><li>Er is mogelijk een probleem met de referenties van de storage-account.</li></ul>Ga voor meer informatie over het oplossen van problemen met de netwerkverbinding naar de [lokale webgebruikersinterface](storsimple-ova-web-ui-admin.md) van het apparaat. |

### <a name="configuration-alerts"></a>Configuratiewaarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Een on-premises virtuele apparaatconfiguratie niet ondersteund. |Trage prestaties. |De huidige configuratie kan leiden tot verminderde prestaties. Zorg ervoor dat uw server voldoet aan de minimale configuratievereisten voor. Ga voor meer informatie naar [StorSimple-vereisten voor virtuele matrix](storsimple-ova-system-requirements.md). |
| U worden uitgevoerd buiten de ingerichte schijfruimte beschikbaar op <*apparaatnaam*>. |Waarschuwing voor schijf-ruimte. |U weinig schijfruimte ingericht. Overweeg om ruimte vrij, het verplaatsen van werkbelastingen naar een ander volume of share of het verwijderen van gegevens. |

### <a name="job-failure-alerts"></a>Waarschuwingen met betrekking tot taak

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Back-up van <*apparaatnaam*> kan niet worden voltooid. |Back-uptaak is mislukt. |Kan een back-up niet maken. Houd rekening met het volgende:<ul><li>Verbindingsproblemen kunnen worden voorkomen dat de back-upbewerking is voltooid. Zorg ervoor dat er geen verbindingsproblemen zijn. Ga voor meer informatie over het oplossen van problemen met de netwerkverbinding naar de [lokale webgebruikersinterface](storsimple-ova-web-ui-admin.md) voor uw virtuele apparaat.</li><li>U kunt de beschikbare opslagruimte limiet hebt bereikt. Overweeg om ruimte vrij, het verwijderen van een back-ups die niet langer nodig zijn.</li></ul> Los de problemen op, schakel de waarschuwing en probeer het opnieuw. |
| Klonen van <*apparaatnaam*> kan niet worden voltooid. |Kloon de taak is mislukt. |Kan een kloon niet maken. Houd rekening met het volgende:<ul><li>Uw back-lijst is mogelijk niet geldig. Vernieuw de lijst om te controleren of dat het nog geldig is.</li><li>Verbindingsproblemen kunnen worden voorkomen dat de kopieerbewerking is voltooid. Zorg ervoor dat er geen verbindingsproblemen zijn.</li><li>U kunt de beschikbare opslagruimte limiet hebt bereikt. Overweeg om ruimte vrij, het verwijderen van een back-ups die niet langer nodig zijn.</li></ul>Los de problemen op, schakel de waarschuwing en probeer het opnieuw. |

### <a name="performance-alerts"></a>Waarschuwingen over toepassingsprestaties

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Er onverwachte vertragingen bij de overdracht van gegevens. |Trage gegevensoverdracht. |Bandbreedtebeperking fouten optreden wanneer u de schaalbaarheidsdoelen van een opslagservice overschrijdt. De opslagservice doet dit om ervoor te zorgen dat geen enkele client of de tenant de service ten koste van anderen kunt gebruiken. Voor meer informatie over het oplossen van uw Azure storage-account, gaat u naar [Monitor, vaststellen en oplossen van Microsoft Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md). |
| Er bijna geen schijfruimte lokale reservering op <*apparaatnaam*>. |Langzame reactietijd. |10% van de grootte van de totale ingerichte voor <*apparaatnaam*> is gereserveerd op de lokale apparaat en u nu weinig gereserveerde ruimte. De werkbelasting op <*apparaatnaam*> genereert een hogere snelheid van het verloop of u mogelijk hebt onlangs gemigreerd een grote hoeveelheid gegevens. Hierdoor kunnen de prestaties verminderen. Houd rekening met een van de volgende acties oplossing:<ul><li>De bandbreedte van de cloud om dit apparaat te vergroten.</li><li>Verminder of werkbelastingen verplaatsen naar een ander volume of share.</li></ul> |

### <a name="security-alerts"></a>Beveiligingswaarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Wachtwoord voor <*apparaatnaam*> verloopt over <*getal*> dagen. |Waarschuwing: wachtwoord. |Uw wachtwoord verloopt < nummer < dagen. U kunt uw wachtwoord te wijzigen. Ga voor meer informatie naar [wijzigen van het beheerderswachtwoord van het virtuele StorSimple-matrix apparaat](storsimple-virtual-array-change-device-admin-password.md). |

### <a name="update-alerts"></a>Waarschuwingen bijwerken

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Er zijn nieuwe updates beschikbaar zijn voor uw apparaat. |Updates voor de virtuele StorSimple-matrix zijn beschikbaar. |U kunt nieuwe updates van installeren de **onderhoud** pagina. |
| Kan niet gescand op nieuwe updates op <*apparaatnaam*>. |Bijwerken is mislukt. |Er is een fout opgetreden tijdens het installeren van nieuwe updates. U kunt de updates handmatig installeren. Als het probleem zich blijft voordoen, neem dan contact op met [Microsoft Support](storsimple-contact-microsoft-support.md). |

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de virtuele StorSimple-matrix](storsimple-ova-overview.md).

