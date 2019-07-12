---
title: Snapshot Debugger voor .NET-apps in Azure App Service inschakelen | Microsoft Docs
description: Snapshot Debugger voor .NET-apps in Azure App Service inschakelen
services: application-insights
documentationcenter: ''
author: brahmnes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/07/2019
ms.author: bfung
ms.openlocfilehash: 3e8ce3c2eff7b1f7184bb37f141e62563d4fe714
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612689"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Snapshot Debugger voor .NET-apps in Azure App Service inschakelen

Snapshot Debugger werkt momenteel voor ASP.NET en ASP.NET Core-apps die worden uitgevoerd op Azure App Service in Windows service-abonnementen.

## <a id="installation"></a> Snapshot Debugger inschakelen
Als u wilt Snapshot Debugger inschakelen voor een app, volg de onderstaande instructies. Als u een ander type Azure-service uitvoert, vindt hier u instructies voor het inschakelen van Snapshot Debugger op andere ondersteunde platforms:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric-services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure virtuele Machines en virtuele-machineschaalsets](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [On-premises virtuele of fysieke machines](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Als u een preview-versie van .NET Core, volgt u de instructies voor het [Snapshot Debugger inschakelen voor andere omgevingen](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) eerst om op te nemen de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet inpakken met de toepassing en voltooi de rest van de onderstaande instructies. 

Application Insights Snapshot Debugger vooraf is geïnstalleerd als onderdeel van de runtime van de App-Services, maar u moet inschakelen om momentopnamen ophalen voor uw App Service-app. Nadat u een app hebt geïmplementeerd, zelfs als u de Application Insights SDK hebt opgenomen in de broncode, volgt u de stappen hieronder om de snapshot debugger inschakelen.

1. Ga naar de **App Services** deelvenster in de Azure-portal.
2. Navigeer naar **instellingen > Application Insights** deelvenster.

   ![App Insights inschakelen in App Services-portal](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Ofwel de instructies in het deelvenster te maken van een nieuwe resource of Selecteer een bestaande App Insights-resource voor het bewaken van uw app. Ook voor zorgen dat beide switches voor Snapshot Debugger zijn **op**.

   ![App Insights-site-extensie toevoegen][Enablement UI]

4. Snapshot Debugger is nu ingeschakeld met behulp van een App-instelling van de App-Services.

    ![App-instelling voor Snapshot Debugger][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Snapshot Debugger uitschakelen

Volg dezelfde stappen als voor **Snapshot Debugger inschakelen**, maar beide switches overschakelen voor Snapshot Debugger te **uit**.
Het is raadzaam dat u Snapshot Debugger ingeschakeld op al uw apps hebt om de diagnostische gegevens over uitzonderingen voor de toepassing.

## <a name="next-steps"></a>Volgende stappen

- Genereert verkeer naar uw toepassing die een uitzondering kunt activeren. Wacht 10 tot 15 minuten voor momentopnamen worden verzonden naar de Application Insights-exemplaar.
- Zie [momentopnamen](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) in Azure portal.
- Zie voor hulp bij het oplossen van problemen met Snapshot Debugger [Snapshot Debugger probleemoplossing](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

