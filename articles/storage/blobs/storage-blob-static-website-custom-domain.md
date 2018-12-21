---
title: 'Zelfstudie: Aangepast domein inschakelen met SSL op een statische website met behulp van Azure CDN - Azure Storage'
description: Leer hoe u een aangepast domein configureert voor het hosten van statische websites.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: tamram
ms.custom: seodec18
ms.openlocfilehash: 6ccd33805fe4b62d3456121321edc4eec3bff2e5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53110283"
---
# <a name="tutorial-use-azure-cdn-to-enable-a-custom-domain-with-ssl-for-a-static-website"></a>Zelfstudie: Azure CDN gebruiken om een aangepast domein met SSL in te schakelen voor een statische website

Deze zelfstudie is deel twee van een serie. Hier leert u om een aangepast domein met SSL voor uw statische website in te schakelen. 

De zelfstudie laat zien hoe u met [Azure CDN](../../cdn/cdn-overview.md) het eindpunt van het aangepaste domein kunt configureren voor uw statische website. Met Azure CDN kunt u aangepaste SSL-certificaten inrichten, een aangepast domein gebruiken en aangepaste herschrijvingsregels configureren, en dat alles op hetzelfde moment. Het configureren van Azure CDN resulteert in extra kosten, maar biedt consistente lage latenties naar uw website vanaf elke locatie ter wereld. Azure CDN biedt ook SSL-versleuteling met uw eigen certificaat. Zie [Azure CDN-prijzen](https://azure.microsoft.com/pricing/details/cdn/) voor meer informatie over prijzen voor Azure CDN.

In deel twee van de serie leert u het volgende:

> [!div class="checklist"]
> * Een CDN-eindpunt op het eindpunt van een statische website maken
> * Aangepast domein en SSL inschakelen

## <a name="prerequisites"></a>Vereisten

Voordat u met deze zelfstudie begint,voert u deel één uit, [Zelfstudie: Een statische website hosten op Blob-opslag](storage-blob-static-website-host.md). 

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/) om aan de slag te gaan.

## <a name="create-a-cdn-endpoint-on-the-static-website-endpoint"></a>Een CDN-eindpunt op het eindpunt van een statische website maken

1. Open [Azure Portal](https://portal.azure.com/) in uw webbrowser. 
1. Zoek uw opslagaccount op en geef het accountoverzicht weer.
1. Selecteer **Azure CDN** onder het menu **Blob-Service** om Azure CDN te configureren.
1. Vul in de sectie **nieuw eindpunt** de velden in om een nieuw CDN-eindpunt te maken.
1. Geef een naam op voor het eindpunt, zoals *mystaticwebsiteCDN*.
1. Geef uw website-domein op als hostnaam voor uw CDN-eindpunt.
1. Geef het eindpunt voor de statische website op als hostnaam van oorsprong. Voor het opzoeken van het eindpunt voor uw statische website gaat u naar de sectie **Statische website** voor uw opslagaccount en kopieert u het eindpunt. 
1. Test uw CDN-eindpunt door naar *mywebsitecdn.azureedge.net* in uw browser te navigeren.

## <a name="enable-custom-domain-and-ssl"></a>Aangepast domein en SSL inschakelen

1. Maak een CNAME-record bij uw domeinnaamprovider als u een aangepast domein naar het CDN-eindpunt wilt omleiden. De CNAME-record voor het *www*subdomein moeten vergelijkbaar zijn met het volgende:

    ![CNAME-record voor het www-subdomein opgeven](media/storage-blob-static-website-custom-domain/subdomain-cname-record.png)

1. Klik in Azure Portal op het zojuist gemaakte eindpunt om het aangepaste domein en het SSL-certificaat te configureren.
1. Selecteer **Aangepast domein toevoegen**, voer uw domeinnaam in en klik op **Toevoegen**.
1. Selecteer de zojuist gemaakte aangepaste domeintoewijzing voor het inrichten van een SSL-certificaat.
1. Stel **HTTPS voor aangepaste domeinen** in op **ON**. Selecteer **CDN beheerd** om Azure CDN uw SSL-certificaat te laten beheren. Klik op **Opslaan**.
1. Test uw website door het openen van de URL van uw website.

## <a name="next-steps"></a>Volgende stappen

In deel twee van deze zelfstudie hebt u geleerd hoe u een aangepast domein kunt configureren met SSL in Azure CDN voor uw statische website.

Volg deze koppeling voor meer informatie over het hosten van statische websites op Azure Storage.

> [!div class="nextstepaction"]
> [Meer informatie over statische websites](storage-blob-static-website.md)
