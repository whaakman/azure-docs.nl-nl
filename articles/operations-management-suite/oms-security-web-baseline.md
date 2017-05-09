---
title: Webbasislijn van de oplossing voor beveiliging en controle voor Operations Management Suite | Microsoft Docs
description: In dit document wordt uitgelegd hoe u de oplossing voor beveiliging en controle voor OMS kunt gebruiken voor het uitvoeren van een evaluatie van de webbasislijn van alle bewaakte webservers, voor nalevings- en beveiligingsdoeleinden.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 0d4707dc0c0ffbf40d0d10a6d12b9709a9655258
ms.contentlocale: nl-nl
ms.lasthandoff: 05/03/2017


---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Evaluatie van de webbasislijn in de oplossing voor beveiliging en controle voor Operations Management Suite
Dit document helpt u om de mogelijkheden van de [oplossing voor beveiliging en controle voor Operations Management Suite](operations-management-suite-overview.md) te gebruiken om evaluaties van de webbasislijn uit te voeren, en zo de beveiligde status van uw bewaakte resources te evalueren.

## <a name="what-is-web-baseline-assessment"></a>Wat is evaluatie van de webbasislijn?
Beveiliging voor OMS biedt momenteel evaluatie van de beveiligingsbasislijn voor besturingssystemen. Het scant de besturingssysteeminstellingen van uw servers elke 24 uur en biedt inzicht in mogelijk kwetsbare instellingen. Lees [Basislijnevaluatie in de oplossing voor beveiliging en controle voor Operations Management Suite](oms-security-baseline.md) voor meer informatie hierover.

Het doel van de evaluatie van de webbasislijn is om mogelijk kwetsbare webserverinstellingen te vinden. De drie primaire bronnen voor de configuraties van de webbasislijn zijn: .NET, ASP.NET en IIS-configuratie.  Net als bij de evaluatie van de besturingssysteembasislijn, scant de beveiliging voor OMS uw webservers elke 24 uur en geeft u inzicht in hun beveiligingsstatus.  In IIS (Internet Information Service) zijn configuraties in hoge mate aanpasbaar, waardoor verschillende niveaus voor sites en toepassingen kunnen worden overschreven. De scanner controleert de instellingen op elk niveau van de toepassing/site en op het standaardhoofdniveau. Zo kunt u de locatie van mogelijk kwetsbare instellingen identificeren en ze snel herstellen.


## <a name="web-security-baseline-assessment"></a>Basislijnevaluatie van de webbeveiliging
In deze preview wordt de functie geopend met de zoekfuntie van OMS. Volg de onderstaande stappen om de toegewezen query uit te voeren:

1. Klik in het hoofddashboard van **Microsoft Operations Management Suite** op de tegel **Beveiliging en controle**.
2. Klik in het dashboard **Beveiliging en controle** op de knop **Zoeken in logboeken**.
3. De eerste query die u kunt gebruiken is de **Web Baseline Assessment Summary** (Overzicht van de webbasislijnevaluatie). Typ in het veld **Beginnen met zoeken** de volgende query: Type*=SecurityBaselineSummary BaselineType=web*. Het volgende scherm heeft een voorbeelduitvoer:

![Overzicht van evaluatie van de webbasislijn](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> In deze query geeft elke record het evaluatieoverzicht op één server weer.

Zodra u **Zoeken in logboeken** hebt geopend, kunt u verschillende query's invoeren om meer informatie over de evaluatie van de webbasislijn te verkrijgen. U kunt naast de voorgaande query ook de volgende query's gebruiken in deze preview.

**Web Baseline Rule Assessment** (Evaluatie van webbasislijnregels): elke record vertegenwoordigt een evaluatie van één webbasislijnregel op één server. Deze bevat alle gegevens voor de regel, locatie, het verwachte resultaat en het werkelijke resultaat.

**Query**: Type*=SecurityBaseline BaselineType=web*

![Evaluatie van webbasislijnregels](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

**Show all results for a specific server** (Alle resultaten voor een specifieke server weergeven): deze query laat zien hoe u de resultaten van een bepaalde server kunt bekijken.

**Query**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*

![Alle resultaten](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

U kunt deze records/query's ook gebruiken om uw eigen dashboards, rapporten en waarschuwingen te maken. Het onderstaande scherm bevat een voorbeeld van een UI-besturingselement die u aan uw dashboard kunt toevoegen. U kunt [hier](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/) leren hoe u uw gegevens visualiseert met behulp van de weergaveontwerper van OMS. Het scherm hieronder bevat een voorbeeld van hoe de tegel eruitziet nadat u deze aanpassing hebt gemaakt.

![Voorbeeld-UI](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> Als u wilt weten welke instellingen worden geselecteerd voor de basislijnevaluatie, kunt u [dit Excel-werkblad](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) met de instellingen downloaden.

## <a name="see-also"></a>Zie ook
In dit document hebt u meer kunnen lezen over het evalueren van de webbasislijn met behulp van de oplossing voor beveiliging en controle voor OMS. Raadpleeg de volgende artikelen voor meer informatie over OMS Beveiliging:

* [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren](oms-security-responding-alerts.md)
* [Resources bewaken in de oplossing Beveiliging en controle van Operations Management Suite ](oms-security-monitoring-resources.md)


