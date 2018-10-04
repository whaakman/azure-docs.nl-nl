---
title: Beheer van Azure DevTest Labs-infrastructuur
description: Dit artikel bevat richtlijnen voor het beheer van Azure DevTest Labs-infrastructuur.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: spelluru
ms.openlocfilehash: 2edfa84530c147e1f716aef10c967ac36eadc922
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48251019"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---resources"></a>Beheer van infrastructuur in Azure DevTest Labs - Resources
In dit artikel komen de uitlijning en het beheer van resources voor DevTest Labs in uw organisatie. 

## <a name="align-within-an-azure-subscription"></a>Uitlijnen binnen een Azure-abonnement 

### <a name="question"></a>Vraag
Hoe ik uitlijnen DevTest Labs-resources binnen een Azure-abonnement?

### <a name="answer"></a>Antwoord
Voordat een organisatie begint om Azure te gebruiken voor het ontwikkelen van een algemene toepassing, moeten IT planners eerste introductie van de mogelijkheid als onderdeel van hun algehele portfolio van services aan controleren. Gebieden voor controle moeten de volgende problemen aanpakken:

- Hoe kunt u voor het meten van de kosten die gepaard gaan met de levenscyclus van toepassingsontwikkeling?
- Hoe de voorgestelde serviceoplossing met de zakelijke beveiligingsbeleid uitlijnen in de organisatie? 
- Is vereist voor het scheiden van de ontwikkeling en productie-omgevingen segmentering? 
- Welke besturingselementen worden geïntroduceerd voor eenvoudig beheer, de stabiliteit en groei van de lange termijn?

De **eerst aanbevolen praktijk** is om te controleren van organisaties Azure taxonomie waar de afdelingen tussen productie en ontwikkeling abonnementen worden beschreven. In het volgende diagram kunt u de voorgestelde taxonomie een logische scheiding van ontwikkeling/testen en productie-omgevingen. Met deze methode kan een organisatie factureringscodes om bij te houden die zijn gekoppeld aan elke omgeving afzonderlijk kosten veroorzaken. Zie voor meer informatie, [Prescriptieve abonnementsgovernance](/architecture/cloud-adoption/appendix/azure-scaffold). Bovendien kunt u [Azure tags](../azure-resource-manager/resource-group-using-tags.md) om resources voor het bijhouden en facturering te organiseren.

De **tweede aanbevolen** bestaat uit het inschakelen van het abonnement DevTest binnen de Azure Enterprise portal. Hierdoor kan een organisatie om uit te voeren van de client-besturingssystemen die niet algemeen beschikbaar zijn in een Azure enterprise-abonnement. Vervolgens gebruikt u zakelijke software waarbij u betaalt alleen voor de compute en niet geen zorgen over de licentieverlening. Het zorgt ervoor dat de facturering voor de aangewezen services, met inbegrip van de galerie met installatiekopieën in IaaS, zoals Microsoft SQL Server, is gebaseerd op verbruik alleen. Meer informatie over de Azure DevTest-abonnement vindt [hier](https://azure.microsoft.com/offers/ms-azr-0148p/) voor Enterprise Agreement (EA)-klanten en [hier](https://azure.microsoft.com/offers/ms-azr-0023p/) voor betalen klanten.

![Uitlijning van de resource met-abonnementen](./media/devtest-lab-guidance-governance/resource-alignment-with-subscriptions.png)

Dit model biedt een organisatie de flexibiliteit voor het implementeren van Azure DevTest Labs op schaal. Een organisatie kunt honderden labs ondersteuning voor verschillende bedrijfseenheden met 100 tot en met 1000 virtuele machines die parallel worden uitgevoerd. Bevordert het concept van een gecentraliseerde lab bedrijfsoplossing die dezelfde principes van Configuratiebeheer en beveiligingsmechanismen kunt delen.

Dit model zorgt er ook voor dat de organisatie niet uitputtend is voor de resourcelimieten die zijn gekoppeld aan hun Azure-abonnement. Zie voor meer informatie over het abonnement en Servicelimieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md). De DevTest Labs inrichtingsproces kunnen groot aantal resourcegroepen gebruiken. U kunt aanvragen voor limieten worden verhoogd door een ondersteuningsaanvraag in de Azure DevTest-abonnement. De resources in de productie-abonnement worden niet beïnvloed wanneer het abonnement ontwikkeling in gebruik groeit. Zie voor meer informatie over het schalen van DevTest Labs [quota en limieten voor schalen in DevTest Labs](devtest-lab-scale-lab.md).

