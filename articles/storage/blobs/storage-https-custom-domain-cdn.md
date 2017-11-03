---
title: Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS
description: Meer informatie over het integreren van Azure CDN met blob storage voor toegang tot blobs met aangepaste domeinen via HTTPS
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mihauss
ms.openlocfilehash: 6bad04df324a374f6e8473890345cf516322abd6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS

Azure Content Delivery Network (CDN) HTTPS wordt nu ondersteuning biedt voor aangepaste domeinnamen.
U kunt gebruikmaken van deze functie voor toegang tot uw aangepaste domein via HTTPS met storage-blobs. Om dit te doen, moet u eerst naar het eindpunt van de blob van Azure CDN inschakelen en de CDN toewijzen aan een aangepaste domeinnaam. Zodra u de volgende stappen uitvoeren, wordt HTTPS inschakelen voor uw aangepaste domein vereenvoudigd, via één klik inschakelen, Certificaatbeheer voltooid en alle waarbij geen extra kosten normale prijzen van CDN.

Deze mogelijkheid is belangrijk omdat Hiermee kunt u de privacy en integriteit van gegevens van uw gevoelige web application gegevens onderweg te beschermen. Met behulp van het SSL-protocol voor verkeer via HTTPS, zorgt u ervoor dat de gegevens worden versleuteld wanneer ze via internet worden verzonden. HTTPS biedt vertrouwensrelatie en de verificatiemethode en uw webtoepassingen worden beschermd tegen aanvallen.

> [!NOTE]
> Naast het bieden van SSL-ondersteuning voor aangepaste domeinnamen, kunt Azure CDN u uw toepassing voor het leveren van inhoud met hoge bandbreedte over de hele wereld worden geschaald.
> Bekijk voor meer informatie, [overzicht van Azure CDN](../../cdn/cdn-overview.md).
>
>

## <a name="quick-start"></a>Snel starten

Dit zijn de stappen die nodig is voor HTTPS voor het eindpunt van de aangepaste blob-opslag:

1.  [Een Azure storage-account integreren met Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).
    Dit artikel begeleidt u bij het maken van een opslagaccount in de Azure Portal als u nog niet gedaan hebt.
2.  [Azure CDN-inhoud toewijzen aan een aangepast domein](../../cdn/cdn-map-content-to-custom-domain.md).
3.  [HTTPS op een aangepast domein van Azure CDN inschakelen](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Handtekeningen voor gedeelde toegang

Als het eindpunt van de blob-opslag is geconfigureerd voor anonieme toegang voor lezen, moet u voor een [Shared Access Signature (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) token bij elke aanvraag die u aanbrengt in uw aangepaste domein. Blob storage-eindpunten niet standaard toestaan van anonieme toegang voor lezen. Zie [anonieme leestoegang tot containers en blobs beheren](storage-manage-access-to-resources.md) voor meer informatie over handtekeningen voor gedeelde toegang.

Azure CDN geen rekening gehouden met eventuele beperkingen die zijn toegevoegd aan het SAS-token. Alle SAS-tokens hebben bijvoorbeeld een verlooptijd. Dit betekent dat de inhoud zijn nog steeds toegankelijk met een verlopen SAS totdat deze inhoud is verwijderd uit de CDN edge-knooppunten. U kunt bepalen hoe lang gegevens opgeslagen in de cache op de CDN door in te stellen de antwoordheader van de cache. Zie [beheer van verlopen van Azure Storage-blobs in Azure CDN](../../cdn/cdn-manage-expiration-of-blob-content.md) voor instructies.

Als u meerdere SAS-URL's voor hetzelfde blobeindpunt maakt, wordt u aangeraden queryreeks cache voor uw Azure CDN inschakelen. Dit is om ervoor te zorgen dat elke URL wordt behandeld als een unieke entiteit. Zie [beheren van Azure CDN cachegedrag met queryreeksen](../../cdn/cdn-query-string.md) voor meer informatie.

## <a name="http-to-https-redirection"></a>HTTP naar HTTPS-omleiding

U kunt aangeven of u HTTP-verkeer omleiden naar HTTPS. U moet hiervoor gebruik van Azure CDN premium van Verizon aanbieden. U moet [overschrijven HTTP gedrag met de engine van Azure CDN regels](../../cdn/cdn-rules-engine.md) aan de volgende regel:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

"Cdn-eindpunt-name" verwijst naar de naam die u hebt geconfigureerd voor uw CDN-eindpunt. U kunt deze waarde uit de vervolgkeuzelijst selecteren. "Oorsprongpad" verwijst naar het pad binnen uw oorsprong storage-account waarin uw statische inhoud zich bevindt.
Als u alle statische inhoud in een enkele container host, vervangt u 'oorsprongpad' met de naam van die container.

Zie voor meer informatie in regels, de [Azure CDN regels engine functies](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u toegang blobs via een Azure CDN tot, betaalt u [Blob storage-prijzen](https://azure.microsoft.com/pricing/details/storage/blobs/) voor verkeer tussen de edge-knooppunten en de oorsprong (Blob-opslag), en [prijzen van CDN](https://azure.microsoft.com/pricing/details/cdn/) voor gegevens die worden gebruikt vanaf de edge-knooppunten.

Stel dat u hebt een opslagaccount in VS-West die wordt geopend met behulp van een Azure CDN. Als iemand zich in het Verenigd Koninkrijk voor toegang tot een van de blobs in dat storage-account via de CDN probeert, controleert Azure eerst het dichtst bij de UK voor blob edge-knooppunt. Als gevonden, deze toegang heeft tot deze kopie van de blob en maakt gebruik van de prijzen van CDN omdat deze wordt geopend op de CDN. Als dat niet wordt gevonden, Azure de blob kopieert naar het edge-knooppunt die resulteren in uitgaande en transactie kosten, zoals opgegeven in de prijzen voor Blob storage en vervolgens toegang tot het bestand op de edge-knooppunt, dat in CDN facturering resulteert.

Bij het onderzoeken van de [CDN pagina met prijzen](https://azure.microsoft.com/pricing/details/cdn/), Let op: HTTPS-ondersteuning voor aangepaste domeinnamen is alleen beschikbaar voor Azure CDN van Verizon producten (standaard en Premium).

## <a name="next-steps"></a>Volgende stappen

[Een aangepaste domeinnaam configureren voor het eindpunt van de Blob-opslag](storage-custom-domain-name.md)
