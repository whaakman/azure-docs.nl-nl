---
title: Wijzigingen bijhouden met een Azure Automation
description: De oplossing wijzigingen bijhouden kunt u software en wijzigingen in de Windows-Service die zich in uw omgeving voordoen identificeren.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 02/19/2019
ms.topic: conceptual
manager: carmonm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 07fd8c41e7817e232513ed9a260c3722a1fdac11
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429261"
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Bijhouden van wijzigingen in uw omgeving met de oplossing wijzigingen bijhouden

Dit artikel helpt u de oplossing wijzigingen bijhouden gebruiken voor het herkennen van wijzigingen in uw omgeving. De oplossing houdt de wijzigingen in Windows en Linux-software, Windows en Linux-bestanden, Windows-registersleutels, Windows-services en Linux-daemons. Wijzigingen in de configuratie te identificeren, kunt u operationele problemen identificeren.

Wijzigingen in de geïnstalleerde software, services voor Windows, Windows-register en bestanden en Linux-daemons op de bewaakte servers worden verzonden naar de Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en de cloudservice registreert de gegevens. Met behulp van de informatie op het dashboard voor wijzigingen bijhouden, kunt u gemakkelijk zien van de wijzigingen die zijn aangebracht in uw serverinfrastructuur.

## <a name="supported-windows-operating-systems"></a>Ondersteunde Windows-besturingssystemen

De volgende versies van het Windows-besturingssysteem worden officieel ondersteund voor de Windows-agent:

* Windows Server 2008 R2 of hoger

## <a name="supported-linux-operating-systems"></a>Ondersteunde Linux-besturingssystemen

De volgende Linux-distributies zijn officieel ondersteund. De Linux-agent kan echter ook uitvoeren in een andere distributie niet wordt vermeld. Tenzij anders vermeld, worden alle secundaire versies worden ondersteund voor elke primaire versie weergegeven.  

### <a name="64-bit"></a>64-bits

* CentOS 6 en 7
* Amazon Linux 2017.09
* Oracle Linux 6 en 7
* Red Hat Enterprise Linux Server 6 and 7
* Debian GNU/Linux 8 en 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS en 18.04 LTS
* SUSE Linux Enterprise Server 12

### <a name="32-bit"></a>32-bits

* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 en 9
* Ubuntu Linux 14.04 LTS en 16.04 LTS

## <a name="onboard"></a>Wijzigingen bijhouden en inventaris inschakelen

Als u wilt bijhouden van wijzigingen, moet u de oplossing wijzigingen bijhouden en inventaris inschakelen. Er zijn veel manieren om Onboarding van machines voor wijzigingen bijhouden en inventaris. De volgende zijn de aanbevolen en manieren waarop u de oplossing wordt ondersteund.

* [Van een virtuele machine](automation-onboard-solutions-from-vm.md)
* [Vanuit het bladeren door meerdere virtuele machines](automation-onboard-solutions-from-browse.md)
* [Via uw Automation-account](automation-onboard-solutions-from-automation-account.md)
* [Met een Azure Automation-runbook](automation-onboard-solutions.md)

## <a name="configuring-change-tracking-and-inventory"></a>Wijzigingen bijhouden en inventaris configureren

Voor meer informatie over hoe onboarding computers op de oplossing gaat u naar: [Onboarding-Automation-oplossingen](automation-onboard-solutions-from-automation-account.md). Zodra u de onboarding van een machine met de oplossing wijzigingen bijhouden en inventaris hebt, kunt u de items die u wilt bijhouden. Wanneer u een nieuw bestand of een registersleutel voor het bijhouden van inschakelt, is deze ingeschakeld voor wijzigingen bijhouden en inventaris.

Voor het bijhouden van wijzigingen in bestanden op zowel Windows als Linux, worden MD5-hashes van de bestanden gebruikt. Deze hashes worden vervolgens gebruikt om te detecteren of er een wijziging is aangebracht sinds de laatste inventarisatie.

### <a name="configure-linux-files-to-track"></a>Linux-bestanden bijhouden configureren

Gebruik de volgende stappen voor het configureren van bestand bijhouden op Linux-computers:

