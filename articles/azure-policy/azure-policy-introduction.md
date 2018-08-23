---
title: Overzicht van Azure-beleid
description: Azure Policy is een service in Azure die u gebruikt om beleidsdefinities in uw Azure-omgeving te maken, toe te wijzen en te beheren.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/31/2018
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 405f69ae1c37e478758d984ddf7dc0e267910fef
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42022668"
---
# <a name="what-is-azure-policy"></a>Wat is Azure Policy?

IT-beheer garandeert dat uw organisatie haar doelen kan bereiken via een effectief en efficiënt gebruik van IT. IT-beheer zorgt voor duidelijkheid tussen zakelijke doelstellingen en IT-projecten.

Ondervindt uw bedrijf een significant aantal IT-problemen die nooit lijken te worden opgelost?
Goed IT-beheer omvat het plannen van uw initiatieven en het stellen van prioriteiten op strategisch niveau, zodat problemen beheerst en voorkomen kunnen worden. Hiervoor gebruikt u Azure Policy.

Azure Policy is een service in Azure die u gebruikt om beleidsregels te maken, toe te wijzen en te beheren. Met deze beleidsregels worden verschillende regels en effecten afgedwongen voor uw resources, zodat deze resources voldoen aan de standaarden en service level agreements in uw bedrijf. Er wordt met Azure Policy een evaluatie uitgevoerd voor de resources, waarbij wordt gezocht naar resources die niet voldoen aan uw beleidsdefinities. U kunt bijvoorbeeld beleid hebben op basis waarvan alleen virtuele machines van een bepaalde SKU-grootte in uw omgeving zijn toegestaan. Zodra dit beleid is geïmplementeerd, wordt het geëvalueerd als er resources worden gemaakt en bijgewerkt, maar ook voor al bestaande resources. Verderop in deze documentatie, wordt dieper ingegaan op het maken en implementeren van beleidsregels met Azure Policy.

> [!IMPORTANT]
> Evaluatie van de naleving van Azure Policy is nu beschikbaar voor alle toewijzingen, ongeacht de prijscategorie. Als uw toewijzingen de nalevingsgegevens niet weergeven, controleert u of het abonnement is geregistreerd bij de resourceprovider Microsoft.PolicyInsights.

## <a name="how-is-it-different-from-rbac"></a>Wat is het verschil met RBAC?

Er zijn enkele belangrijke verschillen tussen beleid en RBAC (op rollen gebaseerd toegangsbeheer). RBAC is gericht op de acties van gebruikers in verschillende bereiken. U kunt bijvoorbeeld zijn toegevoegd aan de rol Inzender voor een resourcegroep in het gewenste bereik. Met deze rol kunt u wijzigingen aanbrengen in deze resourcegroep.
Beleid is gericht op resource-eigenschappen tijdens de implementatie en voor al bestaande resources. Via beleidsregels kunt u bijvoorbeeld de typen resources beheren die kunnen worden ingericht. Of u kunt het aantal locaties beperken waarop resources kunnen worden ingericht. In tegenstelling tot RBAC is beleid een standaardsysteem voor toestaan en expliciet weigeren.

### <a name="rbac-permissions-in-azure-policy"></a>RBAC-machtigingen in Azure Policy

Met Azure Policy worden machtigingen in twee verschillende resourceproviders voorgesteld als bewerkingen:

