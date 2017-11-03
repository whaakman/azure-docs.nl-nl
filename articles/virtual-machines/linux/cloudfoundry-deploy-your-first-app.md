---
title: Uw eerste app implementeren in Cloud Foundry op Microsoft Azure | Microsoft Docs
description: Een toepassing implementeert op Cloud Foundry op Azure
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 8fa04a58-56ad-4e6c-bef4-d02c80d4b60f
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: b617127fc0a3f8dcae293e356ea669edcfa5deff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-first-app-to-cloud-foundry-on-microsoft-azure"></a>Uw eerste app implementeren in Cloud Foundry op Microsoft Azure

[Foundry cloud](http://cloudfoundry.org) is een platform voor populaire open-source beschikbaar op Microsoft Azure. In dit artikel, laten we zien hoe te implementeren en beheren van een toepassing in de Cloud Foundry in een Azure-omgeving.

## <a name="create-a-cloud-foundry-environment"></a>Een Cloud Foundry-omgeving maken

Er zijn verschillende opties voor het maken van een Cloud Foundry-omgeving op Azure:

- Gebruik de [belangrijke Cloud Foundry aanbieding] [ pcf-azuremarketplace] in Azure Marketplace voor het maken van een standaard omgeving die PCF Ops Manager en de Azure Service Broker. U vindt [volledige instructies] [ pcf-azuremarketplace-pivotaldocs] bieden in de documentatie van belangrijke voor het implementeren van de marketplace.
- Maakt een aangepaste omgeving door [belangrijke Cloud Foundry handmatig implementeren][pcf-custom].
- [Implementeert de open source Cloud Foundry-pakketten rechtstreeks] [ oss-cf-bosh] door het instellen van een [BOSH](http://bosh.io) directeur, een virtuele machine die de implementatie van de Cloud Foundry-omgeving coördineert.

> [!IMPORTANT] 
> Als u PCF vanuit Azure Marketplace implementeert, noteer de SYSTEMDOMAINURL en de beheerdersreferenties nodig voor toegang tot de belangrijke Manager van de Apps die worden beschreven in de marketplace-Implementatiehandleiding. Ze zijn nodig om deze zelfstudie te voltooien. Voor implementaties van marketplace is de SYSTEMDOMAINURL in het formulier https://system. *IP-adres*. cf.pcfazure.com.

## <a name="connect-to-the-cloud-controller"></a>Verbinding maken met de domeincontroller van de Cloud

De Cloud-Controller is het primaire toegangspunt aan een Cloud Foundry-omgeving voor het implementeren en beheren van toepassingen. De core Cloud Controller API (CCAPI) is een REST-API, maar het is toegankelijk via verschillende hulpprogramma's. In dit geval we ermee via de [Cloud Foundry CLI][cf-cli]. U kunt de CLI installeren op Linux-, Mac OS- of Windows, maar als u liever niet te installeren op alle, is deze beschikbaar vooraf zijn geïnstalleerd de [Azure Cloud Shell][cloudshell-docs].

Meld u bij, Voeg `api` naar de SYSTEMDOMAINURL die u hebt verkregen via de marketplace-implementatie. Omdat de standaardimplementatie een zelfondertekend certificaat gebruikt, moet u ook opnemen de `skip-ssl-validation` overschakelen.

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

U wordt gevraagd of u zich aanmelden bij de domeincontroller van de Cloud. Gebruik de beheerdersreferenties voor het account dat u hebt aangeschaft van de implementatiestappen marketplace.

Cloud Foundry biedt *organisaties* en *spaties* als naamruimten voor het isoleren van de teams en omgevingen binnen een gedeelde implementatie. De implementatie van de marketplace PCF bevat de standaard *system* org en een set spaties gemaakt van de basisonderdelen, zoals de service automatisch schalen en de Azure service broker. Kies nu de *system* ruimte.


## <a name="create-an-org-and-space"></a>Een organisatie en de ruimte maken

Als u typt `cf apps`, ziet u een set systeemtoepassingen die zijn geïmplementeerd in de systeemruimte in het bijzonder systeem 

U dient te houden met de *system* org gereserveerd voor systeemtoepassingen, dus maak een organisatie en de ruimte voor de voorbeeldtoepassing bevatten.

```bash
cf create-org myorg
cf create-space dev -o myorg
```

Gebruik de opdracht doel overschakelen naar de nieuwe organisatie en de ruimte:

```bash
cf target -o testorg -s dev
```

Nu, wanneer u een toepassing implementeert, deze automatisch is gemaakt in de nieuwe organisatie en de ruimte. Typ om te bevestigen dat er momenteel geen apps in de nieuwe org per ruimte zijn, `cf apps` opnieuw.

> [!NOTE] 
> Zie voor meer informatie over organisaties en spaties en hoe ze kunnen worden gebruikt voor op rollen gebaseerde toegangsbeheer (RBAC), de [Cloud Foundry documentatie][cf-orgs-spaces-docs].

## <a name="deploy-an-application"></a>Een app implementeren

We gebruiken een voorbeeldtoepassing Cloud Foundry aangeroepen Hello Spring Cloud, die is geschreven in Java en op basis van de [Spring Framework](http://spring.io) en [Spring Boot](http://projects.spring.io/spring-boot/).

### <a name="clone-the-hello-spring-cloud-repository"></a>Kloon de opslagplaats Hello Spring-Cloud

De voorbeeldtoepassing Hello Spring Cloud is beschikbaar op GitHub. Aan uw omgeving, maar kloon en naar de nieuwe map wijzigen:

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-the-application"></a>De toepassing bouwen

Maakt de app met [Apache Maven](http://maven.apache.org).

```bash
mvn clean package
```

### <a name="deploy-the-application-with-cf-push"></a>De toepassing implementeren met cf push

U kunt de meeste toepassingen voor het gebruik van Cloud Foundry implementeren de `push` opdracht:

```bash
cf push
```

Wanneer u *push* een toepassing, Cloud Foundry detecteert het type toepassing (in dit geval een Java-app) en de afhankelijkheden ervan (in dit geval het framework Spring) identificeert. Vervolgens pakketten alles vereist voor het uitvoeren van uw code in de container installatiekopie van een zelfstandige, ook wel een *droplet*. Ten slotte Cloud Foundry plant u de toepassing op een van de beschikbare computers in uw omgeving en maakt u een URL waarop u bereiken kan, die beschikbaar is in de uitvoer van de opdracht.

![Uitvoer van cf push-opdracht][cf-push-output]

Overzicht van de toepassing hello-spring-cloud, de opgegeven URL te openen in uw browser:

![Standaardgebruikersinterface voor Hallo Spring Cloud][hello-spring-cloud-basic]

> [!NOTE] 
> Voor meer informatie over wat er tijdens gebeurt `cf push`, Zie [hoe toepassingen worden voorbereid] [ cf-push-docs] in de Cloud Foundry-documentatie.

## <a name="view-application-logs"></a>Logboeken van de toepassing bekijken

De Cloud Foundry CLI kunt u Logboeken voor een toepassing bekijken met de naam:

```bash
cf logs hello-spring-cloud
```

Standaard de logboeken van de opdracht gebruikt *tail*, waarmee nieuwe logboeken wordt weergegeven als ze zijn geschreven. Overzicht van nieuwe logboeken worden weergegeven, vernieuw het Hallo-spring-cloud-app in de browser.

Om Logboeken te raadplegen die al zijn geschreven, voeg de `recent` switch:

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-the-application"></a>De toepassing schalen

Standaard `cf push` slechts één exemplaar van uw toepassing maakt. Om te zorgen voor hoge beschikbaarheid en scale-out voor een hogere doorvoer inschakelt, doorgaans wilt u meer dan één exemplaar van uw toepassingen worden uitgevoerd. U kunt eenvoudig schalen reeds geïmplementeerde toepassingen met behulp van de `scale` opdracht:

```bash
cf scale -i 2 hello-spring-cloud
```

Met de `cf app` opdracht bij de toepassing toont Cloud Foundry maken van een ander exemplaar van de toepassing. Nadat de toepassing is gestart, worden de taakverdeling verkeer naar deze Cloud Foundry automatisch gestart.


## <a name="next-steps"></a>Volgende stappen

- [De Cloud Foundry-documentatie te lezen][cloudfoundry-docs]
- [De Visual Studio Team Services-invoegtoepassing voor Cloud Foundry instellen][vsts-plugin]
- [De Microsoft Log Analytics pijp configureren voor Cloud Foundry][loganalytics-nozzle]

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: http://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png