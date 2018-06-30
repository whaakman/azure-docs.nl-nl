---
title: Veelgestelde vragen voor de overgang van OMS-portal naar Azure-portal voor gebruikers Log Analytics | Microsoft Docs
description: Antwoorden op veelgestelde vragen voor logboekanalyse gebruikers overstappen van OMS-portal naar de Azure-portal.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 6479db44681071932ff92ee17ea0aec6518dc74e
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128636"
---
# <a name="common-questions-for-transition-from-oms-portal-to-azure-portal-for-log-analytics-users"></a>Veelgestelde vragen voor de overgang van OMS-portal naar Azure-portal voor gebruikers Log Analytics
Een eigen portal aangeroepen OMS-portal logboekanalyse in eerste instantie gebruikt voor het beheren van de configuratie en verzamelde gegevens te analyseren.  Alle functionaliteit van deze portal is verplaatst naar de Azure portal waarin wordt voortgezet worden ontwikkeld.

In dit artikel antwoorden op veelgestelde vragen voor gebruikers voor het maken van deze overgang.  Als u Log Analytics in de OMS-portal gebruikt, kunt u hier antwoorden zoeken voor hoe u dezelfde taken in de Azure-portal uitvoeren kunt.

## <a name="where-do-i-find-log-analytics-in-azure"></a>Waar vind ik in Azure Log Analytics?
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com).  Klik op **alle services**, en typt u in de lijst met resources **logboekanalyse**. Selecteer **logboekanalyse** en selecteer vervolgens uw werkruimte. De overzichtspagina voor de werkruimte wordt weergegeven.

![Log Analytics-werkruimte](media/log-analytics-new-portal/log-analytics.png)

