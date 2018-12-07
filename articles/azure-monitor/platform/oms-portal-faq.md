---
title: Veelgestelde vragen voor overgang van OMS-portal naar Azure-portal voor gebruikers van de Log Analytics | Microsoft Docs
description: Antwoorden op veelgestelde vragen over Log Analytics-gebruikers overstappen van de OMS-portal naar de Azure-portal.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 61e1a02c3f1c194432750dce8ddf8162a981771d
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53002536"
---
# <a name="common-questions-for-transition-from-oms-portal-to-azure-portal-for-log-analytics-users"></a>Veelgestelde vragen voor overgang van OMS-portal naar Azure-portal voor Log Analytics-gebruikers
Log Analytics gebruikt een eigen portal met de OMS-portal de naam in eerste instantie de configuratie ervan beheren en analyseren van verzamelde gegevens.  Alle functionaliteit van deze portal is verplaatst naar de Azure-portal waar deze blijft op worden ontwikkeld.

In dit artikel vindt u antwoorden op veelgestelde vragen voor een van deze overgang.  Als u Log Analytics in de OMS-portal gebruikt, kunt u hier antwoorden vinden voor hoe u dezelfde taken in Azure portal uitvoeren kunt.

## <a name="do-i-need-to-migrate-anything"></a>Heb ik nodig voor het migreren van iets?
Nee. Er zijn geen wijzigingen aangebracht in Log Analytics zelf, dus er is niets die moeten worden gemigreerd. Het enige dat wordt gewijzigd, is de interface die u gebruikt om deze te openen. U kunt zelfs de Azure portal nu gebruiken voor toegang tot de dezelfde werkruimten, oplossingen, weergaven en zoekopdrachten die u gebruikt in de OMS-portal vandaag nog.

## <a name="where-do-i-find-log-analytics-in-azure"></a>Waar vind ik Log Analytics in Azure?
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com).  Klik op **alle services**, en typ in de lijst met resources **Log Analytics**. Selecteer **Log Analytics** en selecteer vervolgens uw werkruimte. De overzichtspagina voor de werkruimte wordt weergegeven.

![Log Analytics-werkruimte](media/oms-portal-faq/log-analytics.png)

