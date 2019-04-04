---
title: Standaard TEMP-map is te klein voor een rol | Microsoft Docs
description: Een cloud service-rol heeft een beperkte hoeveelheid ruimte voor de map TEMP. In dit artikel vindt u enkele suggesties voor het om te voorkomen dat wordt uitgevoerd geen schijfruimte meer.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 7862e4d5c4dd603dacf5784df6c4194392ebc351
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918192"
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Standaard is TEMP map te klein voor een cloud service web/worker-rol
De tijdelijke standaardmap van een werknemer of web cloudservicerol heeft een maximale grootte van 100 MB, die volledig op een bepaald moment kan vormen. In dit artikel wordt beschreven hoe om te voorkomen dat met de beschikbare ruimte voor de tijdelijke map.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Waarom kan ik geen schijfruimte meer uitvoeren?
De standaard Windows-omgevingsvariabelen TEMP en TMP zijn beschikbaar voor de code die wordt uitgevoerd in uw toepassing. Zowel TEMP en TMP verwijzen naar één map met een maximale grootte van 100 MB. Alle gegevens die zijn opgeslagen in deze map is niet permanent opgeslagen in de levenscyclus van de cloudservice; Als de rolinstanties in een cloudservice gerecycled worden, wordt de map verwijderd.

## <a name="suggestion-to-fix-the-problem"></a>Suggestie om het probleem te verhelpen
Implementeer een van de volgende alternatieven:

* Configureer een lokale opslag-resource en rechtstreeks in plaats van TEMP of TMP openen. Aanroepen voor toegang tot de resource van een lokale opslag van code die wordt uitgevoerd in uw toepassing, de [RoleEnvironment.GetLocalResource](/previous-versions/azure/reference/ee772845(v=azure.100)) methode.
* Configureer een lokale opslag-resource en wijst u de mappen TEMP en TMP om te verwijzen naar het pad van de bron van de lokale opslag. Deze wijziging moet worden uitgevoerd binnen de [RoleEntryPoint.OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) methode.

Het volgende codevoorbeeld laat zien hoe de doel-mappen voor TEMP en TMP uit binnen de OnStart-methode wijzigen:

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
Lees een blog waarin wordt beschreven [verhogen van de grootte van de Azure Web Role ASP.NET tijdelijke map](https://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Meer weergeven [artikelen over probleemoplossing](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/vs-azure-tools-debugging-cloud-services-overview.md) voor cloudservices.

Voor informatie over het oplossen van problemen met de rol van de cloud service met behulp van Azure PaaS computer diagnostische gegevens, bekijken [van Kevin Williamson blogserie](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
