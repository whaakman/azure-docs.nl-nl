---
title: Het bijhouden van wijzigingen met Azure Automation
description: De oplossing voor wijzigingen bijhouden kunt u identificeren van software- en Windows-Service-wijzigingen die in uw omgeving plaatsvinden.
services: automation
ms.service: automation
ms.component: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b110f83274b2b42896bd18fb364c355ecc97a028
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34258257"
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Het bijhouden van wijzigingen in uw omgeving met de oplossing voor wijzigingen bijhouden

In dit artikel helpt u de oplossing voor wijzigingen bijhouden gebruiken voor het herkennen van wijzigingen in uw omgeving. De oplossing houdt de wijzigingen voor Windows en Linux-software, Windows en Linux-bestanden, registersleutels voor Windows, Windows-services en Linux-daemons. Wijzigingen in de configuratie te identificeren, kunt u operationele problemen te lokaliseren.

Wijzigingen in de geïnstalleerde software, Windows-services, Windows-register en bestanden en Linux-daemons op de bewaakte servers worden verzonden naar de Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en registreert de gegevens van de cloudservice. Met behulp van de informatie op het dashboard bijhouden, kunt u eenvoudig de wijzigingen die zijn aangebracht in uw serverinfrastructuur bekijken.

## <a name="enable-change-tracking-and-inventory"></a>Wijzigingen bijhouden en Inventaris inschakelen

Om te beginnen met het bijhouden van wijzigingen, moet u de oplossing voor wijzigingen bijhouden en inventarisatie inschakelen voor uw Automation-Account.

1. Navigeer in de Azure-portal naar uw Automation-Account
1. Selecteer **bijhouden** onder **configuratie**.
1. Selecteer een bestaande Log analytics-werkruimte of **nieuwe werkruimte maken** en klik op **inschakelen**.

Hierdoor kan de oplossing voor uw automation-account. De oplossing kunnen maximaal 15 minuten om in te schakelen. De blauwe banner waarschuwt u als de oplossing is ingeschakeld. Ga terug naar de **bijhouden** pagina voor het beheren van de oplossing.

## <a name="configuring-change-tracking-and-inventory"></a>Bijhouden van wijzigingen en inventarisatie configureren

Voor meer informatie over hoe vrijgeven computers op de oplossing gaat u naar: [Onboarding Automation oplossingen](automation-onboard-solutions-from-automation-account.md). Zodra u de voorbereiding van een machine met de oplossing voor wijzigingen bijhouden en inventaris hebt, kunt u de items bijhouden configureren. Wanneer u een nieuw bestand of registersleutel bijhouden inschakelt, wordt deze ingeschakeld voor Change Tracking en inventaris.

Voor het bijhouden van wijzigingen in bestanden op Windows- en Linux, worden MD5-hashes van de bestanden die gebruikt. Bij deze hashes worden vervolgens gebruikt om te detecteren als er een wijziging is aangebracht sinds de laatste inventarisatie.

### <a name="configure-linux-files-to-track"></a>Linux-bestanden bijhouden configureren

Gebruik de volgende stappen voor het bestand bijhouden configureren op Linux-computers:

