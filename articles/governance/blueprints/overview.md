---
title: Overzicht van Azure Blueprints
description: Azure Blueprints is een service in Azure waarmee u artefacten kunt maken, definiëren en implementeren in uw Azure-omgeving.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: overview
ms.service: blueprints
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: c7ffbe86407bc776870890e5b4151556f572832e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957502"
---
# <a name="what-is-azure-blueprints"></a>Wat is Azure Blueprints?

Net zoals een blauwdruk het een ingenieur of architect mogelijk maakt om de ontwerpparameters voor een project te schetsen, stelt Azure Blueprints cloudarchitecten en centrale IT in staat om een ​​herhaalbare set Azure-resources te definiëren die voldoet aan de normen, patronen en vereisten van een organisatie en deze implementeert. Met Azure Blueprints kunnen ontwikkelteams snel nieuwe omgevingen inrichten en opbouwen in de wetenschap dat ze voldoen aan de vereisten van de organisatie en een set ingebouwde componenten bevatten – zoals netwerken – om de ontwikkeling en levering te versnellen.

Blauwdrukken zijn een declaratieve manier om de implementatie van meerdere resourcesjablonen en andere artefacten te orkestreren, zoals:

- Roltoewijzingen
- Beleidstoewijzingen
- Azure Resource Manager-sjablonen
- Resourcegroepen

## <a name="how-it-is-different-from-resource-manager-templates"></a>Waarin verschilt het van Resource Manager-sjablonen?

Blueprints is ontworpen om te helpen met _omgevingsconfiguratie_, die vaak bestaat uit een reeks resourcegroepen, beleidsregels en roltoewijzingen, naast implementaties van Resource Manager-sjablonen. Een blauwdruk is een pakket waarmee al deze _artefact_typen bij elkaar worden gebracht en u in staat wordt gesteld om dat pakket samen te stellen en versies ervan te beheren – ook via een CI/CD-pijplijn. Uiteindelijk wordt elke blauwdruk toegewezen aan een abonnement in één bewerking die kan worden gecontroleerd en bijgehouden.

Bijna alles dat u voor implementatie in Blueprints wilt opnemen, kan worden bereikt met een Resource Manager-sjabloon. Een Resource Manager-sjabloon is echter een document dat niet in Azure zelf bestaat – elke sjabloon wordt lokaal of in broncodebeheer opgeslagen. De sjabloon wordt gebruikt voor implementatie van een of meer Azure-bronnen, maar zodra die bronnen zijn geïmplementeerd, gaan de verbinding en de relatie met de gebruikte sjabloon verloren.

Met Blueprints blijft de relatie tussen de blauwdrukdefinitie (wat _moet worden_ geïmplementeerd) en de blauwdruktoewijzing (wat _is_ geïmplementeerd) in stand. Deze verbinding zorgt voor een betere tracking en controle van implementaties, de mogelijkheid om meerdere abonnementen die worden beheerst door dezelfde blauwdruk tegelijk te upgraden, en meer.

U hoeft niet te kiezen tussen een Resource Manager-sjabloon en een blauwdruk. Elke blauwdruk kan bestaan uit nul of meer Resource Manager-sjabloon_artefacten_. Dat betekent dat eerdere inspanningen om een ​​bibliotheek met Resource Manager-sjablonen te ontwikkelen en te onderhouden, kunnen worden gebruikt in Blueprints.

## <a name="how-it-is-different-from-azure-policy"></a>Wat is het verschil met Azure Policy?

Een blauwdruk is een pakket of container voor het samenstellen van focusspecifieke sets van standaarden, patronen en vereisten met betrekking tot de implementatie van Azure-cloudservices, beveiliging en ontwerp, die kunnen worden hergebruikt om consistentie en naleving te waarborgen.

Een [beleid](../policy/overview.md) is een systeem voor standaard toestaan en expliciet weigeren dat is gericht op resource-eigenschappen tijdens implementatie en voor reeds bestaande resources. Het ondersteunt IT-governance door ervoor te zorgen dat resources binnen een abonnement voldoen aan vereisten en normen.

Het opnemen van een beleid in een blauwdruk maakt het niet alleen mogelijk het juiste patroon of ontwerp te maken tijdens het toewijzen van de blauwdruk, maar zorgt ervoor dat er alleen goedgekeurde of verwachte wijzigingen kunnen worden aangebracht in de omgeving om ervoor te zorgen dat deze blijft voldoen aan de intentie van de blauwdruk.

Een beleid kan worden opgenomen als een van de vele _artefacten_ in de definitie van een blauwdruk. Blauwdrukken ondersteunen ook het gebruik van parameters in beleid en initiatieven.

## <a name="blueprint-definition"></a>Blauwdrukdefinitie

Een blauwdruk bestaat uit _artefacten_. Blauwdrukken ondersteunen momenteel de volgende resources als artefacten:

|Resource  | Hiërarchieopties| Beschrijving  |
|---------|---------|---------|
|Resourcegroepen     | Abonnement | Een nieuwe resourcegroep maken voor gebruik door andere artefacten binnen de blauwdruk.  Met deze tijdelijke resourcegroepen kunt u resources precies zo indelen als u ze wilt structureren, en het bereik beperken voor opgenomen beleid- en roltoewijzingsartefacten, evenals Azure Resource Manager-sjablonen.         |
|Azure Resource Manager-sjabloon      | Resourcegroep | Deze sjablonen kunnen worden gebruikt om complexe omgevingen samen te stellen, zoals een SharePoint-farm, Azure Automation State Configuration of een Log Analytics-werkruimte. |
|Beleidstoewijzing     | Abonnement, resourcegroep | Hiermee kan een beleid of initiatief worden toegewezen aan de beheergroep of het abonnement waaraan de blauwdruk wordt toegewezen. Het beleid of initiatief moet binnen het bereik van de blauwdruk vallen (in de blauwdrukbeheergroep). Als het beleid of initiatief parameters heeft, kunnen deze parameters worden toegewezen bij het maken of toewijzen van de blauwdruk.       |
|Roltoewijzing   | Abonnement, resourcegroep | Voeg een bestaande gebruiker of groep toe aan een ingebouwde rol om ervoor te zorgen dat de juiste mensen altijd over de juiste toegang tot uw bronnen beschikken. Roltoewijzingen kunnen voor het hele abonnement worden gedefinieerd of genest in een specifieke resourcegroep die in de blauwdruk is opgenomen. |

### <a name="blueprints-and-management-groups"></a>Blauwdrukken en beheergroepen

Bij het maken van een blauwdrukdefinitie definieert u waar de blauwdruk wordt opgeslagen. Momenteel kunnen blauwdrukken alleen worden opgeslagen in een [beheergroep](../management-groups/overview.md) waartoe u **Inzender**-toegang hebt. De blauwdruk is dan beschikbaar voor toewijzing aan elk onderliggend element (beheergroep of abonnement) van die beheergroep.

> [!IMPORTANT]
> Als u geen toegang hebt tot beheergroepen of als er geen beheergroepen zijn geconfigureerd, wordt bij het laden van de lijst met blauwdrukdefinities aangegeven dat er geen beschikbaar zijn, en wordt er als u op **Bereik** klikt een ​​venster geopend met een waarschuwing over het ophalen van beheergroepen. U kunt dit oplossen door ervoor te zorgen dat een abonnement waartoe u de juiste toegang hebt, deel uitmaakt van een [beheergroep](../management-groups/overview.md).

### <a name="blueprint-parameters"></a>Blauwdrukparameters

Blauwdrukken kunnen parameters doorgeven aan een beleid/initiatief of een Azure Resource Manager-sjabloon.
Wanneer een van deze _​​artefacten_ wordt toegevoegd aan een blauwdruk, kan de auteur een ​​gedefinieerde waarde opgeven voor elke blauwdruktoewijzing, of toestaan ​​dat elke blauwdruktoewijzing een waarde opgeeft ten tijde van de toewijzing. Dankzij deze flexibiliteit hebt u de optie om een ​​vooraf vastgestelde waarde te definiëren voor elk gebruik van de blauwdruk of om die beslissing te nemen op het moment van toewijzing.

> [!NOTE]
> Een blauwdruk kan zijn eigen parameters hebben, maar deze kunnen op dit moment alleen worden gemaakt als een blauwdruk wordt gegenereerd vanuit de REST API in plaats van via de portal.

Zie [blauwdrukparameters](./concepts/parameters.md) voor meer informatie.

### <a name="blueprint-publishing"></a>Blauwdruk publiceren

Wanneer een blauwdruk wordt gemaakt, bevindt deze zich in de modus **Concept**. Wanneer de blauwdruk klaar is om te worden toegewezen, moet deze worden **Gepubliceerd**. Om een blauwdruk te publiceren moet u een **Versie**-tekenreeks definiëren (letters, cijfers en afbreekstreepjes, maximaal 20 tekens lang), plus eventuele **Wijzigingsnotities**.
De **versie** onderscheidt deze blauwdruk van toekomstige gewijzigde versies ervan, en maakt het mogelijk elke versie toe te wijzen. Dit betekent ook dat verschillende **versies** van dezelfde blueprint kunnen worden toegewezen aan hetzelfde abonnement. Wanneer er aanvullende wijzigingen in de blauwdruk worden aangebracht, bestaat de **gepubliceerde** **versie** nog steeds, naast de **niet-gepubliceerde wijzigingen**. Nadat de wijzigingen voltooid zijn, wordt de bijgewerkte blauwdruk **Gepubliceerd** met een nieuwe en unieke **versie**, en kan nu ook worden toegewezen.

## <a name="blueprint-assignment"></a>Blauwdruktoewijzing

Elke **gepubliceerde** **versie** van een blauwdruk kan worden toegewezen aan een bestaand abonnement. In de portal wordt standaard de **versie** van de blauwdruk ingevuld die het laatst is **gepubliceerd**. Als er artefactparameters (of blauwdrukparameters) zijn, worden de parameters tijdens het toewijzingsproces gedefinieerd.

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

Deze machtigingen zijn opgenomen in de rol **​​Eigenaar** en zijn, met uitzondering van de toewijzingsmachtigingen voor blauwdrukken, ook opgenomen in de **Inzender**-machtigingen. Als deze ingebouwde rollen niet aan uw beveiligingsbehoeften voldoen, kunt u een [aangepaste rol](../../role-based-access-control/custom-roles.md) maken.

> [!NOTE]
> De service-principal voor Azure Blueprint vereist de **Eigenaar**-rol op het toegewezen abonnement om implementatie mogelijk te maken. Als u de portal gebruikt, wordt deze rol automatisch verleend en ingetrokken voor de implementatie. Als u de REST API gebruikt, moet deze rol handmatig worden toegekend, maar wordt deze automatisch ingetrokken nadat de implementatie is voltooid.

## <a name="next-steps"></a>Volgende stappen

- [Een blauwdruk maken - Portal](create-blueprint-portal.md)
- [Een blauwdruk maken - REST API](create-blueprint-rest-api.md)