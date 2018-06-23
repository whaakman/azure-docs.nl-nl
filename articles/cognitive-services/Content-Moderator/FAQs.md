---
title: Veelgestelde vragen over Azure Content beheerder | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over inhoud beheerder.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 11/21/2016
ms.author: sajagtap
ms.openlocfilehash: 7bf6c67bd0a83d9455d14253f4f4b2becafd29ba
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344478"
---
# <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

#### <a name="what-does-my-content-moderator-subscription-include"></a>Wat wordt mijn inhoud beheerder abonnement bevatten?
Uw abonnement inhoud beheerder omvat toegang tot het hulpprogramma voor beoordeling en de API's. U kunt beslissen of u wilt gebruiken een of andere of beide, afhankelijk van uw bedrijfsbehoeften.

#### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>Wat zijn de limieten/beperkingen van de inhoud waarop toezicht kan worden gehouden door de API te gebruiken?
Wanneer u de API gebruikt, moeten installatiekopieën hebben van ten minste 128 pixels en een maximale bestandsgrootte van 4 MB. Tekst mag hoogstens 1024 tekens. Er is geen limiet voor de duur van de video.

#### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>Wat gebeurt er als de inhoud die naar de tekst-API of de afbeeldings-API wordt doorgegeven, de maximale grootte overschrijdt?
De tekst API retourneert een foutcode waarin wordt gemeld dat de tekst langer is dan toegestaan. De installatiekopie van het API kunt u ook een foutcode waarin wordt gemeld dat de installatiekopie niet voldoen aan de vereiste grootte retourneert.

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>Houdt u de installatiekopieën, tekst of video's die zijn ingediend voor controle?
Uw inhoud is uw eigen en kan niet door Microsoft worden behouden zonder uw toestemming. Microsoft gebruikt de toonaangevende veiligheidsmaatregelen uw inhoud te helpen beschermen.

#### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>Kan ik inhoud beheerder naar scherm voor ongeldig onderliggend uitbuiting installatiekopieën gebruiken?
Nee. Gekwalificeerde organisaties kunnen gebruiken, de [PhotoDNA Cloudservice](https://www.microsoft.com/photodna "Cloudservice van Microsoft PhotoDNA") voor dit type inhoud.

#### <a name="up-to-how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>Maximaal hoeveel teams bekijken kan een gebruiker koppelen? Kan de gebruiker overschakelen tussen teams?
Een gebruiker kan één team tegelijk toevoegen.

#### <a name="what-kind-of-team-member-roles-are-supported-by-the-review-tool-how-are-they-different"></a>Wat voor soort teamlid functies worden ondersteund door het hulpprogramma controleren? Hoe verschillen ze van elkaar?
De Studio kunt momenteel beheerder en revisor rollen toewijzen. De beheerders kunnen andere gebruikers uitnodigen en hebben toegang tot de configuratie-instellingen terwijl revisoren alleen kunnen bekijken van resultaten van toezicht en label of label inhoud verwijderen.

#### <a name="what-is-a-tag-does-the-review-tool-support-custom-tags"></a>Wat is er een label? Het hulpprogramma voor beoordeling biedt ondersteuning voor aangepaste labels?
Een tag is een of twee letters korte code een vlag toezicht zoals "a" voor volwassenen, 'r' voor mooie enzovoort geeft. Beheerders kunnen nieuwe labels voor hun bedrijf definiëren, indien nodig.

#### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>Hoeveel teamleden kan ik hebben in mijn team controleren?
U kunt maximaal vijf teamleden in een team hebben. Dit is inclusief de beheerder.
