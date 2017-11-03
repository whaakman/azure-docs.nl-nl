---
title: Meld u Analytics Veelgestelde vragen | Microsoft Docs
description: Antwoorden op veelgestelde vragen over de Azure Log Analytics-service.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: ad536ff7-2c60-4850-a46d-230bc9e1ab45
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: magoedte
ms.openlocfilehash: 0b27386cd0f9f3ae50314b8c5d7708aea3e3d028
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-faq"></a>Veelgestelde vragen over Log Analytics
Dit Microsoft-FAQ is een lijst met veelgestelde vragen over logboekanalyse in Microsoft Azure. Als u aanvullende vragen over Log Analytics hebt, gaat u naar de [discussieforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) en stel uw vragen. Wanneer een vraag vaak wordt gevraagd, wordt deze toegevoegd aan dit artikel zodat snel en eenvoudig kunnen worden gevonden.

## <a name="general"></a>Algemeen

### <a name="q-does-log-analytics-use-the-same-agent-as-azure-security-center"></a>Q. Log Analytics maakt gebruik dezelfde agent als Azure Security Center?

A. In eerdere juni 2017 begonnen Azure Security Center met behulp van Microsoft Monitoring Agent voor het verzamelen en opslaan van gegevens. Zie voor meer informatie, [Azure Security Center-Platform migratie Veelgestelde vragen over](../security-center/security-center-platform-migration-faq.md).

### <a name="q-what-checks-are-performed-by-the-ad-and-sql-assessment-solutions"></a>Q. Welke controles worden uitgevoerd door de AD en beoordeling van de SQL-oplossingen?

A. De volgende query ziet u een beschrijving van alle controles die momenteel worden uitgevoerd:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

De resultaten kunnen vervolgens worden geëxporteerd naar Excel voor verder onderzoek.

### <a name="q-why-do-i-see-something-different-than-oms-in-the-system-center-operations-manager-console"></a>V: Waarom zie ik iets anders dan OMS in de System Center Operations Manager-console?

A: afhankelijk van welke Update Rollup van Operations Manager u zich bevindt, ziet u een knooppunt voor *System Center Advisor*, *Operational Insights*, of *logboekanalyse*.

De tekenreeks tekst update *OMS* is opgenomen in een management pack, dat moet handmatig worden geïmporteerd. Als u de huidige tekst en functionaliteit, volg de instructies op de meest recente System Center Operations Manager Update Rollup KB-artikel en vernieuw de console.

### <a name="q-is-there-an-on-premises-version-of-log-analytics"></a>V: is er een on-premises versie van Log Analytics?

A: Nee. Log Analytics is een schaalbare cloudservice die worden verwerkt en opgeslagen grote hoeveelheden gegevens. 

### <a name="q-how-do-i-troubleshoot-if-log-analytics-is-no-longer-collecting-data"></a>Q. Hoe kan ik als logboekanalyse is niet meer gegevens verzamelt oplossen?

A: als u zijn op de gratis prijscategorie en meer dan 500 MB aan gegevens in een dag hebt verzonden, stopt het verzamelen van gegevens voor de rest van de dag. De dagelijkse limiet bereikt, is een veelvoorkomende reden die logboekanalyse stopt het verzamelen van gegevens of gegevens lijkt te ontbreken.

Log Analytics maakt een gebeurtenis van het type *bewerking* wanneer gegevensverzameling wordt gestart en gestopt. 

Voer de volgende query in de zoekopdracht om te controleren als u de dagelijkse limiet bereikt en er gegevens ontbreken:`Type=Operation OperationCategory="Data Collection Status"`

Wanneer het verzamelen van gegevens stopt, de *OperationStatus* is **waarschuwing**. Als gegevensverzameling wordt gestart, wordt de *OperationStatus* is **geslaagd**. 

De volgende tabel beschrijft oorzaken waardoor het verzamelen van gegevens stopt en een voorgestelde actie om het verzamelen van gegevens hervatten:

| Hiermee stopt u de gegevensverzameling reden                       | Verzamelen van gegevens hervatten |
| -------------------------------------------------- | ----------------  |
| Dagelijkse limiet van beschikbare gegevens bereikt<sup>1</sup>       | Wacht totdat de volgende dag voor de verzameling automatisch opnieuw wordt gestart, of<br> Wijzig in een betaald prijscategorie |
| Azure-abonnement is een onderbroken vanwege: <br> Gratis proefversie is beëindigd <br> Verlopen van Azure op te geven <br> Maandelijks uitgavenlimiet bereikt (bijvoorbeeld op een abonnement met MSDN of Visual Studio)                          | Converteren naar een betaald abonnement <br> Converteren naar een betaald abonnement <br> Limiet verwijderen of wacht u totdat de limiet wordt opnieuw ingesteld |

