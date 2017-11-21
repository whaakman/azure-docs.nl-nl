---
title: Wijzigingen bijhouden met Azure Log Analytics | Microsoft Docs
description: De oplossing voor wijzigingen bijhouden in Log Analytics kunt u identificeren van software- en Windows-Service-wijzigingen die in uw omgeving plaatsvinden.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 528b569ff9ffb3659e9210ea70e3aa06921cfe0d
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>Bijhouden van wijzigingen in de software in uw omgeving met de oplossing voor wijzigingen bijhouden

![Bijhouden symbool wijzigen](./media/log-analytics-change-tracking/change-tracking-symbol.png)

In dit artikel helpt u de oplossing voor wijzigingen bijhouden in Log Analytics gebruiken eenvoudig om wijzigingen te bepalen in uw omgeving. De oplossing houdt de wijzigingen voor Windows en Linux-software, Windows-bestanden en registersleutels, services voor Windows en Linux-daemons. Wijzigingen in de configuratie te identificeren, kunt u operationele problemen te lokaliseren.

U installeert de oplossing voor het bijwerken van het type van de agent die u hebt geïnstalleerd. Wijzigingen in geïnstalleerde software, services voor Windows en Linux-daemons op de bewaakte servers worden gelezen. Vervolgens wordt de gegevens verzonden naar de Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en registreert de gegevens van de cloudservice. Met behulp van de informatie op het dashboard bijhouden, kunt u eenvoudig de wijzigingen die zijn aangebracht in uw serverinfrastructuur bekijken.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om te installeren en configureren van de oplossing.

* U moet hebben een [Windows](log-analytics-windows-agents.md), [Operations Manager](log-analytics-om-agents.md), of [Linux](log-analytics-linux-agents.md) -agent op elke computer waarop u wilt bewaken van wijzigingen.
* De oplossing voor wijzigingen bijhouden toevoegen aan uw OMS-werkruimte van de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview). U kunt ook de oplossing met de informatie in toevoegen [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md). Er is geen verdere configuratie vereist.

### <a name="configure-linux-files-to-track"></a>Linux-bestanden bijhouden configureren
Gebruik de volgende stappen voor het configureren van de bestanden bijhouden op Linux-computers.

1. Klik in de OMS-portal op **instellingen** (het symbool tandwielpictogram).
2. Op de **instellingen** pagina, klikt u op **gegevens**, en klik vervolgens op **Linux File Tracking**.
3. Typ onder Linux bestand bijhouden, het gehele pad, inclusief de bestandsnaam van het bestand dat u wilt bijhouden en klik vervolgens op de **toevoegen** symbool. Bijvoorbeeld: '/etc/*.conf'
4. Klik op **Opslaan**.  

> [!NOTE]
> Linux-bestand bijhouden heeft aanvullende mogelijkheden, waaronder directory bijhouden, recursie via mappen en de jokertekens bijhouden.

### <a name="configure-windows-files-to-track"></a>Windows-bestanden bijhouden configureren
Gebruik de volgende stappen voor het configureren van de bestanden in de Windows-computers bij te houden.

