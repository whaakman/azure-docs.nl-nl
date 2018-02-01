---
title: Beveiligingswaarschuwingen per type in Azure Security Center | Microsoft Docs
description: In dit artikel worden de verschillende soorten beveiligingswaarschuwingen die beschikbaar zijn in Azure Security Center besproken.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: yurid
ms.openlocfilehash: 959c0e8693f4bba49488bbe85e334599823ad0ed
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Beveiligingswaarschuwingen in Azure Security Center
Dit artikel helpt u te begrijpen welke verschillende typen beveiligingswaarschuwingen en bijbehorende inzichten er beschikbaar zijn in Azure Security Center. Lees [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md) voor meer informatie over het beheren van waarschuwingen en incidenten.

Als u geavanceerde detectie wilt instellen, voert u een upgrade uit naar Azure Security Center Standard. Er is een gratis proefversie voor 60 dagen beschikbaar. Als u een upgrade wilt uitvoeren, selecteert u de **prijscategorie** in het [beveiligingsbeleid](security-center-policies.md). Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

> [!NOTE]
> Security Center heeft een nieuwe set detecties vrijgegeven voor beperkte preview. Deze detecties maken gebruik van controlerecords, een gemeenschappelijk controleframework, voor het detecteren van de schadelijke gedragingen op Linux-machines. Stuur [ons](mailto:ASC_linuxdetections@microsoft.com) een e-mail met uw abonnement-id's als u wilt deelnemen aan de preview.

## <a name="what-type-of-alerts-are-available"></a>Welk type waarschuwingen zijn er beschikbaar?
Azure Security Center gebruikt allerlei [detectiemogelijkheden](security-center-detection-capabilities.md) om klanten te waarschuwen voor potentiële aanvallen die gericht zijn op hun omgeving. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. De informatie die is opgenomen in een waarschuwing, verschilt op basis van het type analyse dat wordt gebruikt voor het detecteren van de bedreiging. Incidenten kunnen ook aanvullende contextuele informatie bevatten die nuttig kan zijn bij het onderzoeken van een bedreiging.  In dit artikel vindt u informatie over de volgende waarschuwingstypen:

* VMBA (Virtual Machine Behavioral Analysis)
* Netwerkanalyse
* SQL Database- en SQL Data Warehouse-analyse
* Contextuele informatie

## <a name="virtual-machine-behavioral-analysis"></a>VMBA (Virtual Machine Behavioral Analysis)
Azure Security Center kan gebruikmaken van gedragsanalyses om verdachte resources te identificeren op basis van de analyse van gebeurtenislogboeken van virtuele machines. Bijvoorbeeld procesgebeurtenissen en aanmeldgebeurtenissen. Er wordt ook gekeken naar andere signalen om te controleren op ondersteunend bewijs van een wijdverbreide aanval.

> [!NOTE]
> Lees [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md) voor meer informatie over de werking van de detectiemogelijkheden van Security Center.


### <a name="event-analysis"></a>Gebeurtenisanalyse
Security Center maakt gebruik van geavanceerde analyses om verdachte resources te identificeren op basis van de analyse van gebeurtenislogboeken van virtuele machines. Bijvoorbeeld procesgebeurtenissen en aanmeldgebeurtenissen. Er wordt ook gekeken naar andere signalen om te controleren op ondersteunend bewijs van een wijdverbreide aanval.

