---
title: Threat detection voor virtuele machines en servers in Azure Security Center | Microsoft Docs
description: In dit onderwerp wordt de virtuele machine en server waarschuwingen beschikbaar in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 5487b4f49f5dbf7b968cd45d40555c69b54c329a
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571580"
---
# <a name="threat-detection-for-vms--servers-in-azure-security-center"></a>Detectie van bedreigingen voor VM's en servers in Azure Security Center

In dit onderwerp geeft de verschillende typen detectiemethoden en waarschuwingen beschikbaar voor virtuele machines en Servers met de volgende bewerking-systemen. Zie voor een lijst van ondersteunde versies, [Platforms en functies die worden ondersteund door Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Windows <a name="windows-machines"></a>

Security Center kan worden geïntegreerd met Azure-services om te controleren en beveiligen van uw machines op basis van Windows.  Security Center geeft de waarschuwingen en suggesties voor herstel van al deze services in de indeling van een eenvoudig te gebruiken.

### Microsoft Server Defender ATP <a nanme="windows-atp"></a>

Azure Security Center een uitbreiding voor de Cloud Workload Protection Platforms door te integreren met Windows Defender Advanced Threat Protection (ATP). Dit biedt uitgebreide functionaliteit eindpunt detectie en reactie (EDR).

> [!NOTE]
> Windows Server Defender ATP-sensor is automatisch ingeschakeld op Windows-Servers die toegevoegd aan Azure Security Center zijn.

Wanneer Windows Defender ATP voor Server een bedreiging wordt gedetecteerd, wordt een waarschuwing geactiveerd. De waarschuwing wordt weergegeven op het dashboard van Security Center. Vanuit het dashboard, kunt u naar de Windows Defender ATP-console een gedetailleerd onderzoek uitvoert om het bereik van de aanval bloot draaien. Zie voor meer informatie over Windows Defender ATP voor Server [onboarding-servers naar de Windows Defender ATP-service](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

### Crashdumpanalyse <a nanme="windows-dump"></a>

Wanneer software vastloopt, bevat een crashdump een gedeelte van het geheugen ten tijde van de crash.

Een crash kan zijn veroorzaakt door schadelijke software of malware bevatten. Om te voorkomen dat ze worden gedetecteerd door security-producten, gebruikt u diverse varianten schadelijke software een fileless aanval, die schrijven naar schijf of encrypting softwareonderdelen die zijn geschreven voorkomt naar schijf. Dit type aanval is moeilijk te detecteren met traditionele benaderingen op basis van een schijf.

Dit type aanval kan echter worden gedetecteerd met behulp van de analyse van geheugengebruik. Door het geheugen in de crashdump analyseren, kan Security Center de technieken die van de aanval gebruikmaakt misbruik van zwakke plekken in de software, toegang krijgen tot vertrouwelijke gegevens en ongemerkt aanwezig te blijven in een aangetaste machine detecteren. Dit wordt gedaan door de Security Center-back-end met minimale prestatie-invloed op hosts.

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Code-injectie gedetecteerd**|Code-injectie is het invoegen van uitvoerbare modules in actieve processen of threads. Deze techniek wordt gebruikt door malware om toegang tot gegevens, terwijl is verborgen zelf om te voorkomen dat wordt gevonden en kan worden verwijderd. <br/>Met deze waarschuwing wordt aangegeven dat de crashdump een geïnjecteerde module bevat. Onderscheid maken tussen schadelijke en niet-schadelijke geïnjecteerde modules, controleert Security Center of de geïnjecteerde module aan een profiel met verdacht gedrag voldoet.|
|**Verdachte codesegment is gedetecteerd**|Geeft aan dat een codesegment is toegewezen met behulp van niet-standaardmethoden, zoals bij reflectieve injectie en procesuitholling. De waarschuwing bevat aanvullende kenmerken van het codesegment dat is verwerkt voor meer context voor de mogelijkheden en het gedrag van het gerapporteerde codesegment.|
|**Shellcode gedetecteerd**|Shellcode is de nettolading die wordt uitgevoerd nadat de schadelijke software misbruik heeft gemaakt van een beveiligingslek in de software.<br/>Deze waarschuwing geeft aan dat de crashdumpanalyse uitvoerbare code die vaak worden uitgevoerd door schadelijke nettoladingen gedrag vertoont heeft gedetecteerd. Hoewel dit probleem ook door niet-schadelijke software uitvoeren kan, is het niet gebruikelijk van normale softwareontwikkelingsprocedures.|

### Fileless Aanvalsdetectie <a nanme="windows-fileless"></a>

In Azure zien we regelmatig fileless aanvallen die gericht is op de eindpunten voor onze klanten.

Om te voorkomen dat detectie, invoeren fileless aanvallen schadelijke nettoladingen in het geheugen. Aanvaller nettoladingen blijven in het geheugen van verdachte processen en voert u een breed scala aan schadelijke activiteiten.

Met fileless aanvallen detecteren identificeren geautomatiseerde geheugen forensische technieken fileless aanval toolkits, technieken en gedrag. Deze oplossing wordt regelmatig uw machine tijdens runtime wordt gescand en extraheert inzichten die rechtstreeks vanuit het geheugen van de security-essentiële processen.

Bewijs van misbruik, code-injectie en uitvoering van schadelijke nettoladingen is gevonden. Fileless Aanvalsdetectie genereert gedetailleerde beveiligingswaarschuwingen te versnellen waarschuwing classificeren-, correlatie- en downstream reactietijd. Deze aanpak is een aanvulling op gebeurtenissen gebaseerde EDR oplossingen bieden meer dekking voor detectie.

> [!NOTE]
> U kunt Windows waarschuwingen simuleren door bits-download [Azure Security Center Playbook](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef): Beveiligingswaarschuwingen en volg de richtlijnen van de opgegeven

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Fileless aanval techniek gedetecteerd**|Het geheugen van de procedure die hieronder zijn opgegeven, bevat een toolkit fileless aanval: Meterpreter. Fileless aanval toolkits, hebben meestal geen een aanwezigheid op het bestandssysteem, waardoor detectie door traditionele antivirusprogramma's moeilijk.|

### <a name="further-reading"></a>Lees hier meer over

Voor voorbeelden en meer informatie over het detecteren van Security Center:

* [Hoe Azure Security Center de detectie van cyberaanval automatiseert](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)
* [Hoe Azure Security Center detecteert beveiligingsproblemen met behulp van beheerprogramma 's](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

## Linux <a name="linux-machines"></a>

Security Center verzamelt controlerecords van Linux-machines met behulp van **auditd**, een controle van de meest voorkomende Linux frameworks. auditd heeft het voordeel van hebben al voor een lange periode en woont in hoofdlijnen-kernel. 

### Linux auditd waarschuwingen en integratie van Microsoft Monitoring Agent (MMA) <a name="linux-auditd"></a>

Het systeem auditd bestaat uit een subsysteem kernel-niveau, die verantwoordelijk is voor controle van het systeemaanroepen, ze te filteren op een bepaalde regelset en berichten voor hen te schrijven met een socket. Security Center kan worden geïntegreerd met de functionaliteiten van het pakket auditd binnen de Microsoft Monitoring Agent (MMA). Deze integratie kunt u auditd gebeurtenissen verzamelen in alle ondersteunde Linux-distributies zonder eventuele vereisten.  

auditd records zijn verzameld, uitgebreid en samengevoegd in evenementen met behulp van de Linux-MMA-agent. Security Center werkt voortdurend over het toevoegen van nieuwe analytics, die gebruikmaken van Linux signalen voor het detecteren van schadelijke gedragingen op de cloud en on-premises Linux-machines. Net als bij de Windows-mogelijkheden, deze analyses overbruggen verdachte processen, twijfelachtige aanmeldingspogingen, kernel-module laden en andere activiteiten, die wijzen op een virtuele machine is aangevallen of al helemaal wordt geschonden.  

Hieronder volgen enkele voorbeelden van analytics die laten zien hoe we verschillende stadia van de levenscyclus van de aanval overbruggen.

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Toegang tot het geautoriseerde sleutelsbestand SSH op ongewone wijze gezien proces**|Een geautoriseerd sleutelsbestand SSH zijn geopend in een methode die vergelijkbaar is met campagnes met bekende malware. Deze toegang kan duiden dat een aanvaller probeert permanente toegang tot een virtuele machine|
|**Gedetecteerde persistentie poging**|Analyse van host heeft gedetecteerd dat een opstartscript voor de modus voor één gebruiker is geïnstalleerd. <br/>Omdat dit zeldzaam dat een legitieme proces nodig zijn voor de in deze modus worden uitgevoerd, wordt dit kan duiden dat een aanvaller een schadelijke proces heeft toegevoegd aan elk run-niveau om te waarborgen van persistentie.|
|**Manipulatie van geplande taken die zijn gedetecteerd**|Analyse van host aangetroffen mogelijk manipulatie van geplande taken. Geplande taken toevoegen aanvallers vaak voor computers die ze hebben aangetast te krijgen van persistentie.|
|**Verdacht bestand tijdstempel wijzigen**|Analyse van host een wijziging verdachte timestamp gedetecteerd. Tijdstempels kopiëren van bestaande legitieme bestanden naar de nieuwe hulpprogramma's om te voorkomen dat de detectie van deze nieuwe verwijderde bestanden aanvallers vaak|
|**Een nieuwe gebruiker is toegevoegd aan de groep van sudo**|Analyse van host wordt gedetecteerd dat een gebruiker is toegevoegd aan de groep van sudo, waardoor het uitvoeren van opdrachten met hoge bevoegdheden.|
|**Waarschijnlijk misbruik van DynoRoot door een beveiligingslek in dhcp-client**|Host-gegevensanalyse gedetecteerd tijdens de uitvoering van een ongebruikelijke opdracht met bovenliggende proces van dhclient script.|
|**Verdachte kernelmodule gedetecteerd**|Analyse van host een gedeelde objectbestand worden geladen als een kernelmodule gedetecteerd. Dit wordt mogelijk legitieme activiteit of een indicatie dat een van uw computers is aangetast.|
|**Proces dat is gekoppeld aan digitale valuta analysestructuur gedetecteerd**|Host-gegevensanalyse gedetecteerd tijdens de uitvoering van een proces die doorgaans gepaard gaan met digitale valuta gegevensanalyse|
|**Mogelijke poort doorsturen naar externe IP-adres**|Analyse van host de initialisatie van de poort worden doorgestuurd naar een extern IP-adres gedetecteerd.|

> [!NOTE]
> U kunt Windows waarschuwingen simuleren door het downloaden van [Azure Security Center Playbook: Beveiligingswaarschuwingen](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef) en volg de richtlijnen van de opgegeven.


Raadpleeg voor meer informatie de volgende artikelen:  

* [Maak gebruik van Azure Security Center om te detecteren wanneer waarmee is geknoeid Linux-machines aanval](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)

* [Azure Security Center kunt opkomende beveiligingslekken detecteren in Linux](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

 