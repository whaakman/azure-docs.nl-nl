---
title: De stroom in een Cloud Services-toepassing met Azure Diagnostics traceren | Microsoft Docs
description: Tracering berichten toevoegen aan een Azure-toepassing om u te helpen bij foutopsporing, prestaties, bewaking, analyse van het netwerkverkeer en meer meten.
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: robb
ms.openlocfilehash: 35b4a4270846c54a1ca760e803ef7adba60cf03b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>De stroom van een Cloud Services-toepassing met Azure Diagnostics traceren
Tracering is een manier om de uitvoering van uw toepassing bewaken terwijl deze wordt uitgevoerd. U kunt de [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), en [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) klassen voor informatie over de fouten en uitvoeren van toepassingen in Logboeken, tekstbestanden of andere apparaten voor latere analyse vastleggen. Zie voor meer informatie over tracering [tracering en toepassingen Instrumenteren](https://msdn.microsoft.com/library/zs6s4h68.aspx).

## <a name="use-trace-statements-and-trace-switches"></a>Trace-instructies en tracering switches gebruiken
De tracering implementeren in uw Cloud Services-toepassing door toe te voegen de [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) voor configuratie van de toepassing en het aanroepen van System.Diagnostics.Trace of System.Diagnostics.Debug aanbrengen in uw toepassingscode. Gebruik het configuratiebestand *app.config* voor werkrollen en de *web.config* voor web-rollen. Wanneer u een nieuwe gehoste service met behulp van Visual Studio-sjabloon maakt, Azure Diagnostics wordt automatisch toegevoegd aan het project en de DiagnosticMonitorTraceListener wordt toegevoegd aan het juiste configuratiebestand voor de functies die u toevoegt.

Zie voor informatie over het plaatsen van trace-instructies, [hoe: Trace-instructies toevoegen voor toepassingscode](https://msdn.microsoft.com/library/zd83saa2.aspx).

Door het plaatsen van [Trace Switches](https://msdn.microsoft.com/library/3at424ac.aspx) in uw code kunt u bepalen of de tracering plaatsvindt en hoe uitgebreide is. Hiermee kunt u de status bewaken van uw toepassing in een productieomgeving. Dit is vooral belangrijk in een zakelijke toepassing die gebruikmaakt van meerdere onderdelen op meerdere computers. Zie voor meer informatie [hoe: Trace-Switches configureren](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Configureren van de traceringslistener in een Azure-toepassing
Trace, foutopsporing en TraceSource, moet u instellen 'listeners' voor het verzamelen en registreren van de berichten die worden verzonden. Listeners verzamelen, opslaan en routeren van berichten voor tracering. Ze rechtstreeks de traceringsuitvoer naar een geschikte doel, zoals een logboek, venster of tekstbestand. Maakt gebruik van Azure Diagnostics de [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) klasse.

Voordat u de volgende procedure hebt voltooid, moet u de Azure diagnostische monitor initialiseren. Om dit te doen, Zie [diagnostische gegevens inschakelen in Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Houd er rekening mee dat als u de sjablonen die worden geleverd door Visual Studio gebruikt, de configuratie van de listener is toegevoegd automatisch voor u.

### <a name="add-a-trace-listener"></a>Een traceringslistener toevoegen
1. Open het bestand web.config of app.config voor uw rol.
2. Voeg de volgende code naar het bestand. Wijzig het kenmerk versie voor het gebruik van het versienummer van de assembly waarnaar u verwijst. De assembly-versie wijzigen niet per se met elke versie van de Azure SDK tenzij er updates beschikbaar voor deze zijn.
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > Zorg ervoor dat u hebt een projectverwijzing naar de assembly Microsoft.WindowsAzure.Diagnostics. Het versienummer in de bovenstaande xml zodat deze overeenkomen met de versie van de assembly waarnaar wordt verwezen Microsoft.WindowsAzure.Diagnostics bijwerken.
   > 
   > 
3. Sla het configuratiebestand.

Zie voor meer informatie over listeners [traceer-Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Nadat u de stappen voor het toevoegen van de listener hebt voltooid, kunt u de trace-instructies toe aan uw code kunt toevoegen.

### <a name="to-add-trace-statement-to-your-code"></a>Trace-instructie toevoegen aan uw code
1. Open een bronbestand voor uw toepassing. Bijvoorbeeld, de <RoleName>.cs-bestand voor de werkrol of Webrol.
2. Voeg de volgende gebruiksinstructie als al niet is toegevoegd:
    ```
        using System.Diagnostics;
    ```
3. Trace-instructies waarbij u wilt vastleggen van informatie over de status van uw toepassing toevoegen. Een aantal methoden kunt u de uitvoer van de Trace-instructie opmaken. Zie voor meer informatie [hoe: Trace-instructies toevoegen voor toepassingscode](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Sla het bronbestand.

