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
ms.openlocfilehash: 1c2a4f1e463fff278981de2297662a94cca8944e
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850785"
---
## <a name="prepare-your-repository"></a>Voorbereiden van uw opslagplaats

Als u automatische builds vanuit de App Service Kudu build-server, zorg ervoor dat hoofdmap van uw opslagplaats de juiste bestanden in uw project.

| Runtime | Root directory-bestanden |
|-|-|
| ASP.NET (alleen Windows) | _*.sln_, _*.csproj_, of _default.aspx_ |
| ASP.NET Core | _*.sln_ of _*.csproj_ |
| PHP | _index.php_ |
| Ruby (alleen voor Linux) | _Gemfile_ |
| Node.js | _Server.js_, _app.js_, of _package.json_ met een startscript |
| Python (alleen Windows) | _\*.PY_, _requirements.txt_, of _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, of  _iisstart.htm_ |
| Webtaken | _\<job_name > / uitvoeren. \<extensie >_ onder _App\_gegevens/taken/continue_ (voor doorlopende webtaken) of _App\_gegevens, taken/geactiveerd_ (voor geactiveerd WebJobs). Zie voor meer informatie [Kudu WebJobs-documentatie](https://github.com/projectkudu/kudu/wiki/WebJobs) |
| Functions | Zie [continue implementatie voor Azure Functions](../articles/azure-functions/functions-continuous-deployment.md#continuous-deployment-requirements). |

Voor het aanpassen van uw implementatie omvatten een _.deployment_ bestand in de hoofdmap van de opslagplaats. Zie voor meer informatie [implementaties aanpassen](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) en [aangepaste implementatiescript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Als u in Visual Studio ontwikkelen, laten [Visual Studio maakt een opslagplaats voor u](/vsts/git/tutorial/creatingrepo?view=vsts&tabs=visual-studio). Het project is onmiddellijk gereed voor implementatie met Git.
>
>