1. Selecteer in uw Automation-Account **bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het symbool tandwielpictogram).
2. Op de **bijhouden** pagina **Linux bestanden**, klikt u vervolgens op **+ toevoegen** toevoegen van een nieuw bestand om bij te houden.
3. Op de **Linux-bestand toevoegen voor het bijhouden**, voer de gegevens voor het bestand of map bijhouden en klik op **opslaan**.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Ingeschakeld     | Hiermee wordt bepaald of de instelling is toegepast.        |
|Itemnaam     | Beschrijvende naam van het bestand moet worden gevolgd.        |
|Groep     | De groepsnaam van een voor bestanden logisch groeperen.        |
|Pad invoeren     | Het pad naar het bestand controleert. Bijvoorbeeld: '/etc/*.conf'       |
|Padtype     | Type item moet worden bijgehouden, mogelijke waarden zijn bestanden en mappen.        |
|Recursie     | Bepaalt of recursie wordt gebruikt bij het zoeken naar het item dat moet worden bijgehouden.        |
|Sudo gebruiken     | Deze instelling bepaalt of sudo wordt gebruikt bij het controleren op het item.         |
|Koppelingen     | Deze instelling bepaalt hoe symbolische koppelingen worden afgehandeld bij het doorlopen van mappen.<br> **Negeren** - symbolische koppelingen worden genegeerd en de bestanden/mappen waarnaar wordt verwezen niet bevat.<br>**Ga als volgt** - volgt u de symbolische koppelingen tijdens recursie en omvat ook de bestanden/mappen waarnaar wordt verwezen.<br>**Beheren** - volgt de symbolische koppelingen en kunt wijzigen van de geretourneerde inhoud.     |

> [!NOTE]
> Het gebruik van de optie 'Beheren' voor koppelingen wordt niet aanbevolen. Het ophalen van bestandsinhoud wordt niet ondersteund.

### <a name="configure-windows-files-to-track"></a>Windows-bestanden bijhouden configureren

Gebruik de volgende stappen voor het configureren van de bestanden bijhouden op Windows-computers:

1. Selecteer in uw Automation-Account **bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het symbool tandwielpictogram).
2. Op de **bijhouden** pagina **Windows-bestanden**, klikt u vervolgens op **+ toevoegen** toevoegen van een nieuw bestand om bij te houden.
3. Op de **Windows-bestand toevoegen voor het bijhouden**, voer de gegevens voor het bestand bijhouden en klik op **opslaan**.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Ingeschakeld     | Hiermee wordt bepaald of de instelling is toegepast.        |
|Itemnaam     | Beschrijvende naam van het bestand moet worden gevolgd.        |
|Groep     | De groepsnaam van een voor bestanden logisch groeperen.        |
|Pad invoeren     | Het pad voor het controleren op het bestand, bijvoorbeeld: "C:\temp\myfile.txt"       |

### <a name="configure-windows-registry-keys-to-track"></a>Windows-registersleutels bijhouden configureren

Gebruik de volgende stappen uit register sleutel bijhouden configureren op Windows-computers:

1. Selecteer in uw Automation-Account **bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het symbool tandwielpictogram).
2. Op de **bijhouden** pagina **Windows-register**, klikt u vervolgens op **+ toevoegen** toevoegen van een nieuwe registersleutel om bij te houden.
3. Op de **Windows-register toevoegen voor het bijhouden**, voer de gegevens voor de sleutel wilt bijhouden en klik op **opslaan**.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Ingeschakeld     | Hiermee wordt bepaald of de instelling is toegepast.        |
|Itemnaam     | Beschrijvende naam van het bestand moet worden gevolgd.        |
|Groep     | De groepsnaam van een voor bestanden logisch groeperen.        |
|Windows-registersleutel   | Het pad naar het bestand controleert. Bijvoorbeeld: 'HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common opstarten'      |

## <a name="limitations"></a>Beperkingen

De oplossing bijhouden biedt momenteel geen ondersteuning voor de volgende items:

* Mappen voor Windows-bestand bijhouden
* Recursie voor Windows-bestand bijhouden
* Jokertekens voor het bijhouden van Windows-bestand
* Recursie voor Windows-register bijhouden
* Padvariabelen
* Netwerk-bestandssystemen
* Bestandsinhoud

Andere beperkingen:

* De **maximale bestandsgrootte** kolom en -waarden zijn niet worden gebruikt in de huidige implementatie.
* Als u meer dan 2500 bestanden in de 30 minuten Verzamelfase bestanden verzamelt, mogelijk tot slechtere met prestaties van de oplossing.
* Wanneer netwerkverkeer hoog is, is het wijzigingsrecords duurt maximaal zes uur om weer te geven.
* Als u de configuratie wijzigen terwijl een computer is uitgeschakeld, kan de computer na de wijzigingen die deel uitmaakten van de vorige configuratie.

