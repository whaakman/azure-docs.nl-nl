---
title: Herstel na noodgevallen en geo-distributie in duurzame functies - Azure
description: Meer informatie over herstel na noodgevallen en geo-distributie in duurzame functies.
services: functions
author: MS-Santi
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 8eb42a60045304416ec6aa1099a84b1e264c692d
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="disaster-recovery-and-geo-distribution"></a>Herstel bij noodgeval en geodistributie

## <a name="overview"></a>Overzicht
In Azure Duurzame functies, wordt alle statussen permanent in Azure Storage. Een [taak hub](durable-functions-task-hubs.md) is een logische container voor Azure Storage-resources die worden gebruikt voor integraties. Orchestrator en activiteit functies kunnen alleen met elkaar communiceren als ze deel uitmaken van dezelfde taak hub.
De beschreven scenario's voorstellen implementatieopties voor betere beschikbaarheid en uitvaltijd tijdens disaster recovery activiteiten.
Het is belangrijk dat u ziet dat deze scenario's zijn gebaseerd op actief / passief-configuraties, omdat ze worden geleid door het gebruik van Azure Storage. Dit patroon bestaat uit een back-up (passieve) functie-app implementeren op een andere regio. Traffic Manager wordt de primaire (actief) functie-app voor beschikbaarheid bewaken. Het failover naar de back-upfunctie-app als de primaire mislukt. Zie voor meer informatie [Traffic Manager](https://azure.microsoft.com/services/traffic-manager/)van [prioriteit verkeersroutering-methode.](../traffic-manager/traffic-manager-routing-methods.md#a-name--priorityapriority-traffic-routing-method)


>[!NOTE]
>- De configuratie van de voorgestelde actief / passief zorgt ervoor dat een client kan altijd voor het activeren van nieuwe integraties via HTTP. Echter, als gevolg van twee functie apps delen dezelfde opslag hebt, achtergrondverwerking worden gedistribueerd tussen beide parameters concurreren voor berichten in de dezelfde wachtrijen. Deze configuratie leidt ertoe dat toegevoegde uitgaande kosten voor de secundaire functie-app in.
>- De onderliggende opslag account en taak hub in de primaire regio worden gemaakt en worden gedeeld door beide apps de functie.
>- Alle functie-apps die toch toe zijn geïmplementeerd, moeten delen de dezelfde functie sneltoetsen in het geval van wordt geactiveerd via HTTP. De Runtime van Functions beschrijft een [management API](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) waarmee consumenten programmatisch toevoegen, verwijderen en bijwerken van de functie sleutels.

## <a name="scenario-1---load-balanced-compute-with-shared-storage"></a>Scenario 1 - taakverdeling compute met gedeelde opslag
Als de beheerinfrastructuur in Azure is mislukt, is de functie-app niet beschikbaar. Dit scenario gebruikt u de mogelijkheid van dergelijke downtime minimaliseren, twee functie apps die worden geïmplementeerd naar verschillende regio's. Traffic Manager is geconfigureerd om problemen te detecteren in de primaire functie-app en dat u verkeer automatisch omgeleid naar de functie-app in de secundaire regio. Deze functie-app deelt de dezelfde Azure-opslagaccount en de taak Hub. Daarom moet de status van de functie apps niet verloren en werk normaal kan worden hervat. Wanneer status wordt teruggezet naar de primaire regio, wordt Azure Traffic Manager routering aanvragen voor die functie-app automatisch gestart.


![Diagram waarin scenario 1.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario01.png)

Er zijn diverse voordelen wanneer u dit implementatiescenario:
- Als de beheerinfrastructuur mislukt, kunt werken via regio zonder verlies van status hervatten in de failover.
- Traffic Manager zorgt voor de automatische failover naar de app in orde functie automatisch.
- Traffic Manager automatisch opnieuw wordt ingesteld door het verkeer naar de primaire functie-app nadat de onderbreking is opgelost.

