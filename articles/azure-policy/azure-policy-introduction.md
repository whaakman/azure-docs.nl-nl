---
title: Overzicht van Azure Policy | Microsoft Docs
description: Azure Policy is een service in Azure die u gebruikt om beleidsdefinities in uw Azure-omgeving te maken, toe te wijzen en te beheren.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders; nini
ms.date: 01/17/2018
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 37fdb8156e4507046a574ba681e2ea3f6909c4a6
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="what-is-azure-policy"></a>Wat is Azure Policy?

IT-beheer zorgt voor duidelijkheid tussen zakelijke doelstellingen en IT-projecten. Goed IT-beheer omvat het plannen van uw initiatieven en het stellen van prioriteiten op strategisch niveau. Ondervindt uw bedrijf een significant aantal IT-problemen die nooit lijken te worden opgelost? Door beleidsregels te implementeren kunt u deze problemen beter beheren en voorkomen. Voor het implementeren van beleidsregels gebruikt u Azure Policy.

Azure Policy is een service in Azure die u gebruikt om beleidsdefinities te maken, toe te wijzen en te beheren. Met beleidsdefinities worden verschillende regels en acties afgedwongen voor uw resources, zodat deze resources voldoen aan de standaarden en SLA’s (Service Level Agreement) in uw bedrijf. Er wordt met Azure Policy een evaluatie uitgevoerd voor de resources, waarbij wordt gezocht naar resources die niet voldoen aan uw beleidsdefinities. U kunt bijvoorbeeld beleid hebben op basis waarvan alleen bepaalde typen virtuele machines zijn toegestaan. Terwijl op basis van ander beleid is vereist dat alle resources een bepaald label hebben. Deze beleidsregels worden vervolgens geëvalueerd bij het maken en bijwerken van resources.

## <a name="how-is-it-different-from-rbac"></a>Wat is het verschil met RBAC?

Er zijn enkele belangrijke verschillen tussen beleid en RBAC (op rollen gebaseerd toegangsbeheer). RBAC is gericht op de acties van gebruikers in verschillende bereiken. U kunt bijvoorbeeld zijn toegevoegd aan de rol Inzender voor een resourcegroep in het gewenste bereik. Met deze rol kunt u wijzigingen aanbrengen in deze resourcegroep. Beleid is gericht op resource-eigenschappen tijdens de implementatie en voor al bestaande resources. Via beleidsregels kunt u bijvoorbeeld de typen resources beheren die kunnen worden ingericht. Of u kunt het aantal locaties beperken waarop resources kunnen worden ingericht. In tegenstelling tot RBAC is beleid een standaardsysteem voor toestaan en expliciet weigeren.

Als u beleid wilt gebruiken, moet u eerst worden geverifieerd via RBAC. Uw account heeft specifiek het volgende nodig:

- `Microsoft.Authorization/policydefinitions/write`-machtiging voor het definiëren van beleid.
- `Microsoft.Authorization/policyassignments/write`-machtiging voor het toepassen van beleid.

Deze machtigingen zijn niet opgenomen in de rol **Inzender**.


## <a name="policy-definition"></a>Beleidsdefinitie

Elke beleidsdefinitie heeft voorwaarden voor het afdwingen ervan. En er is een bijbehorende actie die wordt uitgevoerd, als aan de voorwaarden wordt voldaan.

In Azure Policy wordt een aantal ingebouwde beleidsregels geboden die u standaard tot uw beschikking hebt. Bijvoorbeeld:

