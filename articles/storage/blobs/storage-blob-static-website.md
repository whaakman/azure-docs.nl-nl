---
title: Statische website hosting in Azure Storage
description: Azure Storage statische website hosting, biedt een voordelige en schaalbare oplossing voor het hosten van moderne web-apps.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/25/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 1794aa26fc725207c4a901c11c345eeaa3d2f65d
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56867731"
---
# <a name="static-website-hosting-in-azure-storage"></a>Statische website hosting in Azure Storage
Azure Storage GPv2-accounts kunt u statische inhoud (HTML, CSS, JavaScript en afbeeldingsbestanden) rechtstreeks vanuit een storage-container met de naam *$web*. U profiteert van hosten in Azure Storage kunt u gebruikmaken van serverloze architecturen, met inbegrip van [Azure Functions](/azure/azure-functions/functions-overview) en andere PaaS-services.

In tegenstelling tot de statische website hosting, dynamische websites die afhankelijk van de servercode zijn beste worden gehost met behulp van [Azure App Service](/azure/app-service/overview).

## <a name="how-does-it-work"></a>Hoe werkt het?
Wanneer u de statische website inschakelt die als host fungeert voor uw opslagaccount u selecteert u de naam van de standaard-bestand en geef optioneel een pad naar een aangepaste 404-pagina. Als de functie is ingeschakeld, een container met de naam *$web* wordt gemaakt als deze nog niet bestaat.

Bestanden in de *$web* container zijn:

- geleverd via anonieme toegangsaanvragen
- alleen beschikbaar via de object-bewerkingen lezen
- hoofdlettergevoelig
- beschikbaar op het openbare Internet dit patroon te volgen:
    - `https://<ACCOUNT_NAME>.<ZONE_NAME>.web.core.windows.net/<FILE_NAME>`
- beschikbaar via een eindpunt van Blob storage dit patroon te volgen:
    - `https://<ACCOUNT_NAME>.blob.core.windows.net/$web/<FILE_NAME>`

U het eindpunt van Blob storage gebruiken om bestanden te uploaden. Bijvoorbeeld: het bestand geüpload naar deze locatie:

```bash
https://contoso.blob.core.windows.net/$web/image.png
```

is beschikbaar in de browser op een locatie die u als volgt:

```bash
https://contoso.z4.web.core.windows.net/image.png
```

De geselecteerde standaard-bestandsnaam wordt gebruikt in de hoofdmap en alle submappen, wanneer een bestandsnaam is niet opgegeven. Als de server een 404-fout retourneert en u geen een fout bij het documentpad opgeeft, wordt een 404 standaardpagina geretourneerd naar de gebruiker.

## <a name="cdn-and-ssl-support"></a>CDN- en SSL-ondersteuning

Om uw statische website bestanden die beschikbaar zijn via HTTPS, Zie [de Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md). Als een onderdeel van dit proces, moet u *uw CDN verwijzen naar het eindpunt op het web* in plaats van de blob-eindpunt. Mogelijk moet u over een paar minuten voordat de inhoud weergegeven wordt als de CDN-configuratie niet onmiddellijk wordt uitgevoerd.

Wanneer u uw statische website bijwerken, moet u de inhoud op het CDN edge-servers in cache wissen door het opschonen van het CDN-eindpunt. Zie voor meer informatie [Een Azure CDN-eindpunt leegmaken](../../cdn/cdn-purge-endpoint.md).

## <a name="custom-domain-names"></a>Aangepaste domeinnamen

