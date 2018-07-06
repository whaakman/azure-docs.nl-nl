---
title: Wijzigingen bijhouden met een Azure Automation
description: De oplossing wijzigingen bijhouden kunt u software en wijzigingen in de Windows-Service die zich in uw omgeving voordoen identificeren.
services: automation
ms.service: automation
ms.component: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 717cf6b2abfb529313699836b790bd3f07844a67
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867950"
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Bijhouden van wijzigingen in uw omgeving met de oplossing wijzigingen bijhouden

Dit artikel helpt u de oplossing wijzigingen bijhouden gebruiken voor het herkennen van wijzigingen in uw omgeving. De oplossing houdt de wijzigingen in Windows en Linux-software, Windows en Linux-bestanden, Windows-registersleutels, Windows-services en Linux-daemons. Wijzigingen in de configuratie te identificeren, kunt u operationele problemen identificeren.

Wijzigingen in de geïnstalleerde software, services voor Windows, Windows-register en bestanden en Linux-daemons op de bewaakte servers worden verzonden naar de Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en de cloudservice registreert de gegevens. Met behulp van de informatie op het dashboard voor wijzigingen bijhouden, kunt u gemakkelijk zien van de wijzigingen die zijn aangebracht in uw serverinfrastructuur.

## <a name="enable-change-tracking-and-inventory"></a>Wijzigingen bijhouden en Inventaris inschakelen

Als u wilt bijhouden van wijzigingen, moet u de oplossing wijzigingen bijhouden en inventaris inschakelen voor uw Automation-Account.

1. Navigeer in de Azure-portal naar uw Automation-Account
1. Selecteer **wijzigingen bijhouden** onder **configuratie**.
1. Selecteer een bestaande Log analytics-werkruimte of **nieuwe werkruimte maken** en klikt u op **inschakelen**.

Hiermee schakelt u de oplossing voor uw automation-account. De oplossing kan maximaal 15 minuten om in te schakelen duren. De blauwe voortgangsbalk waarschuwt u als de oplossing is ingeschakeld. Ga terug naar de **bijhouden** pagina voor het beheren van de oplossing.

## <a name="configuring-change-tracking-and-inventory"></a>Wijzigingen bijhouden en inventaris configureren

Voor meer informatie over hoe onboarding computers op de oplossing gaat u naar: [Onboarding Automation-oplossingen](automation-onboard-solutions-from-automation-account.md). Zodra u de onboarding van een machine met de oplossing wijzigingen bijhouden en inventaris hebt, kunt u de items die u wilt bijhouden kunt configureren. Wanneer u een nieuw bestand of een registersleutel voor het bijhouden van inschakelt, is deze ingeschakeld voor wijzigingen bijhouden en inventaris.

Voor het bijhouden van wijzigingen in bestanden op zowel Windows als Linux, worden MD5-hashes van de bestanden gebruikt. Deze hashes worden vervolgens gebruikt om te detecteren of er een wijziging is aangebracht sinds de laatste inventarisatie.

### <a name="configure-linux-files-to-track"></a>Linux-bestanden bijhouden configureren

Gebruik de volgende stappen voor het configureren van bestand bijhouden op Linux-computers:

1. Selecteer in uw Automation-Account **wijzigingen bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het tandwielsymbool).
2. Op de **bijhouden** weergeeft, schakelt **Linux-bestanden**, klikt u vervolgens op **+ toevoegen** om toe te voegen een nieuw bestand om bij te houden.
3. Op de **Linux-bestand voor wijzigingen bijhouden toevoegen**, voer de gegevens voor het bestand of map bij te houden en klikt u op **opslaan**.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Ingeschakeld     | Hiermee bepaalt u als de instelling wordt toegepast.        |
|Itemnaam     | Beschrijvende naam van het bestand moet worden bijgehouden.        |
|Groep     | De groepsnaam van een voor logisch groeperen van bestanden.        |
|Pad invoeren     | Het pad om te controleren op het bestand. Bijvoorbeeld: "/etc/*.conf"       |
|Padtype     | Type item voor het bijhouden van wijzigingen, mogelijke waarden zijn bestand en map.        |
|Recursie     | Bepaalt of recursie wordt gebruikt bij het zoeken naar het item dat moet worden bijgehouden.        |
|Sudo gebruiken     | Deze instelling bepaalt of sudo wordt gebruikt bij het controleren op het item.         |
|Koppelingen     | Deze instelling bepaalt hoe symbolische koppelingen worden afgehandeld bij het doorlopen van mappen.<br> **Negeren** - symbolische koppelingen worden genegeerd en maakt geen deel uit van de bestanden/mappen waarnaar wordt verwezen.<br>**Ga als volgt** - symbolische koppelingen worden gevolgd tijdens recursie en bevat ook de bestanden/mappen waarnaar wordt verwezen.<br>**Beheren** - symbolische koppelingen worden en kunnen wijzigen van de geretourneerde inhoud.     |
|Bestandsinhoud voor alle instellingen uploaden| Hiermee schakelt u in of uit bestand inhoud te uploaden bijgehouden wijzigingen. Beschikbare opties: **waar** of **False**.|

