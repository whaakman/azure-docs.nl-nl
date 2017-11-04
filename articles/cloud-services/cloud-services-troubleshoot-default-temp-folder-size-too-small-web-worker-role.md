---
title: Standaard tijdelijke map is te klein voor een functie | Microsoft Docs
description: De functie van een cloud-service heeft een beperkte hoeveelheid ruimte voor de map TEMP. Dit artikel vindt enkele suggesties voor het vermijden van buiten de ruimte die wordt uitgevoerd.
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: ab0a7c43393aab10abbce095f801afc4ebaf6b87
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Standaard is tijdelijke map te klein voor een cloud service web/worker-rol
De standaard tijdelijke map van een cloud service worker- of webserver-rol heeft een maximale grootte van 100 MB, die volledig op een bepaald moment kan worden. Dit artikel wordt beschreven hoe u voorkomt dat met de beschikbare ruimte voor de tijdelijke map.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Waarom kan ik geen ruimte meer uitvoeren?
De standaard Windows-omgevingsvariabelen TEMP en TMP zijn beschikbaar voor de code die wordt uitgevoerd in uw toepassing. Zowel TEMP en TMP verwijzen naar één map met een maximale grootte van 100 MB. Alle gegevens die zijn opgeslagen in deze map is niet persistent over de levenscyclus van de cloudservice; Als de rolexemplaren in een cloudservice gerecycled worden, wordt de map verwijderd.

## <a name="suggestion-to-fix-the-problem"></a>Suggestie het probleem op te lossen
Een van de volgende alternatieven implementeren:

* Configureren van een resource voor lokale opslag en toegang tot deze rechtstreeks in plaats van TEMP of TMP. Aanroepen voor toegang tot een bron voor lokale opslag van code die wordt uitgevoerd in uw toepassing, de [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) methode.
* De bron van een lokale opslag configureert en wijst u de mappen TEMP en TMP om te verwijzen naar het pad van de bron van de lokale opslag. Deze wijziging moet worden uitgevoerd binnen de [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) methode.

De volgende voorbeeldcode laat zien hoe de doel-mappen wijzigen voor TEMP en TMP uit binnen de OnStart-methode:

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Lees een blog die beschrijft [het verhogen van de grootte van de Azure-functie ASP.NET tijdelijke webmap](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Meer [probleemoplossing artikelen](/?tag=top-support-issue&product=cloud-services) voor cloudservices.

Voor informatie over het oplossen van problemen met de rol van de cloud service met behulp van de diagnostics-gegevens voor Azure PaaS-computer, geven [blogreeks van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
