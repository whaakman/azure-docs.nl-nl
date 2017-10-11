---
title: Problemen met Cloud Services met Application Insights | Microsoft Docs
description: Informatie over het oplossen van problemen met cloud-service met behulp van Application Insights om gegevens te verwerken van Azure Diagnostics.
services: cloud-services
documentationcenter: .net
author: sbtron
manager: timlt
editor: tysonn
ms.assetid: e93f387b-ef29-4731-ae41-0676722accb6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: saurabh
ms.openlocfilehash: 4001ca908ff00b1a40829d687589080e9b07b18a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-cloud-services-using-application-insights"></a>Problemen met Cloud Services met Application Insights
Met [Azure SDK 2.8](https://azure.microsoft.com/downloads/) en Azure diagnostics extensie 1.5, kunt u Azure Diagnostics gegevens verzenden voor uw Cloudservice rechtstreeks naar Application Insights. De logboeken die is verzameld door Azure Diagnostics&mdash;inclusief toepassingslogboeken, Windows-gebeurtenislogboeken ETW-logboeken en prestatiemeteritems&mdash;kunnen worden verzonden naar Application Insights. U kunt vervolgens visualiseren van deze informatie in de gebruikersinterface van de Application Insights-portal. U kunt vervolgens Application Insights-SDK gebruiken om op te halen van inzicht in de metrische gegevens en de logboeken die afkomstig zijn van uw toepassing, evenals de systeem- en niveau van de infrastructuur die afkomstig zijn van Azure Diagnostics.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Azure Diagnostics om gegevens te verzenden naar Application Insights configureren
Volg deze stappen voor het instellen van uw cloudserviceproject Azure Diagnostics-gegevens verzenden naar Application Insights.

1. In Visual Studio Solution Explorer met de rechtermuisknop op een rol en selecteer **eigenschappen** openen van de rol designer.

    ![Solution Explorer Role-eigenschappen][1]

2. In de **Diagnostics** sectie van de rol designer, selecteer de **diagnostische gegevens verzenden naar Application Insights** optie.

    ![Rol designer diagnostische gegevens verzenden naar application insights][2]

3. Selecteer in het dialoogvenster dat verschijnt, de Application Insights-resource die u wilt de Azure diagnostics-gegevens te verzenden. Het dialoogvenster kunt u een bestaande Application Insights-resource uit uw abonnement selecteren of geef handmatig een instrumentatiesleutel voor Application Insights-resource. Zie voor meer informatie over het maken van een Application Insights-resource [Maak een nieuwe Application Insights-resource](../application-insights/app-insights-create-new-resource.md).

    ![Selecteer application insights-resource][3]

    Nadat u de Application Insights-resource hebt toegevoegd, de instrumentatiesleutel voor die bron wordt opgeslagen als een service-configuratie-instelling met de naam **APPINSIGHTS_INSTRUMENTATIONKEY**. U kunt deze configuratie-instelling voor elke configuratie van de service of de omgeving kunt wijzigen. Om dit te doen, selecteert u een andere configuratie van de **serviceconfiguratie** lijst en geeft u een nieuwe instrumentatiesleutel voor dat de configuratie.

    ![Selecteer de configuratie van de service][4]

    De **APPINSIGHTS_INSTRUMENTATIONKEY** configuratie-instelling wordt gebruikt door Visual Studio om de extensie voor diagnostische gegevens configureren met de juiste Application Insights-resource informatie tijdens de publicatie. De configuratie-instelling is een handige manier om verschillende instrumentatiesleutels voor verschillende configuraties te definiëren. Visual Studio wordt die instelling vertalen en plaats deze in de diagnostics extensie openbare configuratie tijdens het publicatieproces. De uitvoer van het pakket van Visual Studio bevat ook de openbare configuratie-XML met de juiste Application Insights-instrumentatiesleutel voor het vereenvoudigen van de extensie voor diagnostische gegevens configureren met PowerShell. De openbare configuratiebestanden in de map extensies zijn gemaakt en het patroon volgen *PaaSDiagnostics.&lt; RoleName&gt;. PubConfig.xml*. PowerShell-implementaties kunnen dit patroon gebruiken voor elke configuratie worden toegewezen aan een rol.

4) Voor het configureren van Azure diagnostics voor het verzenden van alle prestatiemeteritems en foutniveau logboeken die worden verzameld door de agent Azure diagnostische gegevens naar Application Insights inschakelen de **diagnostische gegevens verzenden naar Application Insights** optie. 

    Als u wilt meer configureren welke gegevens verzonden naar Application Insights, moet u handmatig bewerken de *diagnostics.wadcfgx* -bestand voor elke rol. Zie [Azure Diagnostics configureren om gegevens te verzenden naar Application Insights](#configure-azure-diagnostics-to-send-data-to-application-insights) voor meer informatie over het handmatig bijwerken van de configuratie.

Wanneer de cloudservice is geconfigureerd voor het verzenden van Azure diagnostics-gegevens naar application insights, kunt u deze in Azure implementeert normaal gesproken alleen voor zorgen dat de extensie Azure diagnostics is ingeschakeld. Zie voor meer informatie [publiceren van een Cloudservice met Visual Studio](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Azure diagnostics-gegevens weergeven in Application Insights
De Azure diagnostische telemetrie weergegeven in de Application Insights-resource die is geconfigureerd voor uw cloudservice.

Azure diagnostics typen logboekmap naar Application Insights-concepten op de volgende manieren:

* Prestatie-items worden weergegeven als aangepaste metrische gegevens in Application Insights.
* Windows-gebeurtenislogboeken weergegeven als traceringen en aangepaste gebeurtenissen in Application Insights.
* Toepassing Logboeken, ETW-logboeken en de logboekbestanden van de infrastructuur van diagnostische gegevens worden weergegeven als de traceringen in Application Insights.

Als u wilt gegevens van Azure diagnostics weergeven in Application Insights, een van de volgende te doen:

* Gebruik [Metrics explorer](../application-insights/app-insights-metrics-explorer.md) voor het visualiseren van eventuele aangepaste prestatiemeteritems of de aantallen voor verschillende soorten gebeurtenissen van de Windows-gebeurtenislogboek.

    ![Aangepaste metrische gegevens in Metrics Explorer][5]

* Gebruik [Search](../application-insights/app-insights-diagnostic-search.md) kunt u zoeken in de traceerlogboeken verzonden door Azure Diagnostics. Bijvoorbeeld, als een onverwerkte uitzondering heeft veroorzaakt crashes en gebruik de rol, informatie over de uitzondering wordt weergegeven in de *toepassing* kanaal van *Windows-gebeurtenislogboek*. U kunt zoeken gebruiken om te kijken naar de Windows-gebeurtenislogboek-fout en voert u de volledige stack-trace voor de uitzondering om te helpen de oorzaak van het probleem te vinden.

    ![Traceringen zoeken][6]

## <a name="next-steps"></a>Volgende stappen
* [De Application Insights-SDK toevoegen aan uw cloudservice](../application-insights/app-insights-cloudservices.md) voor het verzenden van gegevens over aanvragen, uitzonderingen, afhankelijkheden en eventuele aangepaste telemetrie van uw toepassing. In combinatie met de Azure Diagnostics-gegevens, deze informatie kunt u een compleet beeld van uw toepassing en het systeem, allemaal in dezelfde toepassing inzicht resource.  

<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
