---
title: 'Zelfstudie: Aangepast domein inschakelen met SSL op een statische website met behulp van Azure CDN - Azure Storage'
description: Leer hoe u een aangepast domein configureert voor het hosten van statische websites.
services: storage
author: normesta
ms.service: storage
ms.topic: tutorial
ms.date: 05/22/2019
ms.author: normesta
ms.reviewer: seguler
ms.custom: seodec18
ms.openlocfilehash: 2b0bb94be2ba8ea983cda8fd015d05fcd532f2bc
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226106"
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

1. Zoek uw opslagaccount in Azure portal en een overzicht van het account weergegeven.
1. Selecteer **Azure CDN** onder het menu **Blob-Service** om Azure CDN te configureren.
1. In de **CDN-profiel** sectie, geeft u een nieuwe of bestaande CDN-profiel. Zie voor meer informatie [Snelstart: Een Azure CDN-profiel en een eindpunt maken](../../cdn/cdn-create-new-endpoint.md).
1. Geef een prijscategorie voor het CDN-eindpunt. In deze zelfstudie wordt de **Standard Akamai** prijscategorie, omdat deze wordt doorgegeven snel, meestal binnen een paar minuten. Andere Prijscategorieën kunnen langer duren doorgegeven, maar kan ook andere voordelen bieden. Zie voor meer informatie, [vergelijken Azure CDN-producteigenschappen](../../cdn/cdn-features.md).
1. In de **de naam van de CDN-eindpunt** veld, geeft u een naam op voor uw CDN-eindpunt. Het CDN-eindpunt moet uniek zijn in Azure.
1. Geef je het eindpunt voor statische website in de **hostnaam van oorsprong** veld. Als u uw eindpunt voor statische website zoekt, gaat u naar de **statische website** -instellingen voor uw storage-account. Het primaire eindpunt Kopieer en plak deze in de CDN-configuratie, het verwijderen van het protocol-id (*bijvoorbeeld*, HTTPS).

    De volgende afbeelding toont een voorbeeld van de endpoint-configuratie:

    ![Schermopname die laat zien CDN-eindpunt voorbeeldconfiguratie](media/storage-blob-static-website-custom-domain/add-cdn-endpoint.png)

1. Maken van het CDN-eindpunt en wacht totdat deze worden doorgegeven.
1. Om te controleren of het CDN-eindpunt juist is geconfigureerd, klikt u op het eindpunt om te navigeren naar de instellingen ervan. Uit het CDN-overzicht voor uw opslagaccount, zoek de hostnaam van eindpunt en Ga naar het eindpunt, zoals wordt weergegeven in de volgende afbeelding. De indeling van uw CDN-eindpunt is vergelijkbaar met `https://staticwebsitesamples.azureedge.net`.

    ![Schermopname die laat zien overzicht van CDN-eindpunt](media/storage-blob-static-website-custom-domain/verify-cdn-endpoint.png)

    Zodra de doorgifte van CDN-eindpunt voltooid is, wordt de inhoud van het index.html-bestand dat u eerder hebt geüpload naar uw statische website te navigeren naar het CDN-eindpunt wordt weergegeven.

1. Navigeer naar de oorsprong om instellingen te controleren voor uw CDN-eindpunt, **oorsprong** onder de **instellingen** sectie voor uw CDN-eindpunt. U ziet dat de **oorsprongtype** veld is ingesteld op *aangepaste oorsprong* en dat de **hostnaam van oorsprong** veld wordt het eindpunt voor statische website weergegeven.

    ![Schermafbeelding van de oorspronkelijke instellingen voor CDN-eindpunt](media/storage-blob-static-website-custom-domain/verify-cdn-origin.png)

## <a name="enable-custom-domain-and-ssl"></a>Aangepast domein en SSL inschakelen

1. Maak een CNAME-record bij uw domeinnaamprovider als u een aangepast domein naar het CDN-eindpunt wilt omleiden. De CNAME-record voor het *www*subdomein moeten vergelijkbaar zijn met het volgende:

    ![CNAME-record voor het www-subdomein opgeven](media/storage-blob-static-website-custom-domain/subdomain-cname-record.png)

1. In de Azure-portal, moet u de instellingen voor uw CDN-eindpunt weergegeven. Navigeer naar **aangepaste domeinen** onder **instellingen** om het aangepaste domein en het SSL-certificaat te configureren.
1. Selecteer **Aangepast domein toevoegen**, voer uw domeinnaam in en klik op **Toevoegen**.
1. Selecteer de nieuwe aangepaste domeintoewijzing voor het inrichten van een SSL-certificaat.
1. Instellen **HTTPS voor aangepast domein** naar **ON**, klikt u vervolgens op **opslaan**. Het duurt enkele uren in uw aangepast domein configureren. Voortgang zoals wordt weergegeven in de volgende afbeelding wordt weergegeven in de portal.

    ![Schermafbeelding van de voortgang van de configuratie van aangepast domein](media/storage-blob-static-website-custom-domain/configure-custom-domain-https.png)

1. De toewijzing van uw statische website aan uw aangepaste domein testen door het openen van de URL voor uw aangepaste domein.

Zie voor meer informatie over het inschakelen van HTTPS voor aangepaste domeinen [zelfstudie: HTTPS configureren op een aangepast Azure CDN-domein](../../cdn/cdn-custom-ssl.md).

## <a name="next-steps"></a>Volgende stappen

In deel twee van deze zelfstudie hebt u geleerd hoe u een aangepast domein kunt configureren met SSL in Azure CDN voor uw statische website.

Zie voor meer informatie over het configureren en het gebruik van Azure CDN [wat is Azure CDN?](../../cdn/cdn-overview.md).