---
title: Replicatiestatus van Active Directory met Azure Log Analytics | Microsoft Docs
description: De replicatiestatus van Active Directory-oplossingspakket controleert regelmatig uw Active Directory-omgeving voor eventuele storingen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 0ccd457295584f871088bc20864ef103648f1654
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128721"
---
# <a name="monitor-active-directory-replication-status-with-log-analytics"></a>Replicatiestatus van Active Directory met Log Analytics

![AD-replicatie statussymbool](./media/log-analytics-ad-replication-status/ad-replication-status-symbol.png)

Active Directory is een belangrijk onderdeel van een onderneming IT-omgeving. Hoge beschikbaarheid en hoge prestaties te garanderen, heeft elke domeincontroller een eigen kopie van de Active Directory-database. Domeincontrollers is gerepliceerd met elkaar om wijzigingen in het hele bedrijf doorvoeren. Fouten in dit replicatieproces kunnen een aantal problemen veroorzaken in het hele bedrijf.

De replicatiestatus van de AD-oplossingspakket controleert regelmatig uw Active Directory-omgeving voor eventuele storingen.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om de oplossing te installeren en configureren.

* U moet de agents installeren op domeincontrollers die lid van het domein zijn moet worden geëvalueerd. Of moet u agents installeren op servers die lid zijn en de agents voor het verzenden van gegevens van de AD-replicatie met logboekanalyse configureren. Zie voor informatie over Windows-computers te verbinden met Log Analytics, [verbinding maken met Windows-computers met logboekanalyse](log-analytics-windows-agent.md). Als uw domeincontroller al deel uit van een bestaande System Center Operations Manager-omgeving die u wilt verbinden met Log Analytics maakt, Zie [Operations Manager verbinden met Log Analytics](log-analytics-om-agents.md).
* De replicatiestatus van Active Directory-oplossing toevoegen aan uw werkruimte voor logboekanalyse met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.

## <a name="ad-replication-status-data-collection-details"></a>AD replicatiestatus Gegevensdetails verzameling
De volgende tabel bevat de methoden van de collectie en andere informatie over hoe gegevens worden verzameld voor de Status van de AD-replicatie.

| Platform | Agent toewijzen | SCOM-agents | Azure Storage | SCOM vereist? | SCOM-agent gegevens die worden verzonden via de beheergroep | Frequentie van de verzameling |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |elke vijf dagen |

## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-log-analytics"></a>Schakel eventueel een niet-domeincontroller AD gegevens verzenden naar Log Analytics
Als u niet wilt dat een van uw domeincontrollers rechtstreeks verbinden met Log Analytics, kunt u een andere computer in uw domein is verbonden met logboekanalyse voor het verzamelen van gegevens voor de replicatiestatus van de AD-oplossingspakket en het verzenden van de gegevens.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-log-analytics"></a>Om in te schakelen van een niet-domeincontroller AD gegevens verzenden naar Log Analytics
1. Controleer of de computer lid is van het domein dat u wilt bewaken met behulp van de replicatiestatus van de AD-oplossing.
2. [De Windows-computer verbinden met Log Analytics](log-analytics-windows-agent.md) of [verbinding maken met behulp van uw bestaande Operations Manager-omgeving met logboekanalyse](log-analytics-om-agents.md), als deze niet al is aangesloten.
3. Stel de volgende registersleutel op die computer:

   * Key: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management Groups\<ManagementGroupName>\Solutions\ADReplication**
   * Waarde: **IsTarget**
   * Waardegegevens: **true**

   > [!NOTE]
   > Deze wijzigingen worden niet doorgevoerd totdat opnieuw te starten de Microsoft Monitoring Agent-service (HealthService.exe).
   >
   >