1. Selecteer in uw Automation-Account **wijzigingen bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het tandwielsymbool).
2. Op de **bijhouden** weergeeft, schakelt **Linux-bestanden**, klikt u vervolgens op **+ toevoegen** om toe te voegen een nieuw bestand om bij te houden.
3. Op de **Linux-bestand voor wijzigingen bijhouden toevoegen**, voer de gegevens voor het bestand of map bij te houden en klikt u op **opslaan**.

|Eigenschap  |Description  |
|---------|---------|
|Ingeschakeld     | Hiermee bepaalt u als de instelling wordt toegepast.        |
|Itemnaam     | Beschrijvende naam van het bestand moet worden bijgehouden.        |
|Groep     | De groepsnaam van een voor logisch groeperen van bestanden.        |
|Pad invoeren     | Het pad om te controleren op het bestand. Bijvoorbeeld: "/etc/*.conf"       |
|Padtype     | Type item voor het bijhouden van wijzigingen, mogelijke waarden zijn bestand en map.        |
|Recursie     | Bepaalt of recursie wordt gebruikt bij het zoeken naar het item dat moet worden bijgehouden.        |
|Sudo gebruiken     | Deze instelling bepaalt of sudo wordt gebruikt bij het controleren op het item.         |
|Koppelingen     | Deze instelling bepaalt hoe symbolische koppelingen worden afgehandeld bij het doorlopen van mappen.<br> **Negeren** - symbolische koppelingen worden genegeerd en de bestanden/mappen waarnaar wordt verwezen niet bevat.<br>**Ga als volgt** - symbolische koppelingen worden gevolgd tijdens recursie en bevat ook de bestanden/mappen waarnaar wordt verwezen.<br>**Beheren** - symbolische koppelingen worden en kunnen wijzigen van de geretourneerde inhoud.     |
|Bestandsinhoud uploaden voor alle instellingen| Schakelt uploaden van bestandsinhoud bij bijgehouden wijzigingen in of uit. Beschikbare opties: **Waar** of **Onwaar**.|

> [!NOTE]
> Het gebruik van de optie 'Beheren' voor koppelingen wordt niet aanbevolen. Het ophalen van bestandsinhoud wordt niet ondersteund.

### <a name="configure-windows-files-to-track"></a>Windows-bestanden bijhouden configureren

Gebruik de volgende stappen uit om te configureren op Windows-computers bijhouden bestanden:

1. Selecteer in uw Automation-Account **wijzigingen bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het tandwielsymbool).
2. Op de **bijhouden** weergeeft, schakelt **Windows bestanden**, klikt u vervolgens op **+ toevoegen** om toe te voegen een nieuw bestand om bij te houden.
3. Op de **Windows-bestand voor wijzigingen bijhouden toevoegen**, voer de gegevens voor het bestand bij te houden en klikt u op **opslaan**.

|Eigenschap  |Description  |
|---------|---------|
|Ingeschakeld     | Hiermee bepaalt u als de instelling wordt toegepast.        |
|Itemnaam     | Beschrijvende naam van het bestand moet worden bijgehouden.        |
|Groep     | De groepsnaam van een voor logisch groeperen van bestanden.        |
|Pad invoeren     | Het pad voor het controleren op het bestand, bijvoorbeeld: "C:\temp\\\*.txt"<br>U kunt ook omgevingsvariabelen gebruiken zoals ' %winDir%\System32\\\*. * "       |
|Recursie     | Bepaalt of recursie wordt gebruikt bij het zoeken naar het item dat moet worden bijgehouden.        |
|Bestandsinhoud uploaden voor alle instellingen| Schakelt uploaden van bestandsinhoud bij bijgehouden wijzigingen in of uit. Beschikbare opties: **Waar** of **Onwaar**.|

## <a name="wildcard-recursion-and-environment-settings"></a>Jokertekens recursie en omgeving instellingen

Recursie kunt u opgeven van jokertekens voor het vereenvoudigen van alle mappen en omgevingsvariabelen waarmee u kunt bestanden bijhouden in omgevingen met meerdere bij te houden of een dynamische schijf namen. De volgende lijst bevat algemene informatie over het bij het configureren van recursie:

