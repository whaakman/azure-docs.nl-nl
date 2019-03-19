---
title: Aan de slag met Cloud Foundry op Microsoft Azure | Microsoft Docs
description: OSS of Pivotal Cloud Foundry uitvoeren op Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: seanmck
manager: jeconnoc
editor: ''
tags: ''
keywords: ''
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 68ae01b814de08098c0ba6b5713f420cfebc3d97
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58001252"
---
# <a name="cloud-foundry-on-azure"></a>Cloud Foundry op Azure

Cloud Foundry is een open-source platform-as-a-service (PaaS) voor het bouwen, implementeren en gebruiken van 12-factor toepassingen die in verschillende talen en frameworks zijn ontwikkeld. Dit document beschrijft de opties die u hebt voor het uitvoeren van de Cloud Foundry op Azure en hoe u kunt aan de slag.

## <a name="cloud-foundry-offerings"></a>Cloud Foundry-aanbiedingen

Er zijn twee soorten Cloud Foundry beschikbaar om uit te voeren op Azure: open-source Cloud Foundry (OSS CF) en Pivotal Cloud Foundry (PCF). OSS CF is een volledig [open-source](https://github.com/cloudfoundry) versie van de Cloud Foundry beheerd door de Cloud Foundry Foundation. Pivotal Cloud Foundry is een enterprise-distributie van de Cloud Foundry van Pivotal Software Inc. We kijken naar enkele van de verschillen tussen de twee aanbiedingen.

### <a name="open-source-cloud-foundry"></a>Open-source Cloud Foundry

U kunt OSS Cloud Foundry op Azure implementeren met de eerste implementatie van een directeur BOSH en het implementeren van Cloud Foundry, met behulp van de [instructies hiervoor vindt u op GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md). Zie voor meer informatie over het gebruik van OSS-CF, de [documentatie](https://docs.cloudfoundry.org/) geleverd door de Cloud Foundry Foundation.

Microsoft biedt ondersteuning voor het best-effort voor OSS CF via de volgende community-kanalen:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>bosh-azure-KPI-kanaal op [Cloud Foundry Slack](https://slack.cloudfoundry.org/)
- [CF-bosh-adressenlijst](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub-problemen voor de [KPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) en [service broker](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> Het niveau van ondersteuning voor Azure-resources, zoals de virtuele machines waarop u Cloud Foundry, uitvoert, is gebaseerd op de overeenkomst van uw Azure-ondersteuning. Ondersteuning van de community van best-effort is alleen van toepassing op de Cloud Foundry-specifieke onderdelen.

### <a name="pivotal-cloud-foundry"></a>Pivotal Cloud Foundry

Pivotal Cloud Foundry omvat het dezelfde kernplatform als het OSS-distributie, samen met een set eigen beheerhulpprogramma's en enterprise-ondersteuning. PCF uitvoeren op Azure, moet u een licentie van Pivotal. De aanbieding PCF in Azure marketplace bevat een 90-daagse Evaluatielicentie.

De hulpprogramma's omvatten [Pivotal Operations Manager](https://docs.pivotal.io/pivotalcf/customizing/), een webtoepassing die vereenvoudigt de implementatie en beheer van een Cloud Foundry foundation en [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/console/), een webtoepassing voor het beheren van gebruikers en toepassingen.

Naast de ondersteuningskanalen voor OSS CF hierboven vermeld, kunt een licentie PCF u contact op met Pivotal voor ondersteuning. Microsoft en Pivotal is ook voorzien van ondersteuning voor werkstromen waarmee u kunt voor hulp contact op met beide partijen en hebben uw aanvraag op de juiste wijze worden doorgestuurd, afhankelijk van waar het probleem wordt veroorzaakt.

## <a name="azure-service-broker"></a>Azure Service Broker

Cloud Foundry moedigt het ['12-factor app'](https://12factor.net/) methodologie die bevordert een duidelijke scheiding van toepassingsprocessen staatloze en stateful services een back-up. [Service-brokers](https://docs.cloudfoundry.org/services/api.html) bieden een consistente manier worden ingericht en ondersteunende services binden aan toepassingen. De [Azure service broker](https://github.com/Azure/meta-azure-service-broker) biedt enkele van de belangrijkste Azure-services via dit kanaal, met inbegrip van Azure storage en Azure SQL.

Als u van Pivotal Cloud Foundry gebruikmaakt, de service broker is ook [beschikbaar als een tegel](https://docs.pivotal.io/azure-sb/installing.html) vanaf het centrale netwerk.

## <a name="related-resources"></a>Gerelateerde resources

### <a name="azure-devops-services-plugin"></a>Azure DevOps-Services-invoegtoepassing

Cloud Foundry is geschikt voor het ontwikkelen van flexibele software, waaronder het gebruik van continue integratie (CI) en continue levering (CD). Als u Azure DevOps-Services gebruiken om uw projecten te beheren en wilt instellen van een CI/CD-pijplijn die gericht is op Cloud Foundry, kunt u de [Azure DevOps Services Cloud Foundry Maak extensie](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension). De invoegtoepassing maakt het eenvoudig te configureren en het automatiseren van implementaties voor Cloud Foundry, of die worden uitgevoerd in Azure of een andere omgeving.

## <a name="next-steps"></a>Volgende stappen

- [Implementeren van Pivotal Cloud Foundry op Azure Marketplace](https://azure.microsoft.com/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Een app voor Cloud Foundry in Azure implementeren](./cloudfoundry-deploy-your-first-app.md)
