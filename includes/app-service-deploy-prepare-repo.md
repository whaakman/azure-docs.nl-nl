---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/05/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 2b9706241bd65ee75869d8b1fe58c6922d9e246a
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64951519"
---
## <a name="prepare-your-repository"></a>Voorbereiden van uw opslagplaats

Als u automatische builds van de Azure App Service Kudu maken server, zorg ervoor dat de hoofdmap van uw opslagplaats de juiste bestanden in uw project.

| Runtime | Root-bestanden |
|-|-|
| ASP.NET (alleen Windows) | _*.sln_, _*.csproj_, of _default.aspx_ |
| ASP.NET Core | _*.sln_ or _*.csproj_ |
| PHP | _index.php_ |
| Ruby (alleen Linux) | _Gemfile_ |
| Node.js | _Server.js_, _app.js_, of _package.json_ met een startscript |
| Python (alleen Windows) | _\*.PY_, _requirements.txt_, of _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, of  _iisstart.htm_ |
| Webtaken | _\<job_name > / run. \<extensie >_ onder _App\_Data/jobs/continuous_ (voor doorlopende webtaken) of _App\_Data/jobs/triggered_ (voor geactiveerd WebJobs). Zie voor meer informatie, [Kudu WebJobs documentatie](https://github.com/projectkudu/kudu/wiki/WebJobs). |
| Functions | Zie [continue implementatie voor Azure Functions](../articles/azure-functions/functions-continuous-deployment.md#requirements-for-continuous-deployment). |

Voor het aanpassen van uw implementatie, omvatten een _.deployment_ bestand in de hoofdmap van de opslagplaats. Zie voor meer informatie, [aanpassen implementaties](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) en [aangepaste implementatiescript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Als u in Visual Studio ontwikkelt, laat [Visual Studio maakt een opslagplaats voor u](/azure/devops/repos/git/creatingrepo?view=vsts&tabs=visual-studio). Het project is onmiddellijk gereed om te worden geïmplementeerd met behulp van Git.
>
>

