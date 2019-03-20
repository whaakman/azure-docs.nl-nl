---
title: PowerShell voor StorSimple-Apparaatbeheer | Microsoft Docs
description: Leer hoe u Windows PowerShell voor StorSimple gebruiken voor het beheren van uw StorSimple-apparaat.
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
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli@microsoft.com
ms.openlocfilehash: e6053ef9b5e1fc113e3bd3057d627347f285cd99
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58003121"
---
# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Windows PowerShell voor StorSimple gebruiken voor het beheren van uw apparaat

## <a name="overview"></a>Overzicht

Windows PowerShell voor StorSimple biedt een opdrachtregelinterface die u gebruiken kunt om uw Microsoft Azure StorSimple-apparaat te beheren. Zoals de naam al aangeeft, is het een opdrachtregelinterface op basis van Windows PowerShell die is ingebouwd in een beperkte runspace. Vanuit het perspectief van de gebruiker op de opdrachtregel, wordt een beperkte runspace weergegeven als een beperkte versie van Windows PowerShell. Tijdens het onderhoud van enkele van de functionaliteit van basis van Windows PowerShell, heeft deze interface extra toegewezen cmdlets die zijn afgestemd op uw Microsoft Azure StorSimple-apparaat te beheren.

Dit artikel worden de Windows PowerShell voor StorSimple-onderdelen, met inbegrip van hoe u verbinding kunt maken met deze interface en bevat koppelingen naar stapsgewijze procedures of werkstromen die u kunt uitvoeren met behulp van deze interface. De werkstromen bevatten informatie over het registreren van uw apparaat, configureer de netwerkinterface op uw apparaat, installeer updates die vereisen het apparaat worden in de onderhoudsmodus bevindt dat, wijzigt de status van het apparaat en oplossen van problemen die kunnen optreden.

Na het lezen van dit artikel, kunt u zich aan:

* Verbinding maken met uw StorSimple-apparaat met Windows PowerShell voor StorSimple.
* Beheren van uw StorSimple-apparaat met Windows PowerShell voor StorSimple.
* Hulp krijgen in Windows PowerShell voor StorSimple.

