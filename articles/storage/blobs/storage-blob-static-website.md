---
title: Statische website hosting in Azure Storage
description: Azure Storage statische website hosting, biedt een voordelige en schaalbare oplossing voor het hosten van moderne web-apps.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.author: normesta
ms.reviewer: seguler
ms.date: 05/29/2019
ms.subservice: blobs
ms.openlocfilehash: 36cc8cebdb567cb9650ad1ad3baf72a0b5478247
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427986"
---
# <a name="static-website-hosting-in-azure-storage"></a>Statische website hosting in Azure Storage

U kunt leveren van statische inhoud (HTML, CSS, JavaScript en afbeeldingsbestanden) rechtstreeks vanuit een storage-container met de naam *$web*. Uw inhoud in Azure Storage te hosten, kunt u gebruikmaken van serverloze architecturen die zijn [Azure Functions](/azure/azure-functions/functions-overview) en ander Platform als een service (PaaS)-services.

> [!NOTE]
> Als uw site is afhankelijk van code op de server, gebruikt u [Azure App Service](/azure/app-service/overview) in plaats daarvan.

## <a name="setting-up-a-static-website"></a>Instellen van een statische website

Statische website hosting, is een functie die u hebt om in te schakelen op het storage-account.

Om in te schakelen statische website hosting, selecteert u de naam van de standaard-bestand en geef vervolgens een pad naar een aangepaste 404-pagina (optioneel). Als een blob storage-container met de naam **$web** nog niet bestaat in het account, een voor u is gemaakt. De bestanden van uw site toevoegen aan deze container.

Zie voor stapsgewijze instructies [hosten van een statische website in Azure Storage](storage-blob-static-website-how-to.md).