* **Suspicious process execution detected**: Aanvallers proberen vaak zonder detectie schadelijke code uit te voeren door zich voor te doen als onschadelijke processen. Deze waarschuwingen geven aan dat de uitvoering van een proces overeenkomt met een van de volgende patronen:
    * Er is een proces uitgevoerd dat wordt gebruikt voor schadelijke doeleinden. Terwijl afzonderlijke opdrachten goedaardig lijken te zijn, wordt de waarschuwing weergegeven op basis van de aggregatie van deze opdrachten.
    * Er is een proces uitgevoerd vanaf een ongewone locatie.
    * Er is een proces uitgevoerd vanaf een locatie die bekend staat vanwege verdachte bestanden.
    * Er is een proces uitgevoerd vanaf een verdacht pad.
    * Er is een proces uitgevoerd in een abnormale context.
    * Er is een proces uitgevoerd door een ongebruikelijke account.
    * Er is een proces met een verdachte extensie uitgevoerd.
    * Er is een proces met een verdachte dubbele extensie uitgevoerd.
    * Er is een proces met een verdacht teken van rechts naar links (RLO) in de bestandsnaam uitgevoerd.
    * Er is een proces uitgevoerd waarvan de naam lijkt op, maar niet hetzelfde is als die van een vaak uitgevoerd proces.
    * Er is een proces uitgevoerd waarvan de naam overeenkomt met een bekend aanvalprogramma.
    * Er is een proces met een willekeurige naam uitgevoerd.
    * Er is een proces met een verdachte extensie uitgevoerd.
    * Er is een verborgen bestand uitgevoerd.
    * Er is een proces uitgevoerd als een onderliggend element van een ander ongerelateerd proces.
    * Er is een ongebruikelijk proces gemaakt door een systeemproces.
    * Er is een afwijkend proces gestart door de Windows Update-service.
    * Er is een proces uitgevoerd met een ongebruikelijke opdrachtregel. Dit is gekoppeld aan legitieme processen die zijn gehijackt voor de uitvoering van schadelijke inhoud.
    * Er is een poging gedaan om alle uitvoerbare bestanden (*.exe) in een map uit te voeren vanaf de opdrachtregel.
    * Er is een proces uitgevoerd door het PsExec-hulpprogramma, dat kan worden gebruikt om processen op afstand uit te voeren.
    * Het bovenliggende uitvoerbare Apache Tomcat®-bestand (Tomcat#.exe) is gebruikt voor het starten van verdachte onderliggende processen, die schadelijke opdrachten kunnen hosten of starten.
    * Het Microsoft Windows-bestand Program Compatibility Assistant (pcalua.exe) is gebruikt om uitvoerbare code te starten die mogelijk schadelijk is.
    * Er is een verdachte burst voor het beëindigen van processen gedetecteerd.
    * Het systeemproces SVCHOST is uitgevoerd in een abnormale context.
    * Het systeemproces SVCHOST is uitgevoerd in een zeldzame servicegroep.
    * Er is een verdachte opdrachtregel uitgevoerd.
    * Een PowerShell-script heeft kenmerken die overeenkomen met bekende verdachte scripts.
    * Er is een bekende schadelijke PowerShell Powersploit-cmdlet uitgevoerd.
    * Een ingebouwde SQL-gebruiker heeft een proces uitgevoerd dat normaal niet door deze gebruiker wordt uitgevoerd
    * Er is een base-64 gecodeerd uitvoerbaar bestand gedetecteerd. Dit kan een poging zijn om detectie nietig te verklaren door een uitvoerbaar bestand te maken via een reeks opdrachten.

* **Suspicious RDP resource activity**: Aanvallers richten zich met beveiligingsaanvallen vaak op open beheerpoorten als RDP. Deze waarschuwingen wijzen op verdachte aanmelding voor Extern bureaublad die het volgende aangeven:
    * Er is een poging tot aanmelding voor Extern bureaublad gedaan.
    * Er is een poging gedaan tot aanmelding voor Extern bureaublad met ongeldige accounts.
    * Er zijn meerdere pogingen gedaan tot aanmelding voor Extern bureaublad, waarbij het aanmelden een aantal keren is gelukt.
* **Suspicious SSH resource activity**: Aanvallers richten zich met beveiligingsaanvallen vaak op open beheerpoorten als SSH. Deze waarschuwingen wijzen op verdachte aanmelding voor SSH die het volgende aangeven:
    * Er zijn mislukte SSH-aanmeldingspogingen gedaan.
    * Er zijn SSH-aanmeldingspogingen gedaan, waarvan een aantal pogingen is gelukt.
* **Suspicious WindowPosition registry value**: deze waarschuwing geeft aan dat er een poging is gedaan om de configuratie van een WindowPosition-register te wijzigen, waardoor toepassingsvensters mogelijk in onzichtbare gedeelten van het bureaublad worden verborgen.
* **Suspicious WindowPosition registry value**: AppLocker kan worden gebruikt om de processen te beperken die kunnen worden uitgevoerd op Windows om blootstelling aan malware te beperken. Deze waarschuwing geeft aan dat er mogelijk een poging is gedaan om AppLocker-beperkingen over te slaan door vertrouwde uitvoerbare bestanden te gebruiken (die zijn toegestaan door het AppLocker-beleid) voor de uitvoering van niet-vertrouwde code.
* **Suspicious named pipe communications**: Deze waarschuwing geeft aan dat gegevens zijn geschreven naar een lokale benoemde pijp van een Windows-consoleopdracht. Benoemde pijpen worden vaak gebruikt door aanvallers om een kwaadaardige implantatie uit te voeren en te communiceren.
* **Decoding of an executable using built-in certutil.exe tool**: Deze waarschuwing geeft aan dat een ingebouwd beheerhulpprogramma, certutil.exe, is gebruikt voor de decodering van een uitvoerbaar bestand. Aanvallers misbruiken vaak de functionaliteit van geldige beheerhulpprogramma's om schadelijke bewerkingen uit te voeren, bijvoorbeeld door gebruik te maken van een hulpprogramma als certutil.exe om een kwaadaardig uitvoerbaar bestand te decoderen dat vervolgens wordt uitgevoerd.
* **An event log was cleared**: deze waarschuwing wijst op een verdachte bewerking van een gebeurtenislogboek, die vaak wordt gebruikt door aanvallers om hun sporen te wissen.
* **Disabling and deleting IIS log files**: deze waarschuwing geeft aan dat IIS-logboekbestanden zijn uitgeschakeld en/of verwijderd, die vaak door aanvallers worden gebruikt om hun sporen te wissen.
* **Suspicious file deletion**: deze waarschuwing geeft een verdachte verwijdering van bestanden aan, waarmee aanvallers bewijs van schadelijke binaire bestanden kunnen wissen.
* **All file shadow copies have been deleted**: deze waarschuwing geeft aan dat schaduwkopieën zijn verwijderd.
* **A history file has been cleared**: Deze waarschuwing geeft aan dat het logboekbestand van de opdrachtgeschiedenis is gewist. Dit bestand kan door een aanvaller worden gebruikt om sporen te wissen.
* **Suspicious file cleanup commands**: deze waarschuwing geeft aan dat een combinatie van systeminfo-opdrachten is gebruikt om zelfopschonende activiteiten uit te voeren.  Hoewel *systeminfo.exe* een legitiem Windows-hulpprogramma is, is het niet gebruikelijk dat dit hulpprogramma tweemaal achter elkaar wordt uitgevoerd gevolgd door een wisopdracht, zoals in dit geval is gedaan.
* **Suspicious account creation**: Deze waarschuwing geeft aan dat er een account is gemaakt dat veel overeenkomsten vertoont met een bestaand ingebouwd account met beheerdersbevoegdheden. Deze techniek kan door aanvallers worden gebruikt om een rogue-account te maken zonder te worden gedetecteerd.
* **Suspicious login activity**: Deze waarschuwing geeft een ongebruikelijke aanmeldingsactiviteit aan, die kan duiden op een SMB-aanval (Server Message Block). Als de betreffende resource fungeert als een IIS-server, wordt deze waarschuwing mogelijk weergegeven vanwege een specifieke legitieme IIS-configuratie voor verificatie.
* **Suspicious volume shadow copy activity**: Deze waarschuwing geeft aan dat de schaduwkopie op de resource is verwijderd. Volume Shadow Copy (VSC) is een belangrijk artefact waarin momentopnamen van de gegevens worden opgeslagen. Deze activiteit wordt meestal veroorzaakt door ransomware, maar dit kan ook een legitieme activiteit zijn.
* **Windows registry persistence method**: Deze waarschuwing duidt op een poging om een uitvoerbaar bestand in het Windows-register te behouden. Deze techniek wordt vaak door malware gebruikt om een opstartsessie te overleven.
* **Windows firewall was disabled**: deze waarschuwing geeft aan dat een Windows-firewall is uitgeschakeld.
* **Suspicious new firewall rule**: deze waarschuwing geeft aan dat een nieuwe firewallregel is toegevoegd via *netsh.exe* om verkeer van een uitvoerbaar bestand op een verdachte locatie mogelijk te maken.
* **New user was added to administrators group**: deze waarschuwing geeft aan dat een nieuwe gebruiker is toegevoegd aan de lokale beheerdersgroep.
* **A new service was created**: deze waarschuwing geeft aan dat een nieuwe service is gemaakt.
* **Suspicious XCOPY executions**: deze waarschuwing geeft een reeks XCOPY-uitvoeringen aan, wat erop kan duiden dat een van uw computers is geïnfecteerd en wordt gebruikt om malware te verspreiden.
* **Suppression of legal notice displayed to users at logon**: Deze waarschuwing geeft een wijziging in de registersleutel aan die bepaalt of een juridische kennisgeving wordt weergegeven voor gebruikers wanneer ze zich aanmelden. Dit is een veelgebruikte activiteit van aanvallers nadat een host is geïnfecteerd.
* **Detected anomalous mix of upper and lower case characters in command line**: Deze waarschuwing geeft aan dat er zowel hoofdletters als kleine letters zijn gebruikt in de opdrachtregel. Dit is een techniek die vaak door aanvallers wordt gebruikt om de hoofdlettergevoelige of hash-gebaseerde machineregel te ontlopen.
* **Obfuscated command line**: deze waarschuwing geeft aan dat er verborgen, verdachte indicatoren zijn gedetecteerd op de opdrachtregel.
* **Multiple domain accounts queried**: Aanvallers voeren vaak een query uit op AD-domeinaccounts om gebruikers, domeinbeheeraccounts, domeincontrollers en vertrouwensrelaties te verkennen tussen domeinen. Deze waarschuwing geeft aan dat een ongebruikelijk aantal afzonderlijke domeinaccounts is opgevraagd binnen een korte periode.
* **Possible local reconnaissance activity**: deze waarschuwing geeft aan dat een combinatie van systeminfo-opdrachten is gekoppeld aan verkenningsactiviteiten.  Hoewel *systeminfo.exe* een legitiem Windows-hulpprogramma is, is het niet gebruikelijk dat dit hulpprogramma tweemaal achter elkaar wordt uitgevoerd.
* **Possible execution of keygen executable**: Deze waarschuwing duidt op een proces dat is uitgevoerd waarvan de naam aangeeft dat een keygen-hulpprogramma is uitgevoerd. Deze hulpprogramma's worden meestal gebruikt om de softwarelicentiemechanismen uit te schakelen. Het downloaden hiervan gaat echter vaak gepaard met andere schadelijke software.
* **Suspicious execution via rundll32.exe**: deze waarschuwing geeft aan dat rundll32.exe is gebruikt voor het uitvoeren van een proces met een ongebruikelijke naam, dat overeenkomt met het proces dat door aanvallers wordt gebruikt om een implantatie in de eerste fase te installeren op een geïnfecteerde host.
* **Suspicious combination of HTA and PowerShell**: Deze waarschuwing geeft aan dat door een HTA (Microsoft HTML Application Host) PowerShell-opdrachten worden gestart. Dit is een techniek die aanvallers gebruiken om schadelijke PowerShell-scripts te starten.
* **Change to a registry key that can be abused to bypass UAC**: Deze waarschuwing geeft aan dat een registersleutel is gewijzigd die kan worden misbruikt om UAC (User Access Control) te omzeilen. Dit is een veelgebruikte methode van aanvallers om op een geïnfecteerde host niet-gemachtigde toegang (standaardgebruiker) te wijzigen in gemachtigde toegang (bijvoorbeeld een beheerder).
* **Use of suspicious domain name within command line**: deze waarschuwing geeft aan dat een verdachte domeinnaam is gebruikt, wat erop kan duiden dat een aanvaller schadelijke hulpprogramma's host en kan duiden op eindpunten voor opdrachtenbeheer en gegevensexfiltratie.
* **An account was created on multiple hosts within a 24-hour time period**: deze waarschuwing geeft aan dat er een poging is gedaan om dezelfde gebruikersaccount op meerdere hosts te maken, wat erop kan duiden dat een aanvaller zich lateraal verplaatst binnen het netwerk nadat een of meerdere netwerkentiteiten zijn geïnfecteerd.
* **Suspicious use of CACLS to lower the security state of the system**: Deze waarschuwing geeft aan dat de CACLS (change access control list) is gewijzigd. Deze techniek wordt vaak gebruikt door aanvallers om volledige toegang te verlenen tot binaire systeembestanden zoals ftp.exe, net.exe wscript.exe, enzovoort.
* **Suspected Kerberos Golden Ticket attack parameters**: Deze waarschuwing geeft aan dat opdrachtregelparameters zijn uitgevoerd die consistent zijn met een Kerberos Golden Ticket-aanval. Een verdachte krbtgt-sleutel kan door een aanvaller worden gebruikt om elke gewenste gebruiker te imiteren.
* **Enabling of the WDigest UseLogonCredential registry key**: deze waarschuwing geeft aan dat de registersleutel is gewijzigd, waardoor aanmeldingsreferenties worden opgeslagen als leesbare tekst in het LSA-geheugen die vervolgens uit het geheugen kunnen worden verzameld.
* **Potentially suspicious use of Telegram tool**: deze waarschuwing geeft aan dat de installatie van Telegram, een gratis cloudgebaseerde instant messaging-service, wordt gebruikt door aanvallers om schadelijke binaire bestanden over te dragen naar een andere computer, telefoon of tablet.
* **New ASEP creation**: deze waarschuwing geeft aan dat een nieuwe ASEP (Auto Start Extensibility Point) is gemaakt, waardoor de procesnaam die is geïdentificeerd op de opdrachtregel automatisch wordt gestart en door een aanvaller kan worden gebruikt voor persistentie.
* **Suspicious Set-ExecutionPolicy and WinRM changes**: deze waarschuwing geeft aan dat er wijzigingen in de configuratie zijn uitgevoerd die zijn gekoppeld aan het gebruik van de schadelijke ChinaChopper-webshell.
* **Disabling of critical services**: deze waarschuwing geeft aan dat de opdracht net.exe stop is gebruikt om essentiële services als SharedAccess of Windows Security Center te stoppen.
* **Suspicious use of FTP -s switch**: deze waarschuwing geeft het gebruik van de s-switch van ftp aan, die door malware kan worden gebruikt om verbinding te maken met een externe ftp-server en schadelijke binaire bestanden te downloaden.
* **Preparation for document exfiltration via IIS backdoo**: deze waarschuwing geeft aan dat documenten worden verzameld en voorbereid op exfiltratie.
* **Suspicious execution of VBScript.Encode command**: Deze waarschuwing geeft aan dat de opdracht *VBScript.Encode* is uitgevoerd, waarmee scripts worden omgezet in onleesbare tekst. Hierdoor is het voor gebruikers moeilijker om de code te onderzoeken.
* **VBScript HTTP object allocation**: deze waarschuwing geeft aan dat er een VBScript-bestand is gemaakt vanaf een opdrachtprompt; dit kan worden gebruikt om schadelijke bestanden te downloaden.
* **Sticky keys attack**: deze waarschuwing geeft aan dat een aanvaller een binair toegankelijkheidsbestand (bijvoorbeeld plaktoetsen, een schermtoetsenbord of verteller) ondermijnt om achterdeurtoegang mogelijk te maken.
* **Petya ransomware indicators**: deze waarschuwing geeft aan dat de technieken die zijn gekoppeld aan Petya-ransomware zijn waargenomen.
* **A kernel module was loaded**: deze waarschuwing geeft aan dat een kernelmodule is geladen.
* **A kernel module was removed**: deze waarschuwing geeft aan dat een kernelmodule is verwijderd.
* **Anomalous login to a machine**: deze waarschuwing geeft aan dat een gebruiker zich heeft aangemeld vanaf een ongebruikelijk IP-adres.
* **A file was downloaded and executed**: deze waarschuwing geeft aan dat een bestand is gedownload naar de computer, er uitvoeringsbevoegdheden aan het bestand zijn toegekend en het bestand vervolgens is uitgevoerd.
* **Attempt to disable AMSI**: deze waarschuwing wijst op een poging om de antimalware scaninterface (AMSI) uit te schakelen, waardoor de detectie van antimalware wordt uitgeschakeld.
* **Ransomware indicators**: deze waarschuwing wijst op een verdachte activiteit die doorgaans is gekoppeld aan ransomware voor het vergrendelen van het scherm en coderingsransomware.
* **Suspicious trace collection output file**: deze waarschuwing geeft aan dat een tracering (bijvoorbeeld van netwerkactiviteit) is opgehaald en uitgevoerd naar een ongebruikelijk bestandstype.
* **High risk software**: Deze waarschuwing duidt op het gebruik van software die is gekoppeld aan de installatie van malware. Aanvallers verpakken malware vaak met anderszins goedaardige hulpprogramma's, zoals het programma dat wordt gemeld in deze waarschuwing, en installeren de malware in de achtergrond.
* **Suspicious file creation**: deze waarschuwing geeft aan dat een proces is gemaakt of uitgevoerd dat door aanvallers wordt gebruikt om extra malware te downloaden naar een geïnfecteerde host nadat een bijlage in een phishing-document is geopend.
* **Suspicious credentials in command line**: Deze waarschuwing wijst op een verdacht wachtwoord dat is gebruikt voor het uitvoeren van een bestand. Deze techniek wordt gebruikt door aanvallers om de Pirpi-malware uit te voeren.
* **Possible execution of malware dropper**: deze waarschuwing geeft een bestandsnaam aan die is gebruikt door aanvallers om malware te installeren.
* **Suspicious execution via rundll32.exe**: deze waarschuwing geeft aan dat rundll32.exe wordt gebruikt voor het uitvoeren van een notepad.exe of reg.exe, consistent met de procesinjectietechniek die door aanvallers wordt gebruikt.
* **Suspicious command line arguments**: deze waarschuwing wijst op verdachte opdrachtregelargumenten die zijn gebruikt in combinatie met een omgekeerde shell die door activiteitengroep HYDROGEN wordt gebruikt.
* **Suspicious document credentials**: deze waarschuwing wijst op een verdachte, vooraf berekende algemene wachtwoordhash die wordt gebruikt door malware om een bestand uit te voeren.
* **Dynamic PS script construction**: Deze waarschuwing geeft aan dat een PowerShell-script dynamisch wordt samengesteld. Aanvallers gebruiken deze techniek om geleidelijk een script te maken om IDS-systemen te omzeilen.
* **Metaploit indicators**: deze waarschuwing wijst op een activiteit die is gekoppeld aan het Metasploit-framework, dat een reeks mogelijkheden en hulpprogramma's voor aanvallers biedt.
* **Suspicious account activity**: deze waarschuwing wijst op een poging om verbinding te maken met een computer door gebruik te maken van een account dat onlangs is aangetast.
* **Possible suspicious scheduling tasks access**: deze waarschuwing geeft aan dat een Cron-taak is uitgevoerd, die door aanvallers kan worden gebruikt om schadelijke programma's volgens een gepland schema uit te voeren.
* **Possible suspicious command history file access**: deze waarschuwing wijst op abnormale toegang tot het opdrachtengeschiedenisbestand.
* **Account creation**: deze waarschuwing geeft aan dat een nieuw account op de computer is gemaakt.
* **Change of bash setting has been**: deze waarschuwing geeft aan dat er toegang is verkregen tot een Bash-profielbestand, wat erop kan duiden dat een aanvaller schadelijke programma's volgens een gepland schema probeert uit te voeren.
* **Suspicious sequence of failed sudo attempts**: deze waarschuwing wijst op een reeks mislukte sudo-opdrachten, wat vaak het geval is bij aanhoudende aanvalspogingen om privileges door niet-geautoriseerde gebruikers te escaleren.
* **Suspicious successful sudo attempts**: deze waarschuwing wijst op een reeks mislukte sudo-opdrachten, gevolgd door een geslaagde sudo-poging, wat vaak het geval is bij aanhoudende aanvalspogingen om privileges door niet-geautoriseerde gebruikers te escaleren.
* **A new user was added to the sudoers group**: deze waarschuwing geeft aan dat een gebruiker is toegevoegd aan de groep van sudo-gebruikers, waardoor de gebruikers opdrachten met hoge privileges kunnen uitvoeren.
* **Network logon with plaintext credentials**: Deze waarschuwing geeft aan dat een aanmelding bij het netwerk is waargenomen waarbij het wachtwoord via het netwerk als leesbare tekst is verzonden. Dit is gebruikelijk voor aanmeldingen via een ASP-script met behulp van de ADVAPI of wanneer een gebruiker zich aanmeldt bij IIS met behulp van IIS-basisverificatie. Basisverificatie is niet de aanbevolen methode, tenzij de verificatie is verpakt in een versleutelingslaag als SSL (bijvoorbeeld door alleen gebruik te maken van HTTPS-verbindingen).

### <a name="crash-analysis"></a>Crashanalyse


De analyse van een crashdumpgeheugen is een methode voor het detecteren van geavanceerde schadelijke software die in staat is traditionele beveiligingsoplossingen te misleiden. Diverse varianten schadelijke software proberen de kansen dat ze worden gedetecteerd door antivirusproducten te verkleinen doordat ze nooit iets naar de schijf schrijven of door softwareonderdelen te versleutelen die naar de schijf worden geschreven. Deze techniek bemoeilijkt de detectie van malware met traditionele antimalware-benaderingen. Echter kan dergelijke schadelijke software worden gedetecteerd met behulp van een geheugenanalyse, omdat de malware traceringen in het geheugen moet achterlaten om te kunnen werken.

Wanneer software vastloopt, bevat een crashdump een gedeelte van het geheugen ten tijde van de crash. De crash kan zijn veroorzaakt door malware, door gewoon gebruik of door systeemproblemen. Door het geheugen in de crashdump te analyseren, kan Security Center technieken detecteren die worden gebruikt om misbruik te maken van zwakke plekken in software, toegang te krijgen tot vertrouwelijke gegevens en ongemerkt aanwezig te blijven in een aangetaste machine. Een crashanalyse wordt uitgevoerd met minimale gevolgen voor de prestaties van hosts omdat deze wordt uitgevoerd door de back-end van Security Center.

* **Code injection discovered**: Code-injectie is het invoegen van uitvoerbare modules in actieve processen of threads. Deze methode wordt gebruikt door schadelijke software om toegang krijgen tot gegevens, zich te verbergen of te voorkomen dat deze wordt verwijderd (bijvoorbeeld persistentie). Met deze waarschuwing wordt aangegeven dat de crashdump een geïnjecteerde module bevat. Legitieme softwareontwikkelaars voeren af en toe code-injectie uit voor niet-kwaadwillende redenen, bijvoorbeeld om een bestaande toepassing of een bestaand besturingssysteemonderdeel te wijzigen of uit te breiden. Om onderscheid te kunnen maken tussen schadelijke en niet-schadelijke geïnjecteerde modules, controleert Security Center of de geïnjecteerde module voldoet aan een profiel met verdacht gedrag. Het resultaat van deze controle wordt aangegeven door het veld "SIGNATURE" van de waarschuwing en is te zien aan de ernst van de waarschuwing, de omschrijving van de waarschuwing en herstelstappen voor de waarschuwing.
* **Suspicious code segment**: Deze waarschuwing geeft aan dat een codesegment is toegewezen met behulp van niet-standaardmethoden, zoals die worden gebruikt bij reflectieve injectie en procesuitholling. Aanvullende kenmerken van het codesegment worden verwerkt voor meer context over de mogelijkheden en het gedrag van het gerapporteerde codesegment.
* **Shellcode discovered**: Shellcode is de nettolading die wordt uitgevoerd nadat de schadelijke software misbruik heeft gemaakt van een beveiligingslek in de software. Deze waarschuwing geeft aan dat de crashdumpanalyse uitvoerbare code heeft gedetecteerd dat gedrag vertoont dat gewoonlijk wordt uitgevoerd door schadelijke nettoladingen. Hoewel niet-schadelijke software dit gedrag kan vertonen, is het niet gebruikelijk voor normale softwareontwikkelingsprocedures.
* **Module hijacking discovered**: Windows maakt gebruik van Dynamic Link Libraries (DLL's) om software toe te staan gebruik te maken van de algemene Windows-systeemfunctionaliteit. DLL-hijacking treedt op wanneer malware de DLL-laadvolgorde wijzigt om schadelijke nettoladingen in het geheugen te laden, waarbij willekeurige code kan worden uitgevoerd. Deze waarschuwing geeft aan dat de crashdumpanalyse een gelijknamige module heeft gedetecteerd die wordt geladen via twee verschillende paden. Een van de geladen paden is afkomstig van een algemene binaire locatie van het Windows-systeem. Van tijd tot tijd wijzigen legitieme softwareontwikkelaars de laadvolgorde van de DLL wegens niet-kwaadwillende redenen, zoals instrumentering, uitbreiding van het Windows-besturingssysteem of Windows-toepassingen. Om te helpen onderscheid te maken tussen kwaadwillende en mogelijk goedaardige wijzigingen in de laadvolgorde van het DLL-bestand controleert Security Center of een geladen module aan een verdacht profiel voldoet.
* **Masquerading Windows module detected**: Malware kan veelvoorkomende namen van binaire Windows-systeembestanden (bijvoorbeeld SVCHOST.EXE) of modules (bijvoorbeeld NTDLL.DLL) gebruiken om ongemerkt binnen te komen en de aard van de schadelijke software voor systeembeheerders te verbergen. Deze waarschuwing geeft aan dat het crashdumpbestand modules bevat die namen van Windows-systeemmodules gebruiken, maar niet voldoen aan andere criteria die gangbaar zijn voor Windows-modules. Analyse van de kopie op schijf van de onechte module kan mogelijk meer informatie bieden over de legitieme of schadelijke aard van deze module.
* **Modified system binary discovered**: Malware kan belangrijke binaire systeembestanden wijzigen om stiekem toegang tot gegevens te krijgen of ongemerkt aanwezig te blijven op een geïnfecteerd systeem. Deze waarschuwing geeft aan dat de crashdumpanalyse heeft gedetecteerd dat belangrijke binaire Windows-besturingssysteembestanden zijn gewijzigd in het geheugen of op schijf. Legitieme softwareontwikkelaars wijzigen van tijd tot tijd systeemmodules in het geheugen voor niet-kwaadwillende redenen, zoals omzeilingen of voor de compatibiliteit van toepassingen. Om te helpen onderscheid te maken tussen kwaadwillende en mogelijk goedaardige modules, controleert Security Center of de gewijzigde module aan een verdacht profiel voldoet.

## <a name="network-analysis"></a>Netwerkanalyse
Het detecteren van netwerkbedreigingen van Security Center werkt volgens het automatisch verzamelen van beveiligingsgegevens van uw Azure IPFIX-verkeer (Internet Protocol Flow Information Export). Deze informatie wordt door Security Center geanalyseerd, waarbij vaak informatie uit meerdere bronnen wordt samengebracht om bedreigingen te analyseren.

* **Possible incoming SQL brute force attempts**: Netwerkverkeeranalyse heeft verdachte binnenkomende SQL-communicatie gedetecteerd. Deze activiteit komt overeen met aanhoudende aanvalspogingen tegen SQL-servers.
* **Suspicious incoming RDP network activity from multiple sources**: Netwerkverkeeranalyse heeft afwijkende binnenkomende Remote Desktop Protocol (RDP)-communicatie uit meerdere bronnen gedetecteerd. De voorbeeldnetwerkgegevens tonen unieke IP-adressen die verbinding maken met uw computer, wat abnormaal is voor deze omgeving. Deze activiteit kan wijzen op een poging om uw RDP-eindpunt vanaf meerdere hosts (Botnet) aan te vallen.
* **Suspicious incoming RDP network activity**: Netwerkverkeeranalyse heeft afwijkende binnenkomende Remote Desktop Protocol (RDP)-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen een hoog aantal binnenkomende verbindingen naar uw computer, wat abnormaal is voor deze omgeving. Deze activiteit kan wijzen op een poging om uw RDP-eindpunt aan te vallen.
* **Suspicious outgoing RDP network activity to multiple destinations**: Netwerkverkeeranalyse heeft afwijkende uitgaande Remote Desktop Protocol (RDP)-communicatie naar meerdere bestemmingen gedetecteerd. Dit kan erop wijzen dat uw computer is geïnfecteerd en wordt gebruikt om externe RDP-eindpunten herhaaldelijk aan te vallen. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.
* **Suspicious outgoing RDP network activity**: Netwerkverkeeranalyse heeft afwijkende uitgaande Remote Desktop Protocol (RDP)-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen een hoog aantal uitgaande verbindingen vanaf uw computer, wat abnormaal is voor deze omgeving. Dit kan erop wijzen dat uw computer is geïnfecteerd en wordt gebruikt om externe RDP-eindpunten herhaaldelijk aan te vallen. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.
* **Suspicious incoming SSH network activity**: Netwerkverkeeranalyse heeft afwijkende binnenkomende SSH-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen een hoog aantal binnenkomende verbindingen naar uw computer, wat abnormaal is voor deze omgeving. Deze activiteit kan wijzen op een poging om uw SSH-eindpunt aan te vallen.
* **Suspicious incoming SSH network activity from multiple sources**: Netwerkverkeeranalyse heeft afwijkende binnenkomende SSH-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen unieke IP-adressen die verbinding maken met uw computer, wat abnormaal is voor deze omgeving. Deze activiteit kan wijzen op een poging om uw SSH-eindpunt vanaf meerdere hosts (Botnet) aan te vallen.
* **Suspicious outgoing SSH network activity**: Netwerkverkeeranalyse heeft afwijkende uitgaande SSH-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen een hoog aantal uitgaande verbindingen vanaf uw computer, wat abnormaal is voor deze omgeving. Dit kan erop wijzen dat uw computer is geïnfecteerd en wordt gebruikt om externe SSH-eindpunten herhaaldelijk aan te vallen. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.
* **Suspicious outgoing SSH network activity to multiple destinations**: Netwerkverkeeranalyse heeft afwijkende uitgaande SSH-communicatie naar meerdere bestemmingen gedetecteerd. De voorbeeldnetwerkgegevens geven aan dat uw computer verbinding maakt met unieke IP-adressen, wat abnormaal is voor deze omgeving. Dit kan erop wijzen dat uw computer is geïnfecteerd en wordt gebruikt om externe SSH-eindpunten herhaaldelijk aan te vallen. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.
* **Network communication with a malicious machine detected**: netwerkverkeeranalyse geeft aan dat de computer heeft gecommuniceerd met een mogelijk opdracht- en beheercentrum.
* **Possible compromised machine detected**: Netwerkverkeeranalyse heeft uitgaande activiteit gedecteerd, wat erop kan duiden dat de computer fungeert als onderdeel van een botnet. De analyse is gebaseerd op IP-adressen die door uw resource worden benaderd en openbare DNS-records.


## <a name="sql-database-and-sql-data-warehouse-analysis"></a>SQL Database- en SQL Data Warehouse-analyse

Een resourceanalyse door Security Center richt zich op PaaS-services (Platform as a Service), zoals de integratie met [Azure SQL Database-Detectie van Bedreigingen](../sql-database/sql-database-threat-detection.md) en Azure SQL Data Warehouse. SQL Threat Detection detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen aangeven om toegang te krijgen of misbruik te maken van databases en activeert de volgende waarschuwingen:

* **Kwetsbaarheid voor SQL-injectie**: Deze waarschuwing wordt geactiveerd wanneer een foutieve SQL-instructie wordt gedetecteerd in een database. Deze waarschuwing kan duiden op mogelijke kwetsbaarheid voor SQL-injectieaanvallen. Er zijn twee mogelijke redenen voor het genereren van een foutieve instructie:
    * Een fout in de toepassingscode die de foutieve SQL-instructie maakt
    * Toepassingscode of opgeslagen procedures schonen gebruikersinvoer niet op tijdens het construeren van de foutieve SQL-instructie, dit kan worden misbruikt voor SQL-injectie
* **Mogelijke SQL-injectie**: deze waarschuwing wordt geactiveerd wanneer een actieve aanval wordt uitgevoerd tegen een geïdentificeerd beveiligingslek voor SQL-injectie in een toepassing. Dit betekent dat de aanvaller schadelijke SQL-instructies probeert te injecteren met de kwetsbare toepassingscode of opgeslagen procedures.
* **Toegang vanaf ongebruikelijke locatie**: deze waarschuwing wordt geactiveerd wanneer er een wijziging is in het toegangspatroon tot de SQL-server, waarbij iemand zich vanuit een ongebruikelijke geografische locatie heeft aangemeld bij de SQL server. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
* **Toegang vanaf ongebruikelijk Azure-datacentrum**: deze waarschuwing wordt geactiveerd wanneer er een wijziging is in het toegangspatroon tot de SQL-server, waarbij iemand zich heeft aangemeld bij de SQL server vanuit een Azure-datacentrum dat ongebruikelijk was op deze server in de recente periode. In sommige gevallen detecteert de waarschuwing een legitieme actie (uw nieuwe toepassing in Azure, Power BI, Azure SQL Query-Editor). In andere gevallen detecteert de waarschuwing een schadelijke actie vanuit een Azure resource/service (voormalig werknemer, externe aanvaller).
* **Toegang vanaf ongebruikelijke klant**: deze waarschuwing wordt geactiveerd wanneer er een wijziging is in het toegangspatroon tot de SQL-server, waarbij iemand zich vanuit een ongebruikelijke klant (SQL-gebruiker) heeft aangemeld bij de SQL-server. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
* **Toegang tot een toepassing die mogelijk schadelijk is**: deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.
* **Brute Force SQL-referenties**: deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties is. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een Brute Force-aanval.

## <a name="contextual-information"></a>Contextuele informatie
Tijdens een onderzoek hebben analisten extra context nodig om tot een eindoordeel te komen over de aard van de bedreiging en hoe deze kan worden tegengehouden.  Als er bijvoorbeeld een afwijkende situatie in het netwerk is gedetecteerd, maar er geen inzicht is in wat er verder gebeurt in het netwerk of met de betreffende resource, is het zeer lastig om te begrijpen welke vervolgacties nodig zijn. Om hierbij te helpen, kan een beveiligingsincident artefacten, gerelateerde gebeurtenissen en andere informatie bevatten die de onderzoeker meer context geven. De beschikbaarheid van extra informatie varieert afhankelijk van het type bedreiging dat is gedetecteerd en de configuratie van uw omgeving, en zal dus niet voor alle beveiligingsincidenten beschikbaar zijn.

Als er extra informatie beschikbaar is, wordt deze weergegeven in het beveiligingsincident onder de lijst met waarschuwingen. Dit kan informatie zijn zoals:

- Wissen van logboekgebeurtenissen
- PNP-apparaat aangesloten vanaf onbekend apparaat
- Waarschuwingen waarop geen actie kan worden uitgevoerd

![Waarschuwing over ongebruikelijke toegang](./media/security-center-alerts-type/security-center-alerts-type-fig20.png)


## <a name="next-steps"></a>Volgende stappen
In dit artikel bent u meer te weten gekomen over de verschillende soorten beveiligingswaarschuwingen in Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsincidenten afhandelen in Azure Security Center](security-center-incident.md)
* [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md)
* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