> [!NOTE]
> Het gebruik van de optie 'Beheren' voor koppelingen wordt niet aanbevolen. Het ophalen van bestandsinhoud wordt niet ondersteund.

### <a name="configure-windows-files-to-track"></a>Windows-bestanden bijhouden configureren

Gebruik de volgende stappen uit om te configureren op Windows-computers bijhouden bestanden:

1. Selecteer in uw Automation-Account **wijzigingen bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het tandwielsymbool).
2. Op de **bijhouden** weergeeft, schakelt **Windows bestanden**, klikt u vervolgens op **+ toevoegen** om toe te voegen een nieuw bestand om bij te houden.
3. Op de **Windows-bestand voor wijzigingen bijhouden toevoegen**, voer de gegevens voor het bestand bij te houden en klikt u op **opslaan**.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Ingeschakeld     | Hiermee bepaalt u als de instelling wordt toegepast.        |
|Itemnaam     | Beschrijvende naam van het bestand moet worden bijgehouden.        |
|Groep     | De groepsnaam van een voor logisch groeperen van bestanden.        |
|Pad invoeren     | Het pad voor het controleren op het bestand, bijvoorbeeld: "C:\temp\myfile.txt"       |
|Bestandsinhoud voor alle instellingen uploaden| Hiermee schakelt u in of uit bestand inhoud te uploaden bijgehouden wijzigingen. Beschikbare opties: **waar** of **False**.|

## <a name="configure-file-content-tracking"></a>Configureren van bestandsinhoud bijhouden

U vindt de inhoud voor en na een wijziging van een bestand met bestand inhoud wijzigingen bijhouden. Dit is beschikbaar voor Windows en Linux-bestanden, voor elke wijziging van het bestand, de inhoud van het bestand is opgeslagen in een storage-account en ziet u het bestand voor en na de wijziging, inline of naast elkaar. Zie voor meer informatie, [weergeven van de inhoud van een bestand bijgehouden](change-tracking-file-contents.md).

![wijzigingen in een bestand weergeven](./media/change-tracking-file-contents/view-file-changes.png)

### <a name="configure-windows-registry-keys-to-track"></a>Windows-registersleutels om bij te houden

Gebruik de volgende stappen voor het configureren van sleutel-register traceren op Windows-computers:

1. Selecteer in uw Automation-Account **wijzigingen bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het tandwielsymbool).
2. Op de **bijhouden** weergeeft, schakelt **Windows-register**, klikt u vervolgens op **+ toevoegen** om toe te voegen een nieuwe registersleutel om bij te houden.
3. Op de **Windows-register voor wijzigingen bijhouden toevoegen**, voer de gegevens voor de sleutel bij te houden en klikt u op **opslaan**.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Ingeschakeld     | Hiermee bepaalt u als de instelling wordt toegepast.        |
|Itemnaam     | Beschrijvende naam van het bestand moet worden bijgehouden.        |
|Groep     | De groepsnaam van een voor logisch groeperen van bestanden.        |
|Windows-registersleutel   | Het pad om te controleren op het bestand. Bijvoorbeeld: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

## <a name="limitations"></a>Beperkingen

De oplossing wijzigingen bijhouden biedt momenteel geen ondersteuning voor de volgende items:

* Mappen voor Windows-bestandstracering
* Recursie voor Windows-bestandstracering
* Jokertekens voor Windows-bestandstracering
* Recursie voor Windows-register bijhouden
* Padvariabelen
* Netwerk-bestandssystemen
* Bestandsinhoud

Andere beperkingen:

* De **maximale bestandsgrootte** kolom en waarden niet worden gebruikt in de huidige implementatie.
* Als u meer dan 2500 bestanden in de verzamelingscyclus van een van 30 minuten hebt verzameld, kan de prestaties van de oplossing worden aangetast.
* Wanneer netwerkverkeer hoog is, kunnen wijzigingsrecords om weer te geven tot zes uur duren.
* Als u de configuratie wijzigen terwijl een computer is uitgeschakeld, kan de computer wijzigingen die deel uitmaakten van de vorige configuratie plaatsen.

