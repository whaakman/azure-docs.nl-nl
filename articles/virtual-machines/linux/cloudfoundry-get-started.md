---
title: Aan de slag met Cloud Foundry op Microsoft Azure | Microsoft Docs
description: OSS of belangrijke Cloud Foundry uitvoeren in Microsoft Azure
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 94fbde7707ea9a91076780fdefc3f5a827e0e7b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="cloud-foundry-on-azure"></a>Cloud Foundry op Azure

Cloud Foundry is een open source platform-as-a-service (PaaS) voor het bouwen, te implementeren en gebruiken van 12-factor-toepassingen die in verschillende talen en frameworks ontwikkeld. Dit document beschrijft de opties die u hebt voor het uitvoeren van de Cloud Foundry op Azure en hoe u kunt aan de slag.

## <a name="cloud-foundry-offerings"></a>De offerings Foundry cloud

Er zijn twee soorten Cloud Foundry beschikbaar voor uitvoeren op Azure: open source Cloud Foundry (OSS CF) en belangrijke Cloud Foundry (PCF). OSS CF is een volledig [open source](https://github.com/cloudfoundry) versie van Cloud Foundry beheerd door de Cloud Foundry Foundation. Belangrijke Cloud Foundry is een enterprise-distributie van Cloud Foundry van belangrijke Software, Inc. Kijken we enkele van de verschillen tussen de twee aanbiedingen.

### <a name="open-source-cloud-foundry"></a>Open-source Cloud Foundry

U kunt OSS Cloud Foundry in Azure door eerst een directeur BOSH implementeren en het implementeren van Cloud-Foundry implementeren met behulp van de [-instructies op GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md). Zie voor meer informatie over het gebruik van OSS CF, de [documentatie](https://docs.cloudfoundry.org/) geleverd door de Cloud Foundry Foundation.

Microsoft biedt ondersteuning voor het best-effort voor OSS CF via de volgende kanalen van de community:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>bosh-azure-KPI-kanaal op [Cloud Foundry Slack](https://slack.cloudfoundry.org/)
- [CF bosh mailinglijst](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub problemen voor de [KPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) en [service broker](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> Het niveau van ondersteuning voor uw Azure-resources, zoals de virtuele machines waarop u Cloud-Foundry uitvoert is gebaseerd op de ondersteuning van Azure-overeenkomst. Communityondersteuning best effort is alleen van toepassing op de Cloud Foundry-specifieke onderdelen.

### <a name="pivotal-cloud-foundry"></a>Belangrijke Cloud Foundry

Belangrijke Cloud Foundry bevat hetzelfde core platform als het distributiepunt OSS samen met een reeks bedrijfseigen beheerhulpprogramma's en enterprise-ondersteuning. PCF op Azure uitgevoerd, moet u een licentie van Pivotal. De aanbieding PCF vanuit Azure marketplace bevat proeflicentie is 90 dagen.

De hulpprogramma's zijn [belangrijke Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), een webtoepassing die vereenvoudigt de implementatie en beheer van een basis Cloud Foundry en [belangrijke Apps Manager](https://docs.pivotal.io/pivotalcf/console/), een webtoepassing voor het beheren van gebruikers en toepassingen.

Naast de ondersteuningskanalen voor OSS CF hierboven vermeld, kunt u Pivotal voor ondersteuning contact op met een licentie PCF. Microsoft en Pivotal hebben ingeschakeld ondersteuning werkstromen waarmee u beide partijen voor hulp contact op met uw vraag op de juiste wijze worden doorgestuurd, afhankelijk van waar het probleem wordt veroorzaakt.

## <a name="azure-service-broker"></a>Azure Service Broker

Cloud Foundry moedigt het ['twaalf factor app'](https://12factor.net/) methodologie die bijdraagt aan een schone scheiding van toepassingsprocessen staatloze en stateful services back-ups. [Service beleggingsmakelaars](https://docs.cloudfoundry.org/services/api.html) bieden een consistente manier te richten en te binden van services voor back-ups maken van toepassingen. De [Azure service broker](https://github.com/Azure/meta-azure-service-broker) biedt enkele van de belangrijkste Azure-services via dit kanaal, met inbegrip van Azure storage en Azure SQL.

Als u van belangrijke Cloud Foundry gebruikmaakt, de service broker is ook [beschikbaar als een tegel](https://docs.pivotal.io/azure-sb/installing.html) vanaf het centrale netwerk.

## <a name="related-resources"></a>Gerelateerde resources

### <a name="visual-studio-team-services-plugin"></a>Visual Studio Team Services-invoegtoepassing

Cloud Foundry is zeer geschikt voor ontwikkeling van flexibele software, waaronder het gebruik van continue integratie (CI) en continue levering (CD). Als u Visual Studio Team Services (VSTS) gebruiken voor het beheren van uw projecten en wilt instellen van een CI/CD pipeline die gericht is op Cloud Foundry, kunt u de [VSTS Cloud Foundry-opbouwextensie](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension). De invoegtoepassing kunt u eenvoudig configureren en implementaties voor Cloud-Foundry automatiseren of uitgevoerd in Azure of een andere omgeving.

## <a name="next-steps"></a>Volgende stappen

- [Foundry vanuit Azure Marketplace belangrijke Cloud implementeren](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Een app Foundry in Azure cloud implementeren](./cloudfoundry-deploy-your-first-app.md)