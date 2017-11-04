---
title: Inleiding tot op Linux-App Service | Microsoft Docs
description: Meer informatie over op Linux-Azure App Service.
keywords: Azure app service, linux, oss
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/16/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 89cb7dc488da42724f212d13f8550064ff8b9188
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Inleiding tot op Linux-Azure App Service

[Web-App](../app-service-web-overview.md) is een volledig beheerd rekenplatform dat is geoptimaliseerd voor het hosten van websites en webtoepassingen. Klanten kunnen gebruiken App Service op Linux op host WebApps systeemeigen op Linux voor stacks ondersteunde toepassing. De volgende secties vindt u de toepassing stapels die momenteel worden ondersteund.

## <a name="languages"></a>Talen

Op Linux-App Service ondersteunt een aantal ingebouwde afbeeldingen om productiviteit van ontwikkelaars. Als de runtime die vereist zijn voor uw toepassing wordt niet ondersteund in de ingebouwde installatiekopieën, er zijn instructies voor het [bouwen van uw eigen installatiekopie Docker](tutorial-custom-docker-image.md) om Web-App voor Containers te implementeren.

| Taal | Ondersteunde versies |
|---|---|
| Node.js | 4.4, 4.5, 6.2, 6.6, 6.9-6.11, 8.0, 8.1 |
| PHP | 5.6, 7.0 |
| .NET Core | 1.0, 1.1 |
| Ruby | 2.3 |

## <a name="deployments"></a>Implementaties

* FTP
* Lokale Git
* GitHub
* Bitbucket

## <a name="devops"></a>DevOps

* Faseringsomgevingen
* [Azure-Container register](https://docs.microsoft.com/azure/container-registry/container-registry-intro) en DockerHub CI/CD

## <a name="console-publishing-and-debugging"></a>-Console, publiceren en foutopsporing

* Omgevingen
* Implementaties
* Basic-console
* SSH

## <a name="scaling"></a>Schalen

* Klanten kunnen worden geschaald web-apps omhoog en omlaag door het wijzigen van de laag van hun [App Service-abonnement](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview?toc=%2fazure%2fapp-service-web%2ftoc.json)

## <a name="locations"></a>Locaties

Controleer de [Azure Status Dashboard](https://azure.microsoft.com/status).

## <a name="limitations"></a>Beperkingen

De Azure-portal bevat alleen de functies die nu ook voor Web-App voor Containers werken. Als er meer functies inschakelt, wordt ze zichtbaar zijn in de portal.

Sommige functies, zoals de integratie van virtueel netwerk, verificatie van Azure Active Directory/van derden of Kudu-site-uitbreidingen zijn nog niet beschikbaar. Zodra deze functies beschikbaar zijn, werken wij onze documentatie en blog over de wijzigingen.

Op Linux-App Service wordt alleen ondersteund met [Basic en Standard](https://azure.microsoft.com/pricing/details/app-service/plans/) app service-abonnementen en niet beschikt over een [Free of Shared](https://azure.microsoft.com/pricing/details/app-service/plans/) laag. Hieronder vindt u ook belangrijke beperkingen voor op Linux-App Service:

* U kunt de Web-App voor Containers maken in een App Service-abonnement al als host voor niet - Linux-Web-Apps.
* Wanneer u Web-App voor Containers in een bronnengroep met niet - Linux-Web-Apps, moet u een App Service-abonnement maken in een andere regio dan de bestaande App Service-abonnement.

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer uw toepassing niet kan worden gestart of u wilt controleren van de logboekregistratie van uw app, Controleer u dat de Docker-Logboeken in de map met logboekbestanden. U kunt toegang tot deze map via uw SCM-site of via de FTP.
Logboek de `stdout` en `stderr` van de container, moet u inschakelen **Docker-Container logboekregistratie** onder **diagnostische logboeken**.

![Logboekregistratie inschakelen][2]

![Met behulp van Kudu Docker-logboeken weergeven][1]

U toegang hebt tot de site SCM bij **geavanceerde hulpmiddelen** in de **ontwikkelingsprogramma's** menu.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppelingen aan de slag met App-Service op Linux. U kunt vragen en problemen op boeken [ons forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Het gebruik van een aangepaste Docker-installatiekopie voor Web-App voor Containers](quickstart-custom-docker-image.md)
* [Met behulp van .NET Core in Azure App Service op Linux](quickstart-dotnetcore.md)
* [Met behulp van Ruby in Azure App Service op Linux](quickstart-ruby.md)
* [Web-App voor Containers Veelgestelde vragen over Azure App Service](app-service-linux-faq.md)
* [SSH-ondersteuning voor Azure App Service op Linux](app-service-linux-ssh-support.md)
* [Faseringsomgevingen in Azure App Service instellen](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Docker Hub continue implementatie met Web-App voor Containers](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
