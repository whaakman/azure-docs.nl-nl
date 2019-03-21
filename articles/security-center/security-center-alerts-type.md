---
title: Beveiligingswaarschuwingen per type in Azure Security Center | Microsoft Docs
description: In dit artikel worden de verschillende soorten beveiligingswaarschuwingen die beschikbaar zijn in Azure Security Center besproken.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2018
ms.author: monhaber
ms.openlocfilehash: 407d0610f919b1145a91e8678f57f8d344c9c187
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286360"
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Beveiligingswaarschuwingen in Azure Security Center
Dit artikel helpt u te begrijpen welke verschillende typen beveiligingswaarschuwingen en bijbehorende inzichten er beschikbaar zijn in Azure Security Center. Lees [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md) voor meer informatie over het beheren van waarschuwingen en incidenten.

Als u geavanceerde detectie wilt instellen, voert u een upgrade uit naar Azure Security Center Standard. Er is een gratis proefversie beschikbaar. Als u een upgrade wilt uitvoeren, selecteert u de **prijscategorie** in het [beveiligingsbeleid](tutorial-security-policy.md). Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

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

* **Uitvoering van verdachte processen gedetecteerd**: Aanvallers proberen vaak zonder detectie schadelijke code uitvoeren door zich voordoet als onschadelijke processen. Deze waarschuwingen geven aan dat de uitvoering van een proces overeenkomt met een van de volgende patronen:
    * Er is een proces uitgevoerd dat wordt gebruikt voor schadelijke doeleinden. Terwijl afzonderlijke opdrachten goedaardig weergegeven mogelijk, wordt de waarschuwing beoordeeld op basis van een aggregatie van deze opdrachten.
    * Er is een proces uitgevoerd vanaf een ongewone locatie.
    * Er is een proces uitgevoerd vanaf een locatie die bekend staat vanwege verdachte bestanden.
    * Er is een proces uitgevoerd vanaf een verdacht pad.
    * Er is een proces uitgevoerd in een abnormale context.
    * Er is een proces uitgevoerd door een ongebruikelijke account.
    * Er is een proces met een verdachte extensie uitgevoerd.
    * Er is een proces met een verdachte dubbele extensie uitgevoerd.
    * Er is een proces met een verdacht teken van rechts naar links (RLO) in de bestandsnaam uitgevoerd.
    * Een proces waarvan de naam vergelijkbaar met, maar een andere uit een vaak uitgevoerd proces is er is uitgevoerd
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

* **Suspicious RDP resource activity**: Aanvallers richten vaak op open beheerpoorten als RDP met brute force-aanvallen. Deze waarschuwingen wijzen op verdachte aanmelding voor Extern bureaublad die het volgende aangeven:
    * Er is een poging tot aanmelding voor Extern bureaublad gedaan.
    * Er is een poging gedaan tot aanmelding voor Extern bureaublad met ongeldige accounts.
    * Aanmelding voor extern bureaublad zijn verzonden, waarvan sommige konden is Meld u aan bij de machine.
* **Suspicious SSH resource activity**: Aanvallers richten vaak op open beheerpoorten als SSH met brute force-aanvallen. Deze waarschuwingen wijzen op verdachte aanmelding voor SSH die het volgende aangeven:
    * Er zijn mislukte SSH-aanmeldingspogingen gedaan.
    * Er zijn SSH-aanmeldingspogingen gedaan, waarvan een aantal pogingen is gelukt.
