---
title: Azure Service Fabric container application manifest voorbeelden | Microsoft Docs
description: Meer informatie over het configureren van de toepassing en service-manifest instellingen voor een Service Fabric-toepassing.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/11/2018
ms.author: ryanwi
ms.openlocfilehash: 8336f0a63f74aa0db176adbb3baf462c903ba2ec
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095576"
---
# <a name="service-fabric-application-and-service-manifest-examples"></a>Service Fabric-toepassing en service manifest van de voorbeelden
In deze sectie bevat voorbeelden van toepassing en service-manifesten. Deze voorbeelden zijn niet bedoeld om belangrijke scenario's weer te geven, maar de verschillende instellingen die beschikbaar zijn en het gebruik ervan weer te geven. 

Hier volgt een overzicht van de functies die worden weergegeven en wanneer ze een deel van de voorbeeld-manifest(s) zijn.

|Functie|Manifest|
|---|---|
|[Resourcebeheer](service-fabric-resource-governance.md)|[Reliable Services-toepassingsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest), [Container-toepassingsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Een service uitvoeren als een lokaal Administrator-account](service-fabric-application-runas-security.md)|[Reliable Services-toepassingsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Een standaardbeleid toepassen op alle service codepakketten](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages)|[Reliable Services-toepassingsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Gebruikers- en groepsgegevens beveiligings-principals maken](service-fabric-application-runas-security.md)|[Reliable Services-toepassingsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|een gegevenspakket tussen service-exemplaren delen|[Reliable Services-toepassingsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Service-eindpunten overschrijven](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)|[Reliable Services-toepassingsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Een script uitvoeren bij het opstarten van de service](service-fabric-run-script-at-service-startup.md)|[VotingWeb-servicemanifest](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Een HTTPS-eindpunt definiëren](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest)|[VotingWeb-servicemanifest](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Een configuratiepakket declareren](service-fabric-application-and-service-manifests.md)|[VotingData-servicemanifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Declareer een gegevenspakket](service-fabric-application-and-service-manifests.md)|[VotingData-servicemanifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Omgevingsvariabelen overschrijven](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Container-toepassingsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Toewijzing van de poort-naar-host configureren](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery)| [Container-toepassingsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Verificatie containerregister configureren](service-fabric-get-started-containers.md#configure-container-registry-authentication)|[Container-toepassingsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[De isolatiemodus instellen](service-fabric-get-started-containers.md#configure-isolation-mode)|[Container-toepassingsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Build-specifieke containerinstallatiekopieën van het besturingssysteem opgeven](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)|[Container-toepassingsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Omgevingsvariabelen instellen](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Container FrontEndService servicemanifest](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [Container BackEndService servicemanifest](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|
|[Een eindpunt configureren](service-fabric-get-started-containers.md#configure-communication)|[Container FrontEndService servicemanifest](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [Container BackEndService servicemanifest](service-fabric-manifest-example-container-app.md#backendservice-service-manifest), [VotingData-servicemanifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|opdrachten doorgeven aan de container|[Container FrontEndService servicemanifest](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Een certificaat importeren in een container](service-fabric-securing-containers.md)|[Container FrontEndService servicemanifest](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Volume stuurprogramma configureren](service-fabric-containers-volume-logging-drivers.md)|[Container BackEndService servicemanifest](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|

