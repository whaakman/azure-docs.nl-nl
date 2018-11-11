---
title: Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS
description: Meer informatie over het Azure CDN integreert met Blob-opslag voor toegang tot blobs met aangepaste domeinen via HTTPS
services: storage
author: michaelhauss
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 110d2d6db4d87294b56b940a02a6633af0d6f482
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219236"
---
# <a name="use-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS

Azure Content Delivery Network (Azure CDN) biedt nu ondersteuning voor HTTPS voor aangepaste domeinnamen. Met Azure CDN, kunt u toegang tot blobs met behulp van de naam van uw aangepaste domein via HTTPS. Azure CDN inschakelt op uw eindpunt blob of web om dit te doen, en vervolgens Azure CDN toewijzen aan een aangepaste domeinnaam. Als u klaar bent, vereenvoudigt Azure HTTPS inschakelen voor uw aangepaste domein via één klik toegang en volledig Certificaatbeheer. Er is geen toename van de normale Azure CDN-tarieven.

Azure CDN beschermt de privacy en integriteit van gegevens van de gegevens van uw web-toepassing zolang het van doorvoer. Met behulp van het SSL-protocol voor het bieden van verkeer via HTTPS, houdt Azure CDN uw gegevens versleuteld wanneer ze via internet worden verzonden. Met behulp van HTTPS met Azure CDN kunt u uw webtoepassingen beschermen tegen een aanval.

> [!NOTE]  
> Naast het verlenen van SSL-ondersteuning voor aangepaste domeinnamen, kunt Azure CDN u schalen van uw toepassing om te leveren van inhoud met hoge bandbreedte over de hele wereld. Zie voor meer informatie, [overzicht van Azure CDN](../../cdn/cdn-overview.md).

## <a name="quickstart"></a>Snelstartgids

Als u wilt HTTPS inschakelen voor uw aangepast eindpunt voor Blob-opslag, het volgende doen:

1.  [Een Azure storage-account integreren met Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).  
    In dit artikel wordt beschreven hoe u een opslagaccount in Azure portal, als u dat nog niet hebt gedaan.

    > [!NOTE]  
    > Selecteer om uw eindpunt op het web opslag toe tijdens de Preview-versie van de statische websites-ondersteuning in Azure Storage **aangepaste oorsprong** in de **oorsprongtype** vervolgkeuzelijst. In de Azure-portal moet u dit doen vanuit uw Azure CDN-profiel in plaats van rechtstreeks in uw storage-account.

2.  [Azure CDN-inhoud toewijzen aan een aangepast domein](../../cdn/cdn-map-content-to-custom-domain.md).

3.  [HTTPS op een aangepast Azure CDN-domein inschakelen](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Handtekeningen voor gedeelde toegang

Blob storage-eindpunten niet standaard toestaan van anonieme toegang voor lezen. Als uw Blob storage-eindpunt is geconfigureerd voor het weigeren van anonieme toegang voor lezen, geeft u een [handtekening voor gedeelde toegang](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) token in elke aanvraag voor uw aangepaste domein. Zie [Anonieme leestoegang tot containers en blobs beheren](storage-manage-access-to-resources.md) voor meer informatie.

Azure CDN niet aansluiten bij de beperkingen die zijn toegevoegd aan de SAS-token. Bijvoorbeeld, verlopen alle SAS-tokens. U kunt nog steeds toegang tot inhoud met een verlopen shared access signature totdat deze inhoud is verwijderd uit de Azure CDN edge-knooppunten. U kunt bepalen hoe lang gegevens worden opgeslagen in de cache van Azure CDN door de Cache-Control-header in te stellen. Voor meer informatie Zie [vervaldatum van Azure Storage-blobs in Azure CDN beheren](../../cdn/cdn-manage-expiration-of-blob-content.md).

Als u twee of meer shared access signature-URL voor de dezelfde blobeindpunt maakt, wordt u aangeraden query queryreeksen opslaan in cache voor uw Azure CDN inschakelen. Deze actie zorgt ervoor dat er elke URL op Azure wordt beschouwd als een unieke entiteit. Zie [Cachegedrag in Azure CDN bepalen met queryreeksen](../../cdn/cdn-query-string.md) voor meer informatie.

## <a name="http-to-https-redirection"></a>HTTP naar HTTPS-omleiding

U kunt HTTP-verkeer omleiden naar HTTPS. In dat geval moet het gebruik van de Azure CDN premium van Verizon-aanbieding. [HTTP-gedrag overschrijven met de Azure CDN regels-engine](../../cdn/cdn-rules-engine.md) door toe te passen van de volgende regel:

![HTTP naar HTTPS-omleiding regel](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

*CDN-eindpunt-name*, die u selecteert in de vervolgkeuzelijst verwijst naar de naam die u hebt geconfigureerd voor uw Azure CDN-eindpunt. *Pad voor de oorsprong* verwijst naar het pad binnen uw opslagaccount oorsprong, waar de statische inhoud is opgeslagen. Als u alle statische inhoud in een enkele container host, vervangt u *-pad voor de oorsprong* met de naam van die container.

Zie voor meer informatie over regels, de [Azure CDN-functies regelengine](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Prijzen en facturering

Als u toegang blobs via Azure CDN tot, betaalt u [Blob opslagprijzen](https://azure.microsoft.com/pricing/details/storage/blobs/) voor verkeer tussen de edge-knooppunten en de oorsprong (Blob storage). U betaalt [Azure CDN-prijzen](https://azure.microsoft.com/pricing/details/cdn/) voor gegevens die toegankelijk vanuit de edge-knooppunten.

Stel dat u hebt een storage-account in VS-West die u toegang wilt via Azure CDN tot krijgen. Wanneer iemand zich in het Verenigd Koninkrijk probeert te krijgen van een blob in het storage-account via Azure CDN, controleert Azure eerst voor de blob in de edge-knooppunt die zich het dichtst bij het Verenigd Koninkrijk. Als Azure vindt de blob, toegang heeft tot een kopie en gebruikt Azure CDN-prijzen, omdat Azure CDN is er toegang toe hebben. Als Azure de blob niet wordt gevonden, wordt de blob gekopieerd naar het edge-knooppunt. Deze actie leidt tot kosten voor uitgaand verkeer en transactie, zoals opgegeven in de prijzen voor Blob storage. Azure heeft vervolgens toegang tot het bestand op het edge-knooppunt, wat in facturering voor Azure CDN resulteert.

Op de [Azure CDN-pagina met prijzen](https://azure.microsoft.com/pricing/details/cdn/), HTTPS-ondersteuning voor namen van aangepaste domeinen is beschikbaar voor Azure CDN van Verizon Standard en Premium-producten alleen.

## <a name="next-steps"></a>Volgende stappen

* [Een aangepaste domeinnaam voor uw Blob storage-eindpunt configureren](storage-custom-domain-name.md)
* [Statische website hosting in Azure Storage (preview)](storage-blob-static-website.md)
