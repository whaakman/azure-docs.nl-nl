---
title: PowerShell voor StorSimple-apparaat management | Microsoft Docs
description: Informatie over het beheren van uw StorSimple-apparaat met Windows PowerShell voor StorSimple.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0ff3bb0d-897a-4676-bdcb-402c2628dac5
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: alkohli@microsoft.com
ms.openlocfilehash: af135518f66eb8c94c183f28191f016fcd601ae1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Windows PowerShell voor StorSimple gebruiken voor het beheren van uw apparaat
## <a name="overview"></a>Overzicht
Windows PowerShell voor StorSimple biedt een opdrachtregelinterface die u gebruiken kunt om uw Microsoft Azure StorSimple-apparaat te beheren. Zoals de naam wordt voorgesteld, is een Windows PowerShell gebaseerde opdrachtregelinterface die is ingebouwd in een beperkte runspace. Vanuit het perspectief van de gebruiker op de opdrachtregel, wordt een beperkte runspace weergegeven als een beperkte versie van Windows PowerShell. Deze interface heeft terwijl sommige van de basismogelijkheden van Windows PowerShell andere speciale cmdlets die zijn gericht op het beheren van uw Microsoft Azure StorSimple-apparaat. 

Dit artikel beschrijft de Windows PowerShell voor StorSimple-onderdelen, met inbegrip van hoe u verbinding kunt maken met deze interface en bevat koppelingen naar stapsgewijze procedures of werkstromen die u kunt uitvoeren met behulp van deze interface. De werkstromen bevatten informatie over het registreren van uw apparaat, het configureren van de netwerkinterface op uw apparaat, installeren van updates waarvoor het apparaat worden in de onderhoudsmodus, wijzig de status van het apparaat en los eventuele problemen die kunnen optreden.

Na het lezen van dit artikel kunt u zich kunt:

* Verbinding maken met uw StorSimple-apparaat met Windows PowerShell voor StorSimple.
* Beheren van uw StorSimple-apparaat met Windows PowerShell voor StorSimple.
* Help opvragen in Windows PowerShell voor StorSimple.

