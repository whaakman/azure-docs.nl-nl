---
title: Detectiemogelijkheden in Azure Security Center | Microsoft Docs
description: In dit document wordt uitgelegd hoe de detectiemogelijkheden van Azure Security Center werken.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 4c5599cc-99a1-430f-895f-601615ef12a0
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/2/2018
ms.author: rkarlin
ms.openlocfilehash: ac3d1a55a707349873132befb25a38358b563138
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875558"
---
# <a name="azure-security-center-detection-capabilities"></a>Detectiemogelijkheden van Azure Security Center
Dit document bevat informatie over de geavanceerde detectie mogelijkheden van Azure Security Center, waarmee u actieve bedreigingen kunt identificeren die gericht zijn op uw Microsoft Azure resources, zowel Windows als Linux, en waarmee u de inzichten hebt die nodig zijn om snel te reageren.

Geavanceerde detecties zijn beschikbaar in de Standard-laag van Azure Security Center. Er is een gratis proefversie beschikbaar. U kunt uw versie opwaarderen door Prijscategorie te selecteren in het gedeelte [Beveiligingsbeleid](tutorial-security-policy.md). Ga naar de [Security Center-pagina](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie over tarieven.


## <a name="responding-to-todays-threats"></a>Reageren op actuele bedreigingen
De bedreigingen zijn de afgelopen 20 jaar aanzienlijk veranderd. In het verleden moesten bedrijven zich meestal alleen zorgen maken over beschadiging van de website door afzonderlijke kwaadwillende gebruikers die voornamelijk wilden zien 'waartoe ze in staat waren'. Vandaag de dag gaan aanvallers veel genuanceerder te werk en zijn ze beter georganiseerd. Ze hebben vaak specifieke financiële en strategische doelstellingen. Ook hebben ze meer middelen beschikbaar, omdat ze mogelijk worden gefinancierd door staten of de georganiseerde misdaad.

Deze benadering heeft geleid tot een ongekende professionaliteit bij de aanvallers. Ze zijn niet langer geïnteresseerd in het beschadigen van websites. Ze zijn nu geïnteresseerd in het stelen van informatie, financiële rekeningen en persoonlijke gegevens - die ze allemaal kunnen gebruiken om geld te verdienen op de markt of gebruik te maken van een bepaalde zakelijke, politieke of militaire functie. Nog zorgwekkender dan de aanvallers met financiële oogmerken zijn de aanvallers die netwerken kraken om schade te berokkenen aan de infrastructuur en personen.

Organisaties implementeren als antwoord vaak verschillende afzonderlijke oplossingen die zich richten op het verdedigen van de bedrijfsomtrek of de eindpunten waarbij specifieke aanvalskenmerken in de gaten worden gehouden. Deze oplossingen genereren meestal een groot aantal onbetrouwbare waarschuwingen die een beveiligingsanalist vervolgens moet bekijken en onderzoeken. De meeste organisaties hebben niet de tijd en expertise die vereist zijn om te reageren op deze waarschuwingen - vele worden niet nader onderzocht.  Ondertussen hebben aanvallers hun methoden veranderd om op handtekening gebaseerde verdediging te ondermijnen en zich [aan te passen aan cloudomgevingen](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Nieuwe handelswijzen zijn vereist om nieuwe bedreigingen sneller te identificeren, te detecteren en erop te reageren.

## Hoe Azure Security Center detecteert en reageert op bedreigingen<a name="asc-detects"></a>
Beveiligingsonderzoekers van Microsoft zijn voortdurend op zoek naar bedreigingen. Ze hebben toegang tot een uitgebreide set telemetrie, afkomstig van de wereldwijde aanwezigheid van Microsoft in de cloud en on-premises. Deze verrijkende en uitgebreide verzameling gegevenssets stelt Microsoft in staat om nieuwe aanvalspatronen en -trends te ontdekken in on-premises producten voor consumenten en ondernemingen, evenals in haar online services. Als gevolg hiervan kan Security Center de detectie-algoritmen snel bijwerken wanneer aanvallers met nieuwe en steeds meer geavanceerde aanvallen komen. Met deze benadering kunt u de snel veranderende bedreigingen bijhouden.

Het detecteren van bedreigingen van Security Center werkt volgens het automatisch verzamelen van beveiligingsgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen. Deze informatie wordt door Security Center geanalyseerd, waarbij vaak informatie uit meerdere bronnen wordt samengebracht om bedreigingen te analyseren. Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging.

![Gegevensverzameling en -presentatie in Security Center](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Security Center maakt gebruik van geavanceerde beveiligingsanalyses die veel verder gaan dan op handtekeningen gebaseerde benaderingen. Doorbraken in big data- en [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/)-technologieën worden gebruikt om gebeurtenissen overal in de cloudinfrastructuur te evalueren - waarbij bedreigingen worden gedetecteerd die onmogelijk te identificeren waren geweest met behulp van handmatige benaderingen en het voorspellen van de ontwikkeling van aanvallen. Deze beveiligingsanalyses omvatten:

* **Geïntegreerde beveiligingsinformatie**: er wordt gezocht naar aanvallers door gebruik te maken van wereldwijde dreigingsinformatie uit Microsoft-producten en -services en van Microsoft Digital Crimes Unit (DCU), Microsoft Security Response Center (MSRC) en externe feeds.
* **Gedragsanalyses**: er worden bekende patronen toegepast om kwaadwillend gedrag te detecteren.
* **Afwijkingsdetectie**: er wordt gebruikgemaakt van statistische profilering om een historische basislijn op te stellen. Er wordt een waarschuwing gegeven bij afwijkingen van vastgestelde basislijnen die aan een mogelijke aanvalsvector voldoen.

### <a name="threat-intelligence"></a>Informatie over bedreigingen
Microsoft heeft een gigantische hoeveelheid informatie over wereldwijde bedreigingen. Telemetrie komt binnen uit meerdere bronnen, zoals Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, de Microsoft Digital Crimes Unit (DCU) en Microsoft Security Response Center (MSRC). Onderzoekers ontvangen ook informatie over bedreigingen die wordt gedeeld tussen de primaire cloudserviceproviders en abonnementen op feeds met informatie over bedreigingen van derden. Azure Security Center kan deze informatie gebruiken om u te waarschuwen over bedreigingen van bekende beveiligingsrisico’s. Voorbeelden zijn:

* **Uitgaande communicatie met een schadelijk IP-adres**: uitgaand verkeer naar een bekend botnet of darknet duidt waarschijnlijk op dat uw bron is aangetast en dat een aanvaller probeert opdrachten uit te voeren op die systeem-of exfiltreren-gegevens. Azure Security Center vergelijkt netwerkverkeer met de wereldwijde bedreigingsdatabase van Microsoft en waarschuwt u als communicatie met een schadelijk IP-adres wordt gedetecteerd.

## <a name="behavioral-analytics"></a>Gedragsanalyse
Gedragsanalyse is een techniek waarbij gegevens worden geanalyseerd en vergeleken met een verzameling bekende patronen. Deze patronen zijn echter geen eenvoudige handtekeningen. Ze worden vastgesteld aan de hand van complexe machine learning-algoritmen die worden toegepast op grote gegevenssets. Ze worden ook vastgesteld via de zorgvuldige analyse van schadelijk gedrag door deskundige analisten. Azure Security Center kan gebruikmaken van gedragsanalyses om verdachte resources te identificeren op basis van de analyse van logboeken van virtuele machines, apparaatlogboeken van virtuele netwerken, infrastructuurlogboeken, crashdumps en andere bronnen.

Er wordt ook gekeken naar andere signalen om te controleren op ondersteunend bewijs van een wijdverbreide aanval. Aan de hand van dit verband kan beter worden vastgesteld welke gebeurtenissen samenhangen met de opgestelde indicatoren van inbreuk. Voorbeelden zijn:

* **Verdachte proces uitvoering**: Kwaadwillende personen gebruiken verschillende technieken om schadelijke software uit te voeren zonder detectie. Een aanvaller kan schadelijke software bijvoorbeeld dezelfde namen geven als legitieme systeembestanden, maar deze bestanden op een andere locatie plaatsen, een naam gebruiken die erg lijkt op een goedaardig bestand of de echte bestandsextensie maskeren. Security Center maakt gedragsmodellen en controleert de uitvoering van processen om uitschieters als deze te detecteren.  
* **Verborgen malware en pogingen tot misbruik**: Geavanceerde malware kan traditionele antimalware-producten omzeilen door nooit naar schijf te schrijven of software onderdelen te versleutelen die zijn opgeslagen op schijf.  Echter kan dergelijke schadelijke software worden gedetecteerd met behulp van een geheugenanalyse, omdat de malware traceringen in het geheugen moet achterlaten om te kunnen werken. Wanneer software vastloopt, bevat een crashdump een gedeelte van het geheugen ten tijde van de crash.  Door het geheugen in de crashdump te analyseren, kan Azure Security Center technieken detecteren die worden gebruikt om misbruik te maken van zwakke plekken in software, toegang te krijgen tot vertrouwelijke gegevens en ongemerkt aanwezig te blijven in een aangetaste machine zonder enige impact op de prestaties van uw computer.
* **Zijdelingse verplaatsing en interne Reconnaissance**: Om te blijven bestaan in een aangetast netwerk en waardevolle gegevens te vinden/verzamelen, proberen aanvallers vaak later van de geïnfecteerde machine naar anderen binnen hetzelfde netwerk te gaan. Security Center controleert processen en aanmeldingsactiviteiten om pogingen van een aanvaller om zijn voetafdruk binnen het netwerk te vergroten te detecteren, zoals het op afstand uitvoeren van opdrachten om zwakke plekken in het netwerk te ontdekken en inventarisatie van accounts.
* **Schadelijke Power shell-scripts**: Power shell wordt door aanvallers gebruikt om schadelijke code uit te voeren op virtuele doel machines voor verschillende doel einden. Security Center inspecteert PowerShell-activiteit op tekenen van verdachte activiteiten.
* **Uitgaande aanvallen**: Aanvallers richten zich vaak op cloud resources met het doel van het gebruik van deze resources om extra aanvallen te koppelen. Aangetaste virtuele machines kunnen bijvoorbeeld worden gebruikt om beveiligingsaanvallen te starten tegen andere virtuele machines, ongewenste e-mail te verzenden of te scannen op open poorten en andere apparaten op internet. Door machine learning toe te passen op netwerkverkeer kan Security Center het detecteren wanneer uitgaande netwerkcommunicatie groter is dan de norm. In het geval van ongewenste e-mail maakt Security Center ook een koppeling tussen ongebruikelijk e-mailverkeer en informatie uit Office 365 om te bepalen of het e-mailbericht waarschijnlijk kwaadwillend is of het resultaat is van een geldige e-campagne.  

### <a name="anomaly-detection"></a>Afwijkingsdetectie
Azure Security Center maakt ook gebruik van afwijkingsdetectie om bedreigingen te identificeren. In tegenstelling tot gedragsanalyses (die afhankelijk zijn van bekende patronen die zijn afgeleid van grote gegevenssets) is afwijkingsdetectie meer "gepersonaliseerd" en richt het zich op basislijnen die specifiek voor uw implementaties zijn. Machine learning wordt toegepast om de normale activiteit voor uw implementaties te bepalen en vervolgens worden regels gegenereerd om afwijkende omstandigheden te definiëren die een veiligheidsrisico zouden kunnen vormen. Hier volgt een voorbeeld:

* **Inkomende RDP/SSH-** aanvals aanvallen: Uw implementaties hebben mogelijk drukke virtuele machines met een groot aantal aanmeldingen per dag en andere virtuele machines met weinig of alle aanmeldingen. Azure Security Center kan de normale aanmeldingsactiviteit voor deze virtuele machines vaststellen en gebruikmaken van machine learning om te bepalen wat buiten de normale aanmeldingsactiviteit valt. Als het aantal aanmeldingen of het tijdstip van de aanmeldingen of de locatie van waaruit de aanmeldingen zijn aangevraagd of andere aanmeldingsgerelateerde kenmerken aanzienlijk verschillen van wat normaal is, kan een waarschuwing worden gegenereerd. Ook hier weer wordt door machine learning bepaald wat een aanzienlijk verschil is.

## <a name="continuous-threat-intelligence-monitoring"></a>Doorlopende controle van informatie over bedreigingen
Voor Azure Security Center werken beveiligingsonderzoeks- en data-scienceteams die continu controleren op veranderingen in bedreigingen. Dit omvat de volgende initiatieven:

* **Threat Intelligence-bewaking**: Bedreigings informatie omvat mechanismen, indica toren, implicaties en bruikbare adviezen over bestaande of opkomende bedreigingen. Deze informatie wordt gedeeld in de beveiligingscommunity en Microsoft volgt continu feeds met informatie over bedreigingen uit interne en externe bronnen.
* **Signaal deling**: Inzichten van beveiligings teams in het hele assortiment van de Cloud en on-premises Services, servers en client eindpunt apparaten van micro soft worden gedeeld en geanalyseerd.
* **Micro soft-beveiligings specialisten**: Lopende betrokkenheid bij teams in micro soft die werken in gespecialiseerde beveiligings velden, zoals forensische en detectie van webaanval.
* **Afstemming van detectie**: Algoritmen worden uitgevoerd op basis van echte klant gegevens sets en beveiligings onderzoekers werken samen met klanten om de resultaten te valideren. Echte en fout-positieven worden gebruikt voor het verfijnen van machine learning-algoritmen.

Deze gecombineerde inspanningen moeten resulteren in nieuwe en verbeterde detecties waarvan u onmiddellijk kunt profiteren: u hoeft helemaal geen actie te ondernemen.

## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe de detectiemogelijkheden in Azure Security Center werken. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Beveiligingswaarschuwingen per type in Azure Security Center](security-center-alerts-type.md)
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
