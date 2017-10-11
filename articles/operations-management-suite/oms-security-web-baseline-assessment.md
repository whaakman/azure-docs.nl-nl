---
title: Evaluatie van de webbasislijn in de oplossing voor beveiliging en controle voor Operations Management Suite | Microsoft Docs
description: In dit document wordt uitgelegd hoe u evaluatie van de webbasislijn kunt gebruiken in de oplossing voor beveiliging en controle voor OMS voor het uitvoeren van een evaluatie van de basislijn van alle bewaakte webservers, voor nalevings- en beveiligingsdoeleinden.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.openlocfilehash: 40b0c6ca933ea02ac9f5fe3bfaaf87a310542a8d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Evaluatie van de webbasislijn in de oplossing voor beveiliging en controle voor Operations Management Suite
Dit document helpt u om de mogelijkheden van de oplossing voor beveiliging en controle voor OMS te gebruiken om evaluaties van de webbasislijn uit te voeren, en zo de beveiligde status van uw bewaakte resources te evalueren.

## <a name="what-is-web-baseline-assessment"></a>Wat is evaluatie van de webbasislijn?
Beveiliging voor OMS biedt momenteel evaluatie van de beveiligingsbasislijn voor besturingssystemen. Het scant de besturingssysteeminstellingen van uw servers elke 24 uur en biedt inzicht in mogelijk kwetsbare instellingen. Lees [Basislijnevaluatie in de oplossing voor beveiliging en controle voor Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/oms-security-baseline) voor meer informatie hierover.

Het doel van de evaluatie van de webbasislijn is om mogelijk kwetsbare webserverinstellingen te vinden. De drie primaire bronnen voor de configuraties van de webbasislijn zijn: .NET, ASP.NET en IIS-configuratie.  Net als bij de evaluatie van de besturingssysteembasislijn, scant de beveiliging voor OMS uw webservers elke 24 uur en geeft u inzicht in hun beveiligingsstatus.  In IIS (Internet Information Service) zijn configuraties in hoge mate aanpasbaar, waardoor verschillende niveaus voor sites en toepassingen kunnen worden overschreven. De scanner controleert de instellingen op elk niveau van de toepassing/site en op het standaardhoofdniveau. Dit helpt u om te bepalen welke instellingen mogelijk kwetsbaar zijn en dit snel te herstellen, samen met onze aanbevelingen voor deze instellingen.

>[!NOTE] 
>Ga naar deze [pagina](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335?redir=0) om de Common Configuration Identifiers en Baseline Rules te downloaden die worden gebruikt door OMS Security.


## <a name="web-security-baseline-assessment"></a>Basislijnevaluatie van de webbeveiliging

Voor dit voorbeeld is de functie toegankelijk via de zoekfunctie van OMS en het OMS-dashboard Beveiliging en controle. Volg de onderstaande stappen om de toegewezen query uit te voeren:

1. Klik in het hoofddashboard van **Microsoft Operations Management Suite** op de tegel **Beveiliging en controle**.
2. In het dashboard **Beveiliging en controle** ziet u het perspectief van de webbasislijn naast het perspectief van de basislijn van het besturingssysteem.
   
    ![Basislijnevaluatie webbeveiliging in Beveiliging en controle in OMS](./media/oms-security-web-baseline/oms-security-web-baseline-fig5.png)

3. In het linkerdeelvenster ziet u het aantal webservers vergeleken met de basislijn, het gemiddelde percentage van regels waaraan is voldaan door de geëvalueerde servers en de lijst met servers die zijn beoordeeld.
4. In het rechterdeelvenster staat een lijst van de unieke regels waaraan niet is voldaan op basis van *Ernst* en *Type regel*. Als u op een van de regels in het rechterdeelvenster klikt, worden de details van die regel weergegeven. Op de onderstaande afbeelding ziet u een voorbeeld. De regel die is geëvalueerd, wordt vermeld onder *Instellingen voor regel*. Het veld *AzId* bevat de unieke id voor elke regel en wordt door Microsoft gebruikt voor het bijhouden van de basislijnregels. Daarnaast kunnen gebruikers het *Verwachte resultaat* (de door Microsoft aanbevolen waarde) en andere details met betrekking tot de beveiligingsimpact van de regel bekijken.
    
    ![Query’s uitvoeren](./media/oms-security-web-baseline/oms-security-web-baseline-fig6.png)

5. U kunt uw eigen query's maken om de resultaten bekijken. 

De eerste query die u kunt gebruiken is de **Web Baseline Assessment Summary** (Overzicht van de webbasislijnevaluatie). Typ in het veld **Beginnen met zoeken** de volgende query: *Type=SecurityBaselineSummary BaselineType=Web*. Hieronder ziet u een voorbeeld van de uitvoer:

![Queryresultaat](./media/oms-security-web-baseline/oms-security-web-baseline-fig7.png)

>[!NOTE] 
>In deze query geeft elke record het evaluatieoverzicht op één server weer.

Zodra u **Zoeken in logboeken** hebt geopend, kunt u verschillende query's invoeren om meer informatie over de evaluatie van de webbasislijn te verkrijgen. U kunt naast de voorgaande query ook de volgende query's gebruiken in deze preview:

**Web Baseline Rule Assessment** (Evaluatie van webbasislijnregels): elke record vertegenwoordigt een evaluatie van één webbasislijnregel op één server. Dit omvat alle gegevens voor een regel die is mislukt, het *Sitepad* op basis waarvan de regel is geëvalueerd, het *Verwachte resultaat* en het *Werkelijke resultaat*.

Query: *Type=SecurityBaseline BaselineType=Web AnalyzeResult=Failed*

![Queryresultaat 2](./media/oms-security-web-baseline/oms-security-web-baseline-fig8.png)

**Alle resultaten weergeven voor een specifieke server**: deze query laat zien hoe u resultaten van een specifieke server kunt bekijken: Query: *Type=SecurityBaseline BaselineType=Web Computer=BaselineTestVM1*

![Queryresultaat 3](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

U kunt deze records/query's gebruiken om uw eigen dashboards, rapporten en waarschuwingen te maken. Hier volgt een voorbeeld van een UI-besturingselement dat u aan uw dashboard kunt toevoegen. U kunt [hier](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/) leren hoe u uw gegevens visualiseert met behulp van de weergaveontwerper van OMS. Het scherm hieronder bevat een voorbeeld van hoe de tegel eruitziet nadat u deze aanpassing hebt gemaakt.

![dashboard](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

## <a name="see-also"></a>Zie ook
In dit document hebt u meer kunnen lezen over het evalueren van de webbasislijn met behulp van de oplossing voor beveiliging en controle voor OMS. Raadpleeg de volgende artikelen voor meer informatie over OMS Beveiliging:

* [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren](oms-security-responding-alerts.md)
* [Resources bewaken in de oplossing Beveiliging en controle van Operations Management Suite ](oms-security-monitoring-resources.md)

