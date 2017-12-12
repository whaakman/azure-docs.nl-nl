---
title: Resources, rollen en toegang beheren in Azure Application Insights | Microsoft Docs
description: Eigenaren, bijdragers en lezers van inzicht in uw organisatie.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: mbullwin
ms.openlocfilehash: 6e811c9b427469fa781cf1f5b7c7deff3a8e6eb3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Resources, rollen en toegangsbeheer in Application Insights
U kunt bepalen wie heeft lezen en bijwerken van toegang tot uw gegevens in Azure [Application Insights][start], met behulp van [toegangsbeheer op basis van rollen in Microsoft Azure](../active-directory/role-based-access-control-configure.md).

> [!IMPORTANT]
> Toewijzen van toegang voor gebruikers in de **resourcegroep of abonnement** die uw toepassingsresource behoort - niet in de bron zelf. Wijs de **Application Insights-onderdeelinzender** rol. Dit zorgt ervoor uniform beheer van toegang tot webtests en waarschuwingen samen met de bron van uw toepassing. [Meer informatie](#access).
> 
> 

## <a name="resources-groups-and-subscriptions"></a>Bronnen, groepen en abonnementen
Eerste, sommige definities:

* **Resource** : een exemplaar van een Microsoft Azure-service. Uw Application Insights-resource verzamelt, analyseert en geeft de telemetriegegevens van uw toepassing verzonden.  Andere soorten Azure-resources zijn web-apps, databases en virtuele machines.
  
    Overzicht van uw resources, opent u de [Azure Portal][portal], aanmelden en alle Resources op. Typ een resource vindt deel van de naam in het filterveld.
  
    ![Lijst met Azure-resources](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Resourcegroep** ] [ group] -elke resource behoort tot één groep. Een groep is een handige manier om verwante resources, met name voor toegangsbeheer te beheren. Bijvoorbeeld, in één resourcegroep kan u zetten een Web-App, een Application Insights-resource voor het bewaken van de app en een opslagresource als geëxporteerde gegevens wilt behouden.

    ![Kies Bladeren, resourcegroepen, en vervolgens kiest u een groep](./media/app-insights-resources-roles-access-control/11-group.png)

* [**Abonnement** ](https://portal.azure.com) - met Application Insights of andere Azure-resources, u zich aanmeldt bij een Azure-abonnement. Elke resourcegroep behoort tot één Azure-abonnement, waar u kiest uw pakket prijs en, als het een organisatie-abonnement, kiest u leden en hun machtigingen voor toegang.
* [**Microsoft-account** ] [ account] -de gebruikersnaam en het wachtwoord dat u aan te melden bij Microsoft Azure-abonnementen, XBox Live, Outlook.com en andere Microsoft-services.

## <a name="access"></a>Toegang beheren in de resourcegroep
Het is belangrijk te weten dat naast de bron die u voor uw toepassing hebt gemaakt, er ook afzonderlijke verborgen bronnen voor waarschuwingen en webtests zijn. Deze zijn gekoppeld aan dezelfde [resourcegroep](#resource-group) als uw toepassing. Mogelijk hebt u andere Azure-services in, zoals websites of opslag is er ook plaatsen.

![Resources in Application Insights](./media/app-insights-resources-roles-access-control/00-resources.png)

Toegang tot deze bronnen die daarom het raadzaam om te beheren:

* Toegangsbeheer op de **resourcegroep of abonnement** niveau.
* Wijs de **Application Insights-onderdeelinzender** rol aan gebruikers. Hierdoor kunnen ze webtests, waarschuwingen en Application Insights-resources, zonder dat toegang biedt tot alle andere services in de groep bewerken.

## <a name="to-provide-access-to-another-user"></a>Om toegang te bieden aan een andere gebruiker
U moet de eigenaar van rechten voor het abonnement of de resourcegroep hebben.

De gebruiker moet beschikken over een [Microsoft-Account][account], of de toegang tot hun [organisatie Microsoft-Account](../active-directory/sign-up-organization.md). U kunt bieden toegang tot personen en gebruikersgroepen die zijn gedefinieerd in Azure Active Directory.

#### <a name="navigate-to-the-resource-group"></a>Navigeer naar de resourcegroep
De gebruiker er toevoegen.

![In de resourceblade van uw toepassing, Essentials openen, opent u de resourcegroep en er instellingen/gebruikers te selecteren. Klik op Toevoegen.](./media/app-insights-resources-roles-access-control/01-add-user.png)

Of u kan hoofdmap en de gebruiker toevoegen aan het abonnement.

#### <a name="select-a-role"></a>Rol selecteren
![Selecteer een rol voor de nieuwe gebruiker](./media/app-insights-resources-roles-access-control/03-role.png)

| Rol | In de resourcegroep |
| --- | --- |
| Eigenaar |Alles, waaronder gebruikerstoegang kunt wijzigen |
| Inzender |Alles zijn, met inbegrip van alle resources kunt bewerken |
| Application Insights-onderdeelinzender |Application Insights-resources, webtests en waarschuwingen kunt bewerken |
| Lezer |Kunt bekijken maar niet van belang. |

De bewerking' omvat het maken, verwijderen en bijwerken:

* Resources
* Webtests
* Waarschuwingen
* Continue export

#### <a name="select-the-user"></a>Selecteer de gebruiker

Als de gebruiker die u wilt dat niet in de map, kunt u iedereen met een Microsoft-account kunt uitnodigen.
(Als deze services zoals Outlook.com, OneDrive, Windows Phone of XBox Live, beschikken over een Microsoft-account.)

## <a name="related-content"></a>Gerelateerde inhoud

* [Op rollen gebaseerde toegangsbeheer in Azure](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
