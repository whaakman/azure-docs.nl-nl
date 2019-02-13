---
title: Instellen van een versleutelingscertificaat en geheimen in Azure Service Fabric Windows-clusters versleutelen | Microsoft Docs
description: Informatie over het instellen van een versleutelingscertificaat en geheimen op Windows-clusters te coderen.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2019
ms.author: vturecek
ms.openlocfilehash: 6aba94aa083127c2300481dbc8e30c224a1813a3
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109262"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-windows-clusters"></a>Instellen van een versleutelingscertificaat en geheimen op Windows-clusters te coderen
In dit artikel laat zien hoe een versleutelingscertificaat instellen en deze gebruiken voor het versleutelen van geheimen op Windows-clusters. Zie voor Linux-clusters, [instellen van een versleutelingscertificaat en geheimen op Linux-clusters te versleutelen.][secret-management-linux-specific-link]

[Azure Key Vault] [ key-vault-get-started] wordt hier gebruikt als een locatie voor de veilige opslag voor certificaten en als een manier om certificaten zijn geïnstalleerd op de Service Fabric-clusters in Azure. Als u niet naar Azure implementeert, hoeft u geen Key Vault gebruikt om geheimen in Service Fabric-toepassingen te beheren. Echter, *met behulp van* geheimen in een toepassing is cloud platform-agnostische om toe te staan van toepassingen op een cluster die overal gehost kunnen worden geïmplementeerd. 

## <a name="obtain-a-data-encipherment-certificate"></a>Een versleutelingscertificaat gegevens ophalen
Een versleutelingscertificaat gegevens wordt alleen gebruikt voor versleuteling en ontsleuteling van [parameters] [ parameters-link] in van een service Settings.xml en [omgevingsvariabelen] [ environment-variables-link] in ServiceManifest.xml van een service. Het is niet wordt gebruikt voor verificatie of het ondertekenen van gecodeerde tekst. Het certificaat moet voldoen aan de volgende vereisten:

* Het certificaat moet een persoonlijke sleutel bevatten.
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, exporteerbaar naar een Personal Information Exchange (PFX)-bestand.
* De certificaat-sleutelgebruik gegevenscodering (10) moet bevatten en mag geen Server-verificatie of clientverificatie. 
  
  Bijvoorbeeld, bij het maken van een zelfondertekend certificaat met behulp van PowerShell, de `KeyUsage` markering moet worden ingesteld op `DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Het certificaat installeren in uw cluster
Dit certificaat moet worden geïnstalleerd op elk knooppunt in het cluster. Zie [over het maken van een cluster met behulp van Azure Resource Manager] [ service-fabric-cluster-creation-via-arm] voor installatie-instructies. 

## <a name="encrypt-application-secrets"></a>Toepassingsgeheimen versleutelen
De volgende PowerShell-opdracht wordt gebruikt voor het versleutelen van een geheim. Met deze opdracht alleen de waarde; gecodeerd Dit gebeurt **niet** Meld u aan de gecodeerde tekst. U moet de dezelfde versleutelingscertificaat dat is geïnstalleerd in uw cluster voor het produceren van gecodeerde tekst voor de geheime waarden gebruiken:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

De resulterende tekenreeks met base 64-codering bevat zowel de geheime gecodeerde tekst, evenals informatie over het certificaat dat is gebruikt om ze te versleutelen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [versleutelde geheimen in een toepassing opgeven.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-overview.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