<sup>1</sup> als uw werkruimte op de gratis prijscategorie is, bent u beperkt tot 500 MB aan gegevens per dag verzenden naar de service. Wanneer u de dagelijkse limiet bereikt, stopt het verzamelen van gegevens tot de volgende dag. Gegevens die worden verzonden tijdens het verzamelen van gegevens is gestopt, is niet geïndexeerd en is niet beschikbaar voor het zoeken. Als gegevensverzameling wordt hervat, vindt de verwerking alleen voor nieuwe gegevens die worden verzonden. 

Log Analytics gebruikt UTC-tijd en elke dag om middernacht UTC begint. Als de werkruimte de dagelijkse limiet bereikt, hervat verwerking tijdens het eerste uur van de volgende dag (UTC).

### <a name="q-how-can-i-be-notified-when-data-collection-stops"></a>Q. Hoe kan ik gewaarschuwd wanneer de gegevens verzamelen stoppen?

A: Gebruik de stappen in [een waarschuwingsregel maakt](log-analytics-alerts-creating.md#create-an-alert-rule) wilt worden gewaarschuwd als het verzamelen van gegevens stopt.

Bij het maken van de waarschuwing voor wanneer het verzamelen van gegevens stopt, stel de:
- **Naam** naar *gegevensverzameling is gestopt*
- **Ernst** op *Waarschuwing*
- **Zoekquery** op `Type=Operation OperationCategory="Data Collection Status" OperationStatus=Warning`
- **Tijdvenster** naar *2 uur*.
- **Waarschuwingsfrequentie** op één uur omdat de gebruiksgegevens slechts één keer per uur worden bijgewerkt.
- **Waarschuwingen genereren op basis van** op het *aantal resultaten*
- **Aantal resultaten** op *Groter dan 0*

Gebruik de stappen in [Acties toevoegen aan waarschuwingsregels](log-analytics-alerts-actions.md) om een e-mail-, webhook- of runbookactie te configureren voor een waarschuwingsregel.


## <a name="configuration"></a>Configuratie
### <a name="q-can-i-change-the-name-of-the-tableblob-container-used-to-read-from-azure-diagnostics-wad"></a>Q. Kan ik de naam van de tabel/blob-container die is gebruikt om te lezen van Azure Diagnostics (af) wijzigen?

A. Nee, is het niet mogelijk om te lezen uit willekeurige tabellen of containers in Azure-opslag.

### <a name="q-what-ip-addresses-does-the-log-analytics-service-use-how-do-i-ensure-that-my-firewall-only-allows-traffic-to-the-log-analytics-service"></a>Q. Welke IP-adressen gebruikt de Log Analytics-service? Hoe kan ik ervoor zorgen dat mijn firewall alleen toestaat dat verkeer naar de Log Analytics-service?

A. De Log Analytics-service is ingebouwd in Azure. Log Analytics IP-adressen zijn de [Microsoft Azure Datacenter IP-adresbereiken](http://www.microsoft.com/download/details.aspx?id=41653).

Als de service-implementaties worden aangebracht, worden de werkelijke IP-adressen van de Log Analytics-service wijzigen. De DNS-namen om toe te staan via de firewall zijn gedocumenteerd op [proxy- en firewall-instellingen configureren in logboekanalyse](log-analytics-proxy-firewall.md).

### <a name="q-i-use-expressroute-for-connecting-to-azure-does-my-log-analytics-traffic-use-my-expressroute-connection"></a>Q. Ik ExpressRoute gebruiken voor het verbinden met Azure. Mijn verkeer Log Analytics maakt gebruik van mijn ExpressRoute-verbinding?

A. De verschillende soorten ExpressRoute verkeer worden beschreven in de [ExpressRoute-documentatie](../expressroute/expressroute-faqs.md#supported-services).

Verkeer met Log Analytics maakt gebruik van het openbare peering ExpressRoute-circuit.

### <a name="q-is-there-a-simple-and-easy-way-to-move-an-existing-log-analytics-workspace-to-another-log-analytics-workspaceazure-subscription"></a>Q. Is er een eenvoudige en eenvoudige manier om een bestaande werkruimte voor logboekanalyse verplaatsen naar een ander Log Analytics-werkruimte/Azure-abonnement?

A. De `Move-AzureRmResource` cmdlet kunt u een werkruimte voor logboekanalyse en een Automation-account van een Azure-abonnement verplaatsen naar een andere. Zie voor meer informatie [verplaatsen AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Deze wijziging kan ook worden gemaakt in de Azure portal.

U geen gegevens van een werkruimte voor logboekanalyse verplaatsen naar een andere, of de regio die Log Analytics-gegevens worden opgeslagen in wijzigen.

### <a name="q-how-do-i-add-log-analytics-to-system-center-operations-manager"></a>V: hoe kan ik logboekanalyse naar System Center Operations Manager toevoegen

A: bijwerken naar de meest recente updatepakket installeren en importeren van management packs kunt u verbinding maken met Operations Manager met logboekanalyse.

>[!NOTE]
>De Operations Manager-verbinding met logboekanalyse is alleen beschikbaar voor System Center Operations Manager 2012 SP1 en hoger.

### <a name="q-how-can-i-confirm-that-an-agent-is-able-to-communicate-with-log-analytics"></a>V: hoe kan ik controleren of een agent kan communiceren met Log Analytics?

A: om ervoor te zorgen dat de agent met OMS communiceren kan, gaat u naar: het Configuratiescherm, beveiliging en -instellingen, **Microsoft Monitoring Agent**.

Onder de **Azure logboekanalyse (OMS)** tabblad, Controleer of er een groen vinkje. Een groen vinkje wordt bevestigd dat de agent kan communiceren met de Azure-service.

Een geel waarschuwingspictogram betekent dat de agent is er communicatie van de problemen met logboekanalyse. Een veelvoorkomende reden hiervoor is dat de Microsoft Monitoring Agent-service is gestopt. Servicebesturingsbeheer gebruiken de service te starten.

### <a name="q-how-do-i-stop-an-agent-from-communicating-with-log-analytics"></a>V: hoe voorkom ik een agent niet communiceren met Log Analytics

A: In System Center Operations Manager, wordt de computer verwijderen uit de lijst van de beheerde computers OMS. Operations Manager werkt u de configuratie van de agent niet langer een rapport met logboekanalyse. Voor agents die zijn verbonden met logboekanalyse rechtstreeks, kunt u ze stopt vanaf communiceren via: het Configuratiescherm, beveiliging en -instellingen, **Microsoft Monitoring Agent**.
Onder **Azure logboekanalyse (OMS)**, verwijdert u alle werkruimten die worden vermeld.

### <a name="q-why-am-i-getting-an-error-when-i-try-to-move-my-workspace-from-one-azure-subscription-to-another"></a>V: Waarom krijg ik een fout als ik wil mijn werkruimte van één Azure-abonnement te verplaatsen naar een andere?

A: als u de Azure-portal gebruikt, zorg er dat alleen in de werkruimte is geselecteerd voor het verplaatsen. Selecteer niet de oplossingen--ze automatisch verplaatst nadat de werkruimte wordt verplaatst. 

Zorg ervoor dat u gemachtigd in beide Azure-abonnementen.

## <a name="agent-data"></a>Gegevens van agent
### <a name="q-how-much-data-can-i-send-through-the-agent-to-log-analytics-is-there-a-maximum-amount-of-data-per-customer"></a>Q. Hoeveel gegevens kan ik verzenden via de agent met Log Analytics? Is er een maximale hoeveelheid gegevens per klant?
A. Het gratis-plan Hiermee stelt u een dagelijkse limiet van 500 MB per werkruimte. De standard en premium-abonnementen hebben geen limiet voor de hoeveelheid gegevens die is geüpload. Als een cloudservice Log Analytics is ontworpen voor automatisch schalen tot het volume verwerken die afkomstig zijn van een klant – zelfs als deze TB per dag.

De agent Log Analytics is ontworpen om te controleren of er een kleine footprint. Een van onze klanten een blog over de tests die ze uitgevoerd tegen onze agent en hoe onder de indruk dat ze zijn geschreven. Het gegevensvolume varieert op basis van de oplossingen die u inschakelt. U vindt gedetailleerde informatie over het gegevensvolume en de verdeling zien door oplossing in de [gebruik](log-analytics-usage.md) pagina.

Voor meer informatie vindt u een [klant blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) over de lage footprint van de OMS-agent.

### <a name="q-how-much-network-bandwidth-is-used-by-the-microsoft-management-agent-mma-when-sending-data-to-log-analytics"></a>Q. Hoeveel netwerkbandbreedte wordt door de Microsoft Management Agent (MMA) gebruikt bij het verzenden van gegevens met Log Analytics?

A. Bandbreedte is een functie van de hoeveelheid gegevens die worden verzonden. Gegevens worden gecomprimeerd wanneer deze via het netwerk worden verzonden.

### <a name="q-how-much-data-is-sent-per-agent"></a>Q. Hoeveel gegevens per agent worden verzonden?

A. Afhankelijk van de hoeveelheid gegevens die zijn verzonden per agent:

* De oplossingen die u hebt ingeschakeld
* Het aantal logboeken en prestatiemeteritems worden verzameld
* De hoeveelheid gegevens in de logboeken

De gratis prijscategorie is een goede manier om vrijgeven verschillende servers en het typische gegevensvolume meter. Algemene informatie over het gebruik wordt weergegeven op de [gebruik](log-analytics-usage.md) pagina.

Voor computers die kunnen worden uitgevoerd van de agent WireData, gebruikt u de volgende query om te zien hoeveel gegevens worden verzonden:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met logboekanalyse](log-analytics-get-started.md) voor meer informatie over Log Analytics en ophalen van actief in minuten.
