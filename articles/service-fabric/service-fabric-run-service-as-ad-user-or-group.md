---
title: Een Azure Service Fabric-service worden uitgevoerd als een AD-gebruiker of groep | Microsoft Docs
description: Informatie over het uitvoeren van een service als een Active Directory-gebruiker of groep op een zelfstandige Service Fabric Windows cluster.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 1cf23a8f564553e65ac2c0fd34d44d81fe2327ea
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="run-a-service-as-an-active-directory-user-or-group"></a>Een service worden uitgevoerd als een Active Directory-gebruiker of groep
U kunt toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen met behulp van Azure Service Fabric. Hierdoor waarop toepassingen worden uitgevoerd, zelfs in een gedeelde gehoste omgeving veiliger van elkaar. Service Fabric-toepassingen worden standaard uitgevoerd onder het account waaronder het Fabric.exe-proces wordt uitgevoerd onder. Voor een zelfstandige cluster van Windows Server, kunt u een service als uitvoeren een [groep beheerde serviceaccounts (gMSA)](service-fabric-run-service-as-gmsa.md) of een Active Directory-gebruiker of een groep met behulp van een RunAs-beleid. Houd er rekening mee dat dit maakt gebruik van Active Directory on-premises binnen uw domein en niet Azure Active Directory (Azure AD).

U kunt andere bronnen in het domein (bijvoorbeeld bestandsshares) die machtigingen hebben gekregen benaderen via een domeingebruiker of -groep.

Het volgende voorbeeld ziet u een Active Directory-gebruiker aangeroepen *testgebruiker* met hun domein wachtwoord dat wordt versleuteld met behulp van een certificaat genoemd *MyCert*. U kunt de `Invoke-ServiceFabricEncryptText` PowerShell-opdracht voor het maken van de geheime gecodeerde tekst. Zie [geheimen in Service Fabric-toepassingen beheren](service-fabric-application-secret-management.md) voor meer informatie.

U moet de persoonlijke sleutel van het certificaat voor ontsleuteling van het wachtwoord voor de lokale computer met behulp van een out-of-band-methode implementeren (in Azure, is dit via Azure Resource Manager). Wanneer het Service Fabric pakket met de service op de machine implementeert, wordt het ontsleutelen van het geheim en verifiëren met Active Directory worden uitgevoerd met deze referenties (samen met de naam van de gebruiker).

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
> Als u een Run as-beleid op een service toepassen en de servicemanifest eindpunt resources met de HTTP-protocol declareert, moet u ook opgeven een **SecurityAccessPolicy**.  Zie voor meer informatie [toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lees de volgende artikelen een volgende stap:
* [Inzicht in het toepassingsmodel](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een toepassing implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
