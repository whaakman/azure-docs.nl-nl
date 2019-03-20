---
title: Uitvoeren van een Azure Service Fabric-service onder een gMSA-account | Microsoft Docs
description: Leer hoe u een service uitvoeren als een gMSA op een zelfstandige Service Fabric Windows cluster.
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
ms.openlocfilehash: 8d14d9191e74cb59c6696568ead425fca61d6f7c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57873900"
---
# <a name="run-a-service-as-a-group-managed-service-account"></a>Een service uitvoeren als door een groep beheerd serviceaccount
Op een zelfstandige cluster van Windows Server, kunt u een service uitvoeren als een groep beheerd-serviceaccount (gMSA) met behulp van een RunAs-beleid.  Standaard wordt Service Fabric-toepassingen uitvoeren onder het account dat de Fabric.exe-proces wordt uitgevoerd onder. Uitvoeren van toepassingen onder verschillende accounts, zelfs in een gedeelde omgeving, zorgt ervoor dat ze beter te beveiligen van elkaar. Houd er rekening mee dat dit maakt gebruik van on-premises Active Directory in uw domein en niet Azure Active Directory (Azure AD). Met behulp van een gMSA, is er geen wachtwoord of het versleutelde wachtwoord opgeslagen in het toepassingsmanifest.  U kunt ook uitvoeren met een service als een [Active Directory-gebruiker of groep](service-fabric-run-service-as-ad-user-or-group.md).

In het volgende voorbeeld laat zien hoe u een gMSA-account met de naam *svc Test$*; over het implementeren van het beheerde serviceaccount voor de clusterknooppunten; en het configureren van de principal van gebruiker.

Vereisten:
- Het domein moet een KDS-hoofdsleutel.
- Het domein moet op een Windows Server 2012 of hoger functionaliteitsniveau.

1. Vraagt u een Active Directory-domein-beheerder maakt een groep beheerde service account met behulp van de `New-ADServiceAccount` commandlet en zorg ervoor dat de `PrincipalsAllowedToRetrieveManagedPassword` bevat alle van de service fabric-clusterknooppunten. `AccountName`, `DnsHostName`, en `ServicePrincipalName` moet uniek zijn.

    ```powershell
    New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
    ```

2. Op elk van de Service Fabric-clusterknooppunten (bijvoorbeeld `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), installeren en testen van de gMSA.
    
    ```powershell
    Add-WindowsFeature RSAT-AD-PowerShell
    Install-AdServiceAccount svc-Test$
    Test-AdServiceAccount svc-Test$
    ```

3. Configureren van de principal van gebruiker en configureer de RunAsPolicy om te verwijzen naar de gebruiker.
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
> Als u een Run as-beleid op een service toepassen en het servicemanifest eindpunt resources met de HTTP-protocol verklaart, moet u een **SecurityAccessPolicy**.  Zie voor meer informatie, [toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lees de volgende stap, de volgende artikelen:
* [Inzicht krijgen in het toepassingsmodel](service-fabric-application-model.md)
* [Resources specificeren in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
