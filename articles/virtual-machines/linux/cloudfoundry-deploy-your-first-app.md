---
title: Uw eerste app implementeren in de Cloud Foundry op Microsoft Azure | Microsoft Docs
description: Een toepassing implementeren in de Cloud Foundry op Azure
services: virtual-machines-linux
documentationcenter: ''
author: seanmck
manager: jeconnoc
editor: ''
tags: ''
keywords: ''
ms.assetid: 8fa04a58-56ad-4e6c-bef4-d02c80d4b60f
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: 9a0b12ec9d825fc665bd5beb89e911b80ed3889f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013871"
---
# <a name="deploy-your-first-app-to-cloud-foundry-on-microsoft-azure"></a>Uw eerste app implementeren in de Cloud Foundry op Microsoft Azure

[Cloud Foundry](https://cloudfoundry.org) is een populaire open-source-toepassingsplatform beschikbaar op Microsoft Azure. In dit artikel laten we zien hoe u kunt implementeren en beheren van een toepassing op Cloud Foundry in een Azure-omgeving.

## <a name="create-a-cloud-foundry-environment"></a>Een Cloud Foundry-omgeving maken

Er zijn verschillende opties voor het maken van een omgeving met Cloud Foundry op Azure:

- Gebruik de [Pivotal Cloud Foundry-aanbieding] [ pcf-azuremarketplace] in de Azure Marketplace om het maken van een standard-omgeving met PCF Operations Manager en de Azure Service Broker. U vindt [volledige instructies] [ pcf-azuremarketplace-pivotaldocs] bieden voor het implementeren van de marketplace in de documentatie van Pivotal.
- Maak een aangepaste omgeving door [handmatig implementeren van Pivotal Cloud Foundry][pcf-custom].
- [De open-source Cloud Foundry-pakketten rechtstreeks implementeren] [ oss-cf-bosh] door het instellen van een [BOSH](https://bosh.io) director, een virtuele machine die coördinaten van de implementatie van de Cloud Foundry-omgeving.

> [!IMPORTANT] 
> Als u PCF vanuit Azure Marketplace implementeert, moet u een notitie van de SYSTEMDOMAINURL en de beheerdersreferenties vereist voor toegang tot de Pivotal Manager van de Apps die worden beschreven in de marketplace-Implementatiehandleiding. Ze zijn nodig om deze zelfstudie te voltooien. Voor implementaties van marketplace, de SYSTEMDOMAINURL is in het formulier https://system. *IP-adres*. cf.pcfazure.com.

## <a name="connect-to-the-cloud-controller"></a>Verbinding maken met de Cloud-Controller

De Controller Cloud is het primaire toegangspunt aan een Cloud Foundry-omgeving voor het implementeren en beheren van toepassingen. De core API voor Cloud-Controller (CCAPI) is een REST-API, maar deze is toegankelijk via de verschillende hulpprogramma's. In dit geval werken met deze via de [Cloud Foundry CLI][cf-cli]. U kunt de CLI installeren op Linux, MacOS of Windows, maar als u liever niet wilt installeren op alle, is deze beschikbaar vooraf geïnstalleerd de [Azure Cloud Shell][cloudshell-docs].

Om aan te melden, voegt u vóór `api` naar de SYSTEMDOMAINURL die u hebt verkregen via de marketplace-implementatie. Omdat de standaardimplementatie een zelf-ondertekend certificaat gebruikt, moet u ook opnemen de `skip-ssl-validation` overschakelen.

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

U wordt gevraagd of u aanmelden bij de Cloud-Controller. Gebruik de beheerdersreferenties voor het account dat u hebt aangeschaft van de implementatiestappen marketplace.

Cloud Foundry biedt *organisaties* en *spaties* als naamruimten voor het isoleren van de teams en omgevingen gebruiken in een gedeelde-implementatie. De implementatie van de marketplace PCF bevat de standaard *system* org en een set met spaties gemaakt waartoe de basis-onderdelen, zoals de service automatisch schalen en de Azure service broker. Kies voor dit voorbeeld de *system* ruimte.


## <a name="create-an-org-and-space"></a>Maken van een organisatie en de ruimte

Als u typt `cf apps`, ziet u een set-toepassingen die zijn geïmplementeerd in de ruimte die op binnen het systeem org. 

Houd rekening met de *system* org gereserveerd voor systeemtoepassingen, zodat een organisatie en ruimte ook voor onze voorbeeldtoepassing maken.

```bash
cf create-org myorg
cf create-space dev -o myorg
```

De doel-opdracht gebruiken om te schakelen naar de nieuwe organisatie en de ruimte:

```bash
cf target -o testorg -s dev
```

Nu, wanneer u een toepassing implementeert, deze wordt automatisch gemaakt in de nieuwe organisatie en de ruimte. Typ om te bevestigen dat er momenteel geen apps in de nieuwe organisatie per ruimte zijn, `cf apps` opnieuw.

> [!NOTE] 
> Zie voor meer informatie over organisaties en spaties en hoe ze kunnen worden gebruikt voor op rollen gebaseerd toegangsbeheer (RBAC), de [documentatie voor Cloud Foundry][cf-orgs-spaces-docs].

## <a name="deploy-an-application"></a>Een app implementeren

We gebruiken een Cloud Foundry-voorbeeldtoepassing met de naam Hello Spring Cloud, die is geschreven in Java en op basis van de [Spring Framework](https://spring.io) en [Spring Boot](https://projects.spring.io/spring-boot/).

### <a name="clone-the-hello-spring-cloud-repository"></a>Kloon de opslagplaats Hello Spring Cloud

De voorbeeldtoepassing Hello Spring Cloud is beschikbaar op GitHub. Deze klonen op uw omgeving en wijzigen in de nieuwe map:

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-the-application"></a>De toepassing bouwen

Bouw de app via [Apache Maven](https://maven.apache.org).

```bash
mvn clean package
```

### <a name="deploy-the-application-with-cf-push"></a>De toepassing implementeren met push voor cf

U kunt de meeste toepassingen voor het gebruik van Cloud Foundry implementeren de `push` opdracht:

```bash
cf push
```

Wanneer u *push* een toepassing, Cloud Foundry detecteert het type van de toepassing (in dit geval een Java-app) en identificeert de afhankelijkheden ervan (in dit geval de Spring-framework). Deze pakketten vervolgens alles wat vereist voor het uitvoeren van uw code in een containerinstallatiekopie zelfstandige, ook wel een *druppel*. Ten slotte Cloud Foundry plant u de toepassing op een van de beschikbare machines in uw omgeving en maakt u een URL waar u deze benaderen kunt, die beschikbaar is in de uitvoer van de opdracht.

![Uitvoer van cf push-opdracht][cf-push-output]

Als u wilt zien van de toepassing hello-spring-cloud, door de opgegeven URL te openen in uw browser:

![Standaard gebruikersinterface voor Hello Spring Cloud][hello-spring-cloud-basic]

> [!NOTE] 
> Voor meer informatie over wat er tijdens het gebeurt `cf push`, Zie [hoe toepassingen worden klaargezet] [ cf-push-docs] in de Cloud Foundry-documentatie.

## <a name="view-application-logs"></a>Logboeken van de toepassing weergeven

U kunt de Cloud Foundry-CLI gebruiken om Logboeken voor een toepassing door de naam ervan weer te geven:

```bash
cf logs hello-spring-cloud
```

Standaard de logboeken gebruikt de opdracht *tail*, nieuwe logboeken laat zien als ze worden geschreven. Overzicht van nieuwe logboeken worden weergegeven, vernieuwt u de Hallo-spring-cloud-app in de browser.

Als u logboeken die al zijn geschreven, voeg de `recent` schakelen:

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-the-application"></a>De toepassing schalen

Standaard `cf push` slechts één exemplaar van uw toepassing maakt. Voor hoge beschikbaarheid garanderen en inschakelen van scale-out voor een hogere doorvoer, doorgaans wilt u meer dan één exemplaar van uw toepassingen uitvoeren. U kunt gemakkelijk schalen reeds geïmplementeerde toepassingen met behulp van de `scale` opdracht:

```bash
cf scale -i 2 hello-spring-cloud
```

Met de `cf app` op de toepassing-opdracht laat zien dat Cloud Foundry is het maken van een ander exemplaar van de toepassing. Nadat de toepassing is gestart, worden de taakverdeling van verkeer naar deze Cloud Foundry automatisch gestart.


## <a name="next-steps"></a>Volgende stappen

- [Raadpleeg de documentatie voor Cloud Foundry][cloudfoundry-docs]
- [Instellen van het Azure DevOps-Services-invoegtoepassing voor Cloud Foundry][vsts-plugin]
- [Configureren van de Microsoft Log Analytics-pijp voor Cloud Foundry][loganalytics-nozzle]

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: https://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png