> [!NOTE]
> * Windows PowerShell voor StorSimple-cmdlets kunt u voor het beheren van uw StorSimple-apparaat van een seriële console of extern via Windows PowerShell voor externe toegang. Voor meer informatie over elk van de afzonderlijke cmdlets die kunnen worden gebruikt in deze interface, gaat u naar [cmdlet-naslaginformatie voor Windows PowerShell voor StorSimple](https://technet.microsoft.com/library/dn688168.aspx).
> * De StorSimple van Azure PowerShell-cmdlets zijn een andere verzameling van cmdlets waarmee u voor het automatiseren van StorSimple serviceniveau en taken van de migratie vanaf de opdrachtregel. Voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple, gaat u naar de [Azure StorSimple-cmdlet-verwijzing](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0&viewFallbackFrom=azuresmps-3.7.0#azure).


U kunt toegang tot de Windows PowerShell voor StorSimple met behulp van een van de volgende methoden:

* [Verbinding maken met de seriële console van het StorSimple-apparaat](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
* [Extern verbinding maken met StorSimple met behulp van Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Verbinding maken met Windows PowerShell voor StorSimple via de seriële console van het apparaat

U kunt [download PuTTY](https://www.putty.org/) of vergelijkbare terminalemulatiesoftware verbinding maakt met Windows PowerShell voor StorSimple. U moet PuTTY specifiek voor toegang tot de Microsoft Azure StorSimple-apparaat configureren. De volgende onderwerpen bevatten gedetailleerde stappen over het configureren van PuTTy en maak verbinding met het apparaat. Verschillende menu-opties in de seriële console worden ook beschreven.

### <a name="putty-settings"></a>Instellingen voor PuTTY

Zorg ervoor dat u de volgende instellingen voor PuTTY gebruiken om te verbinden met de Windows PowerShell-interface van de seriële console.

#### <a name="to-configure-putty"></a>PuTTY configureren

1. In de PuTTY **herconfiguratie** in het dialoogvenster de **categorie** venster **toetsenbord**.
2. Zorg ervoor dat de volgende opties zijn geselecteerd (dit zijn de standaardinstellingen wanneer u een nieuwe sessie starten).
   
   | Toetsenbord-item | Selecteer |
   | --- | --- |
   | BACKSPACE sleutel |Controle-? (127) |
   | Start en End-sleutels |Standard |
   | Functietoetsen en het toetsenblok |ESC[n~ |
   | Initiële status van de cursor sleutels |Normaal |
   | Initiële status van het numerieke toetsenblok |Normaal |
   | Extra toetsenbord-functies inschakelen |CTRL + ALT + wijkt af van AltGr |
   
    ![Instellingen voor Putty ondersteund](./media/storsimple-windows-powershell-administration/IC740877.png)
3. Klik op **Toepassen**.
4. In de **categorie** venster **vertaling**.
5. In de **externe tekenset** keuzelijst, selecteer **UTF-8**.
6. Onder **verwerking van lijntekening tekens**, selecteer **gebruik Unicode lijntekening codepunten**. De volgende schermafbeelding ziet u de juiste PuTTY selecties.
   
    ![Instellingen voor Putty UTF](./media/storsimple-windows-powershell-administration/IC740878.png)
7. Klik op **Toepassen**.

U kunt PuTTY nu verbinding maken met de seriële console van het apparaat met de volgende stappen gebruiken.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="about-the-serial-console"></a>Over de seriële console

Wanneer u toegang de Windows PowerShell tot-interface van uw StorSimple-apparaat via de seriële console, een bannerbericht wordt weergegeven, gevolgd door menu-opties.

Het bericht banner bevat algemene informatie van StorSimple-apparaat, zoals het model, naam, versie van de geïnstalleerde software en status van de controller die u wilt openen. De volgende afbeelding toont een voorbeeld van een banner weergegeven.

![Seriële bannerbericht aangegeven](./media/storsimple-windows-powershell-administration/IC741098.png)

> [!IMPORTANT]
> U kunt het bannerbericht aangegeven gebruiken om te bepalen of de domeincontroller die u met verbonden bent _Active_ of _passieve_.

De volgende afbeelding ziet u de verschillende runspace met de opties die beschikbaar in het menu van de seriële console zijn.

![Uw apparaat 2 registreren](./media/storsimple-windows-powershell-administration/IC740906.png)

U kunt kiezen uit de volgende instellingen:

1. **Meld u aan met volledige toegang tot de** deze optie kunt u verbinding maken (met de juiste referenties) met de **SSAdminConsole** runspace op de lokale domeincontroller. (De lokale domeincontroller is de domeincontroller die u momenteel toegang hebben tot via de seriële console van uw StorSimple-apparaat.) Deze optie kan ook worden gebruikt om toe te staan van Microsoft Support voor toegang tot onbeperkte runspace (een ondersteuningssessie) voor het oplossen van problemen met mogelijke apparaten. Nadat u de optie 1 gebruikt om aan te melden, kunt u het Microsoft Support engineer voor toegang tot onbeperkte runspace door een specifieke cmdlet uit te voeren. Raadpleeg voor meer informatie, [Start een ondersteuningssessie](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).
   
2. **Meld u aan bij de peer-controller met volledige toegang** deze optie is hetzelfde als in optie 1, behalve dat u verbinding (met de juiste referenties) met maken kunt de **SSAdminConsole** runspace op de peer-controller. Omdat het StorSimple-apparaat een hoge beschikbaarheid-apparaat met twee domeincontrollers in een actief-passieve configuratie is, peer verwijzen we naar de andere controller in het apparaat dat u via de seriële console openen wilt).
   Net als bij de optie 1, deze optie kan ook worden gebruikt om toe te staan van Microsoft Support voor toegang tot onbeperkte runspace op een peer-controller.

3. **Verbinding maken met beperkte toegang** deze optie wordt gebruikt voor toegang tot Windows PowerShell-interface in de beperkte modus. U wordt niet gevraagd om toegangsreferenties. Deze optie verbindt met een beperkte runspace in vergelijking met opties voor 1 en 2.  Enkele van de taken die beschikbaar via de optie 1 zijn die **kan geen* worden uitgevoerd in deze runspace zijn:
   
   * De standaardinstellingen herstellen
   * Het wachtwoord wijzigen
   * In- of uitschakelen van toegang tot ondersteuning
   * Updates toepassen
   * Installeren van hotfixes

     > [!NOTE]
     > Dit is de beste optie als u het beheerderswachtwoord voor het apparaat vergeten bent en kan geen verbinding met behulp van de optie 1 of 2 maken.

4. **Taal wijzigen** deze optie kunt u de weergavetaal van de Windows PowerShell-interface wijzigen. De ondersteunde talen zijn Engels, Japans, Russisch, Frans, Zuid-Korea, Spaans, Italiaans, Duits, Chinees en Portugees (Brazilië).

## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Extern verbinding maken met StorSimple met behulp van Windows PowerShell voor StorSimple

U kunt Windows PowerShell voor externe toegang verbinding maken met uw StorSimple-apparaat gebruiken. Wanneer u op deze manier verbinding maakt, ziet u niet een menu. (U ziet een menu alleen als u de seriële console op het apparaat om verbinding te maken. Op afstand verbinding maken gaat u rechtstreeks naar het equivalent van "optie 1: volledige toegang' op de seriële console.) Met Windows PowerShell voor externe toegang verbinding u met een specifieke runspace. U kunt ook de weergavetaal opgeven.

De weergavetaal is onafhankelijk van de taal die u hebt ingesteld met behulp van de **taal wijzigen** in het menu van de seriële console optie. Externe PowerShell haalt automatisch de landinstellingen van het apparaat dat u verbinding maakt vanaf als er niets is opgegeven.

> [!NOTE]
> Als u met virtuele Microsoft Azure worden gehost en StorSimple Cloud Appliances werkt, kunt u Windows PowerShell voor externe toegang en de virtuele host verbinding maken met het cloudapparaat. Als u de sharelocatie van een op de host op voor het opslaan van gegevens uit de Windows PowerShell-sessie hebt ingesteld, moet u zich bewust zijn dat de _iedereen_ principal bevat alleen geverifieerde gebruikers. Dus als u de share hebt ingesteld voor toegang door _iedereen_ en u verbinding maken zonder referenties op te geven, de niet-geverifieerde anonieme principal wordt gebruikt en wordt er een fout. Dit probleem oplossen, op de share host die u hebt moet het gastaccount inschakelen en vervolgens geven de Gast account volledige toegang tot de share of moet u geldige referenties, samen met de Windows PowerShell-cmdlet.


U kunt HTTP of HTTPS gebruiken om te verbinden via Windows PowerShell voor externe toegang. Volg de instructies in de volgende zelfstudies:

* [Verbinding maken op afstand via HTTP](storsimple-8000-remote-connect.md#connect-through-http)
* [Verbinding maken op afstand via HTTPS](storsimple-8000-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Beveiligingsoverwegingen bij verbinding

Bij het bepalen hoe u verbinding maakt met Windows PowerShell voor StorSimple, houd rekening met het volgende:

* Directe verbinding met de seriële console van het apparaat is beveiligd, maar geen verbinding maken met de seriële console via netwerkswitches is. Wees voorzichtig met van de beveiligingsrisico's het seriële apparaat verbinden via netwerkswitches.
* Verbinding maken via een HTTP-sessie biedt meer beveiliging dan verbinding maken via de seriële console via netwerk mogelijk. Hoewel dit niet de meest beveiligde methode is, is het acceptabel in vertrouwde netwerken.
* Verbinding maken via een HTTPS-sessie is de veiligste en de aanbevolen optie.

## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Beheren van uw StorSimple-apparaat met Windows PowerShell voor StorSimple

De volgende tabel bevat een overzicht van de algemene beheertaken en complexe werkstromen die kunnen worden uitgevoerd in de Windows PowerShell-interface van uw StorSimple-apparaat. Klik op de juiste vermelding in de tabel voor meer informatie over elke werkstroom.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell voor StorSimple-werkstromen

| Als u dit wilt doen... | Gebruik deze procedure. |
| --- | --- |
| Uw apparaat registreren |[Configureren en registreren van het apparaat met Windows PowerShell voor StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
| Webproxy configureren</br>Webproxy-instellingen weergeven |[Webproxy voor uw StorSimple-apparaat configureren](storsimple-8000-configure-web-proxy.md) |
| DATA 0 netwerkinterface-instellingen op uw apparaat wijzigen |[DATA 0-netwerkinterface voor uw StorSimple-apparaat wijzigen](storsimple-8000-modify-data-0.md) |
| Stoppen van een domeincontroller </br> Opnieuw opstarten of afsluiten van een domeincontroller </br> Een apparaat uitschakelen</br>Het apparaat terugzetten op fabrieksinstellingen |[Apparaatcontrollers beheren](storsimple-8000-manage-device-controller.md) |
| Updates voor de onderhoudsmodus en hotfixes installeren |[Uw apparaat bijwerken](storsimple-update-device.md) |
| Voer in de onderhoudsmodus </br>Onderhoudsmodus afsluiten |[Modi voor StorSimple-apparaat](storsimple-8000-device-modes.md) |
| Een ondersteuningspakket maken</br>Ontsleutelen en een ondersteuningspakket bewerken |[Maken en beheren van een ondersteuningspakket](storsimple-8000-create-manage-support-package.md) |
| Een ondersteuningssessie starten</br> |[Een ondersteuningssessie starten in Windows PowerShell voor StorSimple](storsimple-8000-create-manage-support-package.md#create-a-support-package) |

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Hulp krijgen in Windows PowerShell voor StorSimple

In Windows PowerShell voor StorSimple is de cmdlet Help beschikbaar. Een actuele versie van deze Help-informatie is ook beschikbaar, die u kunt gebruiken om bij te werken van de Help op uw systeem.

Help-informatie in deze interface is vergelijkbaar met die in Windows PowerShell en de meeste van de Help-gerelateerde cmdlets werkt. Help voor Windows PowerShell online vindt u in de TechNet-bibliotheek: [Met Windows PowerShell-scripts](https://go.microsoft.com/fwlink/?LinkID=108518).

Hier volgt een korte beschrijving van de typen van de Help voor deze Windows PowerShell-interface, met inbegrip van de Help-informatie bijwerken.

### <a name="to-get-help-for-a-cmdlet"></a>Voor hulp bij een cmdlet

* Als u de Help voor een cmdlet of functie, gebruik de volgende opdracht: `Get-Help <cmdlet-name>`
* Voor online-Help voor een cmdlet, gebruikt u de vorige cmdlet met de `-Online` parameter: `Get-Help <cmdlet-name> -Online`
* Volledige Help-informatie, kunt u de `–Full` parameter, en voor voorbeelden gebruiken de `–Examples` parameter.

### <a name="to-update-help"></a>Om bij te werken Help

De Help in de Windows PowerShell-interface kunt u eenvoudig bijwerken. Voer de volgende stappen uit voor het bijwerken van de Help op uw systeem.

#### <a name="to-update-cmdlet-help"></a>Cmdlet Help bijwerken
1. Start Windows PowerShell met de **als administrator uitvoeren** optie.
2. Bij de opdrachtprompt, typt u:  `Update-Help`
3. De bijgewerkte Help-bestanden wordt geïnstalleerd.
4. Nadat de Help-bestanden zijn geïnstalleerd, typt: `Get-Help Get-Command`. Hiermee wordt een lijst met cmdlets waarvoor Help beschikbaar is weergegeven.

> [!NOTE]
> Als u een lijst van de beschikbare cmdlets in een runspace, meld u aan bij de bijbehorende menuoptie en voer de `Get-Command` cmdlet.


## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt met uw StorSimple-apparaat bij het uitvoeren van een van de bovenstaande werkstromen, verwijzen naar [hulpprogramma's voor het oplossen van StorSimple-implementaties](storsimple-8000-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

