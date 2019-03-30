---
title: Het bewaken van de beschikbaarheid van het cluster met Ambari en Azure Monitor-Logboeken
description: Informatie over het gebruik van Ambari en Azure Monitor geregistreerd voor het controleren van de clusterstatus en beschikbaarheid.
keywords: bewaking, ambari, monitor, logboekanalyse, waarschuwing, beschikbaarheid, status
services: hdinsight
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: tyfox
ms.openlocfilehash: 9bf0c0f45007f81312ac08230d571b727220bcd9
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58671330"
---
# <a name="how-to-monitor-cluster-availability-with-ambari-and-azure-monitor-logs"></a>Het bewaken van de beschikbaarheid van het cluster met Ambari en Azure Monitor-Logboeken

HDInsight-clusters zijn zowel Apache Ambari, waarmee u statusinformatie in een oogopslag en vooraf gedefinieerde waarschuwingen, evenals integratie van Azure Monitor Logboeken, waarmee u waarop metrische gegevens en Logboeken, evenals configureerbare meldingen.

Dit document laat zien hoe u deze hulpprogramma's gebruiken voor het bewaken van uw cluster en enkele voorbeelden voor het configureren van een waarschuwing Ambari, beschikbaarheidswaarde knooppunt bewaking en het maken van een waarschuwing voor Azure Monitor die wordt geactiveerd wanneer een heartbeat geen activiteit van een of meer knooppunten ontvangen is begeleid in vijf uur.

## <a name="ambari"></a>Ambari

### <a name="dashboard"></a>Dashboard