* Jokertekens zijn vereist voor het bijhouden van meerdere bestanden
* Als u jokertekens gebruikt, kunnen ze alleen worden gebruikt in het laatste segment van een pad. (zoals `c:\folder\*file*` of `/etc/*.conf`)
* Als een omgevingsvariabele heeft een ongeldig pad, de validatie slaagt maar het opgegeven pad zal mislukken wanneer inventarisatie wordt uitgevoerd.
* Algemene paden zoals voorkomen `c:\*.*` bij het instellen van het pad, aangezien dit zou leiden tot te veel mappen wordt gebruikt.

## <a name="configure-file-content-tracking"></a>Configureren van bestandsinhoud bijhouden

U kunt de inhoud bekijken voor en na een wijziging van een bestand met bestand inhoud wijzigingen bijhouden. Dit is beschikbaar voor Windows en Linux-bestanden, voor elke wijziging van het bestand, de inhoud van het bestand is opgeslagen in een storage-account en ziet u het bestand voor en na de wijziging, inline of naast elkaar. Zie voor meer informatie, [weergeven van de inhoud van een bestand bijgehouden](change-tracking-file-contents.md).

![wijzigingen in een bestand weergeven](./media/change-tracking-file-contents/view-file-changes.png)

### <a name="configure-windows-registry-keys-to-track"></a>Windows-registersleutels om bij te houden

Gebruik de volgende stappen voor het configureren van sleutel-register traceren op Windows-computers:

1. Selecteer in uw Automation-Account **wijzigingen bijhouden** onder **CONFIGURATIEBEHEER**. Klik op **instellingen bewerken** (het tandwielsymbool).
2. Op de **bijhouden** weergeeft, schakelt **Windows-register**, klikt u vervolgens op **+ toevoegen** om toe te voegen een nieuwe registersleutel om bij te houden.
3. Op de **Windows-register voor wijzigingen bijhouden toevoegen**, voer de gegevens voor de sleutel bij te houden en klikt u op **opslaan**.

|Eigenschap  |Description  |
|---------|---------|
|Ingeschakeld     | Hiermee bepaalt u als de instelling wordt toegepast.        |
|Itemnaam     | Beschrijvende naam van de registersleutel moet worden bijgehouden.        |
|Groep     | De groepsnaam van een voor logisch groeperen van registersleutels.        |
|Windows-registersleutel   | Het pad naar het controleren van de registersleutel. Bijvoorbeeld: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup      |

## <a name="limitations"></a>Beperkingen

De oplossing wijzigingen bijhouden niet wordt momenteel ondersteund in de volgende items:

* Recursie voor Windows-register bijhouden
* Netwerk-bestandssystemen

Andere beperkingen:

* De **maximale bestandsgrootte** kolom en waarden niet worden gebruikt in de huidige implementatie.
* Als u meer dan 2500 bestanden in de verzamelingscyclus van een van 30 minuten hebt verzameld, kan de prestaties van de oplossing worden aangetast.
* Wanneer netwerkverkeer hoog is, kunnen wijzigingsrecords om weer te geven tot zes uur duren.
* Als u de configuratie wijzigen terwijl een computer is uitgeschakeld, kan de computer wijzigingen die deel uitmaakten van de vorige configuratie plaatsen.

## <a name="known-issues"></a>Bekende problemen

De oplossing wijzigingen bijhouden is momenteel de volgende problemen:

* Hotfix-updates worden niet verzameld op Windows Server 2016 Core RS3 virtuele machines.

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

De volgende tabel bevat de limieten bijgehouden item per machine voor het bijhouden.

| **Resource** | **Limiet**| **Opmerkingen** |
|---|---|---|
|File|500||
|Register|250||
|Windows-software|250|Software-updates bevat geen|
|Linux-pakketten|1250||
|Services|250||
|Daemon|250||

