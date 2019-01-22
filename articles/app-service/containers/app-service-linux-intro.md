---
title: Inleiding tot App Service in Linux - Azure | Microsoft Docs
description: Meer informatie over Azure App Service onder Linux.
keywords: azure app service, linux, oss
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 1/11/2019
ms.author: msangapu;yili
ms.custom: seodec18
ms.openlocfilehash: 898d663f3ef9a71944d96b0978947d10a3e26b06
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232784"
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Inleiding tot App Service onder Linux

[Web App](../overview.md) is een volledig beheerd rekenplatform dat is geoptimaliseerd voor het hosten van websites en webtoepassingen. Klanten kunnen App Service onder Linux gebruiken voor het systeemeigen hosten van web-apps op Linux voor ondersteunde toepassingsstacks. In de volgende secties worden de toepassingsstacks vermeld die momenteel worden ondersteund.

## <a name="languages"></a>Talen

App Service onder Linux ondersteunt een aantal ingebouwde installatiekopieën om zodoende de productiviteit van ontwikkelaars te verhogen. Als de runtime die uw toepassing vereist, niet in de ingebouwde installatiekopieën wordt ondersteund, zijn er instructies voor het [bouwen van een Docker-installatiekopie](tutorial-custom-docker-image.md) voor implementatie voor Web App for Containers.

| Taal | Ondersteunde versies |
|---|---|
| Node.js | 4.4, 4.5, 4.8, 6.2, 6.6, 6.9, 6.10, 6.11, 8.0, 8.1, 8.2, 8.8, 8.9, 8.11, 9.4, 10.1,10.10 |
| Java * | Tomcat 8.5, 9.0, Java SE, WildFly 14 (voeren alle JRE 8 uit) |
| PHP | 5.6, 7.0, 7.2 |
| Python (Preview) | 2.7, 3.6, 3.7 |
| .NET Core | 1.0, 1.1, 2.0, 2.1 |
| Ruby | 2.3 |

Zie [Een Java-web-app maken in Azure App Service op Linux](https://docs.microsoft.com/azure/app-service/containers/quickstart-java) voor meer informatie.

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

* Klanten kunnen web-apps omhoog of omlaag schalen door de laag van hun [App Service-plan](https://docs.microsoft.com/azure/app-service/overview-hosting-plans?toc=%2fazure%2fapp-service-web%2ftoc.json) te wijzigen

## <a name="locations"></a>Locaties

Controleer [Azure Status Dashboard](https://azure.microsoft.com/status).

## <a name="limitations"></a>Beperkingen

In Azure Portal worden alleen functies getoond die momenteel werken voor Web App for Containers. Naarmate er meer functies mogelijk worden, zullen ze op de portal worden getoond.

Sommige functies, zoals de integratie van virtuele netwerken, verificatie van derden van Azure Active Directory of Kudu-site-extensies, zijn nog niet beschikbaar. Zodra deze functies beschikbaar zijn, worden de documentatie en de blog over de wijzigingen bijgewerkt.

App Service onder Linux wordt alleen ondersteund met de app-serviceplannen [Basic, Standard en Premium](https://azure.microsoft.com/pricing/details/app-service/plans/) en heeft geen [Free- of Shared-](https://azure.microsoft.com/pricing/details/app-service/plans/)laag. U kunt Web App for Containers niet maken met een App Service-plan waarvoor al non-Linux Web Apps wordt gehost. Verder geldt er een beperking waardoor Windows- en Linux-apps niet kunnen worden gecombineerd in dezelfde resourcegroep.

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer uw toepassing niet kan worden gestart of als u de logboeken vanuit de app wilt controleren, controleert u de Docker-logboeken in de map LogFiles. U kunt deze map openen via uw SCM-site of via FTP.
Als u `stdout` en `stderr` in het logboek wilt registreren in de container, moet u **Logboekregistratie voor Docker-container** inschakelen onder **Diagnoselogboeken**.

![Logboekregistratie inschakelen][2]

![Kudu gebruiken om Docker-logboeken weer te geven][1]

U hebt toegang tot de SCM-site via **Geavanceerde hulpmiddelen** in het menu **Ontwikkelprogramma's**.

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen helpen u om aan de slag te gaan met App Service op Linux met web-apps die zijn geschreven in verschillende talen:

* [.NET Core](quickstart-dotnetcore.md)
* [PHP](https://docs.microsoft.com/azure/app-service/containers/quickstart-php)
* [Node.js](quickstart-nodejs.md)
* [Java](quickstart-java.md)
* [Python](quickstart-python.md)
* [Ruby](quickstart-ruby.md)
* [Go](quickstart-docker-go.md)
* [App met meerdere containers](quickstart-multi-container.md)

Zie ook de volgende artikelen voor meer informatie over App Service op Linux:

* [Veelgestelde vragen over App Service voor Linux](app-service-linux-faq.md)
* [SSH-ondersteuning voor App Service op Linux](app-service-linux-ssh-support.md)
* [Faseringsomgevingen in App Service instellen](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Doorlopende implementatie met Docker Hub](app-service-linux-ci-cd.md)

Vragen kunt u posten op [ons forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
