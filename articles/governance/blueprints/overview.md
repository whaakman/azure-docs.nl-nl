---
title: Overzicht van Azure Blueprints
description: Azure Blueprints is een service in Azure waarmee u artefacten kunt maken, definiëren en implementeren in uw Azure-omgeving.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/07/2018
ms.topic: overview
ms.service: blueprints
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: b2e767bf27962472a19e5d2e704b456cffe18423
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277525"
---
# <a name="what-is-azure-blueprints"></a>Wat is Azure Blueprints?

Net zoals een blauwdruk het een ingenieur of architect mogelijk maakt om de ontwerpparameters voor een project te schetsen, stelt Azure Blueprints cloudarchitecten en centrale IT-groepen in staat om een ​​herhaalbare set Azure-resources te definiëren die voldoet aan de normen, patronen en vereisten van een organisatie en deze implementeert. Met Azure Blueprints kunnen ontwikkelteams snel nieuwe omgevingen bouwen en instellen in het volste vertrouwen dat ze voldoen aan de vereisten van de organisatie, met een set ingebouwde componenten, zoals netwerken, om de ontwikkeling en levering te versnellen.

Blauwdrukken zijn een declaratieve manier om de implementatie van diverse resourcesjablonen en andere artefacten te orkestreren, zoals:

- Roltoewijzingen
- Beleidstoewijzingen
- Azure Resource Manager-sjablonen
- Resourcegroepen

De Azure Blueprints-service wordt gesteund door de wereldwijd gedistribueerde [Azure Cosmos DB](../../cosmos-db/introduction.md).
Blauwdrukobjecten worden naar meerdere Azure-regio's gerepliceerd. Deze replicatie biedt lage latentie, hoge beschikbaarheid en consistente toegang tot uw blauwdrukobjecten, ongeacht in welke regio uw resources worden geïmplementeerd.

## <a name="how-its-different-from-resource-manager-templates"></a>Waarin verschilt het van Resource Manager-sjablonen?

De service is ontworpen om u te helpen met de _setup van de omgeving_. Deze setup bestaat vaak uit een set resourcegroepen, beleidsmaatregelen, roltoewijzingen en Resource Manager-sjabloonimplementaties. Een blauwdruk is een pakket waarmee al deze _artefact_typen bij elkaar worden gebracht en u in staat wordt gesteld om dat pakket samen te stellen en versies ervan te beheren – ook via een CI/CD-pijplijn. Uiteindelijk wordt elke blauwdruk toegewezen aan een abonnement in één bewerking die kan worden gecontroleerd en bijgehouden.

Bijna alles dat u voor implementatie in Blueprints wilt opnemen, kan worden bereikt met een Resource Manager-sjabloon. Een Resource Manager-sjabloon is echter een document dat niet in Azure zelf bestaat. Elke sjabloon wordt lokaal of in broncodebeheer opgeslagen. De sjabloon wordt gebruikt voor de implementatie van een of meer Azure-resources, maar zodra die resources zijn geïmplementeerd, is er geen actieve verbinding of relatie meer met de sjabloon.

Met Blueprints blijft de relatie tussen de blauwdrukdefinitie (wat _moet worden_ geïmplementeerd) en de blauwdruktoewijzing (wat _is_ geïmplementeerd) behouden. Deze verbinding ondersteunt verbeterde tracering en controle van implementaties. Met blauwdrukken kunt u bovendien in één keer een upgrade uitvoeren van meerdere abonnementen die zijn onderworpen aan dezelfde blauwdruk.

U hoeft niet te kiezen tussen een Resource Manager-sjabloon en een blauwdruk. Elke blauwdruk kan bestaan uit nul of meer Resource Manager-sjabloon_artefacten_. Deze ondersteuning betekent dat eerdere inspanningen om een ​​bibliotheek met Resource Manager-sjablonen te ontwikkelen en te onderhouden, in Blueprints kunnen worden hergebruikt.

## <a name="how-its-different-from-azure-policy"></a>Wat is het verschil met Azure Policy?

Een blauwdruk is een pakket of container voor het samenstellen van focusspecifieke sets van standaarden, patronen en vereisten met betrekking tot de implementatie van Azure-cloudservices, beveiliging en ontwerp, die kunnen worden hergebruikt om consistentie en naleving te behouden.

Een [beleid](../policy/overview.md) is een systeem voor standaard toestaan en expliciet weigeren dat is gericht op resource-eigenschappen tijdens implementatie en voor reeds bestaande resources. Het ondersteunt cloudgovernance door te valideren dat resources binnen een abonnement voldoen aan vereisten en normen.