Het gemiddelde gebruik van Log Analytics-gegevens voor een virtuele machine met wijzigingen bijhouden en inventaris is ongeveer 40MB per maand. Deze waarde is slechts een schatting en kan worden gewijzigd op basis van uw omgeving. Het raadzaam dat u uw omgeving om te zien van de exacte syntaxis die u hebt gecontroleerd.

### <a name="windows-service-tracking"></a>Windows-service bijhouden

De standaardfrequentie van de verzameling voor Windows-services is 30 minuten. Voor het configureren van de frequentie, gaat u naar **bijhouden**. Onder **instellingen bewerken** op de **Windows Services** tabblad, wordt er een schuifregelaar waarmee u de verzamelingsfrequentie voor Windows-services van zo snel tien seconden wijzigen voor zolang als de 30 minuten. De schuifregelaar verplaatsen naar de gewenste frequentie en wordt het automatisch opgeslagen.

![Schuifregelaar voor Windows-services](./media/automation-change-tracking/windowservices.png)

De agent alleen wijzigingen worden bijgehouden, dit optimaliseert de prestaties van de agent. Een hoge drempel mogelijk wijzigingen gemist als de service is hersteld naar de oorspronkelijke staat. De frequentie instellen op een kleinere waarde, kunt u om af te vangen van wijzigingen die anders mogelijk worden gemist.

> [!NOTE]
> Terwijl de agent kunt wijzigingen bijhouden op een 10 tweede interval, de gegevens nog steeds duurt een paar minuten moet worden weergegeven in de portal. Wijzigingen gedurende de tijd om weer te geven in de portal worden nog steeds worden bijgehouden en geregistreerd.
  
### <a name="registry-key-change-tracking"></a>Belangrijke wijziging in het register bijhouden

Het doel van de bewaking van wijzigingen in de registersleutels is op de speldenpunt uitbreidbaarheidspunten waarin code van derden en schadelijke software kunnen activeren. De volgende lijst bevat de lijst met vooraf geconfigureerde registersleutels. Deze sleutels zijn geconfigureerd, maar niet is ingeschakeld. Voor het volgen van deze registersleutels, moet u elk inschakelen.

> [!div class="mx-tdBreakAll"]
> |  |
> |---------|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en gewoonlijk uitgevoerd in-process met Explorer.exe van een toepassing aansluiten.    |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup**     |
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
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor nieuwe Internet Explorer-uitbreidingen, zoals aangepaste hulpprogramma's en aangepaste knoppen.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors voor nieuwe Internet Explorer-uitbreidingen, zoals aangepaste hulpprogramma's en aangepaste knoppen voor 32-bits programma's die worden uitgevoerd op 64-bits computers.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;De 32-bits stuurprogramma's die zijn gekoppeld aan wavemapper, wave1 en wave2 msacm.imaadpcm, .msadpcm, .msgsm610 en vidc bewaakt. Dit is vergelijkbaar met de sectie [drivers] in het systeem. INI-bestand.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitors de 32-bits stuurprogramma's die zijn gekoppeld aan wavemapper, wave1 en wave2 msacm.imaadpcm, .msadpcm, .msgsm610 en vidc voor 32-bits programma's die worden uitgevoerd op 64-bits computers. Dit is vergelijkbaar met de sectie [drivers] in het systeem. INI-bestand.|
> |**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls**|
|&nbsp;&nbsp;&nbsp;&nbsp;Controleert de lijst met bekende of gebruikte systeem-dll's; Dit systeem wordt voorkomen dat mensen misbruik van zwakke Toepassingsmachtigingen voor directory door slepen en neerzetten in Trojaans paard versies van systeem-dll's.|
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify**|
|&nbsp;&nbsp;&nbsp;&nbsp;De lijst met pakketten kunnen ontvangen van meldingen van gebeurtenissen van Winlogon, het model van de ondersteuning voor interactief aanmelden voor het besturingssysteem Windows controleert.|

## <a name="network-requirements"></a>Netwerkvereisten

De volgende adressen moeten specifiek voor wijzigingen bijhouden. Communicatie met deze adressen wordt gedaan via poort 443.

|Azure Public  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

## <a name="use-change-tracking"></a>Gebruik bijhouden