## <a name="understanding-replication-errors"></a>Wat zijn replicatiefouten opgetreden?
Zodra u AD status replicatiegegevens verzonden naar het Log Analytics hebt, ziet u een tegel vergelijkbaar met de volgende afbeelding in die aangeeft hoeveel replicatiefouten die u momenteel hebt logboekanalyse.  
![Tegel Status van de AD-replicatie](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritieke replicatiefouten** fouten die aan of hoger dan 75% van zijn de [tombstone-levensduur](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) voor uw Active Directory-forest.

Wanneer u op de tegel klikt, kunt u meer informatie over de fouten kunt weergeven.
![AD replicatiestatus dashboard](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Doel-serverstatus en de Status van de bron-Server
Deze kolommen bevatten de status van de doelserver en bronservers waarop replicatiefouten problemen zich. Het aantal nadat de naam van elke domeincontroller geeft het aantal replicatiefouten op die domeincontroller.

De fouten voor zowel doelservers en bronservers worden weergegeven omdat sommige problemen gemakkelijker om op te lossen vanuit het perspectief van de server bron en anderen die vanuit het perspectief van de server bestemming.

In dit voorbeeld ziet u dat veel doelservers ongeveer hetzelfde aantal fouten hebben, maar er is een bronserver (ADDC35) met veel meer fouten dan alle andere. Is het waarschijnlijk dat er is een probleem op ADDC35 die wordt veroorzaakt door deze gegevens verzenden naar de replicatiepartners mislukken. De problemen op ADDC35 hersteld kan mogelijk worden opgelost veel van de fouten die worden weergegeven in het gebied van de server bestemming.

### <a name="replication-error-types"></a>Replicatie fouttypen
Dit gedeelte vindt u informatie over de verschillende typen fouten gedetecteerd in uw onderneming. Elke fout heeft een unieke numerieke code en een bericht dat kunt u de hoofdoorzaak van de fout te bepalen.

De ring boven krijgt u een idee van welke fouten meer voorkomen en minder vaak in uw omgeving.

Hier ziet u wanneer meerdere domeincontrollers dezelfde Replicatiefout optreden. In dit geval wordt u mogelijk kan detecteren of een oplossing op een domeincontroller te identificeren en herhaal vervolgens deze op andere domeincontrollers die zijn getroffen door dezelfde fout.

### <a name="tombstone-lifetime"></a>Tombstone-levensduur
De tombstone-levensduur bepaalt hoe lang een verwijderd object, aangeduid als een tombstone, wordt bewaard in de Active Directory-database. Wanneer een verwijderd object de tombstone-levensduur verstrijkt, een proces van de verzameling afval deze automatisch verwijderd uit de Active Directory-database.

De standaard tombstone-levensduur is 180 dagen voor de meest recente versies van Windows, maar was 60 dagen op oudere versies, en deze expliciet kan worden gewijzigd door een Active Directory-beheerder.

Het is belangrijk te weten als er replicatiefouten die bijna hebben bereikt of voorbij de tombstone-levensduur. Als twee domeincontrollers een replicatiefout dat behouden na de tombstone-levensduur ondervindt blijft, wordt replicatie uitgeschakeld tussen deze twee domeincontrollers, zelfs als de onderliggende Replicatiefout is opgelost.

Het gebied van de Tombstone-levensduur helpt u identificeren plaatsen waar uitgeschakelde replicatie in gevaar gebeurt is. Elke fout optreedt in de **meer dan 100% TSL** categorie vertegenwoordigt een partitie die niet is gerepliceerd tussen de bron- en doelserver server voor ten minste de tombstone-levensduur voor het forest.

In dit geval gewoon de Replicatiefout is hersteld worden niet voldoende. U moet ten minste handmatig onderzoeken om te identificeren en opschonen achtergebleven objecten voordat u kunt replicatie opnieuw. Mogelijk moet u ook een domeincontroller buiten gebruik stellen.

Naast het identificeren van replicatiefouten dat persistent is voorbij de tombstone-levensduur hebben gemaakt, wilt u ook Let op eventuele fouten in de **50 75% TSL** of **75 100% TSL** categorieën.

Dit zijn de fouten die zijn duidelijk achtergebleven, niet tijdelijk is en dus moet waarschijnlijk grijpen om op te lossen. Goed nieuws is dat ze nog niet is bereikt de tombstone-levensduur. Als u deze problemen snel oplossen en *voordat* ze de tombstone-levensduur bereikt, replicatie met minimale handmatige interventie kan opnieuw worden opgestart.

Als u eerder hebt genoteerd, de dashboardtegel voor de replicatiestatus van de AD-oplossing het aantal toont *kritieke* replicatiefouten in uw omgeving, die is gedefinieerd als fouten die meer dan 75% van de tombstone-levensduur zijn (inclusief fouten dat zijn meer dan 100% van het TSL). Willen laten dit nummer 0.

> [!NOTE]
> Alle tombstone-levensduur percentage berekeningen zijn gebaseerd op de werkelijke tombstone-levensduur voor uw Active Directory-forest, zodat u vertrouwen kunt, dat deze percentages kloppen, zelfs als u een aangepaste tombstone-levensduur-waarde ingesteld.
>
>

### <a name="ad-replication-status-details"></a>Details van status van AD-replicatie
Wanneer u een item in een van de lijsten klikt, ziet u aanvullende informatie over het logboek zoekactie. De resultaten worden gefilterd zodat alleen de fouten die zijn gerelateerd aan dat het item. Bijvoorbeeld, als u klikt u op de eerste domeincontroller vermeld onder **bestemming serverstatus (ADDC02)**, ziet u de zoekresultaten gefilterd op fouten weergeven met die domeincontroller wordt vermeld als de doelserver:

![AD-replicatiefouten status opgetreden in zoekresultaten](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Hier kunt kunt u verder te filteren, de zoekopdracht te wijzigen, enzovoort. Zie voor meer informatie over het gebruik van de zoekopdracht logboek [Meld zoekopdrachten](log-analytics-log-searches.md).

De **HelpLink** veld bevat de URL van een TechNet-pagina met aanvullende informatie over de specifieke fout. U kunt kopiëren en plakken van deze koppeling in het venster van de webbrowser voor informatie over het oplossen van problemen en de fout is hersteld.

U kunt ook klikken op **exporteren** de resultaten exporteren naar Excel. Exporteren van de gegevens, kunt u gegevens van de replicatie-fout op een manier die u wilt dat visualiseren.

![geëxporteerde AD-replicatiefouten status opgetreden in Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>Replicatiestatus AD Veelgestelde vragen
**V: hoe vaak is AD status replicatiegegevens bijgewerkt?**
A: de gegevens worden elke vijf dagen bijgewerkt.

**V: is er een manier om te configureren hoe vaak deze gegevens worden bijgewerkt?**
A: niet op dit moment.

**V: moet ik mijn domeincontrollers toevoegen aan mijn werkruimte voor logboekanalyse om te zien van de replicatiestatus?**
A: Nee, slechts één domeincontroller moet worden toegevoegd. Als u meerdere domeincontrollers in uw werkruimte voor logboekanalyse hebt, worden gegevens uit deze verzonden met logboekanalyse.

**V: ik wil niet alle domeincontrollers toevoegen aan mijn werkruimte voor logboekanalyse. Kan ik de replicatiestatus van de AD-oplossing nog steeds gebruiken?**
A: Ja. U kunt instellen dat de waarde van een registersleutel in te schakelen. Zie [om in te schakelen van een niet-domeincontroller AD gegevens verzenden naar logboekanalyse](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**V: Wat is de naam van het proces dat het verzamelen van gegevens biedt?**
A: AdvisorAssessment.exe

**V: hoe lang duurt het voor gegevens moeten worden verzameld?**
A: gegevens verzameling tijd is afhankelijk van de grootte van de Active Directory-omgeving, maar duurt normaal gesproken minder dan 15 minuten.

**V: welk type gegevens worden verzameld?**
A: replicatie gegevens worden verzameld via LDAP.

**V: is er een manier om te configureren wanneer gegevens worden verzameld?**
A: niet op dit moment.

**V: welke machtigingen heb ik nodig voor het verzamelen van gegevens?**
A: normale gebruikersmachtigingen voor Active Directory zijn voldoende.

## <a name="troubleshoot-data-collection-problems"></a>Gegevens verzamelen problemen oplossen
De replicatiestatus van de AD-oplossingspakket vereist om het verzamelen van gegevens ten minste één domeincontroller zijn verbonden met uw werkruimte voor logboekanalyse. Totdat u verbinding maakt met een domeincontroller, verschijnt een bericht dat **nog steeds worden gegevens verzameld**.

Als u verbinding maken met een van uw domeincontrollers hulp nodig hebt vindt u documentatie op [verbinding maken met Windows-computers met logboekanalyse](log-analytics-windows-agent.md). Als uw domeincontroller al met een bestaande System Center Operations Manager-omgeving verbonden is, kunt u ook documentatie op bekijken [verbinding maken met System Center Operations Manager met logboekanalyse](log-analytics-om-agents.md).

Als u niet wilt verbinding maken met een van uw domeincontrollers rechtstreeks logboekanalyse of met System Center Operations Manager, raadpleegt u [om in te schakelen van een niet-domeincontroller AD gegevens verzenden naar logboekanalyse](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-searches.md) om gedetailleerde gegevens van Active Directory-replicatie status te bekijken.
