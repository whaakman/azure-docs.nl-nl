---
title: Een verbindingsreeks configureren voor Azure Storage | Microsoft Docs
description: "Configureer een verbindingsreeks voor Azure storage-account. Een verbindingsreeks bevat de informatie die nodig zijn voor het verifiëren van toegang tot een opslagaccount van uw toepassing tijdens runtime."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: tamram
ms.openlocfilehash: 192799cb44dc9a56c65a6414c1267c506252fe29
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-azure-storage-connection-strings"></a>Azure Storage-verbindingsreeksen configureren

Een verbindingsreeks bevat de verificatiegegevens die nodig zijn voor uw toepassing voor toegang tot gegevens in een Azure Storage-account tijdens runtime. U kunt de verbindingsreeksen om te configureren:

* Verbinding maken met de Azure-opslagemulator.
* Toegang tot een opslagaccount in Azure.
* Toegang tot de opgegeven resources in Azure via een shared access signature (SAS).

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Opslaan van de verbindingsreeks
Uw toepassing moet toegang tot de verbindingsreeks in runtime te verifiëren van aanvragen voor Azure Storage. U hebt verschillende mogelijkheden voor het opslaan van de verbindingsreeks:

* De verbindingsreeks in kan worden opgeslagen in een toepassing wordt uitgevoerd op het bureaublad of op een apparaat een **app.config** of **web.config** bestand. Toevoegen van de verbindingsreeks naar de **AppSettings** sectie in deze bestanden.
* Een toepassing die wordt uitgevoerd in een Azure-cloud-service kan opslaan van de verbindingsreeks in de [Azure-service-configuratiebestand (.cscfg) schema](https://msdn.microsoft.com/library/ee758710.aspx). Toevoegen van de verbindingsreeks naar de **ConfigurationSettings** gedeelte van het configuratiebestand van de service.
* Rechtstreeks in uw code kunt u de verbindingsreeks. We raden echter aan de verbindingsreeks op te slaan in een configuratiebestand in de meeste scenario's.

De verbindingstekenreeks opslaan in een configuratiebestand, kunt u gemakkelijk bijwerken van de verbindingsreeks in te schakelen tussen de opslagemulator en Azure storage-account in de cloud. Alleen moet u de verbindingsreeks om te verwijzen naar de doelomgeving.

U kunt de [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) voor toegang tot de verbindingsreeks in runtime ongeacht waar uw toepassing wordt uitgevoerd.

## <a name="create-a-connection-string-for-the-storage-emulator"></a>Maken van een verbindingsreeks voor de opslagemulator
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Zie voor meer informatie over de opslagemulator [de Azure-opslagemulator gebruiken voor ontwikkeling en tests](storage-use-emulator.md).

## <a name="create-a-connection-string-for-an-azure-storage-account"></a>Een verbindingsreeks voor Azure storage-account maken
Gebruik de volgende notatie voor het maken van een verbindingsreeks voor uw Azure storage-account. Aangeven of u wilt verbinding maken met het opslagaccount via HTTPS (aanbevolen) of HTTP-, vervangen door `myAccountName` met de naam van uw opslagaccount en vervang `myAccountKey` door uw toegangssleutel voor account:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

De verbindingsreeks er ongeveer als:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

Hoewel Azure Storage zowel HTTP als HTTPS in een verbindingsreeks ondersteunt *HTTPS wordt nadrukkelijk aanbevolen*.

> [!TIP]
> U vindt de verbindingsreeksen uw storage-account in de [Azure-portal](https://portal.azure.com). Navigeer naar **instellingen** > **toegangssleutels** in uw opslagaccount menu blade verbindingsreeksen voor beide primaire en secundaire sneltoetsen te zien.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Een verbindingsreeks met behulp van een shared access signature maken
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Een verbindingsreeks voor een eindpunt expliciete opslag maken
U kunt expliciete service-eindpunten in de verbindingsreeks in plaats van de Standaardeindpunten opgeven. Geef het volledige service-eindpunt voor elke service, inclusief de specificatie van het protocol (HTTPS (aanbevolen) of HTTP) in de volgende notatie voor het maken van een verbindingsreeks die Hiermee geeft u een expliciete eindpunt:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Een scenario waarin u wilt mogelijk een expliciete eindpunt opgeven is wanneer u het eindpunt van de Blob-opslag naar hebt toegewezen een [aangepast domein](../blobs/storage-custom-domain-name.md). In dat geval kunt u uw aangepaste eindpunt voor Blob storage in de verbindingsreeks. Als uw toepassing worden gebruikt, kunt u eventueel de Standaardeindpunten voor de andere services opgeven.

Hier volgt een voorbeeld van een verbindingsreeks waarmee een expliciete eindpunt voor de Blob-service:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

In dit voorbeeld geeft expliciet eindpunten voor alle services, met inbegrip van een aangepast domein voor de Blob-service:

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

De eindpunt-waarden in een verbindingsreeks worden gebruikt voor het maken van de aanvraag-URI's voor de storage-services en dicteren de vorm van een URI's die worden geretourneerd aan uw code.

Als u een eindpunt opslag hebt toegewezen aan een aangepast domein en dat eindpunt van een verbindingsreeks weglaat, klikt zich u niet kunnen deze verbindingsreeks gebruiken voor toegang tot gegevens in die service vanuit uw code.

> [!IMPORTANT]
> Service-eindpunt waarden in de verbindingsreeksen moeten juist opgemaakte URI's, met inbegrip van `https://` (aanbevolen) of `http://`. Omdat Azure Storage nog geen HTTPS voor aangepaste domeinen ondersteunt u *moet* Geef `http://` voor een willekeurig eindpunt-URI die naar een aangepast domein verwijst.
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Een verbindingsreeks met het achtervoegsel van een eindpunt maken
Gebruik de indeling van de volgende verbindingsreeks voor het maken van een verbindingsreeks voor een service-opslag in regio's of exemplaren met verschillende eindpunt achtervoegsels, zoals voor Azure China of Azure Government. Aangeven of u wilt verbinding maken met het opslagaccount via HTTPS (aanbevolen) of HTTP-, vervangen door `myAccountName` vervangen door de naam van uw opslagaccount, `myAccountKey` met uw toegangssleutel en vervangen `mySuffix` met het achtervoegsel URI:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Hier volgt een voorbeeld van de verbindingsreeks voor storage-services in Azure voor China:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Een verbindingsreeks te parseren
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Volgende stappen
* [De Azure-opslagemulator gebruiken voor ontwikkeling en testen](storage-use-emulator.md)
* [Azure Storage explorers](storage-explorers.md)
* [Met behulp van Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md)