* **Suspicious WindowPosition registerwaarde**: Deze waarschuwing geeft aan dat de configuratiewijziging van een WindowPosition-register is uitgevoerd, kan die worden verborgen in onzichtbare gedeelten van de desktop-toepassing voor windows.
* **AppLocker windowposition**: AppLocker kan worden gebruikt om te beperken van de processen die kunnen worden uitgevoerd op Windows, blootstelling aan malware te beperken. Deze waarschuwing geeft aan dat er mogelijk een poging is gedaan om AppLocker-beperkingen over te slaan door vertrouwde uitvoerbare bestanden te gebruiken (die zijn toegestaan door het AppLocker-beleid) voor de uitvoering van niet-vertrouwde code.
* **Suspicious met de naam van de pipe communications**: Deze waarschuwing geeft aan dat gegevens naar een lokale benoemde pijp van een Windows-consoleopdracht zijn geschreven. Benoemde pijpen worden vaak gebruikt door aanvallers om een kwaadaardige implantatie uit te voeren en te communiceren.
* **Decodering van een uitvoerbaar bestand met behulp van built-in certutil.exe tool**: Deze waarschuwing geeft aan dat een ingebouwd beheerhulpprogramma, certutil.exe, is gebruikt voor het decoderen van een uitvoerbaar bestand. Aanvallers misbruiken vaak de functionaliteit van geldige beheerhulpprogramma's om schadelijke bewerkingen uit te voeren, bijvoorbeeld door gebruik te maken van een hulpprogramma als certutil.exe om een kwaadaardig uitvoerbaar bestand te decoderen dat vervolgens wordt uitgevoerd.
* **Een gebeurtenislogboek is gewist**: Deze waarschuwing wijst op een verdachte gebeurtenislogboek bewerking, die vaak door aanvallers wordt gebruikt om te proberen om hun sporen te wissen.
* **Uitschakelen en verwijderen van IIS-logboekbestanden**: Deze waarschuwing geeft aan dat IIS-logboekbestanden zijn uitgeschakeld en/of verwijderd, die vaak door aanvallers wordt gebruikt om te proberen om hun sporen te wissen.
* **Verdachte bestanden hebt verwijderd**: Deze waarschuwing geeft een verdachte verwijdering van bestanden, die kan worden gebruikt door een aanvaller bewijs van schadelijke binaire bestanden.
* **Alle schaduwkopieën van het bestand is verwijderd**: Deze waarschuwing geeft aan dat schaduwkopieën zijn verwijderd.
* **Verdacht bestand opschonen opdrachten**: Deze waarschuwing geeft aan dat een combinatie van systeminfo-opdrachten gebruikt om uit te voeren na inbreuk op zelfopschonende activiteiten.  Hoewel *systeminfo.exe* een legitiem Windows-hulpprogramma is, is het niet gebruikelijk dat dit hulpprogramma tweemaal achter elkaar wordt uitgevoerd gevolgd door een wisopdracht, zoals in dit geval is gedaan.
* **Het maken van verdachte accounts**: Deze waarschuwing geeft aan dat een account is gemaakt met een vertoont met een bestaand ingebouwd account met beheerdersbevoegdheden. Deze techniek kan door aanvallers worden gebruikt om een rogue-account te maken zonder te worden gedetecteerd.
* **Suspicious volume shadow copy-activiteit**: Deze waarschuwing geeft aan dat shadow copy-activiteit voor verwijdering op de resource. Volume Shadow Copy (VSC) is een belangrijk artefact waarin momentopnamen van de gegevens worden opgeslagen. Deze activiteit is gekoppeld aan Ransomware, maar dit kan ook worden legitieme.
* **Windows registry persistence methode**: Deze waarschuwing wijst op een poging om vast te leggen van een uitvoerbaar bestand in het Windows-register. Deze techniek wordt vaak door malware gebruikt om een opstartsessie te overleven.
* **Verdachte nieuwe firewallregel**: Deze waarschuwing geeft aan dat een nieuwe firewallregel is toegevoegd *netsh.exe* waarmee verkeer van een uitvoerbaar bestand op een verdachte locatie.
* **Suspicious XCOPY executions**: Deze waarschuwing geeft aan dat een reeks XCOPY-uitvoeringen die dat een van uw computers duiden kan is geïnfecteerd en is gebruikt om malware te verspreiden.
* **Onderdrukking van de juridische kennisgeving weergegeven voor gebruikers bij aanmelding**: Deze waarschuwing geeft aan dat een wijziging in de registersleutel die bepaalt of een juridische kennisgeving wordt weergegeven aan gebruikers wanneer ze zich aanmelden. Dit is een veelgebruikte activiteit van aanvallers nadat een host is geïnfecteerd.
* **Ongebruikelijke combinatie van hoofdletters en kleine letters gedetecteerd in vanaf de opdrachtregel**: Deze waarschuwing geeft aan dat het gebruik van een combinatie van hoofdletters en kleine letters op de opdrachtregel, dit is een techniek die wordt gebruikt door aanvallers hoofdlettergevoelige of hash-gebaseerde machineregel ontlopen.
* **Verborgen vanaf de opdrachtregel**: Deze waarschuwing geeft aan dat er verdachte indicatoren zijn gedetecteerd op de opdrachtregel.
* **Meerdere domeinaccounts uitgevoerd**: Kwaadwillende personen vaak query uitvoeren op AD-domeinaccounts tijdens het uitvoeren van reconnaissance voor gebruikers, domeinbeheeraccounts, domeincontrollers en vertrouwensrelaties tussen domeinen. Deze waarschuwing geeft aan dat een ongebruikelijk aantal afzonderlijke domeinaccounts is opgevraagd binnen een korte periode.
* **Mogelijke lokale reconnaissance activiteit**: Deze waarschuwing geeft aan dat een combinatie van systeminfo-opdrachten die zijn gekoppeld aan reconnaissance-activiteit is uitgevoerd.  Hoewel *systeminfo.exe* een legitiem Windows-hulpprogramma is, is het niet gebruikelijk dat dit hulpprogramma tweemaal achter elkaar wordt uitgevoerd.
* **Mogelijke uitvoering van uitvoerbare keygen**: Deze waarschuwing geeft aan dat een proces waarvan de naam van een keygen-hulpprogramma indicatieve is zijn uitgevoerd. Deze hulpprogramma's worden meestal gebruikt om de softwarelicentiemechanismen uit te schakelen. Het downloaden hiervan gaat echter vaak gepaard met andere schadelijke software.
* **Verdachte uitvoeren via rundll32.exe**: Deze waarschuwing geeft aan dat rundll32.exe is gebruikt voor het uitvoeren van een proces met een ongebruikelijke naam, consistent zijn met het proces naamgevingsschema die door aanvallers worden gebruikt voor het installeren van een eerste fase implantatie op een geïnfecteerde host.
* **Verdachte combinatie van HTA en PowerShell**: Deze waarschuwing geeft aan dat een Microsoft HTML Application Host (HTA) PowerShell-opdrachten worden gestart. Dit is een techniek die aanvallers gebruiken om schadelijke PowerShell-scripts te starten.
* **Wijzigen in een registersleutel die can be abused to bypass UAC**: Deze waarschuwing geeft aan dat een registersleutel op die can be abused to bypass UAC (User Account Control) is gewijzigd, die vaak door aanvallers wordt gebruikt om te verplaatsen van niet-gemachtigd (standaardgebruiker) voor toegang tot de bevoegdheden (bijvoorbeeld administrator) op een geïnfecteerde host.
* **Gebruik van verdachte domeinnaam binnen vanaf de opdrachtregel**: Deze waarschuwing geeft aan dat een verdachte domeinnaam is gebruikt, wat kan duiden dat een aanvaller zijn als host fungeert voor schadelijke programma's en als eindpunten voor de opdracht en controle en gegevensexfiltratie.
* **Een account is gemaakt op meerdere hosts binnen een periode van 24-uurs**: Deze waarschuwing geeft aan dat een poging gedaan om te maken van hetzelfde gebruikersaccount op meerdere hosts, dit kunnen duiden dat een aanvaller zich lateraal verplaatst binnen het netwerk nadat een of meerdere netwerkentiteiten zijn aangetast.
* **Verdacht gebruik van CACLS te verlagen van de beveiligingsstatus van het systeem**: Deze waarschuwing geeft aan dat de wijziging access control list (CACLS) is gewijzigd. Deze techniek wordt vaak gebruikt door aanvallers om volledige toegang te verlenen tot binaire systeembestanden zoals ftp.exe, net.exe wscript.exe, enzovoort.
* **Verdacht Kerberos Golden Ticket-aanval parameters**: Deze waarschuwing geeft aan dat opdrachtregelparameters consistent zijn met een Kerberos Golden Ticket-aanval zijn uitgevoerd. Een verdachte krbtgt-sleutel kan door een aanvaller worden gebruikt om elke gewenste gebruiker te imiteren.
* **Inschakelen van de registersleutel WDigest UseLogonCredential**: Deze waarschuwing geeft aan dat de registersleutel is gewijzigd zodat teken in de referenties op die moeten worden opgeslagen als leesbare tekst in LSA-geheugen, dat vervolgens kan worden verzameld uit het geheugen.
* **Mogelijk verdacht gebruik van Telegram tool**: Deze waarschuwing geeft aan dat de installatie van Telegram, een gratis cloudgebaseerde instant messaging-service wordt gebruikt door aanvallers om over te dragen van schadelijke binaire bestanden naar een andere computer, telefoon of tablet.
* **Maken van nieuwe ASEP**: Deze waarschuwing geeft aan dat het maken van een nieuwe ASEP (Auto Start Extensibility Point), waardoor de procesnaam die is geïdentificeerd in de opdrachtregel automatisch wordt gestart en door een aanvaller kan worden gebruikt voor persistentie.
* **Suspicious Set-ExecutionPolicy and WinRM wijzigingen**: Deze waarschuwing geeft aan dat de wijzigingen in de configuratie die gekoppeld aan het gebruik van de schadelijke ChinaChopper-webshell zijn.
* **Uitschakelen van kritieke services**: Deze waarschuwing geeft aan dat de opdracht "net.exe stop" essentiële services als SharedAccess of Windows Security Center te stoppen is gebruikt.
* **Verdacht gebruik van de FTP -s switch**: Deze waarschuwing duidt op het gebruik van s "-s" overschakelen, die kan worden gebruikt door malware om te verbinden met een externe ftp_server en schadelijke binaire bestanden downloaden.
* **Verdachte uitvoering van de opdracht VBScript.Encode**: Deze waarschuwing geeft aan dat de *VBScript.Encode* opdracht is uitgevoerd, waarmee scripts in onleesbare tekst, waardoor het moeilijker voor gebruikers om de code te onderzoeken.
* **VBScript HTTP object allocation**: Deze waarschuwing geeft aan dat het maken van een VBScript-bestand vanaf een opdrachtprompt; dat kan worden gebruikt om schadelijke bestanden te downloaden.
* **Plaktoetsen aanval**: Deze waarschuwing geeft aan dat een aanvaller kan toegankelijkheidsbestand binaire (bijvoorbeeld Plaktoetsen, toetsenbord op het scherm, Verteller) om via een achterdeur toegang te verschaffen.
* **Petya ransomware indicators**: Deze waarschuwing geeft aan dat de technieken die zijn gekoppeld aan Petya-ransomware zijn waargenomen.
* **Poging tot het uitschakelen van AMSI**: Deze waarschuwing wijst op een poging om uit te schakelen van de antimalware scaninterface (AMSI), die detectie van antimalware wordt uitgeschakeld.
* **Ransomware indicators**: Deze waarschuwing wijst op een verdachte activiteit traditioneel die zijn gekoppeld aan ransomware scherm vergrendelen en versleuteling.
* **Suspicious trace collection uitvoerbestand**: Deze waarschuwing geeft aan dat een tracering (bijvoorbeeld van netwerkactiviteit) is verzameld en de uitvoer naar een ongebruikelijk bestandstype.
* **Software met een hoog risico**: Deze waarschuwing duidt op het gebruik van de software die gekoppeld aan de installatie van malware is. Aanvallers verpakken malware vaak met anderszins goedaardige hulpprogramma's, zoals het programma dat wordt gemeld in deze waarschuwing, en installeren de malware in de achtergrond.
* **Het maken van verdachte bestanden**: Deze waarschuwing geeft aan dat is gemaakt of uitgevoerd van een proces dat wordt gebruikt door aanvallers aanvullende kwaadaardige software downloaden naar een geïnfecteerde host nadat een bijlage in een phishing-document is geopend.
* **Verdachte referenties in vanaf de opdrachtregel**: Deze waarschuwing geeft aan een verdacht wachtwoord dat is gebruikt voor het uitvoeren van een bestand. Deze techniek wordt gebruikt door aanvallers om de Pirpi-malware uit te voeren.
* **Kan de uitvoering van malware dropper**: Deze waarschuwing geeft aan dat een bestandsnaam die is gebruikt door aanvallers om malware te installeren.
* **Verdachte uitvoeren via rundll32.exe**: Deze waarschuwing geeft aan dat rundll32.exe wordt gebruikt voor het uitvoeren van een notepad.exe of reg.exe, consistent zijn met de proces-injectietechniek die aanvallers gebruiken.
* **Verdachte opdrachtregelargumenten**: Deze waarschuwing wijst op verdachte opdrachtregelargumenten die zijn gebruikt in combinatie met een omgekeerde shell die door activiteitengroep hydrogen wordt gebruikt.
* **Suspicious document referenties**: Deze waarschuwing wijst op een verdachte, vooraf berekende algemene wachtwoordhash die worden gebruikt door schadelijke software wordt gebruikt voor het uitvoeren van een bestand.
* **Dynamic PS script bouw**: Deze waarschuwing geeft aan dat een PowerShell-script dynamisch wordt samengesteld. Aanvallers gebruiken deze techniek om geleidelijk een script te maken om IDS-systemen te omzeilen.
* **Metasploit indicatoren**: Deze waarschuwing wijst op een activiteit die is gekoppeld aan het Metasploit-framework, waarmee u een scala aan mogelijkheden en hulpprogramma's.
* **Verdachte accountactiviteit**: Deze waarschuwing wijst op een poging verbinding maken met een machine met een account dat onlangs is aangetast.
* **Maken van het account**: Deze waarschuwing geeft aan dat een nieuw account op de machine.


