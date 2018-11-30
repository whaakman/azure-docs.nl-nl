---
title: Herstel na noodgevallen en geo-distributie in duurzame functies - Azure
description: Meer informatie over herstel na noodgevallen en geo-distributie in duurzame functies.
services: functions
author: MS-Santi
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: d999350f309dbd2bf74bbb3d10e74feddf6ee602
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642380"
---
# <a name="disaster-recovery-and-geo-distribution"></a>Herstel bij noodgeval en geodistributie

## <a name="overview"></a>Overzicht
In de duurzame functies van Azure, worden alle statussen persistent gemaakt in Azure Storage. Een [taak hub](durable-functions-task-hubs.md) is een logische container voor Azure Storage-resources die worden gebruikt voor indelingen. Orchestrator en activiteit functies kunnen alleen met elkaar communiceren wanneer ze deel uitmaken van dezelfde taak hub.
De beschreven scenario's voorstellen implementatieopties voor betere beschikbaarheid en uitvaltijd tijdens de disaster recovery activiteiten.
Het is belangrijk dat u ziet dat deze scenario's zijn gebaseerd op actief-passief-configuraties, omdat ze worden geleid door het gebruik van Azure Storage. Dit patroon bestaat uit een back-up (passieve) functie-app implementeren naar een andere regio. Traffic Manager bewaakt de primaire (actieve) functie-app voor beschikbaarheid. Het wordt failover naar de back-functie-app als de primaire mislukt. Zie voor meer informatie, [Traffic Manager](https://azure.microsoft.com/services/traffic-manager/)van [prioriteit Verkeersrouteringsmethode.](../../traffic-manager/traffic-manager-routing-methods.md#a-name--priorityapriority-traffic-routing-method)


>[!NOTE]
>- De voorgestelde actief-passieve configuratie zorgt ervoor dat een client altijd kunnen voor het activeren van nieuwe indelingen via HTTP. Als met twee functie-apps delen dezelfde opslag, wordt verwerking op de achtergrond worden verdeeld tussen beide providers, en dingen om de berichten op de dezelfde wachtrijen. Deze configuratie worden in rekening gebracht op de kosten voor de secundaire functie-app toegevoegd uitgaand verkeer.
>- De onderliggende opslag account en de taak hub in de primaire regio worden gemaakt en worden gedeeld door beide functie-apps.
>- Alle functie-apps die toch toe worden geïmplementeerd, moeten delen dezelfde functie toegang tot sleutels in het geval van via HTTP wordt geactiveerd. De Functions-Runtime wordt aangegeven dat een [management API](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) waarmee consumenten programmatisch toevoegen, verwijderen en bijwerken van de functietoetsen.

## <a name="scenario-1---load-balanced-compute-with-shared-storage"></a>Scenario 1: met gelijke taakverdeling Computing met gedeelde opslag
Als de beheerinfrastructuur in Azure is mislukt, wordt de functie-app beschikbaar. In dit scenario gebruikt u de mogelijkheid van dergelijke downtime minimaliseren, twee functie-apps geïmplementeerd naar verschillende regio's. Traffic Manager is geconfigureerd voor detectie van problemen op de primaire functie-app en automatisch verkeer omleiden naar de functie-app in de secundaire regio. Deze functie-app deelt de dezelfde Azure-opslagaccount en de taak Hub. De status van de functie-apps niet verloren en daarom werken normaal kan worden hervat. Zodra de status is hersteld naar de primaire regio, wordt Azure Traffic Manager Routeringsaanvragen voor die functie-app automatisch gestart.


![Diagram waarin scenario 1.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario01.png)

Er zijn verschillende voordelen bij het gebruik van dit implementatiescenario:
- Als de rekeninfrastructuur mislukt, kunt werken via regio zonder verlies van de status in de failover hervatten.
- Traffic Manager zorgt dat de automatische failover wordt uitgevoerd aan de orde functie-app automatisch.
- Traffic Manager automatisch opnieuw wordt ingesteld door het verkeer naar de primaire functie-app nadat de onderbreking is verholpen.

