---
title: Statische website die als host fungeert in Azure Storage (Preview) | Microsoft Docs
description: Azure Storage biedt nu een statische website die als host fungeert voor (Preview), een rendabele, schaalbare oplossing wordt geboden voor het hosten van moderne webtoepassingen.
services: storage
author: MichaelHauss
manager: vamshik
ms.service: storage
ms.topic: article
ms.date: 06/26/18
ms.author: mihauss
ms.openlocfilehash: 7021a0499547818d702d14aecb9d8e451a820181
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025736"
---
# <a name="static-website-hosting-in-azure-storage-preview"></a>Statische website die als host fungeert in Azure Storage (Preview)
Azure Storage biedt nu een statische website die als host fungeert voor (Preview), waarmee u rendabele en schaalbare moderne webtoepassingen in Azure te implementeren. Op een statische website bevatten webpagina's statische inhoud en JavaScript of andere code aan de clientzijde. Daarentegen dynamische websites afhankelijk zijn van servercode en kan worden gehost met [Azure Web Apps](/app-service/app-service-web-overview.md).

Zoals implementaties werktijd naar elastische, rendabele modellen, is de mogelijkheid om te leveren webinhoud zonder de noodzaak voor Serverbeheer is essentieel. De introductie van statische website die als host fungeert in Azure Storage maakt dit mogelijk, het inschakelen van back-end voor uitgebreide mogelijkheden met zonder server architecturen gebruik [Azure Functions](/azure-functions/functions-overview.md) en andere PaaS-services.

## <a name="how-does-it-work"></a>Hoe werkt het?
Wanneer u statische websites voor uw opslagaccount inschakelt, een nieuwe webservice-eindpunt wordt gemaakt van het formulier `<account-name>.<zone-name>.web.core.windows.net`.

De webservice-eindpunt altijd kan anonieme toegang voor lezen, retourneert opgemaakte HTML-pagina's in reactie op fouten van de service, en kan alleen objecten van het leesbewerkingen. De webservice-eindpunt wordt het Indexdocument in de aangevraagde map voor de hoofdmap en alle submappen. Wanneer de storage-service een 404-fout retourneert, retourneert de web-eindpunt een aangepaste fout-document als u deze heeft geconfigureerd.

Inhoud voor uw statische website wordt gehost in een speciale container met de naam '$web'. Als onderdeel van het proces voor activering '$web' voor u gemaakt als deze niet al bestaat. Inhoud in '$web' toegankelijk zijn in de hoofdmap van het account met behulp van de web-eindpunt. Bijvoorbeeld `https://contoso.z4.web.core.windows.net/` retourneert de Indexdocument dat u voor uw website is geconfigureerd als een document met die naam in de hoofdmap van $web bestaat.

Wanneer u inhoud uploadt naar uw website, gebruikt u het blobeindpunt-opslag. Voor het uploaden van een blob met de naam 'image.jpg', die toegankelijk zijn in de hoofdmap van het account gebruikt u de volgende URL `https://contoso.blob.core.windows.net/$web/image.jpg`. De installatiekopie van het geüploade kan worden weergegeven in een webbrowser op het webeindpunt van de bijbehorende `https://contoso.z4.web.core.windows.net/image.jpg`.


## <a name="custom-domain-names"></a>Aangepaste domeinnamen
U kunt een aangepast domein gebruiken voor het hosten van inhoud van uw website. Volg hiervoor de instructies in [een aangepaste domeinnaam configureren voor uw Azure Storage-account](storage-custom-domain-name.md). Zie voor toegang tot uw website gehost op een aangepaste domeinnaam via HTTPS moet [Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md).

## <a name="pricing-and-billing"></a>Prijzen en facturering
Statische website host is opgegeven zonder extra kosten. Bekijk voor meer informatie over prijzen voor Azure Blob Storage, de [Azure Blob Storage pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Snelstartgids
### <a name="azure-portal"></a>Azure Portal
Als u wilt starten die als host fungeert voor uw webtoepassing op Azure Storage, kunt u de functie met de Azure-Portal configureren en klik op 'Statische website (preview)' onder 'Instellingen' in de linkernavigatiebalk. Klik op "Ingeschakeld" en voer de naam van het Indexdocument en (optioneel) het documentpad aangepaste fout.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Uw web-assets uploaden naar de container '$web' die is gemaakt als onderdeel van een statische website inschakelen. U kunt dit doen rechtstreeks in Azure Portal of u kunt profiteren van [Azure Opslagverkenner](https://azure.microsoft.com/features/storage-explorer/) gehele Active directory-structuren te uploaden. Zorg ervoor dat een Indexdocument met de naam die u hebt geconfigureerd. In dit voorbeeld is de naam van het document 'index.html'.

Ten slotte gaat u naar uw webeindpunt voor het testen van uw website.

## <a name="faq"></a>Veelgestelde vragen
**Statische websites voor alle typen voor storage-accounts beschikbaar is?**  
Nee, statische website die als host fungeert, is alleen beschikbaar in standard GPv2-opslagaccounts.

**Opslag VNET en firewallregels ondersteund op het nieuwe webeindpunt zijn?**  
Ja, is het nieuwe webeindpunt gehoorzaamt aan het VNET en firewall-regels die zijn geconfigureerd voor het opslagaccount.

## <a name="next-steps"></a>Volgende stappen
* [Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)
* [Een aangepaste domeinnaam voor uw eindpunt blob- of webserver configureren](storage-custom-domain-name.md)
* [Azure Functions](/azure-functions/functions-overview.md)
* [Azure Web Apps](/app-service/app-service-web-overview.md)
* [Bouw uw eerste zonder server web-app](https://aka.ms/static-serverless-webapp)
