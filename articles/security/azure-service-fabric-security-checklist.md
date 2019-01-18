---
title: Controlelijst voor de beveiliging van de Azure service fabric | Microsoft Docs
description: In dit artikel biedt een set met controlelijst voor beveiliging van Azure-infrastructuur.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2019
ms.author: tomsh
ms.openlocfilehash: 49c043853cede0a20e3d61011ded40e57d7a0303
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389425"
---
# <a name="azure-service-fabric-security-checklist"></a>Controlelijst voor beveiliging van Azure Service Fabric
Dit artikel bevat een controlelijst met eenvoudig te gebruiken waarmee u uw Azure Service Fabric-omgeving te beveiligen.

## <a name="introduction"></a>Inleiding
Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric biedt ook een oplossing voor de grote uitdaging van het ontwikkelen en beheren van cloudtoepassingen. Ontwikkelaars en beheerders kunnen complexe infrastructuurproblemen voorkomen en zich concentreren op het implementeren van bedrijfsspecifieke, veeleisende werkbelastingen die schaalbaar, betrouwbaar en beheerbaar zijn.

## <a name="checklist"></a>Controlelijst
Gebruik de volgende controlelijst om u te helpen u ervoor zorgen dat u eventuele belangrijke problemen in beheer en de configuratie van een beveiligd Azure Service Fabric-oplossing nog niet vergeten.


|Controlelijst voor categorie| Description |
| ------------ | -------- |
|[Op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles) | <ul><li>Toegangsbeheer kan de Clusterbeheerder om te beperken van toegang tot bepaalde bewerkingen voor een cluster voor verschillende groepen gebruikers, waardoor het cluster beter te beveiligen.</li><li>Beheerders hebben volledige toegang tot de mogelijkheden voor beheer (met inbegrip van de mogelijkheden voor lezen/schrijven). </li><li> Gebruikers hebben standaard alleen leestoegang tot de mogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.</li></ul>|
|[X.509-certificaten en Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) | <ul><li>[Certificaten](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates) gebruikt in clusters actieve werkbelastingen voor productie moeten worden gemaakt met een correct geconfigureerde service voor Windows Server-certificaat of verkregen van een goedgekeurde [certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority).</li><li>Gebruik nooit een [tijdelijke of voor het testen van certificaten](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) in productie die zijn gemaakt met hulpprogramma's zoals [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx). </li><li>U kunt een [zelf-ondertekend certificaat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) , maar moet alleen doen voor testclusters en niet in productie.</li></ul>|
|[Clusterbeveiliging](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) | <ul><li>De cluster security scenario's zijn knooppunt-naar-knooppunt beveiliging, Client-naar-knooppunt, [rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).</li></ul>|
|[Cluster-verificatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Verifieert [knooppunt-naar-knooppunt communicatie](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) voor cluster Federatie. </li></ul>|
|[Server-verificatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Verifieert de [eindpunten voor beheer van cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal) aan een Beheerclient.</li></ul>|
|[Toepassingsbeveiliging](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm)| <ul><li>Versleuteling en ontsleuteling van waarden van de configuratie van toepassing.</li><li>   Versleuteling van gegevens tussen knooppunten tijdens de replicatie.</li></ul>|
|[Clustercertificaat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) | <ul><li>Dit certificaat is vereist om de communicatie tussen de knooppunten op een cluster te beveiligen.</li><li>    Stel de vingerafdruk van het primaire certificaat in de sectie vingerafdruk en die de secundaire database in de variabelen ThumbprintSecondary.</li></ul>|
|[ServerCertificate](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security)| <ul><li>Dit certificaat wordt weergegeven op de client als er wordt geprobeerd verbinding maken met dit cluster. U kunt twee verschillende certificaten, een primaire en een secundaire gebruiken voor de upgrade.</li></ul>|
|ClientCertificateThumbprints| <ul><li>Dit is een set van certificaten die u wilt installeren op de geverifieerde clients. </li></ul>|
|ClientCertificateCommonNames| <ul><li>De algemene naam van het eerste certificaat ingesteld voor de CertificateCommonName. De CertificateIssuerThumbprint is de vingerafdruk voor de verlener van dit certificaat. </li></ul>|
|ReverseProxyCertificate| <ul><li>Dit is een optionele certificaat dat kan worden opgegeven als u wilt voor het beveiligen van uw [omgekeerde Proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). </li></ul>|
|Key Vault| <ul><li>Gebruikt voor het beheren van certificaten voor Service Fabric-clusters in Azure.  </li></ul>|


## <a name="next-steps"></a>Volgende stappen

- [Aanbevolen beveiligingsprocedures voor service Fabric](azure-service-fabric-security-best-practices.md)
- [Het upgradeproces service Fabric-Cluster en de verwachtingen van u](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade)
- [Beheren van uw Service Fabric-toepassingen in Visual Studio](https://docs.microsoft.com/azure/service-fabric/service-fabric-manage-application-in-visual-studio).
- [Status van de Fabric service model inleiding](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction).
