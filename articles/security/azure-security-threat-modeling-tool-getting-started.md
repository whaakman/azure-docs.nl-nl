---
title: Aan de slag - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: Dit is een beter overzicht Threat Modeling Tool in actie te markeren.
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
ms.openlocfilehash: 340c92d2830069a9d957f4ece79416a707062629
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096771"
---
# <a name="getting-started-with-the-threat-modeling-tool"></a>Aan de slag met de Threat Modeling Tool

Het team van Cloud en Enterprise beveiligingsprogramma's de Threat Modeling Tool Preview vrijgegeven eerder dit jaar als een gratis  **[Klik hier om te downloaden](https://aka.ms/tmtpreview)**. De wijziging in een leveringsmechanisme kan we de meest recente verbeteringen en oplossingen voor problemen om naar te pushen klanten telkens wanneer die ze opent u het hulpprogramma, waardoor het gemakkelijker te onderhouden en te gebruiken.
In dit artikel doorloopt u het proces van het aan de slag met de SDL Microsoft threat modeling benadering en ziet u hoe u het hulpprogramma te gebruiken voor het ontwikkelen van geweldige bedreigingsmodellen als een backbone van de beveiliging wordt uitgevoerd.

In dit artikel is gebaseerd op bestaande kennis van de SDL threat modeling benadering. Raadpleeg voor een kort overzicht **[Threat Modeling webtoepassingen](https://msdn.microsoft.com/library/ms978516.aspx)** en een gearchiveerde versie van **[bloot Security fouten met behulp van de STRIDE-methode](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)** MSDN-artikel gepubliceerd in 2006.

Als u wilt snel samen te vatten, omvat de aanpak van het maken van een diagram, identificeren van bedreigingen, ze beperkende en valideren van elke beperking. Hier volgt een diagram waarin dit proces:

![SDL-proces](./media/azure-security-threat-modeling-tool-feature-overview/sdlapproach.png)

## <a name="starting-the-threat-modeling-process"></a>Starten van de threat modeling proces

Wanneer u de Threat Modeling Tool start, ziet u een aantal dingen, zoals in de afbeelding wordt weergegeven:

![Lege startpagina](./media/azure-security-threat-modeling-tool-feature-overview/tmtstart.png)

### <a name="threat-model-section"></a>Sectie Threat-model

| Onderdeel                                   | Details                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Feedback, suggesties en problemen met knop** | U gaat de **[MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)** voor alles wat SDL. Dit biedt u een mogelijkheid om te lezen wat andere gebruikers, samen met tijdelijke oplossingen en aanbevelingen doen. Als u nog steeds niet kunt vinden wat u zoekt, e- tmtextsupport@microsoft.com voor ons ondersteuningsteam om u te helpen                                                                                                                            |
| **Een Model maken**                          | Hiermee opent u een leeg canvas voor u het diagram te tekenen. Zorg ervoor dat u selecteert welke sjabloon u wilt gebruiken voor uw model                                                                                                                                                                                                                                                                                                                                                                       |
| **Sjabloon voor nieuwe modellen**                 | U moet selecteren welke sjabloon u wilt gebruiken voor het maken van een model. Onze belangrijkste de sjabloon is Azure Threat Model, waarin Azure-specifieke stencils, bedreigingen en oplossingen. Voor algemene modellen, selecteert u de SDL TM Knowledge Base uit de vervolgkeuzelijst. Wilt u uw eigen sjabloon maken of een nieuw wachtwoord voor alle gebruikers verzenden? Bekijk onze **[Sjabloonopslagplaats](https://github.com/Microsoft/threat-modeling-templates)** GitHub Page voor meer informatie                              |
| **Een Model openen**                            | <p>Bedreigingsmodellen wordt eerder opgeslagen door wordt geopend. De functie modellen onlangs geopend is handig als u nodig hebt om uw meest recente bestanden te openen. Als u de muisaanwijzer op de selectie, ziet u 2 manieren om te openen van modellen:</p><p><ul><li>Open vanaf deze Computer: klassieke manier van het openen van een bestand met behulp van lokale opslag</li><li>Open vanuit OneDrive-teams mappen in OneDrive kunnen gebruiken voor het opslaan en delen van hun bedreigingsmodellen op één locatie om u te helpen hun productiviteit en samenwerking</li></ul></p> |
| **Handleiding aan de slag**                   | Hiermee opent u de **[Microsoft Threat Modeling Tool](./azure-security-threat-modeling-tool.md)** hoofdpagina                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Sjabloon

| Onderdeel               | Details                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Nieuwe sjabloon maken** | Hiermee opent u een lege sjabloon voor u het bouwen van op. Tenzij u uitgebreide kennis bij het bouwen van sjablonen helemaal hebt, raden we u bouwt op basis van bestaande toepassingen |
| **Sjabloon openen**       | Hiermee opent u bestaande sjablonen voor u wijzigingen aan te brengen                                                                                                              |

Het team Threat Modeling Tool werkt voortdurend te verbeteren van de functionaliteit van het hulpprogramma en ervaring. Enkele kleine wijzigingen kunnen plaatsvinden in de loop van het jaar, maar alle belangrijke wijzigingen moeten regeneraties in de handleiding. Raadpleeg het vaak om ervoor te zorgen dat u de meest recente aankondigingen.

## <a name="building-a-model"></a>Een model bouwen

In deze sectie volgen we de:

- Cristina (ontwikkelaars)
- Ricardo (programmamanager) en
- Ashish (een tester)

Ze gaan door het proces van het ontwikkelen van hun eerste risicomodel.

> Ricardo: Hallo Cristina, kan ik het model threat diagram gewerkt en willen wij de details van de juiste. Kunt u mij via zoeken?
> Cristina: absoluut. Laten we eens.
> Ricardo Hiermee opent u het hulpprogramma en zijn scherm met Cristina deelt.

![Basic risicomodel](./media/azure-security-threat-modeling-tool-feature-overview/basictmt.png)

> Cristina: OK klikt, ziet er eenvoudig, maar kunt u stapsgewijs mij het?
> Ricardo: ervoor! Hier volgt de uitsplitsing van de:
> - Onze menselijke gebruiker wordt getekend als een externe entiteit, een vierkant
> - Deze opdrachten op uw webserver verzendt, de cirkel
> - De webserver is overleg met een database (twee parallelle lijnen)

Wat Ricardo NET heeft laten zien Cristina is een GSD, kort voor  **[gegevensstroom-Diagram](https://en.wikipedia.org/wiki/Data_flow_diagram)**. De Threat Modeling Tool kunnen gebruikers de grenzen van vertrouwensrelaties, aangegeven door de rode stippellijn, om weer te geven waarin verschillende entiteiten worden in het besturingselement opgeven. IT-beheerders vereisen bijvoorbeeld een Active Directory-systeem voor verificatiedoeleinden wordt gebruikt, zodat de Active Directory buiten hun beheer vallen is.

> Cristina: Klopt aan mij. Hoe zit het met de bedreigingen?
> Ricardo: Ik wil u laten.

## <a name="analyzing-threats"></a>Analyseren van bedreigingen

Nadat hij op de weergave van de analyse van het pictogram menuselectie (bestand met Vergrootglas) klikt, hij wordt ondernomen om een lijst van bedreigingen voor de gegenereerde de Threat Modeling Tool gevonden op basis van de standaardsjabloon die gebruikmaakt van de SDL-benadering met de naam  **[ STRIDE (vervalsing, knoeien, vrijgeven van informatie, ontkenning, Denial of Service en misbruik van bevoegdheden)](https://en.wikipedia.org/wiki/STRIDE_(security))**. Het idee is dat software wordt geleverd bij een voorspelbare set bedreigingen, die kan worden gevonden met behulp van deze categorieën 6.

Deze benadering is, net als een mechanisme voor vergrendeling in plaats voor het toevoegen van een alarmsysteem of chasing na de dief huis beveiligen door ervoor te zorgen dat elke klep en het venster heeft.

![Basic bedreigingen](./media/azure-security-threat-modeling-tool-feature-overview/basicthreats.png)

Ricardo begint met het eerste item in de lijst selecteren. Dit is wat er gebeurt:

Eerst is de interactie tussen de twee stencils verbeterd

![Interactie](./media/azure-security-threat-modeling-tool-feature-overview/interaction.png)

Tweede, als u meer informatie over de bedreiging wordt weergegeven in het venster Eigenschappen van de bedreiging

![Interactie Info](./media/azure-security-threat-modeling-tool-feature-overview/interactioninfo.png)

De gegenereerde bedreiging helpt hem potentiële zwakke plekken van ontwerp begrijpen. De categorisatie STRIDE geeft hij een idee op mogelijke aanvalsvectoren, terwijl de aanvullende beschrijving toegangsinformatie precies wat is het probleem, samen met mogelijke manieren om te beperken het. Hij kan bewerkbare velden gebruiken notities in de details van de reden of prioriteit classificaties, afhankelijk van zijn organisatie bug balk wijzigen.

Azure-sjablonen hebt u meer informatie over de manier gebruikers niet alleen wat er mis is, maar ook voor het oplossen van het door beschrijvingen, voorbeelden en hyperlinks toevoegen aan Azure-specifieke documentatie te begrijpen.

De beschrijving die hij het belang van het toevoegen van een verificatiemethode om te voorkomen dat gebruikers vervalsen, realiseren onthullen van de eerste bedreiging worden gewerkt. Een paar minuten in de discussie met Cristina, begrijpen ze het belang van de implementatie van access control en rollen. Ricardo ingevuld sommige Losse notities om ervoor te zorgen dat deze zijn geïmplementeerd.

Als Ricardo is een in de bedreigingen onder openbaarmaking van informatie fout, gerealiseerde hij dat de dit plan sommige alleen-lezen-accounts vereist voor controle en rapportage. Hij zich afgevraagd of dit een nieuwe bedreiging moet, maar de oplossingen hetzelfde, zijn zodat hij de bedreiging dienovereenkomstig hebt genoteerd.
Hij ook iets meer over het vrijgeven van informatie wordt beschouwd en dat de back-uptapes zijn gaan coderen, een taak voor het operationele team moet gerealiseerd.

Niet van toepassing op het ontwerp vanwege bestaande oplossingen of security bedreigingen garandeert kan worden gewijzigd in "Niet van toepassing" in de vervolgkeuzelijst Status. Er zijn drie andere opties: niet gestart: standaardselectie, onderzoek moet – gebruikt moeten worden opgevolgd items en Mitigated – nadat deze volledig wordt gewerkt.

## <a name="reports--sharing"></a>Rapporten en delen

Zodra de Ricardo doorloopt de lijst met Cristina en belangrijke opmerkingen, oplossingen/redenen, prioriteit en statuswijzigingen toegevoegd, selecteert hij rapporten -> volledig rapport -> rapport opslaan, die een goed rapport voor hem te doorlopen met collega's het tekstvenster maken om te zorgen dat de juiste beveiliging-werk is geïmplementeerd.

![Interactie Info](./media/azure-security-threat-modeling-tool-feature-overview/report.png)

Als Ricardo wil deelt u het bestand in plaats daarvan, kan hij dus heel gemakkelijk doen door op te slaan in de OneDrive-account van de organisatie. Nadat hij doet, kan hij de documentkoppeling kopiëren en delen met zijn collega's. 

## <a name="threat-modeling-meetings"></a>Threat modellering vergaderingen

Wanneer Ricardo zijn risicomodel te zijn met OneDrive, Ashish, het testprogramma collega verzonden, is underwhelmed. Leek zoals Ricardo en Cristina gemist van een groot aantal belangrijke hoek gevallen, die eenvoudig kunnen worden aangetast. Zijn telefoontjes vormt een aanvulling op bedreigingsmodellen.

In dit scenario nadat Ashish via de risicomodel heeft hij aangeroepen voor twee threat modellering vergaderingen: één vergadering om te synchroniseren op het proces en stapsgewijs door de diagrammen en vervolgens een tweede vergadering voor bedreigingen bekijken en goedkeuring.

In de eerste vergadering besteed Ashish 10 minuten walking iedereen via de SDL threat modeling proces. Hij vervolgens opgehaald van het model threat diagram en uit te leggen in detail is gestart. Binnen vijf minuten waren een ontbrekend onderdeel dat belangrijk geïdentificeerd.

Een paar minuten hoger Ashish en Ricardo is in een uitgebreide beschrijving van de manier waarop de server is gemaakt. Het is niet de optimale methode voor een vergadering om door te gaan, maar iedereen uiteindelijk overeengekomen dat vroeg detecteren van het verschil zou ze om tijd te besparen in de toekomst.

Enkele manieren om ermee om besproken en de risicomodel goedgekeurd in de tweede vergadering, het team stapsgewijs de bedreigingen. Ze gecontroleerd van het document in broncodebeheer en voortgezet met ontwikkeling.

## <a name="thinking-about-assets"></a>Na te denken over assets

Sommige lezers die gemodelleerd bedreiging zijn is mogelijk dat we nog niet hebt gehad over activa helemaal. We hebben ontdekt dat veel softwareontwikkelaars hun software beter begrijpt dan ze het concept van assets begrijpen en welke elementen een aanvaller mogelijk in geïnteresseerd bent.

Als u naar risicomodel een huis gaat, kan u worden gestart door na te denken over uw familie, onvervangbare foto's of waardevolle illustratie. Misschien kunt u beginnen met nadenken over wie kan defect raken en het huidige beveiligingssysteem. Of kunt u beginnen met de fysieke kenmerken, zoals de groep of de voorste porch overweegt. Dit zijn vergelijkbaar met nadenken over activa, aanvallers of softwareontwerp. Een van deze drie methoden werken.

De benadering van modellering die we hier hebben weergegeven van de bedreiging is aanzienlijk eenvoudiger is dan wat Microsoft heeft gedaan in het verleden. Wij vinden dat de software-ontwerpbenadering geschikt is voor veel teams. We hopen dat die uw bevatten.

## <a name="next-steps"></a>Volgende stappen

Stuur uw vragen, opmerkingen en problemen van tmtextsupport@microsoft.com. **[Download](https://aka.ms/tmtpreview)**  Threat Modeling Tool aan de slag.
