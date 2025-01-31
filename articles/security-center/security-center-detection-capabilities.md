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
ms.openlocfilehash: d4ca08a7b757889f79fb7ea5b85d7bc5c1c72259
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569355"
---
# <a name="azure-security-center-detection-capabilities"></a>Detectiemogelijkheden van Azure Security Center
Dit document bespreekt geavanceerde detectiemogelijkheden van Azure Security Center, die helpt bij het identificeren van actieve bedreigingen die gericht is op uw Microsoft Azure-resources - zowel Windows als Linux - en biedt u de inzichten die nodig zijn voor het snel reageren.

Geavanceerde detecties zijn beschikbaar in de Standard-laag van Azure Security Center. Er is een gratis proefversie beschikbaar. U kunt uw versie opwaarderen door Prijscategorie te selecteren in het gedeelte [Beveiligingsbeleid](tutorial-security-policy.md). Ga naar de [Security Center-pagina](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie over tarieven.


## <a name="responding-to-todays-threats"></a>Reageren op actuele bedreigingen
De bedreigingen zijn de afgelopen 20 jaar aanzienlijk veranderd. In het verleden moesten bedrijven zich meestal alleen zorgen maken over beschadiging van de website door afzonderlijke kwaadwillende gebruikers die voornamelijk wilden zien 'waartoe ze in staat waren'. Vandaag de dag gaan aanvallers veel genuanceerder te werk en zijn ze beter georganiseerd. Ze hebben vaak specifieke financiële en strategische doelstellingen. Ook hebben ze meer middelen beschikbaar, omdat ze mogelijk worden gefinancierd door staten of de georganiseerde misdaad.

Deze benadering heeft geleid tot een ongekende professionaliteit bij de aanvallers. Ze zijn niet langer geïnteresseerd in het beschadigen van websites. Ze zijn nu geïnteresseerd in het stelen van informatie, financiële rekeningen en persoonlijke gegevens - die ze allemaal kunnen gebruiken om geld te verdienen op de markt of gebruik te maken van een bepaalde zakelijke, politieke of militaire functie. Nog zorgwekkender dan de aanvallers met financiële oogmerken zijn de aanvallers die netwerken kraken om schade te berokkenen aan de infrastructuur en personen.

Organisaties implementeren als antwoord vaak verschillende afzonderlijke oplossingen die zich richten op het verdedigen van de bedrijfsomtrek of de eindpunten waarbij specifieke aanvalskenmerken in de gaten worden gehouden. Deze oplossingen genereren meestal een groot aantal onbetrouwbare waarschuwingen die een beveiligingsanalist vervolgens moet bekijken en onderzoeken. De meeste organisaties hebben niet de tijd en expertise die vereist zijn om te reageren op deze waarschuwingen - vele worden niet nader onderzocht.  Ondertussen hebben aanvallers hun methoden veranderd om op handtekening gebaseerde verdediging te ondermijnen en zich [aan te passen aan cloudomgevingen](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Nieuwe handelswijzen zijn vereist om nieuwe bedreigingen sneller te identificeren, te detecteren en erop te reageren.

## Hoe Azure Security Center detecteert en reageert op bedreigingen <a name="asc-detects"></a>
Beveiligingsonderzoekers van Microsoft zijn voortdurend op zoek naar bedreigingen. Ze hebben toegang tot een uitgebreide set telemetrie, afkomstig van de wereldwijde aanwezigheid van Microsoft in de cloud en on-premises. Deze verrijkende en uitgebreide verzameling gegevenssets stelt Microsoft in staat om nieuwe aanvalspatronen en -trends te ontdekken in on-premises producten voor consumenten en ondernemingen, evenals in haar online services. Als gevolg hiervan kan Security Center de detectie-algoritmen snel bijwerken wanneer aanvallers met nieuwe en steeds meer geavanceerde aanvallen komen. Met deze benadering kunt u de snel veranderende bedreigingen bijhouden.

Het detecteren van bedreigingen van Security Center werkt volgens het automatisch verzamelen van beveiligingsgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen. Deze informatie wordt door Security Center geanalyseerd, waarbij vaak informatie uit meerdere bronnen wordt samengebracht om bedreigingen te analyseren. Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging.

![Gegevensverzameling en -presentatie in Security Center](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Security Center maakt gebruik van geavanceerde beveiligingsanalyses die veel verder gaan dan op handtekeningen gebaseerde benaderingen. Doorbraken in big data- en [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/)-technologieën worden gebruikt om gebeurtenissen overal in de cloudinfrastructuur te evalueren - waarbij bedreigingen worden gedetecteerd die onmogelijk te identificeren waren geweest met behulp van handmatige benaderingen en het voorspellen van de ontwikkeling van aanvallen. Deze beveiligingsanalyses omvatten:

* **Geïntegreerde beveiligingsinformatie**: er wordt gezocht naar aanvallers door gebruik te maken van wereldwijde dreigingsinformatie uit Microsoft-producten en -services en van Microsoft Digital Crimes Unit (DCU), Microsoft Security Response Center (MSRC) en externe feeds.
* **Gedragsanalyses**: er worden bekende patronen toegepast om kwaadwillend gedrag te detecteren.
* **Afwijkingsdetectie**: er wordt gebruikgemaakt van statistische profilering om een historische basislijn op te stellen. Er wordt een waarschuwing gegeven bij afwijkingen van vastgestelde basislijnen die aan een mogelijke aanvalsvector voldoen.

### <a name="threat-intelligence"></a>Informatie over bedreigingen
Microsoft heeft een gigantische hoeveelheid informatie over wereldwijde bedreigingen. Telemetrie komt binnen uit meerdere bronnen, zoals Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, de Microsoft Digital Crimes Unit (DCU) en Microsoft Security Response Center (MSRC). Onderzoekers ontvangen ook informatie over bedreigingen die wordt gedeeld tussen de primaire cloudserviceproviders en abonnementen op feeds met informatie over bedreigingen van derden. Azure Security Center kan deze informatie gebruiken om u te waarschuwen over bedreigingen van bekende beveiligingsrisico’s. Voorbeelden zijn:

* **Uitgaande communicatie naar een kwaadwillend IP-adres**: uitgaand verkeer naar een bekend botnet of darknet geeft waarschijnlijk aan dat de resource is aangetast en een aanvaller probeert om opdrachten op dat systeem uit te voeren of gegevens te exfiltreren. Azure Security Center vergelijkt netwerkverkeer met de wereldwijde bedreigingsdatabase van Microsoft en waarschuwt u als communicatie met een schadelijk IP-adres wordt gedetecteerd.

## <a name="behavioral-analytics"></a>Gedragsanalyse
Gedragsanalyse is een techniek waarbij gegevens worden geanalyseerd en vergeleken met een verzameling bekende patronen. Deze patronen zijn echter geen eenvoudige handtekeningen. Ze worden vastgesteld aan de hand van complexe machine learning-algoritmen die worden toegepast op grote gegevenssets. Ze worden ook vastgesteld via de zorgvuldige analyse van schadelijk gedrag door deskundige analisten. Azure Security Center kan gebruikmaken van gedragsanalyses om verdachte resources te identificeren op basis van de analyse van logboeken van virtuele machines, apparaatlogboeken van virtuele netwerken, infrastructuurlogboeken, crashdumps en andere bronnen.

Er wordt ook gekeken naar andere signalen om te controleren op ondersteunend bewijs van een wijdverbreide aanval. Aan de hand van dit verband kan beter worden vastgesteld welke gebeurtenissen samenhangen met de opgestelde indicatoren van inbreuk. Voorbeelden zijn:

* **Uitvoering van verdachte processen**: Aanvallers maken gebruik van verschillende technieken voor het uitvoeren van schadelijke software zonder te worden gedetecteerd. Een aanvaller kan schadelijke software bijvoorbeeld dezelfde namen geven als legitieme systeembestanden, maar deze bestanden op een andere locatie plaatsen, een naam gebruiken die erg lijkt op een goedaardig bestand of de echte bestandsextensie maskeren. Security Center maakt gedragsmodellen en controleert de uitvoering van processen om uitschieters als deze te detecteren.  
* **Verborgen schadelijke software en pogingen tot misbruik**: Geavanceerde schadelijke software kan omzeilen traditionele antimalwareproducten door nooit naar schijf schrijven of softwareonderdelen die zijn opgeslagen op schijf.  Echter kan dergelijke schadelijke software worden gedetecteerd met behulp van een geheugenanalyse, omdat de malware traceringen in het geheugen moet achterlaten om te kunnen werken. Wanneer software vastloopt, bevat een crashdump een gedeelte van het geheugen ten tijde van de crash.  Door het geheugen in de crashdump te analyseren, kan Azure Security Center technieken detecteren die worden gebruikt om misbruik te maken van zwakke plekken in software, toegang te krijgen tot vertrouwelijke gegevens en ongemerkt aanwezig te blijven in een aangetaste machine zonder enige impact op de prestaties van uw computer.
* **Laterale verplaatsing en interne verkenning**: Als u wilt behouden in een aangetast netwerk en zoeken/verzamelen waardevolle gegevens, proberen aanvallers vaak zich lateraal van het aangetaste apparaat om naar te verplaatsen anderen binnen hetzelfde netwerk. Security Center controleert processen en aanmeldingsactiviteiten om pogingen van een aanvaller om zijn voetafdruk binnen het netwerk te vergroten te detecteren, zoals het op afstand uitvoeren van opdrachten om zwakke plekken in het netwerk te ontdekken en inventarisatie van accounts.
* **Schadelijke PowerShell-Scripts**: PowerShell wordt gebruikt door aanvallers om uit te voeren van schadelijke code op de virtuele doelmachines voor verschillende doeleinden. Security Center inspecteert PowerShell-activiteit op tekenen van verdachte activiteiten.
* **Uitgaande aanvallen**: Aanvallers richten vaak op cloudresources met het doel van het gebruik van deze resources voor het uitvoeren van extra aanvallen. Aangetaste virtuele machines kunnen bijvoorbeeld worden gebruikt om beveiligingsaanvallen te starten tegen andere virtuele machines, ongewenste e-mail te verzenden of te scannen op open poorten en andere apparaten op internet. Door machine learning toe te passen op netwerkverkeer kan Security Center het detecteren wanneer uitgaande netwerkcommunicatie groter is dan de norm. In het geval van ongewenste e-mail maakt Security Center ook een koppeling tussen ongebruikelijk e-mailverkeer en informatie uit Office 365 om te bepalen of het e-mailbericht waarschijnlijk kwaadwillend is of het resultaat is van een geldige e-campagne.  

### <a name="anomaly-detection"></a>Afwijkingsdetectie
Azure Security Center maakt ook gebruik van afwijkingsdetectie om bedreigingen te identificeren. In tegenstelling tot gedragsanalyses (die afhankelijk zijn van bekende patronen die zijn afgeleid van grote gegevenssets) is afwijkingsdetectie meer "gepersonaliseerd" en richt het zich op basislijnen die specifiek voor uw implementaties zijn. Machine learning wordt toegepast om de normale activiteit voor uw implementaties te bepalen en vervolgens worden regels gegenereerd om afwijkende omstandigheden te definiëren die een veiligheidsrisico zouden kunnen vormen. Hier volgt een voorbeeld:

* **Inkomende RDP/SSH-beveiligingsaanvallen**: Uw implementaties hebben mogelijk drukke virtuele machines met een groot aantal aanmeldingen elke dag en andere virtuele machines met maar weinig of geen aanmeldingen. Azure Security Center kan de normale aanmeldingsactiviteit voor deze virtuele machines vaststellen en gebruikmaken van machine learning om te bepalen wat buiten de normale aanmeldingsactiviteit valt. Als het aantal aanmeldingen of het tijdstip van de aanmeldingen of de locatie van waaruit de aanmeldingen zijn aangevraagd of andere aanmeldingsgerelateerde kenmerken aanzienlijk verschillen van wat normaal is, kan een waarschuwing worden gegenereerd. Ook hier weer wordt door machine learning bepaald wat een aanzienlijk verschil is.

## <a name="continuous-threat-intelligence-monitoring"></a>Doorlopende controle van informatie over bedreigingen
Voor Azure Security Center werken beveiligingsonderzoeks- en data-scienceteams die continu controleren op veranderingen in bedreigingen. Dit omvat de volgende initiatieven:

* **Controle van informatie over bedreigingen**: Informatie over bedreigingen omvat mechanismen, indicatoren, implicaties en advies over bestaande of nieuwe bedreigingen. Deze informatie wordt gedeeld in de beveiligingscommunity en Microsoft volgt continu feeds met informatie over bedreigingen uit interne en externe bronnen.
* **Signalen delen**: Inzichten op basis van beveiliging teams van Microsofts brede portfolio van cloud en on-premises services, servers en client-endpointapparaten worden gedeeld en geanalyseerd.
* **Microsoft-beveiligingsspecialisten**: Continue inzet van teams overal bij Microsoft die op gespecialiseerde beveiligingsgebieden, zoals forensisch onderzoek doen en web aanvalsdetectie werken.
* **Detectieafstemming**: Algoritmen worden uitgevoerd op echte gegevenssets en beveiligingsonderzoekers werken samen met klanten om de resultaten te valideren. Echte en fout-positieven worden gebruikt voor het verfijnen van machine learning-algoritmen.

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