### <a name="crash-analysis"></a>Crashanalyse


De analyse van een crashdumpgeheugen is een methode voor het detecteren van geavanceerde schadelijke software die in staat is traditionele beveiligingsoplossingen te misleiden. Diverse varianten schadelijke software proberen de kansen dat ze worden gedetecteerd door antivirusproducten te verkleinen doordat ze nooit iets naar de schijf schrijven of door softwareonderdelen te versleutelen die naar de schijf worden geschreven. Deze techniek bemoeilijkt de detectie van malware met traditionele antimalware-benaderingen. Echter kan dergelijke schadelijke software worden gedetecteerd met behulp van een geheugenanalyse, omdat de malware traceringen in het geheugen moet achterlaten om te kunnen werken.

Wanneer software vastloopt, bevat een crashdump een gedeelte van het geheugen ten tijde van de crash. De crash kan zijn veroorzaakt door malware, door gewoon gebruik of door systeemproblemen. Door het geheugen in de crashdump te analyseren, kan Security Center technieken detecteren die worden gebruikt om misbruik te maken van zwakke plekken in software, toegang te krijgen tot vertrouwelijke gegevens en ongemerkt aanwezig te blijven in een aangetaste machine. Een crashanalyse wordt uitgevoerd met minimale gevolgen voor de prestaties van hosts omdat deze wordt uitgevoerd door de back-end van Security Center.

