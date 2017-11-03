---
title: Cloudservice lifecycle gebeurtenissen verwerken | Microsoft Docs
description: Meer informatie over hoe de methoden van de levenscyclus van een Cloudservice-rol kunnen worden gebruikt in .NET
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 39b30acd-57b9-48b7-a7c4-40ea3430e451
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: eb78c05df3b3cdf3887334c11bdabd5cebb74747
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>De levenscyclus van een web- of werkrol in .NET aanpassen
Wanneer u een werkrol maakt, breid u de [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klasse biedt methoden voor u overschrijven waarmee u kunnen reageren op de levenscyclus van gebeurtenissen. Deze klasse is voor webrollen optioneel, zodat u deze gebruiken moet om te reageren op de levenscyclus van gebeurtenissen.

## <a name="extend-the-roleentrypoint-class"></a>De klasse RoleEntryPoint uitbreiden
De [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klasse bevat methoden die door Azure worden aangeroepen wanneer het **begint**, **wordt uitgevoerd**, of **stopt** een web- of worker-rol. U kunt eventueel deze methoden voor het beheren van de initialisatie van de rol of functie afsluiten reeksen de uitvoeringsthread van de rol overschrijven. 

Wanneer u uitbreidt **RoleEntryPoint**, u moet rekening houden met het volgende gedrag van de methoden:

* De [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) en [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) methoden retourneren een Booleaanse waarde, zodat het mogelijk om terug te keren **false** van deze methoden.
  
   Als uw code retourneert **false**, rol proces is onverwacht beëindigd, zonder dat elke afsluitvolgorde uitgevoerd mogelijk hebt u in plaats. In het algemeen moet u vermijden retourneren **false** van de **OnStart** methode.
* Een onbekende uitzondering opgetreden binnen een overload van een **RoleEntryPoint** methode wordt behandeld als een onverwerkte uitzondering.
  
   Als er een uitzondering optreedt in een van de levenscyclus van methoden, Azure verhoogt de [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) gebeurtenis en het proces wordt beëindigd. Nadat uw rol heeft offline is gezet, wordt deze opnieuw starten door Azure. Wanneer er een niet-verwerkte uitzondering optreedt, de [stoppen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) gebeurtenis wordt niet gegenereerd, en de **OnStop** methode is niet aangeroepen.

Als de rol niet gestart wordt of is recycling tussen de initialisatie zijn druk is, en moet worden gestopt statussen, kan uw code worden er wordt een onverwerkte uitzondering in een van de levenscyclus van gebeurtenissen telkens wanneer de functie opnieuw wordt opgestart. In dit geval gebruiken de [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) gebeurtenis om de oorzaak van de uitzondering en op juiste manier verwerken. Uw rol kan ook worden teruggekeerd uit het [uitvoeren](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode die ervoor zorgt dat de rol op te starten. Zie voor meer informatie over implementatiestatussen [veelvoorkomende problemen die oorzaak rollen recyclen](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [!NOTE]
> Als u de **Azure-hulpprogramma's voor Microsoft Visual Studio** voor het ontwikkelen van uw toepassing, de projectsjablonen rol automatisch uitbreiden de **RoleEntryPoint** -klasse voor u, in de *WebRole.cs* en *WorkerRole.cs* bestanden.
> 
> 

## <a name="onstart-method"></a>OnStart-methode
De **OnStart** methode wordt aangeroepen wanneer uw rolexemplaar online is gebracht door Azure. Terwijl de OnStart-code wordt uitgevoerd, de rolinstantie is gemarkeerd als **bezet** en er is geen externe verkeer wordt omgeleid naar het door de load balancer. U kunt deze methode initialisatie taken, zoals het implementeren van gebeurtenis-handlers en worden gestart overschrijven [Azure Diagnostics](cloud-services-how-to-monitor.md).

Als **OnStart** retourneert **true**, het exemplaar is geïnitialiseerd en Azure roept de **RoleEntryPoint.Run** methode. Als **OnStart** retourneert **false**, de rol wordt onmiddellijk beëindigd zonder eventuele reeksen geplande afsluiting wordt uitgevoerd.

Het volgende codevoorbeeld toont het onderdrukken van de **OnStart** methode. Deze methode wordt geconfigureerd en een diagnostische monitor wordt gestart wanneer de rolinstantie wordt gestart en u een overdracht van de logboekgegevens naar een opslagaccount stelt:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop methode
De **OnStop** methode wordt aangeroepen nadat een rolinstantie offline is genomen door Azure en voordat het proces wordt afgesloten. U kunt deze methode voor het aanroepen van de code is vereist voor uw rolexemplaar aan de juiste wijze afgesloten overschrijven.

> [!IMPORTANT]
> Code die wordt uitgevoerd de **OnStop** methode heeft een beperkte tijd voltooid wanneer deze wordt aangeroepen voor andere doeleinden dan een afsluiten van de gebruiker is gestart. Nadat deze tijd is verstreken, het proces wordt beëindigd, dus moet u ervoor zorgen dat code in de **OnStop** methode snel kan worden uitgevoerd of niet volledig uitgevoerd maximaal wordt toegestaan. De **OnStop** methode wordt aangeroepen nadat de **stoppen** gebeurtenis is opgetreden.
> 
> 

## <a name="run-method"></a>Methode niet uitvoeren
U kunt vervangen de **uitvoeren** methode voor het implementeren van een langlopende-thread voor uw rolexemplaar.

Overschrijven de **uitvoeren** methode is niet vereist; de standaardimplementatie wordt gestart van een thread die slaapstand permanent verloren gaat. Als u overschrijven de **uitvoeren** methode, uw code dient te blokkeren voor onbepaalde tijd. Als de **uitvoeren** methode retourneert, de rol wordt automatisch probleemloos gerecycled; met andere woorden, Azure genereert de **stoppen** gebeurtenis en roept de **OnStop** methode zodat uw reeksen afsluiten mogen worden uitgevoerd voordat de rol offline is gehaald.

### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>De methoden van de levenscyclus van ASP.NET voor een Webrol implementeren
U kunt de ASP.NET-lifecycle-methoden, naast die worden verstrekt door de **RoleEntryPoint** klasse voor het beheren van initialisatie en afsluiten die gebruikmaken van een Webrol. Dit kan zijn handig voor compatibiliteit als u bij het overbrengen van een bestaande ASP.NET-toepassing naar Azure. De methoden van de levenscyclus van ASP.NET worden aangeroepen vanuit de **RoleEntryPoint** methoden. De **toepassing\_Start** methode wordt aangeroepen nadat de **RoleEntryPoint.OnStart** methode is voltooid. De **toepassing\_End** methode wordt aangeroepen voordat de **RoleEntryPoint.OnStop** methode wordt aangeroepen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [maken van een cloud service-pakket](cloud-services-model-and-package.md).

