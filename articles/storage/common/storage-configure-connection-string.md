---
title: Een verbindingsreeks configureren voor Azure Storage | Microsoft Docs
description: Een verbindingsreeks voor een Azure storage-account configureren. Een verbindingsreeks bevat de informatie die nodig is om toegang te verlenen aan een storage-account van uw toepassing tijdens runtime.
services: storage
author: craigshoemaker
ms.service: storage
ms.topic: article
ms.date: 04/12/2017
ms.author: cshoe
ms.component: common
ms.openlocfilehash: 1c6ba727bf9c19b5da0d0d0313377d5d4ad1e2ac
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39531103"
---
# <a name="configure-azure-storage-connection-strings"></a>Azure Storage-verbindingsreeksen configureren

Een verbindingsreeks bevat de verificatiegegevens die nodig zijn voor uw toepassing toegang tot gegevens in een Azure Storage-account tijdens runtime. U kunt de verbindingsreeksen om te configureren:

* Verbinding maken met de Azure-opslagemulator.
* Toegang tot een opslagaccount in Azure.
* Toegang tot opgegeven resources in Azure via een shared access signature (SAS).

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Opslaan van de verbindingsreeks
De toepassing nodig heeft voor toegang tot de verbindingsreeks in runtime voor het toestaan van aanvragen voor Azure Storage. U hebt verschillende mogelijkheden voor het opslaan van de verbindingsreeks:

* Een toepassing die wordt uitgevoerd op het bureaublad of op een apparaat kunt opslaan de verbindingsreeks in een **app.config** of **web.config** bestand. Toevoegen van de verbindingsreeks voor de **AppSettings** sectie in deze bestanden.
* Een toepassing die wordt uitgevoerd in een Azure-cloudservice kunt opslaan de verbindingsreeks in de [schema (.cscfg) van Azure serviceconfiguratiebestand](https://msdn.microsoft.com/library/ee758710.aspx). Toevoegen van de verbindingsreeks voor de **ConfigurationSettings** gedeelte van het configuratiebestand van de service.
* Rechtstreeks in uw code kunt u de verbindingsreeks. We raden echter aan de verbindingsreeks op te slaan in een configuratiebestand in de meeste scenario's.

De verbindingsreeks opslaan in een configuratiebestand, kunt u eenvoudig om bij te werken van de verbindingsreeks in te schakelen tussen de opslagemulator en Azure storage-account in de cloud. U hoeft alleen te bewerken van de verbindingsreeks om te verwijzen naar uw doelomgeving.

U kunt de [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) voor toegang tot uw verbindingsreeks in runtime, ongeacht waar uw toepassing wordt uitgevoerd.

## <a name="create-a-connection-string-for-the-storage-emulator"></a>Maken van een verbindingsreeks voor de opslagemulator
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Zie voor meer informatie over de opslagemulator [de Azure-opslagemulator gebruiken voor ontwikkelen en testen](storage-use-emulator.md).

## <a name="create-a-connection-string-for-an-azure-storage-account"></a>Een verbindingsreeks voor een Azure storage-account maken
Gebruik de volgende indeling voor het maken van een verbindingsreeks voor uw Azure storage-account. Aangeven of u wilt verbinding maken met de storage-account via HTTPS (aanbevolen) of HTTP, Vervang `myAccountName` met de naam van uw opslagaccount en vervang `myAccountKey` door de toegangssleutel voor uw account:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Bijvoorbeeld, de verbindingsreeks kan als volgt uitzien:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

Hoewel Azure Storage biedt ondersteuning voor HTTP en HTTPS in een verbindingsreeks *HTTPS wordt ten zeerste aangeraden*.

> [!TIP]
> U vindt de verbindingsreeksen uw storage-account in de [Azure-portal](https://portal.azure.com). Navigeer naar **instellingen** > **toegangssleutels** in het menu-blade om te zien van verbindingsreeksen voor beide primaire en secundaire toegangssleutel van uw opslagaccount.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Een verbindingsreeks met behulp van een shared access signature maken
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Een verbindingsreeks voor een expliciete storage-eindpunt maken
U kunt expliciete service-eindpunten in de verbindingsreeks in plaats van de Standaardeindpunten. Voor het maken van een verbindingsreeks die Hiermee geeft u een expliciete eindpunt, geef het volledige service-eindpunt voor elke service, met inbegrip van de specificatie van het protocol (HTTPS (aanbevolen) of HTTP), in de volgende indeling:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Een scenario waarin u mogelijk wilt u een expliciete-eindpunt is wanneer u uw Blob storage-eindpunt naar hebt toegewezen een [aangepast domein](../blobs/storage-custom-domain-name.md). In dat geval kunt u uw aangepast eindpunt voor Blob-opslag in de verbindingsreeks opgeven. U kunt optioneel de Standaardeindpunten voor de andere services opgeven als de toepassing worden gebruikt.

Hier volgt een voorbeeld van een verbindingsreeks die Hiermee geeft u een expliciete eindpunt voor de Blob-service:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

In dit voorbeeld geeft expliciete eindpunten voor alle services, met inbegrip van een aangepast domein voor de Blob-service:

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

De eindpunt-waarden in een verbindingsreeks worden te maken van de aanvraag-URI's voor de storage-services en dicteren de vorm van een URI's die worden geretourneerd aan uw code gebruikt.

Als u een opslageindpunt hebt toegewezen aan een aangepast domein en dat eindpunt van een verbindingsreeks weglaat, klikt u vervolgens zich kunt u niet kunnen deze verbindingsreeks gebruiken voor toegang tot gegevens in die service vanuit uw code.

> [!IMPORTANT]
> Service-eindpunt waarden in uw verbindingsreeksen moeten opgemaakte URI's, met inbegrip van `https://` (aanbevolen) of `http://`. Omdat Azure Storage biedt nog geen ondersteuning HTTPS voor aangepaste domeinen, u *moet* opgeven `http://` voor een willekeurig eindpunt-URI die naar een aangepast domein verwijst.
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Een verbindingsreeks met het achtervoegsel van een eindpunt maken
Gebruik de volgende indeling van de verbindingsreeks voor het maken van een verbindingsreeks voor een opslagservice in regio's of instanties met een ander eindpunt achtervoegsels, zoals voor Azure China of Azure Government. Aangeven of u wilt verbinding maken met de storage-account via HTTPS (aanbevolen) of HTTP, Vervang `myAccountName` vervangen door de naam van uw opslagaccount, `myAccountKey` door uw toegangssleutel en vervang `mySuffix` met het URI-achtervoegsel:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Hier volgt een voorbeeld van de verbindingsreeks voor storage-services in Azure China:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Een verbindingsreeks te parseren
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Volgende stappen
* [De Azure-opslagemulator gebruiken voor ontwikkelen en testen](storage-use-emulator.md)
* [Azure opslagverkenners](storage-explorers.md)
* [Shared Access Signatures (SAS) gebruiken](storage-dotnet-shared-access-signature-part-1.md)