Door een beleid in een blauwdruk op te nemen, kunnen tijdens de toewijzing van de blauwdruk het juiste patroon of ontwerp worden gemaakt. Door een beleid op te nemen, wordt ervoor gezorgd dat alleen goedgekeurde of verwachte wijzigingen kunnen worden aangebracht in de omgeving, om continue naleving van de intentie van de blauwdruk te beschermen.

Een beleid kan worden opgenomen als een van de vele _artefacten_ in de definitie van een blauwdruk. Blauwdrukken ondersteunen ook het gebruik van parameters in beleid en initiatieven.

## <a name="blueprint-definition"></a>Blauwdrukdefinitie

Een blauwdruk bestaat uit _artefacten_. Blauwdrukken ondersteunen momenteel de volgende resources als artefacten:

|Resource  | Hiërarchieopties| Beschrijving  |
|---------|---------|---------|
|Resourcegroepen     | Abonnement | Een nieuwe resourcegroep maken voor gebruik door andere artefacten binnen de blauwdruk.  Met deze tijdelijke resourcegroepen kunt u resources precies zo indelen als u ze wilt structureren, en het bereik beperken voor opgenomen beleids- en roltoewijzingsartefacten, en Azure Resource Manager-sjablonen.         |
|Azure Resource Manager-sjabloon      | Resourcegroep | Sjablonen worden gebruikt om complexe omgevingen samen te stellen. Voorbeeldomgevingen: een SharePoint-farm, Azure Automation State Configuration of een Log Analytics-werkruimte. |
|Beleidstoewijzing     | Abonnement, resourcegroep | Hiermee kan een beleid of initiatief worden toegewezen aan het abonnement waaraan de blauwdruk wordt toegewezen. Het beleid of initiatief moet binnen het bereik van de blauwdruk vallen (in de blauwdrukbeheergroep). Als het beleid of initiatief parameters heeft, worden deze parameters toegewezen bij het maken of toewijzen van de blauwdruk.       |
|Roltoewijzing   | Abonnement, resourcegroep | Voeg een bestaande gebruiker of groep toe aan een ingebouwde rol om ervoor te zorgen dat de juiste mensen altijd over de juiste toegang tot uw bronnen beschikken. Roltoewijzingen kunnen voor het hele abonnement worden gedefinieerd of genest in een specifieke resourcegroep die in de blauwdruk is opgenomen. |

### <a name="blueprints-and-management-groups"></a>Blauwdrukken en beheergroepen

Bij het maken van een blauwdrukdefinitie definieert u waar de blauwdruk wordt opgeslagen. Momenteel kunnen blauwdrukken alleen worden opgeslagen in een [beheergroep](../management-groups/overview.md) waartoe u **Inzender**-toegang hebt. De blauwdruk is beschikbaar voor toewijzing aan elk onderliggend abonnement van die beheergroep.

> [!IMPORTANT]
> Als u geen toegang hebt tot beheergroepen of als er geen beheergroepen zijn geconfigureerd, wordt bij het laden van de lijst met blauwdrukdefinities aangegeven dat er geen beschikbaar zijn, en wordt er als u op **Bereik** klikt een ​​venster geopend met een waarschuwing over het ophalen van beheergroepen. U kunt dit oplossen door ervoor te zorgen dat een abonnement waartoe u de juiste toegang hebt, deel uitmaakt van een [beheergroep](../management-groups/overview.md).

### <a name="blueprint-parameters"></a>Blauwdrukparameters

Blauwdrukken kunnen parameters doorgeven aan een beleid/initiatief of een Azure Resource Manager-sjabloon.
Tijdens het toevoegen van een van deze _​​artefacten_ aan een blauwdruk, besluit de auteur om een ​​gedefinieerde waarde op te geven voor elke blauwdruktoewijzing, of toe te staan ​​dat elke blauwdruktoewijzing op het moment van de toewijzing een waarde opgeeft. Dankzij deze flexibiliteit hebt u de optie om een ​​vooraf vastgestelde waarde te definiëren voor elk gebruik van de blauwdruk of om die beslissing te nemen op het moment van toewijzing.

> [!NOTE]
> Een blauwdruk kan zijn eigen parameters hebben, maar deze kunnen op dit moment alleen worden gemaakt als een blauwdruk wordt gegenereerd vanuit de REST API in plaats van via de portal.

Zie [blauwdrukparameters](./concepts/parameters.md) voor meer informatie.

