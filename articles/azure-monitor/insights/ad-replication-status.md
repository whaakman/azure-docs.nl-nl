---
title: De replicatiestatus van de Active Directory met Azure Log Analytics controleren | Microsoft Docs
description: De Status van Active Directory-replicatie-oplossingenpakket controleert regelmatig uw Active Directory-omgeving voor replicatiefouten.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/24/2018
ms.author: magoedte
ms.openlocfilehash: c8cc6ccae59b8ee530ad679c492419a348423553
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184115"
---
# <a name="monitor-active-directory-replication-status-with-log-analytics"></a>Status van de Active Directory-replicatie met Log Analytics controleren

![Symbool van AD-replicatiestatus](./media/ad-replication-status/ad-replication-status-symbol.png)

Active Directory is een belangrijk onderdeel van een onderneming IT-omgeving. Hoge beschikbaarheid en hoge prestaties te garanderen, heeft elke domeincontroller een eigen kopie van de Active Directory-database. Domeincontrollers repliceren met elkaar om de wijzigingen worden doorgegeven binnen de hele organisatie. Fouten in dit replicatieproces kunnen leiden tot een aantal problemen binnen de hele organisatie.

Het AD-replicatiestatus-oplossingenpakket controleert regelmatig uw Active Directory-omgeving voor replicatiefouten.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om de oplossing te installeren en configureren.

* U moet agenten installeren op domeincontrollers die lid van het domein zijn moet worden geëvalueerd. Of u moet agenten installeren op lidservers en configureren van de agents AD-replicatie om gegevens te verzenden naar Log Analytics. Zie voor meer informatie over hoe u Windows-computers verbinden met Log Analytics, [verbinding maken met Windows-computers naar Log Analytics](../../azure-monitor/platform/agent-windows.md). Als uw domeincontroller al deel van een bestaande System Center Operations Manager-omgeving die u wilt koppelen aan Log Analytics uitmaakt, Zie [Operations Manager verbinden met Log Analytics](../../azure-monitor/platform/om-agents.md).
* De oplossing Active Directory Replication Status toevoegen aan uw Log Analytics-werkruimte met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen uit de galerie van oplossingen](../../azure-monitor/insights/solutions.md).  Er is geen verdere configuratie nodig.

## <a name="ad-replication-status-data-collection-details"></a>Details van AD-replicatiestatus gegevens verzamelen
De volgende tabel bevat de methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor AD-replicatiestatus.

| Platform | Agent toewijzen | SCOM-agents | Azure Storage | SCOM vereist? | SCOM-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |elke vijf dagen |

## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-log-analytics"></a>Schakel eventueel een niet-domeincontroller AD-gegevens te verzenden naar Log Analytics
Als u niet wilt dat uw domeincontrollers rechtstreeks verbinden met Log Analytics, kunt u een andere computer in uw domein verbonden met Log Analytics gebruiken voor het verzamelen van gegevens voor het AD-replicatiestatus-oplossingenpakket en het verzenden van de gegevens.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-log-analytics"></a>Om in te schakelen van een niet-domeincontroller AD-gegevens te verzenden naar Log Analytics
1. Controleer of de computer lid is van het domein dat u wilt bewaken met behulp van de AD-replicatiestatus-oplossing.
2. [De Windows-computer verbinden met Log Analytics](../../azure-monitor/platform/om-agents.md) of [verbinding maken met behulp van uw bestaande Operations Manager-omgeving naar Log Analytics](../../azure-monitor/platform/om-agents.md), als deze nog niet is verbonden.
3. Stel de volgende registersleutel op die computer:

   * Sleutel: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management groepen\<ManagementGroupName > \Solutions\ADReplication**
   * Waarde: **IsTarget**
   * Waardegegevens: **true**

   > [!NOTE]
   > Deze wijzigingen kunnen pas van kracht opnieuw te starten de service Microsoft Monitoring Agent (HealthService.exe).
   >
   >

## <a name="understanding-replication-errors"></a>Wat zijn replicatiefouten opgetreden?
Zodra u AD status replicatiegegevens verzonden naar Log Analytics hebt, ziet u een tegel die vergelijkbaar is met de volgende afbeelding in Log Analytics, waarmee wordt aangegeven hoeveel replicatiefouten u momenteel hebt.  
![Tegel AD-replicatiestatus](./media/ad-replication-status/oms-ad-replication-tile.png)

**Kritieke replicatiefouten** fouten die aan of hoger dan 75% van zijn de [tombstone-levensduur](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) voor uw Active Directory-forest.