Met behulp van dit scenario echter overwegen:
- Als de functie-app wordt geïmplementeerd met behulp van een toegewezen App Service-plan, verhoogt de rekeninfrastructuur in de failover via datacenter repliceren kosten.
- In dit scenario bevat informatie over storingen op de compute-infrastructuur, maar het opslagaccount blijft de single point of failure voor de functie-App. Als er een storing in de opslag, heeft de toepassing een uitvaltijd.
- Als de failover van de functie-app wordt uitgevoerd, wordt er verhoogde latentie omdat deze toegang krijgen de storage-account in regio's tot.
- Toegang tot de storage-service in een andere regio waar deze zich bevinden in hogere kosten vanwege uitgaande netwerkverkeer worden in rekening gebracht.
- In dit scenario is afhankelijk van de op Traffic Manager. U overweegt [hoe Traffic Manager werkt](../../traffic-manager/traffic-manager-how-it-works.md), kan het enige tijd totdat een clienttoepassing die gebruikmaakt van een duurzame functie moet het adres in de functie-app vanuit Traffic Manager opnieuw een query zijn. 


## <a name="scenario-2---load-balanced-compute-with-regional-storage"></a>Scenario 2: met gelijke taakverdeling Computing met regionale opslag
Het voorgaande scenario omvat alleen het geval van storing in de rekeninfrastructuur. Als de storage-service is mislukt, wordt er een storing optreedt van de functie-app leiden.
In dit scenario gebruikt om ervoor te zorgen, ononderbroken werking van de duurzame functies, een lokale storage-account op elke regio waarop de functie-apps zijn geïmplementeerd.

![Diagram waarin scenario 2.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario02.png)

Deze aanpak voegt verbeteringen toe op het voorgaande scenario:
- Als de functie-app is mislukt, zorgt Traffic Manager van failover wordt uitgevoerd naar de secundaire regio. Omdat de functie-app, is afhankelijk van zijn eigen storage-account, moet de duurzame functies blijven echter werken.
- Tijdens een failover wordt uitgevoerd is geen extra latentie in de failover via regio, omdat de functie-app en het opslagaccount geplaatst worden.
- Fout van de storage-laag zorgt ervoor dat van fouten in de duurzame functies die op zijn beurt een omleiding naar de failover wordt geactiveerd via de regio. Nogmaals, omdat de functie-app en de opslag, geïsoleerd per regio zijn, de duurzame functies blijven werken.
 
Belangrijke overwegingen voor dit scenario:
- Als de functie-app wordt geïmplementeerd met behulp van een toegewezen App service-plan, verhoogt de rekeninfrastructuur in de failover via datacenter repliceren kosten.
- Huidige status is niet een failover, die aangeeft dat wordt uitgevoerd en controlepunt functies mislukken. Het is aan de clienttoepassing opnieuw proberen/restart het werk.

## <a name="scenario-3---load-balanced-compute-with-grs-shared-storage"></a>Scenario 3: gelijke Computing met gedeelde GRS-opslag
Dit scenario is een wijziging in het eerste scenario implementeren van een gedeelde storage-account. Het belangrijkste verschil dat het opslagaccount is gemaakt met geo-replicatie is ingeschakeld.
Functioneel, in dit scenario biedt dezelfde voordelen als Scenario 1, maar kunnen aanvullende gegevens recovery voordelen:
- Geografisch redundante opslag (GRS) en Read-access GRS (RA-GRS) maximale beschikbaarheid voor uw opslagaccount.
- Als er een storing in de regio van de storage-service, is een van de mogelijkheden die de datacenterbewerkingen te bepalen dat opslag moet worden failover naar de secundaire regio. In dit geval wordt toegang tot opslagaccount transparant worden omgeleid naar de kopie geo-replicatie van de storage-account, zonder tussenkomst van de gebruiker.
- In dit geval worden de status van de duurzame functies tot de laatste replicatie van de storage-account, deze wordt weergegeven om de paar minuten bewaard.
Net als bij de andere scenario's, zijn er enkele belangrijke overwegingen:
- Een failover uitvoeren naar de replica wordt uitgevoerd door de datacenter-operators en het kan enige tijd duren. Tot die tijd, de functie-app heeft nadelige gevolgen voor een storing.
- Er is een hogere kosten voor het gebruik van accounts voor opslag met geo-replicatie.
- GRS optreedt asynchroon. Enkele van de meest recente transacties verloren zijn gegaan vanwege de latentie van het replicatieproces.

![Diagram waarin scenario 3.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario03.png)


## <a name="next-steps"></a>Volgende stappen

U kunt meer lezen over [het ontwerpen van maximaal beschikbare toepassingen met RA-GRS](../../storage/common/storage-designing-ha-apps-with-ragrs.md)
