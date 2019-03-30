---
title: Een Azure Service Fabric-service uitvoeren als een AD-gebruiker of groep | Microsoft Docs
description: Leer hoe u een service uitvoeren als een Active Directory-gebruiker of groep op een zelfstandige Service Fabric Windows cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: dekapur
ms.openlocfilehash: 3e0bb62609f13430bd2beab2332a31983874eb8e
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664740"
---
# <a name="run-a-service-as-an-active-directory-user-or-group"></a>Een service uitvoeren als een Active Directory-gebruiker of groep
U kunt op een zelfstandige cluster van Windows Server, een service uitvoeren als een Active Directory-gebruiker of groep met behulp van een RunAs-beleid.  Standaard wordt Service Fabric-toepassingen uitvoeren onder het account dat de Fabric.exe-proces wordt uitgevoerd onder. Uitvoeren van toepassingen onder verschillende accounts, zelfs in een gedeelde omgeving, zorgt ervoor dat ze beter te beveiligen van elkaar. Houd er rekening mee dat dit maakt gebruik van on-premises Active Directory in uw domein en niet Azure Active Directory (Azure AD).  U kunt ook uitvoeren met een service als een [beheerd serviceaccount (gMSA)](service-fabric-run-service-as-gmsa.md).

Met behulp van een domeingebruiker of groep, kunt u vervolgens toegang tot andere resources in het domein (bijvoorbeeld-bestandsshares) die machtigingen hebben gekregen.

Het volgende voorbeeld ziet u een Active Directory-gebruiker met de naam *TestUser* met hun domein wachtwoord versleuteld met behulp van een certificaat met de naam *MyCert*. U kunt de `Invoke-ServiceFabricEncryptText` PowerShell-opdracht voor het maken van de geheime gecodeerde tekst. Zie [geheimen in Service Fabric-toepassingen beheren](service-fabric-application-secret-management.md) voor meer informatie.

U moet de persoonlijke sleutel van het certificaat voor het ontsleutelen van het wachtwoord voor de lokale computer met behulp van een out-of-band-methode implementeren (dit is in Azure, via Azure Resource Manager). Vervolgens, wanneer Service Fabric het servicepakket naar de machine is geïmplementeerd, is het kunnen het geheim te ontsleutelen en te verifiëren met Active Directory om uit te voeren onder deze referenties (samen met de naam van de gebruiker).

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```

> [!NOTE] 
> Als u een Run as-beleid op een service toepassen en het servicemanifest eindpunt resources met de HTTP-protocol verklaart, moet u ook opgeven een **SecurityAccessPolicy**.  Zie voor meer informatie, [toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lees de volgende stap, de volgende artikelen:
* [Inzicht krijgen in het toepassingsmodel](service-fabric-application-model.md)
* [Resources specificeren in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