## <a name="known-issues"></a>Bekende problemen

De oplossing voor wijzigingen bijhouden is momenteel met de volgende problemen:

* Hotfix-updates worden niet verzameld voor Windows 10 auteurs en Update voor Windows Server 2016 Core RS3 machines.

## <a name="change-tracking-data-collection-details"></a>Details van verzameling gegevens bijhouden wijzigen

De volgende tabel toont de frequentie van de verzameling gegevens voor de soorten wijzigingen. De momentopname van de gegevens van de huidige status wordt ook ten minste elke 24 uur vernieuwd voor elk type:

| **Type wijzigen** | **Frequentie** |
| --- | --- |
| Windows-register | 50 minuten |
| Windows-bestand | 30 minuten |
| Linux-bestand | 15 minuten |
| Windows-services | 30 minuten |
| Daemons Linux | 5 minuten |
| Windows-software | 30 minuten |
| Linux-software | 5 minuten |

### <a name="registry-key-change-tracking"></a>Wijziging van de registersleutel bijhouden

Het doel van het controleren van wijzigingen registersleutels is op de speldenpunt Uitbreidingspunten waar de code van derden en schadelijke software kunnen activeren. De volgende lijst bevat de lijst met vooraf geconfigureerde registersleutels. Deze sleutels worden geconfigureerd, maar niet ingeschakeld. Als u wilt deze registersleutels bijhouden, moet u elk criterium inschakelen.

> [!div class="mx-tdBreakAll"]
> |  |
> |---------|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en meestal uitvoeren in-process met Explorer.exe koppelen.    |
> |**HKEY\_lokale\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors scripts die worden uitgevoerd bij het opstarten.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown**    |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors scripts die worden uitgevoerd bij het afsluiten.     |
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Sleutels die worden geladen voordat de gebruiker zich aanmeldt bij hun Windows-account worden gecontroleerd. De sleutel wordt gebruikt voor 32-bits programma's uitgevoerd op 64-bits computers.    |
> |**HKEY\_lokale\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed onderdelen**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Gecontroleerd op wijzigingen toepassingsinstellingen.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en meestal uitvoeren in-process met Explorer.exe koppelen.|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en meestal uitvoeren in-process met Explorer.exe koppelen.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor pictogram overlay handler registratie.|
|**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor pictogram overlay handler-registratie voor 32-bits programma's uitgevoerd op 64-bits computers.|
> |**HKEY\_lokale\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper-objecten**|
|&nbsp;&nbsp;&nbsp;&nbsp;De monitoren voor nieuwe browser helper object invoegtoepassingen voor Internet Explorer. Gebruikt voor toegang tot het Document Object Model (DOM) van de huidige pagina te bepalen en navigatie.|
> |**HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper-objecten**|
|&nbsp;&nbsp;&nbsp;&nbsp;De monitoren voor nieuwe browser helper object invoegtoepassingen voor Internet Explorer. Gebruikt voor toegang tot het Document Object Model (DOM) van de huidige pagina te bepalen en navigatie voor 32-bits programma's uitgevoerd op 64-bits computers.|
> |**HKEY\_lokale\_MACHINE\Software\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;De monitoren voor nieuwe Internet Explorer-uitbreidingen, zoals aangepaste hulpprogramma's en aangepaste werkbalkknoppen.|
> |**HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;De monitoren voor nieuwe Internet Explorer-uitbreidingen, zoals aangepaste hulpprogramma's en aangepaste werkbalkknoppen voor 32-bits programma's uitgevoerd op 64-bits computers.|
> |**HKEY\_lokale\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;De 32-bits stuurprogramma's die zijn gekoppeld aan wavemapper, wave1 en wave2 msacm.imaadpcm, .msadpcm, .msgsm610 en vidc bewaakt. Vergelijkbaar met de sectie [drivers] in het systeem. INI-bestand.|
> |**HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors de 32-bits stuurprogramma's die zijn gekoppeld aan wavemapper, wave1 en wave2 msacm.imaadpcm, .msadpcm, .msgsm610 en vidc voor 32-bits programma's uitgevoerd op 64-bits computers. Vergelijkbaar met de sectie [drivers] in het systeem. INI-bestand.|
> |**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls**|
|&nbsp;&nbsp;&nbsp;&nbsp;Bewaakt de lijst met bekende of veelgebruikte systeem-dll's; Dit systeem wordt voorkomen dat mensen misbruik van zwakke toepassing mapmachtigingen door slepen en neerzetten in Trojaanse paard versies van systeem-dll's.|
> |**HKEY\_lokale\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify**|
|&nbsp;&nbsp;&nbsp;&nbsp;De lijst met pakketten kunnen ontvangen van meldingen van gebeurtenissen van Winlogon, het model van de interactieve aanmelding ondersteuning voor het besturingssysteem Windows controleert.|

