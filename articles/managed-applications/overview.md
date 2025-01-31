---
title: Overzicht van Azure Managed Applications | Microsoft Docs
description: Beschrijft de concepten van Azure Managed Applications
author: tfitzmac
ms.service: managed-applications
ms.topic: overview
ms.date: 05/31/2019
ms.author: tomfitz
ms.openlocfilehash: 5b6cb030c6eba5d80dfd046f1c3950609da1ed73
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479821"
---
# <a name="azure-managed-applications-overview"></a>Overzicht van Azure Managed Applications

Met Azure Managed Applications kunt cloudoplossingen bieden die consumenten eenvoudig kunnen implementeren en gebruiken. U implementeert de infrastructuur en biedt blijvende ondersteuning. Als u een beheerde toepassing beschikbaar wilt stellen voor alle klanten, publiceert u de toepassing in de Azure Marketplace. Als u de toepassing alleen beschikbaar wilt stellen voor gebruikers in uw organisatie, publiceert u de toepassing in een interne catalogus. 

Een beheerde toepassing is vergelijkbaar met een oplossingssjabloon in de Microsoft Azure Marketplace, met één belangrijk verschil. In een beheerde toepassing worden de resources geïmplementeerd vanuit een resourcegroep die wordt beheerd door de uitgever van de app. De resourcegroep is opgenomen in het abonnement van de consument, maar een identiteit in de tenant van de uitgever heeft toegang tot de resourcegroep. De uitgever bepaalt de kosten voor de voortdurende ondersteuning van de oplossing.

## <a name="advantages-of-managed-applications"></a>Voordelen van beheerde toepassingen

Beheerde toepassingen verminderen de barrières voor consumenten om uw oplossingen te gebruiken. Ze hebben geen kennis van de cloudinfrastructuur nodig om uw oplossing te kunnen gebruiken. Gebruikers hebben beperkte toegang tot de kritieke bronnen, dus ze hoeven zich geen zorgen te maken wanneer ze fouten maken bij het beheren hiervan. 

Met beheerde toepassingen kunt u een langdurige relatie tot stand brengen met uw consumenten. U bepaalt de voorwaarden voor het beheer van de toepassing en alle kosten worden afgehandeld via Azure Billing.

Hoewel klanten deze beheerde toepassingen implementeren in hun abonnementen, hoeven ze deze niet te onderhouden of bij te werken. U kunt ervoor zorgen dat alle klanten goedgekeurde versies gebruiken. Klanten hoeven geen toepassingsspecifieke domeinkennis te ontwikkelen om deze toepassingen te beheren. Klanten verkrijgen automatisch toepassingsupdates en hoeven zich geen zorgen te maken over probleemoplossing en diagnoseproblemen met de toepassingen. 

Met beheerde toepassingen kunnen IT-teams de gebruikers in hun organisatie vooraf goedgekeurde oplossingen aanbieden. U weet dat deze oplossingen voldoen aan de organisatiestandaarden.

Beheerde toepassingen ondersteuning [beheerde identiteiten voor een Azure-resources](./publish-managed-identity.md).

## <a name="types-of-managed-applications"></a>Soorten beheerde toepassingen

U kunt uw beheerde toepassing zowel extern als intern publiceren.

![Intern of extern publiceren](./media/overview/manage_app_options.png)

### <a name="service-catalog"></a>Servicecatalogus

De servicecatalogus is een interne catalogus van goedgekeurde oplossingen voor gebruikers in een organisatie. U gebruikt de catalogus om te voldoen aan de organisatiestandaarden en biedt tegelijkertijd oplossingen voor organisaties. Werknemers gebruiken de catalogus om eenvoudig toepassingen te vinden die worden aanbevolen en zijn goedgekeurd door hun IT-afdelingen. Ze zien welke beheerde toepassingen andere personen in hun organisatie met hen delen.

Zie [Een beheerde toepassing voor intern verbruik publiceren](publish-service-catalog-app.md) voor meer informatie over het publiceren van beheerde toepassing in een servicecatalogus.

### <a name="marketplace"></a>Marketplace