## <a name="known-issues"></a>Bekende problemen

De oplossing wijzigingen bijhouden is momenteel de volgende problemen:

* Hotfix-updates worden niet verzameld voor Windows 10 Creators Update en Windows Server 2016 Core RS3 machines.

## <a name="change-tracking-data-collection-details"></a>Traceergegevens gegevens verzameling wijzigen

De volgende tabel ziet u de frequentie van de verzameling gegevens voor de soorten wijzigingen. De momentopname van de gegevens van de huidige status wordt ook ten minste elke 24 uur vernieuwd voor elk type:

| **Type wijzigen** | **Frequentie** |
| --- | --- |
| Windows-register | 50 minuten |
| Windows-bestand | 30 minuten |
| Linux-bestand | 15 minuten |
| Windows-services | 10 seconden tot 30 minuten</br> Standaard: 30 minuten |
| Linux-daemons | 5 minuten |
| Windows-software | 30 minuten |
| Linux-software | 5 minuten |

### <a name="windows-service-tracking"></a>Windows-service bijhouden

De standaardfrequentie van de verzameling voor Windows-services is 30 minuten. Het configureren van de frequentie gaat u naar **bijhouden**. Onder **instellingen bewerken** op de **Windows Services** tabblad, wordt er een schuifregelaar waarmee u de verzamelingsfrequentie voor Windows-services van zo snel tien seconden wijzigen voor zolang als de 30 minuten. De schuifregelaar verplaatsen naar de gewenste frequentie en wordt het automatisch opgeslagen.

![Schuifregelaar voor Windows-services](./media/automation-change-tracking/windowservices.png)

De agent alleen wijzigingen worden bijgehouden, dit optimaliseert de prestaties van de agent. Door in te stellen, te hoge drempel worden wijzigingen overgeslagen als de service is hersteld naar de oorspronkelijke staat. De frequentie instellen op een kleinere waarde, kunt u om af te vangen van wijzigingen die anders mogelijk worden gemist.

> [!NOTE]
> Terwijl de agent kunt wijzigingen bijhouden op een 10 tweede interval, de gegevens nog steeds duurt een paar minuten moet worden weergegeven in de portal. Wijzigingen gedurende de tijd om weer te geven in de portal worden nog steeds worden bijgehouden en geregistreerd.
  
### <a name="registry-key-change-tracking"></a>Belangrijke wijziging in het register bijhouden

Het doel van de bewaking van wijzigingen in de registersleutels is op de speldenpunt uitbreidbaarheidspunten waarin code van derden en schadelijke software kunnen activeren. De volgende lijst bevat de lijst met vooraf geconfigureerde registersleutels. Deze sleutels zijn geconfigureerd, maar niet is ingeschakeld. Voor het volgen van deze registersleutels, moet u elk inschakelen.

> [!div class="mx-tdBreakAll"]
> |  |
> |---------|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en gewoonlijk uitgevoerd in-process met Explorer.exe van een toepassing aansluiten.    |
> |**HKEY\_lokale\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors scripts die worden uitgevoerd bij het opstarten.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown**    |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors scripts die worden uitgevoerd bij het afsluiten.     |
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors sleutels die worden geladen voordat de gebruiker zich aanmeldt bij Azure aan het Windows-account. De sleutel wordt gebruikt voor 32-bits programma's die worden uitgevoerd op 64-bits computers.    |
> |**HKEY\_lokale\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed onderdelen**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors wijzigingen in de toepassingsinstellingen van de.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en gewoonlijk uitgevoerd in-process met Explorer.exe van een toepassing aansluiten.|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en gewoonlijk uitgevoerd in-process met Explorer.exe van een toepassing aansluiten.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor pictogram overlay handler registratie.|
|**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor pictogram overlay registratie-handler voor 32-bits programma's die worden uitgevoerd op 64-bits computers.|
> |**HKEY\_lokale\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper-objecten**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor nieuwe browser helper object invoegtoepassingen voor Internet Explorer. Gebruikt voor toegang tot het Document Object Model (DOM) van de huidige pagina en de navigatie.|
> |**HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper-objecten**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor nieuwe browser helper object invoegtoepassingen voor Internet Explorer. Gebruikt voor toegang tot het Document Object Model (DOM) van de huidige pagina en de navigatie voor 32-bits programma's die worden uitgevoerd op 64-bits computers.|
> |**HKEY\_lokale\_MACHINE\Software\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor nieuwe Internet Explorer-uitbreidingen, zoals aangepaste hulpprogramma's en aangepaste knoppen.|
> |**HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor nieuwe Internet Explorer-uitbreidingen, zoals aangepaste hulpprogramma's en aangepaste knoppen voor 32-bits programma's die worden uitgevoerd op 64-bits computers.|
> |**HKEY\_lokale\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;De 32-bits stuurprogramma's die zijn gekoppeld aan wavemapper, wave1 en wave2 msacm.imaadpcm, .msadpcm, .msgsm610 en vidc bewaakt. Dit is vergelijkbaar met de sectie [drivers] in het systeem. INI-bestand.|
> |**HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors de 32-bits stuurprogramma's die zijn gekoppeld aan wavemapper, wave1 en wave2 msacm.imaadpcm, .msadpcm, .msgsm610 en vidc voor 32-bits programma's die worden uitgevoerd op 64-bits computers. Dit is vergelijkbaar met de sectie [drivers] in het systeem. INI-bestand.|
> |**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls**|
|&nbsp;&nbsp;&nbsp;&nbsp;Controleert de lijst met bekende of gebruikte systeem-dll's; Dit systeem wordt voorkomen dat mensen misbruik van zwakke Toepassingsmachtigingen voor directory door slepen en neerzetten in Trojaans paard versies van systeem-dll's.|
> |**HKEY\_lokale\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify**|
|&nbsp;&nbsp;&nbsp;&nbsp;De lijst met pakketten kunnen ontvangen van meldingen van gebeurtenissen van Winlogon, het model van de ondersteuning voor interactief aanmelden voor het besturingssysteem Windows controleert.|