## <a name="how-do-i-manage-permissions"></a>Hoe beheer ik machtigingen?
Als u geen toegang tot uw Log Analytics-werkruimte in de Azure-portal hebt, moet u het configureren van uw machtigingen met behulp van [Azure op rollen gebaseerde toegang](../../role-based-access-control/role-assignments-portal.md). Zie voor meer informatie over het beheren van machtigingen voor de werkruimte [werkruimten beheren](manage-access.md#manage-accounts-and-users). Zie voor meer informatie over het beheren van machtigingen voor waarschuwingen [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../../monitoring-and-diagnostics/monitoring-roles-permissions-security.md).

## <a name="how-do-i-create-a-new-workspace"></a>Hoe kan ik een nieuwe werkruimte maken? 
Klik in de lijst met werkruimten in Azure portal, op **toevoegen** in de lijst met werkruimten.  Zie voor meer informatie, [een Log Analytics-werkruimte maken in Azure portal](../learn/quick-create-workspace.md).

![Overzichtspagina](media/oms-portal-faq/new-workspace.png)

## <a name="where-is-my-overview-page"></a>Waar zijn mijn overzichtspagina?
De belangrijkste scherm in de OMS-portal wordt weergegeven de tegels voor alle beheeroplossingen die zijn geïnstalleerd in uw werkruimte en alle aangepaste weergaven die u hebt gemaakt. Deze dezelfde weergave is beschikbaar in de Azure-portal. Selecteer in uw werkruimte **werkruimte overzicht**.

![Overzichtspagina](media/oms-portal-faq/overview.png)

## <a name="how-do-i-open-log-search-and-view-designer"></a>Hoe open ik zoeken in Logboeken en Designer bekijken?
Beide **zoeken in logboeken** en **Weergaveontwerper** zijn rechts naast beschikbaar op de startpagina en in het menu links van uw werkruimte in de Azure-portal voor **overzicht**.

## <a name="where-do-i-find-settings"></a>Waar vind ik de instellingen?
Veel van de instellingen in de **instellingen** sectie van de OMS-portal zijn beschikbaar in de **geavanceerde instellingen** menu in de Azure-portal voor de werkruimte.

![Geavanceerde instellingen](media/oms-portal-faq/advanced-settings.png)

De volgende secties bevatten een volledige lijst van hoe u toegang krijgen tot instellingen die reeds waren eerder beschikbaar was in de **instellingen** sectie van de OMS-portal.

### <a name="accounts"></a>Accounts 
Accountinstellingen worden beheerd op verschillende plaatsen in Azure portal, zoals beschreven in de volgende tabel.

| Instelling in de OMS-portal | Equivalent in de Azure-portal |
|:---|:---|
| Automation-account | **Automation-Account** menu voor de werkruimte. |
| Azure-abonnement en Data-abonnement | **Prijscategorie** menu voor de werkruimte. |
| Gebruikers beheren | Gebruik Azure op rollen gebaseerde toegang tot [machtigingen beheren voor uw werkruimte](#how-do-i-manage-permissions). |
| Werkruimtegegevens | Informatie die beschikbaar is op **OMS-werkruimte** menu voor de werkruimte. |

### <a name="alerts"></a>Waarschuwingen
Regels voor waarschuwingen op basis van Log Analytics-query's worden nu beheerd de [geïntegreerde ervaring waarschuwingen](#how-do-i-create-and-manage-alerts). 

### <a name="computer-groups"></a>Computergroepen
Beheren van computergroepen in de **geavanceerde instellingen** menu voor de werkruimte. 

### <a name="connected-sources"></a>Verbonden bronnen
De meeste instellingen voor de verbonden gegevensbron beheren de in de **geavanceerde instellingen** menu voor de werkruimte. De volgende tabel bevat details voor elke sectie van dit menu.

| Instelling in de OMS-portal | Equivalent in de Azure-portal |
|:---|:---|
| Windows-servers   | **Geavanceerde instellingen** menu voor de werkruimte. |
| Linux-Servers   | **Geavanceerde instellingen** menu voor de werkruimte. |
| Azure Storage     | **Geavanceerde instellingen** menu voor de werkruimte. |
| System Center     | **Geavanceerde instellingen** menu voor de werkruimte. |
| Office 365        | Zie de [documentatie voor Office 365-beheeroplossing](../insights/solution-office-365.md) voor informatie over de configuratie. |
| Windows-telemetrie | Instellingenmenu voor de oplossing. Zie [Windows Analytics in Azure portal](/windows/deployment/update/windows-analytics-azure-portal) voor informatie over de configuratie. |
| ITSM-Connector    | Zie [verbinding maken met ITSM-producten en services met IT Service Management Connector](itsmc-connections.md) voor instructies voor uw ITSM-service te verbinden met Log Analytics. |

### <a name="data"></a>Gegevens
Instellingen voor de meeste gegevens beheren de in de **geavanceerde instellingen** menu voor de werkruimte. De volgende tabel bevat details voor elke sectie van dit menu.

| Instelling in de OMS-portal | Equivalent in de Azure-portal |
|:---|:---|
| Windows-gebeurtenislogboeken           | **Geavanceerde instellingen** menu voor de werkruimte. |
| Windows-prestatiemeteritems | **Geavanceerde instellingen** menu voor de werkruimte. |
| Linux-prestatiemeteritems   | **Geavanceerde instellingen** menu voor de werkruimte. |
| IIS-logboeken                     | **Geavanceerde instellingen** menu voor de werkruimte. |
| Aangepaste velden                | **Geavanceerde instellingen** menu voor de werkruimte. |
| Aangepaste logboeken                  | **Geavanceerde instellingen** menu voor de werkruimte. |
| Syslog                       | **Geavanceerde instellingen** menu voor de werkruimte. |
| Application Insights         | Deze oplossing is afgeschaft nu dat Log Analytics en Application Insights dezelfde gegevensengine delen.  |
| Windows-bestandstracering        | **Wijzigingen bijhouden** in het menu in Azure Automation. Zie [bijhouden van wijzigingen in uw omgeving met de oplossing wijzigingen bijhouden](../../automation/automation-change-tracking.md) voor meer informatie. |
| Windows-register traceren        | **Wijzigingen bijhouden** in het menu in Azure Automation. Zie [bijhouden van wijzigingen in uw omgeving met de oplossing wijzigingen bijhouden](../../automation/automation-change-tracking.md) voor meer informatie. |
| Bijhouden van Linux-bestanden          | **Wijzigingen bijhouden** in het menu in Azure Automation. Zie [bijhouden van wijzigingen in uw omgeving met de oplossing wijzigingen bijhouden](../../automation/automation-change-tracking.md) voor meer informatie. |

### <a name="solutions"></a>Oplossingen
Beheren van oplossingen in de **oplossingen** menu voor de werkruimte. 

## <a name="how-do-i-install-and-remove-management-solutions"></a>Hoe ik installeren en verwijderen van oplossingen voor het beheer?
In de OMS-portal installeert oplossingen voor het beheer van de galerie van oplossingen en verwijderd uit **instellingen**. In de Azure-portal [installeren beheeroplossingen](../insights/solutions.md#install-a-management-solution) vanuit Azure Marketplace. [Verwijderen van oplossingen](../insights/solutions.md#remove-a-management-solution) uit de lijst met geïnstalleerde oplossingen.

## <a name="how-do-i-create-and-manage-alerts"></a>Hoe ik waarschuwingen maken en beheren?
Regels voor waarschuwingen op basis van Log Analytics-query's worden nu beheerd de [geïntegreerde ervaring waarschuwingen](../../monitoring-and-diagnostics/alert-metric.md). Zie [voor het uitbreiden van waarschuwingen van Log Analytics in Azure-waarschuwingen](../../monitoring-and-diagnostics/monitoring-alerts-extend-tool.md) voor meer informatie over het configureren en gebruiken van waarschuwingen in Azure portal.

## <a name="how-do-i-access-my-dashboards"></a>Hoe krijg ik toegang tot Mijn dashboards?
De [mijn Dashboard](dashboards.md) functie in Log Analytics is afgeschaft. Deze functie u hebt een persoonlijke verzameling van onderdelen van de Weergaveontwerper toegestaan en wordt vervangen door de ingebouwde functionaliteit voor Azure-Dashboard. U kunt doorgaan met het visualiseren van gegevens in Log Analytics met behulp van [Weergaveontwerper](view-designer.md) voor gedeelde weergaven. U kunt ook visualisaties uit deze weergaven of voor vastmaken [afzonderlijke query's](../learn/tutorial-logs-dashboards.md) aan Azure-Dashboards.

## <a name="how-do-i-check-my-usage"></a>Hoe kan ik mijn gebruik controleren?
U kunt nu eenvoudig bekijken en beheren van uw gebruik en kosten van Log Analytics door te selecteren **gebruik en geraamde kosten** in uw werkruimte.

![Gebruik en geraamde kosten](media/oms-portal-faq/usage.png)


## <a name="can-i-still-use-the-classic-portal"></a>Kan ik nog steeds de klassieke portal gebruiken?
Voor een beperkte periode, u kunt nog steeds toegang tot de portal via deze URL met de Werkruimtenaam van uw eigen: https://\<naam van uw werkruimte\>. portal.mms.microsoft.com. We raden echter met behulp van Azure portal en geef ons feedback op LAUpgradeFeedback@microsoft.com op eventuele problemen met blokkeringen.

## <a name="next-steps"></a>Volgende stappen

- [Zoeken en installeren van oplossingen voor](../insights/solutions.md) met behulp van de Azure portal.
- Meer informatie over [zoeken in Logboeken in Azure portal](../log-query/portals.md).
