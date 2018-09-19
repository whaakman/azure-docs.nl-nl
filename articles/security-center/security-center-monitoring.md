---
title: Resource beveiliging hygiëne in Azure Security Center | Microsoft Docs
description: In dit artikel helpt u bij het bewaken van uw resource beveiliging hygiëne in Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/21/2018
ms.author: rkarlin
ms.openlocfilehash: 361fc2c6931dcfaed062bd39b75be7bef059b467
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46122059"
---
# <a name="monitoring-resource-security-hygiene-in-azure-security-center"></a>Resource beveiliging hygiëne in Azure Security Center bewaken
Dit artikel helpt u de bewakingsmogelijkheden in Azure Security Center gebruikt om te controleren of de resourcebeveiliging van uw als nauwe als mogelijke en monitor naleving van beleid.

## <a name="what-is-resource-security-hygiene"></a>Wat is resource beveiliging hygiëne?
Bij het woord bewaking wordt misschien gesuggereerd dat er wordt gewacht tot een gebeurtenis plaatsvindt en dat er dan op de situatie wordt gereageerd. Resource beveiliging hygiëne verwijst naar het met een proactieve strategie waarbij uw resources voor het identificeren van systemen die niet voldoen aan de standaarden van de organisatie of aanbevolen procedures worden gecontroleerd.

## <a name="resource-security-hygiene"></a>Beveiligingscontroles van resources
Nadat u een [beveiligingsbeleid](security-center-policies.md) voor de resources van een abonnement hebt ingeschakeld, analyseert Security Center de beveiliging van uw resources om mogelijke beveiligingsproblemen op te sporen. Informatie over uw netwerkconfiguratie is onmiddellijk beschikbaar. Afhankelijk van het aantal virtuele machines en computers waarop de agent is geïnstalleerd, kan het een uur of langer duren voordat informatie is verzameld over configuratie van de VM's en de computer, zoals de status van de beveiligingsupdates en de configuratie van het besturingssysteem. U vindt een volledige lijst van problemen en manieren om uw netwerk beveiligen en herstellen van risico's in de **aanbevelingen** tegel.

U kunt de beveiligingsstatus van uw resources en eventuele problemen per resourcetype bekijken:

- Zie voor het bewaken van de status van uw computerbronnen en uw apps en ontvangen van aanbevelingen voor het verbeteren van de beveiliging, [bescherming van uw computers en toepassingen in Azure Security Center](security-center-virtual-machine-protection.md)
- Zie voor het bewaken van uw netwerkbronnen, zoals virtuele machines, netwerkbeveiligingsgroepen en -eindpunten en ontvangen van aanbevelingen voor het verbeteren van de beveiliging, [beveiligen van uw netwerk in Azure Security Center](security-center-network-recommendations.md) voor meer informatie informatie. 
- Zie voor het bewaken van uw gegevens en resources, zoals SQL-servers en storage-accounts, en ontvangen van aanbevelingen voor het verbeteren van de beveiliging, [beveiligen van Azure SQL-service en de gegevens in Azure Security Center](security-center-sql-service-recommendations.md) voor meer informatie . 
- Zie voor het bewaken van uw resources identiteits- en toegangsbeheer, met inbegrip van MFA en account machtigingen, en ontvangen van aanbevelingen voor het verbeteren van de beveiliging, [identiteit en toegang bewaken in Azure Security Center](security-center-identity-access.md) voor meer informatie. 
- Zie voor het controleren van Just in Time-toegang tot uw resources, [beheer van de virtuele machine toegang met just-in-time](security-center-just-in-time.md) voor meer informatie. 


Zie [Beveiligingsaanbevelingen implementeren in Azure Security Center](security-center-recommendations.md) voor meer informatie over het toepassen van aanbevelingen.



![De tegel Beveiligingsstatus van de resource](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)



## <a name="see-also"></a>Zie ook
In dit artikel hebt u kunnen lezen hoe u de bewakingsmogelijkheden in Azure Security Center gebruikt. Zie de volgende onderwerpen voor meer informatie over het Azure Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md): leer hoe u beveiligingsinstellingen configureert in Azure Security Center.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
