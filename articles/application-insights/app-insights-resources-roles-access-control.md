---
title: Resources, rollen en toegangsbeheer in Azure Application Insights | Microsoft Docs
description: Eigenaren, bijdragers en lezers van inzichten van uw organisatie.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: mbullwin
ms.openlocfilehash: a5ea0879e9b67a27f437b1d59a1b0998c770f5f0
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842944"
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Resources, rollen en toegangsbeheer in Application Insights

U kunt bepalen wie heeft lezen en bijwerken van toegang tot uw gegevens in Azure [Application Insights][start], met behulp van [rollen gebaseerd toegangsbeheer in Microsoft Azure](../role-based-access-control/role-assignments-portal.md).

> [!IMPORTANT]
> Toegang toewijzen aan gebruikers in de **resourcegroep of abonnement** die uw toepassingsresource behoort - niet in de resource zelf. Toewijzen de **Application Insights-onderdeelinzender** rol. Dit zorgt ervoor uniform beheer van toegang tot webtests en waarschuwingen, samen met de bron van uw toepassing. [Meer informatie](#access).

## <a name="resources-groups-and-subscriptions"></a>Resources, groepen en abonnementen

Eerste, sommige definities:

* **Resource** : een exemplaar van een Microsoft Azure-service. Uw Application Insights-resource verzamelt, analyseert en worden de telemetriegegevens die zijn verzonden vanuit uw toepassing wordt weergegeven.  Andere soorten Azure-resources zijn web-apps, databases en virtuele machines.
  
    Als u wilt zien van uw resources, opent u de [Azure-portal][portal], meld u aan en klik op alle Resources. Om een resource, typt u deel van de naam in het filterveld in.
  
    ![Lijst met Azure-resources](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Resourcegroep** ] [ group] -elke resource behoort tot één groep. Een groep is een handige manier voor het beheren van de bijbehorende resources, met name voor toegangsbeheer. Bijvoorbeeld, in één resourcegroep. kunt u plaatsen een Web-App, een Application Insights-resource voor het bewaken van de app en een opslagresource om te voorkomen dat de geëxporteerde gegevens.

* [**Abonnement** ](https://portal.azure.com) : als u wilt gebruikmaken van Application Insights of andere Azure-resources, kunt u aanmelden bij een Azure-abonnement. Elke resourcegroep behoort tot één Azure-abonnement, waarbij u uw prijs pakket kiezen en, als het abonnement van een organisatie, kiest u de leden en de toegangsmachtigingen.
* [**Microsoft-account** ] [ account] -de gebruikersnaam en het wachtwoord dat u zich aanmeldt bij Microsoft Azure-abonnementen, XBox Live, Outlook.com en andere Microsoft-services.

## <a name="access"></a> Toegang beheren in de resourcegroep

Het is belangrijk om te begrijpen dat naast de resource die u voor uw toepassing hebt gemaakt, er ook afzonderlijke verborgen resources voor waarschuwingen en webtests zijn. Ze zijn gekoppeld aan dezelfde [resourcegroep](#resource-group) als uw toepassing. Mogelijk hebt u andere Azure-services er, zoals websites of storage ook geplaatst.

Toegang tot deze resources die daarom het raadzaam om te beheren:

* Toegangsbeheer op de **resourcegroep of abonnement** niveau.
* Toewijzen de **Inzender voor Application Insights-onderdeel** rol aan gebruikers. Hierdoor kunnen ze webtests, waarschuwingen en Application Insights-resources, zonder dat toegang biedt tot alle andere services in de groep bewerken.

## <a name="to-provide-access-to-another-user"></a>Om toegang te bieden aan een andere gebruiker

U moet de eigenaar van rechten voor het abonnement of de resourcegroep hebben.

De gebruiker moet beschikken over een [Microsoft-Account][account], of toegang tot hun [organisatie Microsoft-Account](../active-directory/fundamentals/sign-up-organization.md). U kunt toegang bieden tot personen, en ook tot gebruikersgroepen die zijn gedefinieerd in Azure Active Directory.

#### <a name="navigate-to-resource-group-or-directly-to-the-resource-itself"></a>Navigeer naar de resourcegroep of rechtstreeks naar de resource zelf

Kies **toegangsbeheer (IAM)** in het menu links.

![Schermafbeelding van de Access control knop in Azure portal](./media/app-insights-resources-roles-access-control/0001-access-control.png)

Selecteer **toevoegen**

![Schermafbeelding van de toegang tot het systeemmenu met knop toevoegen gemarkeerd in het rood](./media/app-insights-resources-roles-access-control/0002-add.png)

De **machtigingen toevoegen** onderstaande weergave is voornamelijk specifiek naar Application Insights-resources, als u de machtigingen voor toegangsbeheer van een hoger niveau, zoals resourcegroepen, bekeek ziet u aanvullende niet-toepassing Insights-georiënteerde rollen.

Informatie weergeven over alle op rollen gebaseerd toegangsbeheer in Azure ingebouwde rollen gebruiken de [officiële referentie-inhoud](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

![Schermafbeelding van de gebruiker rol toegangsbeheerlijst](./media/app-insights-resources-roles-access-control/0003-user-roles.png)

#### <a name="select-a-role"></a>Rol selecteren

Indien van toepassing koppelen we aan de bijbehorende officiële documentatie.

| Rol | In de resourcegroep |
| --- | --- |
| [Eigenaar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) |Alles zijn, met inbegrip van gebruikerstoegang kunt wijzigen. |
| [Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) |Alles zijn, met inbegrip van alle resources kunt bewerken. |
| [Inzender voor Application Insights-onderdeel](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#application-insights-component-contributor) |Application Insights-resources, -webtests en -waarschuwingen kunnen worden bewerkt. |
| [Lezer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) |Kan weergeven, maar niet van belang. |
| [Application Insights Snapshot Debugger](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#application-insights-snapshot-debugger) | Wordt de gebruiker gemachtigd om functies van Application Insights Snapshot Debugger te gebruiken. Houd er rekening mee dat deze rol wordt opgenomen in de rollen niet de eigenaar of Inzender. |
| Inzender voor versiebeheer voor implementatie van Azure Service | De inzendersrol voor services implementeren via Azure-Service implementeren. |
| [Gegevens Purger](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#data-purger) | Speciale functie voor het verwijderen van persoonlijke gegevens. Zie onze [richtlijnen voor persoonlijke gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-customer-data) voor meer informatie.   |
| ExpressRoute-beheerder | Delete maken en beheren van express-routes.|
| [Inzender van log Analytics](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-contributor) | Inzender van log Analytics kan alle controlegegevens lezen en bewerken van instellingen voor controle. Bewerken van instellingen voor controle houdt het toevoegen van de VM-extensie voor virtuele machines; lezen van opslagaccountsleutels om te kunnen verzamelen van Logboeken van Azure Storage; configureren het maken en configureren van Automation-accounts; toevoegen van oplossingen en Azure diagnostics configureren op alle Azure-resources.  |
| [Lezer van log Analytics](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-reader) | De lezer van Log Analytics kan alle controlegegevens weergeven en doorzoeken en de controle-instellingen weergeven, inclusief het weergeven van de configuratie van Azure Diagnostics op alle Azure-resources. |
| masterreader | Hiermee kan een gebruiker Alles weergeven maar geen wijzigingen aanbrengen. |
| [Controlebijdrager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) | Kan alle controlegegevens lezen en de controle-instellingen bijwerken. |
| [Uitgever van de metrische gegevens controleren](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-metrics-publisher) | Hiermee schakelt u het publiceren van metrische gegevens bij Azure-resources. |
| [Controlelezer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) | Kan alle controlegegevens lezen. |
| Inzender voor resourcebeleid (preview-versie) | Supportticket gebruikers gevuld vanuit EA, met machtigingen voor het maken/wijzigen voor resourcebeleid, maken en het lezen van resources/hiërarchie.  |
| [Beheerder van gebruikerstoegang](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) | Kan een gebruiker voor het beheren van toegang voor andere gebruikers voor het Azure-resources.|
| [Inzender voor websites](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#website-contributor) | Hiermee kunt u websites (niet webabonnementen), maar niet de toegang tot beheren...|

'Bewerken' omvat het maken, verwijderen en bijwerken:

* Resources
* Webtests
* Waarschuwingen
* Continue export

#### <a name="select-the-user"></a>Selecteer de gebruiker

Als de gebruiker die u wilt niet in de directory, kunt u iedereen met een Microsoft-account kunt uitnodigen.
(Als ze gebruikmaakt van services zoals Outlook.com, OneDrive, Windows Phone of XBox Live, hebben ze een Microsoft-account.)

## <a name="related-content"></a>Gerelateerde inhoud

* [Op rollen gebaseerd toegangsbeheer in Azure](../role-based-access-control/role-assignments-portal.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