U kunt [een aangepaste domeinnaam voor uw Azure Storage-account configureren](storage-custom-domain-name.md) om uw statische website beschikbaar maken via een aangepast domein. Voor een diepgaande blik op het hosten van uw domein op [Azure, Zie uw domein in Azure DNS hosten](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Prijzen
Statische website hosting wordt geleverd zonder extra kosten. Bekijk voor meer informatie over prijzen voor Azure Blob Storage, de [Azure Blob Storage pagina met prijzen van](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Snelstartgids

### <a name="azure-portal"></a>Azure Portal
Beginnen met het openen van de Azure portal op https://portal.azure.com en doorloopt u de volgende stappen uit op uw GPv2-opslagaccount:

1. Klik op **instellingen**
2. Klik op **statische website**
3. Voer een *naam van het Indexdocument*. (Er is een algemene waarde *index.html)*
4. (Optioneel) Voer een *fout bij het documentpad* naar een aangepaste 404-pagina. (Er is een algemene waarde *404.html)*

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Upload vervolgens uw items naar de *$web* container via de Azure portal of met de [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) hele mappen uploaden. Zorg ervoor dat u een bestand dat overeenkomt met de *naam van het Indexdocument* u hebt geselecteerd bij het inschakelen van de functie.

Tot slot gaat u naar uw webeindpunt voor het testen van uw website.

### <a name="azure-cli"></a>Azure-CLI
De uitbreiding voor de opslag van Preview-versie installeren:

```azurecli-interactive
az extension add --name storage-preview
```
Stel uw CLI aan het abonnement van het GPv2-opslagaccount dat u wilt inschakelen in het geval van meerdere abonnementen:

```azurecli-interactive
az account set --subscription <SUBSCRIPTION_ID>
```
De functie inschakelen. Zorg ervoor dat alle tijdelijke aanduiding voor waarden, inclusief punthaken door uw eigen waarden vervangt:

```azurecli-interactive
az storage blob service-properties update --account-name <ACCOUNT_NAME> --static-website --404-document <ERROR_DOCUMENT_NAME> --index-document <INDEX_DOCUMENT_NAME>
```
Query voor de eindpunt-URL:

```azurecli-interactive
az storage account show -n <ACCOUNT_NAME> -g <RESOURCE_GROUP> --query "primaryEndpoints.web" --output tsv
```

Uploaden van objecten die u wilt de *$web* container uit een bronmap. Zorg ervoor dat u goed escape voor de verwijzing naar de *$web* container in de opdracht. Bijvoorbeeld, als u van Azure CLI uit CloudShell in Azure portal gebruikmaakt, als de *$web* container zoals wordt weergegeven:

```azurecli-interactive
az storage blob upload-batch -s <SOURCE_PATH> -d \$web --account-name <ACCOUNT_NAME>
```

## <a name="deployment"></a>Implementatie

Methoden voor het implementeren van inhoud naar een storage-container omvatten het volgende:

- [AzCopy](../common/storage-use-azcopy.md)
- [Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)
- [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
- [Visual Studio Code-extensie](https://code.visualstudio.com/tutorials/static-website/getting-started)

In alle gevallen, zorg ervoor dat u bestanden kopiëren naar de *$web* container.

## <a name="metrics"></a>Metrische gegevens

Om in te schakelen metrische gegevens over uw statische website's, klikt u op **instellingen** > **bewaking** > **metrische gegevens**.

Metrische gegevens worden gegenereerd door het Inhaken op verschillende metrische gegevens over API's. API-leden die wordt gebruikt binnen een bepaalde periode om te kunnen alleen richten op de leden die als resultaat de gegevens alleen worden weergegeven in de portal. Om ervoor te zorgen, selecteer een lid van de benodigde API kunt u, is de eerste stap om uit te breiden het tijdsbestek.

Klik op de knop tijdsbestek en selecteer **afgelopen 24 uur** en klik vervolgens op **toepassen**

![Metrische gegevens van Azure Storage statische websites tijdsbereik](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

Selecteer vervolgens **Blob** uit de *Namespace* vervolgkeuzelijst.

![Azure Storage statische websites metrische gegevens naamruimte](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

Selecteer vervolgens de **uitgaande** metrische gegevens.

![Azure Storage statische websites metrische gegevens over metrische gegevens](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

Selecteer **som** uit de *aggregatie* selector.

![Azure aggregatie van opslag statische websites metrische gegevens](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

Klik vervolgens op de **filter toevoegen** knop en kies **API-naam** uit de *eigenschap* selector.

![Statische websites metrische API voor de naam van Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

Ten slotte, schakel het selectievakje in naast **GetWebContent** in de *waarden* selector voor het vullen van het rapport metrische gegevens.

![Azure Storage statische websites metrics GetWebContent](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

Statistieken voor bestanden in het verkeer naar eenmaal is ingeschakeld, de *$web* container worden gerapporteerd in het dashboard.

## <a name="faq"></a>Veelgestelde vragen

**Is de functie voor statische websites voor alle storage-accounttypen beschikbaar?**  
Nee, hosting van statische website is alleen beschikbaar in standard GPv2-opslagaccounts.

**Zijn opslag-VNET en firewall-regels op het nieuwe eindpunt op het web ondersteund?**  
Ja, is het nieuwe eindpunt op het web gehoorzaamt aan de VNET- en firewall-regels die zijn geconfigureerd voor het opslagaccount.

**Is het eindpunt op het web hoofdlettergevoelig?**  
Ja, is het eindpunt op het web hoofdlettergevoelig net als de blob-eindpunt. 

## <a name="next-steps"></a>Volgende stappen
* [Azure-CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)
* [Een aangepaste domeinnaam voor uw blob- en web-eindpunten configureren](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Uw eerste serverloze web-app bouwen](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Zelfstudie: Host uw domein in Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
