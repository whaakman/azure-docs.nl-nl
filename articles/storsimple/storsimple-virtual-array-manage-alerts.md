---
title: Microsoft Azure StorSimple Virtual Array waarschuwingen weergeven en beheren | Microsoft Docs
description: Beschrijving van waarschuwing voorwaarden StorSimple Virtual Array en ernst en het gebruik van de StorSimple Manager-service voor het beheren van waarschuwingen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 97ee25a1-0ec3-4883-9a0a-54b722598462
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/12/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0cd3f506205a3ee4f4435b16fd2185eeb20aef3c
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447221"
---
# <a name="use-storsimple-device-manager-to-manage-alerts-for-the-storsimple-virtual-array"></a>Gebruik StorSimple Device Manager voor het beheren van waarschuwingen voor de StorSimple Virtual Array

## <a name="overview"></a>Overzicht

De waarschuwingsfunctie in de StorSimple Device Manager-service biedt een manier om te controleren en meldingen met betrekking tot virtuele StorSimple-matrices op basis van realtime wissen. U kunt de waarschuwingen op de **Serviceoverzicht** blade voor het bewaken van de problemen met het controleren van uw virtuele StorSimple-matrices en de algemene Microsoft Azure StorSimple-oplossing.

Deze zelfstudie wordt beschreven hoe u meldingen van waarschuwingen, algemene voorwaarden van de waarschuwing, ernstniveau van waarschuwingen configureert en hoe u waarschuwingen weergeven en bijhouden. Daarnaast bevat deze waarschuwing Naslaggids tabellen, zodat ze snel vinden van een specifieke waarschuwing en reageren op de juiste wijze.

