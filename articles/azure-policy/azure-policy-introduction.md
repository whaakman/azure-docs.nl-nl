---
title: Overzicht van Azure beleid | Microsoft Docs
description: Beleid voor Azure is een service in Azure, waarmee u kunt maken, toewijzen en, beleidsdefinities in uw Azure-omgeving beheren.
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 11/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: b784c79166eb614988d84b4553bb95e4d1e457af
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="what-is-azure-policy"></a>Wat is Azure beleid?

IT-beheer maakt duidelijkheid tussen zakelijke doelstellingen en IT-projecten. Goede IT-beheer omvat het plannen van uw initiatieven en prioriteiten op strategisch niveau configureren. Uw bedrijf een groot aantal IT-problemen die nooit lijken om op te halen opgelost ervaren? Beleidsregels implementeren, kunt u beter beheren en te voorkomen dat ze. Implementatie van beleid is waar Azure beleid wordt geleverd.

Beleid voor Azure is een service in Azure die u maakt, toewijst en, beleidsdefinities beheren. Beleidsdefinities van het afdwingen verschillende regels en acties via uw resources, zodat deze bronnen voldoen aan uw bedrijfsnormen en de serviceovereenkomsten blijven. Azure-beleid wordt een evaluatie van uw resources, scannen op die niet compatibel zijn met de beleidsdefinities die u hebt uitgevoerd. U kunt bijvoorbeeld een beleid zodat alleen bepaalde typen virtuele machines zijn hebben. Een andere vereist dat alle resources een bepaald label. Deze beleidsregels worden vervolgens geëvalueerd bij het maken en bijwerken van resources.

## <a name="how-is-it-different-from-rbac"></a>Wat is het verschil van RBAC?

Er zijn enkele belangrijke verschillen tussen het beleid en op rollen gebaseerde toegangsbeheer (RABC). RBAC richt zich op de acties van gebruikers op verschillende bereiken. U kan bijvoorbeeld worden toegevoegd aan de rol Inzender voor een resourcegroep op het gewenste bereik. De rol kunt u wijzigingen aanbrengen in die resourcegroep. Beleid is gericht op broneigenschappen tijdens de implementatie en voor het reeds bestaande resources. U kunt bijvoorbeeld via het beleid, de typen bronnen die kunnen worden ingericht beheren. Of u de locaties waar resources kunnen worden ingericht kunt beperken. In tegenstelling tot RBAC, beleid is een standaard toestaan en expliciete system weigeren.

Voor gebruik van beleid, moet u eerst worden geverifieerd via RBAC. In het bijzonder uw account moet het:

- `Microsoft.Authorization/policydefinitions/write`machtiging voor het definiëren van een beleid.
- `Microsoft.Authorization/policyassignments/write`machtiging voor het toewijzen van een beleid.

Deze machtigingen niet zijn opgenomen in de **Inzender** rol.


## <a name="policy-definition"></a>Beleidsdefinitie

De beleidsdefinitie van elk heeft voorwaarden waaronder deze wordt afgedwongen. En er is een bewerking die uitgevoerd wordt als de voorwaarden wordt voldaan.

In Azure-beleid bieden we een aantal ingebouwde beleidsregels die standaard beschikbaar zijn. Bijvoorbeeld:

- **Vereist SQL Server 12.0**: de beleidsdefinitie voor dit heeft voorwaarden/regels om ervoor te zorgen dat alle SQL-servers versie 12.0 gebruiken. De actie is voor het weigeren van alle servers die niet aan deze criteria voldoen.
- **Storage-Account-SKU's toegestaan**: de beleidsdefinitie voor dit heeft een set voorwaarden/regels die bepalen of een opslagaccount die wordt geïmplementeerd in een set van SKU-grootte. De actie is voor het weigeren van alle servers die niet in overeenstemming met de reeks gedefinieerde SKU-grootte.
- **Brontype toegestaan**: de beleidsdefinitie voor dit heeft een set voorwaarden/regels om op te geven van de volgende resourcetypen die uw organisatie kunt implementeren. De actie is voor het weigeren van alle resources die geen deel uitmaken van deze lijst.
- **Toegestane locaties**: dit beleid kunt u de locaties die uw organisatie opgeven kunt bij het implementeren van resources te beperken. De actie wordt gebruikt voor het afdwingen van uw geografisch nalevingsvereisten.
- **Virtuele Machine SKU's toegestaan**: dit beleid kunt u opgeven van een set van virtuele machine SKU's die uw organisatie kunt implementeren.
- **Label en de standaardwaarde toepassen**: dit beleid van toepassing is een vereiste label en de standaardwaarde als dit niet is opgegeven door de gebruiker.
- **Afdwingen van code en de waarde ervan**: dit beleid zorgt ervoor dat een vereiste label en de bijbehorende waarde aan een resource.
- **Niet toegestaan brontypen**: dit beleid kunt u opgeven welke resourcetypen die uw organisatie kan niet worden geïmplementeerd.

