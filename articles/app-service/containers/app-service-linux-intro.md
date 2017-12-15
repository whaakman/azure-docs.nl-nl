---
title: Inleiding tot App Service onder Linux | Microsoft Docs
description: Meer informatie over Azure App Service onder Linux.
keywords: azure app service, linux, oss
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
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Inleiding tot App Service onder Linux

[Web App](../app-service-web-overview.md) is een volledig beheerd rekenplatform dat is geoptimaliseerd voor het hosten van websites en webtoepassingen. Klanten kunnen App Service onder Linux gebruiken voor het systeemeigen hosten van web-apps op Linux voor ondersteunde toepassingsstacks. In de volgende secties worden de toepassingsstacks vermeld die momenteel worden ondersteund.

## <a name="languages"></a>Talen

App Service onder Linux ondersteunt een aantal ingebouwde installatiekopieën om zodoende de productiviteit van ontwikkelaars te verhogen. Als de runtime die uw toepassing vereist, niet in de ingebouwde installatiekopieën wordt ondersteund, zijn er instructies voor het [bouwen van een Docker-installatiekopie](tutorial-custom-docker-image.md) voor implementatie voor Web App for Containers.

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
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro) en DockerHub-CI/CD

## <a name="console-publishing-and-debugging"></a>Console, publiceren en foutopsporing

* Omgevingen
* Implementaties
* Basisconsole
* SSH

## <a name="scaling"></a>Schalen

* Klanten kunnen web-apps omhoog of omlaag schalen door de laag van hun [App Service-plan](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview?toc=%2fazure%2fapp-service-web%2ftoc.json) te wijzigen

## <a name="locations"></a>Locaties

Controleer [Azure Status Dashboard](https://azure.microsoft.com/status).

## <a name="limitations"></a>Beperkingen

In Azure Portal worden alleen functies getoond die momenteel werken voor Web App for Containers. Naarmate er meer functies mogelijk worden, zullen ze op de portal worden getoond.

Sommige functies, zoals de integratie van virtuele netwerken, verificatie van derden van Azure Active Directory of Kudu-site-extensies, zijn nog niet beschikbaar. Zodra deze functies beschikbaar zijn, worden de documentatie en de blog over de wijzigingen bijgewerkt.

App Service onder Linux wordt alleen ondersteund met de app-serviceplannen [Basic en Standard](https://azure.microsoft.com/pricing/details/app-service/plans/) en heeft geen [Free- of Shared-](https://azure.microsoft.com/pricing/details/app-service/plans/)laag. De volgende beperkingen zijn ook belangrijk voor App Service onder Linux:

* U kunt Web App for Containers niet maken met een App Service-plan waarvoor al non-Linux Web Apps wordt gehost.
* Als u Web App for Containers maakt in een resourcegroep die non-Linux Web Apps bevat, moet u een App Service-plan maken in een andere regio dan die van het bestaande plan.

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer uw toepassing niet kan worden gestart of als u de logboeken vanuit de app wilt controleren, controleert u de Docker-logboeken in de map LogFiles. U kunt deze map openen via uw SCM-site of via FTP.
Als u `stdout` en `stderr` in het logboek wilt registreren in de container, moet u **Logboekregistratie voor Docker-container** inschakelen onder **Diagnoselogboeken**.

![Logboekregistratie inschakelen][2]

![Kudu gebruiken om Docker-logboeken weer te geven][1]

U hebt toegang tot de SCM-site via **Geavanceerde hulpmiddelen** in het menu **Ontwikkelprogramma's**.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppelingen om aan de slag te gaan met App Service onder Linux. Vragen kunt u posten op [ons forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Een aangepaste Docker-installatiekopie uitvoeren voor Web App for Containers](quickstart-custom-docker-image.md)
* [.NET Core gebruiken in Azure App Service onder Linux](quickstart-dotnetcore.md)
* [Ruby gebruiken in Azure App Service onder Linux](quickstart-ruby.md)
* [Web App for Containers van Azure App Service: veelgestelde vragen](app-service-linux-faq.md)
* [SSH-ondersteuning voor Azure App Service onder Linux](app-service-linux-ssh-support.md)
* [Faseringsomgevingen in Azure App Service instellen](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Continue implementatie van Docker Hub met Web App for Containers](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