Nadat de oplossing is ingeschakeld, kunt u het overzicht van wijzigingen voor de bewaakte computers weergeven door te selecteren **bijhouden** onder **CONFIGURATIEBEHEER** in uw Automation-account.

U kunt wijzigingen in uw computers en inzoomen naar details voor elke gebeurtenis bekijken. Vervolgkeuzelijsten zijn beschikbaar aan de bovenkant van de grafiek om te beperken van de grafiek en gedetailleerde informatie op basis van het type en adresbereiken wijzigen. U kunt ook Klik en sleep in de grafiek om te selecteren van een aangepast tijdsbereik.

![afbeelding van dashboard wijzigingen bijhouden](./media/automation-change-tracking/change-tracking-dash01.png)

De gedetailleerde informatie over deze wijziging wordt te klikken op een wijziging of de gebeurtenis. Zoals u in het voorbeeld zien kunt, is het opstarttype van de service van de handmatig gewijzigd in automatisch.

![afbeelding van het bijhouden van informatie](./media/automation-change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Logboeken zoeken

Naast de informatie die beschikbaar zijn in de portal kunnen zoekopdrachten worden gedaan op basis van de logboeken. Met de **bijhouden** pagina openen, klikt u op **Log Analytics**, Hiermee opent u de **zoeken in logboeken** pagina.

### <a name="sample-queries"></a>Voorbeeldquery's

De volgende tabel bevat voorbeelden van zoekopdrachten voor records die zijn verzameld door deze oplossing wijzigen:

|Query’s uitvoeren  |Description  |
|---------|---------|
|ConfigurationData<br>&#124;waar ConfigDataType == "WindowsServices" en SvcStartupType == "Automatisch"<br>&#124;waar SvcState == 'Stopped'<br>&#124;samenvatten arg_max(TimeGenerated, *) door SoftwareName, Computer         | Weergegeven met de meest recente inventarisrecords voor Windows-Services die zijn ingesteld op automatisch, maar zijn gerapporteerd als wordt gestopt<br>Resultaten zijn beperkt tot de meest recente record voor die softwarenaam en op Computer      |
|ConfigurationChange<br>&#124;waar ConfigChangeType == 'Software' en ChangeCategory == 'Verwijderd'<br>&#124;order by-TimeGenerated desc|Ziet u de wijzigingsrecords voor de verwijderde software|

## <a name="alert-on-changes"></a>Waarschuwing bij wijzigingen

Een belangrijke functie van wijzigingen bijhouden en inventaris is de mogelijkheid om te waarschuwen wanneer de status van de configuratie en eventuele wijzigingen in de status van de configuratie van uw hybride omgeving.  

In het volgende voorbeeld wordt de schermafbeelding ziet u dat het bestand `C:\windows\system32\drivers\etc\hosts` op een virtuele machine is gewijzigd. Dit bestand is belangrijk omdat het Hosts-bestand wordt gebruikt door Windows voor het omzetten van hostnamen naar IP-adressen en voorrang boven zelfs DNS, hetgeen in problemen met de netwerkverbinding of het omleiden van verkeer naar schadelijke of anderszins schadelijke websites resulteren kan.

![Een grafiek met de hosts bestand wijzigen](./media/automation-change-tracking/changes.png)

Voor het analyseren van deze wijziging verder, gaat u naar zoeken in Logboeken van het klikken op **Log Analytics**. Eenmaal in zoeken in Logboeken, zoeken naar inhoud die is gewijzigd in het Hosts-bestand met de query `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`. Deze query zoekt naar wijzigingen die een wijziging van de bestandsinhoud voor bestanden waarvan het volledig gekwalificeerde pad het woord "hosts bevat" opgenomen. U kunt ook vragen voor een bepaald bestand door het wijzigen van het padgedeelte in de volledig gekwalificeerde vorm (zoals `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`).

Nadat de query de gewenste resultaten retourneert, klikt u op de **nieuwe waarschuwingsregel** knop in het logboek zoekervaring wordt geboden voor het openen van de pagina voor het maken van waarschuwingen. U ook naar deze ervaring via kan navigeren **Azure Monitor** in Azure portal. In de ervaring van het maken van waarschuwingen, onze query opnieuw controleren en wijzigen van de waarschuwing logica. In dit geval wilt u de waarschuwing wordt geactiveerd als er een wijziging gedetecteerd voor alle machines in de omgeving.

![Een installatiekopie van de query wijzigen voor bijhouden in het hosts-bestand wijzigingen](./media/automation-change-tracking/change-query.png)

Nadat de voorwaardelijke logica is ingesteld, toewijzen actiegroepen acties uit te voeren in reactie op de waarschuwing wordt geactiveerd. In dit geval ingestelde ik e-mailberichten worden verzonden en een ITSM-ticket moet worden gemaakt.  Veel andere handige acties kunnen ook worden uitgevoerd, zoals de activering van een Azure-functie, Automation-runbook, webhook of logische App.

![Een installatiekopie van een actiegroep op waarschuwing configureren op de wijziging](./media/automation-change-tracking/action-groups.png)

Nadat alle parameters en logica zijn ingesteld, kunnen we de waarschuwing toepassen op de omgeving.

### <a name="alert-suggestions"></a>Waarschuwing suggesties

Waarschuwingen over wijzigingen in het Hosts-bestand is een goede toepassing van waarschuwingen voor wijzigingen bijhouden en inventaris gegevens, maar er zijn veel meer scenario's voor waarschuwingen, met inbegrip van de gevallen gedefinieerd samen met hun voorbeelden van query's in de onderstaande sectie.

|Query’s uitvoeren  |Description  |
|---------|---------|
|ConfigurationChange <br>&#124;waar ConfigChangeType == 'Files' en FileSystemPath bevat "c:\\windows\\system32\\stuurprogramma's\\"|Handig voor het bijhouden van wijzigingen in essentiële systeembestanden|
|ConfigurationChange <br>&#124;waar FieldsChanged bevat "FileContentChecksum" en FileSystemPath == "c:\\windows\\system32\\stuurprogramma's\\enzovoort\\hosts"|Handig voor het bijhouden van wijzigingen in belangrijke configuratiebestanden|
|ConfigurationChange <br>&#124;waar ConfigChangeType == "WindowsServices" en SvcName bevat "w3svc" en SvcState == "Stopped"|Handig voor het bijhouden van wijzigingen in kritieke systeemservices|
|ConfigurationChange <br>&#124;waar ConfigChangeType == "Daemons" en SvcName bevat 'ssh' en SvcState! = "Uitvoeren"|Handig voor het bijhouden van wijzigingen in kritieke systeemservices|
|ConfigurationChange <br>&#124;waar ConfigChangeType == 'Software' en ChangeCategory == "Toevoegen"|Handig voor omgevingen die moeten worden beveiligd softwareconfiguraties|
|ConfigurationData <br>&#124;waar SoftwareName bevat 'Monitoring Agent' en CurrentVersion! = "8.0.11081.0"|Nuttig als u wilt zien welke computers een verouderde of niet-compatibele softwareversie geïnstalleerd hebt. De laatste status van de gerapporteerde configuratie, niet de wijzigingen worden gerapporteerd.|
|ConfigurationChange <br>&#124;waar RegistryKey == "HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\QualityCompat"| Handig voor het bijhouden van wijzigingen in belangrijke antivirusprogramma's sleutels|
|ConfigurationChange <br>&#124;waar RegistryKey bevat "HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\SharedAccess\\Parameters\\FirewallPolicy"| Handig voor het bijhouden van wijzigingen in de firewall-instellingen|

## <a name="next-steps"></a>Volgende stappen

Ga naar de zelfstudie over het bijhouden van wijzigingen voor meer informatie over het gebruik van de oplossing:

> [!div class="nextstepaction"]
> [Problemen met wijzigingen in uw omgeving oplossen](automation-tutorial-troubleshoot-changes.md)

* Gebruik [zoekopdrachten in Logboeken in Log Analytics](../log-analytics/log-analytics-log-searches.md) om gedetailleerde bijhouden van gegevens weer te geven.

