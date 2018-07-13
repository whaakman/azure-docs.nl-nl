---
title: Levenscyclusgebeurtenissen uit een Cloud | Microsoft Docs
description: Ontdek hoe de methoden van de levenscyclus van de cloudservicerol van een Service in de kunnen worden gebruikt in .NET
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 39b30acd-57b9-48b7-a7c4-40ea3430e451
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 56f7b5e3b303ce68868f15528d1ec200919b52aa
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001555"
---
# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>De levenscyclus van een web- of werkrol in .NET aanpassen
Wanneer u een werkrol maakt, u kunt uitbreiden de [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klasse biedt methoden voor u voor de onderdrukking waarmee u kunnen reageren op gebeurtenissen in de levensduur. Voor webrollen is deze klasse is optioneel, zodat u deze gebruiken moet om te reageren op gebeurtenissen in de levensduur.

## <a name="extend-the-roleentrypoint-class"></a>De klasse RoleEntryPoint uitbreiden
De [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klasse bevat de methoden die door Azure worden aangeroepen wanneer het **begint**, **wordt uitgevoerd**, of **stopt** een web- of worker-rol. Eventueel kunt u deze methoden voor het beheren van de initialisatie van de rol, rol afsluiten reeksen of de uitvoering van thread van de rol overschrijven. 

Bij het uitbreiden van **RoleEntryPoint**, u moet rekening houden met het volgende gedrag van de methoden:

* De [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) en [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) methoden retourneren een Booleaanse waarde, zodat het mogelijk om terug te keren **false** van deze methoden.
  
   Als uw code retourneert **false**, de rol-proces onverwacht is beëindigd, zonder dat een afsluitprocedure worden uitgevoerd dat u hebt in plaats. In het algemeen dient u te vermijden retourneren **false** uit de **OnStart** methode.
* Een niet-onderschepte uitzondering binnen een overbelasting van een **RoleEntryPoint** methode wordt beschouwd als een onverwerkte uitzondering.
  
   Als er een uitzondering optreedt in een van de levenscyclus van methoden, Azure verhoogt de [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) gebeurtenis, waarna het proces is beëindigd. Nadat uw rol offline genomen is, wordt deze opnieuw gestart door Azure. Wanneer er een niet-verwerkte uitzondering optreedt, de [stoppen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) gebeurtenis niet wordt gegenereerd, en de **OnStop** methode is niet aangeroepen.

Als uw rol niet wordt gestart, of tussen de initialiseren, bezet zijn, en moet worden gestopt Staten wordt gerecycled, kan uw code is een onverwerkte uitzondering in een van de van levenscyclusgebeurtenissen telkens wanneer de rol opnieuw wordt opgestart genereren. In dit geval gebruiken de [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) gebeurtenis om de oorzaak van de uitzondering en op de juiste manier verwerken. Uw rol kan ook worden geretourneerd van de [uitvoeren](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode, waardoor de rol opnieuw wordt gestart. Zie voor meer informatie over implementatiestatussen [algemene problemen die oorzaak rollen worden gerecycled](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [!NOTE]
> Als u de **Azure-hulpprogramma's voor Microsoft Visual Studio** voor het ontwikkelen van uw toepassing, de rol projectsjablonen automatisch uitgebreid de **RoleEntryPoint** klasse voor u, in de  *WebRole.cs* en *WorkerRole.cs* bestanden.
> 
> 

## <a name="onstart-method"></a>OnStart-methode
De **OnStart** methode wordt aangeroepen wanneer uw rolinstantie online door Azure gezet wordt. Terwijl de OnStart-code wordt uitgevoerd, de rolinstantie is gemarkeerd als **bezet** en er is geen extern verkeer worden omgeleid naar het door de load balancer. U kunt overschrijft deze methode om uit te voeren van de initialisatie van werk, zoals implementatie van gebeurtenis-handlers en starten van [Azure Diagnostics](cloud-services-how-to-monitor.md).

Als **OnStart** retourneert **waar**, het exemplaar is geïnitialiseerd en roept Azure de **RoleEntryPoint.Run** methode. Als **OnStart** retourneert **false**, de rol wordt onmiddellijk beëindigd zonder een geplande afsluiting reeksen dat wordt uitgevoerd.

Het volgende codevoorbeeld toont het onderdrukken van de **OnStart** methode. Deze methode wordt geconfigureerd en een diagnostische monitor wordt gestart wanneer de rolinstantie wordt gestart en u een overdracht van gegevens voor logboekregistratie naar een storage-account stelt:

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

## <a name="onstop-method"></a>OnStop-methode
De **OnStop** methode wordt aangeroepen nadat een rolinstantie offline is genomen door Azure en voordat het proces wordt afgesloten. U kunt deze methode voor het aanroepen van code nodig is voor uw rolinstantie naar de juiste wijze afgesloten overschrijven.

> [!IMPORTANT]
> Code die wordt uitgevoerd de **OnStop** methode heeft een beperkte tijd om te voltooien wanneer deze wordt aangeroepen voor reden dan een door de gebruiker geïnitieerde afsluiting. Nadat deze tijd is verstreken, het proces is beëindigd, dus moet u ervoor zorgen dat code in de **OnStop** methode snel kunnen worden uitgevoerd of niet wordt uitgevoerd tot voltooiing maximaal wordt toegestaan. De **OnStop** methode wordt aangeroepen na de **stoppen** gebeurtenis wordt geactiveerd.
> 
> 

## <a name="run-method"></a>Methode uitvoeren
U kunt overschrijven de **uitvoeren** methode voor het implementeren van een thread langlopende voor uw rolinstantie.

Overschrijven de **uitvoeren** methode is niet vereist; de standaardimplementatie wordt gestart een thread die altijd in slaapstand gaat. Als u overschrijf de **uitvoeren** methode, uw code moet voor onbepaalde tijd geblokkeerd. Als de **uitvoeren** methode retourneert, de rol wordt automatisch zonder problemen gerecycled; met andere woorden, Azure genereert de **stoppen** gebeurtenis en roept de **OnStop** methode zodat uw afsluiten reeksen kunnen worden uitgevoerd voordat de rol offline is gehaald.

### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Implementatie van de methoden van de levenscyclus van ASP.NET voor een Webrol
U kunt de ASP.NET-lifecycle-methoden, naast die worden verstrekt door de **RoleEntryPoint** klasse voor het beheren van initialisatie en afsluiten reeksen voor een Webrol. Dit kan zijn nuttig voor compatibiliteit van toepassing als u een bestaande ASP.NET-toepassing naar Azure zijn overzetten. De methoden van de levenscyclus van ASP.NET worden aangeroepen vanuit de **RoleEntryPoint** methoden. De **toepassing\_Start** methode wordt aangeroepen na de **RoleEntryPoint.OnStart** methode is voltooid. De **toepassing\_End** methode wordt aangeroepen voordat de **RoleEntryPoint.OnStop** methode wordt aangeroepen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [maken van een cloud service-pakket](cloud-services-model-and-package.md).