![Pagina met waarschuwingen](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Instellingen voor waarschuwingen configureren

U kunt kiezen of u wilt een melding per e-mail van de waarschuwing voorwaarden voor elk van uw virtuele StorSimple-matrices. Bovendien kunt u andere ontvangers in waarschuwingsmeldingen identificeren door te voeren van hun e-mailadressen in de **aanvullende e-mailontvangers** vak, gescheiden door puntkomma's.

> [!NOTE]
> U kunt maximaal 20 e-mailadressen per virtuele matrix invoeren.

Nadat u e-mailmelding voor een virtuele matrix hebt ingeschakeld, wordt een e-mailbericht telkens wanneer een kritieke waarschuwing wordt gegenereerd door leden van de meldingenlijst ontvangen. De berichten worden verzonden vanuit *storsimple-alerts-noreply@mail.windowsazure.com* en vindt u een voorwaarde voor de waarschuwing. Ontvangers kunnen klikken op **Afmelden** om zelf te verwijderen uit de lijst met e-melding.

#### <a name="to-enable-email-notification-for-alerts"></a>E-mailmeldingen voor waarschuwingen inschakelen

1. Ga naar uw StorSimple Device Manager-service en in de **Management** sectie, selecteer en klik op **apparaten**. Selecteer in de lijst met apparaten weergegeven, en klikt u op uw apparaat.
   
    ![instellingen voor waarschuwingen](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Hiermee opent u de **instellingen** blade. In de **apparaatinstellingen** sectie, selecteer **algemene**. Hiermee opent u de **algemene instellingen** blade.
   
    ![configuratie voor meldingen van waarschuwingen](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. In de **algemene instellingen** blade, Ga naar **waarschuwingsinstellingen** sectie en stel het volgende:
   
   1. In de **Activeer e-mailmelding** veld **Ja**.
   2. In de **e-servicebeheerders** veld **Ja** als u wilt dat de servicebeheerder en alle medebeheerders de waarschuwingsmeldingen moeten ontvangen.
   3. In de **aanvullende e-mailontvangers** en voer de e-mailadressen van alle andere ontvangers die de waarschuwingsmeldingen moeten ontvangen. Geef de namen in de indeling *someone@somewhere.com*. Gebruik puntkomma te scheiden van de e-mailadressen. U kunt maximaal 20 e-mailadressen per virtuele apparaat configureren.
      
       ![configuratie voor meldingen van waarschuwingen](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. Als u wilt een test-e-mailmelding verzenden, klikt u op **testbericht verzenden**. De StorSimple Device Manager-service wordt statusberichten weergegeven als de Testmelding wordt doorgestuurd.
      
       ![Waarschuwingen test-e-mailmelding verzonden](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Als het meldingsbericht test kan niet worden verzonden, kan de StorSimple Device Manager-service een toepasselijk bericht wordt weergegeven. Klik op **OK**, wacht een paar minuten en probeer het vervolgens opnieuw uw test-melding verzenden.
      >
      >
   5. Aan de onderkant van de pagina, klikt u op **opslaan** aan uw configuratie op te slaan. Klik op **Ja** als u om bevestiging wordt gevraagd.
      
      ![Waarschuwingen test-e-mailmelding verzonden](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Algemene voorwaarden van de waarschuwing

Uw StorSimple Virtual Array genereert waarschuwingen in reactie op tal van voorwaarden. Hier volgen de meest voorkomende typen waarschuwingen voorwaarden:

* **Problemen met de netwerkverbinding** – deze waarschuwingen gegenereerd wanneer er problemen bij het overbrengen van gegevens. Communicatieproblemen kunnen optreden tijdens de overdracht van gegevens naar en vanuit de Azure storage-account of vanwege een gebrek aan connectiviteit tussen de virtuele apparaten en de StorSimple Device Manager-service. Communicatieproblemen zijn enkele van de moeilijkst om op te lossen omdat er zoveel Point of failure. U moet altijd eerst controleren of verbinding met het netwerk en toegang tot Internet beschikbaar zijn voordat u doorgaat u aan bij de meer geavanceerde probleemoplossing. Voor informatie over de poorten en firewall-instellingen, gaat u naar [systeemvereisten voor StorSimple Virtual Array](storsimple-ova-system-requirements.md). Voor hulp bij het oplossen van problemen, gaat u naar [problemen oplossen met de cmdlet Test-Connection](storsimple-troubleshoot-deployment.md).
* **Prestatieproblemen** – deze waarschuwingen zijn veroorzaakt doordat het systeem is niet optimaal, zoals wanneer het zich onder een zware belasting.

Bovendien ziet u mogelijk waarschuwingen met betrekking tot beveiliging, updates of mislukte.

## <a name="alert-severity-levels"></a>Ernstniveau van waarschuwingen

Waarschuwingen hebben verschillende niveaus, afhankelijk van de impact die de waarschuwing situatie heeft en de noodzaak van een reactie op de waarschuwing. De niveaus zijn:

* **Kritieke** : deze waarschuwing wordt in reactie op een voorwaarde die invloed heeft op de geslaagde prestaties van uw systeem. Actie vereist om ervoor te zorgen dat de service niet wordt onderbroken StorSimple.
* **Waarschuwing** – dit probleem kritiek kan worden als niet is opgelost. U moet de situatie onderzoeken en elke actie die vereist is om te wissen van het probleem.
* **Informatie** : deze waarschuwing bevat informatie die nuttig zijn kan bij het bijhouden en beheren van uw systeem.

## <a name="view-and-track-alerts"></a>Waarschuwingen weergeven en bijhouden

De overzichtsblade van de StorSimple Device Manager-service biedt u een snelle weergave op het aantal waarschuwingen op uw virtuele apparaten, gerangschikt op ernstniveau.

![Waarschuwingen-dashboard](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Klik op de ernst op om te openen de **waarschuwingen** blade. De resultaten bevatten alleen de waarschuwingen die overeenkomen met die ernstniveau.

![Rapport waarschuwingen binnen het bereik van Waarschuwingstype](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Klik op een waarschuwing in de lijst voor meer informatie over de waarschuwing, de laatste keer dat de waarschuwing is gemeld, waaronder het aantal exemplaren van de waarschuwing op het apparaat en de aanbevolen actie de waarschuwing te verhelpen.

![Lijst met waarschuwingen en details](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Als u de gegevens te verzenden naar Microsoft Support nodig hebt, kunt u de details van de waarschuwing kopiëren naar een tekstbestand. Nadat u de aanbeveling wordt gevolgd en de waarschuwingsvoorwaarde on-premises opgelost, moet u de waarschuwing in de lijst wissen. Selecteer de waarschuwing in de lijst en klik vervolgens op **wissen**. Schakel meerdere waarschuwingen, elke waarschuwing selecteert, klikt u op een kolom met uitzondering van de **waarschuwing** kolom en klik vervolgens op **wissen** nadat u hebt geselecteerd de waarschuwingen worden gewist.

Wanneer u klikt op **wissen**, hebt u de mogelijkheid om opmerkingen over de waarschuwing en de stappen die u hebt genomen om het probleem te verhelpen.

![Waarschuwing opmerkingen](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Sommige gebeurtenissen worden verwijderd door het systeem als een andere gebeurtenis wordt geactiveerd met nieuwe gegevens.

## <a name="sort-and-review-alerts"></a>Waarschuwingen voor sorteren en controleren

De **waarschuwingen** blade kan maximaal 250 waarschuwingen worden weergegeven. Als u dit aantal waarschuwingen hebt overschreden, wordt niet alle waarschuwingen worden weergegeven in de standaardweergave. De volgende velden om aan te passen welke waarschuwingen worden weergegeven, kunt u combineren:

* **Status** – u kunt een weergeven **Active** of **uitgeschakeld** waarschuwingen. Actieve waarschuwingen worden nog steeds op uw systeem wordt geactiveerd terwijl uitgeschakeld waarschuwingen zijn handmatig verwijderd door een beheerder of via een programma is uitgeschakeld omdat het systeem voorwaarde voor de waarschuwing met nieuwe informatie bijgewerkt.
* **Ernst** – u kunt waarschuwingen van alle niveaus (kritiek, waarschuwing, informatie), of alleen een bepaalde prioriteit, zoals alleen kritieke waarschuwingen weergeven.
* **Bron** – u kunt waarschuwingen weergeven vanaf alle bronnen of beperken van de waarschuwingen die afkomstig van de service of een of alle van de virtuele apparaten zijn.
* **Tijdsbereik** – door op te geven de **van** en **naar** datums en tijdstempels, u kunt kijken waarschuwingen gedurende de periode waarin u geïnteresseerd bent.

## <a name="alerts-quick-reference"></a>Snelzoekgids voor waarschuwingen

De volgende tabellen worden enkele van de StorSimple-waarschuwingen die u tegenkomen kunt, evenals aanvullende informatie en aanbevelingen indien beschikbaar. StorSimple Virtual Array waarschuwingen kunnen worden onderverdeeld in een van de volgende categorieën:

* [Connectiviteitswaarschuwingen cloud](#cloud-connectivity-alerts)
* [Van configuratiewaarschuwingen](#configuration-alerts)
* [Waarschuwingen voor taak](#job-failure-alerts)
* [Waarschuwingen over toepassingsprestaties](#performance-alerts)
* [Beveiligingswaarschuwingen](#security-alerts)

### <a name="cloud-connectivity-alerts"></a>Connectiviteitswaarschuwingen cloud

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Apparaat <*apparaatnaam*> niet is verbonden met de cloud. |Het apparaat met de naam kan geen verbinding maken met de cloud. |Kan geen verbinding maken met de cloud. Dit kan een van de volgende oorzaken hebben:<ul><li>Het is mogelijk dat er een probleem met de instellingen op uw apparaat.</li><li>Het is mogelijk dat er een probleem met de referenties van het storage-account.</li></ul>Voor meer informatie over het oplossen van problemen met de netwerkverbinding, gaat u naar de [lokale web-UI](storsimple-ova-web-ui-admin.md) van het apparaat. |

### <a name="configuration-alerts"></a>Van configuratiewaarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| On-premises virtueel apparaatconfiguratie wordt niet ondersteund. |Trage prestaties. |De huidige configuratie kan leiden tot verminderde prestaties. Zorg ervoor dat uw server voldoet aan de minimale configuratievereisten. Ga voor meer informatie naar [StorSimple virtuele matrix: vereisten](storsimple-ova-system-requirements.md). |
| U worden uitgevoerd buiten de ingerichte schijfruimte beschikbaar op <*apparaatnaam*\>. |Waarschuwing voor schijf-ruimte. |U weinig ingerichte schijfruimte beschikbaar. Als u wilt Maak ruimte vrij, houd rekening met workloads verplaatst naar een ander volume of share of het verwijderen van gegevens. |

### <a name="job-failure-alerts"></a>Waarschuwingen voor taak

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Back-up van <*apparaatnaam* \> kan niet worden voltooid. |Back-uptaak is mislukt. |Kan een back-up niet maken. Houd rekening met een van de volgende:<ul><li>Problemen met de netwerkverbinding kunnen worden voorkomen dat de back-upbewerking is voltooid. Zorg ervoor dat er geen verbindingsproblemen zijn. Voor meer informatie over het oplossen van problemen met de netwerkverbinding, gaat u naar de [lokale web-UI](storsimple-ova-web-ui-admin.md) voor uw virtuele apparaat.</li><li>U kunt de beschikbare opslaglimiet hebt bereikt. Overweeg het verwijderen van een back-ups die niet meer nodig zijn om ruimte vrij.</li></ul> De problemen oplossen, wist u de waarschuwing en voer de bewerking opnieuw uit. |
| Klonen van <*apparaatnaam* \> kan niet worden voltooid. |Kloon de migratietaak is mislukt. |Kan geen een kloon maken. Houd rekening met een van de volgende:<ul><li>De lijst met back-up is mogelijk niet geldig. Vernieuw de lijst om te controleren of dat het nog geldig is.</li><li>Problemen met de netwerkverbinding kunnen worden voorkomen dat de kloonbewerking is voltooid. Zorg ervoor dat er geen verbindingsproblemen zijn.</li><li>U kunt de beschikbare opslaglimiet hebt bereikt. Overweeg het verwijderen van een back-ups die niet meer nodig zijn om ruimte vrij.</li></ul>De problemen oplossen, wist u de waarschuwing en voer de bewerking opnieuw uit. |

### <a name="networking-alerts"></a>Waarschuwingen voor netwerken
| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Kan geen verbinding maken met de service voor verificatie. |Gegevenspad-fout |De URL die wordt gebruikt voor verificatie is niet bereikbaar. Zorg ervoor dat uw firewall-regels de URL-patronen die is opgegeven voor het StorSimple-apparaat bevatten. Voor meer informatie over URL-patronen in Azure portal, gaat u naar [StorSimple Virtual Array netwerkvereisten](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).|

### <a name="performance-alerts"></a>Waarschuwingen over toepassingsprestaties

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| U ervaart onverwachte vertragingen bij overdracht van gegevens. |Trage gegevensoverdracht. |Beperkingsfouten optreden wanneer u de schaalbaarheidsdoelen van een opslagservice overschrijdt. De opslagservice doet dit om ervoor te zorgen dat geen enkele client of de tenant de service ten koste van anderen kunt gebruiken. Voor meer informatie over het oplossen van uw Azure storage-account, gaat u naar [bewaken, problemen vaststellen en oplossen van Microsoft Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md). |
| U hebt onvoldoende lokale ruimte gereserveerd geen schijfruimte meer beschikbaar op <*apparaatnaam*\>. |Trage reactietijd. |10% van de totale ingerichte grootte voor <*apparaatnaam* \> is gereserveerd op de lokale apparaat en u nu weinig de gereserveerde ruimte. De werkbelasting op <*apparaatnaam* \> genereert een hogere snelheid van verloop of u mogelijk hebt onlangs gemigreerd een grote hoeveelheid gegevens. Dit kan leiden tot verminderde prestaties. Houd rekening met een van de volgende acties om dit probleem oplossen:<ul><li>Vergroot de bandbreedte van de cloud op dit apparaat.</li><li>Verminder of werkbelastingen verplaatsen naar een ander volume of share.</li></ul> |

### <a name="security-alerts"></a>Beveiligingswaarschuwingen

| Tekst van de waarschuwing | Gebeurtenis | Meer informatie / aanbevolen acties |
|:--- |:--- |:--- |
| Wachtwoord voor <*apparaatnaam* \> verloopt <*getal* \> dagen. |Waarschuwing: wachtwoord. |Uw wachtwoord verloopt <*getal* \> dagen. U kunt uw wachtwoord wijzigen. Ga voor meer informatie naar [wijzigen van het beheerderswachtwoord van de StorSimple Virtual Array](storsimple-virtual-array-change-device-admin-password.md). |

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de StorSimple Virtual Array](storsimple-ova-overview.md).
