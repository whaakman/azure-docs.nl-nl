---
title: Azure CDN gebruiken om toegang te krijgen tot blobs met aangepaste domeinen via HTTPS
description: Meer informatie over het integreren van Azure CDN met Blob Storage voor toegang tot blobs met aangepaste domeinen via HTTPS
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: normesta
ms.reviewer: dineshm
ms.subservice: blobs
ms.openlocfilehash: f3386d04cd3316b38a094524d0d5d4f3c5ab36bb
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68986844"
---
# <a name="use-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>Azure CDN gebruiken om toegang te krijgen tot blobs met aangepaste domeinen via HTTPS

Azure Content Delivery Network (Azure CDN) biedt nu ondersteuning voor HTTPS voor aangepaste domein namen. Met Azure CDN kunt u toegang krijgen tot blobs met behulp van uw aangepaste domein naam via HTTPS. Als u dit wilt doen, schakelt u Azure CDN in op uw BLOB-of Web-eind punt en vervolgens wijst u Azure CDN toe aan een aangepaste domein naam. Wanneer u klaar bent, vereenvoudigt Azure het inschakelen van HTTPS voor uw aangepaste domein via één klik toegang en volledig certificaat beheer. De prijzen voor normale Azure CDN worden niet verhoogd.

Azure CDN beschermt de privacy en gegevens integriteit van uw gegevens van webtoepassingen tijdens de overdracht. Door het SSL-protocol te gebruiken om verkeer via HTTPS te verwerken, Azure CDN uw gegevens versleuteld wanneer ze via internet worden verzonden. Door gebruik te maken van HTTPS met Azure CDN helpt u uw webtoepassingen te beschermen tegen aanvallen.

> [!NOTE]  
> Naast het bieden van SSL-ondersteuning voor aangepaste domein namen, kunt Azure CDN u helpen de schaal van uw toepassing te verg Roten voor het leveren van inhoud met een hoge band breedte over de hele wereld. Zie [overzicht van Azure CDN](../../cdn/cdn-overview.md)voor meer informatie.

## <a name="quickstart"></a>Quick Start

Ga als volgt te werk om HTTPS in te scha kelen voor uw aangepaste Blob Storage-eind punt:

1.  [Integreer een Azure-opslag account met Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).  
    In dit artikel vindt u instructies voor het maken van een opslag account in de Azure Portal, als u dit nog niet hebt gedaan.

    > [!NOTE]  
    > Als u uw opslag-webeindpunt wilt toevoegen tijdens de preview-versie van de ondersteuning van statische websites in Azure Storage, selecteert u **aangepaste oorsprong** in de vervolg keuzelijst **oorsprongs type** . In de Azure Portal moet u dit doen vanuit uw Azure CDN-profiel in plaats van rechtstreeks in uw opslag account.

2.  [Azure CDN inhoud toewijzen aan een aangepast domein](../../cdn/cdn-map-content-to-custom-domain.md).

3.  [Schakel HTTPS in voor een Azure CDN aangepast domein](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Shared Access signatures

Standaard staat Blob Storage-eind punten geen anonieme lees toegang toe. Als uw Blob Storage-eind punt is geconfigureerd om anonieme lees toegang niet toe te staan, geeft u in elke aanvraag een [Shared Access Signature](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) -token op voor uw aangepaste domein. Zie [Anonieme leestoegang tot containers en blobs beheren](storage-manage-access-to-resources.md) voor meer informatie.

Azure CDN respecteert geen beperkingen die worden toegevoegd aan het token voor Shared Access-hand tekeningen. Zo verlopen alle Shared Access Signature-tokens. U hebt nog steeds toegang tot inhoud met een verlopen Shared Access-hand tekening, totdat de inhoud wordt verwijderd uit de Azure CDN Edge-knoop punten. U kunt bepalen hoe lang gegevens worden opgeslagen in de cache van Azure CDN door de Cache-Control-header in te stellen. Zie de [verval datum van Azure Storage-blobs in azure CDN beheren](../../cdn/cdn-manage-expiration-of-blob-content.md)voor meer informatie.

Als u twee of meer Url's voor gedeelde toegangs handtekeningen voor hetzelfde BLOB-eind punt maakt, wordt u aangeraden de query reeks in cache opslaan in te scha kelen voor uw Azure CDN. Met deze actie zorgt u ervoor dat Azure elke URL als een unieke entiteit behandelt. Zie [Cachegedrag in Azure CDN bepalen met queryreeksen](../../cdn/cdn-query-string.md) voor meer informatie.

## <a name="http-to-https-redirection"></a>HTTP-naar-HTTPS-omleiding

U kunt HTTP-verkeer omleiden naar HTTPS. Hiervoor is het gebruik van de Azure CDN Premium-aanbieding van Verizon vereist. Het [HTTP-gedrag met de engine van Azure CDN regels negeren](../../cdn/cdn-rules-engine.md) door de volgende regel toe te passen:

![HTTP-naar-HTTPS-omleidings regel](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

*CDN-endpoint-name*, die u in de vervolg keuzelijst selecteert, verwijst naar de naam die u hebt geconfigureerd voor uw Azure CDN-eind punt. Bronpad verwijst naar het pad binnen uw bron-opslag account, waar uw statische inhoud wordt opgeslagen. Als u alle statische inhoud in één container host, vervangt u het *bronpad* door de naam van die container.

Zie de functies van de [Azure CDN-regel engine](../../cdn/cdn-rules-engine-reference-features.md)voor een diep gaande kennis van regels.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u via Azure CDN toegang hebt tot blobs, betaalt u de [prijzen voor Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/) voor verkeer tussen de Edge-knoop punten en de oorsprong (Blob Storage). U betaalt [Azure CDN prijzen](https://azure.microsoft.com/pricing/details/cdn/) voor gegevens die worden geopend vanuit de Edge-knoop punten.

Stel dat u een opslag account hebt in West-VS dat u via Azure CDN kunt openen. Wanneer iemand in het Verenigd Konink rijk probeert toegang te krijgen tot een BLOB in dat opslag account via Azure CDN, controleert Azure eerst op de BLOB in het Edge-knoop punt dat zich het dichtst bij het Verenigd Konink rijk bevindt. Als Azure de BLOB vindt, krijgt deze een kopie en maakt deze gebruik van Azure CDN prijzen, omdat Azure CDN deze opent. Als Azure de BLOB niet vindt, wordt de BLOB naar het Edge-knoop punt gekopieerd. Met deze actie worden de uitstaande en transactie kosten in rekening gebracht, zoals is opgegeven in de prijzen voor Blob Storage. Azure opent vervolgens het bestand op het Edge-knoop punt, wat leidt tot Azure CDN facturering.

Op de [pagina met Azure CDN prijzen](https://azure.microsoft.com/pricing/details/cdn/)is HTTPS-ondersteuning voor aangepaste domein namen alleen beschikbaar voor Azure CDN van Verizon Standard-en Premium-producten.

## <a name="next-steps"></a>Volgende stappen

* [Een aangepaste domein naam configureren voor het eind punt van de Blob-opslag](storage-custom-domain-name.md)
* [Een statische website hosten in Azure Storage](storage-blob-static-website.md)
