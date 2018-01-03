---
title: -Microsoft Threat Modeling hulpprogramma - Azure aan de slag | Microsoft Docs
description: Dit is een beter overzicht markering van het hulpprogramma Threat Modeling in te grijpen.
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 73d6b42e7a97d6041f6213a1f7d060806734d763
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="getting-started-with-the-threat-modeling-tool"></a>Aan de slag met het hulpprogramma Threat Modeling

De Cloud en Enterprise beveiligingsprogramma's team uitgebracht de Threat Modeling hulpprogramma Preview eerder dit jaar als een gratis  **[Klik hier om te downloaden](https://aka.ms/tmtpreview)**. De wijziging in bezorgingsmechanisme kan we de meest recente verbeteringen en oplossingen voor problemen om naar te pushen klanten telkens wanneer die ze het hulpprogramma Open, waardoor het gemakkelijker te onderhouden en gebruiken.
In dit artikel gaat u door het proces van het aan de slag met de Microsoft SDL threat modeling benadering en ziet u hoe u het hulpprogramma voor het ontwikkelen van modellen geweldige threat als een backbone van uw beveiliging.

In dit artikel is gebaseerd op bestaande kennis over de SDL threat modeling benadering. Raadpleeg voor een kort overzicht  **[Threat Modeling webtoepassingen](https://msdn.microsoft.com/library/ms978516.aspx)**  en een gearchiveerde versie van  **[onthullen beveiligingsfouten met behulp van de aanpak STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)**  MSDN-artikel in 2006 gepubliceerd.

Samenvattend snel omvat de aanpak het maken van een diagram, bedreigingen te identificeren, beperkende ze en elke risicobeperking valideren. Hier volgt een diagram dit proces illustreert:

![SDL-proces](./media/azure-security-threat-modeling-tool-feature-overview/sdlapproach.png)

## <a name="starting-the-threat-modeling-process"></a>De threat modeling proces wordt gestart

Wanneer u het hulpprogramma Threat Modeling start, ziet u een aantal dingen, zoals in de afbeelding:

![Lege startpagina](./media/azure-security-threat-modeling-tool-feature-overview/tmtstart.png)

### <a name="threat-model-section"></a>Sectie Threat-model

| Onderdeel                                   | Details                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Feedback, suggesties en problemen knop** | U gaat de  **[MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)**  voor alle zaken SDL. Dit biedt u een kans om Lees wat andere gebruikers, samen met tijdelijke oplossingen en aanbevelingen doen. Als u nog steeds niet kunt vinden wat u zoekt, e- tmtextsupport@microsoft.com voor ons ondersteuningsteam om u te helpen                                                                                                                            |
| **Een Model maken**                          | Hiermee opent u een leeg canvas voor u het diagram te tekenen. Zorg ervoor dat u selecteren welke sjabloon die u wilt gebruiken voor uw model                                                                                                                                                                                                                                                                                                                                                                       |
| **Sjabloon voor nieuwe modellen**                 | U moet selecteren welke sjabloon te gebruiken voordat het maken van een model. Onze belangrijkste de sjabloon is Azure Threat Model, waarin de Azure-specifieke stencils, bedreigingen en oplossingen. Voor algemene modellen, selecteert u de SDL TM Knowledge Base uit de vervolgkeuzelijst. Wilt u uw eigen sjabloon maken of een nieuwe voor alle gebruikers verzenden? Bekijk onze  **[sjabloon opslagplaats](https://github.com/Microsoft/threat-modeling-templates)**  GitHub-Page voor meer informatie                              |
| **Een Model openen**                            | <p>Wordt geopend eerder opgeslagen threat modellen. De functie modellen onlangs geopend is ideaal als u nodig hebt om uw meest recente bestanden te openen. Wanneer u de muisaanwijzer op de selectie, vindt u 2 manieren modellen openen:</p><p><ul><li>Open vanaf deze Computer – klassieke manier van het openen van een bestand met behulp van lokale opslag</li><li>Openen van OneDrive-teams kunt mappen in OneDrive opslaan en delen van hun threat modellen in op één locatie naar de productiviteit te verhogen en samenwerking</li></ul></p> |
| **Getting Started Guide**                   | Hiermee opent u de  **[Microsoft Threat Modeling Tool](./azure-security-threat-modeling-tool.md)**  hoofdpagina                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Sjabloonsectie

| Onderdeel               | Details                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Nieuwe sjabloon maken** | Hiermee opent u een lege sjabloon voor u op maken. Tenzij u uitgebreide kennis bij het maken van sjablonen helemaal hebt, raden we u aan op basis van bestaande samenstellen |
| **Sjabloon openen**       | Hiermee opent u de bestaande sjablonen voor u wijzigingen aan te brengen                                                                                                              |

Het team Threat Modeling hulpprogramma wordt steeds verder te verbeteren hulpprogramma functionaliteit en ervaren. Enkele kleine wijzigingen in de loop van het jaar kunnen plaatsvinden, maar alle belangrijke wijzigingen vereisen regeneraties in de handleiding. Raadpleeg het vaak om ervoor te zorgen dat u de meest recente aankondigingen.

## <a name="building-a-model"></a>Een model bouwen

In deze sectie we Ga als volgt:

- Cristina (developer)
- Ricardo (een programmamanager) en
- Ashish (tester)

Ze gaan door het proces van hun eerste risicomodel ontwikkelt.

> Ricardo: Hallo Cristina, in het model threat diagram is gegaan en wilden om ervoor te zorgen wij de details van de juiste. Kan u helpen mij via zoeken?
> Cristina: volkomen. Laten we.
> Ricardo Hiermee opent u het hulpprogramma en zijn scherm deelt met Cristina.

![Basic risicomodel](./media/azure-security-threat-modeling-tool-feature-overview/basictmt.png)

> Cristina: OK klikt, ziet er eenvoudig, maar kunt u doorlopen mij deze?
> Ricardo: zeker! Hier volgt de uitsplitsing:
> - Onze menselijke gebruiker wordt getekend als een externe entiteit: een vierkant
> - Deze opdrachten op uw webserver verzendt: de cirkel
> - De webserver is een database (twee parallelle lijnen) advies

Wat Ricardo net hebt u geleerd Cristina is een GSD, afkorting voor  **[gegevensstroom-Diagram](https://en.wikipedia.org/wiki/Data_flow_diagram)**. Het hulpprogramma Threat Modeling kunnen gebruikers de grenzen van vertrouwensrelaties, aangegeven door de rode stippellijn om weer te geven waarin verschillende entiteiten zich bevindt in het besturingselement opgeven. IT-beheerders vereisen bijvoorbeeld een Active Directory-systeem voor verificatiedoeleinden zodat Active Directory buiten hun besturingselement is.

> Cristina: Gezicht aan mij. Hoe zit het omgaan met bedreigingen?
> Ricardo: Ik laat zien.

## <a name="analyzing-threats"></a>Bedreigingen analyseren

Nadat hij op de weergave van de analyse van de pictogram menuselectie (bestand met Vergrootglas) klikt, hij wordt uitgevoerd om een lijst van bedreigingen het Threat Modeling hulpprogramma gevonden die zijn gegenereerd op basis van de standaardsjabloon dat gebruikmaakt van de SDL-methode aangeroepen  **[ STRIDE (vervalsing, knoeien, vrijgeven van informatie, DOS-aanval en bevoegdheden)](https://en.wikipedia.org/wiki/STRIDE_(security))**. Het idee is dat software onder een voorspelbare set van bedreigingen die u kunt vinden met behulp van deze categorieën 6 afkomstig is.

Deze benadering is dat uw huis beveiligen door ervoor te zorgen elke deur- en beschikt over een vergrendelingsfout mechanisme voor het toevoegen van een alarmsysteem of chasing na de dief.

![Basic bedreigingen](./media/azure-security-threat-modeling-tool-feature-overview/basicthreats.png)

Ricardo begint met het selecteren van het eerste item in de lijst. Dit is wat er gebeurt:

Eerst is de interactie tussen de twee stencils verbeterd

![Interactie](./media/azure-security-threat-modeling-tool-feature-overview/interaction.png)

Tweede, aanvullende informatie over de bedreiging wordt weergegeven in het venster Eigenschappen Threat

![Interactie Info](./media/azure-security-threat-modeling-tool-feature-overview/interactioninfo.png)

De gegenereerde bedreiging helpt hem potentiële fouten met ontwerp begrijpen. De categorisatie STRIDE geeft hij een idee op mogelijke aanvalsvectoren, terwijl de extra beschrijving toegangsinformatie precies wat is het probleem en mogelijke manieren om te beperken van het. Hij kan bewerkbare velden notities bij de rechtvaardiging-gegevens schrijven of wijzigen van de prioriteit classificaties, afhankelijk van zijn organisatie bug balk gebruiken.

Azure-sjablonen hebben extra details om te helpen gebruikers begrijpen niet alleen wat is het probleem, maar ook hoe u dit herstelt door beschrijvingen, voorbeelden en hyperlinks Azure-specifieke documentatie toe te voegen.

De beschrijving die hij het belang van het toevoegen van een verificatiemethode om te voorkomen dat gebruikers worden vervalst, houd er rekening mee weer te geven in de eerste gevaar worden gewerkt. Een paar minuten in de bespreking van de met Cristina, begrepen zij het belang van de implementatie van toegangsbeheer en rollen. Ricardo gevuld in sommige snelle notities om ervoor te zorgen dat deze zijn geïmplementeerd.

Als Ricardo is een in de bedreigingen onder openbaarmaking van informatie fout, gerealiseerde hij dat de dit plan sommige alleen-lezen accounts vereist voor controle en rapportage. Hij zich wel eens afgevraagd of dit een nieuwe bedreiging moet zijn, maar de oplossingen hetzelfde, zijn zodat hij de bedreiging dienovereenkomstig hebt genoteerd.
Hij ook iets meer over het vrijgeven van informatie wordt beschouwd en realiseren dat de back-uptapes moesten versleuteling, een taak voor het operationele team nodig.

Niet van toepassing op het ontwerp vanwege bestaande oplossingen of -beveiligingsgroep bedreigingen wordt gegarandeerd dat kan worden gewijzigd in "Kan niet van toepassing" in de vervolgkeuzelijst Status. Er zijn drie andere opties: niet gestart: standaardselectie moet onderzoek – gebruikt voor het opvolgen van items en Mitigated – nadat het volledig is gegaan op.

## <a name="reports--sharing"></a>Rapporten en delen

Zodra Ricardo de lijst met Cristina doorloopt en belangrijke opmerkingen, oplossingen/redenen, prioriteit en statuswijzigingen toevoegt, selecteert hij rapporten -> volledige rapport opslaan rapport afdrukken van een rapport nice voor hem te doorlopen met collega's -> maken om te controleren of de juiste beveiliging werk is geïmplementeerd.

![Interactie Info](./media/azure-security-threat-modeling-tool-feature-overview/report.png)

Als Ricardo wil dit bestand te delen in plaats daarvan, kunt hij eenvoudig doen door op te slaan in de organisatie OneDrive-account. Wanneer hij dat doet, kan hij het documentkoppeling kopiëren en delen met zijn collega's. 

## <a name="threat-modeling-meetings"></a>Threat modellering vergaderingen

Wanneer zijn risicomodel Ricardo naar zijn collega met OneDrive, Ashish, de tester verzonden, is underwhelmed. Leek zoals Ricardo en Cristina ontbreekt een aantal belangrijke complexere cases, die kunnen worden gegarandeerd. Zijn telefoontjes vormt een aanvulling op threat modellen.

In dit scenario nadat Ashish via de risicomodel duurde hij aangeroepen voor twee threat modellering vergaderingen: één vergadering synchroniseren op het proces en de diagrammen doorlopen en vervolgens een tweede vergadering voor bedreiging bekijken en afmelden.

In de eerste vergadering gespendeerd Ashish 10 minuten roulatie van iedereen via de SDL threat modeling proces. Hij vervolgens opgehaald van het model threat diagram en uit te leggen in detail gestart. Binnen vijf minuten waren een belangrijk onderdeel van de ontbrekende geïdentificeerd.

Een paar minuten later, Ashish en Ricardo is in een uitgebreide bespreking van hoe de webserver is opgebouwd. Het is momenteel niet de optimale methode voor een vergadering om door te gaan, maar iedereen uiteindelijk overeengekomen dat het verschil vroeg detecteren zou ze om tijd te besparen in de toekomst.

Enkele manieren deze besproken en op de risicomodel afgemeld in de tweede vergadering het team doorlopen van de bedreigingen. Ze het document in broncodebeheer ingecheckt en voortgezet met ontwikkeling.

## <a name="thinking-about-assets"></a>Nadenken over activa

Sommige lezers die beschikken over de bedreiging gemodelleerd merkt dat we nog niet is besproken activa helemaal. We hebben gedetecteerd dat veel softwareontwikkelaars hun software beter dan ze het concept van activa begrijpen en welke activa een aanvaller mogelijk geïnteresseerd in begrijpen.

Als u risicomodel een huis gaat, kunt u beginnen met nadenken over uw familie, onvervangbare foto's of waardevolle illustratie. Mogelijk kunt u beginnen met nadenken over wie in mogelijk verbroken en het huidige beveiligingssysteem. Of kunt u beginnen met de fysieke kenmerken, zoals de groep of de front-porch overwegen. Dit zijn vergelijkbaar met nadenken over activa, aanvallers of softwareontwerp. Een van deze drie methoden werken.

De aanpak voor het modelleren we hebt die hier wordt gepresenteerd dreiging is aanzienlijk eenvoudiger is dan wat Microsoft in het verleden heeft gedaan. We vinden het ontwerpplan software geschikt is voor veel teams. We hopen dat die jouw e-mailadres bevatten.

## <a name="next-steps"></a>Volgende stappen

Verzenden van uw vragen, opmerkingen en problemen op tmtextsupport@microsoft.com. **[Download](https://aka.ms/tmtpreview)**  Threat Modeling hulpprogramma aan de slag.