Leveranciers die voor hun services betaald willen krijgen, kunnen een beheerde toepassing beschikbaar maken via de Azure Marketplace. Nadat de leverancier een toepassing heeft gepubliceerd, is deze beschikbaar voor gebruikers buiten de organisatie. Op deze manier kunnen MSP's (providers van beheerde services), ISV's (onafhankelijke softwareleveranciers) en SI's (systeemintegrators) hun oplossingen aanbieden aan alle Azure-klanten.

Zie [Marketplace-toepassing maken](publish-marketplace-app.md) voor meer informatie over het publiceren van beheerde toepassingen in de Microsoft Azure Marketplace.

## <a name="resource-groups-for-managed-applications"></a>Resourcegroepen voor beheerde toepassingen

Meestal zijn de resources voor een beheerde toepassing in twee resourcegroepen. De consument beheert één resourcegroep en de uitgever beheert de andere. Bij het definiëren van de beheerde toepassing bepaalt de uitgever de toegangsniveaus. De uitgever kan een van beide een permanente rol wilt toewijzen, aanvragen of [just-in-time-toegang](request-just-in-time-access.md) voor een opdracht die is beperkt tot een bepaalde periode.

Het beperken van de toegang voor [gegevensbewerkingen](../role-based-access-control/role-definitions.md) wordt momenteel niet voor alle gegevensproviders in Azure ondersteund.

In de volgende afbeelding ziet u een scenario waarbij de uitgever de rol van eigenaar voor de beheerde resourcegroep aanvraagt. De uitgever plaatst een alleen-lezen-vergrendeling op deze resourcegroep voor de consument. De uitgeversidentiteiten die toegang tot de beheerde resourcegroep krijgen, zijn uitgesloten van de vergrendeling.

![Toegang tot resourcegroep](./media/overview/access.png)

### <a name="application-resource-group"></a>Resourcegroep van toepassing

Deze resourcegroep bevat het exemplaar van de beheerde toepassing. Deze resourcegroep mag maar één resource bevatten. Het resourcetype van de beheerde toepassing is **Microsoft.Solutions/applications**.

De consument heeft volledige toegang tot de resourcegroep en gebruikt deze om de levenscyclus van de beheerde toepassing te beheren.

### <a name="managed-resource-group"></a>Beheerde resourcegroep

Deze resourcegroep bevat alle resources die de beheerde toepassing nodig heeft. Deze resourcegroep bevat bijvoorbeeld de virtuele machines, opslagaccounts en virtuele netwerken voor de oplossing. De consument heeft beperkte toegang tot deze resourcegroep omdat de consument de afzonderlijke resources voor de beheerde toepassing niet beheert. De toegang van de uitgever tot deze resourcegroep komt overeen met de rol die is opgegeven in de definitie van de beheerde toepassing. De uitgever kan bijvoorbeeld de rol van eigenaar of bijdrager voor deze resourcegroep aanvragen. De toegang is beperkt tot een bepaald tijdstip of permanent.

Wanneer de gebruiker de beheerde toepassing verwijdert, wordt ook de beheerde resourcegroep verwijderd.

## <a name="azure-policy"></a>Azure Policy

U kunt een [Azure Policy](../governance/policy/overview.md) toepassen op uw beheerde toepassing. U past beleidsregels toe om te verzekeren dat geïmplementeerde exemplaren van uw beheerde toepassing aan gegevens- en beveiligingsvereisten voldoen. Als uw toepassing met gevoelige gegevens werkt, zorg er dan voor dat u hebt geëvalueerd hoe die moeten worden beschermd. Als uw toepassing bijvoorbeeld met gegevens uit Office 365 werkt, past u een beleid toe om te verzekeren dat gegevensversleuteling is ingeschakeld.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de voordelen van het gebruik van beheerde toepassingen. Ga naar het volgende artikel om een definitie van een ​​beheerde toepassing te maken.

> [!div class="nextstepaction"]
> [Snelstart: De definitie van een door Azure beheerde toepassing publiceren](publish-managed-app-definition-quickstart.md)