![Azure Storage statische websites metrische gegevens over metrische gegevens](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

Bestanden in de **$web** container zijn hoofdlettergevoelig, die via anonieme aanvragen en zijn alleen beschikbaar via leesbewerkingen.

## <a name="uploading-content"></a>Inhoud te uploaden

U kunt een van deze hulpprogramma's gebruiken voor het uploaden van inhoud naar de **$web** container:

> [!div class="checklist"]
> * [Azure-CLI](storage-blob-static-website-how-to.md#cli)
> * [Azure PowerShell-module](storage-blob-static-website-how-to.md#powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Azure-opslagverkenner](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
> * [Visual Studio Code-extensie](https://code.visualstudio.com/tutorials/static-website/getting-started)

## <a name="viewing-content"></a>Inhoud weergeven

Gebruikers kunnen site-inhoud via een browser met behulp van de openbare URL van de website bekijken. U vindt de URL met behulp van de Azure portal, Azure CLI of PowerShell. Gebruik deze tabel als richtlijn.

|Hulpprogramma| Richtlijnen |
|----|----|
|**Azure Portal** | [De website-URL zoeken met behulp van de Azure-portal](storage-blob-static-website-how-to.md#portal-find-url) |
|**Azure-CLI** | [De website-URL zoeken met behulp van de Azure CLI](storage-blob-static-website-how-to.md#cli-find-url) |
|**Azure PowerShell-module** | [De website-URL zoeken met behulp van PowerShell](storage-blob-static-website-how-to.md#powershell-find-url) |

De URL van uw site bevat een regionale code. Bijvoorbeeld de URL `https://contosoblobaccount.z22.web.core.windows.net/` regionale code bevat `z22`.

Hoewel die code de URL blijven moet, dit geldt alleen voor intern gebruik en u hoeft niet te gebruiken die code op een andere manier.

De Indexdocument dat u opgeeft wanneer u de statische website hosting, inschakelt wordt weergegeven wanneer gebruikers de site openen en een specifiek bestand geen opgeeft (bijvoorbeeld: `https://contosoblobaccount.z22.web.core.windows.net`).  

Als de server een 404-fout retourneert en u hebt opgegeven een document fout wanneer u de website hebt ingeschakeld, wordt een 404 standaardpagina geretourneerd naar de gebruiker.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Gevolgen van de instelling van het niveau openbare toegang van de webcontainer

U kunt het niveau openbare toegang van de **$web** container, maar dit heeft geen invloed op de primaire statische website-eindpunt omdat deze bestanden worden aangeboden via anonieme aanvragen. Dit betekent dat openbare (alleen-lezen) toegang tot alle bestanden.

De volgende schermafbeelding ziet u de instelling voor openbare toegang in Azure portal:

![Schermopname die laat zien hoe niveau openbare toegang instelt in de portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Terwijl het primaire statische website-eindpunt wordt niet beïnvloed, is het primaire blob service-eindpunt invloed op een wijziging in de niveau openbare toegang.

Bijvoorbeeld, als u het niveau openbare toegang van de **$web** -container vanuit **privé (geen anonieme toegang)** naar **Blob (anonieme leestoegang voor alleen blobs)** , dan zal de niveau van openbare toegang tot het eindpunt van de primaire statische website `https://contosoblobaccount.z22.web.core.windows.net/index.html` niet wijzigen.

Echter, de openbare toegang tot de primaire blob-service-eindpunt `https://contosoblobaccount.blob.core.windows.net/$web/index.html` van persoonlijke wordt gewijzigd in openbare. Gebruikers kunnen nu dat bestand openen met behulp van een van deze twee eindpunten.

## <a name="content-delivery-network-cdn-and-secure-socket-layer-ssl-support"></a>Content Delivery Network (CDN) en Secure Socket Layer (SSL)-ondersteuning

Als u uw bestanden statische website beschikbaar maken via uw aangepaste domein- en HTTPS, Zie [de Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md). Als onderdeel van dit proces, moet u uw CDN verwijzen naar de primaire *statische website* eindpunt in plaats van de primaire *blob-service* eindpunt. Mogelijk moet u over een paar minuten voordat de inhoud weergegeven wordt als de CDN-configuratie niet onmiddellijk wordt uitgevoerd.

Wanneer u uw statische website bijwerken, moet u de inhoud op het CDN edge-servers in cache wissen door het opschonen van het CDN-eindpunt. Zie voor meer informatie [Een Azure CDN-eindpunt leegmaken](../../cdn/cdn-purge-endpoint.md).

> [!NOTE]
> HTTPS wordt systeemeigen ondersteund door het eindpunt van de web-account, zodat de webeindpunt toegankelijk via HTTP en HTTPS is. Echter, als het opslagaccount dat is geconfigureerd voor veilige overdracht vereisen via HTTPS, klikt u vervolgens gebruikers mogen gebruiken het HTTPS-eindpunt. Zie voor meer informatie, [veilige overdracht in Azure Storage vereisen](../common/storage-require-secure-transfer.md).
>
> Het gebruik van aangepaste domeinen via HTTPS is vereist voor het gebruik van Azure CDN op dit moment.

## <a name="custom-domain-names"></a>Aangepaste domeinnamen

U kunt uw statische website beschikbaar maken via een aangepast domein. Zie voor meer informatie, [een aangepaste domeinnaam voor uw Azure Storage-account configureren](storage-custom-domain-name.md).

Zie voor een diepgaande blik op die als host fungeert voor uw domein in Azure, [uw domein in Azure DNS hosten](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Prijzen

U kunt statische website hosting gratis. U wordt gefactureerd alleen voor de blobopslag die gebruikmaakt van uw site en de operationele kosten. Bekijk voor meer informatie over prijzen voor Azure Blob Storage, de [Azure Blob Storage pagina met prijzen van](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="metrics"></a>Metrische gegevens

U kunt metrische gegevens op pagina's met statische website inschakelen. Nadat u de metrische gegevens hebt ingeschakeld, verkeer statistieken voor bestanden in de **$web** container worden gerapporteerd in het dashboard.

Zie voor het inschakelen van metrische gegevens over uw statische website's [metrische gegevens op pagina's met statische website inschakelen](storage-blob-static-website-how-to.md#metrics).

## <a name="next-steps"></a>Volgende stappen

* [Host een statische website in Azure Storage](storage-blob-static-website-how-to.md)
* [Het Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)
* [Een aangepaste domeinnaam voor uw blob- en web-eindpunten configureren](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Uw eerste serverloze web-app bouwen](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Zelfstudie: Host uw domein in Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
