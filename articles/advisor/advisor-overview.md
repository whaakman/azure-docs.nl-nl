---
title: Inleiding tot Azure Advisor | Microsoft Docs
description: Gebruik Azure Advisor om te optimaliseren van uw Azure-implementaties.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 906450f75557820bb27762707c3328b08b23cccb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2017
---
# <a name="introduction-to-azure-advisor"></a>Inleiding tot Azure Advisor

Informatie over de belangrijkste mogelijkheden van Azure Advisor en antwoorden op veelgestelde vragen.

## <a name="what-is-advisor"></a>Wat is Advisor?
Advisor is een persoonlijke cloud-consultant waarmee u Volg de aanbevolen procedures om uw Azure-implementaties te optimaliseren. Het analyseert uw resourceconfiguratie en de telemetrie van de informatie over het gebruik en vervolgens raadt aan om de oplossingen waarmee u de kosteneffectiviteit, prestaties, beschikbaarheid en beveiliging van uw Azure-resources te verbeteren.

Met Advisor, kunt u het volgende doen:
* Proactieve, bruikbare ophalen en gepersonaliseerde aanbevolen procedures. 
* Verbeteren de prestaties, beveiliging en hoge beschikbaarheid van uw resources, zoals u Identificeer mogelijkheden om te beperken van uw algehele Azure hoeven te besteden aan.
* Aanbevelingen met voorgestelde acties inline worden opgehaald.

U hebt toegang tot Advisor via de [Azure-portal](https://aka.ms/azureadvisordashboard). Aanmelden bij de [portal](https://portal.azure.com), zoek **Advisor** in het navigatiemenu of zoekt u in de **meer services** menu.

De Advisor-dashboard worden persoonlijke aanbevelingen voor al uw abonnementen weergegeven.  U kunt filters om weer te geven van de aanbevelingen voor specifieke abonnementen en brontypen toepassen.  De aanbevelingen worden onderverdeeld in vier categorieën: 

* **Hoge beschikbaarheid**: om te controleren en de continuïteit van uw bedrijfskritieke toepassingen te verbeteren. Zie voor meer informatie [aanbevelingen voor hoge beschikbaarheid van Advisor](advisor-high-availability-recommendations.md).
* **Beveiliging**: voor het detecteren van bedreigingen en zwakke plekken die tot beveiligingsrisico's leiden mogelijk. Zie voor meer informatie [Advisor beveiligingsaanbevelingen](advisor-security-recommendations.md).
* **Prestaties**: voor het verbeteren van de snelheid van uw toepassingen. Zie voor meer informatie [Advisor prestaties aanbevelingen](advisor-performance-recommendations.md).
* **Kosten**: om te optimaliseren en uw algehele Azure uitgaven te verminderen. Zie voor meer informatie [aanbevelingen van Advisor kosten](advisor-cost-recommendations.md).

  ![Advisor aanbeveling typen](./media/advisor-overview/advisor-dashboard.png)

> [!NOTE]
> Gebruik Azure Advisor met een abonnement, een abonnement *eigenaar* moet het dashboard Advisor starten.  Deze actie wordt het abonnement met Advisor geregistreerd.  Vanaf dat moment op een abonnement *eigenaar*, *Inzender*, of *lezer* toegang heeft tot de aanbevelingen van Advisor voor het abonnement. 

U kunt op een categorie om de lijst met aanbevelingen in die categorie weer te geven en een aanbeveling voor meer informatie over het selecteren.  U kunt ook meer informatie over acties die u uitvoeren kunt om te profiteren van een kans of een probleem oplost.

![Advisor aanbeveling categorie](./media/advisor-overview/advisor-ha-category-example.png) 

Selecteer de aanbevolen actie om een aanbeveling voor het implementeren van de aanbeveling.  Een eenvoudige interface, waarmee u de aanbeveling implementeren of u Raadpleeg de documentatie die u met implementatie helpt wordt geopend.  Wanneer u een aanbeveling implementeert, kan het op een dag voor Advisor herkennen die duren.

Als u niet onmiddellijk actie ondernemen op een aanbeveling wilt, kunt u deze uitstellen voor een opgegeven periode of genegeerd.  Als u niet ontvangen van aanbevelingen voor een specifiek abonnement of resourcegroep wilt, kunt u Advisor alleen om aanbevelingen te genereren voor de opgegeven abonnementen en resourcegroepen configureren.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-do-i-access-advisor"></a>Hoe krijg ik toegang tot Advisor?
U hebt toegang tot Advisor via de [Azure-portal](https://aka.ms/azureadvisordashboard). Aanmelden bij de [portal](https://portal.azure.com), zoek **Advisor** in het navigatiemenu of zoekt u in de **meer services** menu.

U kunt ook aanbevelingen van Advisor weergeven via de interface van de resource virtuele machine. Kies een virtuele machine en schuif vervolgens naar de Advisor-aanbevelingen in het menu. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Welke machtigingen heb ik nodig voor toegang tot Advisor?

Als u de aanbevelingen voor een abonnement Advisor te ontvangen, moet u uw abonnement eerst registreren bij Advisor. Een abonnement wordt geregistreerd wanneer een abonnement *eigenaar* start van de Advisor-dashboard. Dit is een eenmalige bewerking. Nadat het abonnement is geregistreerd, kunt u de aanbevelingen van Advisor als openen *eigenaar*, *Inzender*, of *lezer* van een abonnement.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Welke bronnen biedt Advisor aanbevelingen voor?

Advisor bevat aanbevelingen voor virtuele machines, beschikbaarheidssets Toepassingsgateways, App-Services, SQL-servers, SQL-databases en Redis-Cache.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Kan ik uitstellen of een aanbeveling negeren?

Als u wilt uitstellen of een aanbeveling negeren, klikt u op de **uitstellen** koppeling. U kunt opgeven dat een tijd uitstellen periode of selecteer **nooit** naar de aanbeveling negeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor aanbevelingen:

* [Aan de slag met Advisor](advisor-get-started.md)
* [Hoge beschikbaarheid aanbevelingen Advisor te ontvangen](advisor-high-availability-recommendations.md)
* [Aanbevelingen voor beveiliging van Advisor](advisor-security-recommendations.md)
* [Aanbevelingen van Advisor-prestaties](advisor-performance-recommendations.md)
* [Advisor kosten aanbevelingen](advisor-cost-recommendations.md)