> [!NOTE]
> * Windows PowerShell voor StorSimple-cmdlets kunt u uw StorSimple-apparaat van een seriële console of extern via Windows PowerShell op afstand beheren. Ga voor meer informatie over elk van de afzonderlijke cmdlets die kunnen worden gebruikt in deze interface naar [cmdlet-naslaginformatie voor Windows PowerShell voor StorSimple](https://technet.microsoft.com/library/dn688168.aspx).
> * De StorSimple van Azure PowerShell-cmdlets zijn een andere verzameling van cmdlets waarmee u StorSimple serviceniveau automatiseren en taken voor migratie vanaf de opdrachtregel. Voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple, gaat u naar de [Azure StorSimple-cmdlet-verwijzing](/powershell/module/azure/?view=azuresmps-3.7.0).
> 
> 

U kunt toegang tot de Windows PowerShell voor StorSimple met behulp van een van de volgende methoden:

* [Verbinding maken met de seriële console van StorSimple-apparaat](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
* [Extern verbinding maken met behulp van Windows PowerShell StorSimple](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Verbinding maken met Windows PowerShell voor StorSimple via de seriële console van het apparaat
U kunt [download PuTTY](http://www.putty.org/) of soortgelijke terminalemulatiesoftware verbinding maken met Windows PowerShell voor StorSimple. U moet configureren PuTTY specifiek voor toegang tot de Microsoft Azure StorSimple-apparaat. De volgende onderwerpen bevatten gedetailleerde stappen over het configureren van PuTTy en verbinding maken met het apparaat. Verschillende opties in de seriële console, worden ook beschreven.

### <a name="putty-settings"></a>Instellingen voor PuTTY
Zorg ervoor dat u de volgende instellingen voor PuTTY verbinding maken met de Windows PowerShell-interface van de seriële console.

#### <a name="to-configure-putty"></a>PuTTY configureren
1. In de PuTTY **herconfiguratie** het dialoogvenster de **categorie** deelvenster **toetsenbord**.
2. Zorg ervoor dat de volgende opties zijn geselecteerd (dit zijn de standaardinstellingen wanneer u een nieuwe sessie start). 
   
   | Toetsenbord-item | Selecteer |
   | --- | --- |
   | BACKSPACE sleutel |Controle-? (127) |
   | Start en End sleutels |Standard |
   | Functietoetsen en toetsenblok |ESC [n ~ |
   | Oorspronkelijke status van de cursor sleutels |Normaal |
   | Oorspronkelijke status van het numerieke toetsenblok |Normaal |
   | Extra toetsenbord-functies inschakelen |CTRL + ALT + verschilt van AltGr |
   
    ![Ondersteunde instellingen voor Putty](./media/storsimple-windows-powershell-administration/IC740877.png)
3. Klik op **Toepassen**.
4. In de **categorie** deelvenster **vertaling**.
5. In de **externe tekenset** keuzelijst met invoervak, selecteer **UTF-8**.
6. Onder **afhandeling van regel tekening tekens**, selecteer **gebruik Unicode-regel tekenen-codepunten**. De volgende afbeelding ziet de juiste PuTTY selecties.
   
    ![Instellingen voor Putty UTF](./media/storsimple-windows-powershell-administration/IC740878.png)
7. Klik op **Toepassen**.

U kunt nu PuTTY gebruiken om te verbinden met de seriële console van het apparaat door de volgende stappen uit te voeren.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="about-the-serial-console"></a>Over de seriële console
Wanneer u toegang de Windows PowerShell tot-interface van uw StorSimple-apparaat via de seriële console, een bannerbericht wordt weergegeven, gevolgd door de menuopties. 

Het bericht banner bevat algemene StorSimple-apparaat informatie zoals het model, de naam, de versie van de geïnstalleerde software en de status van de domeincontroller die u wilt openen. De volgende afbeelding toont een voorbeeld van een bannerbericht.

![Seriële bannerbericht aangegeven](./media/storsimple-windows-powershell-administration/IC741098.png)

> [!IMPORTANT]
> U kunt het bannerbericht aangegeven gebruiken om te bepalen of de domeincontroller die u met verbonden bent actief of passief is.
> 
> 

De volgende afbeelding ziet u de verschillende runspace opties die beschikbaar in het menu van de seriële console zijn.

![Registreer uw apparaat 2](./media/storsimple-windows-powershell-administration/IC740906.png)

U kunt kiezen uit de volgende instellingen:

1. **Meld u aan met volledige toegang** met deze optie kunt u verbinding maken (met de juiste referenties) met de **SSAdminConsole** runspace op de lokale domeincontroller. (De lokale domeincontroller is de domeincontroller die u momenteel via de seriële console van uw StorSimple-apparaat opent). Deze optie kan ook worden gebruikt om toe te staan van Microsoft Support voor toegang tot onbeperkte runspace (een ondersteuningssessie) voor het oplossen van problemen van mogelijke apparaten. Nadat u de optie 1 gebruikt om aan te melden, kunt u het Microsoft Support engineer van toegang tot onbeperkte runspace door een specifieke cmdlet wordt uitgevoerd. Raadpleeg voor meer informatie, [ondersteuningssessie starten](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).
2. **Aanmelden bij de domeincontroller peer met volledige toegang** deze optie is hetzelfde als de optie 1, behalve dat u verbinding kunt maken (met de juiste referenties) voor de **SSAdminConsole** runspace op de peer-controller. Omdat het StorSimple-apparaat een hoge beschikbaarheid-apparaat met twee domeincontrollers in een actief / passief-configuratie, peer verwijzen we naar de andere domeincontrollers in het apparaat die u via de seriële console openen wilt).
   Net als bij de optie 1, deze optie kan ook worden gebruikt om toe te staan van Microsoft Support voor toegang tot onbeperkte runspace op een peer-domeincontroller.
3. **Verbinding maken met beperkte toegang** deze optie wordt gebruikt voor toegang tot Windows PowerShell-interface in de beperkte modus. U wordt niet gevraagd om referenties voor toegang. Deze optie verbindt met een meer beperkte runspace vergeleken met de opties 1 en 2.  Enkele van de taken die beschikbaar via de optie 1 zijn die **kan niet* worden uitgevoerd in deze runspace zijn:
   
   * De fabrieksinstellingen terugzetten
   * Het wachtwoord wijzigen
   * In- of uitschakelen van ondersteuning toegang
   * Updates toepassen
   * Installeren van hotfixes 

    > [!NOTE]
    > Dit is de beste optie als u het apparaat administrator-wachtwoord bent vergeten, en kan geen verbinding met behulp van de optie 1 of 2 maken.

4. **Taal wijzigen** met deze optie kunt u de weergavetaal van de Windows PowerShell-interface wijzigen. De ondersteunde talen zijn Engels, Japans, Russisch, Frans, Zuid Koreaans, Spaans, Italiaans, Duits, Chinees en Portugees (Brazilië).

## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Extern verbinding maken met behulp van Windows PowerShell voor StorSimple StorSimple
U kunt Windows PowerShell op afstand verbinding maken met uw StorSimple-apparaat gebruiken. Als u op deze manier verbinding, ziet u een menu. (U ziet een menu alleen als u de seriële console op het apparaat om verbinding te gebruiken. Extern verbinding maakt gaat u rechtstreeks naar het equivalent van de 'optie 1: volledige toegang' op de seriële console.) Met Windows PowerShell op afstand verbinding u met een specifieke runspace. U kunt ook de weergavetaal opgeven. 

De weergavetaal is onafhankelijk van de taal die u met behulp van instelt de **taal wijzigen** in het menu van de seriële console optie. De landinstellingen van het apparaat dat u verbinding maakt vanaf als niets wordt opgegeven externe PowerShell automatisch opgehaald.

> [!NOTE]
> Als u met virtuele Microsoft Azure-hosts en virtuele StorSimple-apparaten werkt, kunt u Windows PowerShell op afstand en de virtuele host verbinding maken met het virtuele apparaat. Als u een locatie op de host op voor het opslaan van gegevens uit de Windows PowerShell-sessie hebt ingesteld, moet u zich bewust zijn dat de principal iedereen alleen geverifieerde gebruikers bevat. Als u de share voor toegang door iedereen hebt ingesteld en u verbinding maken zonder het opgeven van referenties, blijven de niet-geverifieerde anonieme principal wordt gebruikt en worden er een fout. Los het probleem op, in de share hosten u moet de gastaccount inschakelen en geef vervolgens de Gast volledige toegang tot de share of moet u geldige referenties samen met de Windows PowerShell-cmdlet.
> 
> 

U kunt HTTP of HTTPS gebruiken via Windows PowerShell op afstand te verbinden. Volg de instructies in de volgende handleidingen:

* [Verbinding maken op afstand via HTTP](storsimple-remote-connect.md#connect-through-http)
* [Verbinding maken op afstand via HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Beveiligingsoverwegingen voor verbinding
Wanneer u verbinding maken met Windows PowerShell voor StorSimple beslist, het volgende overwegen:

* Rechtstreeks verbinding maken met de seriële console van het apparaat is beveiligd, maar geen verbinding maken met de seriële console via netwerkswitches is. Wees voorzichtig met over de beveiligingsrisico's het verbinding maken met de seriële apparaat via netwerkswitches.
* Verbinding maken via een HTTP-sessie biedt meer beveiliging dan verbinding maken via de seriële console via netwerk mogelijk. Hoewel dit niet de meest beveiligde methode, is het is acceptabel in vertrouwde netwerken.
* Verbinding maken via een HTTPS-sessie is de veiligste en de aanbevolen optie.

## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Beheren van uw StorSimple-apparaat met Windows PowerShell voor StorSimple
De volgende tabel bevat een overzicht van de algemene beheertaken en complexe werkstromen die kunnen worden uitgevoerd in de Windows PowerShell-interface van uw StorSimple-apparaat. Klik op de desbetreffende vermelding in de tabel voor meer informatie over elke werkstroom.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell voor StorSimple-werkstromen
| Als u dit wilt doen... | Gebruik deze procedure. |
| --- | --- |
| Registreer uw apparaat |[Configureren en registreren van het apparaat met Windows PowerShell voor StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
| Webproxy configureren</br>Web proxy-instellingen weergeven |[Webproxy voor uw StorSimple-apparaat configureren](storsimple-configure-web-proxy.md) |
| DATA 0 netwerkinterface-instellingen op uw apparaat wijzigen |[DATA 0-netwerkinterface voor uw StorSimple-apparaat wijzigen](storsimple-modify-data-0.md) |
| Stoppen van een domeincontroller </br> Opnieuw opstarten of afsluiten van een domeincontroller </br> Een apparaat afsluiten</br>Het apparaat niet Fabrieksinstellingen terugzetten |[Apparaatcontrollers beheren](storsimple-manage-device-controller.md) |
| Onderhoud modus updates en hotfixes installeren |[Uw apparaat bijwerken](storsimple-update-device.md) |
| Voer de onderhoudsmodus </br>Onderhoudsmodus afsluiten |[Modi voor StorSimple-apparaat](storsimple-device-modes.md) |
| Een ondersteuningspakket maken</br>Ontsleutelen en een ondersteuningspakket bewerken |[Maken en beheren van een ondersteuningspakket](storsimple-create-manage-support-package.md) |
| Een ondersteuningssessie starten</br> |[Een support-sessie te starten in Windows PowerShell voor StorSimple](storsimple-create-manage-support-package.md#manually-create-a-support-package) |

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Help opvragen in Windows PowerShell voor StorSimple
In Windows PowerShell voor StorSimple is de cmdlet Help beschikbaar. Een actuele versie van dit Help-onderwerp is ook beschikbaar die u kunt gebruiken voor het bijwerken van de Help op uw systeem.

Help opvragen in deze interface is vergelijkbaar met Windows PowerShell en de meeste van de Help-gerelateerde cmdlets zal werken. Help voor Windows PowerShell online vindt u in de TechNet Library: [met Windows PowerShell-scripts](http://go.microsoft.com/fwlink/?LinkID=108518).

Hier volgt een korte beschrijving van de typen van de Help voor deze Windows PowerShell-interface, inclusief het bijwerken van de Help.

#### <a name="to-get-help-for-a-cmdlet"></a>Voor hulp bij een cmdlet
* Als u de Help voor de functie of cmdlet, moet u de volgende opdracht gebruiken:`Get-Help <cmdlet-name>`
* Als u online-Help voor een cmdlet, gebruikt u de vorige cmdlet met de `-Online` parameter:`Get-Help <cmdlet-name> -Online`
* Volledige hulp nodig hebt, kunt u de `–Full` parameter, en voor voorbeelden gebruiken de `–Examples` parameter.

#### <a name="to-update-help"></a>Help bijwerken
U kunt de Help in de Windows PowerShell-interface eenvoudig bijwerken. Voer de volgende stappen uit voor het bijwerken van de Help op uw systeem.

#### <a name="to-update-cmdlet-help"></a>Cmdlet Help bijwerken
1. Start Windows PowerShell met de **als administrator uitvoeren** optie.
2. Typ het volgende achter de opdrachtprompt:`Update-Help`
3. De bijgewerkte Help-bestanden wordt geïnstalleerd.
4. Nadat de helpbestanden zijn geïnstalleerd, typt: `Get-Help Get-Command`. Hiermee wordt een lijst met cmdlets waarvoor Help beschikbaar is weergegeven.

> [!NOTE]
> Als u een lijst van de beschikbare cmdlets in een runspace, meld u aan de bijbehorende menuoptie bij en voer de `Get-Command` cmdlet.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Als u problemen met uw StorSimple-apparaat ondervindt bij het uitvoeren van een van de bovenstaande werkstromen, raadpleegt u [hulpprogramma's voor probleemoplossing StorSimple implementaties](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

