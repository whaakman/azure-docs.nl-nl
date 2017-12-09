---
title: Controlelijst voor de beveiliging van de Azure service fabric | Microsoft Docs
description: In dit artikel biedt een reeks controlelijst voor beveiliging van de Azure-infrastructuur.
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 526f10bab30b7d0fae796e47f5a27a58428b9a3b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-service-fabric-security-checklist"></a>Controlelijst van Azure Service Fabric-beveiliging
Dit artikel bevat een controlelijst eenvoudig te gebruiken, waarmee u uw Azure Service Fabric-omgeving te beveiligen.

## <a name="introduction"></a>Inleiding
Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric biedt ook een oplossing voor de grote uitdaging van het ontwikkelen en beheren van cloudtoepassingen. Ontwikkelaars en beheerders kunnen complexe infrastructuurproblemen voorkomen en zich concentreren op het implementeren van bedrijfsspecifieke, veeleisende werkbelastingen die schaalbaar, betrouwbaar en beheerbaar zijn.

## <a name="checklist"></a>Controlelijst
Gebruik de volgende controlelijst om u te helpen u ervoor zorgen dat u dit nog niet hebt over het hoofd eventuele belangrijke problemen in beheer en configuratie van een veilige Azure Service Fabric-oplossing gezien.


|Controlelijst categorie| Beschrijving |
| ------------ | -------- |
|[Op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles) | <ul><li>Toegangsbeheer kan de Clusterbeheerder om te beperken van toegang tot bepaalde clusterbewerkingen voor verschillende groepen gebruikers, zodat het cluster beter te beveiligen.</li><li>Beheerders hebben volledige toegang tot de beheerfuncties (inclusief lezen/schrijven-mogelijkheden). </li><li> Gebruikers hebben standaard alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.</li></ul>|
|[X.509-certificaten en Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) | <ul><li>[Certificaten](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates) gebruikt in clusters actief productieworkloads moeten worden gemaakt met behulp van een juist geconfigureerde service voor Windows Server-certificaat of verkregen van een goedgekeurde [certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority).</li><li>Gebruik nooit een [tijdelijke of certificaten testen](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) in productie die zijn gemaakt met hulpprogramma's zoals [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx). </li><li>U kunt een [zelf-ondertekend certificaat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) maar moet alleen doen voor testclusters en niet in de productieomgeving.</li></ul>|
|[Clusterbeveiliging](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) | <ul><li>De cluster security scenario's omvatten knooppunt naar beveiliging, Client-naar-knooppunt, [op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).</li></ul>|
|[Cluster-verificatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Verifieert [knooppunt naar communicatie](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) voor cluster Federatie. </li></ul>|
|[Server-verificatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Verifieert de [eindpunten voor beheer van cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal) naar een management-client.</li></ul>|
|[Toepassingsbeveiliging](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm)| <ul><li>Versleuteling en ontsleuteling van configuratiewaarden die van toepassing.</li><li>   Codering van gegevens over knooppunten tijdens de replicatie.</li></ul>|
|[Cluster-certificaat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) | <ul><li>Dit certificaat is vereist om de communicatie tussen de knooppunten op een cluster te beveiligen.</li><li>    Stel de vingerafdruk van het primaire certificaat in de sectie vingerafdruk en die van de secundaire in de ThumbprintSecondary-variabelen.</li></ul>|
|[ServerCertificate](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security)| <ul><li>Dit certificaat wordt geverifieerd op de client als er wordt geprobeerd verbinding maken met dit cluster. U kunt twee verschillende servercertificaten, een primaire en een secundaire voor een upgrade.</li></ul>|
|ClientCertificateThumbprints| <ul><li>Dit is een set van certificaten die u wilt installeren op de geverifieerde clients. </li></ul>|
|ClientCertificateCommonNames| <ul><li>De algemene naam van het certificaat van de eerste client ingesteld voor de CertificateCommonName. De CertificateIssuerThumbprint is de vingerafdruk voor de verlener van dit certificaat. </li></ul>|
|ReverseProxyCertificate| <ul><li>Dit is een optioneel certificaat dat kan worden opgegeven als u wilt beveiligen uw [Reverse Proxy](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reverseproxy). </li></ul>|
|Key Vault| <ul><li>Gebruikt voor het beheren van certificaten voor Service Fabric-clusters in Azure.  </li></ul>|


## <a name="next-steps"></a>Volgende stappen
- [Het upgradeproces service Fabric-Cluster en de verwachtingen van u](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade)
- [Het beheren van uw Service Fabric-toepassingen in Visual Studio](https://docs.microsoft.com/azure/service-fabric/service-fabric-manage-application-in-visual-studio).
- [Status van de Fabric-service model inleiding](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction).