* **Code-injectie gedetecteerd**: Code-injectie is het invoegen van uitvoerbare modules in actieve processen of threads. Deze techniek wordt gebruikt door malware om toegang tot gegevens, verbergen of te voorkomen dat deze wordt verwijderd (bijvoorbeeld persistentie). Met deze waarschuwing wordt aangegeven dat de crashdump een geïnjecteerde module bevat. Legitieme softwareontwikkelaars voeren af en toe code-injectie uit voor niet-kwaadwillende redenen, bijvoorbeeld om een bestaande toepassing of een bestaand besturingssysteemonderdeel te wijzigen of uit te breiden. Om onderscheid te kunnen maken tussen schadelijke en niet-schadelijke geïnjecteerde modules, controleert Security Center of de geïnjecteerde module voldoet aan een profiel met verdacht gedrag. Het resultaat van deze controle wordt aangegeven door het veld "SIGNATURE" van de waarschuwing en is te zien aan de ernst van de waarschuwing, de omschrijving van de waarschuwing en herstelstappen voor de waarschuwing.
* **Verdachte codesegment**: De waarschuwing geeft aan dat een codesegment is toegewezen met behulp van niet-standaardmethoden, zoals die worden gebruikt bij reflectieve injectie en procesuitholling. Aanvullende kenmerken van het codesegment worden verwerkt voor meer context over de mogelijkheden en het gedrag van het gerapporteerde codesegment.
* **Shellcode gedetecteerd**: Shellcode is de nettolading die wordt uitgevoerd nadat de schadelijke software misbruik heeft gemaakt van een beveiligingslek in de software. Deze waarschuwing geeft aan dat de crashdumpanalyse uitvoerbare code heeft gedetecteerd dat gedrag vertoont dat gewoonlijk wordt uitgevoerd door schadelijke nettoladingen. Hoewel niet-schadelijke software dit gedrag kan vertonen, is het niet gebruikelijk voor normale softwareontwikkelingsprocedures.
* **Module-hijacking gedetecteerd**: Windows maakt gebruik van Dynamic Link Libraries (DLL's) om software toe te staan gebruik te maken van de algemene Windows-systeemfunctionaliteit. DLL-hijacking treedt op wanneer malware de DLL-laadvolgorde wijzigt om schadelijke nettoladingen in het geheugen te laden, waarbij willekeurige code kan worden uitgevoerd. Deze waarschuwing geeft aan dat de crashdumpanalyse een gelijknamige module heeft gedetecteerd die wordt geladen via twee verschillende paden. Een van de geladen paden is afkomstig van een algemene binaire locatie van het Windows-systeem. Van tijd tot tijd wijzigen legitieme softwareontwikkelaars de laadvolgorde van de DLL wegens niet-kwaadwillende redenen, zoals instrumentering, uitbreiding van het Windows-besturingssysteem of Windows-toepassingen. Om te helpen onderscheid te maken tussen kwaadwillende en mogelijk goedaardige wijzigingen in de laadvolgorde van het DLL-bestand controleert Security Center of een geladen module aan een verdacht profiel voldoet.
* **Onechte Windows-module gedetecteerd**: Malware kan veelvoorkomende namen van binaire Windows-systeembestanden (bijvoorbeeld SVCHOST gebruiken. EXE) of modules (bijvoorbeeld NTDLL. DLL-bestand) te combineren en de aard van de schadelijke software voor systeembeheerders worden verborgen. Deze waarschuwing geeft aan dat het crashdumpbestand modules bevat die namen van Windows-systeemmodules gebruiken, maar niet voldoen aan andere criteria die gangbaar zijn voor Windows-modules. Analyse van de kopie op schijf van de onechte module kan mogelijk meer informatie bieden over de legitieme of schadelijke aard van deze module.
* **Gewijzigd binair systeembestand gedetecteerd**: Malware kan belangrijke binaire systeembestanden wijzigen om stiekem toegang tot gegevens te krijgen of ongemerkt aanwezig te blijven op een geïnfecteerd systeem. Deze waarschuwing geeft aan dat de crashdumpanalyse heeft gedetecteerd dat belangrijke binaire Windows-besturingssysteembestanden zijn gewijzigd in het geheugen of op schijf. Legitieme softwareontwikkelaars wijzigen van tijd tot tijd systeemmodules in het geheugen voor niet-kwaadwillende redenen, zoals omzeilingen of voor de compatibiliteit van toepassingen. Om te helpen onderscheid te maken tussen kwaadwillende en mogelijk goedaardige modules, controleert Security Center of de gewijzigde module aan een verdacht profiel voldoet.

## <a name="network-analysis"></a>Netwerkanalyse
Het detecteren van netwerkbedreigingen van Security Center werkt volgens het automatisch verzamelen van beveiligingsgegevens van uw Azure IPFIX-verkeer (Internet Protocol Flow Information Export). Deze informatie wordt door Security Center geanalyseerd, waarbij vaak informatie uit meerdere bronnen wordt samengebracht om bedreigingen te analyseren.

* **Mogelijke incoming SQL brute force pogingen**: Netwerkverkeeranalyse heeft verdachte binnenkomende SQL-communicatie gedetecteerd. Deze activiteit komt overeen met aanhoudende aanvalspogingen tegen SQL-servers.
* **Verdachte binnenkomende RDP network activity uit meerdere bronnen**: Netwerkverkeeranalyse heeft afwijkende binnenkomende Remote Desktop Protocol (RDP)-communicatie uit meerdere bronnen gedetecteerd. De voorbeeldnetwerkgegevens tonen unieke IP-adressen die verbinding maken met uw computer, wat abnormaal is voor deze omgeving. Deze activiteit kan wijzen op een poging om uw RDP-eindpunt vanaf meerdere hosts (Botnet) aan te vallen.
* **Verdachte binnenkomende RDP network activity**: Netwerkverkeeranalyse heeft afwijkende binnenkomende Remote Desktop Protocol (RDP)-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen een hoog aantal binnenkomende verbindingen naar uw computer, wat abnormaal is voor deze omgeving. Deze activiteit kan wijzen op een poging om uw RDP-eindpunt aan te vallen.
* **Suspicious outgoing RDP network activity naar meerdere bestemmingen**: Netwerkverkeeranalyse heeft afwijkende uitgaande Remote Desktop Protocol (RDP)-communicatie naar meerdere bestemmingen gedetecteerd. Dit kan erop wijzen dat uw computer is geïnfecteerd en wordt gebruikt om externe RDP-eindpunten herhaaldelijk aan te vallen. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.
* **Suspicious outgoing RDP network activity**: Netwerkverkeeranalyse heeft afwijkende uitgaande Remote Desktop Protocol (RDP)-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen een hoog aantal uitgaande verbindingen vanaf uw computer, wat abnormaal is voor deze omgeving. Dit kan erop wijzen dat uw computer is geïnfecteerd en wordt gebruikt om externe RDP-eindpunten herhaaldelijk aan te vallen. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.
* **Verdachte binnenkomende SSH network activity**: Netwerkverkeeranalyse heeft afwijkende binnenkomende SSH-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen een hoog aantal binnenkomende verbindingen naar uw computer, wat abnormaal is voor deze omgeving. Deze activiteit kan wijzen op een poging om uw SSH-eindpunt aan te vallen.
* **Verdachte binnenkomende SSH network activity uit meerdere bronnen**: Netwerkverkeeranalyse heeft afwijkende binnenkomende SSH-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen unieke IP-adressen die verbinding maken met uw computer, wat abnormaal is voor deze omgeving. Deze activiteit kan wijzen op een poging om uw SSH-eindpunt vanaf meerdere hosts (Botnet) aan te vallen.
* **Suspicious outgoing SSH network activity**: Netwerkverkeeranalyse heeft afwijkende uitgaande SSH-communicatie gedetecteerd. De voorbeeldnetwerkgegevens tonen een hoog aantal uitgaande verbindingen vanaf uw computer, wat abnormaal is voor deze omgeving. Dit kan erop wijzen dat uw computer is geïnfecteerd en wordt gebruikt om externe SSH-eindpunten herhaaldelijk aan te vallen. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.
* **Suspicious outgoing SSH network activity naar meerdere bestemmingen**: Netwerkverkeeranalyse heeft afwijkende uitgaande SSH-communicatie naar meerdere bestemmingen gedetecteerd. De voorbeeldnetwerkgegevens geven aan dat uw computer verbinding maakt met unieke IP-adressen, wat abnormaal is voor deze omgeving. Dit kan erop wijzen dat uw computer is geïnfecteerd en wordt gebruikt om externe SSH-eindpunten herhaaldelijk aan te vallen. Houd er rekening mee dat dit type activiteit ervoor kan zorgen dat uw IP-adres wordt gemarkeerd als kwaadaardig bij externe entiteiten.
* **Netwerkcommunicatie met een schadelijke computer gedetecteerd**: Netwerkverkeeranalyse geeft aan dat de computer heeft gecommuniceerd met wat mogelijk een Command and Control center is.
* **Mogelijk aangetast computer gedetecteerd**: Netwerkverkeeranalyse heeft uitgaande activiteit gedecteerd, wat erop kan wijzen fungeert als onderdeel van een botnet. De analyse is gebaseerd op IP-adressen die door uw resource worden benaderd en openbare DNS-records.


## <a name="sql-database-and-sql-data-warehouse-analysis"></a>SQL Database- en SQL Data Warehouse-analyse

Een resourceanalyse door Security Center richt zich op PaaS-services (Platform as a Service), zoals de integratie met [Azure SQL Database-Detectie van Bedreigingen](../sql-database/sql-database-threat-detection.md) en Azure SQL Data Warehouse. SQL Threat Detection detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen aangeven om toegang te krijgen of misbruik te maken van databases en activeert de volgende waarschuwingen:

* **Vulnerability to SQL Injection**: Deze waarschuwing wordt geactiveerd wanneer een toepassing een foutieve SQL-instructie in de database genereert. Deze waarschuwing kan duiden op mogelijke kwetsbaarheid voor SQL-injectieaanvallen. Er zijn twee mogelijke redenen voor het genereren van een foutieve instructie:
    * Een fout in de toepassingscode die de foutieve SQL-instructie maakt
    * Toepassingscode of opgeslagen procedures schonen gebruikersinvoer niet op tijdens het construeren van de foutieve SQL-instructie, dit kan worden misbruikt voor SQL-injectie
* **Mogelijke SQL-injectie**: Deze waarschuwing wordt geactiveerd wanneer een actieve aanval wordt uitgevoerd tegen een geïdentificeerde toepassing beveiligingslek voor SQL-injectie. Dit betekent dat de aanvaller schadelijke SQL-instructies probeert te injecteren met de kwetsbare toepassingscode of opgeslagen procedures.
* **Toegang vanaf ongebruikelijke locatie**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de SQL server, waarbij iemand heeft aangemeld bij de SQL server vanuit een ongebruikelijke geografische locatie. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
* **Toegang vanaf ongebruikelijk Azure-datacentrum**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de SQL server, waarbij iemand heeft aangemeld bij de SQL server vanuit een ongebruikelijke Azure-datacenter die tijdens de recente periode is gedetecteerd op deze server. In sommige gevallen detecteert de waarschuwing een legitieme actie (uw nieuwe toepassing in Azure, Power BI, Azure SQL Query-Editor). In andere gevallen detecteert de waarschuwing een schadelijke actie vanuit een Azure resource/service (voormalig werknemer, externe aanvaller).
* **Toegang vanaf ongebruikelijke**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de SQL server, waarbij iemand zich heeft aangemeld met de SQL-server met behulp van een ongebruikelijke klant (SQL-gebruiker). In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of onderhoud door ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller).
* **Toegang vanaf een mogelijk schadelijke toepassing**: Deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een aanval met behulp van gebruikelijk aanvalsprogramma’s.
* **Brute force SQL-referenties**: Deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties. In sommige gevallen detecteert de waarschuwing het uitvoeren van testen om binnen te dringen. In andere gevallen detecteert de waarschuwing een Brute Force-aanval.

