---
title: De Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS
description: Meer informatie over het Azure CDN integreren met blob-opslag voor toegang tot blobs met aangepaste domeinen via HTTPS
services: storage
author: michaelhauss
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 7c4acc7d0832442b94735619ea3a01cb319da993
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398252"
---
# <a name="using-the-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>De Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS
Azure Content Delivery Network (CDN) biedt nu ondersteuning voor HTTPS voor aangepaste domeinnamen. U kunt gebruikmaken van deze functie voor toegang tot opslag-blobs met behulp van uw aangepaste domein via HTTPS. Om dit te doen, moet u eerst Azure CDN inschakelen op uw blob of web-eindpunt en het CDN toewijzen aan een aangepaste domeinnaam. Nadat u de volgende stappen uitvoeren, is inschakeling van HTTPS voor uw aangepaste domein vereenvoudigd via één klik mogelijk maken, volledig Certificaatbeheer en alle zonder extra kosten voor normale CDN-prijzen.

Deze mogelijkheid is belangrijk omdat Hiermee kunt u de privacy en integriteit van gegevens van uw gevoelige web toepassingsgegevens die onderweg zijn. Gebruik het SSL-protocol voor het bieden van verkeer via HTTPS, zorgt u ervoor dat de gegevens worden versleuteld wanneer ze via internet worden verzonden. HTTPS biedt vertrouwen en verificatie, en uw webtoepassingen beschermt tegen aanvallen.

> [!NOTE]  
> Naast het verlenen van SSL-ondersteuning voor aangepaste domeinnamen, kunt het Azure CDN u schalen van uw toepassing om te leveren van inhoud met hoge bandbreedte over de hele wereld. Bekijk voor meer informatie, [overzicht van het Azure CDN](../../cdn/cdn-overview.md).

## <a name="quick-start"></a>Snel starten
Dit zijn de stappen die nodig zijn voor het inschakelen van HTTPS voor uw aangepaste blob storage-eindpunt:

1.  [Een Azure storage-account integreren met Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).
    In dit artikel wordt beschreven hoe u een opslagaccount in de Azure-Portal als u dit nog niet hebt gedaan.

    > [!NOTE]  
    > Tijdens de Preview-versie van ondersteuning voor statische websites in Azure Storage, selecteert u 'aangepaste oorsprong' in de vervolgkeuzelijst 'oorsprongtype ' om toe te voegen uw storage-web-eindpunt. In Azure Portal moet u dit doen vanuit uw CDN-profiel in plaats van rechtstreeks in uw storage-account.

2.  [Azure CDN-inhoud toewijzen aan een aangepast domein](../../cdn/cdn-map-content-to-custom-domain.md).
3.  [HTTPS op een aangepast Azure CDN-domein inschakelen](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Handtekeningen voor gedeelde toegang
Als uw blob storage-eindpunt is geconfigureerd voor het weigeren van anonieme toegang voor lezen, moet u voor een [Shared Access Signature (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) token in elke aanvraag die u aanbrengt in uw aangepaste domein. Blob storage-eindpunten niet standaard toestaan van anonieme toegang voor lezen. Zie [anonieme leestoegang tot containers en blobs beheren](storage-manage-access-to-resources.md) voor meer informatie over handtekeningen voor gedeelde toegang.

Azure CDN geen rekening gehouden met eventuele beperkingen die zijn toegevoegd aan het SAS-token. Alle SAS-tokens hebben bijvoorbeeld een verlooptijd. Dit betekent dat de inhoud nog steeds toegankelijk met een verlopen SAS totdat deze inhoud is verwijderd uit het CDN edge-knooppunten. U kunt bepalen hoe lang gegevens opgeslagen in de cache op het CDN door in te stellen de antwoordheader van de cache. Zie [vervaldatum van Azure Storage-blobs in Azure CDN beheren](../../cdn/cdn-manage-expiration-of-blob-content.md) voor instructies.

Als u meerdere SAS-URL's voor het dezelfde blobeindpunt maakt, wordt u aangeraden querytekenreeksen voor uw Azure CDN inschakelen. Dit is om ervoor te zorgen dat elke URL wordt beschouwd als een unieke entiteit. Zie [beheren van Azure CDN-cachinggedrag met queryreeksen](../../cdn/cdn-query-string.md) voor meer informatie.

## <a name="http-to-https-redirection"></a>HTTP naar HTTPS-omleiding
U kunt aangeven of u HTTP-verkeer omleiden naar HTTPS. Hiervoor moet gebruik van de Azure CDN premium van Verizon-aanbieding. U moet [overschrijven HTTP-gedrag met behulp van de regels-engine van Azure CDN](../../cdn/cdn-rules-engine.md) aan de volgende regel:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

"Cdn-eindpunt-name" verwijst naar de naam die u hebt geconfigureerd voor uw CDN-eindpunt. U kunt deze waarde in de vervolgkeuzelijst selecteren. "Bronpad" verwijst naar het pad binnen uw oorsprong storage-account waarin uw statische inhoud zich bevindt. Als u alle statische inhoud in een enkele container host, vervangt u 'oorsprongpad' met de naam van die container.

Zie voor meer informatie over regels, de [Azure CDN-functies regelengine](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Prijzen en facturering
Als u toegang blobs via een Azure CDN tot, betaalt u [Blob opslagprijzen](https://azure.microsoft.com/pricing/details/storage/blobs/) voor verkeer tussen de edge-knooppunten en de oorsprong (Blob storage), en [CDN-prijzen](https://azure.microsoft.com/pricing/details/cdn/) voor gegevens die worden gebruikt in de edge-knooppunten.

Stel dat u hebt een storage-account in VS-West die wordt geopend met behulp van een Azure CDN. Als iemand zich in het Verenigd Koninkrijk voor toegang tot een van de blobs in dat opslagaccount via het CDN probeert, wordt eerst het edge-knooppunt die het dichtst bij het Verenigd Koninkrijk van die blob in Azure gecontroleerd. Als gevonden, deze toegang heeft tot deze kopie van de blob en wordt gebruik CDN-prijzen, omdat deze wordt geopend op het CDN. Als dat niet wordt gevonden, kopieert Azure de blob op het edge-knooppunt, die leiden tot kosten voor uitgaand verkeer en transactie zoals opgegeven in de prijzen voor Blob storage, en vervolgens toegang tot het bestand op het edge-knooppunt tot CDN-facturering leiden zal.

Ga hiervoor naar de [pagina met prijzen voor CDN](https://azure.microsoft.com/pricing/details/cdn/), houd er rekening mee dat HTTPS-ondersteuning voor aangepaste domeinnamen is alleen beschikbaar voor Azure CDN van Verizon-producten (Standard en Premium).

## <a name="next-steps"></a>Volgende stappen
* [Een aangepaste domeinnaam voor uw Blob storage-eindpunt configureren](storage-custom-domain-name.md)
* [Statische website hosting in Azure Storage (Preview)](storage-blob-static-website.md)