## <a name="use-change-tracking"></a>Gebruik bijhouden

Nadat de oplossing is ingeschakeld, kunt u het overzicht van wijzigingen voor de bewaakte computers weergeven door te selecteren **bijhouden** onder **CONFIGURATIEBEHEER** in uw Automation-account.

U kunt wijzigingen in uw computers en inzoomen naar details voor elke gebeurtenis bekijken. Vervolgkeuzelijsten zijn beschikbaar aan de bovenkant van de grafiek om te beperken van de grafiek en gedetailleerde informatie op basis van het type en adresbereiken wijzigen. U kunt ook Klik en sleep in de grafiek om te selecteren van een aangepast tijdsbereik.

![afbeelding van dashboard wijzigingen bijhouden](./media/automation-change-tracking/change-tracking-dash01.png)

De gedetailleerde informatie over deze wijziging wordt te klikken op een wijziging of de gebeurtenis. Zoals u in het voorbeeld zien kunt, is het opstarttype van de service van de handmatig gewijzigd in automatisch.

![afbeelding van het bijhouden van informatie](./media/automation-change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Zoeken in Logboeken

Naast de informatie die beschikbaar zijn in de portal kunnen zoekopdrachten worden gedaan op basis van de logboeken. Met de **bijhouden** pagina openen, klikt u op **Log Analytics**, Hiermee opent u de **zoeken in logboeken** pagina.

### <a name="sample-queries"></a>Voorbeeldquery's

De volgende tabel bevat voorbeelden van zoekopdrachten voor records die zijn verzameld door deze oplossing wijzigen:

|Query’s uitvoeren  |Beschrijving  |
|---------|---------|
|ConfigurationData<br>&#124;waar ConfigDataType == "WindowsServices" en SvcStartupType == "Automatisch"<br>&#124;waar SvcState == 'Stopped'<br>&#124;samenvatten arg_max(TimeGenerated, *) door SoftwareName, Computer         | Weergegeven met de meest recente inventarisrecords voor Windows-Services die zijn ingesteld op automatisch, maar zijn gerapporteerd als wordt gestopt<br>Resultaten zijn beperkt tot de meest recente record voor die softwarenaam en op Computer      |
|ConfigurationChange<br>&#124;waar ConfigChangeType == 'Software' en ChangeCategory == 'Verwijderd'<br>&#124;order by-TimeGenerated desc|Ziet u de wijzigingsrecords voor de verwijderde software|

## <a name="next-steps"></a>Volgende stappen

Ga naar de zelfstudie over het bijhouden van wijzigingen voor meer informatie over het gebruik van de oplossing:

> [!div class="nextstepaction"]
> [Problemen met wijzigingen in uw omgeving oplossen](automation-tutorial-troubleshoot-changes.md)

* Gebruik [zoekopdrachten in Logboeken in Log Analytics](../log-analytics/log-analytics-log-searches.md) om gedetailleerde bijhouden van gegevens weer te geven.