### <a name="blueprint-publishing"></a>Blauwdruk publiceren

Wanneer een blauwdruk wordt gemaakt, bevindt deze zich in de modus **Concept**. Wanneer de blauwdruk klaar is om te worden toegewezen, moet deze worden **Gepubliceerd**. Om een blauwdruk te publiceren moet u een **Versie**-tekenreeks definiëren (letters, cijfers en afbreekstreepjes, maximaal 20 tekens lang), plus eventuele **Wijzigingsnotities**. De **versie** onderscheidt deze blauwdruk van toekomstige gewijzigde versies ervan, en maakt het mogelijk elke versie toe te wijzen. Dit versiebeheer betekent ook dat verschillende **versies** van dezelfde blueprint kunnen worden toegewezen aan hetzelfde abonnement. Wanneer er aanvullende wijzigingen in de blauwdruk worden aangebracht, bestaat de **gepubliceerde** **versie** nog steeds, net als de **niet-gepubliceerde wijzigingen**. Nadat de wijzigingen voltooid zijn, wordt de bijgewerkte blauwdruk **Gepubliceerd** met een nieuwe en unieke **versie**, en kan nu ook worden toegewezen.

## <a name="blueprint-assignment"></a>Blauwdruktoewijzing

Elke **gepubliceerde** **versie** van een blauwdruk kan worden toegewezen aan een bestaand abonnement. In de portal wordt standaard de **versie** van de blauwdruk gebruikt die het laatst is **gepubliceerd**. Als er artefactparameters (of blauwdrukparameters) zijn, worden de parameters tijdens het toewijzingsproces gedefinieerd.

## <a name="permissions-in-azure-blueprints"></a>Machtigingen in Azure Blueprints

Om blauwdrukken te kunnen gebruiken, moet u zijn gemachtigd via [op rollen gebaseerd toegangsbeheer](../../role-based-access-control/overview.md) (RBAC). Om blauwdrukken te maken, moet uw account de volgende machtigingen hebben:

- `Microsoft.Blueprint/blueprints/write` - Een blauwdrukdefinitie maken
- `Microsoft.Blueprint/blueprints/artifacts/write` - Artefacten in een blauwdrukdefinitie maken
- `Microsoft.Blueprint/blueprints/versions/write` - Een blauwdruk publiceren

Om blauwdrukken te verwijderen, moet uw account de volgende machtigingen hebben:

- `Microsoft.Blueprint/blueprints/delete`
- `Microsoft.Blueprint/blueprints/artifacts/delete`
- `Microsoft.Blueprint/blueprints/versions/delete`

> [!NOTE]
> Omdat blauwdrukdefinities in een beheergroep worden gemaakt, moeten de machtigingen voor blauwdrukdefinities worden toegekend voor een beheergroepbereik, of worden overgenomen in een beheergroepbereik.

Als u een blauwdruk wilt toewijzen of de toewijzing ongedaan wilt maken, heeft uw account de volgende machtigingen nodig:

- `Microsoft.Blueprint/blueprintAssignments/write` - Een blauwdruk toewijzen
- `Microsoft.Blueprint/blueprintAssignments/delete` - De toewijzing van een blauwdruk ongedaan maken

> [!NOTE]
> Omdat blauwdruktoewijzingen op een abonnement worden gemaakt, moeten de machtigingen voor het toewijzen van blauwdrukken en het ongedaan maken van toewijzingen worden toegekend op abonnementsbereik of worden overgenomen in een abonnementsbereik.

Behalve toewijzingsmachtigingen voor blauwdrukken zijn deze machtigingen opgenomen in de rol **​​Eigenaar** en de **Inzender**-rollen. Als deze ingebouwde rollen niet aan uw beveiligingsbehoeften voldoen, kunt u een [aangepaste rol](../../role-based-access-control/custom-roles.md) maken.

> [!NOTE]
> De service-principal voor Azure Blueprint vereist de **Eigenaar**-rol op het toegewezen abonnement om implementatie mogelijk te maken. Als u de portal gebruikt, wordt deze rol automatisch verleend en ingetrokken voor de implementatie. Als u de REST API gebruikt, moet deze rol handmatig worden toegekend, maar wordt deze automatisch ingetrokken nadat de implementatie is voltooid.

## <a name="next-steps"></a>Volgende stappen

- [Een blauwdruk maken - Portal](create-blueprint-portal.md)
- [Een blauwdruk maken - REST API](create-blueprint-rest-api.md)