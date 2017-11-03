---
title: Overzicht van Azure beleid | Microsoft Docs
description: Beleid voor Azure is een service in Azure, waarmee u kunt maken, toewijzen en, beleidsdefinities in uw Azure-omgeving beheren.
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 10/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: 01b0915cdf37ddc00a4e6a38c99ce7f6f8c4f9c9
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-policy"></a>Wat is Azure beleid?

IT-beheer maakt duidelijkheid tussen zakelijke doelstellingen en IT-projecten. Goede IT-beheer omvat het plannen van uw initiatieven en prioriteiten op strategisch niveau configureren. Als uw bedrijf optreedt in een groot aantal IT-problemen die nooit lijken te verkrijgen opgelost, implementatie van beleid kunt u beter beheren en te voorkomen dat ze. Dit is waar Azure beleid wordt geleverd.

Beleid voor Azure is een service in Azure die u maakt, toewijst en, beleidsdefinities beheren. Beleidsdefinities van het afdwingen verschillende regels en acties via uw resources, zodat deze bronnen voldoen aan uw bedrijfsnormen en de serviceovereenkomsten blijven. Dit gebeurt door het uitvoeren van een evaluatie van uw resources, om te scannen waarvoor die zijn niet compatibel zijn met de beleidsdefinities aanwezig is.

## <a name="policy-definition"></a>Beleidsdefinitie

De beleidsdefinitie van elk heeft voorwaarden waaronder deze wordt afgedwongen en een bijbehorende actie onderneemt plaatsen als de voorwaarden wordt voldaan.

In Azure-beleid bieden we een aantal ingebouwde beleidsregels die standaard beschikbaar zijn. Bijvoorbeeld:

- **Vereist SQL Server 12.0**: de beleidsdefinitie voor dit heeft voorwaarden/regels om ervoor te zorgen dat alle SQL-servers versie 12.0 gebruiken. De actie is voor het weigeren van alle servers die niet aan deze criteria voldoen.
- **Storage-Account-SKU's toegestaan**: de beleidsdefinitie voor dit heeft een set voorwaarden/regels die bepalen of een opslagaccount die wordt geïmplementeerd in een set van SKU-grootte. De actie is voor het weigeren van alle servers die niet in overeenstemming met de reeks gedefinieerde SKU-grootte.
- **Brontype toegestaan**: de beleidsdefinitie voor dit heeft een set voorwaarden/regels om op te geven van de volgende resourcetypen die uw organisatie kunt implementeren. De actie is voor het weigeren van alle resources die geen deel uitmaken van deze lijst.