- **SQL Server 12.0 vereisen**: deze beleidsdefinitie bevat voorwaarden/regels om ervoor te zorgen dat alle SQL-servers gebruikmaken van versie 12.0. De bijbehorende actie is om alle servers te weigeren die niet voldoen aan deze criteria.
- **SKU’s voor opslagaccount toegestaan**: deze beleidsdefinitie bevat een set voorwaarden/regels die bepalen of een opslagaccount dat wordt geïmplementeerd, zich binnen een set SKU-grootten bevindt. De bijbehorende actie is om alle servers te weigeren die niet in overeenstemming zijn met de gedefinieerde SKU-grootten.
- **Resourcetype toegestaan**: deze beleidsdefinitie bevat een set voorwaarden/regels om de resourcetypen op te geven die kunnen worden geïmplementeerd in uw organisatie. De bijbehorende actie is om alle resources te weigeren die geen deel uitmaken van deze gedefinieerde lijst.
- **Locaties toegestaan**: op basis van dit beleid kunt u de locaties beperken die uw organisatie kan opgeven tijdens het implementeren van resources. De bijbehorende actie wordt gebruikt om uw geografisch nalevingsvereisten af te dwingen.
- **SKU’s voor virtuele machines toegestaan**: op basis van dit beleid kunt u een set SKU's voor virtuele machines opgeven die kunnen worden geïmplementeerd in uw organisatie.
- **Label en bijbehorende standaardwaarde toepassen**: op basis van dit beleid wordt een vereist label en de bijbehorende standaardwaarde toegepast, indien dit niet is opgegeven door de gebruiker.
- **Label en bijbehorende waarde afdwingen**: op basis van dit beleid wordt een vereist label en de bijbehorende waarde afgedwongen voor een resource.
- **Resourcetypen niet toegestaan**: op basis van dit beleid kunt u de resourcetypen opgeven die niet kunnen worden geïmplementeerd in uw organisatie.

U kunt elk van deze typen beleid toewijzen via Azure Portal, PowerShell of Azure CLI.

Raadpleeg het artikel [Structuur van beleidsdefinities](policy-definition.md) voor meer informatie over de structuur van beleidsdefinities.

## <a name="policy-assignment"></a>Beleidstoewijzing

Een beleidstoewijzing is een beleidsdefinitie die is toegewezen om te worden toegepast binnen een bepaald bereik. Dit bereik kan variëren van een beheergroep tot een resourcegroep. De term *bereik* verwijst naar alle resourcegroepen, abonnementen of beheergroepen waaraan de beleidsdefinitie is toegewezen. Beleidstoewijzingen worden overgenomen door alle onderliggende resources. Dus als beleid wordt toegepast op een resourcegroep, is dit beleid ook van toepassing op alle resources in deze resourcegroep. U kunt echter een subbereik uitsluiten van een beleidstoewijzing. U kunt op het abonnementsbereik bijvoorbeeld een beleid toepassen op basis waarvan het maken van netwerkresources wordt voorkomen. U sluit echter één resourcegroep binnen het abonnement uit die is bedoeld voor netwerkinfrastructuur. U verleent gebruikers aan wie u het maken van netwerkresources toevertrouwt, toegang tot deze netwerkresourcegroep.

Zie [Create a policy assignment to identify non-compliant resources in your Azure environment](assign-policy-definition.md) (Een beleidstoewijzing maken om niet-compatibele resources in uw Azure-abonnement te identificeren) voor meer informatie over het instellen van beleidsdefinities en -toewijzingen.

## <a name="policy-parameters"></a>Beleidsparameters

Beleidsparameters helpen uw beleidsbeheer te vereenvoudigen door het aantal beleidsdefinities te verminderen dat u moet maken. U kunt bij het maken van een beleidsdefinitie parameters definiëren om deze algemener te maken. Vervolgens kunt u deze beleidsdefinitie hergebruiken voor verschillende scenario's. Dit doet u door verschillende waarden door te voeren bij het toepassen van de beleidsdefinitie. Bijvoorbeeld, door een reeks locaties voor een abonnement op te geven.

Parameters worden gedefinieerd/gemaakt bij het maken van een beleidsdefinitie. Als een parameter wordt gedefinieerd, krijgt deze een naam en optioneel een waarde. U kunt bijvoorbeeld een parameter definiëren voor een beleid met de naam *locatie*. Vervolgens kunt u deze verschillende waarden toekennen, zoals *EastUS* of *WestUS* bij het toewijzen van beleid.

