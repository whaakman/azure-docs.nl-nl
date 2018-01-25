---
title: Oplossingen voor het beheer van Azure-logboekanalyse toevoegen | Microsoft Docs
description: Operations Management Suite (OMS) / Log Analytics beheeroplossingen zijn een verzameling logica, visualiseren en gegevens overname regels waarmee de metrische gegevens om een bepaald probleemgebied wordt gedraaid.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d951387882a5a8f5e0ebdc01841bb8384e4848ee
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="add-azure-log-analytics-management-solutions-to-your-workspace"></a>Oplossingen voor het beheer van Azure-logboekanalyse toevoegen aan uw werkruimte

Log Analytics-oplossingen voor beheer zijn een verzameling **logica**, **visualisatie**, en **gegevens overname regels** die metrische gegevens om een bepaalde gedraaid bieden probleemgebied. In dit artikel bevat oplossingen voor het beheer wordt ondersteund door Log Analytics en ziet u hoe toevoegen en verwijderen voor een werkruimte met behulp van de Azure-portal. U kunt ook oplossingen toevoegen in de OMS-portal met behulp van de galerie met oplossingen.

Oplossingen voor toestaan meer inzicht te:

* Te onderzoeken en oplossen van operationele problemen sneller
* Verzamelen en verschillende soorten gegevens van de machine correleren
* Kunt u actie moet ondernemen met activiteiten die de oplossing beschrijft.

> [!NOTE]
> Log Analytics bevat logboek zoekfunctionaliteit, dus u hoeft niet te installeren van een beheeroplossing voor het inschakelen. Echter, krijgt u gegevensvisualisaties, voorgestelde zoekopdrachten en inzichten door beheersystemen toe te voegen aan uw werkruimte.

In dit artikel gebruikt, toevoegen u beheeroplossingen aan een werkruimte met de Azure Marketplace-portal. Nadat u een oplossing hebt toegevoegd, worden gegevens verzameld van de servers in uw infrastructuur en verzonden naar de OMS-service. Verwerkt door de OMS duurt service doorgaans een paar minuten een uur. Nadat de gegevens worden verwerkt door de service, kunt u deze bekijken in OMS.

Wanneer deze niet langer nodig is, kunt u eenvoudig een oplossing voor het beheer verwijderen. Wanneer u een oplossing voor het beheer verwijdert, worden de gegevens worden niet verzonden met OMS. Als u zich op de prijscategorie gratis, kan verwijderen van een oplossing voor de hoeveelheid gegevens die worden gebruikt, zodat u kunt blijven onder dagelijkse quotum van gegevens verminderen.

## <a name="view-available-management-solutions"></a>Weergave beschikbaar oplossingen