## <a name="how-do-i-manage-permissions"></a>Hoe beheer ik machtigingen?
Als u geen toegang tot uw werkruimte voor logboekanalyse in de Azure portal hebt, moet u voor het configureren van uw machtigingen met [Azure op rollen gebaseerde toegang](../active-directory/role-based-access-control-configure.md). Zie voor meer informatie over het beheren van machtigingen voor de werkruimte [werkruimten beheren](../log-analytics/log-analytics-manage-access.md#manage-accounts-and-users). Zie voor meer informatie over het beheren van machtigingen voor waarschuwingen [aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md).

## <a name="how-do-i-create-a-new-workspace"></a>Hoe maak ik een nieuwe werkruimte 
Klik in de lijst met werkruimten in de Azure portal op **toevoegen** in de lijst met werkruimten.  Zie voor meer informatie [een Log Analytics-werkruimte maken in de Azure portal](../log-analytics/log-analytics-quick-create-workspace.md).

![Overzichtspagina](media/log-analytics-new-portal/new-workspace.png)

## <a name="where-is-my-overview-page"></a>Waar kan ik mijn overzichtspagina?
Het startscherm in de OMS-portal weergegeven de tegels voor alle beheeroplossingen geïnstalleerd in uw werkruimte en aangepaste weergaven die u hebt gemaakt. Deze dezelfde weergave is beschikbaar in de Azure-portal. Selecteer in de werkruimte **werkruimte overzicht**.

![Overzichtspagina](media/log-analytics-new-portal/overview.png)

## <a name="how-do-i-open-log-search-and-view-designer"></a>Hoe kan ik logboek zoeken en weergeven Designer openen?
Beide **logboek zoeken** en **ontwerper** beschikbaar zijn op de hoofdpagina en in het menu links van uw werkruimte in de Azure portal rechts naast naar **overzicht**.

## <a name="where-do-i-find-settings"></a>Waar vind ik instellingen?
Veel van de instellingen in de **instellingen** sectie van de OMS-portal zijn beschikbaar in de **geavanceerde instellingen** menu in de Azure portal voor de werkruimte.

![Geavanceerde instellingen](media/log-analytics-new-portal/advanced-settings.png)

De volgende secties bevatten een volledige lijst van hoe u toegang hebt tot de instellingen die eerder beschikbaar waren in de **instellingen** sectie van de OMS-portal.

### <a name="accounts"></a>Accounts 
Accountinstellingen worden beheerd op verschillende plaatsen in de Azure portal, zoals beschreven in de volgende tabel.

| De instelling in de OMS-portal | Equivalent in de Azure portal |
|:---|:---|
| Automation-account | **Automation-Account** menu voor de werkruimte. |
| Azure-abonnement & Data-abonnement | **Prijscategorie** menu voor de werkruimte. |
| Gebruikers beheren | Gebruikmaken van Azure op rollen gebaseerde toegang tot [machtigingen beheren voor uw werkruimte](#how-do-i-manage-permissions). |
| Werkruimtegegevens | Informatie die beschikbaar is op **OMS-werkruimte** menu voor de werkruimte. |

### <a name="alerts"></a>Waarschuwingen
Regels voor waarschuwingen op basis van logboekanalyse query's worden nu beheerd de [uniforme ervaring waarschuwingen](#how-do-i-create-and-manage-alerts). 

### <a name="computer-groups"></a>Computergroepen
Beheren van groepen van de Computer in de **geavanceerde instellingen** menu voor de werkruimte. 

### <a name="connected-sources"></a>Verbonden bronnen
De meeste verbonden broninstellingen beheren de in de **geavanceerde instellingen** menu voor de werkruimte. De volgende tabel biedt details voor elke sectie van dit menu.

| De instelling in de OMS-portal | Equivalent in de Azure portal |
|:---|:---|
| Windows-servers   | **Geavanceerde instellingen** menu voor de werkruimte. |
| Linux-Servers   | **Geavanceerde instellingen** menu voor de werkruimte. |
| Azure Storage     | **Geavanceerde instellingen** menu voor de werkruimte. |
| System Center     | **Geavanceerde instellingen** menu voor de werkruimte. |
| Office 365        | Zie de [documentatie voor Office 365-beheeroplossing](../operations-management-suite/oms-solution-office-365.md) voor configuratie-informatie. |
| Windows-telemetrie | Nog niet beschikbaar in Azure-portal. |
| ITSM-Connector    | Zie [ITSM verbinding producten/services met IT Service Management-Connector](../log-analytics/log-analytics-itsmc-connections.md) voor instructies over het verbinden van uw service ITSM met logboekanalyse. |

### <a name="data"></a>Gegevens
Instellingen voor de meeste gegevens beheren de in de **geavanceerde instellingen** menu voor de werkruimte. De volgende tabel biedt details voor elke sectie van dit menu.

| De instelling in de OMS-portal | Equivalent in de Azure portal |
|:---|:---|
| Windows-gebeurtenislogboeken           | **Geavanceerde instellingen** menu voor de werkruimte. |
| Windows-prestatiemeteritems | **Geavanceerde instellingen** menu voor de werkruimte. |
| Linux-prestatiemeteritems   | **Geavanceerde instellingen** menu voor de werkruimte. |
| IIS-logboeken                     | **Geavanceerde instellingen** menu voor de werkruimte. |
| Aangepaste velden                | **Geavanceerde instellingen** menu voor de werkruimte. |
| Aangepaste logboeken                  | **Geavanceerde instellingen** menu voor de werkruimte. |
| Syslog                       | **Geavanceerde instellingen** menu voor de werkruimte. |
| Application Insights         | Deze oplossing is afgeschaft nu dat Log Analytics en Application Insights dezelfde gegevensengine delen.  |
| Windows-bestandstracering        | **Het bijhouden van** menu in Azure Automation. Zie [bijhouden van wijzigingen in uw omgeving met de oplossing voor wijzigingen bijhouden](../automation/automation-change-tracking.md) voor meer informatie. |
| Windows-register traceren        | **Het bijhouden van** menu in Azure Automation. Zie [bijhouden van wijzigingen in uw omgeving met de oplossing voor wijzigingen bijhouden](../automation/automation-change-tracking.md) voor meer informatie. |
| Bijhouden van Linux-bestanden          | **Het bijhouden van** menu in Azure Automation. Zie [bijhouden van wijzigingen in uw omgeving met de oplossing voor wijzigingen bijhouden](../automation/automation-change-tracking.md) voor meer informatie. |

### <a name="solutions"></a>Oplossingen
Beheren van oplossingen in de **oplossingen** menu voor de werkruimte. 

## <a name="how-do-i-install-and-remove-management-solutions"></a>Hoe ik installeren en verwijderen van oplossingen voor het beheer?
In de OMS-portal installeert beheeroplossingen uit de galerie oplossingen en verwijderd uit **instellingen**. In de Azure portal [installeren beheeroplossingen](../monitoring/monitoring-solutions.md#install-a-management-solution) vanuit Azure Marketplace. [Verwijderen van oplossingen](../monitoring/monitoring-solutions.md#remove-a-management-solution) uit de lijst met geïnstalleerde oplossingen.

## <a name="how-do-i-create-and-manage-alerts"></a>Hoe ik waarschuwingen maken en beheren?
Regels voor waarschuwingen op basis van logboekanalyse query's worden nu beheerd de [uniforme ervaring waarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). Zie [waarschuwingen van logboekanalyse uitbreiden naar Azure waarschuwingen](../monitoring-and-diagnostics/monitoring-alerts-extend-tool.md) voor meer informatie over het configureren en waarschuwingen in de Azure portal gebruiken.

## <a name="how-do-i-access-my-dashboards"></a>Hoe krijg ik toegang tot Mijn dashboards?
[Dashboards](../log-analytics/log-analytics-dashboards.md) in logboekanalyse zijn afgeschaft.  U kunt gegevens in het gebruik van logboekanalyse visualiseren [ontwerper](../log-analytics/log-analytics-view-designer.md) die aanvullende functionaliteit en pincode query en weergaven voor het Azure dashboards heeft.

## <a name="how-do-i-check-my-usage"></a>Hoe kan ik mijn gebruik controleren?
U kunt nu eenvoudig bekijken en beheren van uw gebruik en de kosten van logboekanalyse door te selecteren **gebruik en de geschatte kosten** in uw werkruimte.

![Gebruik en geraamde kosten](media/log-analytics-new-portal/usage.png)


## <a name="can-i-still-use-the-classic-portal"></a>Kan ik de klassieke portal toch gebruiken?
Een beperkte periode, u kunt nog steeds toegang tot de portal via deze URL met de naam van uw eigen werkruimte: https://\<naam van uw werkruimte\>. portal.mms.microsoft.com. We raden u echter met behulp van Azure portal en ons de feedback geven LAUpgradeFeedback@microsoft.com op tegen blokkerende problemen.

## <a name="next-steps"></a>Volgende stappen

- [Zoeken en installeren van beheeroplossingen](../monitoring/monitoring-solutions.md) met de Azure portal.
- Meer informatie over [logboek zoeken in de Azure portal](log-analytics-log-search-portals.md).