<!--
Next link should point to new Concept page for Parameters
-->
Zie [Overzicht van resourcebeleid - parameters](policy-definition.md#parameters) voor meer informatie over beleidsparameters.

## <a name="initiative-definition"></a>Initiatiefdefinitie
Een initiatiefdefinitie is een verzameling beleidsdefinities die zijn aangepast voor het bereiken van één overkoepelend doel. Initiatiefdefinities maken het beheren en toewijzen van beleidsdefinities eenvoudiger. Dit gebeurt door het groeperen van een reeks beleidsregels als één item. U kunt bijvoorbeeld een initiatief maken met de titel **Controleren inschakelen in Azure Security Center** met als doel om alle beschikbare beveiligingsaanbevelingen in uw Azure Security Center te controleren.

Onder dit initiatief vallen beleidsdefinities zoals:

1. **Niet-versleutelde SQL-database controleren in Security Center** – Voor het controleren van niet-versleutelde SQL-databases en -servers.
2. **OS-kwetsbaarheden controleren in Security Center** – Voor het controleren van servers die niet voldoen aan de geconfigureerde uitgangswaarde.
3. **Ontbrekende Endpoint Protection controleren in Security Center** – Voor het controleren van servers zonder een geïnstalleerde Endpoint Protection-agent.

<!--
For more information about initiative definitions, see Initiative Definitions.+ (instead of linking to this, link out to Concept page on Initiative Definitions)
-->

## <a name="initiative-assignment"></a>Initiatieftoewijzing
Net zoals een beleidstoewijzing is een initiatieftoewijzing een initiatiefdefinitie die is toegewezen aan een bepaald bereik. Initiatieftoewijzingen verminderen de noodzaak om verschillende initiatiefdefinities te maken voor elk bereik. Dit bereik kan ook variëren van een beheergroep tot een resourcegroep.

Het initiatief **Controleren inschakelen in Azure Security Center** uit het vorige voorbeeld kan worden toegewezen aan verschillende bereiken. Eén toewijzing kan bijvoorbeeld worden toegewezen aan **subscriptionA**. Een andere toewijzing kan worden toegewezen aan **subscriptionB**.

## <a name="initiative-parameters"></a>Initiatiefparameters
Net zoals beleidsparameters helpen initiatiefparameters om initiatiefbeheer te vereenvoudigen door redundantie te verminderen. Initiatiefparameters vormen in feite de lijst met parameters die wordt gebruikt voor de beleidsdefinities binnen het initiatief.

Neem bijvoorbeeld een scenario waarin u een initiatiefdefinitie **initiativeC** hebt, met twee beleidsdefinities. Elke beleidsdefinitie heeft één gedefinieerde parameter:

| Beleid | naam van parameter |Type parameter  |Opmerking |
|---|---|---|---|
| policyA | allowedLocations | matrix  |Op basis van deze parameter wordt een lijst met tekenreeksen verwacht voor een waarde, omdat het parametertype is gedefinieerd als een matrix |
| policyB | allowedSingleLocation |tekenreeks |Op basis van deze parameter wordt één woord verwacht voor een waarde, omdat het parametertype is gedefinieerd als een tekenreeks |

In dit scenario hebt u, bij het definiëren van de initiatiefparameters voor **initiativeC**, drie opties:

1. Gebruik de parameters van de beleidsdefinities binnen dit initiatief: in dit voorbeeld worden *allowedLocations* en *allowedSingleLocation* initiatiefparameters voor **initiativeC** .
2. Geef waarden op voor de parameters van de beleidsdefinities binnen deze initiatiefdefinitie. In dit voorbeeld kunt u een lijst met locaties opgeven naar **parameter van policyA – allowedLocations** en **parameter van policyB – allowedSingleLocation**.
U kunt ook waarden opgeven bij het toewijzen van dit initiatief.
3. Geef een lijst met *waarde*opties op die kunnen worden gebruikt bij het toewijzen van dit initiatief. Als u dit initiatief toewijst, kunnen de overgenomen parameters van de beleidsdefinities binnen het initiatief, alleen waarden hebben uit de opgegeven lijst.

U kunt bijvoorbeeld een lijst met waardeopties maken in een initiatiefdefinitie met *EastUS*, *WestUS*, *CentralUS* en *WestEurope*. Als u dit doet, kunt u tijdens de initiatieftoewijzing geen andere waarde (bijvoorbeeld *Southeast Asia*) invoeren, omdat deze waarde niet voorkomt in de lijst.

## <a name="recommendations-for-managing-policies"></a>Aanbevelingen voor het beheren van beleid

Hier volgen enkele aanwijzingen die u kunt opvolgen tijdens het maken en beheren van beleidsdefinities en -toewijzingen:

- Als u beleidsdefinities in uw omgeving maakt, raden we u aan om te beginnen met een controleactie in plaats van een weigeractie. Op deze manier kunt u de impact van de beleidsdefinitie op de resources in uw omgeving bijhouden. Als u al actieve scripts hebt voor het automatisch omhoog schalen van uw toepassingen, kan het instellen van een weigeractie een belemmering vormen voor deze automatische taken die u al hebt geïmplementeerd.
- Het is belangrijk dat u bij het maken van definities en toewijzingen rekening houdt met organisatiehiërarchieën. We raden u aan om definities op een hoger niveau te maken, bijvoorbeeld op het niveau van de beheergroep of het abonnement, en om toewijzingen op het eerstvolgende onderliggende niveau te maken. Als u bijvoorbeeld een beleidsdefinitie op groepsbeheerniveau maakt, kan een beleidstoewijzing van deze definitie een niveau lager plaatsvinden op abonnementsniveau binnen deze beheergroep.
- Het gebruik van de standaardprijscategorie wordt aanbevolen voor een beter begrip van de nalevingsstatus van uw omgeving. Bekijk de [prijzen](https://azure.microsoft.com/pricing/details/azure-policy) voor meer informatie over onze prijsmodellen en wat elk model te bieden heeft.
- We raden u aan altijd initiatiefdefinities te gebruiken in plaats van beleidsdefinities, zelfs als u slechts één beleid wilt instellen. Voorbeeld: u hebt een beleidsdefinitie, *policyDefA*, en deze maakt u onder de initiatiefdefinitie *initiativeDefC*. Als u later besluit om nog een beleidsdefinitie voor *policyDefB* te maken met doelen die vergelijkbaar zijn met die van *policyDefA*, kunt u deze toevoegen onder *initiativeDefC*. Op deze manier kunt u ze beter bijhouden.

   Onthoud dat wanneer u eenmaal een initiatieftoewijzing hebt gemaakt van een initiatiefdefinitie, alle nieuwe beleidsdefinities die worden toegevoegd aan deze initiatiefdefinitie, automatisch worden geïmplementeerd onder de initiatieftoewijzing(en) onder deze initiatiefdefinitie. Als er echter een nieuwe parameter in de nieuwe beleidsdefinitie wordt geïntroduceerd, moet u de initiatiefdefinitie en -toewijzingen bijwerken door de initiatiefdefinitie of -toewijzing te bewerken.

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht hebt van Azure Policy en enkele van de belangrijkste geïntroduceerde concepten, zijn dit de voorgestelde volgende stappen:

- [Een beleidsdefinitie toewijzen](./assign-policy-definition.md)
- [Een beleidsdefinitie toewijzen met behulp van Azure CLI](./assign-policy-definition-cli.md)
- [Een beleidsdefinitie toewijzen met behulp van PowerShell](./assign-policy-definition-ps.md)