Wanneer u op de tegel klikt, kunt u meer informatie over de fouten bekijken.
![Dashboard voor AD-replicatiestatus](./media/ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Status van doelserver en de Status van bronserver
Deze kolommen bevatten de status van doelservers en problemen met replicatie zijn bronservers. Het getal na elke naam van de domeincontroller geeft het aantal replicatiefouten van die domeincontroller.

De fouten voor zowel de doelservers als de bronservers worden weergegeven omdat sommige problemen zijn eenvoudiger is om op te lossen van het perspectief van de server bron en anderen vanuit het perspectief van de server bestemming.

In dit voorbeeld ziet u dat veel doelservers ongeveer hetzelfde aantal fouten hebben, maar er is een bronserver (ADDC35), die veel fouten meer zijn dan alle anderen heeft. Is het waarschijnlijk dat er een probleem is opgetreden op ADDC35 waardoor deze niet kunnen gegevens verzenden naar de replicatiepartners. Oplossen van de problemen op ADDC35 mogelijk oplossen veel van de fouten die worden weergegeven in het gebied van de doel-server.

### <a name="replication-error-types"></a>Typen voor replicatiefouten
Dit gedeelte vindt u informatie over de typen fouten die zijn gedetecteerd in uw onderneming. Elke fout heeft een unieke numerieke code en een bericht dat kunt u de hoofdoorzaak van de fout te bepalen.

De ring aan de bovenkant biedt u een idee van welke fouten voorkomen en minder vaak in uw omgeving.

Hier ziet u wanneer meerdere domeincontrollers dezelfde Replicatiefout optreden. In dit geval u mogelijk kunnen detecteren of een oplossing op één domeincontroller worden geïdentificeerd en herhaal vervolgens deze op andere domeincontrollers beïnvloed door dezelfde fout.

### <a name="tombstone-lifetime"></a>Tombstone-levensduur
De tombstone-levensduur bepaalt hoe lang een verwijderd object, aangeduid als een tombstone, wordt bewaard in de Active Directory-database. Wanneer u een verwijderd object de tombstone-levensduur is verstreken, wordt deze in het verzamelingsproces voor een garbagecollection automatisch verwijderd uit de Active Directory-database.

De standaard tombstone-levensduur is 180 dagen voor de meest recente versies van Windows, maar deze is 60 dagen op oudere versies en deze expliciet kan worden gewijzigd door een Active Directory-beheerder.

Het is belangrijk te weten als u ondervindt replicatiefouten die bijna zijn bereikt of voorbij de tombstone-levensduur. Als twee domeincontrollers een replicatiefout die zich blijft na de tombstone-levensduur ondervindt voordoen, is replicatie tussen deze twee domeincontrollers, uitgeschakeld, zelfs als de onderliggende Replicatiefout is opgelost.

Het gebied van Tombstone-levensduur kunt u identificeren locaties waar uitgeschakelde replicatie in gevaar voor gebeurt is. Elke fout in de **meer dan 100% TSL** categorie vertegenwoordigt een partitie die nog niet gerepliceerd tussen de bron en doel-server voor ten minste de tombstone-levensduur voor het forest.

In dit geval zich gewoon vaststelling van de Replicatiefout niet voldoende. U moet ten minste handmatig onderzoeken om te identificeren en het opschonen van achtergebleven objecten voordat u replicatie opnieuw kunt starten. U moet mogelijk ook een domeincontroller buiten gebruik stellen.

Naast het identificeren van replicatiefouten dat voorbij de tombstone-levensduur hebben geduurd, wilt u ook aandacht te besteden aan eventuele fouten die vallen onder de **50 75% TSL** of **75-100% TSL** categorieën.

Dit zijn de fouten die duidelijk achtergebleven, niet tijdelijk is en daarom moeten ze waarschijnlijk uw tussenkomst van de op te lossen. Het goede nieuws is dat ze nog niet is bereikt de tombstone-levensduur. Als u deze problemen snel oplossen en *voordat* ze zich bij de tombstone-levensduur, replicatie opnieuw kunt starten met zo weinig mogelijk handmatige tussenkomst.

Als u eerder hebt genoteerd, de dashboardtegel voor de AD-replicatiestatus-oplossing het aantal toont *kritieke* replicatiefouten in uw omgeving, dat is gedefinieerd als fouten die meer dan 75% van de tombstone-levensduur (met inbegrip van fouten dat zijn meer dan 100% van de TSL). Streven ernaar om dit nummer houden op 0.

> [!NOTE]
> Alle tombstone-levensduur percentage berekeningen zijn gebaseerd op de werkelijke tombstone-levensduur voor uw Active Directory-forest, zodat u vertrouwen kunt dat deze percentages juist zijn, zelfs als u de ingestelde waarde voor een aangepaste tombstone-levensduur hebben.
>
>

### <a name="ad-replication-status-details"></a>Details van status van AD-replicatie
Wanneer u een item in een van de lijsten op klikt, ziet u aanvullende informatie over het gebruik van zoeken in Logboeken. De resultaten worden gefilterd om alleen de fouten met betrekking tot dat item weer te geven. Bijvoorbeeld, als u klikt op de eerste domeincontroller vermeld onder **Status van doelserver (ADDC02)**, ziet u de zoekresultaten gefilterd op fouten weergeven met die domeincontroller wordt vermeld als de doelserver:

![AD-status replicatiefouten opgetreden in de zoekresultaten](./media/ad-replication-status/oms-ad-replication-search-details.png)

Hier kunt kunt u verder filteren, wijzigen van de zoekopdracht, enzovoort. Zie voor meer informatie over het gebruik van zoeken in logboeken [zoekopdrachten](../../azure-monitor/log-query/log-query-overview.md).

De **HelpLink** veld ziet u de URL van een TechNet-pagina met aanvullende informatie over de specifieke fout. U kunt kopiëren en plakken van deze koppeling in het browservenster om informatie over het oplossen van problemen en het verhelpen van de fout te bekijken.

U kunt ook klikken op **exporteren** de resultaten exporteren naar Excel. Exporteren van de gegevens, kunt u gegevens van de replicatie-fout op een manier die u wilt visualiseren.

![geëxporteerde AD-replicatie status fouten in Excel](./media/ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>Veelgestelde vragen over AD-replicatiestatus
**VRAAG: Hoe vaak wordt AD status replicatiegegevens bijgewerkt?**
A: De gegevens worden elke vijf dagen bijgewerkt.

**VRAAG: Is er een manier om te configureren hoe vaak deze gegevens worden bijgewerkt?**
A: Momenteel niet.

**VRAAG: Moet ik mijn domeincontrollers toevoegen aan mijn Log Analytics-werkruimte als u wilt bekijken van de replicatiestatus?**
A: Nee, slechts één domeincontroller moet worden toegevoegd. Als u meerdere domeincontrollers in uw Log Analytics-werkruimte hebt, worden gegevens van al deze wordt verzonden naar Log Analytics.

**VRAAG: Ik wil niet alle domeincontrollers toevoegen aan mijn werkruimte voor logboekanalyse. Kan ik nog steeds de AD-replicatiestatus-oplossing gebruiken?**
A: Ja. U kunt de waarde van een registersleutel te kunnen instellen. Zie [om in te schakelen van een niet-domeincontroller AD-gegevens te verzenden naar Log Analytics](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**VRAAG: Wat is de naam van het proces dat het verzamelen van gegevens wordt?**
A: AdvisorAssessment.exe

**VRAAG: Hoe lang duurt het voordat gegevens worden verzameld?**
A: Gegevens verzameling tijd is afhankelijk van de grootte van de Active Directory-omgeving, maar duurt normaal gesproken minder dan 15 minuten.

**VRAAG: Welk type gegevens worden verzameld?**
A: Replicatiegegevens worden verzameld via in LDAP.

**VRAAG: Is er een manier om te configureren wanneer gegevens worden verzameld?**
A: Momenteel niet.

**VRAAG: Welke machtigingen heb ik nodig voor het verzamelen van gegevens?**
A: Normale gebruikersmachtigingen voor Active Directory zijn voldoende.

## <a name="troubleshoot-data-collection-problems"></a>Problemen met gegevens verzamelen
Als u wilt verzamelen van gegevens, moet het AD-replicatiestatus-oplossingenpakket ten minste één domeincontroller worden verbonden met uw Log Analytics-werkruimte. Totdat u verbinding maakt met een domeincontroller, er een bericht weergegeven dat aangeeft dat **nog steeds gegevens worden verzameld**.

Als u hulp bij het verbinden van een van uw domeincontrollers, vindt u de documentatie bij [verbinding maken met Windows-computers naar Log Analytics](../../azure-monitor/platform/om-agents.md). Als uw domeincontroller al met een bestaande System Center Operations Manager-omgeving verbonden is, kunt u ook documentatie bij bekijken [System Center Operations Manager verbinden met Log Analytics](../../azure-monitor/platform/om-agents.md).

Als u niet wilt verbinding maken met een van uw domeincontrollers rechtstreeks Log Analytics of met System Center Operations Manager, raadpleegt u [om in te schakelen van een niet-domeincontroller AD-gegevens te verzenden naar Log Analytics](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten in Logboeken in Log Analytics](../../azure-monitor/log-query/log-query-overview.md) om gedetailleerde gegevens van Active Directory-replicatie de status weer te geven.
