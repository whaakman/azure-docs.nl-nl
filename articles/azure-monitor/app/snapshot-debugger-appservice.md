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
ms.author: brahmnes
ms.openlocfilehash: 7ff93dffe2dd82bdbba204b4235a297b337438f5
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57864036"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Snapshot Debugger voor .NET-apps in Azure App Service inschakelen

Snapshot Debugger werkt momenteel voor ASP.NET en ASP.NET Core-apps die worden uitgevoerd op Azure App Service in Windows service-abonnementen.

## <a id="installation"></a> Snapshot Debugger inschakelen
Als u wilt Snapshot Debugger inschakelen voor een app, volg de onderstaande instructies. Als u een ander type Azure-service uitvoert, vindt hier u instructies voor het inschakelen van Snapshot Debugger op andere ondersteunde platforms:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric-services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure virtuele Machines en virtuele-machineschaalsets](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [On-premises virtuele of fysieke machines](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

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

* [Werken met Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

