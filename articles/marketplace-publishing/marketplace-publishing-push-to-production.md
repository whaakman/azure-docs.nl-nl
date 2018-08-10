---
title: Implementeren van uw aanbieding in Azure Marketplace | Microsoft Docs
description: Meer informatie over en stapsgewijs door de instructies voor het implementeren van uw aanbieding--VM-installatiekopie, developer-service, gegevensservice, enz.--in de Azure Marketplace.
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 8f79b891-84e2-4f41-ba0d-66420e2c6b2e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2016
ms.author: hascipio
ms.openlocfilehash: 12dc81642905cd9449a1032c7ab57298e6b69ba8
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714278"
---
# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Implementeren van uw aanbieding in Azure Marketplace
Wanneer u tevreden bent met uw aanbieding (dat wil zeggen, u hebt getest klantscenario's, marketing-inhoud, enzovoort) en u bent klaar om te starten, aanvragen **sturen naar productie** op de **publiceren** tabblad.  

1. De vier stappen onder deze stapsgewijze KENNISMAKING pagina in de publicatie portal moet zijn voltooid en groen. Zorg ervoor dat de volgende richtlijnen worden gevolgd voor virtuele Machine-aanbiedingen.
   
    ![tekenen][img-pubportal-walkthru-checked]
2. Selecteer de **publiceren** tabblad in de lijst aan de linkerkant.
   
    ![tekenen][img-pubportal-menu-publish]
3. Klik op de knop **goedkeuring aanvragen om door te sturen naar productie**. Zodra de aanvraag wordt gedaan, het team goedkeuring voert een uiteindelijke beoordeling en vervolgens uw aanbieding is beschikbaar in de Azure Marketplace.
   
    ![tekenen][img-pubportal-publish-pushproduction]

> [!IMPORTANT]
> In het geval van virtuele Machines, wanneer u op de knop goedkeuren om door te sturen naar productie, klikt u op de volgende stappen uitgevoerd achter de scène. U wordt mogelijk zijn om de voortgang van elke stap onder het tabblad ' PUBLISH ' in de publicatie portal. U moet controleren op deze pagina op een vast interval (totdat de status 'Hier') voor alle informatie over de fout die correctie van uw nodig.
> 
> * In eerste instantie gaat uw productie-aanvraag naar de certificering-team dat de vhd te valideren. Echter, als u uw aanbieding al vermelde bijwerkt en de aanvraag maar liefst alleen marketing wijzigen, klikt u vervolgens de certificeringsstap is overgeslagen.
> * Op de volgende stap komen de aanvraag naar het team van validatie van inhoud die controleren of de marketing inhoud van de aanbieding.
> * Als de bovenstaande stappen voltooid zijn, is klikt u vervolgens de aanbieding goedgekeurd in de productieomgeving. Op dit moment de status worden 'weergegeven' in de portal voor publiceren. Deze status 'Hier' betekent echter niet dat het proces voltooid is. De volgende stappen moeten zijn voltooid voordat de aanbieding beschikbaar in de Azure Marketplace is.
> * Wanneer de aanbieding is goedgekeurd in de productieomgeving in de bovenstaande stap, replicatie van de aanbieding wordt gestart in alle de datacenters van Azure. In het algemeen neemt 24-48hours voor de replicatie is voltooid maar duurt ongeveer een week, afhankelijk van de grootte van de vhd. Als u uw aanbieding al vermelde bijwerkt en je kunt alleen marketing wijzigen, klikt u vervolgens is de replicatie echter sneller.
> * Wanneer de replicatie voltooid is, klikt u vervolgens is de aanbieding beschikbaar in de Azure Marketplace.
> 
> U kunt altijd de aanbieding verwijderen terwijl het in een **Draft** status (dat wil zeggen, nooit **overgaat tot fasering** of **sturen naar productie**). Op de **geschiedenis** en klik op de **concept verwijderen** onder aan de pagina om het verwijderen van een concept.
> 
> 

## <a name="production-checklist-for-all-virtual-machine-offers"></a>Controlelijst voor productie voor alle virtuele Machine-aanbiedingen
* Zorg ervoor dat u een Microsoft Azure Certified-partner
* Klik op het tabblad SKU's moeten de optie 'verbergen deze SKU van de Marketplace omdat moet altijd worden gekocht via een oplossingssjabloon"worden gemarkeerd als Ja alleen als de SKU een onderdeel van een oplossingssjabloon is. In alle andere gevallen, moet deze optie altijd worden gemarkeerd als Nee.
* Houd er rekening mee: U moet niet de SKU zichtbaarheid instelling wijzigen wanneer de SKU wordt vermeld. We bieden geen ondersteuning voor deze functionaliteit.
* Zorg ervoor dat de logo's aan de richtlijnen voor het logo van Azure Marketplace hieronder voldoen.
* Beschrijving van aanbieding en SKU mag niet hetzelfde zijn.
* SKU's titel en bieden lange samenvatting mag niet hetzelfde zijn.
* SKU-titel en samenvatting bieden mag niet hetzelfde zijn.
* Er mag geen SKU titels identiek zijn voor een aanbieding met meerdere SKU's.

**Richtlijnen voor Azure Marketplace-logo**

* Het Azure-ontwerp heeft een eenvoudig kleurenpalet. Houd het aantal primaire en secundaire kleuren op uw logo laag.
* De themakleuren van het Azure-portal zijn wit en zwart. Vermijd daarom het gebruik van deze kleuren als de achtergrondkleur van uw logo's. Sommige kleur die uw logo's opvallende in Azure portal gebruiken. We adviseren eenvoudige primaire kleuren. Als u transparante achtergrond gebruikt, zorg ervoor dat het logo/tekst is geen witte of zwarte wordt weergegeven.
* Gebruik niet een kleurovergang achtergrond op het logo.
* Voorkomen dat tekst, zelfs uw bedrijf of de naam van het merk, geplaatst op het logo.
* Het uiterlijk van uw logo moet 'vast' en vermijd kleurovergangen.
* Het logo mag niet worden uitgebreid.

**Aanvullende richtlijnen voor de Hero-logo:**

* De Hero-logo is optioneel. De publisher kunt niet een Hero-logo uploaden. **Maar eenmaal geüploade de hero-pictogram kan niet worden verwijderd uit de publicatie portal. Op dat moment moet de partner de Azure Marketplace-richtlijnen voor de Hero pictogrammen anders die de aanbieding niet worden goedgekeurd volgen naar productie.**
* De weergavenaam van de uitgever, SKU titel en de lange samenvatting aanbieding worden weergegeven in de kleur wit. Daarom moet u niet een lichte kleur houden op de achtergrond van het Hero-pictogram. Zwart-wit en transparante achtergrond is niet toegestaan voor Hero pictogrammen.
* De uitgever van de weergegeven naam, SKU titel, de aanbieding voor lange samenvatting en de knop maken wanneer de aanbieding wordt weergegeven via een programma in de Hero-logo zijn ingesloten. U moet dus niet als er geen tekst invoeren bij het ontwerpen van de Hero-logo. Laat alleen lege ruimte aan de rechterkant omdat de tekst (dat wil zeggen weergavenaam van de uitgever, SKU titel, de aanbieding voor lange samenvatting) worden opgenomen via een programma door ons hier. De lege ruimte voor de tekst moet 415 x 100 aan de rechterkant (en deze wordt gecompenseerd door 370px vanaf de linkerkant).

## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Biedt aanvullende productie controlelijst voor het al genoemde virtuele Machine
* Controleer of er al een aanbieding met dezelfde naam van uw bedrijf bieden. Als u Ja kiest, moet u een nieuwe versie van de SKU van het bestaande aanbod in plaats van het maken van een nieuwe aanbieding voor dubbele toevoegen.
* Gegevensschijf moet tussen twee versies van dezelfde SKU niet wijzigen.
* De Azure Marketplace biedt geen ondersteuning voor prijswijziging van de vermelde SKU's omdat dit invloed heeft op de facturering van de bestaande klanten. Zorg ervoor dat u niet wijzigt de prijzen van de vermelde SKU's in de regio's waar de SKU beschikbaar is. U kunt echter nieuwe SKU's toevoegen of nieuwe regio's toevoegen aan een bestaande SKU.

## <a name="next-steps"></a>Volgende stappen
Wanneer de aanbieding live meteen, test u de klantscenario's voor het valideren van alle de opdrachten en functies alleen goed in de productieomgeving als werken getest en gevalideerd in de faseringsomgeving.

## <a name="see-also"></a>Zie ook
* [Aan de slag: een aanbieding publiceren op Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