- [Microsoft.Authorization](../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsight](../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Voor enkele van de ingebouwde rollen bestaan diverse machtigingsniveaus voor Azure Policy-resources. Zo kan de **beveiligingsbeheerder** beleidstoewijzingen en -definities beheren maar geen informatie over naleving bekijken. Een **lezer** kan details betreffende beleidstoewijzingen en -definities lezen maar geen wijzigingen aanbrengen of informatie over naleving bekijken. Terwijl een **eigenaar** volledige rechten heeft, heeft een **inzender** geen Azure Policy-machtigingen. Maak een [aangepaste rol](../role-based-access-control/custom-roles.md) als u machtigingen wilt verlenen om de details van Azure Policy-naleving wilt bekijken.

## <a name="policy-definition"></a>Beleidsdefinitie

Het maken en implementeren van een beleidsregel in Azure Policy begint met het maken van een beleidsdefinitie. Elke beleidsdefinitie heeft voorwaarden voor het afdwingen ervan. En er is een bijbehorend effect dat wordt uitgevoerd als aan de voorwaarden wordt voldaan.

In Azure Policy wordt een aantal ingebouwde beleidsregels geboden die u standaard tot uw beschikking hebt. Bijvoorbeeld:

- **SQL Server 12.0 vereisen**: deze beleidsdefinitie bevat voorwaarden/regels om ervoor te zorgen dat alle SQL-servers gebruikmaken van versie 12.0. Het bijbehorende effect is om alle servers te weigeren die niet voldoen aan deze criteria.
- **SKU’s voor opslagaccount toegestaan**: deze beleidsdefinitie bevat een set voorwaarden/regels die bepalen of een opslagaccount dat wordt geïmplementeerd, zich binnen een set SKU-grootten bevindt. Het bijbehorende effect is om alle opslagaccounts te weigeren die niet in overeenstemming zijn met de gedefinieerde SKU-grootten.
- **Resourcetype toegestaan**: deze beleidsdefinitie bevat een set voorwaarden/regels om de resourcetypen op te geven die kunnen worden geïmplementeerd in uw organisatie. Het bijbehorende effect is om alle resources te weigeren die geen deel uitmaken van deze gedefinieerde lijst.
- **Locaties toegestaan**: op basis van dit beleid kunt u de locaties beperken die uw organisatie kan opgeven tijdens het implementeren van resources. Het bijbehorende effect wordt gebruikt om uw geografisch nalevingsvereisten af te dwingen.
- **SKU’s voor virtuele machines toegestaan**: op basis van dit beleid kunt u een set SKU's voor virtuele machines opgeven die kunnen worden geïmplementeerd in uw organisatie.
- **Label en bijbehorende standaardwaarde toepassen**: op basis van dit beleid wordt een vereist label en de bijbehorende standaardwaarde toegepast, indien dit niet is opgegeven door de gebruiker.
- **Label en bijbehorende waarde afdwingen**: op basis van dit beleid wordt een vereist label en de bijbehorende waarde afgedwongen voor een resource.
- **Resourcetypen niet toegestaan**: op basis van dit beleid kunt u de resourcetypen opgeven die niet kunnen worden geïmplementeerd in uw organisatie.

Als u deze beleidsdefinities (zowel de ingebouwde als de aangepaste) wilt implementeren, dient u ze eerst toe te wijzen. U kunt elk van deze typen beleid toewijzen via Azure Portal, PowerShell of Azure CLI.

Houd er rekening mee dat een herevaluatie van een beleidsregel ongeveer eenmaal per uur plaatsvindt. Dat betekent dat als u wijzigingen aanbrengt aan de beleidsdefinitie nadat de beleidsregel is geïmplementeerd (waardoor een beleidstoewijzing wordt gemaakt), de beleidsdefinitie binnen het uur opnieuw voor uw resources wordt geëvalueerd.

Raadpleeg [Structuur van beleidsdefinities](policy-definition.md) voor meer informatie over de structuur van beleidsdefinities.

## <a name="policy-assignment"></a>Beleidstoewijzing

Een beleidstoewijzing is een beleidsdefinitie die is toegewezen om te worden toegepast binnen een bepaald bereik. Dit bereik kan variëren van een [beheergroep](../azure-resource-manager/management-groups-overview.md) tot een resourcegroep. De term *bereik* verwijst naar alle resourcegroepen, abonnementen of beheergroepen waaraan de beleidsdefinitie is toegewezen. Beleidstoewijzingen worden overgenomen door alle onderliggende resources. Dit betekent dat als beleid wordt toegepast op een resourcegroep, dit beleid ook van toepassing is op alle resources in deze resourcegroep. U kunt echter een subbereik uitsluiten van een beleidstoewijzing.

U kunt op het abonnementsbereik bijvoorbeeld een beleid toepassen op basis waarvan het maken van netwerkresources wordt voorkomen. U sluit echter één resourcegroep binnen het abonnement uit die is bedoeld voor netwerkinfrastructuur. U verleent gebruikers aan wie u het maken van netwerkresources toevertrouwt, toegang tot deze netwerkresourcegroep.

In een ander voorbeeld wilt u mogelijk een whitelistbeleid voor resourcetypen toewijzen op het niveau van de beheergroep. En vervolgens wilt u een ruimer beleid (zodat meer resourcetypen zijn toegestaan) toewijzen via een onderliggende beheergroep of zelfs rechtstreeks in abonnementen. Dit voorbeeld zou echter niet goed werken, omdat beleid een expliciet weigersysteem is. In plaats daarvan moet u de onderliggende beheergroep of het abonnement uitsluiten van de beleidstoewijzing op beheergroepniveau. Vervolgens kunt u het ruimere beleid toewijzen aan het niveau van de onderliggende beheergroep of het abonnement. Samenvattend: als beleidsresultaten in een resource worden geweigerd, vormt het aanpassen van het weigeringsbeleid de enige manier om de resource toe te staan.

Zie [Create a policy assignment to identify non-compliant resources in your Azure environment](assign-policy-definition.md) (Een beleidstoewijzing maken om niet-compatibele resources in uw Azure-abonnement te identificeren) voor meer informatie over het instellen van beleidsdefinities en -toewijzingen.

## <a name="policy-parameters"></a>Beleidsparameters

Beleidsparameters helpen uw beleidsbeheer te vereenvoudigen door het aantal beleidsdefinities te verminderen dat u moet maken. U kunt bij het maken van een beleidsdefinitie parameters definiëren om deze algemener te maken. Vervolgens kunt u deze beleidsdefinitie hergebruiken voor verschillende scenario's. Dit doet u door verschillende waarden door te voeren bij het toepassen van de beleidsdefinitie. Bijvoorbeeld, door een reeks locaties voor een abonnement op te geven.

Parameters worden gedefinieerd/gemaakt bij het maken van een beleidsdefinitie. Als een parameter wordt gedefinieerd, krijgt deze een naam en optioneel een waarde. U kunt bijvoorbeeld een parameter definiëren voor een beleid met de naam *locatie*. Vervolgens kunt u deze verschillende waarden toekennen, zoals *EastUS* of *WestUS* bij het toewijzen van beleid.

Zie [Overzicht van resourcebeleid - parameters](policy-definition.md#parameters) voor meer informatie over beleidsparameters.

## <a name="initiative-definition"></a>Initiatiefdefinitie

Een initiatiefdefinitie is een verzameling beleidsdefinities die zijn aangepast voor het bereiken van één overkoepelend doel. Initiatiefdefinities maken het beheren en toewijzen van beleidsdefinities eenvoudiger. Dit gebeurt door het groeperen van een reeks beleidsregels als één item. U kunt bijvoorbeeld een initiatief maken met de titel **Controleren inschakelen in Azure Security Center** met als doel om alle beschikbare beveiligingsaanbevelingen in uw Azure Security Center te controleren.

Onder dit initiatief vallen beleidsdefinities zoals:

- **Niet-versleutelde SQL-database controleren in Security Center** – Voor het controleren van niet-versleutelde SQL-databases en -servers.
- **OS-kwetsbaarheden controleren in Security Center** – Voor het controleren van servers die niet voldoen aan de geconfigureerde uitgangswaarde.
- **Ontbrekende Endpoint Protection controleren in Security Center** – Voor het controleren van servers zonder een geïnstalleerde Endpoint Protection-agent.

## <a name="initiative-assignment"></a>Initiatieftoewijzing

Net zoals een beleidstoewijzing is een initiatieftoewijzing een initiatiefdefinitie die is toegewezen aan een bepaald bereik. Initiatieftoewijzingen verminderen de noodzaak om verschillende initiatiefdefinities te maken voor elk bereik. Dit bereik kan ook variëren van een beheergroep tot een resourcegroep.

Het initiatief **Controleren inschakelen in Azure Security Center** uit het vorige voorbeeld kan worden toegewezen aan verschillende bereiken. Eén toewijzing kan bijvoorbeeld worden toegewezen aan **subscriptionA**.
Een andere toewijzing kan worden toegewezen aan **subscriptionB**.

## <a name="initiative-parameters"></a>Initiatiefparameters

Net zoals beleidsparameters helpen initiatiefparameters om initiatiefbeheer te vereenvoudigen door redundantie te verminderen. Initiatiefparameters vormen in feite de lijst met parameters die wordt gebruikt voor de beleidsdefinities binnen het initiatief.

Neem bijvoorbeeld een scenario met initiatiefdefinitie **initiativeC**, waarbij voor beleidsdefinities **policyA** en **policyB** elk een ander type parameter wordt verwacht:

| Beleid | Naam van parameter |Type parameter  |Opmerking |
|---|---|---|---|
| policyA | allowedLocations | matrix  |Op basis van deze parameter wordt een lijst met tekenreeksen verwacht voor een waarde, omdat het parametertype is gedefinieerd als een matrix |
| policyB | allowedSingleLocation |tekenreeks |Op basis van deze parameter wordt één woord verwacht voor een waarde, omdat het parametertype is gedefinieerd als een tekenreeks |

In dit scenario hebt u, bij het definiëren van de initiatiefparameters voor **initiativeC**, drie opties:

- Gebruik de parameters van de beleidsdefinities binnen dit initiatief: in dit voorbeeld worden *allowedLocations* en *allowedSingleLocation* initiatiefparameters voor **initiativeC** .
- Geef waarden op voor de parameters van de beleidsdefinities binnen deze initiatiefdefinitie. In dit voorbeeld kunt u een lijst met locaties opgeven naar **parameter van policyA – allowedLocations** en **parameter van policyB – allowedSingleLocation**. U kunt ook waarden opgeven bij het toewijzen van dit initiatief.
- Geef een lijst met *waarde*opties op die kunnen worden gebruikt bij het toewijzen van dit initiatief. Als u dit initiatief toewijst, kunnen de overgenomen parameters van de beleidsdefinities binnen het initiatief, alleen waarden hebben uit de opgegeven lijst.

U kunt bijvoorbeeld een lijst met waardeopties maken in een initiatiefdefinitie met *EastUS*, *WestUS*, *CentralUS* en *WestEurope*. Als u dit doet, kunt u tijdens de initiatieftoewijzing geen andere waarde (bijvoorbeeld *Azië - zuidoost*) invoeren, omdat deze waarde niet voorkomt in de lijst.

## <a name="maximum-count-of-policy-objects"></a>Maximum aantal Policy-objecten

[!INCLUDE [policy-limits](../../includes/azure-policy-limits.md)]

## <a name="recommendations-for-managing-policies"></a>Aanbevelingen voor het beheren van beleid

Hier volgen enkele aanwijzingen die u kunt opvolgen en tips om rekening mee te houden bij het maken en beheren van beleidsdefinities en -toewijzingen:

- Als u beleidsdefinities in uw omgeving maakt, raden we u aan om te beginnen met een controleactie in plaats van een weigeractie. Op deze manier kunt u de impact van de beleidsdefinitie op de resources in uw omgeving bijhouden. Als u al actieve scripts hebt voor het automatisch omhoog schalen van uw toepassingen, kan het instellen van een weigeractie een belemmering vormen voor deze automatische taken die u al hebt geïmplementeerd.
- Het is belangrijk dat u bij het maken van definities en toewijzingen rekening houdt met organisatiehiërarchieën. We raden u aan om definities op een hoger niveau te maken, bijvoorbeeld op het niveau van de beheergroep of het abonnement, en om toewijzingen op het eerstvolgende onderliggende niveau te maken. Als u bijvoorbeeld een beleidsdefinitie op groepsbeheerniveau maakt, kan een beleidstoewijzing van deze definitie een niveau lager plaatsvinden op abonnementsniveau binnen deze beheergroep.
- We raden u aan altijd initiatiefdefinities te gebruiken in plaats van beleidsdefinities, zelfs als u slechts één beleid wilt instellen. Voorbeeld: u hebt een beleidsdefinitie, *policyDefA*, en deze maakt u onder de initiatiefdefinitie *initiativeDefC*. Als u later besluit om nog een beleidsdefinitie voor *policyDefB* te maken met doelen die vergelijkbaar zijn met die van *policyDefA*, kunt u deze toevoegen onder *initiativeDefC*. Op deze manier kunt u ze beter bijhouden.
- Onthoud dat wanneer u eenmaal een initiatieftoewijzing hebt gemaakt van een initiatiefdefinitie, alle nieuwe beleidsdefinities die worden toegevoegd aan deze initiatiefdefinitie, automatisch worden geïmplementeerd onder de initiatieftoewijzing(en) onder deze initiatiefdefinitie.
- Zodra een initiatieftoewijzing wordt geactiveerd, worden alle beleidsregels binnen het initiatief ook geactiveerd. Als u echter een beleidsregel afzonderlijk wilt uitvoeren, is het beter deze regel niet op te nemen in een initiatief.

## <a name="video-overview"></a>Video met overzicht

Het volgende overzicht van Azure Policy is afkomstig van build 2018. Voor het downloaden van dia's of video's, gaat u naar[Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Azure-omgeving beheren via Azure Policy) op kanaal 9.

> [!VIDEO https://channel9.msdn.com/events/Build/2018/THR2030/player]

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht hebt van Azure Policy en enkele van de belangrijkste geïntroduceerde concepten, ziet u hier de voorgestelde volgende stappen:

- [Een beleidsdefinitie toewijzen](assign-policy-definition.md)
- [Een beleidsdefinitie toewijzen met behulp van Azure CLI](assign-policy-definition-cli.md)
- [Een beleidsdefinitie toewijzen met behulp van PowerShell](assign-policy-definition-ps.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../azure-resource-manager/management-groups-overview.md)
- Bekijk [Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Azure-omgeving beheren via Azure Policy) op kanaal 9