Echter, in dit scenario overwegen:
- Als de functie-app wordt geïmplementeerd met behulp van een specifieke App Service-abonnement, extra het repliceren van de beheerinfrastructuur in de failover via datacenter kosten leidt.
- Dit scenario bevat informatie over storingen op de beheerinfrastructuur, maar het storage-account blijft de storingspunt voor de functie App. Als er een storing van de opslag, is de toepassing lijden heeft onder een uitvaltijd.
- Als de failover van de functie-app wordt uitgevoerd, zullen er langere latentie omdat deze toegang hebben tot de opslagaccount tussen regio's.
- Toegang tot de storage-service vanaf een andere regio waar deze zich bevindt in hogere kosten als gevolg van uitgaand netwerkverkeer in rekening worden gebracht.
- Dit scenario is afhankelijk van op Traffic Manager. U overweegt [de werking van Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), kan het enige tijd totdat een clienttoepassing die een duurzame functie verbruikt moet de functie app-adres van Traffic Manager opnieuw een query zijn. 


## <a name="scenario-2---load-balanced-compute-with-regional-storage"></a>Scenario 2 - taakverdeling compute met regionale opslag
Het voorgaande scenario wordt alleen het geval van storing in de beheerinfrastructuur behandeld. Als de storage-service is mislukt, leidt dit tot een storing van de functie-app.
Dit scenario gebruikt om ervoor te zorgen, ononderbroken werking van de functies voor duurzame, een lokale storage-account op elke regio waarop de functie-apps zijn geïmplementeerd.

![Diagram waarin scenario 2.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario02.png)

Deze aanpak voegt verbeteringen op het vorige scenario:
- Als de functie-app is mislukt, zorgt het Traffic Manager mislukt via naar de secundaire regio. Omdat de functie-app, is afhankelijk van zijn eigen storage-account, moet de duurzame functies blijven echter werken.
- Tijdens een failover is er geen extra latentie in de failover via regio, omdat de functie-app en het opslagaccount geplaatst zijn.
- Mislukken van de opslaglaag veroorzaakt fouten in de duurzame functies die op zijn beurt via regio een omleiding naar de failover wordt geactiveerd. Opnieuw, omdat de functie-app en de opslag geïsoleerd per regio zijn, de duurzame functies blijven werken.
 
Belangrijke aandachtspunten voor dit scenario:
- Als de functie-app wordt geïmplementeerd met behulp van een speciale AppService-abonnement, extra het repliceren van de beheerinfrastructuur in de failover via datacenter kosten leidt.
- Huidige status is niet failover, die impliceert dat wordt uitgevoerd en controlepunt functies mislukt. Het is aan de clienttoepassing opnieuw/herstart van het werk.

## <a name="scenario-3---load-balanced-compute-with-grs-shared-storage"></a>Scenario 3: met gelijke taakverdeling compute met GRS gedeelde opslag
Dit scenario is een wijziging via het eerste scenario implementeren van een account met gedeelde opslag. Het belangrijkste verschil dat het opslagaccount is gemaakt met geo-replicatie is ingeschakeld.
Functioneel, dit scenario biedt dezelfde voordelen als Scenario 1, maar kunnen de voordelen van aanvullende gegevens herstellen:
- Geografisch redundante opslag (GRS) en GRS leestoegang (RA-GRS) maximaliseren beschikbaarheid voor uw opslagaccount.
- Als er een storing van de regio van de storage-service, is een van de mogelijkheden dat de bewerkingen van het datacenter dat opslag moet failover naar de secundaire regio bepalen. Account voor toegang tot opslag wordt in dit geval transparant worden omgeleid naar de geogerepliceerde kopie van het opslagaccount, zonder tussenkomst van de gebruiker.
- Status van de duurzame functies in dit geval blijven behouden tot de laatste replicatie van de storage-account om de paar minuten plaatsvindt.
Net als bij de andere scenario's en zijn er enkele belangrijke overwegingen:
- Failover uitvoeren naar de replica wordt uitgevoerd door datacenterbeheerders en het kan enige tijd duren. Tot die tijd, wordt de functie-app te maken met een storing.
- Er is een extra kosten voor het gebruik van geogerepliceerde storage-accounts.
- GRS optreedt asynchroon. Sommige van de meest recente transacties mogelijk verloren gegaan vanwege de latentie van het replicatieproces.

![Diagram waarin scenario 3.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario03.png)


## <a name="next-steps"></a>Volgende stappen

U kunt meer lezen over [ontwerpen van maximaal beschikbare toepassingen met behulp van RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md)
