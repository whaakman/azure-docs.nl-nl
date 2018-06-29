---
title: Inschakelen van beveiligde TLS voor Azure Storage client | Microsoft Docs
description: Informatie over het inschakelen van TLS 1.2 op de client van Azure Storage.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/25/2018
ms.author: fryu
ms.openlocfilehash: 5c21df2b3bdeee6ac7c3956fe1cafa4f947dd6dd
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035927"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Beveiligde TLS voor Azure Storage client inschakelen

Als u wilt controleren uw services in het gebruik van Azure Storage op basis van de meest recente naleving en de beveiligingsvereisten, SSL 1.0, 2.0, 3.0 en TLS 1.0 zijn bekend als niet-compatibele communicatieprotocollen.

SSL 1.0, 2.0 en 3.0 zijn kwetsbaar gevonden. Ze hebben door RFC is verboden. TLS 1.0 wordt onbeveiligde voor het gebruik van onbeveiligde blokcodering (DES CBC en RC2 CBC) en de Stream cipher (RC4). PCI Raad wordt ook aangeraden de migratie naar de hogere versies van TLS. Voor meer informatie ziet u [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

Azure Storage SSL 3.0 is gestopt omdat 2015 en maakt gebruik van TLS 1.2 op openbare HTTPs-eindpunten maar TLS 1.0- en TLS 1.1 worden nog steeds ondersteund voor compatibiliteit met eerdere versies.

Om ervoor te zorgen beveiligd en compatibel verbinding met Azure Storage, moet u TLS 1.2 in client-side inschakelen voordat verzendt aanvragen naar Azure Storage-service werkt.

## <a name="enable-tls-12-in-net-client"></a>TLS 1.2 in .NET-client inschakelen

Voor de client om te onderhandelen over TLS 1.2, het besturingssysteem en de .NET Framework-versie moeten beide ondersteunen TLS 1.2. Zie voor meer informatie [ondersteuning voor TLS 1.2](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).

Het volgende voorbeeld laat zien hoe TLS 1.2 inschakelen in uw .NET-client.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## <a name="enable-tls-12-in-powershell-client"></a>Inschakelen van TLS 1.2 in PowerShell-client

Het volgende voorbeeld laat zien hoe TLS 1.2 inschakelen in uw PowerShell-client.

```powershell

# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGropuName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzureStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers

```

## <a name="verify-tls-12-connection"></a>TLS 1.2-verbinding controleren

U kunt Fiddler gebruiken om te controleren als TLS 1.2 daadwerkelijk wordt gebruikt. Open Fiddler beginnen met client-netwerkverkeer opnemen en klik hierboven voorbeeld uitvoeren. Vervolgens kunt u het TLS-versie vinden in de verbinding waarmee het voorbeeld.

De volgende schermafbeelding is een voorbeeld voor de verificatie.

![schermopname van het TLS-versie in Fiddler controleren](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Zie ook

* [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
