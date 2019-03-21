---
title: Instellen van waarschuwingen en meldingen met behulp van Azure portal | Microsoft Docs
description: De Azure portal gebruiken voor het maken van waarschuwingen van de SQL-Database, die meldingen of automatisering activeren kunnen wanneer de door u opgegeven voorwaarden wordt voldaan.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 11/02/2018
ms.openlocfilehash: 93337e39a117c1f8d38f24dc416ff8ae95513a34
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57855585"
---
# <a name="create-alerts-for-azure-sql-database-and-data-warehouse-using-azure-portal"></a>Waarschuwingen maken voor Azure SQL Database en Data Warehouse met behulp van Azure portal

## <a name="overview"></a>Overzicht
In dit artikel leest u hoe het instellen van Azure SQL Database en Data Warehouse waarschuwingen via Azure portal. Waarschuwingen kunnen u een e-mail verzenden of een webhook aanroepen wanneer bepaalde metrische gegevens (bijvoorbeeld de grootte van de database of de CPU-gebruik) de drempel bereikt. Dit artikel bevat ook aanbevolen procedures voor het instellen van waarschuwingen perioden.    

> [!IMPORTANT]
> Deze functie is nog niet beschikbaar in het beheerde exemplaar. Als alternatief kunt u SQL-Agent kunt gebruiken voor het verzenden van e-mailwaarschuwingen voor sommige metrische gegevens op basis van [dynamische beheerweergaven](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views).

U kunt een waarschuwing op basis van bewaking metrische gegevens voor of gebeurtenissen op uw Azure-services kunt ontvangen.

* **Metrische waarden** -de waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven metrische gegevens van een drempel die u in beide richtingen toewijst overschrijden. Dat wil zeggen, deze beide wordt geactiveerd wanneer eerst aan de voorwaarde is voldaan en klik daarna als die voorwaarde is niet meer wordt voldaan aan.    
* **Gebeurtenissen in het activiteitenlogboek** -een waarschuwing kunt activeren op *elke* gebeurtenis of alleen wanneer een bepaald aantal gebeurtenissen optreden.

Een waarschuwing als u wilt de volgende handelingen uit wanneer deze wordt geactiveerd, kunt u configureren:

* e-mailmeldingen verzenden naar de servicebeheerder en medebeheerders
* e-mail verzenden naar extra e-mailadressen die u opgeeft.
* Een webhook aanroepen

U kunt configureren en informatie over met behulp van regels voor waarschuwingen

