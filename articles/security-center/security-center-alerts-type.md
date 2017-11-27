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
ms.date: 11/22/2017
ms.author: yurid
ms.openlocfilehash: 829657664cf1e37b22d57c62614300a205b5e91c
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
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
* Resourceanalyse
* Contextuele informatie

## <a name="virtual-machine-behavioral-analysis"></a>VMBA (Virtual Machine Behavioral Analysis)
Azure Security Center kan gebruikmaken van gedragsanalyses om verdachte resources te identificeren op basis van de analyse van gebeurtenislogboeken van virtuele machines. Bijvoorbeeld procesgebeurtenissen en aanmeldgebeurtenissen. Er wordt ook gekeken naar andere signalen om te controleren op ondersteunend bewijs van een wijdverbreide aanval.

> [!NOTE]
> Lees [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md) voor meer informatie over de werking van de detectiemogelijkheden van Security Center.
>

### <a name="crash-analysis"></a>Crashanalyse
De analyse van een crashdumpgeheugen is een methode voor het detecteren van geavanceerde schadelijke software die in staat is traditionele beveiligingsoplossingen te misleiden. Diverse varianten schadelijke software proberen de kansen dat ze worden gedetecteerd door antivirusproducten te verkleinen doordat ze nooit iets naar de schijf schrijven of door softwareonderdelen te versleutelen die naar de schijf worden geschreven. Deze techniek bemoeilijkt de detectie van malware met traditionele antimalware-benaderingen. Echter kan dergelijke schadelijke software worden gedetecteerd met behulp van een geheugenanalyse, omdat de malware traceringen in het geheugen moet achterlaten om te kunnen werken.

Wanneer software vastloopt, bevat een crashdump een gedeelte van het geheugen ten tijde van de crash. De crash kan zijn veroorzaakt door malware, door gewoon gebruik of door systeemproblemen. Door het geheugen in de crashdump te analyseren, kan Security Center technieken detecteren die worden gebruikt om misbruik te maken van zwakke plekken in software, toegang te krijgen tot vertrouwelijke gegevens en ongemerkt aanwezig te blijven in een aangetaste machine. Een crashanalyse wordt uitgevoerd met minimale gevolgen voor de prestaties van hosts omdat deze wordt uitgevoerd door de back-end van Security Center.

De volgende velden worden vaak weergegeven in de voorbeelden van waarschuwingen voor crashdumps die verderop in dit artikel worden behandeld:

* DUMPFILE: naam van het crashdumpbestand.
* PROCESSNAME: naam van het vastgelopen proces.
* PROCESSVERSION: versie van het vastgelopen proces.

### <a name="code-injection-discovered"></a>Code-injectie gedetecteerd
Code-injectie is het invoegen van uitvoerbare modules in actieve processen of threads.  Deze methode wordt gebruikt door schadelijke software om toegang krijgen tot gegevens, zich te verbergen of te voorkomen dat deze wordt verwijderd (bijvoorbeeld persistentie). Met deze waarschuwing wordt aangegeven dat de crashdump een geïnjecteerde module bevat. Legitieme softwareontwikkelaars voeren af en toe code-injectie uit voor niet-kwaadwillende redenen, bijvoorbeeld om een bestaande toepassing of een bestaand besturingssysteemonderdeel te wijzigen of uit te breiden.  Om onderscheid te kunnen maken tussen schadelijke en niet-schadelijke geïnjecteerde modules, controleert Security Center of de geïnjecteerde module voldoet aan een profiel met verdacht gedrag. Het resultaat van deze controle wordt aangegeven door het veld "SIGNATURE" van de waarschuwing en is te zien aan de ernst van de waarschuwing, de omschrijving van de waarschuwing en herstelstappen voor de waarschuwing. 

Deze waarschuwing bevat de volgende extra velden:

- ADDRESS: De locatie in het geheugen van de geïnjecteerde module
- IMAGENAME: De naam van de geïnjecteerde module. Deze kan leeg zijn als de naam van de installatiekopie niet is opgegeven in de installatiekopie.
- SIGNATURE: Hiermee wordt aangegeven of de geïnjecteerde module voldoet aan een profiel met verdacht gedrag. 

In de onderstaande tabel ziet u voorbeelden van mogelijke resultaten en de bijbehorende beschrijving:

| Waarde handtekening                      | Beschrijving                                                                                                       |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Verdacht misbruik door reflective loader | Dit verdachte gedrag komt vaak overeen met het laden van geïnjecteerde code, onafhankelijk van het laadprogramma van het besturingssysteem |
| Verdacht misbruik via injectie          | Geeft een aanval aan die vaak overeenkomt met het injecteren van code in het geheugen                                       |
| Verdachte injectie-aanval         | Geeft een aanval aan die vaak overeenkomt met het gebruik van geïnjecteerde code in het geheugen                                   |
| Verdachte geïnjecteerde foutopsporingsprogramma-aanval | Geeft een aanval aan die vaak overeenkomt met detectie of omzeiling van een foutopsporingsprogramma                         |
| Verdachte injectie-aanval op afstand   | Geeft een aanval aan die vaak overeenkomt met command-n-control-scenario’s (C2)                                 |

Hier volgt een voorbeeld van dit type waarschuwing:

![Waarschuwing voor code-injectie](./media/security-center-alerts-type/security-center-alerts-type-fig21.png)

### <a name="suspicious-code-segment"></a>Verdacht codesegment
Hiermee wordt aangegeven dat een codesegment is toegewezen met behulp van niet-standaardmethoden, zoals die worden gebruikt bij reflectieve injectie en procesuitholling.  Deze waarschuwing bevat ook aanvullende kenmerken van het codesegment voor meer context over de mogelijkheden en het gedrag van het gerapporteerde codesegment.

Deze waarschuwing bevat de volgende extra velden:

- ADDRESS: De locatie in het geheugen van de geïnjecteerde module
- SIZE: De grootte van het verdachte codesegment
- STRINGSIGNATURES: In dit veld ziet u een lijst met mogelijkheden van API's waarvan functienamen zijn opgenomen in het codesegment. Voorbeelden van mogelijkheden zijn:
    - Descriptoren voor secties van installatiekopieën, dynamische code uitvoeren voor x64, mogelijkheid voor het toewijzen en laden van geheugen, mogelijkheid voor het injecteren van externe code, mogelijkheid tot het overnemen van de besturing, omgevingsvariabelen lezen, geheugen voor willekeurige processen lezen, tokenbevoegdheden opvragen of wijzigen, HTTP-/HTTPS-netwerkcommunicatie en netwerksocketcommunicatie.
- IMAGEDETECTED: In dit veld wordt aangegeven of een PE-installatiekopie is geïnjecteerd in het proces waarin het verdachte codesegment is gedetecteerd, en op welk adres de geïnjecteerde module wordt gestart.
- SHELLCODE: In dit veld wordt de aanwezigheid aangegeven van gedrag dat vaak wordt gebruikt door schadelijke nettoladingen om toegang te krijgen tot extra beveiligingsgevoelige functies van het besturingssysteem. 

Hier volgt een voorbeeld van dit type waarschuwing:

![Waarschuwing voor verdacht codesegment](./media/security-center-alerts-type/security-center-alerts-type-fig22.png)

### <a name="shellcode-discovered"></a>Shellcode gedetecteerd
Shellcode is de nettolading die wordt uitgevoerd nadat de schadelijke software misbruik heeft gemaakt van een beveiligingslek in de software. Deze waarschuwing geeft aan dat de crashdumpanalyse uitvoerbare code heeft gedetecteerd dat gedrag vertoont dat gewoonlijk wordt uitgevoerd door schadelijke nettoladingen. Hoewel niet-schadelijke software dit gedrag kan vertonen, is het niet gebruikelijk voor normale softwareontwikkelingsprocedures.

Dit voorbeeld van een Shellcode-waarschuwing geeft het volgende extra veld:

* ADDRESS: de locatie in het geheugen van de shellcode.

Hier volgt een voorbeeld van dit type waarschuwing:

![Waarschuwing voor shellcode](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Module-hijacking gedetecteerd
Windows maakt gebruik van Dynamic Link Libraries (DLL's) om software toe te staan gebruik te maken van de algemene Windows-systeemfunctionaliteit. DLL-hijacking treedt op wanneer malware de DLL-laadvolgorde wijzigt om schadelijke nettoladingen in het geheugen te laden, waarbij willekeurige code kan worden uitgevoerd. Deze waarschuwing geeft aan dat de crashdumpanalyse een gelijknamige module heeft gedetecteerd die wordt geladen via twee verschillende paden. Een van de geladen paden is afkomstig van een algemene binaire locatie van het Windows-systeem.

Van tijd tot tijd wijzigen legitieme softwareontwikkelaars de laadvolgorde van de DLL wegens niet-kwaadwillende redenen, zoals instrumentering, uitbreiding van het Windows-besturingssysteem of Windows-toepassingen. Om te helpen onderscheid te maken tussen kwaadwillende en mogelijk goedaardige wijzigingen in de laadvolgorde van de DLL-bestand, controleert Azure Security Center of een geladen module aan een verdacht profiel voldoet. Het resultaat van deze controle wordt aangegeven door het veld "SIGNATURE" van de waarschuwing en is te zien aan de ernst van de waarschuwing, de omschrijving van de waarschuwing en herstelstappen voor de waarschuwing. Onderzoek of de module betrouwbaar of schadelijk is door de kopie op de schijf van de hijacking-module te analyseren. U kunt bijvoorbeeld de digitale handtekening van het bestand controleren of een antivirusprogramma een scan laten uitvoeren.

Naast de algemene velden die eerder zijn beschreven in het gedeelte "Shellcode gedetecteerd", biedt deze waarschuwing de volgende velden:

* SIGNATURE: geeft aan of de hijacking-module aan een verdacht profiel voldoet.
* HIJACKEDMODULE: de naam van de gehijackte Windows-systeemmodule.
* HIJACKEDMODULEPATH: het pad van de gehijackte Windows-systeemmodule.
* HIJACKINGMODULEPATH: het pad van de hijacking-module.

Hier volgt een voorbeeld van dit type waarschuwing:

![Module-hijackingswaarschuwing](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Onechte Windows-module gedetecteerd
Malware kan veelvoorkomende namen van binaire Windows-systeembestanden (bijvoorbeeld SVCHOST.EXE) of modules (bijvoorbeeld NTDLL.DLL) gebruiken om *ongemerkt* binnen te komen en de aard van de schadelijke software voor systeembeheerders te verbergen. Deze waarschuwing geeft aan dat de crashdumpanalyse heeft gedetecteerd dat het crashdumpbestand modules bevat die namen van Windows-systeemmodules gebruiken, maar niet voldoen aan andere criteria die gangbaar zijn voor Windows-modules. Analyse van de kopie op schijf van de onechte module kan mogelijk meer informatie bieden over de legitieme of schadelijke aard van deze module. De analyse kan het volgende omvatten:

* Bevestig dat het bestand in kwestie is geleverd als onderdeel van een betrouwbaar softwarepakket.
* Controleer de digitale handtekening van het bestand.
* Voer een antivirusprogramma uit op het bestand.

Naast de algemene velden die eerder zijn beschreven in het gedeelte "Shellcode gedetecteerd" biedt deze waarschuwing de volgende aanvullende velden:

* DETAILS: beschrijft of de metagegevens van de modules geldig zijn en of de module vanuit een systeempad is geladen.
* NAME: de naam van de onechte Windows-module.
* PATH: het pad naar de onechte Windows-module.

Deze waarschuwing extraheert en toont ook bepaalde velden uit de PE-kop van de module, zoals "CHECKSUM" en "TIMESTAMP". Deze velden worden alleen weergegeven als de velden in de module zijn opgenomen. Zie de [Microsoft PE- en COFF-specificatie](https://msdn.microsoft.com/windows/hardware/gg463119.aspx) voor meer informatie over deze velden.

Hier volgt een voorbeeld van dit type waarschuwing:

![Onechte Windows-waarschuwing](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Gewijzigd binair systeembestand gedetecteerd
Malware kan belangrijke binaire systeembestanden wijzigen om stiekem toegang tot gegevens te krijgen of ongemerkt aanwezig te blijven op een geïnfecteerd systeem. Deze waarschuwing geeft aan dat de crashdumpanalyse heeft gedetecteerd dat belangrijke binaire Windows-besturingssysteembestanden zijn gewijzigd in het geheugen of op schijf.

Legitieme softwareontwikkelaars wijzigen van tijd tot tijd systeemmodules in het geheugen voor niet-kwaadwillende redenen, zoals omzeilingen of voor de compatibiliteit van toepassingen. Om te helpen onderscheid te maken tussen kwaadwillende en mogelijk goedaardige modules, controleert Azure Security Center of de gewijzigde module aan een verdacht profiel voldoet. Het resultaat van deze controle wordt aangegeven door de ernst van waarschuwing, de omschrijving van de waarschuwing en de herstelstappen voor de waarschuwing.

Naast de algemene velden die eerder zijn beschreven in het gedeelte "Shellcode gedetecteerd" biedt deze waarschuwing de volgende aanvullende velden:

* MODULENAME: de naam van het gewijzigde binaire systeembestand.
* MODULEVERSION: de versie van het gewijzigde binaire systeembestand.

Hier volgt een voorbeeld van dit type waarschuwing:

![Waarschuwing voor binair systeembestand](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Verdachte processen uitgevoerd
Security Center stelt vast dat er een verdacht proces wordt uitgevoerd in de virtuele doelmachine en activeert een waarschuwing. De detectie zoekt niet op de specifieke naam niet ziet, maar op de parameter van het uitvoerbare bestand. Daarom kan Security Center het verdachte proces zelfs detecteren als de aanvaller de naam van het uitvoerbare bestand wijzigt.

Hier volgt een voorbeeld van dit type waarschuwing:

![Waarschuwing dat er verdachte processen worden uitgevoerd](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domains-accounts-queried"></a>Query's op meerdere domeinaccounts
Security Center kan meerdere querypogingen voor Active Directory-domeinaccounts detecteren, iets wat aanvallers doorgaans doen om een netwerk te verkennen. Aanvallers kunnen deze methode gebruiken om query's uit te voeren op het domein met het doel de gebruikers te identificeren, wat de beheerdersaccounts van het domein zijn, welke computers domeincontrollers zijn en of er mogelijk vertrouwensrelaties zijn tussen het domein en andere domeinen.

Hier volgt een voorbeeld van dit type waarschuwing:

![Waarschuwing over meerdere domeinaccounts](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

### <a name="local-administrators-group-members-were-enumerated"></a>Leden van de groep Lokale beheerders zijn geïnventariseerd

In Security Center wordt een waarschuwing geactiveerd wanneer in Windows Server 2016 of Windows 10 beveiligingsgebeurtenis 4798 is geactiveerd. Dit gebeurt wanneer lokale-beheerdersgroepen worden geïnventariseerd, iets wat aanvallers doorgaans doen om het netwerk te verkennen. Aanvallers kunnen deze techniek gebruiken om de identiteit op te vragen van gebruikers met beheerdersrechten.

Hier volgt een voorbeeld van dit type waarschuwing:

![Lokale beheerder](./media/security-center-alerts-type/security-center-alerts-type-fig14-new.png)

### <a name="anomalous-mix-of-upper-and-lower-case-characters"></a>Ongebruikelijke combinatie van hoofdletters en kleine letters

In Security Center wordt een waarschuwing geactiveerd wanneer het gebruik van een combinatie van hoofdletters en kleine letters op de opdrachtregel wordt gedetecteerd. Sommige aanvallers gebruiken deze techniek om de hoofdlettergevoelige of hash-gebaseerde machineregel te ontlopen.

Hier volgt een voorbeeld van dit type waarschuwing:

![Ongebruikelijke combinatie](./media/security-center-alerts-type/security-center-alerts-type-fig15-new.png)

### <a name="suspected-kerberos-golden-ticket-attack"></a>Verdachte Golden Ticket Kerberos-aanval

Een verdachte [krbtgt](https://technet.microsoft.com/library/dn745899.aspx)-sleutel kan door een aanvaller worden gebruikt om Kerberos Golden Tickets te maken, waarmee de aanvaller elke gewenste gebruiker kan imiteren. In Security Center wordt een waarschuwing geactiveerd wanneer dit type activiteit wordt gedetecteerd.

> [!NOTE] 
> Meer informatie over Kerberos Golden Tickets vindt u in de [Windows 10 credential theft mitigation guide](http://download.microsoft.com/download/C/1/4/C14579CA-E564-4743-8B51-61C0882662AC/Windows%2010%20credential%20theft%20mitigation%20guide.docx) (Handleiding voor beperking van het risico op referentiediefstal in Windows 10).

Hier volgt een voorbeeld van dit type waarschuwing:

![Golden Ticket](./media/security-center-alerts-type/security-center-alerts-type-fig16-new.png)

### <a name="suspicious-account-created"></a>Verdacht account gemaakt

In Security Center wordt een waarschuwing geactiveerd wanneer er een account wordt gemaakt dat veel overeenkomsten vertoont met een bestaand ingebouwd account met beheerdersbevoegdheden. Deze techniek kan door aanvallers worden gebruikt om een rogue-account te maken en te voorkomen dat het door menselijke verificatie wordt opgemerkt.
 
Hier volgt een voorbeeld van dit type waarschuwing:

![Verdacht account](./media/security-center-alerts-type/security-center-alerts-type-fig17-new.png)

### <a name="suspicious-firewall-rule-created"></a>Verdachte firewallregel gemaakt

Aanvallers kunnen proberen de hostbeveiliging te omzeilen door aangepaste firewallregels te maken die mogelijk maken dat schadelijke toepassingen communiceren met opdrachten en besturing, of via de aangetaste host aanvallen op het netwerk uitvoeren. In Security Center wordt een waarschuwing geactiveerd wanneer wordt gedetecteerd dat er een nieuwe firewallregel is gemaakt vanuit een uitvoerbaar bestand op een verdachte locatie.
 
Hier volgt een voorbeeld van dit type waarschuwing:

![Firewallregel](./media/security-center-alerts-type/security-center-alerts-type-fig18-new.png)

### <a name="suspicious-combination-of-hta-and-powershell"></a>Verdachte combinatie van HTA en PowerShell

In Security Center wordt een waarschuwing geactiveerd wanneer wordt gedetecteerd dat door een HTA (Microsoft HTML Application) PowerShell-opdrachten worden gestart. Dit is een techniek die aanvallers gebruiken om schadelijke PowerShell-scripts te starten.
 
Hier volgt een voorbeeld van dit type waarschuwing:

![HTA en PS](./media/security-center-alerts-type/security-center-alerts-type-fig19-new.png)


## <a name="network-analysis"></a>Netwerkanalyse
Het detecteren van netwerkbedreigingen van Security Center werkt volgens het automatisch verzamelen van beveiligingsgegevens van uw Azure IPFIX-verkeer (Internet Protocol Flow Information Export). Deze informatie wordt door Security Center geanalyseerd, waarbij vaak informatie uit meerdere bronnen wordt samengebracht om bedreigingen te analyseren.

### <a name="suspicious-outgoing-traffic-detected"></a>Verdacht uitgaand verkeer gedetecteerd
Netwerkapparaten kunnen op ongeveer dezelfde manier worden gedetecteerd en geprofileerd als andere typen systemen. Aanvallers beginnen gewoonlijk met het scannen of sweepen van poorten. In het volgende voorbeeld hebt u verdachte SSH-verkeer (Secure Shell) van een virtuele machine. In dit scenario zijn een SSH-beveiligingsaanval of een poortsweep op een externe resource mogelijk.

![Waarschuwing verdacht uitgaand verkeer](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Deze waarschuwing geeft informatie die u kunt gebruiken om de resource die is gebruikt voor het starten van deze aanval te identificeren. Deze waarschuwing bevat ook informatie om u te helpen de verdachte machine, de detectietijd en de gebruikte poort te identificeren. Deze pagina bevat ook een lijst van herstelstappen die kunnen worden gebruikt om dit probleem te verhelpen.

### <a name="network-communication-with-a-malicious-machine"></a>Netwerkcommunicatie met een kwaadwillende machine
Door gebruik te maken van de feeds van Microsoft met informatie over bedreigingen kan Azure Security Center verdachte computers die met kwaadwillende IP-adressen communiceren identificeren. In veel gevallen is het kwaadwillende adres een opdracht- en beheercentrum. In dit geval heeft Security Center gedetecteerd dat de communicatie is uitgevoerd met behulp van Pony Loader-malware (ook wel bekend als [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![Waarschuwing over netwerkcommunicatie](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Deze waarschuwing biedt informatie waarmee u de resource die is gebruikt om deze aanval te initiëren, de aangevallen resource, het IP-adres van het slachtoffer, het IP-adres van de aanvaller en de detectietijd kunt identificeren.

> [!NOTE]
> Live IP-adressen zijn verwijderd uit deze schermafbeelding wegens privacydoeleinden.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Mogelijke uitgaande Denial of Service-aanval gedetecteerd
Afwijkend netwerkverkeer afkomstig van een virtuele machine kan ertoe leiden dat Security Center een aanval van het type Denial of Service activeert.

Hier volgt een voorbeeld van dit type waarschuwing:

![Uitgaande DOS](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Resourceanalyse
Een resourceanalyse door Security Center richt zich op PaaS-services (Platform as a Service), zoals de integratie met de functie [Azure SQL Database-detectie van bedreigingen](../sql-database/sql-database-threat-detection.md). Op basis van de analyseresultaten van deze gebieden, activeert Security Center een aan een resource gerelateerde waarschuwing.

### <a name="potential-sql-injection"></a>Mogelijke SQL-injectie
SQL-injectie is een aanval waarbij schadelijke code wordt ingevoegd in tekenreeksen die later worden doorgegeven aan een exemplaar van SQL Server om te worden geparseerd en uitgevoerd. Elke procedure die SQL-instructies construeert, moet worden gecontroleerd op beveiligingsproblemen met injectie omdat SQL Server alle syntactisch geldige query's uitvoert die het ontvangt. SQL-bedreigingsdetectie maakt gebruik van machine learning, gedragsanalyse en detectie van afwijkingen om verdachte gebeurtenissen op te sporen die in uw Azure SQL-databases mogelijk plaatsvinden. Bijvoorbeeld:

* een voormalig medewerker heeft geprobeerd toegang tot een database te krijgen.
* Waarschuwing over SQL-injectieaanvallen
* Ongebruikelijke toegang tot de productiedatabase van een gebruiker thuis

![Waarschuwing over mogelijke SQL-injectie](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

De informatie in deze waarschuwing kan worden gebruikt om de aangevallen resource, de detectietijd en de status van de aanval te identificeren. De waarschuwing bevat ook een koppeling naar stappen voor verder onderzoek.

### <a name="vulnerability-to-sql-injection"></a>Beveiligingslek met betrekking tot SQL-injectie
Deze waarschuwing wordt geactiveerd wanneer een fout wordt gedetecteerd op een database. Deze waarschuwing kan duiden op een mogelijk beveiligingsprobleem met SQL-injectieaanvallen.

![Waarschuwing over mogelijke SQL-injectie](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Ongebruikelijke toegang vanaf onbekende locatie
Deze waarschuwing wordt geactiveerd wanneer er op een server een poging is gedetecteerd om toegang vanaf een onbekend IP-adres te krijgen, en wanneer een dergelijke poging in de afgelopen periode niet heeft plaatsgevonden.

![Waarschuwing over ongebruikelijke toegang](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="contextual-information"></a>Contextuele informatie
Tijdens een onderzoek hebben analisten extra context nodig om tot een eindoordeel te komen over de aard van de bedreiging en hoe deze kan worden tegengehouden.  Als er bijvoorbeeld een afwijkende situatie in het netwerk is gedetecteerd, maar er geen inzicht is in wat er verder gebeurt in het netwerk of met de betreffende resource, is het zeer lastig om te begrijpen welke vervolgacties nodig zijn. Om hierbij te helpen, kan een beveiligingsincident artefacten, gerelateerde gebeurtenissen en andere informatie bevatten die de onderzoeker meer context geven. De beschikbaarheid van extra informatie varieert afhankelijk van het type bedreiging dat is gedetecteerd en de configuratie van uw omgeving, en zal dus niet voor alle beveiligingsincidenten beschikbaar zijn.

Als er extra informatie beschikbaar is, wordt deze weergegeven in het beveiligingsincident onder de lijst met waarschuwingen. Dit kan informatie zijn zoals:

- Wissen van logboekgebeurtenissen
- PNP-apparaat aangesloten vanaf onbekend apparaat
- Waarschuwingen waarop geen actie kan worden uitgevoerd 

![Waarschuwing over ongebruikelijke toegang](./media/security-center-alerts-type/security-center-alerts-type-fig20.png) 


## <a name="see-also"></a>Zie ook
In dit artikel bent u meer te weten gekomen over de verschillende soorten beveiligingswaarschuwingen in Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsincidenten afhandelen in Azure Security Center](security-center-incident.md)
* [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md)
* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