## <a name="use-change-tracking"></a>Gebruik bijhouden

Nadat de oplossing is ingeschakeld, kunt u het overzicht van wijzigingen voor de bewaakte computers weergeven door te selecteren **bijhouden** onder **CONFIGURATIEBEHEER** in uw Automation-account.

U kunt wijzigingen in uw computers en inzoomen naar details voor elke gebeurtenis bekijken. DROP keuzelijsten zijn aan de bovenkant van de grafiek voor het beperken van de grafiek en gedetailleerde informatie op basis van het type- en tijdbereiken wijzigen beschikbaar. U kunt ook Klik en sleep op de grafiek te selecteren van een aangepaste periode.

![afbeelding van dashboard bijhouden](./media/automation-change-tracking/change-tracking-dash01.png)

De gedetailleerde informatie over deze wijziging wordt te klikken op een wijziging of een gebeurtenis. Als u in het voorbeeld zien kunt, is het opstarttype van de service gewijzigd van handmatige in automatisch.

![afbeelding van informatie voor het bijhouden](./media/automation-change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Zoeken in Logboeken

Naast de informatie die beschikbaar zijn in de portal kunnen zoekopdrachten worden gedaan op basis van de logboeken. Met de **bijhouden** pagina openen, klikt u op **logboekanalyse**, Hiermee opent u de **logboek zoeken** pagina.

### <a name="sample-queries"></a>Voorbeeldquery's

De volgende tabel bevat de voorbeeld-logboek zoekt records verzameld door deze oplossing zijn gewijzigd:

|Query’s uitvoeren  |Beschrijving  |
|---------|---------|
|ConfigurationData<br>&#124;waar ConfigDataType == 'WindowsServices' en SvcStartupType == 'Auto'<br>&#124;waar SvcState == 'Gestopt'<br>&#124;overzicht van arg_max(TimeGenerated, *) door SoftwareName, Computer         | Bevat de meest recente inventarisrecords voor Windows-Services die zijn ingesteld op Auto, maar die zijn gerapporteerd als wordt gestopt<br>Resultaten zijn beperkt tot de meest recente record voor die SoftwareName en Computer      |
|Configuratiewijziging<br>&#124;waar ConfigChangeType == 'Software' en ChangeCategory == 'Verwijderd'<br>&#124;order by-TimeGenerated desc|De wijzigingsrecords voor verwijderde software bevat|

## <a name="next-steps"></a>Volgende stappen

Ga naar de zelfstudie over het bijhouden van wijzigingen voor meer informatie over het gebruik van de oplossing:

> [!div class="nextstepaction"]
> [Wijzigingen in uw omgeving oplossen](automation-tutorial-troubleshoot-changes.md)

* Gebruik [zoekopdrachten aanmelden met logboekanalyse](../log-analytics/log-analytics-log-searches.md) om gedetailleerde gegevens bijhouden weer te geven.