* [Azure-portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../azure-monitor/platform/alerts-classic-portal.md)
* [Opdrachtregelinterface (CLI)](../azure-monitor/platform/alerts-classic-portal.md)
* [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Een waarschuwingsregel maken op een metrische waarde met de Azure-portal
1. In de [portal](https://portal.azure.com/), zoek de resource die u geïnteresseerd bent in controle en selecteer deze.
2. Selecteer **waarschuwingen (klassiek)** onder de sectie bewaking. De tekst en het pictogram kunnen verschillen voor verschillende resources.  
   
     ![Bewaking](media/sql-database-insights-alerts-portal/AlertsClassicButton.JPG)
  
   - **ALLEEN SQL DW**: Klik op de **DWU-gebruik** graph. Selecteer **klassieke waarschuwingen weergeven**

3. Selecteer de **metrische waarschuwing toevoegen (klassiek)** knop en vul de velden in.
   
    ![Waarschuwing toevoegen](media/sql-database-insights-alerts-portal/AddDBAlertPageClassic.JPG)
4. **Naam** de waarschuwing regel en kies een **beschrijving**, die ook weergegeven in e-mailmeldingen.
5. Selecteer de **Metric** u wilt controleren, en kies vervolgens een **voorwaarde** en **drempelwaarde** waarde voor de metrische gegevens. Ook voor kiezen de **periode** van de tijd waarop de regel voor metrische gegevens moet worden voldaan voordat de waarschuwing triggers. Bijvoorbeeld als u de periode 'PT5M' en de waarschuwing voor CPU ziet er uit meer dan 80%, de waarschuwing wordt geactiveerd wanneer de **gemiddelde** CPU gedurende vijf minuten meer dan 80% is geweest. Nadat de eerste trigger is het geval is, wordt deze opnieuw geactiveerd wanneer de gemiddelde CPU lager dan 80% meer dan vijf minuten is. De meting van CPU doet zich voor elke minuut. Raadpleeg de onderstaande tabel voor ondersteunde tijdvensters en typt u de aggregatie die elke waarschuwing gebruikt en niet alle waarschuwingen de gemiddelde waarde gebruiken.   
6. Controleer **e-eigenaren...**  als u wilt dat beheerders en medebeheerders moet worden verzonden wanneer de waarschuwing wordt geactiveerd.
7. Als u aanvullende e-mailberichten wilt die een melding ontvangen wanneer de waarschuwing wordt geactiveerd, voegt u ze op in de **beheerder email(s)** veld. Meerdere e-mailberichten gescheiden door puntkomma's - *e\@contoso.com;email2\@contoso.com*
8. Plaats in een geldige URI in de **Webhook** veld als u wilt dat deze wordt aangeroepen wanneer de waarschuwing wordt geactiveerd.
9. Selecteer **OK** wanneer u klaar bent om de waarschuwing te genereren.   

Binnen een paar minuten, wordt de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="managing-your-alerts"></a>Uw waarschuwingen beheren
Als u een waarschuwing hebt gemaakt, kunt u dit selecteren en:

* Bekijk een grafiek weer met de drempelwaarde voor metrische gegevens en de werkelijke waarden van de vorige dag.
* Bewerken of te verwijderen.
* **Uitschakelen** of **inschakelen** als u wilt tijdelijk stoppen of doorgaan met het ontvangen van meldingen voor deze waarschuwing.


## <a name="sql-database-alert-values"></a>Waarschuwing waarden SQL-Database

| Resourcetype | Naam van meetwaarde | Beschrijvende naam | Aggregatietype | Minimale waarschuwing tijdvenster|
| --- | --- | --- | --- | --- |
| SQL-database | cpu_percent | CPU-percentage | Gemiddeld | 5 minuten |
| SQL-database | physical_data_read_percent | Gegevens-I/O-percentage | Gemiddeld | 5 minuten |
| SQL-database | log_write_percent | Percentage logboek-IO | Gemiddeld | 5 minuten |
| SQL-database | dtu_consumption_percent | DTU-percentage | Gemiddeld | 5 minuten |
| SQL-database | opslag | Totale databasegrootte | Maximum | 30 minuten |
| SQL-database | connection_successful | Geslaagde verbindingen | Totaal | 10 minuten |
| SQL-database | connection_failed | Mislukte verbindingen | Totaal | 10 minuten |
| SQL-database | blocked_by_firewall | Geblokkeerd door Firewall | Totaal | 10 minuten |
| SQL-database | Impasse | Deadlocks | Totaal | 10 minuten |
| SQL-database | storage_percent | Databaseomvangpercentage | Maximum | 30 minuten |
| SQL-database | xtp_storage_percent | In-Memory OLTP-opslag percent(Preview) | Gemiddeld | 5 minuten |
| SQL-database | workers_percent | Werknemerspercentage | Gemiddeld | 5 minuten |
| SQL-database | sessions_percent | Sessies procent | Gemiddeld | 5 minuten |
| SQL-database | dtu_limit | DTU-limiet | Gemiddeld | 5 minuten |
| SQL-database | dtu_used | DTU gebruikt | Gemiddeld | 5 minuten |
||||||
| Elastische pool | cpu_percent | CPU-percentage | Gemiddeld | 10 minuten |
| Elastische pool | physical_data_read_percent | Gegevens-I/O-percentage | Gemiddeld | 10 minuten |
| Elastische pool | log_write_percent | Percentage logboek-IO | Gemiddeld | 10 minuten |
| Elastische pool | dtu_consumption_percent | DTU-percentage | Gemiddeld | 10 minuten |
| Elastische pool | storage_percent | Opslagpercentage | Gemiddeld | 10 minuten |
| Elastische pool | workers_percent | Werknemerspercentage | Gemiddeld | 10 minuten |
| Elastische pool | eDTU_limit | eDTU-limiet | Gemiddeld | 10 minuten |
| Elastische pool | storage_limit | Limiet voor opslag | Gemiddeld | 10 minuten |
| Elastische pool | eDTU_used | eDTU gebruikt | Gemiddeld | 10 minuten |
| Elastische pool | storage_used | Gebruikte opslag | Gemiddeld | 10 minuten |
||||||               
| SQL data warehouse | cpu_percent | CPU-percentage | Gemiddeld | 10 minuten |
| SQL data warehouse | physical_data_read_percent | Gegevens-I/O-percentage | Gemiddeld | 10 minuten |
| SQL data warehouse | connection_successful | Geslaagde verbindingen | Totaal | 10 minuten |
| SQL data warehouse | connection_failed | Mislukte verbindingen | Totaal | 10 minuten |
| SQL data warehouse | blocked_by_firewall | Geblokkeerd door Firewall | Totaal | 10 minuten |
| SQL data warehouse | service_level_objective | De servicelaag van de database | Totaal | 10 minuten |
| SQL data warehouse | dwu_limit | dwu-limiet | Maximum | 10 minuten |
| SQL data warehouse | dwu_consumption_percent | DWU-percentage | Gemiddeld | 10 minuten |
| SQL data warehouse | dwu_used | Gebruikte DWU | Gemiddeld | 10 minuten |
||||||


## <a name="next-steps"></a>Volgende stappen
* [Bekijk een overzicht van Azure-bewaking](../monitoring-and-diagnostics/monitoring-overview.md) met inbegrip van de typen gegevens die u kunt verzamelen en controleren.
* Meer informatie over [configureren van webhooks in waarschuwingen](../azure-monitor/platform/alerts-webhooks.md).
* Krijgen een [overzicht van diagnostische logboeken](../azure-monitor/platform/diagnostic-logs-overview.md) en verzamelen van gedetailleerde metrische gegevens voor hoge frequentie op uw service.
* Krijgen een [overzicht van metrische gegevens verzameling](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