U kunt een van deze beleidsregels via de Azure-portal, PowerShell of Azure CLI.

Bekijk voor meer informatie over de structuur van de beleidsdefinities, in dit artikel - [definitie beleidsstructuur](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>Toewijzing van beleid

Een beleidstoewijzing is een beleidsdefinitie die is toegewezen aan plaatsvinden binnen een bepaald bereik. Dit bereik kan variëren van een beheergroep aan een resourcegroep. De term *bereik* verwijst naar alle resourcegroepen, abonnementen of beheergroepen waaraan de beleidsdefinitie is toegewezen. Beleidstoewijzingen worden overgenomen door alle onderliggende resources. Dus als een beleid wordt toegepast op een resourcegroep, deze op alle resources in die resourcegroep toegepast wordt. U kunt echter een subscope uitsluiten van de beleidstoewijzing. U kunt bijvoorbeeld een beleid dat voorkomt het maken van netwerkresources dat toewijzen voor de scope abonnement. Echter, u een resourcegroep in het abonnement dat is bedoeld om de netwerkinfrastructuur uitsluiten. U kunt toegang verlenen aan deze resourcegroep netwerken voor gebruikers die u met het maken van netwerkresources vertrouwt.

Zie voor meer informatie over beleidsdefinities en toewijzingen [een beleidstoewijzing om te identificeren van niet-compatibele bronnen in uw Azure-omgeving maken](assign-policy-definition.md).

## <a name="policy-parameters"></a>Parameters voor beleid

Parameters voor beleid vereenvoudigen doordat het aantal beleidsdefinities die moet u uw beleidsbeheer aan. U kunt de parameters definiëren bij het maken van een beleidsdefinitie zodat het algemene. Vervolgens kunt u beleid gedefinieerd voor verschillende scenario's hergebruiken. U doen door door te geven in verschillende waarden bij het toewijzen van de beleidsdefinitie. Bijvoorbeeld, een reeks locaties voor een abonnement op te geven.

Parameters zijn gedefinieerd/gemaakt bij het maken van een beleidsdefinitie. Als een parameter is gedefinieerd, is het een naam en eventueel een gegeven waarde. U kunt bijvoorbeeld een parameter voor een beleid met de titel definiëren *locatie*. U kunt en geef vervolgens verschillende waarden zoals *EastUS* of *WestUS* bij het toewijzen van een beleid.

<!--
Next link should point to new Concept page for Parameters
-->
Zie voor meer informatie over beleidsparameters [overzicht Resource - Parameters](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Initiatief definitie
De definitie van een initiatief is een verzameling beleidsdefinities die zijn aangepast aan de verwezenlijking van een enkelvoudige algehele doel. Initiatief definities eenvoudiger beheren en het toewijzen van beleidsdefinities. Ze vereenvoudigen door een reeks beleidsregels als één object groeperen. U kunt bijvoorbeeld een initiatief met de titel maken **bewaking inschakelen in Azure Security Center**, met een doel voor het bewaken van de beschikbare beveiligingsaanbevelingen in uw Azure Security Center.

Dit initiatief hebt u beleidsdefinities, zoals:

1. **Monitor voor niet-versleuteld SQL-Database in Security Center** – voor het bewaken van niet-versleutelde SQL-databases en -servers.
2. **OS-beveiligingslekken in Security Center bewaken** – voor het bewaken van servers die niet voldoen aan de geconfigureerde basislijn.
3. **Ontbrekende Endpoint Protection in Security Center controleren** – voor het bewaken van servers zonder een geïnstalleerde endpoint protection-agent.

<!--
For more information about initiative definitions, see Initiative Definitions.+ (instead of linking to this, link out to Concept page on Initiative Definitions)
-->

## <a name="initiative-assignment"></a>Initiatief toewijzing
Een initiatief toewijzing is zoals een beleidstoewijzing een definitie van een initiatief toegewezen aan een bepaald bereik. Initiatief toewijzingen Verminder de noodzaak om diverse initiatief definities voor elke scope. Dit bereik kan ook variëren van een beheergroep aan een resourcegroep.

Uit het vorige voorbeeld de **bewaking inschakelen in Azure Security Center** initiatief kan worden toegewezen aan verschillende bereiken. Bijvoorbeeld, een toewijzing kan worden toegewezen aan **subscriptionA**. Een andere kan worden toegewezen aan **subscriptionB**.

## <a name="initiative-parameters"></a>Initiatief parameters
Zoals beleidsparameters, parameters initiatief initiatief beheer vereenvoudigen doordat redundantie. Initiatief parameters zijn in feite de lijst met parameters wordt gebruikt door de beleidsdefinities binnen het initiatief.

Bijvoorbeeld eens een scenario waarin het hebben van de definitie van een initiatief - **initiativeC**, met twee beleidsdefinities. Elke beleidsdefinitie met één gedefinieerde parameter:

| Beleid | naam van parameter |Type van parameter  |Opmerking |
|---|---|---|---|
| policyA | allowedLocations | matrix  |Deze parameter verwacht een lijst met tekenreeksen voor een waarde, omdat het parametertype is gedefinieerd als een matrix |
| policyB | allowedSingleLocation |Tekenreeks |Deze parameter verwacht een woord naar een waarde, omdat het parametertype is gedefinieerd als een tekenreeks |

In dit scenario wordt bij het definiëren van de initiatief parameters voor **initiativeC**, hebt u drie opties:

1. Gebruik de parameters van de beleidsdefinities binnen dit initiatief: In dit voorbeeld *allowedLocations* en *allowedSingleLocation* worden initiatief parameters voor **initiativeC** .
2. Geef de waarde voor de parameters van de beleidsdefinities binnen deze initiatief definitie. In dit voorbeeld kunt u een lijst van locaties te verstrekken **policyA van de parameter – allowedLocations** en **policyB van de parameter – allowedSingleLocation**.
Als u dit initiatief toewijst, kunt u waarden opgeven.
3. Een lijst verstrekken van *waarde* opties die kunnen worden gebruikt bij het toewijzen van dit initiatief. Als u dit initiatief toewijst, kan de overgenomen parameters van de beleidsdefinities binnen het initiatief alleen waarden uit de opgegeven lijst hebben.

U kunt bijvoorbeeld een lijst met waardeopties maken in de definitie van een initiatief met *EastUS*, *WestUS*, *CentralUS*, en *WestEurope*. Als u, kan niet bent voor het invoeren van een andere waarde, zoals *Zuidoost-Azië* tijdens de toewijzing initiatief, omdat deze geen deel uitmaakt van de lijst.

## <a name="recommendations-for-managing-policies"></a>Aanbevelingen voor het beheren van beleid

Tijdens het maken en beheren van beleidsdefinities en toewijzingen, volgen hier enkele punten die we u adviseren moet volgen:

- Als u beleidsdefinities in uw omgeving maakt, kunt u het beste beginnen met een effect audit in plaats van een effect weigeren voor het bijhouden van de impact van de beleidsdefinitie van de op de resources in uw omgeving. Als u al scripts hebt voor automatisch schalen van uw toepassingen, instellen van een effect weigeren kan een belemmering vormen deze automatische taken die u al geïmplementeerd hebt.
- Het is belangrijk rekening moet houden organisatie hiërarchieën bij het maken van definities en toewijzingen. Het is raadzaam om definities op een hoger niveau, bijvoorbeeld voor de beheergroep of een abonnement maken en toewijzen op de volgende onderliggende niveau. Bijvoorbeeld, als u een beleidsdefinitie op het niveau van de groep management maken, kan een beleidstoewijzing van die definitie worden gericht op naar beneden op het gewenste abonnement binnen deze beheergroep.
- We raden het gebruik van de standard-prijscategorie, om de compatibiliteitsstatus van uw omgeving beter te begrijpen. Voor meer informatie over onze prijscategorie modellen en wat elke van deze aanbieding, Bekijk [prijzen](https://azure.microsoft.com/pricing/details/azure-policy).
- Wordt u aangeraden altijd initiatief definities in plaats van de beleidsdefinities, zelfs als u slechts één beleid in gedachten hebt. Bijvoorbeeld, als er een beleidsdefinitie – *policyDefA* en u dit hebt gemaakt onder het initiatief definitie - *initiativeDefC*, als u besluit te maken van een andere beleidsdefinitie later voor *policyDefB* met doelen is vergelijkbaar met die van *policyDefA*, kunt u het toevoegen onder *initiativeDefC* en deze volgen betere manier.

   Houd er rekening mee dat wanneer u hebt een initiatief toewijzing gemaakt van een initiatief definitie eventuele nieuwe beleidsdefinities automatisch toegevoegd aan de definitie van het initiatief draaien onder het initiatief toewijzing(en) onder die initiatief definitie. Als er een nieuwe parameter in de nieuwe beleidsdefinitie geïntroduceerd, moet u de definitie van het initiatief en toewijzingen bijwerken door de definitie van het initiatief of de toewijzing te bewerken.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt een overzicht van Azure-beleid en enkele van de belangrijkste concepten die we je introductie, vindt hier u de voorgestelde volgende stappen uit:

- [Een beleidsdefinitie toewijzen](./assign-policy-definition.md)
- [De beleidsdefinitie van een met de Azure CLI toewijzen](./assign-policy-definition-cli.md)
- [De beleidsdefinitie van een met behulp van PowerShell toewijzen](./assign-policy-definition-ps.md)
