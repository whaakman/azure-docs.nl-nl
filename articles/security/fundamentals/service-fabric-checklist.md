---
title: Controle lijst voor Azure service Fabric-beveiliging | Microsoft Docs
description: Dit artikel bevat een set controle lijsten voor Azure Fabric-beveiligings beveiliging.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2019
ms.author: tomsh
ms.openlocfilehash: c30b70d2fccb7580dcb94c2322c0ad3a52461f34
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927726"
---
# <a name="azure-service-fabric-security-checklist"></a>Controle lijst voor Azure Service Fabric-beveiliging
Dit artikel bevat een gebruiks vriendelijke controle lijst waarmee u uw Azure Service Fabric-omgeving kunt beveiligen.

## <a name="introduction"></a>Inleiding
Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric biedt ook een oplossing voor de grote uitdaging van het ontwikkelen en beheren van cloudtoepassingen. Ontwikkelaars en beheerders kunnen complexe infrastructuurproblemen voorkomen en zich concentreren op het implementeren van bedrijfsspecifieke, veeleisende werkbelastingen die schaalbaar, betrouwbaar en beheerbaar zijn.

## <a name="checklist"></a>Controlelijst
Gebruik de volgende controle lijst om ervoor te zorgen dat u geen belang rijke problemen hebt met het beheer en de configuratie van een beveiligde Azure Service Fabric-oplossing.


|Controlelijst categorie| Description |
| ------------ | -------- |
|[Op rollen gebaseerd toegangs beheer (RBAC)](../../service-fabric/service-fabric-cluster-security-roles.md) | <ul><li>Met toegangs beheer kan de Cluster beheerder de toegang tot bepaalde cluster bewerkingen voor verschillende groepen gebruikers beperken, waardoor het cluster beter is beveiligd.</li><li>Beheerders hebben volledige toegang tot beheer mogelijkheden (inclusief Lees-en schrijf mogelijkheden). </li><li> Gebruikers hebben standaard alleen lees toegang tot beheer mogelijkheden (bijvoorbeeld query mogelijkheden) en de mogelijkheid om toepassingen en services op te lossen.</li></ul>|
|[X. 509-certificaten en-Service Fabric](../../service-fabric/service-fabric-cluster-security.md) | <ul><li>[Certificaten](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates) die worden gebruikt in clusters die productie werkbelastingen uitvoeren, moeten worden gemaakt met een correct geconfigureerde Windows Server Certificate-Service of zijn verkregen van een goedgekeurde certificerings [instantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority).</li><li>Gebruik nooit [tijdelijke of test certificaten](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) in productie die zijn gemaakt met hulpprogram Ma's zoals [Makecert. exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx). </li><li>U kunt een [zelfondertekend certificaat](../../service-fabric/service-fabric-windows-cluster-x509-security.md) gebruiken, maar dit mag alleen voor test clusters en niet voor productie doeleinden.</li></ul>|
|[Cluster beveiliging](../../service-fabric/service-fabric-cluster-security.md) | <ul><li>De scenario's voor cluster beveiliging omvatten beveiliging tussen knoop punten, beveiliging van client naar knoop punt, [op rollen gebaseerd toegangs beheer (RBAC)](../../service-fabric/service-fabric-cluster-security-roles.md).</li></ul>|
|[Cluster verificatie](../../service-fabric/service-fabric-cluster-creation-via-arm.md) | <ul><li>Verifieert de [communicatie tussen](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) knoop punten voor de cluster Federatie. </li></ul>|
|[Server verificatie](../../service-fabric/service-fabric-cluster-creation-via-arm.md) | <ul><li>Verifieert de [Cluster beheer-eind punten](../../service-fabric/service-fabric-cluster-creation-via-portal.md) aan een Management-client.</li></ul>|
|[Toepassingsbeveiliging](../../service-fabric/service-fabric-cluster-creation-via-arm.md)| <ul><li>Versleuteling en ontsleuteling van toepassings configuratie waarden.</li><li>   Versleuteling van gegevens tussen knoop punten tijdens de replicatie.</li></ul>|
|[Cluster certificaat](../../service-fabric/service-fabric-windows-cluster-x509-security.md) | <ul><li>Dit certificaat is vereist voor het beveiligen van de communicatie tussen de knoop punten in een cluster.</li><li>    Stel de vinger afdruk van het primaire certificaat in in het gedeelte vinger afdruk en de secundaire in de ThumbprintSecondary variabelen.</li></ul>|
|[ServerCertificate](../../service-fabric/service-fabric-windows-cluster-x509-security.md)| <ul><li>Dit certificaat wordt weer gegeven aan de client wanneer deze probeert verbinding te maken met dit cluster. U kunt twee verschillende server certificaten gebruiken, een primaire en een secundaire voor upgrade.</li></ul>|
|ClientCertificateThumbprints| <ul><li>Dit is een set certificaten die u wilt installeren op de geverifieerde clients. </li></ul>|
|ClientCertificateCommonNames| <ul><li>Stel de algemene naam in van het eerste client certificaat voor de CertificateCommonName. De CertificateIssuerThumbprint is de vinger afdruk voor de verlener van dit certificaat. </li></ul>|
|ReverseProxyCertificate| <ul><li>Dit is een optioneel certificaat dat kan worden opgegeven als u de [omgekeerde proxy](../../service-fabric/service-fabric-reverseproxy.md)wilt beveiligen. </li></ul>|
|Key Vault| <ul><li>Wordt gebruikt voor het beheren van certificaten voor Service Fabric clusters in Azure.  </li></ul>|


## <a name="next-steps"></a>Volgende stappen

- [Aanbevolen procedures voor Service Fabric beveiliging](service-fabric-best-practices.md)
- [Upgrade proces en verwachtingen van de Cluster Service Fabric](../../service-fabric/service-fabric-cluster-upgrade.md)
- [Uw service Fabric-toepassingen beheren in Visual Studio](../../service-fabric/service-fabric-manage-application-in-visual-studio.md).
- [Inleiding tot service Fabric status model](../../service-fabric/service-fabric-health-introduction.md).