Een algemene niveau voor uw abonnement die worden gereserveerd moet voor is hoe de netwerk-toewijzingen voor IP-bereik ter ondersteuning van zowel productie en ontwikkeling abonnementen worden toegewezen. Deze toewijzingen moeten rekening houden met groei na verloop van tijd (ervan uitgaande dat on-premises connectiviteit of een andere netwerktopologie die het bedrijf voor het beheren van hun netwerkstack in plaats van standaard naar de Azure-implementatie vereist). De aanbevolen procedure is dat een paar virtuele netwerken met een grote IP-adresvoorvoegsel toegewezen en gedeeld met veel grote subnetten in plaats van meerdere virtuele netwerken met kleine subnetten hebben. U kunt bijvoorbeeld 10 virtuele netwerken (één voor elk abonnement) definiëren met 10 abonnementen. Alle labs waarvoor isolatie niet kunnen delen hetzelfde subnet op van het abonnement vnet.

## <a name="maintain-naming-conventions"></a>Naamgevingsregels onderhouden

### <a name="question"></a>Vraag
Hoe ik een naamconventie onderhouden in mijn DevTest Labs-omgeving?

### <a name="answer"></a>Antwoord
Kunt u ze consistent maken in de DevTest Labs-omgeving om uit te breiden huidige enterprise-naamgevingsregels voor Azure-bewerkingen.

Bij het implementeren van DevTest Labs, wordt u aangeraden dat u specifieke vanaf beleidsregels hebt. U implementeert deze beleidsregels door een centrale script en JSON-sjablonen voor het afdwingen van consistentie. Naamgeving van beleid kan worden geïmplementeerd via Azure-beleid toegepast op het abonnementsniveau. Zie voor JSON-voorbeelden voor Azure Policy, [voorbeelden voor Azure Policy](../azure-policy/json-samples.md).

## <a name="number-of-users-per-lab-and-labs-per-organization"></a>Aantal gebruikers per lab en labs per organisatie

### <a name="question"></a>Vraag 
Hoe bepaal ik het aantal gebruikers per lab en het totale aantal labs binnen een organisatie nodig?

### <a name="answer"></a>Antwoord
Het is raadzaam dat bedrijfseenheden en ontwikkeling-groepen die gekoppeld aan hetzelfde ontwikkelingsproject zijn zijn gekoppeld aan de dezelfde lab. Hiermee kunt voor dezelfde typen beleidsregels, afbeeldingen en afsluiten beleidsregels worden toegepast op beide groepen. 

U moet mogelijk ook rekening houden met geografische grenzen. Voor ontwikkelaars in de Noordoost Verenigde Staten (VS) kunnen bijvoorbeeld gebruiken een lab ingericht in VS-Oost 2. En ontwikkelaars in Dallas, Texas, en Denver, Colorado kunnen worden omgeleid naar het gebruik van een resource in VS Zuid-centraal. Als er een gezamenlijke inspanning met een externe partij, kunnen deze worden toegewezen aan een lab die niet wordt gebruikt door interne ontwikkelaars. 

U kunt ook een lab voor een bepaald project in Visual Studio Team Services-teamproject. Vervolgens kunt toepassen u beveiliging door middel van een opgegeven Azure Active Directory-groep, waarmee u toegang krijgt tot beide set resources. Het virtuele netwerk dat is toegewezen aan het lab mag een andere grens consolideren van gebruikers.

## <a name="deletion-of-resources"></a>Het verwijderen van resources

### <a name="question"></a>Vraag
Hoe kunnen we voorkomen dat het verwijderen van resources in een lab?

### <a name="answer"></a>Antwoord
U wordt aangeraden de juiste machtigingen op het niveau van het lab te stellen zodat alleen gemachtigde gebruikers kunnen resources verwijderen of wijzigen van beleid voor lab maken. Ontwikkelaars moeten worden geplaatst in de **DevTest Labs gebruikers** groep. De ontwikkelaar van de potentiële klant of de infrastructuur potentiële klant moet de **DevTest Labs eigenaar**. Het is raadzaam dat u slechts twee lab-eigenaren hebt. Dit beleid is een uitbreiding naar de codeopslagplaats om te voorkomen dat beschadigd. Lab gebruikt rechten voor het gebruik van resources hebben maar lab-beleid kunnen niet worden bijgewerkt. Zie het volgende artikel met een lijst met de functies en de rechten die elke ingebouwde groep in een lab heeft: [eigenaars en gebruikers toevoegen in Azure DevTest Labs](devtest-lab-add-devtest-user.md).

## <a name="move-lab-to-another-resource-group"></a>Lab verplaatsen naar een andere resourcegroep 

### <a name="question"></a>Vraag
Is dit dan ondersteund voor het verplaatsen van een lab in een andere resourcegroep?

### <a name="answer"></a>Antwoord
Ja. Navigeer naar de pagina van de resourcegroep van de startpagina van uw testomgeving. Selecteer **verplaatsen** op de werkbalk en selecteert u de testomgeving die u wilt verplaatsen naar een andere resourcegroep. Wanneer u een lab maakt, wordt een resourcegroep automatisch voor u gemaakt. U wilt echter in het lab te verplaatsen naar een andere resourcegroep die volgt op de enterprise-naamconventies. 

## <a name="next-steps"></a>Volgende stappen
Zie [beheren van kosten en het eigendom](devtest-lab-guidance-governance-cost-ownership.md).