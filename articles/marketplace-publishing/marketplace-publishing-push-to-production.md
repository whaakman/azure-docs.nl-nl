---
title: Implementeren van uw aanbieding voor Azure Marketplace | Microsoft Docs
description: Informatie over en helpt u stapsgewijs door de instructies voor het implementeren van uw aanbieding--installatiekopie van virtuele machine, developer-service, data-service, enzovoort--naar Azure Marketplace.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 8f79b891-84e2-4f41-ba0d-66420e2c6b2e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2016
ms.author: hascipio
ms.openlocfilehash: 12dc81642905cd9449a1032c7ab57298e6b69ba8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Implementeren van uw aanbieding voor Azure Marketplace
Wanneer u tevreden bent met uw aanbieding (dat wil zeggen, u hebt getest scenario's van klanten, marketing inhoud, enz.) en u bent klaar om te starten en aanvragen **Push naar productie** op de **publiceren** tabblad.  

1. De vier stappen onder deze stapsgewijze KENNISMAKING pagina in de publicatie portal moet zijn voltooid en groen. Voor aanbiedingen van de virtuele Machine, moet u ervoor zorgen dat de volgende richtlijnen zijn gevolgd.
   
    ![tekenen][img-pubportal-walkthru-checked]
2. Selecteer de **publiceren** tabblad in de lijst aan de linkerkant.
   
    ![tekenen][img-pubportal-menu-publish]
3. Klik op de knop **goedkeuring aanvragen om te pushen naar productie**. Zodra de aanvraag wordt gedaan, het team goedkeuring voert een definitieve beoordeling en vervolgens uw aanbieding is beschikbaar in Azure Marketplace.
   
    ![tekenen][img-pubportal-publish-pushproduction]

> [!IMPORTANT]
> In geval van virtuele Machines dat wanneer u op de knop goedkeuren van aanvragen voor de push naar de productie, de volgende stappen zijn uitgevoerd achter de scene. U mag de voortgang van elke stap op het tabblad publiceren weergeven bij het publiceren portal. U moet controleren op deze pagina op een vast interval (totdat de status geeft 'Hier') voor alle foutinformatie die correctie van uw nodig.
> 
> * In eerste instantie gaat uw productie-aanvraag naar het certificeringsinstantie-team dat de vhd te valideren. Echter, als u uw al genoemde aanbieding bijwerken wilt en de aanvraag heeft kreeg alleen marketing wijzigen, klikt u vervolgens de certificering stap overgeslagen.
> * Op de volgende stap wordt de aanvraag komen aan het team van validatie van inhoud die de marketing inhoud van de aanbieding verifiëren.
> * Als de bovenstaande stappen voltooid zijn, is de aanbieding goedgekeurd in de productieomgeving. Op dit moment de status worden 'weergegeven' in de portal voor publiceren. Deze status 'Hier' betekent echter niet dat het proces is voltooid. De volgende stappen moeten zijn voltooid voordat de aanbieding beschikbaar in Azure Marketplace is.
> * Zodra de aanbieding is goedgekeurd in de productieomgeving in de bovenstaande stap, de replicatie van de aanbieding gestart in de Azure datacenters. In het algemeen neemt 24-48hours voor de replicatie is voltooid maar een week, afhankelijk van de grootte van de vhd kan duren. Als u uw al genoemde aanbieding bijwerken wilt en die bevat immers alleen marketing wijzigen, klikt u vervolgens is de replicatie echter sneller.
> * Wanneer de replicatie voltooid is, klikt u vervolgens zijn de aanbieding beschikbaar in Azure Marketplace.
> 
> U kunt altijd de aanbieding verwijderen wanneer deze is in een **concept** status (dat wil zeggen, nooit **Push voor fasering** of **Push naar productie**). Op de **geschiedenis** en klik op de **concept verwijderen** knop onder aan de pagina om het verwijderen van een concept.
> 
> 

## <a name="production-checklist-for-all-virtual-machine-offers"></a>Productie controlelijst voor alle aanbiedingen voor virtuele Machine
* Zorg ervoor dat u een Microsoft Azure Certified partner
* Klik op het tabblad SKU's moeten de optie 'verbergen de SKU van de Marketplace omdat moet altijd worden aangekocht via een oplossingssjabloon' worden gemarkeerd als Ja alleen als de SKU een onderdeel van een oplossingssjabloon is. In alle andere gevallen, moet deze optie altijd worden gemarkeerd als Nee.
* Let op: U moet niet de SKU zichtbaarheid instelling wijzigen zodra de SKU wordt vermeld. We ondersteund deze functionaliteit niet.
* Zorg ervoor dat de logo's aan de onderstaande richtlijnen voor de Azure Marketplace-logo voldoen.
* Aanbieding en SKU beschrijving mag niet hetzelfde zijn.
* SKU van titel en bieden lange samenvatting mag niet hetzelfde zijn.
* SKU titel en bieden samenvatting mag niet hetzelfde zijn.
* SKU titels mag niet zijn identiek voor een aanbieding met meerdere SKU's.

**Richtlijnen voor Azure Marketplace-logo**

* Het ontwerp van de Azure heeft een eenvoudige kleurenpalet. Houd het aantal primaire en secundaire kleuren op uw logo laag.
* Themakleur van de Azure portal zijn wit en zwarte. Daarom Vermijd het gebruik van deze kleuren als de achtergrondkleur van uw logo's. Sommige kleur waaruit zou uw logo's opvallende meldingen in de Azure portal gebruiken. Het wordt aangeraden de eenvoudige primaire kleuren. Als u met behulp van transparante achtergrond, zorg ervoor dat de logo/tekst is geen witte of zwarte.
* Een achtergrond met kleurovergang niet gebruiken op het logo.
* Vermijd het logo van tekst, zelfs uw bedrijf of de naam van het merk, brengen.
* Het uiterlijk van uw logo moet 'platte' en moet niet verlopen.
* Het logo mag niet worden uitgerekt.

**Aanvullende richtlijnen voor het logo Hero:**

* Het logo Hero is optioneel. De publisher kunt niet een logo Hero uploaden. **Echter eenmaal geüploade het pictogram hero kan niet worden verwijderd uit de publicatie van de portal. Op dat moment wordt moet de partner de richtlijnen voor Azure Marketplace voor Hero pictogrammen anders die de aanbieding niet goedgekeurd voor productie volgen.**
* De weergavenaam van de uitgever, SKU titel en de lange samenvatting aanbieding worden weergegeven in wit tekstkleur. Daarom moet u niet een lichte kleur op de achtergrond van het pictogram Hero houden. Zwart, wit en transparante achtergrond is niet toegestaan voor Hero pictogrammen.
* De uitgever van de weergegeven naam, SKU titel, de aanbieding lange samenvatting en de knop maken zodra de aanbieding gaat vermelde programmatisch binnen het logo Hero zijn ingesloten. U moet dus niet alle tekst invoeren bij het ontwerpen van het logo Hero. Laat u lege ruimte aan de rechterkant omdat de tekst (dat wil zeggen Publisher weergavenaam, SKU titel, de aanbieding lange samenvatting) wordt opgenomen via programmacode door ons hier. De lege ruimte voor de tekst moet 415 x 100 aan de rechterkant (en deze worden gecompenseerd via 370px vanaf de linkerkant).

## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Biedt extra productie controlelijst voor het al genoemde virtuele Machine
* Controleer of er al een aanbieding met dezelfde aanbiedingsnaam van uw bedrijf. Zo ja, moet u een nieuwe versie van de SKU toevoegen in de bestaande aanbieding in plaats van een nieuwe aanbieding dubbele maken.
* Gegevensschijf moet niet wijzigen tussen twee versies van dezelfde SKU.
* Azure Marketplace biedt geen ondersteuning voor prijscategorie wijzigen van de vermelde SKU's zoals dit van invloed is op de facturering van de bestaande klanten. Zorg ervoor dat u niet wijzigt de prijzen van de vermelde SKU's in de regio's waar de SKU beschikbaar is. U kunt echter toevoegen van nieuwe SKU's of nieuwe regio's toevoegen aan een bestaande SKU.

## <a name="next-steps"></a>Volgende stappen
Zodra de aanbieding live gaat, test u de klant scenario's voor het valideren van alle contracten en functionaliteit goed in de productieomgeving als werkt getest en gevalideerd in de testomgeving.

## <a name="see-also"></a>Zie ook
* [Aan de slag: hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