De Ambari-dashboard kan worden geopend door te klikken op de **Ambari home** herstelkoppeling in de **Clusterdashboards** sectie van de HDInsight-overzichtsblade in Azure portal, zoals hieronder. U kunt ook deze kan worden geopend door te voeren van de volgende URL in een browser [https://\<clustername\>. azurehdinsight.net](https://clustername.azurehdinsight.net/)

![HDInsight resource portal weergeven](media/hdinsight-cluster-availability/portal-overview.png)

U wordt vervolgens gevraagd om een gebruikersnaam en wachtwoord. Voer de referenties u hebt gekozen tijdens het maken van het cluster.

Vervolgens gaat u naar het dashboard Ambari, waarin widgets waarin een aantal metrische gegevens geven u een kort overzicht van de gezondheid van uw HDInsight-cluster. Deze widgets weergeven metrische gegevens zoals het aantal live DataNodes (worker-knooppunten) en JournalNodes (zookeeper-knooppunt), actieve tijdsduur van NameNodes (hoofdknooppunten), als ook metrische gegevens die specifiek zijn voor bepaalde clustertypen zoals actieve tijdsduur van YARN ResourceManager voor Spark en Hadoop-clusters.

![Ambari-dashboard](media/hdinsight-cluster-availability/ambari-dashboard.png)

### <a name="hosts--view-individual-node-status"></a>-Hosts – status van de afzonderlijke knooppunten weergeven

U kunt ook de statusinformatie voor afzonderlijke knooppunten weergeven. Klik op de **Hosts** tabblad voor een lijst weergeven met alle knooppunten in het cluster en basisinformatie over elk knooppunt te bekijken. De groen vinkje aan de linkerkant van de naam van elk knooppunt geeft aan dat alle onderdelen zijn op het knooppunt. Als een onderdeel is niet actief op een knooppunt is, ziet u een rode waarschuwing driehoek in plaats van een groen vinkje.

![Ambari-hosts bekijken](media/hdinsight-cluster-availability/ambari-hosts.png)

U kunt vervolgens klikken op de **naam** van een knooppunt om weer te geven gedetailleerde metrische gegevens voor hosts voor dat specifieke knooppunt. In deze weergave toont de status/beschikbaarheid van elk afzonderlijk onderdeel.

![Ambari-hosts één weergave van het knooppunt](media/hdinsight-cluster-availability/ambari-hosts-node.png)

### <a name="ambari-alerts"></a>Ambari-waarschuwingen

Ambari biedt ook diverse configureerbare meldingen die u een melding van bepaalde gebeurtenissen krijgt. Wanneer waarschuwingen worden geactiveerd, worden ze weergegeven in de linkerbovenhoek van de Ambari in een rode badge met het aantal waarschuwingen. Te klikken op deze badge, ziet u een lijst met huidige waarschuwingen.

![Ambari-waarschuwingen tellen](media/hdinsight-cluster-availability/ambari-alerts.png)

Een lijst van de waarschuwing definities en hun status wilt weergeven, klikt u op de **waarschuwingen** tabblad, zoals hieronder wordt weergegeven.

![Definities van de Ambari-waarschuwingen weergeven](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambari biedt veel vooraf gedefinieerde waarschuwingen met betrekking tot beschikbaarheid, met inbegrip van:

| Naam van de waarschuwing                        | Description                                                                                                                                                                           |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Samenvatting van status DataNode           | Deze service level-waarschuwing wordt geactiveerd als er niet in orde DataNodes                                                                                                                |
| Status van de NameNode hoge beschikbaarheid | Deze service level-waarschuwing wordt geactiveerd als de actieve NameNode of de stand-by NameNode niet worden uitgevoerd.                                                                              |
| Percentage JournalNodes beschikbaar    | Deze waarschuwing wordt geactiveerd als het aantal omlaag JournalNodes in het cluster groter dan de kritieke drempelwaarde is. Het combineert de resultaten van JournalNode proces controles. |
| Percentage DataNodes beschikbaar       | Deze waarschuwing wordt geactiveerd als het aantal omlaag DataNodes in het cluster groter dan de kritieke drempelwaarde is. Het combineert de resultaten van DataNode proces controles.       |

Een volledige lijst met Ambari waarschuwingen die de beschikbaarheid van een cluster kan worden gevonden. help-monitor [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui),

Als u wilt weergeven van details voor een waarschuwing of criteria wijzigen, klikt u op de **naam** van de waarschuwing. Nemen **samenvatting van status DataNode** als voorbeeld. Hier ziet u een beschrijving van de waarschuwing, evenals de specifieke criteria waarmee een waarschuwing 'waarschuwing' of 'kritiek' en de controle-interval voor de criteria die wordt geactiveerd. Als u wilt bewerken in de configuratie, klikt u op de **bewerken** knop in de rechterbovenhoek van het configuratie.

![Configuratie van de Ambari-waarschuwing](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

Hier kunt u de beschrijving en, nog belangrijker, de controle-interval en drempelwaarden voor waarschuwingen of kritieke waarschuwingen.

![Ambari-weergave voor het bewerken van configuratie van de waarschuwing](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

U kunt in dit voorbeeld 2 niet in orde DataNodes een kritieke waarschuwing en 1 niet in orde DataNode alleen trigger een waarschuwing geactiveerd. Klik op **opslaan** wanneer u klaar bent bewerken.

### <a name="email-notifications"></a>E-mailmeldingen

U kunt eventueel ook e-mailmeldingen voor waarschuwingen van de Ambari configureren. Dit wilt doen wanneer op de **waarschuwingen** en klik op de **acties** knop in de linkerbovenhoek, klikt u vervolgens **meldingen beheren.**

![Ambari actie meldingen beheren](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Een dialoogvenster voor het beheren van meldingen van waarschuwingen te openen. Klik op de **+** aan de onderkant van het dialoogvenster en vul de vereiste velden om te voorzien van Ambari e-mail Serverdetails in voor het verzenden van e-mailberichten.

> [!TIP]
> Instellen van Ambari kunnen e-mailmeldingen een goede manier om waarschuwingen te ontvangen op één plek bij het beheren van vele HDInsight-clusters zijn.

## <a name="azure-monitor-logs-integration"></a>Integratie van Azure Monitor-Logboeken

Azure Monitor-logboeken kunnen gegevens die worden gegenereerd door meerdere resources, zoals HDInsight-clusters, om te worden verzameld en samengevoegd op één plek om een uniforme ervaring voor de bewaking.

Als een vereiste moet u een Log Analytics-werkruimte voor het opslaan van de verzamelde gegevens. Als u nog geen hebt gemaakt een, kunt u hier instructies volgen: [Een Log Analytics-werkruimte maken](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

### <a name="enable-hdinsight-azure-monitor-logs-integration"></a>Integratie van HDInsight Azure Monitor Logboeken inschakelen

De HDInsight-cluster resource-pagina in de portal, klik op de **Operations Management Suite** blade. Klik vervolgens op **inschakelen** en selecteer uw Log Analytics-werkruimte in de vervolgkeuzelijst.

![HDInsight Operations Management Suite-blade](media/hdinsight-cluster-availability/portal-enable-oms.png)

### <a name="query-metrics-and-logs-tables-in-the-logs-blade"></a>Metrische gegevens en logboeken tabellen in de blade Logboeken

Zodra de integratie van Azure Monitor is ingeschakeld (dit kan enkele minuten duren), gaat u naar uw **Log Analytics-werkruimte** resource en klik op de **logboeken** blade

![Blade logboeken log Analytics-werkruimte](media/hdinsight-cluster-availability/portal-logs.png)

De **logboeken** blade bevat een aantal voorbeeldquery's, zoals:

| De naam van query                      | Description                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Computers beschikbaarheid vandaag    | Grafiek van het aantal computers verzenden van Logboeken, elk uur                     |
| Lijst met heartbeats                 | Lijst van alle computer heartbeats van het afgelopen uur                           |
| Laatste heartbeat van elke computer | De laatste heartbeat verzonden door elke computer weergeven                             |
| Niet-beschikbare computers           | Lijst van alle bekende computers die een heartbeat heeft verzonden in de afgelopen 5 uur |
| Beschikbaarheidswaarde               | Berekenen van de snelheid van de beschikbaarheid van elke computer verbonden                |

Uitvoeren als u bijvoorbeeld de **beschikbaarheidswaarde** voorbeeldquery door te klikken op **uitvoeren** query, zoals wordt weergegeven in de bovenstaande schermafbeelding. Hiermee wordt de snelheid van de beschikbaarheid van elk knooppunt in het cluster weergegeven als een percentage. Als u meerdere HDInsight-clusters metrische gegevens verzenden naar Log Analytics-werkruimte hebt ingeschakeld, ziet u de frequentie van beschikbaarheid voor alle knooppunten in de clusters weergegeven.

![Meld u Analytics-werkruimte logboeken blade 'beschikbaarheidswaarde' voorbeeld-query](media/hdinsight-cluster-availability/portal-availability-rate.png)

> [!NOTE] 
> Beschikbaarheidswaarde wordt gemeten in een periode van 24 uur, zodat uw cluster nodig heeft om uit te voeren voor ten minste 24 uur voordat u nauwkeurige beschikbaarheidstarieven ziet.

U kunt deze tabel naar een gedeeld dashboard vastmaken door te klikken op **pincode** in de rechterbovenhoek. Als u geen beschrijfbare gedeelde dashboards hebt, kunt u zien over het maken van een hier: [Maken en delen van dashboards in de Azure-portal](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-a-dashboard-and-manage-access-control).

### <a name="azure-monitor-alerts"></a>Azure Monitor-waarschuwingen

U kunt ook instellen van Azure Monitor-waarschuwingen die wordt geactiveerd wanneer de waarde van een metrische waarde of de resultaten van een query aan bepaalde voorwaarden voldoen. Als voorbeeld maken we een waarschuwing als u wilt een e-mailbericht verzenden wanneer een of meer knooppunten 5 uur een heartbeat is niet verzonden (dat wil zeggen wordt ervan uitgegaan dat niet beschikbaar).

Uit de **logboeken** blade, voer de **niet-beschikbare computers** voorbeeldquery door te klikken op **uitvoeren** query, zoals hieronder wordt weergegeven.

![Meld u Analytics-werkruimte logboeken blade 'niet-beschikbare computers' voorbeeld-query](media/hdinsight-cluster-availability/portal-unavailable-computers.png)

Als alle knooppunten beschikbaar zijn, moet deze query 0 resultaten retourneren voor nu. Klik op **nieuwe waarschuwingsregel** om te beginnen met het configureren van de waarschuwing voor deze query.

![Log Analytics-werkruimte nieuwe waarschuwingsregel](media/hdinsight-cluster-availability/portal-logs-new-alert-rule.png)

Er zijn drie onderdelen op een waarschuwing: de *resource* waarvoor de regel (de Log Analytics-werkruimte in dit geval), maken de *voorwaarde* om de waarschuwing te activeren en de *actiegroepen*  om te bepalen wat er gebeurt wanneer de waarschuwing wordt geactiveerd.

Klik op de **voorwaarde titel**, zoals hieronder wordt weergegeven op de signaallogica configureren is voltooid.

![Waarschuwingsregel voorwaarde](media/hdinsight-cluster-availability/portal-condition-title.png)

Hiermee opent u de **signaallogica configureren** blade.

Stel de **waarschuwen logische** sectie als volgt:

*Op basis van: Het aantal resultaten, voorwaarde: Groter dan de drempelwaarde: 0.*

Omdat deze query alleen niet-beschikbare knooppunten als resultaat retourneert als het aantal resultaten ooit groter dan 0 is, kan de waarschuwing moet worden geactiveerd.

In de **Evaluated op basis van** sectie, stelt u de **periode** en **frequentie** gebaseerd op hoe vaak u wilt controleren op niet-beschikbare knooppunten.

Houd er rekening mee dat ten behoeve van deze waarschuwing u wilt controleren of **periode = frequentie.** Meer informatie over periode, frequentie en andere parameters voor waarschuwing vindt [hier](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types).

Klik op **gedaan** nadat u hebt de signaallogica configureren.

![Waarschuwingsregel signaallogica configureren](media/hdinsight-cluster-availability/portal-configure-signal-logic.png)

Als u een bestaande actiegroep niet al hebt, klikt u op **nieuw** onder de **actiegroepen** sectie.

![Nieuwe actiegroep waarschuwingsregel](media/hdinsight-cluster-availability/portal-create-new-action-group.png)

Hiermee opent u de **actiegroep toevoegen** blade. Kies een **naam van de actiegroep**, **afkorting**, **abonnement**, en **resourcegroep.** Onder de **acties** sectie, kiest u een **actienaam** en selecteer **e-mailadres/SMS/Push/stem** als de **actietype.**

> [!NOTE]
> Er zijn verschillende andere acties die een waarschuwing naast een e-mailadres/SMS/Push/spraak, zoals een Azure-functie, LogicApp, Webhook, ITSM en Automation-Runbook kunt activeren. [Meer informatie.](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information)

Hiermee opent u de **e-mailadres/SMS/Push/stem** blade. Kies een **naam** voor de ontvanger, **controleren** de **e** , en typt u een e-mailadres waarnaar u waarschuwingen verzenden wilt. Klik op **OK** in de **e-mailadres/SMS/Push/stem** blade vervolgens in de **actiegroep toevoegen** blade voor het configureren van de actiegroep is voltooid.

![Waarschuwingsregel actiegroep toevoegen](media/hdinsight-cluster-availability/portal-add-action-group.png)

Nadat deze blades hebt gesloten, ziet u de actiegroep vermeld in de **actiegroepen** sectie. Voltooi ten slotte de **Waarschuwingsdetails** sectie door een **waarschuwing regelnaam** en **beschrijving** en het kiezen van een **ernst**.
Klik op **maken van waarschuwingsregel** om te voltooien.

![Maken van waarschuwingsregel voltooien](media/hdinsight-cluster-availability/portal-create-alert-rule-finish.png)

> [!TIP]
> De mogelijkheid om op te geven **ernst** is een krachtig hulpprogramma dat kan worden gebruikt bij het maken van meerdere waarschuwingen. Bijvoorbeeld, kunt u een waarschuwing als u wilt een waarschuwing (Sev 1) als een enkel hoofdknooppunt gaat naar beneden en uitvallen van een nieuwe waarschuwing die wordt gegeven Kritiek (Sev 0) in het onwaarschijnlijke geval beide hoofdknooppunten.

Als de voorwaarde voor deze waarschuwing wordt voldaan, wordt de waarschuwing wordt geactiveerd en u ontvangt een e-mail met de details van de waarschuwing als volgt:

![Azure Monitor alert e-mailadres](media/hdinsight-cluster-availability/alert-email.png)

U kunt ook weergeven met alle waarschuwingen die zijn geactiveerd, gegroepeerd op basis van ernst, door te gaan naar de **waarschuwingen** -blade in uw **Log Analytics-werkruimte**.

![Waarschuwingen in log Analytics-werkruimte](media/hdinsight-cluster-availability/portal-alerts.png)

Te klikken op een groepering ernst (dat wil zeggen **Sev 1,** als gemarkeerde hierboven)-records voor alle waarschuwingen van dat ernst die zijn geactiveerd, zoals hieronder wordt weergegeven:

![Waarschuwingen in log Analytics-werkruimte sev 1](media/hdinsight-cluster-availability/portal-alerts-sev-1.png)

## <a name="next-steps"></a>Volgende stappen
- [Beschikbaarheid en betrouwbaarheid van Apache Hadoop-clusters in HDInsight](hdinsight-high-availability-linux.md)