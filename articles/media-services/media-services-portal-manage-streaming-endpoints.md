---
title: Streaming-eindpunten met de Azure-portal beheren | Microsoft Docs
description: Dit onderwerp leest hoe voor het beheren van streaming-eindpunten met de Azure-portal.
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: juliako
ms.openlocfilehash: e6f551a7970f226ba40753009b24bd4c5eeb67fb
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="manage-streaming-endpoints-with-the-azure-portal"></a>Streaming-eindpunten met de Azure-portal beheren

In dit artikel laat zien hoe de Azure portal gebruiken voor het beheren van streaming-eindpunten. 

>[!NOTE]
>Leest de [overzicht](media-services-streaming-endpoints-overview.md) artikel. 

Zie voor meer informatie over het schalen van het streaming-eindpunt [dit](media-services-portal-scale-streaming-endpoints.md) artikel.

## <a name="start-managing-streaming-endpoints"></a>Beginnen met het streaming-eindpunten beheren 

Als u wilt beginnen met het beheren van streaming-eindpunten voor uw account, het volgende te doen.

1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. In de **instellingen** blade Selecteer **Streaming-eindpunten**.
   
    ![Streaming-eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> U wordt alleen gefactureerd als uw Streaming-eindpunt wordt uitgevoerd.

## <a name="adddelete-a-streaming-endpoint"></a>Een streaming-eindpunt toevoegen/verwijderen

>[!NOTE]
>Het standaard streaming-eindpunt kan niet worden verwijderd.

Als u wilt een streaming-eindpunt met de Azure portal toevoegen/verwijderen, het volgende doen:

1. Als u wilt een streaming-eindpunt toevoegen, klikt u op de **+ eindpunt** boven aan de pagina. 

    U kunt meerdere Streaming-eindpunten als u van plan bent een andere CDN of een CDN en directe toegang heeft.

2. Als u wilt verwijderen een streaming-eindpunt, drukt u op **verwijderen** knop.      
3. Klik op de **Start** om te beginnen het streaming-eindpunt.
   
    ![Streaming-eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Het Streaming-eindpunt configureren
Streaming-eindpunt, kunt u de volgende eigenschappen configureren:

* Toegangsbeheer
* Cache-control
* Cross-site-beleid

Zie voor gedetailleerde informatie over deze eigenschappen [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

>[!NOTE]
>Wanneer CDN is ingeschakeld, kunt u IP-toegang niet openen. IP-toegang is alleen van toepassing wanneer u geen CDN hebt.

U kunt de streaming-eindpunt configureren als volgt:

1. Selecteer het streaming-eindpunt dat u wilt configureren.
2. Klik op **instellingen**.

Hier volgt een korte beschrijving van de velden.

![Streaming-eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. Maximale cachegrootte beleid: gebruikt voor het configureren van de maximumtijd in cache voor bedrijfsmiddelen die worden geleverd via deze streaming-eindpunt. Als geen waarde is ingesteld, wordt de standaardwaarde gebruikt. De standaardwaarden kunnen ook rechtstreeks in Azure-opslag worden gedefinieerd. Als Azure CDN is ingeschakeld voor het streaming-eindpunt, moet u de beleidswaarde van cache niet ingesteld op minder dan 600 seconden.  
2. Toegestane IP-adressen: gebruikt voor het IP-adressen die verbinding maken met het gepubliceerde streaming-eindpunt mogen opgeven. Als geen IP-adressen is opgegeven, heeft elk IP-adres zou zijn, geen verbinding maken. IP-adressen kunnen worden opgegeven als een enkel IP-adres (bijvoorbeeld: (10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld (10.0.0.1/22) of een IP-adresbereik IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld: 10.0.0.1 ' () 255.255.255.0)').
3. Configuratie voor Akamai signature header-verificatie: gebruikt om op te geven hoe handtekening header-verificatieaanvraag van Akamai servers is geconfigureerd. Verlooptijd is ingesteld op UTC.

## <a name="scale-your-premium-streaming-endpoint"></a>Schalen van uw Premium streaming-eindpunt

Raadpleeg [dit artikel](media-services-portal-scale-streaming-endpoints.md) voor meer informatie.

## <a id="enable_cdn"></a>Integratie van Azure CDN inschakelen

Wanneer u een nieuw account maakt, wordt standaard Streaming-eindpunt Azure CDN-integratie is standaard ingeschakeld.

Als u later de CDN inschakelen/uitschakelen, uw streaming-eindpunt moet in de **gestopt** status. Dit kan maximaal twee uur voor de integratie van Azure CDN ophalen ingeschakeld en de wijzigingen worden actief zijn op alle de CDN POP's duren. Echter, u kunt uw streaming-eindpunt en stream zonder onderbrekingen te starten vanaf het streaming-eindpunt en zodra de integratie voltooid is, de stream wordt geleverd vanaf de CDN. Tijdens het inrichtingsproces periode uw streaming-eindpunt worden weergegeven in **vanaf** status en u merkt wellicht degredad prestaties.

CDN-integratie is ingeschakeld in de Azure-datacenters met uitzondering van China en overheid regio's.

Zodra deze is ingeschakeld, de **toegangsbeheer**, ** aangepaste hostnaam, en **Akamai Signature-verificatie** configuratie opgehaald uitgeschakeld.
 
> [!IMPORTANT]
> Azure Media Services-integratie met Azure CDN is geïmplementeerd op **Azure CDN van Verizon** voor standaard streaming-eindpunten. Premium-streaming-eindpunten kan worden geconfigureerd met alle **Azure CDN prijzen lagen en providers**. Zie voor meer informatie over de functies van Azure CDN de [overzicht van CDN](../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Aanvullende overwegingen

* Wanneer CDN voor streaming-eindpunt is ingeschakeld, kunnen geen clients inhoud opvragen rechtstreeks vanuit de oorsprong. Als u de mogelijkheid om uw inhoud met of zonder CDN te testen, kunt u een ander streaming-eindpunt dat niet ingeschakeld CDN is kunt maken.
* Hostnaam van uw streaming-eindpunt hetzelfde is gebleven na het inschakelen van CDN. U hoeft niet te wijzigen voor uw media services-werkstroom nadat CDN is ingeschakeld. Bijvoorbeeld, als uw streaming endpoint-hostnaam strasbourg.streaming.mediaservices.windows.net, nadat de CDN is ingeschakeld, wordt de exacte dezelfde hostnaam gebruikt.
* Voor nieuwe streaming-eindpunten, kunt u CDN inschakelen door te maken van een nieuw eindpunt; voor bestaande streaming-eindpunten moet u eerst het eindpunt te stoppen en klik vervolgens in-of uitschakelen de CDN.
* Standaard streaming-eindpunt kan alleen worden geconfigureerd met behulp van **Verizon standaard CDN-provider** met behulp van de klassieke Azure-portal. U kunt echter andere Azure CDN-providers met REST API's inschakelen.

## <a name="configure-cdn-profile"></a>CDN-profiel configureren

U kunt de CDN-profiel configureren met behulp van de **beheren CDN** knop vanaf de bovenkant.

![Streaming-eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

