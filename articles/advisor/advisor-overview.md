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
ms.openlocfilehash: 96925f251cf4984a11516a962740e19a7b9589dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-advisor"></a>Inleiding tot Azure Advisor

Meer informatie over Azure Advisor en de belangrijkste mogelijkheden en antwoorden op veelgestelde vragen.

## <a name="what-is-advisor"></a>Wat is Advisor?
Advisor is een persoonlijke cloud-consultant waarmee u Volg de aanbevolen procedures om uw Azure-implementaties te optimaliseren. Het analyseert uw resourceconfiguratie en de telemetrie van de informatie over het gebruik en vervolgens raadt aan om de oplossingen waarmee u de kosteneffectiviteit, prestaties, beschikbaarheid en beveiliging van uw Azure-resources te verbeteren.

Met Advisor, kunt u het volgende doen:
* Proactieve, bruikbare ophalen en gepersonaliseerde aanbevolen procedures. 
* Verbeteren de prestaties, beveiliging en hoge beschikbaarheid van uw resources, zoals u Identificeer mogelijkheden om te beperken van uw algehele Azure hoeven te besteden aan.
* Aanbevelingen met voorgestelde acties inline worden opgehaald.

U hebt toegang tot Advisor via de [Azure-portal](https://aka.ms/azureadvisordashboard). Aanmelden bij de [portal](https://portal.azure.com), selecteer **Bladeren**, en schuif vervolgens naar **Azure Advisor**. De Advisor-dashboard toont de persoonlijke aanbevelingen voor een geselecteerde abonnement. 

De aanbevelingen worden onderverdeeld in vier categorieën: 

* **Hoge beschikbaarheid**: om te controleren en de continuïteit van uw bedrijfskritieke toepassingen te verbeteren. Zie voor meer informatie [aanbevelingen voor hoge beschikbaarheid van Advisor](advisor-high-availability-recommendations.md).

* **Beveiliging**: voor het detecteren van bedreigingen en zwakke plekken die tot beveiligingsrisico's leiden mogelijk. Zie voor meer informatie [Advisor beveiligingsaanbevelingen](advisor-security-recommendations.md).

* **Prestaties**: voor het verbeteren van de snelheid van uw toepassingen. Zie voor meer informatie [Advisor prestaties aanbevelingen](advisor-performance-recommendations.md).

* **Kosten**: te optimaliseren en reduceren uw algehele Azure besteden. Zie voor meer informatie [aanbevelingen van Advisor kosten](advisor-cost-recommendations.md).

  ![Advisor aanbeveling typen](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> Voor toegang tot de aanbevelingen Advisor te ontvangen, moet u eerst *registreren van uw abonnement* met Advisor. Een abonnement is geregistreerd als een *abonnement eigenaar* start van de Advisor-dashboard en klikt op de **aanbevelingen krijgen** knop. Dit is een *eenmalige bewerking*. Nadat het abonnement is geregistreerd, kunt u de aanbevelingen van Advisor als openen *eigenaar*, *Inzender*, of *lezer* voor een abonnement, resourcegroep of een specifieke bron.

U kunt klikken op een aanbeveling voor meer informatie over deze. U kunt ook meer informatie over acties die u uitvoeren kunt om te profiteren van een kans of een probleem oplost. 

Advisor biedt aanbevelingen met inline acties of documentatie koppelingen. Te klikken op een inline-actie doorloopt u via een 'begeleide gebruiker reis' om dit te implementeren. Een documentatie-koppeling te klikken, wordt u verwijst naar documentatie die wordt beschreven hoe u de actie handmatig implementeren. 

Advisor updates aanbevelingen per uur. Als u niet dat onmiddellijk actie ondernemen op een aanbeveling wilt, kunt u deze uitstellen voor een opgegeven periode of genegeerd. 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-do-i-access-advisor"></a>Hoe krijg ik toegang tot Advisor?
U hebt toegang tot Advisor via de [Azure-portal](https://aka.ms/azureadvisordashboard). Aanmelden bij de [portal](https://portal.azure.com), selecteer **Bladeren**, en schuif vervolgens naar **Azure Advisor**. De Advisor-dashboard toont de persoonlijke aanbevelingen voor een geselecteerde abonnement. 

U kunt ook aanbevelingen van Advisor weergeven via de resourceblade van de virtuele machine. Kies een virtuele machine en schuif vervolgens naar de Advisor-aanbevelingen in het menu. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Welke machtigingen heb ik nodig voor toegang tot Advisor?

Voor toegang tot de aanbevelingen Advisor te ontvangen, moet u eerst *registreren van uw abonnement* met Advisor. Een abonnement is geregistreerd als een *abonnement eigenaar* start van de Advisor-dashboard en klikt op de **aanbevelingen krijgen** knop. Dit is een *eenmalige bewerking*. Nadat het abonnement is geregistreerd, kunt u de aanbevelingen van Advisor als openen *eigenaar*, *Inzender*, of *lezer* voor een abonnement, resourcegroep of een specifieke bron.

### <a name="how-often-are-advisor-recommendations-updated"></a>Hoe vaak worden aanbevelingen van Advisor bijgewerkt?

Advisor aanbevelingen worden elk uur bijgewerkt.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Welke bronnen biedt Advisor aanbevelingen voor?

Advisor bevat aanbevelingen voor virtuele machines, beschikbaarheidssets Toepassingsgateways, App-Services, SQL-servers, SQL-databases en Redis-Cache.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Kan ik uitstellen of een aanbeveling negeren?

Als u wilt uitstellen of een aanbeveling negeren, klikt u op de **uitstellen** knop of koppeling. U kunt opgeven dat een tijd uitstellen periode of selecteer **nooit** naar de aanbeveling negeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor aanbevelingen:

* [Aan de slag met Advisor](advisor-get-started.md)
* [Hoge beschikbaarheid aanbevelingen Advisor te ontvangen](advisor-high-availability-recommendations.md)
* [Aanbevelingen voor beveiliging van Advisor](advisor-security-recommendations.md)
* [Aanbevelingen van Advisor-prestaties](advisor-performance-recommendations.md)
* [Advisor kosten aanbevelingen](advisor-cost-recommendations.md)
