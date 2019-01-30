---
title: Azure Stack-validatie als een Service-belangrijkste concepten | Microsoft Docs
description: Beschrijft de belangrijkste concepten in Azure Stack-validatie als een Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 01/07/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 1f5c47dd3453c0c8f02f1b0a87e5f2fff123f8be
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242804"
---
# <a name="validation-as-a-service-key-concepts"></a>Validatie uit als een Service-belangrijkste concepten

Dit artikel beschrijft de belangrijkste concepten in de validatie als een Service (VaaS).

## <a name="solutions"></a>Oplossingen

Een oplossing VaaS vertegenwoordigt een Azure Stack-oplossing met een bepaalde hardware stuklijst (BoM). De oplossing VaaS fungeert als een container voor de werkstromen die worden uitgevoerd op basis van de Azure Stack-oplossing.

### <a name="create-a-solution-in-the-vaas-portal"></a>Een oplossing maken in de portal VaaS

1. Aanmelden bij de [VaaS portal](https://azurestackvalidation.com).
2. Selecteer op het dashboard oplossingen op **nieuwe oplossing**.
3. Voer een naam voor de oplossing. Zie voor het benoemen van suggesties, [naamgevingsconventie voor VaaS oplossingen](azure-stack-vaas-best-practice.md#naming-convention-for-vaas-solutions).
4. Selecteer **opslaan** om de oplossing te maken.

## <a name="workflows"></a>Workflows

Een werkstroom VaaS werkt binnen de context van een VaaS-oplossing. Hiermee geeft u een verzameling testsuites die de functionaliteit van een Azure Stack-implementatie. Een werkstroom moet worden gemaakt voor elke implementatie of software-update van een Azure Stack-oplossing.

Werkstromen worden gecategoriseerd op scenariotype kunt testen. Niet-officiële tests, de **testronde** werkstroom kunt u de tests uit alle beschikbare VaaS kunnen selecteren. In het officiële testen, het **validatie** werkstromen gericht op specifieke Testscenario's door Microsoft is geselecteerd.

![VaaS werkstroom tegels](media/tile_all-workflows.png)

> [!NOTE]
> De **oplossing validatie** werkstroom ondersteunt momenteel twee scenario's: [OEM-pakketten valideren](azure-stack-vaas-validate-oem-package.md) en [valideren van software-updates van Microsoft](azure-stack-vaas-validate-microsoft-updates.md).

Zie voor meer informatie over werkstroomtypen [wat wordt gevalideerd als een Service voor Azure Stack?](azure-stack-vaas-overview.md).

### <a name="getting-started-with-vaas-workflows"></a>Aan de slag met VaaS werkstromen

1. Een nieuwe oplossing maken op het dashboard oplossingen, of Selecteer een bestaande resourcegroep. Dit wordt vernieuwd en u kunt de tegels van de werkstroom.
2. Als u wilt een nieuwe werkstroom maakt, selecteert u op **Start** op een willekeurige tegel. Zie de volgende artikelen voor informatie die specifiek zijn voor elke werkstroom:
    - Testronde: [Snelstart: De validatie gebruiken als een serviceportal voor het plannen van uw eerste test](azure-stack-vaas-schedule-test-pass.md)
    - Validatie van de oplossing: [Een nieuwe Azure Stack-oplossing te valideren](azure-stack-vaas-validate-solution-new.md)
    - Validatie van de oplossing: [Valideren van software-updates van Microsoft](azure-stack-vaas-validate-microsoft-updates.md)
    - Validatie van de oplossing: [OEM-pakketten valideren](azure-stack-vaas-validate-oem-package.md)

3. Als u wilt beheren of bewaken van een bestaande werkstroom, selecteert u op **beheren** op de tegel van de werkstroom. Selecteer de naam van de werkstroom en het gebruik de **bewerken** knop Eigenschappen weergeven of wijzigen van algemene testparameters.

Zie voor meer informatie over de werkstroomeigenschappen van de en parameters [werkstroom algemene parameters voor Azure Stack-validatie als een Service](azure-stack-vaas-parameters.md).

## <a name="tests"></a>Tests

Een test in VaaS bestaat uit een reeks acties worden uitgevoerd voor een Azure Stack-oplossing. Tests hebben verschillende beoogde doeleinden die zijn geïdentificeerd door een categorie, zoals functionele of de betrouwbaarheid, en een of meer services van Azure Stack als doel. Elke test definieert een eigen set parameters, waarvan een aantal door de algemene parameters van de werkstroom die zijn opgegeven.

Zie voor meer informatie over het beheren en controleren van tests, [bewaken en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md).

Zie voor meer informatie over testparameters [werkstroom algemene parameters voor Azure Stack-validatie als een Service](azure-stack-vaas-parameters.md).

## <a name="agents"></a>Agents

Een agent VaaS stations testuitvoering. Twee typen agents uitvoeren VaaS tests:

- De **cloudagent**. Dit is de standaard-agent die beschikbaar zijn in VaaS. Geen installatie vereist is, is, maar dit is vereist in afhankelijk van de verbinding met uw omgeving en Azure Stack-eindpunten moeten kunnen worden omgezet via internet. Sommige tests zijn niet compatibel met de cloudagent.
- Een **lokale agent**. Hiermee kunt u voor het uitvoeren van validatie in scenario's waarin in afhankelijk van de verbinding met uw omgeving niet haalbaar is. Enkele tests uit vereist via de lokale agent.

Lokale agenten zijn niet gekoppeld aan een bepaald Azure Stack of VaaS-oplossing. Als een best practice, moeten ze worden uitgevoerd buiten een Azure Stack-omgeving.

Zie voor instructies over het toevoegen van een lokale agent [implementeren van de lokale agent](azure-stack-vaas-local-agent.md).

## <a name="next-steps"></a>Volgende stappen

- [Aanbevolen procedures voor validatie als een Service](azure-stack-vaas-best-practice.md)