## <a name="contextual-information"></a>Contextuele informatie
Tijdens een onderzoek hebben analisten extra context nodig om tot een eindoordeel te komen over de aard van de bedreiging en hoe deze kan worden tegengehouden.  Als er bijvoorbeeld een afwijkende situatie in het netwerk is gedetecteerd, maar er geen inzicht is in wat er verder gebeurt in het netwerk of met de betreffende resource, is het zeer lastig om te begrijpen welke vervolgacties nodig zijn. Om te helpen bij die, kan een beveiligingsincident artefacten, gerelateerde gebeurtenissen en informatie die nuttig de onderzoeker kan bevatten. De beschikbaarheid van extra informatie varieert afhankelijk van het type bedreiging dat is gedetecteerd en de configuratie van uw omgeving, en zal dus niet voor alle beveiligingsincidenten beschikbaar zijn.

Als er extra informatie beschikbaar is, wordt deze weergegeven in het beveiligingsincident onder de lijst met waarschuwingen. Dit kan informatie zijn zoals:

- Wissen van logboekgebeurtenissen
- PNP-apparaat aangesloten vanaf onbekend apparaat
- Waarschuwingen die geen bruikbare
- Het maken van nieuwe accounts
- Bestand gedecodeerd met het hulpprogramma certutil

![Waarschuwing over ongebruikelijke toegang](./media/security-center-alerts-type/security-center-alerts-type-fig20.png)


## <a name="next-steps"></a>Volgende stappen
In dit artikel bent u meer te weten gekomen over de verschillende soorten beveiligingswaarschuwingen in Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsincidenten afhandelen in Azure Security Center](security-center-incident.md)
* [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md)
* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): Raadpleeg de blogberichten over beveiliging en naleving in Azure.
