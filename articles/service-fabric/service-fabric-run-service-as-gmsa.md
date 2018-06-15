---
title: Uitvoeren van een Azure Service Fabric-service onder een beheerd serviceaccount | Microsoft Docs
description: Informatie over het uitvoeren van een service als een gMSA op een zelfstandige Service Fabric Windows cluster.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: mfussell
ms.openlocfilehash: 5f93285061708172b9b6ac40dc97fce08f7b2a86
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206710"
---
# <a name="run-a-service-as-a-group-managed-service-account"></a>Een service uitvoeren als door een groep beheerd serviceaccount
Op een zelfstandige cluster van Windows Server, kunt u een service uitvoeren als een groep beheerde serviceaccount (gMSA) voor met behulp van een RunAs-beleid.  Service Fabric-toepassingen worden standaard uitgevoerd onder het account waaronder het Fabric.exe-proces wordt uitgevoerd onder. Uitvoeren van toepassingen onder verschillende accounts, zelfs in een gedeelde gehoste omgeving zorgt ervoor dat ze van elkaar veiliger. Houd er rekening mee dat dit maakt gebruik van Active Directory on-premises binnen uw domein en niet Azure Active Directory (Azure AD). Met behulp van een gMSA, is er geen wachtwoord of versleutelde wachtwoord opgeslagen in het toepassingsmanifest.  U kunt ook uitvoeren met een service als een [Active Directory-gebruiker of groep](service-fabric-run-service-as-ad-user-or-group.md).

Het volgende voorbeeld ziet u het maken van een beheerd serviceaccount voor aangeroepen *svc-Test$*; beheerde serviceaccount implementeren op de clusterknooppunten; en het configureren van de user principal.

Vereisten:
- Het domein moet een KDS-hoofdsleutel.
- Het domein moet op een Windows Server 2012 of hoger functionaliteitsniveau.

1. Vraagt u een Active Directory-domein-beheerder maakt een groep beheerde service account met behulp van de `New-ADServiceAccount` commandlet en zorg ervoor dat de `PrincipalsAllowedToRetrieveManagedPassword` bevat alle knooppunten van het service fabric. `AccountName`, `DnsHostName`, en `ServicePrincipalName` moeten uniek zijn.

    ```poweshell
    New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
    ```

2. Op elk van de Service Fabric clusterknooppunten (bijvoorbeeld `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), installeren en testen van de gMSA.
    
    ```powershell
    Add-WindowsFeature RSAT-AD-PowerShell
    Install-AdServiceAccount svc-Test$
    Test-AdServiceAccount svc-Test$
    ```

3. De User principal configureren en configureer de RunAsPolicy om te verwijzen naar de gebruiker.
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
          <ConfigOverrides />
          <Policies>
              <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
          </Policies>
        </ServiceManifestImport>
      <Principals>
        <Users>
          <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
        </Users>
      </Principals>
    </ApplicationManifest>
    ```

> [!NOTE] 
> Als u een Run as-beleid op een service toepassen en de servicemanifest eindpunt resources met de HTTP-protocol declareert, moet u een **SecurityAccessPolicy**.  Zie voor meer informatie [toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lees de volgende artikelen een volgende stap:
* [Inzicht in het toepassingsmodel](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
