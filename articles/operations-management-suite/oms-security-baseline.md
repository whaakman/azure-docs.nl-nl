---
title: Basislijn van de oplossing Beveiliging en controle in Operations Management Suite | Microsoft Docs
description: In dit document wordt uitgelegd hoe u de oplossing Beveiliging en controle in OMS kunt gebruiken voor het uitvoeren van een evaluatie van de basislijn van alle bewaakte computers, voor nalevings- en beveiligingsdoeleinden.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 6f4cfda6f367cb8a68d038fa0a3390442be034c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Basislijnevaluatie in de oplossing Beveiliging en controle in Operations Management Suite
Dit document helpt u om de mogelijkheden van de [oplossing Beveiliging en controle in Operations Management Suite](operations-management-suite-overview.md) te gebruiken om basislijnevaluaties uit te voeren, en zo de beveiligde status van uw bewaakte resources te evalueren.

## <a name="what-is-baseline-assessment"></a>Wat is basislijnevaluatie?
Microsoft definieert samen met brancheorganisaties en overheidsinstanties overal ter wereld een Windows-configuratie die garandeert dat maximaal beveiligde serverimplementaties worden gebruikt. Deze configuratie bestaat uit een verzameling registersleutels, controlebeleidsinstellingen en beveiligingsbeleidsinstellingen, gecombineerd met waarden die door Microsoft voor deze instellingen worden aanbevolen. Deze verzameling staat bekend als de beveiligingsbasislijn. Met de functie van Beveiliging en controle in OMS om een basislijnevaluatie uit te voeren, kunt u al uw computers naadloos scannen om te zien of ze aan nalevingsvereisten voldoen. 

Er zijn drie soorten regels:

* **Registerregels**: deze controleren of registersleutels juist zijn ingesteld.
* **Controlebeleidsregels**: regels die betrekking hebben op uw controlebeleid.
* **Veiligheidsbeleidsregels**: regels die betrekking hebben op de machtigingen van de gebruiker op de machine.

> [!NOTE]
> Lees [Use OMS Security to assess the Security Configuration Baseline](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) (OMS-beveiliging gebruiken om de basislijn van de beveiligingsconfiguratie te evalueren) voor een kort overzicht van deze functie.
> 
> 

## <a name="security-baseline-assessment"></a>Evaluatie van de beveiligingsbasislijn
U kunt het dashboard gebruiken om uw huidige evaluatie van de beveiligingbasislijn te bekijken voor alle computers die worden bewaakt door Beveiliging en controle in OMS. Voer de volgende stappen uit om toegang te krijgen tot het dashboard voor het evalueren van de beveiligingsbasislijn:

1. Klik in het hoofddashboard van **Microsoft Operations Management Suite** op de tegel **Beveiliging en controle**.
2. Klik in het dashboard **Beveiliging en controle** op **Evaluatie van de basislijn** onder **Beveiligingsdomeinen**. Het dashboard **Evaluatie van de beveiligingsbasislijn** wordt weergeven zoals in de volgende afbeelding:
   
    ![Basislijnevaluatie in Beveiliging en controle in OMS](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Dit dashboard is onderverdeeld in drie belangrijke gebieden:

* **Computers vergeleken met de basislijn**: deze sectie bevat een overzicht van het aantal computers dat werd geopend en het percentage computers dat is geslaagd voor de evaluatie. Er wordt ook een top 10 van computers weergegeven en het resultaat voor de evaluatie in een percentage.
* **Status van de vereiste regels**: de bedoeling van deze sectie is om de aandacht te vestigen op de status van de mislukte regels op volgorde van ernst, en door mislukte regels per type weer te geven. Door naar de eerste grafiek te kijken, kunt u snel bepalen of de meeste mislukte regels kritiek zijn of niet. Er wordt ook een top 10 met mislukte regels en de ernst ervan weergegeven. De tweede grafiek geeft het type regel aan dat is mislukt tijdens de evaluatie. 
* **Computers waarop de evaluatie van de basislijn ontbreekt**: in deze sectie worden de computers weergegeven die niet zijn geopend als gevolg van compatibiliteitsproblemen met het besturingssysteem of als gevolg van fouten. 

### <a name="accessing-computers-compared-to-baseline"></a>Toegang tot computers vergeleken met de basislijn
In het ideale geval voldoen al uw computers aan de evaluatie van de beveiligingsbasislijn. In sommige omstandigheden is dat echter niet het geval. Als onderdeel van het proces voor beveiligingsbeheer is het belangrijk om ook een controle uit te voeren van de computers die niet voldeden aan alle beveiligingsevaluatietests. Een snelle manier om dat zichtbaar te maken, is door de optie **Computers geopend** in de sectie **Computers vergeleken met de basislijn** in te schakelen. Hier ziet u het resultaat van een zoekopdracht in het logboek in de vorm van de lijst met computers, zoals in het volgende scherm wordt weergegeven:

![Resultaten van geopende computers](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Het zoekresultaat wordt weergegeven in de vorm van een tabel, waarin de eerste kolom de computernaam bevat en de tweede het aantal mislukte regels. Klik op het aantal mislukte regels naast de computernaam voor informatie over het type regel dat is mislukt. U ziet een resultaat zoals in de volgende afbeelding:

![Details van resultaten van geopende computers](./media/oms-security-baseline/oms-security-baseline-fig3.png)

In dit zoekresultaat wordt het aantal gebruikte regels, het aantal mislukte kritieke regels, de waarschuwingsregels en de informatie over mislukte regels weergegeven.

### <a name="accessing-required-rules-status"></a>Status van toegang tot vereiste regels
Nadat u de informatie hebt gekregen over het percentage computers dat is geslaagd voor de evaluatie, wilt u mogelijk meer informatie ontvangen over welke regels mislukken overeenkomstig het kritieke karakter ervan. Deze visualisatie helpt u om prioriteiten te stellen wat betreft de computers die het eerst aan de beurt zijn, zodat u zeker weet dat ze tijdens de volgende evaluatie aan de nalevingsvereisten voldoen. Beweeg de muisaanwijzer over het onderdeel Kritiek van de grafiek dat zich op de tegel **Mislukte regels op ernst** onder **Status van de vereiste regels** bevindt en klik daarop. U ziet een resultaat zoals in het volgende scherm:

![Mislukte regels per gegevens over ernst](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

In dit logboekresultaat ziet u het type basislijnregel dat is mislukt, de beschrijving van deze regel en de CCE-id (Common Configuration Enumeration) van deze beveiligingsregel. Deze kenmerken moeten volstaan om een corrigerende actie te kunnen uitvoeren om dit probleem op de doelcomputer op te lossen.

> [!NOTE]
> Meer informatie over CCE vindt u in de [National Vulnerability Database](https://nvd.nist.gov/cce/index.cfm).
> 
> 

### <a name="accessing-computers-missing-baseline-assessment"></a>Computers openen waarop de evaluatie van de basislijn ontbreekt
OMS ondersteunt het basislijnprofiel van het domeinlid en de Domain Controller op Windows Server 2008 R2 tot en met Windows Server 2012 R2. De basislijn voor Windows Server 2016 is nog niet helemaal klaar en wordt toegevoegd zodra deze is gepubliceerd. Alle andere besturingssystemen die via basislijnevaluatie in Beveiligings en controle in OMS zijn gescand, worden weergegeven onder de sectie **Computers waarop de evaluatie van de basislijn ontbreekt**.

## <a name="see-also"></a>Zie ook
In dit document hebt u meer kunnen lezen over het evalueren van de basislijn door Beveiliging- en controle in OMS. Raadpleeg de volgende artikelen voor meer informatie over OMS Beveiliging:

* [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren](oms-security-responding-alerts.md)
* [Resources bewaken in de oplossing Beveiliging en controle van Operations Management Suite ](oms-security-monitoring-resources.md)