Bekijk voor meer informatie over de structuur van de beleidsdefinities, in dit artikel - [definitie beleidsstructuur](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>Toewijzing van beleid
Een beleidstoewijzing is een beleidsdefinitie die is toegewezen aan plaatsvinden binnen een bepaald bereik. Dit bereik kan variëren van een beheergroep aan een resourcegroep.

Zie voor meer informatie over beleidsdefinities en toewijzingen [Resource overzicht](../azure-resource-manager/resource-manager-policy.md).

## <a name="policy-parameters"></a>Parameters voor beleid
Parameters voor beleid vereenvoudigen doordat het aantal beleidsdefinities die moet u uw beleidsbeheer aan. U kunt de parameters definiëren bij het maken van een beleidsdefinitie zodat het algemene. U kunt gebruiken als beleid gedefinieerd voor verschillende scenario's door door te geven in verschillende waarden (zoals het opgeven van een reeks locaties voor een abonnement) wanneer het toewijzen van beleid.

Parameters zijn gedefinieerd/gemaakt bij het maken van een beleidsdefinitie. Als een parameter is gedefinieerd, is het een naam en eventueel een gegeven waarde. U kan bijvoorbeeld een parameter voor een beleid definiëren als een locatie en wijs hieraan de verschillende waarden zoals *EastUS* of *WestUS* bij het toewijzen van een beleid.

Zie voor meer informatie over beleidsparameters [overzicht Resource - Parameters](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Initiatief definitie
De definitie van een initiatief is een verzameling beleidsdefinities die zijn aangepast aan de verwezenlijking van een enkelvoudige algehele doel. U kunt bijvoorbeeld een initiatief met de titel maken **bewaking inschakelen in Azure Security Center**, met een doel voor het bewaken van de beschikbare beveiligingsaanbevelingen in uw Azure Security Center.

Dit initiatief hebt u beleidsdefinities, zoals:

1. **Monitor voor niet-versleuteld SQL-Database in Security Center** – voor het bewaken van niet-versleutelde SQL-databases en -servers.
2. **OS-beveiligingslekken in Security Center bewaken** – voor het bewaken van servers die niet voldoen aan de geconfigureerde basislijn.
3. **Ontbrekende Endpoint Protection in Security Center controleren** – voor het bewaken van servers zonder een geïnstalleerde endpoint protection-agent.

## <a name="initiative-assignment"></a>Initiatief toewijzing
Een initiatief toewijzing is zoals een beleidstoewijzing een definitie van een initiatief toegewezen aan een bepaald bereik. Initiatief toewijzingen Verminder de noodzaak om diverse initiatief definities voor elke scope. Dit bereik kan ook variëren van een beheergroep aan een resourcegroep.

Uit het vorige voorbeeld de **bewaking inschakelen in Azure Security Center** initiatief kan worden toegewezen aan verschillende bereiken. Bijvoorbeeld, een toewijzing kan worden toegewezen aan **subscriptionA**, terwijl andere kan worden toegewezen aan **subscriptionB**.

## <a name="initiative-parameters"></a>Initiatief parameters
Zoals beleidsparameters, parameters initiatief initiatief beheer vereenvoudigen doordat redundantie. Initiatief parameters zijn in feite de lijst met parameters wordt gebruikt door de beleidsdefinities binnen het initiatief.

Bijvoorbeeld eens een scenario waarin het hebben van de definitie van een initiatief - **initiativeC**, met twee beleidsdefinities. Elke beleidsdefinitie met één gedefinieerde parameter:

|Beleid  |naam van parameter     |Type van parameter  |Opmerking                                                                                                |
|--------|----------------------|-------|----------------------------------------------------------------------------------------------------------------|
|policyA |allowedLocations      |matrix  |Deze parameter verwacht een lijst met tekenreeksen voor een waarde, omdat het parametertype is gedefinieerd als een matrix |
|policyB |allowedSingleLocation |Tekenreeks |Deze parameter verwacht een woord naar een waarde, omdat het parametertype is gedefinieerd als een tekenreeks          |

In dit scenario wordt bij het definiëren van de initiatief parameters voor **initiativeC**, hebt u drie opties:

1. Gebruikmaken van de parameters van de beleidsdefinities binnen dit initiatief: In dit voorbeeld *allowedLocations* en *allowedSingleLocation* worden initiatief parameters voor  **initiativeC**.
2. Geef de waarde voor de parameters van de beleidsdefinities binnen deze initiatief definitie. In dit voorbeeld kunt u een lijst van locaties te verstrekken **policyA van de parameter – allowedLocations** en **policyB van de parameter – allowedSingleLocation**.
Als u dit initiatief toewijst, kunt u waarden opgeven.
3. Een lijst verstrekken van *waarde* opties die kunnen worden gebruikt bij het toewijzen van dit initiatief. Dit betekent dat als u dit initiatief toewijst, de overgenomen parameters van de beleidsdefinities binnen het initiatief kunnen alleen waarden van deze lijst opgegeven.

Als de opgegeven lijst met waarde opties bij het maken van de definitie van het initiatief, heeft bijvoorbeeld – *EastUS*, *WestUS*, *CentralUS*, en *WestEurope* , kunt u zich niet voor het invoeren van een andere waarde, zoals *Zuidoost-Azië* tijdens de toewijzing initiatief, omdat deze geen deel uitmaakt van de lijst.

## <a name="recommendations-for-managing-policies"></a>Aanbevelingen voor het beheren van beleid

Tijdens het maken en beheren van beleidsdefinities en toewijzingen, volgen hier enkele punten die we u adviseren moet volgen:

- Als u beleidsdefinities in uw omgeving maakt, kunt u het beste beginnen met een track audit-effect, in plaats van een effect weigeren te houden de impact van de beleidsdefinitie in uw omgeving. Als u al scripts hebt voor automatisch schalen van uw toepassingen, instellen van een effect weigeren kan een belemmering vormen deze automatische taken die u al geïmplementeerd hebt.
- Het is belangrijk rekening moet houden organisatie hiërarchieën bij het maken van definities en toewijzingen. Het is raadzaam om definities op een hoger niveau, bijvoorbeeld voor de beheergroep of een abonnement maken en toewijzen op de volgende onderliggende niveau. Bijvoorbeeld, als u een beleidsdefinitie op het niveau van de groep management maken, kan een beleidstoewijzing van die definitie worden gericht op naar beneden op abonnementsniveau.
- We raden het gebruik van de standard-prijscategorie, om de compatibiliteitsstatus van uw omgeving beter te begrijpen.
- Wordt u aangeraden altijd initiatief definities in plaats van de beleidsdefinities, zelfs als u slechts één beleid in gedachten hebt. Bijvoorbeeld, als er een beleidsdefinitie – *policyDefA* en u dit hebt gemaakt onder het initiatief definitie - *initiativeDefC*, als u besluit te maken van een andere beleidsdefinitie met doelstellingen zoals die van *policyDefA*, kunt u gewoon het toevoegen onder *initiativeDefC* en deze volgen betere manier.

   Houd er rekening mee dat wanneer u hebt een initiatief toewijzing gemaakt van een initiatief definitie eventuele nieuwe beleidsdefinities wordt toegevoegd aan de definitie van het initiatief automatisch onder het initiatief toewijzing(en) onder die initiatief definitie draait. Als er een nieuwe parameter in de nieuwe beleidsdefinitie geïntroduceerd, moet u echter bijwerken van de definitie van het initiatief en toewijzingen in overeenstemming met dit door de definitie of de toewijzing te bewerken.

## <a name="next-steps"></a>Volgende stappen:
Nu dat u hebt een overzicht van Azure-beleid en enkele van de belangrijkste concepten die we je introductie, vindt hier u de voorgestelde volgende stappen uit:

- [Een beleidsdefinitie toewijzen](./assign-policy-definition.md)
- [De beleidsdefinitie van een met de Azure CLI toewijzen](./assign-policy-definition-cli.md)
- [De beleidsdefinitie van een met behulp van PowerShell toewijzen](./assign-policy-definition-ps.md)
