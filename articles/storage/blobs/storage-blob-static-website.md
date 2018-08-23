---
title: Statische website hosting in Azure Storage (Preview) | Microsoft Docs
description: Azure Storage biedt nu de statische website die als host fungeert (Preview), biedt een voordelige en schaalbare oplossing voor het hosten van moderne web-apps.
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: article
ms.date: 08/17/18
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 65a1cd85baf18ac1f0d193e7e6d6c3139919fb59
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617394"
---
# <a name="static-website-hosting-in-azure-storage-preview"></a>Statische website hosting in Azure Storage (Preview)
Azure Storage biedt nu de statische website die als host fungeert (Preview), waardoor u rendabele en schaalbare moderne webtoepassingen op Azure implementeren. Webpagina's bevatten een statische website statische inhoud en JavaScript of een andere client-side-code. Daarentegen, dynamische websites afhankelijk zijn van code op de server en kan worden gehost met behulp van [Azure Web Apps](/azure/app-service/app-service-web-overview).

Als u implementaties wilt voor flexibele, kosteneffectieve modellen, is de mogelijkheid om te leveren webinhoud zonder de noodzaak voor het serverbeheer van essentieel. De introductie van statische website hosting in Azure Storage maakt dit mogelijk, uitgebreide back-endmogelijkheden inschakelen met serverloze architecturen gebruik te maken van [Azure Functions](/azure/azure-functions/functions-overview) en andere PaaS-services.

## <a name="how-does-it-work"></a>Hoe werkt het?
Wanneer u statische websites op uw storage-account inschakelt, een nieuwe webservice-eindpunt wordt gemaakt van het formulier `<account-name>.<zone-name>.web.core.windows.net`.

De webservice-eindpunt altijd kunt anonieme toegang voor lezen, retourneert opgemaakte HTML-pagina's in reactie op fouten van de service en kan alleen object leesbewerkingen. De webservice-eindpunt retourneert het Indexdocument in de aangevraagde map voor de hoofdmap en alle submappen. Wanneer de storage-service een 404-fout wordt, retourneert de webeindpunt een aangepaste fout-document als u deze hebt geconfigureerd.

Inhoud voor uw statische website wordt gehost in een speciale container met de naam '$web'. Als onderdeel van het proces van activering "$web" voor u gemaakt als deze niet al bestaat. Inhoud in '$web' zijn toegankelijk in de hoofdmap van het account met behulp van het eindpunt op het web. Bijvoorbeeld `https://contoso.z4.web.core.windows.net/` retourneert de Indexdocument dat u voor uw website is geconfigureerd als een document met die naam in de hoofdmap van $web bestaat.

Wanneer u inhoud uploadt naar uw website, gebruikt u het eindpunt van blob storage. Het uploaden van een blob met de naam 'image.jpg' die kunnen worden geopend in de hoofdmap van het account gebruikt u de volgende URL `https://contoso.blob.core.windows.net/$web/image.jpg`. De geüploade afbeelding kan worden weergegeven in een webbrowser op het bijbehorende eindpunt op het web `https://contoso.z4.web.core.windows.net/image.jpg`.


## <a name="custom-domain-names"></a>Aangepaste domeinnamen
U kunt een aangepast domein gebruiken voor het hosten van inhoud van uw website. Om dit te doen, volg de instructies in [een aangepaste domeinnaam voor uw Azure Storage-account configureren](storage-custom-domain-name.md). Voor toegang tot uw website die wordt gehost op een aangepaste domeinnaam via HTTPS, Zie [de Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md). Uw CDN verwijzen naar het eindpunt op het web in plaats van het eindpunt van blob en houd er rekening mee dat de configuratie van het CDN onmiddellijk, gebeurt niet dus u over een paar minuten moet misschien voordat de inhoud van uw zichtbaar is.

## <a name="pricing-and-billing"></a>Prijzen en facturering
Statische website hosting wordt geleverd zonder extra kosten. Bekijk voor meer informatie over prijzen voor Azure Blob Storage, de [Azure Blob Storage pagina met prijzen van](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Snelstartgids
### <a name="azure-portal"></a>Azure Portal
Als u dat nog niet gedaan hebt, [maken van een GPv2-opslagaccount](../common/storage-quickstart-create-account.md) voor het starten van uw webtoepassing te hosten, kunt u de functie met behulp van de Azure-Portal configureren en klik op 'Statische website (preview)' onder 'Instellingen' in de linker navigatiebalk. Klik op 'Ingeschakeld' en voer de naam van het Indexdocument en (optioneel) de aangepaste fout bij het documentpad.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Upload uw web-items naar de container '$web' die is gemaakt als onderdeel van de statische website mogelijk maken. U kunt dit rechtstreeks doen in Azure Portal of u kunt profiteren van [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) gehele mapstructuren uploaden. Zorg ervoor dat u het opnemen van een Indexdocument met de naam die u hebt geconfigureerd. In dit voorbeeld is de naam van het document 'index.html'.

> [!NOTE]
> Naam van het document is hoofdlettergevoelig en moet daarom precies overeenkomen met de naam van het bestand in de opslag.

Tot slot gaat u naar uw webeindpunt voor het testen van uw website.

### <a name="azure-cli"></a>Azure-CLI
De uitbreiding voor de opslag van Preview-versie installeren:

```azurecli-interactive
az extension add --name storage-preview
```
De functie inschakelen:

```azurecli-interactive
az storage blob service-properties update --account-name <account-name> --static-website --404-document <error-doc-name> --index-document <index-doc-name>
```
Query voor de eindpunt-URL:

```azurecli-interactive
az storage account show -n <account-name> -g <resource-group> --query "primaryEndpoints.web" --output tsv
```

Objecten naar de container $web uploaden:

```azurecli-interactive
az storage blob upload-batch -s deploy -d $web --account-name <account-name>
```

## <a name="faq"></a>Veelgestelde vragen
**Statische websites voor alle storage-accounttypen beschikbaar is?**  
Nee, hosting van statische website is alleen beschikbaar in standard GPv2-opslagaccounts.

**Zijn opslag-VNET en firewall-regels op het nieuwe eindpunt op het web ondersteund?**  
Ja, is het nieuwe eindpunt op het web gehoorzaamt aan de VNET- en firewall-regels die zijn geconfigureerd voor het opslagaccount.

**Is het eindpunt op het web hoofdlettergevoelig?**  
Ja, is het eindpunt op het web hoofdlettergevoelig net als de blob-eindpunt. 

## <a name="next-steps"></a>Volgende stappen
* [De Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)
* [Een aangepaste domeinnaam voor uw blob- en web-eindpunten configureren](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure Web Apps](/azure/app-service/app-service-web-overview)
* [Uw eerste serverloze web-app bouwen](https://aka.ms/static-serverless-webapp)
