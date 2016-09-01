<properties 
    pageTitle="Wat zijn Logic Apps?" 
    description="Meer informatie over Logic Apps in App Service" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="07/12/2016"
    ms.author="klam"/>

#Wat zijn Logic Apps?

Logic Apps biedt een manier om schalbare integraties en workflows in de cloud te vereenvoudigen en te implementerne. Het biedt een visuele designer om uw proces te modelleren en te automatiseren als een reeks stappen bekend als een werkstroom.  Er zijn [vele connectoren](../connectors/apis-list.md) in de cloud en on-premises om snel te integreren over services en protocols.  een logic app begint met een trigger (zoals “Wanneer is een account toegevoegd aan Dynamics CRM”) en na het starten kunnen vele combinaties acties, conversies en voorwaardleijke logica beginnen.

De voordelen van het gebruik van Logic Apps omvatten het volgende:  

- Tijd besparen door complexe processen met eenvoudig te begrijpen ontwerpfuncties ontwerpen
- Patronen en werkstromen naadloos implementeren, dat zou met code anders moeilijk zijn
- Snel aan de slag met voorbeelden
- Uw logic app aanpassen met uw eigen API’s, code en acties
- Verbind en synchroniseer ongelijksoortige systemen on-premises verspreid en in de cloud
- Opbouwen van BizTalk-server, API Management, Azure Functions en Azure Service Bus met eersteklas integratieondersteuning

Logica Apps is een volledig beheerd iPaaS (integratie Platform as a Service) waarmee gebruiker zich geen zorgen moeten maken over het voorzien van hosting, schaalbaarheid, beschikbaarheid en beheer.  Logic Apps zal de schaal automatisch aanpassen in functie van de vraag.

![App-ontwerper voor stromen](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Zoals eerder vermeld kunt u met Logic Apps bedrijfsprocessen automatiseren. Hieronder vindt u enkele voorbeelden:  
 
* Bestanden geüpload naar een FTP-server verplaatsen naar Azure Storage
* Proces- en routeorders verspreid over de cloud en on-premises systemen
* Bewaken van alle tweets over een bepaald onderwerp, het gevoel analyseren en meldingen en taken maken voor items die moeten worden opgevolgd.

Dergelijke scenario's kunnen worden geconfigureerd via de visuele ontwerper, zonder dat hiervoor ook maar één regel code hoeft te worden geschreven. Ga nu aan de slag met het [bouwen van een logische app][maken].  Na het schrijven - kan een logische app[snel worden geïmplementeerd en opnieuw geconfigureerd](app-service-logic-create-deploy-template.md) over verschillende omgevingen en regio’s.

## Waarom Logic Apps?

Logic App brengt snelheid en schaalbaarheid naar de integratieruimte van de onderneming.  Het gebruiksgemak van de ontwerper, de diversiteit van beschikbare triggers en acties en krachtige beheerstools maken het centraliseren van uw API’s eenvoudiger dan ooit.  Aangezien bedrijven steeds meer streven naar digitalisering, kunt u met Logic Apps oude en ultramoderne systemen met elkaar verbinden.

Bovendien kunt u met onze [Enterprise Integration Account][biztalk] schalen naar volgroeide integratiescenario’s met de kracht van een [XML messaging][xml], [handelspartnerbeheer][tpm] en meer.

- **Eenvoudig te gebruiken ontwerptools** - Logic Apps kunnen end-to-end in de browser of met andere Visual Studio-tools worden ontworpen. Begin met een trigger - van een eenvoudig schema tot wanneer een GitHub-probleem ontstaat. Vervolgens kunt u een willekeurig aantal acties indelen met behulp van de uitgebreide galerie met connectors.

- **API’s gemakkelijk verbinden** - Zelfs compositietaken die gemakkelijk te beschrijven zijn, zijn moeilijk te integreren in code. Logic Apps maakt het gemakkelijk om ongelijksoortige systemen met elkaar te verbinden. Wilt u uw marketingoplossing in de cloud verbinden met uw on-premises factureringssysteem? Wilt u messaging centraliseren over API’s en systemen met een Enterprise Service Bus? De snelste en betrouwbaarste manier om deze problemen op te lossen, is door gebruik te maken van Logic Apps.

- **Snel aan de slag met behulp van sjablonen** - Om u op weg te helpen, is er een [galerie met sjablonen][sjablonen] waarmee u snel een enkele algemene oplossingen kunt maken. Van geavanceerde B2B-oplossingen zo eenvoudig als SaaS-connectiviteit en zelfs enkele gewoon “voor de lol” - de galerie is de snelste manier om an de slag te gaan met de kracht van Logic Apps.

- **Ingebakken uitbreidbaarheid** - Ziet u niet de connector die u nodig hebt? Logic Apps is ontworpen om te werken met uw eigen API’s en code; u kunt gemakkelijk uw eigen API-app maken om te gebruiken als aangepaste connector of beroep doen op een [Azure Function](https://functions.azure.com) om exclusieve snippets code on-demand uit te voeren. 

- **Echte paardenkracht bij integratie** - Start eenvoudig en groei wanneer dat nodig is. Met Logic Apps kunt u de kracht van BizTalk, de toonaangevende integratieoplossing van Microsoft, gebruiken om integratieprofessionals de oplossingen te laten bouwen die ze nodig hebben. Meer informatie over het [Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md).

## Concept van Logic Apps

In het onderstaande overzicht ziet u enkele belangrijke onderdelen van Logic Apps. 

- **Werkstroom** - Met Logic Apps kunt u uw bedrijfsprocessen grafisch weergeven als een reeks stappen of als een werkstroom.
- **Beheerde connectoren** - Uw logic apps vragen toegang tot data en service. Beheerde connectoren zijn specifiek gemaakt om u te helpen bij het maken van verbinding en werken met uw data. Raadpleeg de lijst van connectoren nu beschikbaar in [beheerde connectoren][beheerdeapi‘s].
- **Triggers** - Sommige Beheerde Connectoren kunnen ook werken als trigger. Met een trigger wordt een nieuw exemplaar van een werkstroom gestart op basis van een bepaalde gebeurtenis, zoals de aankomst van een e-mail of een wijziging in uw Azure Storage-account.
-  **Acties** - Elke stap na de trigger in een werkstroom wordt een actie genoemd. Elke actie verwijst normaal naar een bewerking op uw beheerde connector of aangepaste API-apps.
- **Enterprise Integration Pack** - Voor meer geavanceerde integratiescenario’s omvat Logic Apps functies van BizTalk. BizTalk is Microsoft’s toonaangevende integratieplatform. De Enterprise Integration Pack-connectoren zorgen ervoor dat u gemakkelijk validering, transpformatie en meer kunt opnemen in uw Logic App-workflows.

## Aan de slag  

- Als u aan de slag wilt met Logic Apps, volgt u de zelfstudie [Create a Logic App][maken] (Een logische app maken).  
- [Algemene voorbeelden en scenario's weergeven](app-service-logic-examples-and-scenarios.md)
- [Met Logic Apps kunt u bedrijfsprocessen automatiseren.](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Meer informatie over hoe u Logic Apps kunt integreren in uw systemen](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[maken]: app-service-logic-create-a-logic-app.md
[beheerdeapi‘s]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[sjablonen]: app-service-logic-use-logic-app-templates.md



<!--HONumber=ago16_HO4-->


