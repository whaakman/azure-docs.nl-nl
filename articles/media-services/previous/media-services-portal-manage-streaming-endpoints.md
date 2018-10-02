---
title: Streaming-eindpunten met Azure portal beheren | Microsoft Docs
description: In dit onderwerp laat zien hoe voor het beheren van streaming-eindpunten met Azure portal.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: cfowler
editor: ''
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: juliako
ms.openlocfilehash: a023cb2ea1cf74c8029cc207c9b899631facb929
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018261"
---
# <a name="manage-streaming-endpoints-with-the-azure-portal"></a>Streaming-eindpunten met Azure portal beheren

In dit artikel laat zien hoe de Azure-portal gebruiken voor het beheren van streaming-eindpunten. 

>[!NOTE]
>Controleer de [overzicht](media-services-streaming-endpoints-overview.md) artikel. 

Zie voor meer informatie over het schalen van het streaming-eindpunt [dit](media-services-portal-scale-streaming-endpoints.md) artikel.

## <a name="start-managing-streaming-endpoints"></a>Beginnen met het streaming-eindpunten beheren 

Als u wilt beginnen met het beheren van streaming-eindpunten voor uw account, het volgende te doen.

1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. In de **instellingen** Selecteer **Streaming-eindpunten**.
   
    ![Streaming-eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> U wordt alleen gefactureerd wanneer uw Streaming-eindpunt wordt uitgevoerd.

## <a name="adddelete-a-streaming-endpoint"></a>Een streaming-eindpunt toevoegen/verwijderen

>[!NOTE]
>Het standaardstreaming-eindpunt kan niet worden verwijderd.

Voor het streaming-eindpunt met behulp van de Azure-portal toevoegen/verwijderen, het volgende doen:

1. Als u wilt een streaming-eindpunt toevoegen, klikt u op de **+ eindpunt** aan de bovenkant van de pagina. 

    U kunt meerdere Streaming-eindpunten als u van plan bent andere CDN's of een CDN en directe toegang.

2. Als u wilt verwijderen van een streaming-eindpunt, drukt u op **verwijderen** knop.      
3. Klik op de **Start** om te beginnen het streaming-eindpunt.
   
    ![Streaming-eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Het Streaming-eindpunt configureren
Streaming-eindpunt kunt u de volgende eigenschappen configureren:

* Toegangsbeheer
* Cachebeheer
* Cross-site-beleid

Zie voor gedetailleerde informatie over deze eigenschappen [streamingendpoint zo](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

>[!NOTE]
>Wanneer CDN is ingeschakeld, kunt u geen toegang krijgt IP-toegang. IP-toegang is alleen van toepassing als u geen CDN.

U kunt de streaming-eindpunt configureren door het volgende te doen:

1. Selecteer het streaming-eindpunt dat u wilt configureren.
2. Klik op **instellingen**.

Er volgt een korte beschrijving van de velden.

![Streaming-eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. Beleid voor maximale cachegrootte: gebruikt voor het configureren van voor de assets die via dit streaming-eindpunt. Als er geen waarde is ingesteld, wordt de standaardwaarde gebruikt. De standaardwaarden kunnen ook rechtstreeks in Azure storage worden gedefinieerd. Als Azure CDN is ingeschakeld voor het streaming-eindpunt, moet u de beleidswaarde cache niet instellen op minder dan 600 seconden.  
2. Toegestane IP-adressen: Hiermee geeft u IP-adressen die is toegestaan verbinding maken met het gepubliceerde streaming-eindpunt. Als er geen IP-adressen is opgegeven, is elk IP-adres is geen verbinding maken. IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld (10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld ' 10.0.0.1/22') of een IP-bereik met IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld: 10.0.0.1 ' () 255.255.255.0)').
3. Configuratie voor de verificatie van Akamai handtekening-header: gebruikt om op te geven hoe handtekening header-verificatieaanvraag van Akamai-servers is geconfigureerd. Verlooptijd is ingesteld op UTC.

## <a name="scale-your-premium-streaming-endpoint"></a>Schalen van uw Premium streaming-eindpunt

Raadpleeg [dit artikel](media-services-portal-scale-streaming-endpoints.md) voor meer informatie.

## <a id="enable_cdn"></a>Azure CDN-integratie inschakelen

Wanneer u een nieuw account maakt, wordt standaard Streaming-eindpunt Azure CDN-integratie is standaard ingeschakeld.

Als u later wilt naar het CDN inschakelen/uitschakelen, de streaming-eindpunt moet zich in de **gestopt** staat. Dit kan maximaal twee uur voor de Azure CDN-integratie ingeschakeld en de wijzigingen die actief zijn in alle de CDN POP's duren. Echter, u kunt uw streaming-eindpunt en stream zonder onderbrekingen te starten vanaf het streaming-eindpunt en zodra de integratie voltooid is, de stream vanuit het CDN wordt geleverd. Tijdens de inrichting periode uw streaming-eindpunt worden weergegeven in **vanaf** staat en u merkt wellicht verminderde prestaties.

CDN-integratie is ingeschakeld in de Azure-datacenters met uitzondering van China en federale overheid regio's.

Zodra deze is ingeschakeld, de **Access Control**, ** aangepaste hostnaam, en **Akamai Signature-verificatie** configuratie wordt uitgeschakeld.
 
> [!IMPORTANT]
> Azure Media Services-integratie met Azure CDN is geïmplementeerd op **Azure CDN van Verizon** voor standard streaming-eindpunten. Premium-streaming-eindpunten kan worden geconfigureerd met alle **Azure CDN-prijzen van lagen en providers**. Zie voor meer informatie over de functies van Azure CDN de [overzicht van CDN](../../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Aanvullende overwegingen

* Bij het CDN is geactiveerd voor een streaming-eindpunt, aanvragen geen clients inhoud rechtstreeks vanuit de oorsprong. Als u de mogelijkheid om uw inhoud met of zonder CDN te testen, kunt u een ander streaming-eindpunt dat is niet CDN is ingeschakeld.
* De hostnaam van uw streaming eindpunt blijft ongewijzigd na het inschakelen van CDN. U hoeft niet te wijzigen voor uw media services-werkstroom na de CDN is geactiveerd. Bijvoorbeeld, als de hostnaam van uw streaming eindpunt strasbourg.streaming.mediaservices.windows.net, na het inschakelen van CDN, wordt de exacte dezelfde hostnaam gebruikt.
* Nieuwe streaming-eindpunten, kunt u CDN inschakelt door het maken van een nieuw eindpunt; bestaand streaming-eindpunten moet u eerst het eindpunt stoppen en vervolgens inschakelen/uitschakelen het CDN.
* Standard streaming-eindpunt kan alleen worden geconfigureerd met behulp van **standaard CDN van Verizon provider** met behulp van de klassieke Azure-portal. U kunt echter andere Azure CDN-providers met behulp van REST-API's inschakelen.

## <a name="configure-cdn-profile"></a>CDN-profiel configureren

U kunt het CDN-profiel configureren door het selecteren van de **CDN beheren** knop vanaf de bovenkant.

![Streaming-eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

