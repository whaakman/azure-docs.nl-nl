---
title: Wat is een werkruimte en het project? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Een werkruimte is een werkgebied voor samenstellen en het bouwen van uw aangepaste vertaalsysteem. Een werkruimte kan bevatten meerdere projecten, modellen en documenten. Een project is een wrapper voor een model, documenten en tests. Elk project omvat automatisch alle documenten die in deze werkruimte zijn geüpload met de combinatie van de juiste taal.
services: cognitive-services
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: 6f88ff8ecd1aee588cb82c08ae2eda58fe2eb1e7
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627368"
---
# <a name="what-is-a-custom-translator-workspace"></a>Wat is een aangepaste Translator-werkruimte?

Een werkruimte is een werkgebied voor samenstellen en het bouwen van uw aangepaste vertaalsysteem. Een werkruimte kan bevatten meerdere projecten, modellen en documenten. Er is al het werk u in aangepaste Translator doet binnen een specifieke werkruimte.

Werkruimte is exclusief voor u en de mensen die u in uw werkruimte uitnodigen. Ingetrokken gebruikers geen toegang tot de inhoud van uw werkruimte. U kunt zo veel mensen als u in uw werkruimte wilt en wijzigen of verwijderen van de toegang op elk gewenst moment uitnodigen. U kunt ook een nieuwe werkruimte maken. Een werkruimte wordt niet standaard bevatten alle projecten of documenten die binnen uw andere werkruimten.

## <a name="what-is-a-custom-translator-project"></a>Wat is een aangepaste Translator-project?

Een project is een wrapper voor een model, documenten en tests. Elk project omvat automatisch alle documenten die in deze werkruimte zijn geüpload met de combinatie van de juiste taal. Als u een Engels naar het Spaans-project en een Spaans-Engels-project hebt, worden er bijvoorbeeld dezelfde documenten worden opgenomen in beide projecten. Elk project heeft een categorie-id die is gekoppeld aan deze die wordt gebruikt bij het opvragen van de [V3 API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl) voor vertalingen. Categorie-id is de parameter die wordt gebruikt om op te halen van vertalingen van een aangepast systeem die zijn gebouwd met aangepaste Translator.

## <a name="project-categories"></a>Projectcategorieën

De categorie geeft het domein: het gebied van de terminologie en stijl die u wilt gebruiken: voor uw project. Kies het meest relevant voor uw documenten categorie. In sommige gevallen van uw keuze van de categorie rechtstreeks invloed op het gedrag van de aangepaste Translator.

Er zijn geen aangepaste modellen voor categorieën, maar met uitzondering van een algemene basislijn-systeem. Maar nog steeds wordt aangeraden gebruikers het meest van toepassing zijn op hun domein categorie selecteren, zodat deze kan worden gebruikt als een id in de categorie-id. Voor projecten in het domein van de technologie selecteren "Technologie" zorgt ervoor dat wanneer een basislijn-model beschikbaar is, dat uw project zich om deze te gebruiken.

U kunt in dezelfde werkruimte en projecten voor de dezelfde combinatie van taal in verschillende categorieën maken. Aangepaste Translator wordt voorkomen dat het maken van een dubbele project met de dezelfde combinatie van taal en categorie. Een label toe te passen aan uw project, kunt u om te voorkomen dat deze beperking. Gebruik de labels niet, tenzij u automatische vertalingssystemen voor meerdere clients toe te voegen dat aan uw project een unieke label, worden weergegeven in uw projecten CategoryID bouwt.

## <a name="project-labels"></a>Project-labels

Aangepaste Translator kunt u een project-label toewijzen aan uw project. Het label van het project wordt onderscheid gemaakt tussen meerdere projecten met dezelfde combinatie van taal en categorie. Als een best practice, Vermijd het gebruik van project labels, tenzij die nodig zijn.

Het label van het project wordt gebruikt als onderdeel van de categorie-id. Als het label project niet-ingestelde blijft of identiek in projecten die is ingesteld, klikt u vervolgens voor projecten met dezelfde categorie en *verschillende* taal paren delen de dezelfde categorie-id. Deze aanpak is nuttig omdat u Hiermee u of uw klant om over te schakelen tussen talen bij het gebruik van de tekst-API van Translator zonder dat u een categorie-id die uniek is voor elk project.

Als ik wil inschakelen vertalingen in de technologie domein van het Engels Frans en naar Frans naar het Engels, zou ik bijvoorbeeld twee projecten maken: één voor Engels -\> Frans en één voor Frans -\> Engels. Ik zou de dezelfde categorie (technologie) opgeven voor zowel en het label project leeg laten. De categorie-id voor beide projecten wordt er gezocht, zodat ik de API voor Engelse en Franse vertalingen zoeken kan zonder mijn CategoryID wijzigen.

Als u een taal-serviceprovider en meerdere klanten van dienst met verschillende modellen die de dezelfde categorie en de combinatie van taal behouden wilt, zou klikt u vervolgens met behulp van een label project onderscheid maken tussen de klanten een verstandig besluit zijn.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Training en het model](training-and-model.md) te weten over het bouwen van een objectmodel vertaling efficiënt.