1. Klik in de OMS-portal op **instellingen** (het symbool tandwielpictogram).
2. Op de **instellingen** pagina, klikt u op **gegevens**, en klik vervolgens op **Windows File Tracking**.
3. Typ onder Windows bestand bijhouden, het gehele pad, inclusief de bestandsnaam van het bestand dat u wilt bijhouden en klik vervolgens op de **toevoegen** symbool. Bijvoorbeeld: C:\Program Files (x86) \Internet Explorer\iexplore.exe of C:\Windows\System32\drivers\etc\hosts.
4. Klik op **Opslaan**.  
   ![Windows-bestand met bijhouden](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="configure-windows-registry-keys-to-track"></a>Windows-registersleutels bijhouden configureren
Gebruik de volgende stappen voor het configureren van registersleutels om bij te houden op Windows-computers.

1. Klik in de OMS-portal op **instellingen** (het symbool tandwielpictogram).
2. Op de **instellingen** pagina, klikt u op **gegevens**, en klik vervolgens op **Windows-register bijhouden**.
3. Typ onder Windows-register bijhouden, de gehele sleutel die u wilt bijhouden en klik vervolgens op de **toevoegen** symbool.
4. Klik op **Opslaan**.  
   ![Windows-register bijhouden](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### <a name="explanation-of-linux-file-collection-properties"></a>Uitleg van de verzameling van Linux bestandseigenschappen
1. **Type**
   * **Bestand** (rapport bestandsmetagegevens - grootte, wijzigingsdatum, hash, enz.)
   * **Directory** (rapport directory metagegevens - grootte, wijzigingsdatum, enz.)
2. **Koppelingen** (afhandeling van Linux symlink verwijzingen naar andere bestanden of mappen)
   * **Negeren** (symlinks negeren tijdens recursie niet op te nemen de bestanden/mappen waarnaar wordt verwezen)
   * **Ga als volgt** (Ga als volgt de symlinks tijdens recursie naar alle bestanden/mappen waarnaar wordt verwezen)
   * **Beheren** (Ga als volgt de symlinks en de behandeling van geretourneerde inhoud alter)

   > [!NOTE]   
   > De optie 'Beheren' koppelingen wordt niet aanbevolen. Inhoud ophalen van bestand wordt niet ondersteund.

3. **Recurse** (Recurse via niveaus van de map en alle bestanden die voldoen aan de padinstructie bijhouden)
4. **Sudo** (voor het inschakelen van toegang tot bestanden of mappen die sudo bevoegdheid vereist)

### <a name="limitations"></a>Beperkingen
De oplossing bijhouden biedt momenteel geen ondersteuning voor de volgende items:

* Mappen voor het bijhouden van de Windows-bestand
* Recursie voor Windows-bestand bijhouden
* Jokertekens voor het bijhouden van de Windows-bestand
* Padvariabelen
* Netwerk-bestandssystemen
* Bestandsinhoud

Andere beperkingen:

* De **maximale bestandsgrootte** kolom en -waarden zijn niet worden gebruikt in de huidige implementatie.
* Als u meer dan 2500 bestanden in de 30 minuten Verzamelfase bestanden verzamelt, mogelijk tot slechtere met prestaties van de oplossing.
* Wanneer het netwerkverkeer hoog, kunnen wijzigingsrecords duren tot maximaal zes uur om weer te geven.
* Als u de configuratie wijzigen terwijl een computer is uitgeschakeld, kan de computer bestandswijzigingen die deel uitmaakten van de vorige configuratie boeken.

### <a name="known-issues"></a>Bekende problemen
De oplossing voor wijzigingen bijhouden is momenteel met de volgende problemen:
* Hotfix-updates worden niet verzameld voor Windows 10 auteurs en Update voor Windows Server 2016 Core RS3 machines.

## <a name="change-tracking-data-collection-details"></a>Details van verzameling gegevens bijhouden wijzigen
Bijhouden van wijzigingen worden verzameld voor software-inventaris en metagegevens van de Windows-Service met behulp van de agents die u hebt ingeschakeld.

De volgende tabel bevat de methoden van de collectie en andere informatie over hoe gegevens worden verzameld voor het bijhouden.

| Platform | Directe Agent | Operations Manager-agent | Linux-agent | Azure Storage | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows- en Linux | &#8226; | &#8226; | &#8226; |  |  | &#8226; | vijf minuten 50 minuten, afhankelijk van het wijzigingstype. Weergeven in de volgende tabel voor meer informatie. |


De volgende tabel toont de frequentie van de verzameling gegevens voor de soorten wijzigingen.

| **type wijzigen** | **frequentie** | **Biedt****agent****verschillen wanneer gevonden verzenden?**  |
| --- | --- | --- |
| Windows-register | 50 minuten | Nee |
| Windows-bestand | 30 minuten | Ja. Als er geen wijziging in 24 uur, wordt een momentopname wordt verzonden. |
| Linux-bestand | 15 minuten | Ja. Als er geen wijziging in 24 uur, wordt een momentopname wordt verzonden. |
| Windows-services | 30 minuten | Ja, elke 30 minuten wanneer wijzigingen zijn gevonden. Elke 24 uur een momentopname wordt verzonden, ongeacht de wijziging. Ja, de momentopname zelfs wordt verzonden wanneer er geen wijzigingen zijn. |
| Daemons Linux | 5 minuten | Ja. Als er geen wijziging in 24 uur, wordt een momentopname wordt verzonden. |
| Windows-software | 30 minuten | Ja, elke 30 minuten wanneer wijzigingen zijn gevonden. Elke 24 uur een momentopname wordt verzonden, ongeacht de wijziging. Ja, de momentopname zelfs wordt verzonden wanneer er geen wijzigingen zijn. |
| Linux-software | 5 minuten | Ja. Als er geen wijziging in 24 uur, wordt een momentopname wordt verzonden. |

### <a name="registry-key-change-tracking"></a>Wijziging van de registersleutel bijhouden

Log Analytics voert Windows-register bewaken en bijhouden van de oplossing voor wijzigingen bijhouden. Het doel van het controleren van wijzigingen registersleutels is op de speldenpunt Uitbreidingspunten waar de code van derden en schadelijke software kunnen activeren. De volgende lijst ziet u de registersleutels die worden bijgehouden door de oplossing en waarom elk wordt bijgehouden.

- HKEY\_lokale\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - Monitors scripts die worden uitgevoerd bij het opstarten.
- HKEY\_lokale\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - Monitors scripts die worden uitgevoerd bij het afsluiten.
- HKEY\_lokale\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - Sleutels die worden geladen voordat de gebruiker zich aanmeldt bij hun Windows-account worden gecontroleerd. De sleutel wordt gebruikt voor 32-bits programma's uitgevoerd op 64-bits computers.
- HKEY\_lokale\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed onderdelen
    - Gecontroleerd op wijzigingen toepassingsinstellingen.
- HKEY\_lokale\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en meestal uitvoeren in-process met Explorer.exe koppelen.
- HKEY\_lokale\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en meestal uitvoeren in-process met Explorer.exe koppelen.
- HKEY\_lokale\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Monitors algemene autostart vermeldingen die rechtstreeks in Windows Verkenner en meestal uitvoeren in-process met Explorer.exe koppelen.
- HKEY\_lokale\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Monitors voor pictogram overlay handler registratie.
- HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Monitors voor pictogram overlay handler-registratie voor 32-bits programma's uitgevoerd op 64-bits computers.
- HKEY\_lokale\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper-objecten
    - De monitoren voor nieuwe browser helper object invoegtoepassingen voor Internet Explorer. Gebruikt voor toegang tot het Document Object Model (DOM) van de huidige pagina te bepalen en navigatie.
- HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper-objecten
    - De monitoren voor nieuwe browser helper object invoegtoepassingen voor Internet Explorer. Gebruikt voor toegang tot het Document Object Model (DOM) van de huidige pagina te bepalen en navigatie voor 32-bits programma's uitgevoerd op 64-bits computers.
- HKEY\_lokale\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - De monitoren voor nieuwe Internet Explorer-uitbreidingen, zoals aangepaste hulpprogramma's en aangepaste werkbalkknoppen.
- HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - De monitoren voor nieuwe Internet Explorer-uitbreidingen, zoals aangepaste hulpprogramma's en aangepaste werkbalkknoppen voor 32-bits programma's uitgevoerd op 64-bits computers.
- HKEY\_lokale\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - De 32-bits stuurprogramma's die zijn gekoppeld aan wavemapper, wave1 en wave2 msacm.imaadpcm, .msadpcm, .msgsm610 en vidc bewaakt. Vergelijkbaar met de sectie [drivers] in het systeem. INI-bestand.
- HKEY\_lokale\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Monitors de 32-bits stuurprogramma's die zijn gekoppeld aan wavemapper, wave1 en wave2 msacm.imaadpcm, .msadpcm, .msgsm610 en vidc voor 32-bits programma's uitgevoerd op 64-bits computers. Vergelijkbaar met de sectie [drivers] in het systeem. INI-bestand.
- HKEY\_lokale\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - Bewaakt de lijst met bekende of veelgebruikte systeem-dll's; Dit systeem wordt voorkomen dat mensen misbruik van zwakke toepassing mapmachtigingen door slepen en neerzetten in Trojaanse paard versies van systeem-dll's.
- HKEY\_lokale\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - De lijst met pakketten kunnen ontvangen van meldingen van gebeurtenissen van Winlogon, het model van de interactieve aanmelding ondersteuning voor het besturingssysteem Windows controleert.


## <a name="use-change-tracking"></a>Gebruik bijhouden
Nadat de oplossing is geïnstalleerd, kunt u het overzicht van wijzigingen voor uw bewaakte servers weergeven met behulp van de **bijhouden** tegel op de **overzicht** pagina in OMS.

![afbeelding van de tegel bijhouden](./media/log-analytics-change-tracking/change-tracking-tile.png)

U kunt wijzigingen in uw infrastructuur en inzoomen naar details voor de volgende categorieën bekijken:

* Wijzigingen per configuratietype voor software- en Windows-services
* Wijzigingen in de software voor toepassingen en updates voor afzonderlijke servers
* Totaal aantal wijzigingen in de software voor elke toepassing
* Linux-pakketten
* Windows-servicewijzigingen voor afzonderlijke servers
* Linux-daemonwijzigingen

![afbeelding van dashboard bijhouden](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![afbeelding van dashboard bijhouden](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>Als u wilt weergeven wijzigen wijzigingen voor elk type
1. Op de **overzicht** pagina, klikt u op de **bijhouden** tegel.
2. Op de **wijzigen bijhouden** dashboard, Controleer de overzichtsgegevens in een van de wijziging type blades en klik op een of meer gedetailleerde informatie over deze in de **logboek zoeken** pagina.
3. U kunt op elk van de zoekpagina logboek kunt resultaten weergeven door de tijd, gedetailleerde resultaten en uw Logboekgeschiedenis zoeken. U kunt ook filteren op facetten om de resultaten te beperken.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-searches.md) om gedetailleerde gegevens bijhouden weer te geven.