De Azure marketplace bevat de lijst met [beheeroplossingen voor logboekanalyse](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

U kunt beheeroplossingen vanuit Azure marketplace installeren door te klikken op de **nu downloaden** koppeling aan de onderkant van elke oplossing.

## <a name="add-a-management-solution"></a>Toevoegen van een beheeroplossing
1. Meld u met uw Azure-abonnement aan bij [Azure Portal](https://portal.azure.com) als u dit nog niet hebt gedaan.
2. In de **nieuw** blade onder **Marketplace**, selecteer **bewaking + management**.
3. In de **bewaking + management** blade, klikt u op **alle**.  
    ![Bewaking + beheerblade](./media/log-analytics-add-solutions/monitoring-management-blade.png)  
4. Aan de rechterkant van **beheeroplossingen**, klikt u op **meer**.
5. In de **beheeroplossingen** blade, selecteert u een oplossing die u wilt toevoegen aan een werkruimte.  
    ![Bewaking + beheerblade](./media/log-analytics-add-solutions/management-solutions.png)  
6. Lees de informatie over de oplossing voor beheer op de blade van de oplossing voor beheer en klik vervolgens op **maken**.
7. Selecteer op de blade *Naam beheeroplossing* de werkruimte die u aan de beheeroplossing wilt koppelen.
8. Wijzig eventueel de werkruimte-instellingen voor de Azure-abonnement, resourcegroep en locatie. U kunt ook **automatiseringsopties**. Klik op **Create**.  
    ![werkruimte voor de oplossing](./media/log-analytics-add-solutions/solution-workspace.png)  
9. Om te starten met behulp van de beheeroplossing die u hebt toegevoegd aan uw werkruimte, gaat u naar **logboekanalyse** > **abonnementen** > ***Werkruimtenaam***  >  **Overzicht**. Een nieuwe tegel voor uw beheeroplossing wordt weergegeven. Klik op de tegel te openen en het aan de slag met de oplossing nadat de gegevens voor de oplossing zijn verzameld.

## <a name="remove-a-management-solution"></a>Een oplossing voor het beheer verwijderen

1. In de [Azure-portal](https://portal.azure.com), gaat u naar **logboekanalyse** > **abonnementen** > ***Werkruimtenaam*** en Klik in de ***Werkruimtenaam*** blade, klikt u op **oplossingen**.
2. Selecteer in de lijst met oplossingen voor het beheer, de oplossing die u wilt verwijderen.
3. Klik op de blade oplossing voor uw werkruimte **verwijderen**.  
    ![oplossing verwijderen](./media/log-analytics-add-solutions/solution-delete.png)  
4. Klik in het bevestigingsvenster **Ja**.

## <a name="offers-and-pricing-tiers"></a>Aanbiedingen en Prijscategorieën

De volgende tabel identificeert die deel uitmaken van oplossingen voor elke aanbieding Operations Management Suite.
De tabel identificeert ook de Prijscategorieën die beschikbaar voor elk beheersysteem zijn.
Alle oplossingen in de volgende tabel zijn beschikbaar vanuit de Azure portal en de galerie met oplossingen in de Log Analytics-portal.

| Oplossing voor het beheer                                                                       | Aanbieding                                                                     | Prijscategorieën<sup>1</sup>                                                 | Opmerkingen |
| ---                                                                                       | ---                                                                       | ---                                                                                                       | ---   |
| [Activity Log Analytics](log-analytics-activity.md)                                                                   | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | negentig dagen aan gegevens beschikbaar zijn gratis<br>Gegevens niet onderworpen aan het kapje gratis laag |
| [AD-evaluatie](log-analytics-ad-assessment.md)                                           | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [AD-replicatiestatus](log-analytics-ad-replication-status.md)                           | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [Status van agent](../operations-management-suite/oms-solution-agenthealth.md)                                                                                | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | Gegevens niet onderworpen aan het kapje gratis laag<br> Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [Waarschuwingsbeheer](log-analytics-solution-alert-management.md)                            | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [Application Insights-Connector (Preview)](log-analytics-app-insights-connector.md)                                               | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Automation Hybrid Worker](../automation/automation-hybrid-runbook-worker.md)                                                                     | <ul><li>Automation en besturing</li></ul>                                  | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                         | Uw werkruimte voor logboekanalyse moet worden gekoppeld aan een Automation-account vereist |
| [Azure Application Gateway Analytics](log-analytics-azure-networking-analytics.md)    | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Azure Network Security Group Analytics](log-analytics-azure-networking-analytics.md)     | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Azure SQL Analytics (Preview)](log-analytics-azure-sql.md)                                                       | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br>Per&nbsp;knooppunt&nbsp;(OMS)                                                                          | Uw werkruimte voor logboekanalyse moet worden gekoppeld aan een Automation-account vereist|
| [Azure Web Apps-analyse](log-analytics-azure-web-apps-analytics.md)     | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
|[Een back-up maken](../backup/backup-introduction-to-azure-backup.md)                                                                                 | <ul><li>Inzicht en analyses</li></ul>                                   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                       | Vereist een klassieke Backup-kluis.<br> Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [Capaciteit en prestaties (Preview)](log-analytics-capacity.md)                                                   | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Tracering wijzigen](log-analytics-change-tracking.md)                                       | <ul><li>Automation en besturing</li></ul>                                  | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                         | Uw werkruimte voor logboekanalyse moet worden gekoppeld aan een Automation-account vereist |
| [Containers](log-analytics-containers.md)                                                 | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [IT Service Management Connector](log-analytics-itsmc-overview.md)                                                | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)     | |
| HDInsight HBase Monitoring <br>(Preview)                                                  | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Key Vault-analyse](log-analytics-azure-key-vault.md)                   | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Logic Apps B2B](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)                    | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [Malware-evaluatie](log-analytics-malware.md)                                            | <ul><li>Beveiliging en naleving</li></ul>                                 | Gratis<br> Zelfstandig<br>Per&nbsp;knooppunt&nbsp;(OMS)                                                                           | Als u de beveiliging en naleving oplossingen na 19 juni 2017 toevoegt [facturering per knooppunt is](https://azure.microsoft.com/pricing/details/security-compliance/), ongeacht de prijscategorie werkruimte. De eerste 60 dagen zijn gratis.  |
| [Netwerkprestatiemeter](log-analytics-network-performance-monitor.md) <br>  | <ul><li>Inzicht en analyses</li></ul>                                   | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                         | |
| [Office 365 Analytics (Preview)](../operations-management-suite/oms-solution-office-365.md)                                                       | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Beveiliging en controle](../operations-management-suite/oms-security-getting-started.md)      | <ul><li>Beveiliging&nbsp;en&nbsp;naleving</li></ul>                       | Gratis<br> Zelfstandig<br>Per&nbsp;knooppunt&nbsp;(OMS)                                                                           | Deze oplossing vereist voor het verzamelen van gebeurtenislogboeken<br>Als u de beveiliging en naleving oplossingen na 19 juni 2017 toevoegt [facturering per knooppunt is](https://azure.microsoft.com/pricing/details/security-compliance/), ongeacht de prijscategorie werkruimte. De eerste 60 dagen zijn gratis. |
| [Service Fabric Analytics (Preview)](log-analytics-service-fabric.md)                     | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Serviceoverzicht (Preview)](../operations-management-suite/operations-management-suite-service-map.md) | <ul><li>Inzicht en analyses</li></ul>                      | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                         | Beschikbaar in VS-Oost, West-Europa en West-Centraal VS    |
| [Site Recovery](../site-recovery/site-recovery-overview.md)                                                                               | <ul><li>Inzicht en analyses</li></ul>                                   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                       | Vereist een klassieke Site Recovery-kluis.<br> Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [SQL-evaluatie](log-analytics-sql-assessment.md)                                         | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| VM's starten/stoppen buiten kantooruren<br>(Preview)                                              | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                         | Uw werkruimte voor logboekanalyse moet worden gekoppeld aan een Automation-account vereist |
| [SurfaceHub](log-analytics-surface-hubs.md)                                               | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [System Center Operations Manager Assessment (Preview)](log-analytics-scom-assessment.md)  | <ul><li>Inzicht en analyses</li><li>Log Analytics</li></ul>        | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Updatebeheer](../operations-management-suite/oms-solution-update-management.md)                                                                         | <ul><li>Automation en besturing</li></ul>                                  | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                         | Uw werkruimte voor logboekanalyse moet worden gekoppeld aan een Automation-account vereist |
| [Updatevereisten (Preview)](https://docs.microsoft.com/windows/deployment/update/update-compliance-get-started)                                                             | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | Geen kosten voor gegevens of knooppunten<br>De gegevens niet onderworpen aan het kapje gratis laag.<br> Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [Gereedheid voor upgrade](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started)                                                          | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | Geen kosten voor gegevens of knooppunten<br>De gegevens niet onderworpen aan het kapje gratis laag.<br> Niet beschikbaar vanuit Azure portal/marketplace toevoegen. |
| [VMware Monitoring (Preview)](log-analytics-vmware.md)                                | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Standard<br> Premium&nbsp;(OMS)<br> Per&nbsp;GB&nbsp;(zelfstandig)<br> Per&nbsp;knooppunt&nbsp;(OMS)   | |
| [Draadgegevens worden geleverd 2.0 (Preview)](log-analytics-wire-data.md)                                                                 | <ul><li>Inzicht en analyses</li></ul>                                   | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)                                                                         | Beschikbaar in VS-Oost, West-Europa en West-Centraal VS |

