---
title: Veelgestelde vragen - Content Moderator
titlesuffix: Azure Cognitive Services
description: Krijg antwoorden op veelgestelde vragen over Content Moderator.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: faq
ms.date: 11/21/2016
ms.author: sajagtap
ms.openlocfilehash: 1c55ef6fdfefbd50442da63637ba98bc43865d27
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206099"
---
# <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

#### <a name="what-does-my-content-moderator-subscription-include"></a>Wat doet de Content Moderator abonnement?
Uw Content Moderator-abonnement omvat toegang tot het beoordelingsprogramma en de API's. U kunt beslissen of u wilt gebruiken een of andere of beide, afhankelijk van de behoeften van uw bedrijf.

#### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>Wat zijn de limieten/beperkingen van de inhoud waarop toezicht kan worden gehouden door de API te gebruiken?
Wanneer u de API, moeten de afbeeldingen hebben minimaal 128 pixels en een maximale bestandsgrootte van 4 MB. Tekst mag maximaal 1024 tekens. Er is geen limiet voor de duur van de video.

#### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>Wat gebeurt er als de inhoud die naar de tekst-API of de afbeeldings-API wordt doorgegeven, de maximale grootte overschrijdt?
De tekst-API retourneert een foutcode die aangeeft dat de tekst langer is dan toegestaan. De afbeeldings-API retourneert ook een foutcode die aangeeft dat de installatiekopie niet voldoen aan de groottevereisten voldoet.

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>Houd u de afbeeldingen, tekst of video's die zijn ingediend voor het beheer?
Uw inhoud is uw eigen en kan niet door Microsoft worden behouden zonder uw toestemming. Microsoft maakt gebruik van toonaangevende beveiligingsmaatregelen uw inhoud te helpen beschermen.

#### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>Kan ik Content Moderator scherm voor ongeldig onderliggend misbruik installatiekopieën gebruiken?
Nee. Echter gekwalificeerde organisaties kunnen gebruiken de [PhotoDNA Cloudservice](https://www.microsoft.com/photodna "Cloudservice van Microsoft PhotoDNA") scherm voor dit type inhoud.

#### <a name="up-to-how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>Maximaal hoeveel teams bekijken kan een gebruiker toevoegen? Kan de gebruiker wisselen tussen teams?
Een gebruiker kan deelnemen aan één team tegelijk.

#### <a name="what-kind-of-team-member-roles-are-supported-by-the-review-tool-how-are-they-different"></a>Wat voor soort teamlid functies worden ondersteund door het beoordelingsprogramma? Hoe verschillen ze van elkaar?
De Studio kunt momenteel beheerder en de revisor rollen toewijzen. De beheerders kunnen andere gebruikers uitnodigen en hebben toegang tot de configuratie-instellingen terwijl revisoren alleen kunnen bekijken van resultaten van toezicht en het label of inhoud label verwijderen.

#### <a name="what-is-a-tag-does-the-review-tool-support-custom-tags"></a>Wat is een code? Het beoordelingsprogramma biedt ondersteuning voor aangepaste labels?
Een label is een of twee letters korte code die een vlag toezicht op, zoals 'a' voor volwassenen, 'r' voor het gebruik en enzovoort geeft. Beheerders kunnen nieuwe tags voor hun bedrijf definiëren, indien nodig.

#### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>Hoeveel teamleden kan ik heb in mijn beoordelingsteam?
U kunt maximaal vijf teamleden in een team hebben. Dit is inclusief de beheerder.
