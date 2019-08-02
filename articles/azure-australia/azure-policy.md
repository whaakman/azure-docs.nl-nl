---
title: Beveiligings compatibiliteit met Azure Policy en Azure-blauw drukken
description: Naleving en afdwingen van beveiliging met Azure Policy en Azure-blauw drukken voor Australische overheids instanties in verband met de ASD-ISM en essentiële 8
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 0eb3e669f622a9c09c903530cbbab19206ce227f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571742"
---
# <a name="security-compliance-with-azure-policy-and-azure-blueprints"></a>Beveiligings compatibiliteit met Azure Policy en Azure-blauw drukken

De uitdaging van het afdwingen van governance binnen uw IT-omgeving, ongeacht of het een on-premises, Cloud systeem eigen of een hybride omgeving is, bestaat voor alle organisaties. Er moet een robuust technisch governance-Framework aanwezig zijn om ervoor te zorgen dat uw Microsoft Azure omgeving voldoet aan de ontwerp-, regelgevings-en beveiligings vereisten.

Voor de Australische overheids instanties kunnen zij overwegen om rekening te houden met de belangrijkste controles bij het beoordelen van het risico in de [ACSC-informatie (Information Security Manual) van het Australische Cyber Security Center](https://acsc.gov.au/infosec/ism/index.htm) (ISM). Voor het meren deel van de controles die in de ISM worden beschreven, moet de toepassing van Technical governance effectief worden beheerd en afgedwongen. Het is belang rijk dat u beschikt over de juiste hulpprogram ma's om de configuratie in uw omgevingen te evalueren en af te dwingen.

Microsoft Azure biedt twee aanvullende services die u helpen bij deze uitdagingen, Azure Policy en Azure-blauw drukken.

## <a name="azure-policy"></a>Azure-beleid

Azure Policy kunt de toepassing van de technische elementen van het IT-bestuur van een organisatie. Azure Policy bevat een voortdurend groeiende bibliotheek met ingebouwde beleids regels. Elk beleid dwingt regels en effecten af op de doel-Azure-resources.

Zodra een beleid is toegewezen aan resources, kan de algehele naleving van het beleid worden geëvalueerd en zo nodig worden hersteld.

Met deze bibliotheek van ingebouwde Azure policies kunnen organisaties snel de typen besturings elementen afdwingen die in de ACSC-ISM worden gevonden. Voor beelden van besturings elementen zijn:

* Virtuele machines bewaken voor ontbrekende systeem updates
* Accounts met verhoogde machtigingen voor multi-factor Authentication controleren
* Niet-versleutelde SQL-data bases identificeren
* Het gebruik van aangepaste op rollen gebaseerd toegangs beheer (RBAC) in azure controleren
* De Azure-regio's beperken waarin resources kunnen worden gemaakt

Als niet aan de governance-of regulerende controle wordt voldaan door een ingebouwde Azure Policy definitie, kan een aangepaste definitie worden gemaakt en toegewezen. Alle Azure Policy definities worden gedefinieerd in JSON en volgen een standaard [definitie](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure). Bestaande Azure Policy definities kunnen ook worden gedupliceerd en worden gebruikt om de basis van een aangepaste beleids definitie te vormen.

Het toewijzen van afzonderlijke Azure-beleids regels aan resources, met name in complexe omgevingen of in omgevingen met strikte wettelijke vereisten, kan grote overhead voor uw beheerders maken. Om u te helpen bij deze uitdagingen, kan een set van Azure-beleid samen worden gegroepeerd om een Azure Policy initiatief te vormen. Beleids initiatieven worden gebruikt voor het combi neren van gerelateerde Azure-beleids regels die, wanneer ze samen worden toegepast als groep, de basis vormen van een specifiek beveiligings-of nalevings doel. Micro soft voegt ingebouwde Azure Policy initiatief definities toe, met inbegrip van definities die zijn ontworpen om te voldoen aan specifieke wettelijke vereisten:

![Initiatieven voor nalevings beleid voor regelgeving](media/regulatory-initiatives.png)

Alle Azure-beleids regels en-initiatieven worden toegewezen aan een toewijzings bereik. Dit bereik is gedefinieerd op het niveau van het Azure-abonnement, de Azure-beheer groep of de Azure-resource groep. Zodra de vereiste Azure-beleids regels of beleids initiatieven zijn toegewezen, kan een organisatie de configuratie vereisten afdwingen voor alle nieuw gemaakte Azure-resources.

Het toewijzen van een nieuwe Azure Policy of initiatief heeft geen invloed op bestaande Azure-resources. Azure Policy kan; een organisatie echter in staat stellen om de compatibiliteit van bestaande Azure-resources te bekijken. Resources die als niet-compatibel zijn geïdentificeerd, kunnen worden doorgevoerd op de keuze van de organisatie

### <a name="azure-policy-and-initiatives-in-action"></a>Azure Policy en initiatieven in actie

De beschik bare ingebouwde Azure Policy en initiatief definities vindt u onder het knoop punt definitie in de sectie beleid van de Azure Portal:

![Ingebouwde Azure Policy definities](media/policy-definitions.png)

Met de bibliotheek met ingebouwde definities kunt u snel zoeken naar beleids regels die voldoen aan een organisatorische vereiste, de beleids definitie controleren en het beleid toewijzen aan de juiste resources. De ISM vereist bijvoorbeeld multi-factor Authentication (MFA) voor alle bevoegde gebruikers en voor alle gebruikers met toegang tot belang rijke gegevensopslag plaatsen. In Azure Policy kunt u zoeken naar ' MFA ' op de Azure Policy definities:

![Azure MFA-beleid](media/mfa-policies.png)

Zodra een geschikt beleid is geïdentificeerd, wijst u het beleid toe aan het gewenste bereik. Als er geen ingebouwd beleid is dat aan uw vereisten voldoet, kunt u het bestaande beleid dupliceren en de gewenste wijzigingen aanbrengen:

![Bestaande Azure Policy dupliceren](media/duplicate-policy.png)

Micro soft biedt ook een verzameling Azure Policy-voor beelden op [github](https://github.com/Azure/azure-policy) als een Snelstartgids waarmee u aangepaste Azure-beleids regels kunt maken. Deze beleids voorbeelden kunnen rechtstreeks naar de Azure Policy Editor worden gekopieerd binnen de Azure Portal.

Wanneer u Azure Policy-initiatieven maakt, kunt u de lijst met beschik bare beleids definities, zowel ingebouwde als aangepaste, met de vereiste definities sorteren.

U kunt bijvoorbeeld zoeken in de lijst met beschik bare Azure Policy definities voor alle beleids regels die betrekking hebben op virtuele Windows-machines. Vervolgens worden deze definities toegepast op een initiatief dat is ontworpen om aanbevolen procedures voor de beveiliging van virtuele machines uit te voeren:

![Lijst met Azure-beleids regels](media/initiative-definitions.png)

Wanneer u een Azure Policy of beleid toewijst aan een toewijzings bereik, kunt u Azure-resources uitsluiten van de effecten van het beleid door Azure Beheergroepen of Azure-resource groepen uit te sluiten.

### <a name="real-time-enforcement-and-compliance-assessment"></a>Beoordeling in realtime en naleving

Azure Policy nalevings scans van in-Scope Azure-resources worden uitgevoerd wanneer aan de volgende voor waarden wordt voldaan:

* Wanneer een Azure Policy of Azure Policy initiatief wordt toegewezen
* Wanneer het bereik van een bestaande Azure Policy of initiatief wordt gewijzigd
* Op aanvraag via de API Maxi maal 10 scans per uur
* Eenmaal per 24 uur: het standaard gedrag

Een beleids nalevings scan voor één Azure-resource wordt 15 minuten uitgevoerd nadat er een wijziging is aangebracht aan de resource.

Een overzicht van de Azure Policy naleving van resources kan worden gecontroleerd binnen de Azure Portal via het dash board beleids naleving:

![Nalevings Score Azure Policy](media/simple-compliance.png)

Het totale percentage voor bron naleving is een aggregatie van de naleving van alle geïmplementeerde resources in het bereik voor al uw toegewezen Azure-beleid. Zo kunt u de resources in een omgeving identificeren die niet compatibel zijn en het plan voor het beste oplossen van deze resources.

Het dash board beleids naleving bevat ook de wijzigings geschiedenis voor elke resource. Als een bron wordt geïdentificeerd als niet langer compatibel met het toegewezen beleid en automatisch herstel niet is ingeschakeld, kunt u zien wie de wijziging heeft aangebracht, wat er is gewijzigd en wanneer de wijzigingen zijn aangebracht in die resource.

## <a name="azure-blueprints"></a>Azure Blueprints

Met Azure blauw drukken worden de mogelijkheden van Azure Policy uitgebreid door ze te combi neren met:

* Azure RBAC
* Azure-resource groepen
* [Azure Resource Manager sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

Met blauw drukken kunt u omgevings ontwerpen maken waarmee u Azure-resources van Resource Manager-sjablonen implementeert, RBAC configureert en configuratie afdwingen en controleren door Azure Policy toe te wijzen. Blauw drukken vormen een bewerkbaar en opnieuw te implementeren omgeving sjabloon. Zodra de blauw druk is gemaakt, kan deze worden toegewezen aan een Azure-abonnement. Eenmaal toegewezen, worden alle Azure-resources die in de blauw druk zijn gedefinieerd, gemaakt en worden de Azure-beleids regels toegepast. De implementatie en configuratie van resources die zijn gedefinieerd in een Azure Blueprint, kunnen worden bewaakt vanuit de Azure Blueprint-console in de Azure Portal.

Azure-blauw drukken die zijn bewerkt, moeten opnieuw worden gepubliceerd in de Azure Portal. Telkens wanneer een blauw druk opnieuw wordt gepubliceerd, wordt het versie nummer van de blauw druk verhoogd. Met het versie nummer kunt u bepalen welke specifieke versie van een blauw druk is geïmplementeerd voor de Azure-abonnementen van een organisatie. De momenteel toegewezen versie van de blauw druk kan desgewenst worden bijgewerkt naar de meest recente versie.

Resources die zijn geïmplementeerd met een Azure Blueprint kunnen worden geconfigureerd met [Azure-resource vergrendelingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) op het moment van de implementatie. Resource vergrendelingen voor komen dat resources per ongeluk worden gewijzigd of verwijderd.

Micro soft ontwikkelt Azure Blueprint sjablonen voor diverse sectoren en wettelijke vereisten. De huidige bibliotheek met beschik bare Azure Blueprint definities kan worden weer gegeven op de pagina Azure Portal of [Azure-blauwdruk voor beveiliging en naleving](https://servicetrust.microsoft.com/ViewPage/BlueprintOverview/) in de service Trust-Portal.

### <a name="azure-blueprint-artifacts"></a>Azure Blueprint artefacten

Als u een Azure Blueprint wilt maken, kunt u beginnen met een lege blauw druk sjabloon of een van de bestaande voorbeeld blauw drukken als uitgangs punt gebruiken. U kunt artefacten toevoegen aan de blauw druk die als onderdeel van de implementatie wordt geconfigureerd:

![Azure Blueprint artefacten](media/blueprint-artifacts.png)

Deze artefacten kunnen de Azure-resource groep en-resources en de bijbehorende Azure Policy en beleids initiatieven bevatten voor het afdwingen van de configuratie die vereist is voor uw omgeving, zodat u aan de vereisten voldoet, bijvoorbeeld de ISM-besturings elementen voor systeem beveiliging.

Elk van deze artefacten kan ook worden geconfigureerd met para meters. Deze waarden worden gegeven wanneer de blauw druk is toegewezen aan een Azure-abonnement en geïmplementeerd. Met para meters kunt u één blauw druk maken en gebruiken om resources te implementeren in verschillende omgevingen, zonder dat u de onderliggende blauw druk hoeft te bewerken.

Micro soft ontwikkelt Azure PowerShell en CLI-cmdlets voor het maken en beheren van Azure-blauw drukken met de bedoeling dat een blauw druk kan worden behouden en geïmplementeerd door een organisatie via een CI/CD-pijp lijn.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt uitgelegd hoe governance en beveiliging kan worden afgedwongen met Azure Policy en Azure-blauw drukken. Nu u op hoog niveau hebt weer gegeven, leert u hoe u elke service in detail kunt gebruiken:

* [Overzicht van Azure Policy](https://docs.microsoft.com/azure/governance/policy/overview)
* [Overzicht van Azure-blauw drukken](https://azure.microsoft.com/services/blueprints/)
* [Voorbeelden van Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/index)
* [Opslag plaats voor Azure Policy-voor beelden](https://github.com/Azure/azure-policy)
* [Structuur van Azure Policy definitie](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure)
* [Azure Policy effecten](https://docs.microsoft.com/azure/governance/policy/concepts/effects)
