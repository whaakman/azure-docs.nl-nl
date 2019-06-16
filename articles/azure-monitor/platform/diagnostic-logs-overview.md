---
title: Overzicht van diagnostische logboeken in Azure
description: Meer informatie over Azure diagnostische logboeken in Azure Monitor en hoe u deze kunt gebruiken om te begrijpen van gebeurtenissen die plaatsvinden binnen een Azure-resource.
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: 8902e29baa5802e3416bcda97ca59a5576d41829
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244868"
---
# <a name="overview-of-azure-diagnostic-logs"></a>Overzicht van diagnostische logboeken in Azure

**Diagnostische logboeken** bieden uitgebreide, regelmatig gegevens over de werking van een Azure-resource. Met Azure Monitor worden de beschikbare twee typen logboeken met diagnostische gegevens:

* **Logboeken voor de tenantsleutel** -deze logboeken afkomstig zijn van het tenantniveau-services die buiten een Azure-abonnement bestaan, zoals Azure Active Directory-Logboeken.
* **Logboeken van de resource** -deze logboeken afkomstig zijn van de Azure-services die resources binnen een Azure-abonnement, zoals Network Security Groups of Storage-Accounts implementeren.

    ![Resource diagnostische logboeken en andere typen logboeken](media/diagnostic-logs-overview/Diagnostics_Logs_vs_other_logs_v5.png)

De inhoud van deze logboeken is afhankelijk van het Azure-service en resource-type. Tellers van regels voor Network Security Group en Key Vault controles zijn bijvoorbeeld twee typen logboeken met diagnostische gegevens.

Deze logboeken afwijken van de [activiteitenlogboek](activity-logs-overview.md). Het activiteitenlogboek biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement met behulp van Resource Manager, bijvoorbeeld, het maken van een virtuele machine of een logische app verwijderen. Het activiteitenlogboek is een logboek op abonnementsniveau. Resourceniveau diagnoselogboeken bieden inzicht in bewerkingen die zijn uitgevoerd binnen die resource zelf, bijvoorbeeld een geheim ophalen uit een Key Vault.

Deze logboeken afwijken van de Gast OS-niveau van diagnostische logboeken. Diagnostische logboeken van Guest OS zijn deze die worden verzameld door een agent die wordt uitgevoerd op een virtuele machine of andere ondersteund resourcetype. Diagnostische logboeken resourceniveau vereisen geen gegevens van de resource-specifieke agent en vastleggen van de Azure-platform zelf wordt geboden, terwijl Gast OS-niveau logboeken met diagnostische gegevens vastleggen van gegevens uit het besturingssysteem en toepassingen die worden uitgevoerd op een virtuele machine.

Niet alle services die ondersteuning voor de diagnostische logboeken die hier worden beschreven. [In dit artikel bevat een sectie aanbieding welke services diagnostische logboeken ondersteunen](./../../azure-monitor/platform/diagnostic-logs-schema.md).

## <a name="what-you-can-do-with-diagnostic-logs"></a>U kunt doen met diagnostische logboeken
Hier volgen enkele dingen die u met Logboeken met diagnostische gegevens doen kunt:

![Logische plaatsing van diagnostische logboeken](./media/diagnostic-logs-overview/Diagnostics_Logs_Actions.png)

* Opslaan naar een [ **Opslagaccount** ](../../azure-monitor/platform/archive-diagnostic-logs.md) voor controle of handmatige controle. U kunt opgeven de bewaartermijn (in dagen) via **instellingen voor resourcediagnose**.
* [Stream ze **Event Hubs** ](diagnostic-logs-stream-event-hubs.md) voor opname van een service van derden of aangepaste analyseoplossing zoals Power BI.
* Analyseren met [Azure Monitor](../../azure-monitor/platform/collect-azure-metrics-logs.md), waar de gegevens worden geschreven onmiddellijk naar Azure Monitor met hoeft niet de gegevens eerst naar opslag schrijven.  

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

U kunt een opslagaccount of Event Hubs-naamruimte die zich niet in hetzelfde abonnement bevinden als het abonnement dat Logboeken verzendt. De gebruiker die de instelling configureert, moet de juiste RBAC-toegang tot beide abonnementen hebben.

> [!NOTE]
>  U kan momenteel niet archiveren stroomlogboeken netwerk naar een opslagaccount die zich achter een beveiligd virtueel netwerk bevindt.

## <a name="diagnostic-settings"></a>Diagnostische instellingen

Diagnostische logboeken van resources zijn geconfigureerd met behulp van de instellingen voor resourcediagnose. Diagnostische logboeken tenant zijn geconfigureerd met behulp van de diagnostische instelling van een tenant. **Diagnostische instellingen** voor een service-besturingselement:

* Waar diagnostische logboeken en metrische gegevens worden verzonden (Storage-Account, Event Hubs en/of Azure Monitor).
* Welke logboekcategorieën worden verzonden en of u metrische gegevens ook worden verzonden.
* Hoe lang elke categorie logboekbestanden worden bewaard in een storage-account.
    - Een bewaarperiode van nul dagen betekent dat Logboeken altijd worden bewaard. De waarde kan anders een willekeurig aantal dagen tussen 1 en 365 zijn.
    - Als Logboeken opslaan in een Storage-Account is uitgeschakeld (bijvoorbeeld, als er alleen Event Hubs of Log Analytics-opties zijn geselecteerd), bewaarbeleid worden ingesteld, maar hebben het bewaarbeleid geen effect.
    - Bewaarbeleid zijn toegepast per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode van beleid worden verwijderd. Bijvoorbeeld, als u een beleid voor het bewaren van één dag had, worden aan het begin van de dag vandaag nog de logboeken van de dag voor gisteren vernietigd. De verwijderbewerking begint bij middernacht UTC, maar houd er rekening mee dat het kan tot 24 uur duren voor de logboeken worden verwijderd uit uw storage-account.

Deze instellingen zijn geconfigureerd in de diagnostische instellingen in de portal, met Azure PowerShell en CLI-opdrachten, of met de [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/).

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Bijvoorbeeld*: De metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>Ondersteunde services, categorieën en schema's voor diagnostische logboeken

[Raadpleeg dit artikel](../../azure-monitor/platform/diagnostic-logs-schema.md) voor een volledige lijst van ondersteunde services en de logboekcategorieën en schema's die worden gebruikt door deze services.

## <a name="next-steps"></a>Volgende stappen

* [Diagnostische logboeken van de resource naar Stream **Event Hubs**](diagnostic-logs-stream-event-hubs.md)
* [Diagnostische instellingen voor resources met behulp van de REST-API van Azure Monitor wijzigt](https://docs.microsoft.com/rest/api/monitor/)
* [Logboeken van Azure storage met Azure Monitor analyseren](collect-azure-metrics-logs.md)