<sup>1</sup> de *standaard* en *Premium (OMS)* Prijscategorieën zijn alleen beschikbaar voor klanten die hun werkruimte voor logboekanalyse vóór 21 September 2016 gemaakt.

### <a name="community-provided-management-solutions"></a>Opgegeven beheeroplossingen community

Community opgegeven oplossingen beschikbaar zijn op de [Azure sjablonengalerie](https://azure.microsoft.com/resources/templates/?term=Per&nbsp;Node&nbsp;(OMS)) en direct van de auteurs.

| Oplossing voor het beheer               | Aanbieding                                                                     | Prijscategorieën                         | Opmerkingen |
| ---                               | ---                                                                       | ---                                   | ---   |
| Alle community opgegeven oplossingen  | <ul><li>Inzicht&nbsp;en&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Gratis<br> Per&nbsp;knooppunt&nbsp;(OMS)     |   Uw werkruimte voor logboekanalyse moet worden gekoppeld aan een Automation-account vereist |




## <a name="data-collection-details"></a>Details van de verzameling gegevens
De volgende tabellen geven de methoden van de collectie en andere informatie over hoe gegevens worden verzameld van oplossingen voor het beheer van logboekanalyse en gegevensbronnen. De tabellen worden gecategoriseerd door de oplossing biedt, die kan worden vergeleken met [abonnement Prijscategorieën](https://go.microsoft.com/fwlink/?linkid=827926). De activiteit Log Analytics-oplossing is beschikbaar voor alle Prijscategorieën gratis.

De agent Log Analytics Windows en System Center Operations Manager-agent zijn in wezen hetzelfde. De Windows-agent bevat aanvullende functionaliteit, zodat deze verbinding maakt met de OMS-werkruimte en routeren via een proxy. Als u een Operations Manager-agent gebruikt, moet u deze als een OMS-agent om te communiceren met OMS gericht. Operations Manager-agenten in deze tabel zijn de OMS-agents die zijn verbonden met Operations Manager. Zie [Operations Manager verbinden met Log Analytics](log-analytics-om-agents.md) voor informatie over het verbinden van uw bestaande omgeving voor Operations Manager met OMS.

> [!NOTE]
> Het type van de agent waarmee u bepaalt hoe gegevens worden verzonden naar OMS, met de volgende voorwaarden:
> - U gebruikmaken van de Windows-agent of een Operations Manager-attached OMS-agent.
> - Als Operations Manager vereist is, wordt Operations Manager-agent gegevens voor de oplossing altijd verzonden naar OMS met behulp van de Operations Manager-beheergroep. Bovendien, als Operations Manager vereist is, wordt alleen de Operations Manager-agent gebruikt door de oplossing.
> - Wanneer Operations Manager is niet vereist en de tabel worden de gegevens van die Operations Manager-agent wordt verzonden naar de beheergroep met OMS en vervolgens de gegevens in Operations Manager-agent wordt altijd verzonden naar de OMS-beheergroepen met. Windows-agents overslaan van de beheergroep en verzenden van hun gegevens rechtstreeks aan OMS.
> - Wanneer gegevens in Operations Manager-agent niet met behulp van een beheergroep verzonden is, klikt u vervolgens de gegevens worden verzonden rechtstreeks met OMS: het omzeilen van de beheergroep.

### <a name="insight--analytics--log-analytics"></a>Inzicht & Analytics / Log Analytics

| Oplossing voor het beheer | Platform | Microsoft monitoring agent | Operations Manager-agent | Azure Storage | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Analyse van activiteitenlogboek | Azure |   |   |   |   |   | op de melding |
| AD-evaluatie |Windows |&#8226; |&#8226; |  |  |&#8226; |7 dagen |
| AD-replicatiestatus |Windows |&#8226; |&#8226; |  |  |&#8226; |Er zijn vijf dagen |
| Status van agent | Windows en Linux | &#8226; | &#8226; |   |   | &#8226; | 1 minuut |
| Beheer van waarschuwingen (Nagios) |Linux |&#8226; |  |  |  |  |bij ontvangst |
| Beheer van waarschuwingen (Zabbix) |Linux |&#8226; |  |  |  |  |1 minuut |
| Beheer van waarschuwingen (Operations Manager) |Windows |  |&#8226; |  |&#8226; |&#8226; |3 minuten |
| Application Insights-Connector (Preview) | Azure |   |   |   |   |   | op de melding |
| Azure Application Gateway Analytics | Azure |   |   |   |   |   | op de melding |
| Netwerkbeveiligingsgroep Azure Analytics | Azure |   |   |   |   |   | op de melding |
| Azure SQL Analytics (Preview) |Windows |  |  |  |  |  | 10 minuten |
| Capaciteitsbeheer |Windows |&#8226; |&#8226; |  |  |&#8226; |bij ontvangst |
| Containers | Windows en Linux | &#8226; | &#8226; |   |   |   | 3 minuten |
| Sleutelkluis Analytics |Windows |  |  |  |  |  |op de melding |
| Netwerkprestatiemeter | Windows | &#8226; | &#8226; |   |   |   | TCP-handshakes om de vijf seconden gegevens verzonden om de 3 minuten |
| Office 365 Analytics (Preview) |Windows |  |  |  |  |  |op de melding |
| Service Fabric Analytics |Windows |  |  |&#8226; |  |  |5 minuten |
| Serviceoverzicht | Windows en Linux | &#8226; | &#8226; |   |   |   | 15 seconden |
| SQL-evaluatie |Windows |&#8226; |&#8226; |  |  |&#8226; |7 dagen |
| SurfaceHub |Windows |&#8226; |  |  |  |  |bij ontvangst |
| System Center Operations Manager Assessment (Preview) | Windows | &#8226; | &#8226; |   |   | &#8226; | zeven dagen |
| Upgrade Analytics (Preview) | Windows | &#8226; |   |   |   |   | 2 dagen |
| VMware Monitoring (Preview) | Linux | &#8226; |   |   |   |   | 3 minuten |
| Bedradingsgegevens |Windows (2012 R2 / 8.1 of hoger) |&#8226; |&#8226; |  |  |  | 1 minuut |


### <a name="automation--control"></a>Automation en besturing

| Oplossing voor het beheer | Platform | Microsoft monitoring agent | Operations Manager-agent | Azure Storage | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Automation Hybrid Worker | Windows | &#8226; | &#8226; |   |   |   | N.v.t. |
| Tracering wijzigen |Windows |&#8226; |&#8226; |  |  |&#8226; |elk uur |
| Tracering wijzigen |Linux |&#8226; |  |  |  |  |elk uur |
| Updatebeheer | Windows |&#8226; |&#8226; |  |  |&#8226; |ten minste 2 keer per dag en 15 minuten nadat u een update installeert |

### <a name="security--compliance"></a>Beveiliging en naleving

| Oplossing voor het beheer | Platform | Microsoft monitoring agent | Operations Manager-agent | Azure Storage | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Evaluatie van Antimalware |Windows |&#8226; |&#8226; |  |  |&#8226; |elk uur |
| Beveiliging en Audit<sup>1</sup> | Windows en Linux | gedeeltelijke | gedeeltelijke | gedeeltelijke |   | gedeeltelijke | verschillende |

<sup>1</sup> oplossing voor de beveiliging en controle kunt verzamelen van Logboeken van Windows-, Operations Manager- en Linux-agents. Zie [gegevensbronnen](#data-sources) voor gegevens verzamelen informatie over:

- Syslog
- Windows-beveiligingslogboek
- Windows firewall-Logboeken
- Windows-gebeurtenislogboeken



### <a name="protection--recovery"></a>Beveiliging en herstel

| Oplossing voor het beheer | Platform | Microsoft monitoring agent | Operations Manager-agent | Azure Storage | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Back-up maken | Azure |   |   |   |   |   | N.v.t. |
| Azure Site Recovery | Azure |   |   |   |   |   | N.v.t. |


### <a name="data-sources"></a>Gegevensbronnen


| Gegevensbron | Platform | Microsoft monitoring agent | Operations Manager-agent | Azure Storage | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Azure activiteitenlogboeken |Windows |  |  |  |  |  |op de melding |
| Azure diagnostische logboeken |Windows |  |  |  |  |  |op de melding |
| Azure diagnostische metrische gegevens |Windows |  |  |  |  |  |op de melding |
| ETW |Windows |  |  |&#8226; |  |  |5 minuten |
| IIS-logboeken |Windows |&#8226; |&#8226; |&#8226; |  |  |5 minuten |
| Prestatiemeteritems |Windows |&#8226; |&#8226; |  |  |  |Als gepland, minimaal 10 seconden |
| Prestatiemeteritems |Linux |&#8226; |  |  |  |  |Als gepland, minimaal 10 seconden |
| Syslog |Linux |&#8226; |  |  |  |  |naar Azure storage: 10 minuten. Agent: bij ontvangst |
| Windows-beveiligingslogboek |Windows |&#8226; |&#8226; |&#8226; |  |  |voor Azure storage: 10 minuten; voor de agent: bij ontvangst |
| Windows firewall-Logboeken |Windows |&#8226; |&#8226; |  |  |  |bij ontvangst |
| Windows-gebeurtenislogboeken |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; |voor Azure storage: 10 minuten; voor de agent: bij ontvangst |



## <a name="preview-management-solutions-and-features"></a>Preview oplossingen en functies
Door een service wordt uitgevoerd en devops procedures te volgen, kunnen we samenwerken met klanten voor het ontwikkelen van functies en oplossingen.

Tijdens de Preview-versie private geven wij een kleine groep klanten toegang tot een eerdere implementatie van de functie of een oplossing voor feedback krijgen en verbeteringen. Deze vroege implementatie heeft minimale functies en operationele mogelijkheden.

Ons doel is om te proberen dingen snel zodat we vinden kunnen wat werkt en wat niet werkt. We doorlopen dit proces totdat de feedback van de private preview-klanten ons meldt dat we klaar zijn voor een openbare preview.

Tijdens de openbare preview maken we de functie of een oplossing beschikbaar voor alle gebruikers meer feedback ophalen en valideren van onze schalen en efficiëntie. Tijdens deze fase:

* Preview-functies weergegeven in het tabblad instellingen en kunnen worden ingeschakeld door een gebruiker.
* Preview-oplossingen worden toegevoegd door de galerie of met behulp van een script.

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Wat moet ik weten over de Preview-functies en oplossingen
We zijn blij met nieuwe functies en oplossingen voor het beheer en we graag werken met kunt ontwikkelen.

Zijn niet geschikt is voor iedereen Preview-functies en oplossingen. Voordat een private preview lid of het inschakelen van een openbare preview, zorg ervoor dat werkt u OK met iets dat nog in ontwikkeling.

Bij het inschakelen van een preview-functie via de portal ziet u een waarschuwing om eraan te herinneren dat de functie is een Preview-versie.

#### <a name="for-both-private-and-public-preview"></a>Voor beide *persoonlijke* en *openbare* preview
De volgende informatie is van toepassing op openbare en persoonlijke previews:

* Dingen werken niet altijd correct.
  * Bereik van een secundaire hinder via op iets niet werkt op alle wordt verstrekt.
* Er is mogelijk voor het voorbeeld om een negatieve invloed hebben op uw systemen / omgeving.
  * We proberen om te voorkomen dat negatieve dingen plaatsvinden naar de systemen u met OMS, maar soms onverwachte dingen optreden.
* Gegevensverlies / beschadigd raken.
* Kan vragen we u voor het verzamelen van diagnostische logboeken of andere gegevens problemen op te lossen.
* De functie of oplossing kan worden verwijderd (tijdelijk of permanent).
  * Op basis van onze geleerde lessen tijdens de preview die we besluiten de functie of de oplossing niet vrijgeven.
* Voorbeelden werkt mogelijk niet of niet getest met alle configuraties en we kunnen beperken:
  * De besturingssystemen die kunnen worden gebruikt (bijvoorbeeld, een functie kan alleen van toepassing op Linux in de preview).
  * Het type van de agent (MMA, Operations Manager) die kan worden gebruikt (bijvoorbeeld, een functie werkt niet met Operations Manager in de preview).  
* Preview-oplossingen en functies zijn niet wordt gedekt door de Service Level Agreement.
* Gebruikskosten leidt ertoe dat gebruik van de preview-functies.
* Functies en mogelijkheden die u nodig hebt voor de functie / oplossing nuttig mogelijk ontbreken of zijn onvolledig.
* Functies / oplossingen mogelijk niet beschikbaar in alle regio's.
* Functies / oplossingen kunnen niet worden gelokaliseerd.
* Functies / oplossingen mogelijk een limiet van het aantal klanten of apparaten die kunnen worden gebruikt.
* Wellicht moet u scripts gebruiken om te configureren en inschakelen van de oplossing of onderdeel.
* De gebruikersinterface (UI) is niet volledig en dag kan worden gewijzigd.
* Openbare previews mogelijk niet geschikt is voor uw productie / kritieke systemen.

#### <a name="for-private-preview"></a>Voor *persoonlijke* preview
Naast de bovenstaande items zijn de volgende informatie is specifiek voor persoonlijke previews:

* We verwachten dat u om te voorzien van ons feedback over uw ervaringen zodat we de functie of oplossing kunt verbeteren.
* We kunnen contact met u voor feedback met enquêtes, telefoongesprekken of e-mail.
* Dingen werken niet altijd goed.
* We kunnen vertrouwelijke inhoud bevatten of mogelijk niet vrijgeven overeenkomst (geheimhoudingsverklaring) voor deelname.
  * Voordat u weblog, tweeting of anders communicatie met externe partijen, neem contact op met de beheerder van het programma dat verantwoordelijk is voor het voorbeeld om de beperkingen op openbaarmaking.
* Niet worden uitgevoerd voor productie- / kritieke systemen.

### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Hoe krijg ik toegang tot persoonlijke preview-functies en oplossingen
We nodigen klanten persoonlijke previews via verschillende manieren, afhankelijk van de Preview-versie.

* Het beantwoorden van de maandelijkse enquête en geeft ons toestemming voor het opvolgen met u verbetert de kans op worden uitgenodigd voor een private preview.
* Uw Microsoft-accountteam hebt ontvangen, kunt u wijst.
* U kunt zich aanmelden op basis van details gepost op twitter [msopsmgmt](https://twitter.com/msopsmgmt).
* U kunt zich aanmelden op basis van gebeurtenissen voor details gedeelde community – kijken we voldoen aan ups, vergaderingen en in de online-community's.

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde informatie verzameld door oplossingen weer te geven.
