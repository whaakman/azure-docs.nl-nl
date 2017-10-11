---
title: Configureren van beleidsregels voor beveiliging van inhoud met behulp van de Azure-portal | Microsoft Docs
description: In dit artikel laat zien hoe de Azure portal gebruiken voor het configureren van beleid voor beveiliging van inhoud. Ook wordt uitgelegd hoe u dynamische versleuteling voor de activa in te schakelen.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 67b3fa9936daebeafb7e87fe3a7b0c7e0105b3b3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Beleidsregels voor beveiliging van inhoud met behulp van de Azure-portal configureren
> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
> 
> 

## <a name="overview"></a>Overzicht
Microsoft Azure Media Services (AMS) kunt u voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via de opslag, verwerking en levering. Media Services kunt u leveren de inhoud versleuteld dynamisch met Advanced Encryption Standard (AES) (met behulp van coderingssleutels 128-bits), common encryption (CENC) met PlayReady en/of Widevine DRM en Apple FairPlay. 

AMS voorziet in een service voor het leveren van DRM-licenties en AES-sleutels geautoriseerde clients wissen. De Azure-portal kunt u een maken **sleutel/licentie autorisatiebeleid** voor alle typen versleutelingen.

In dit artikel laat zien hoe beleid voor beveiliging van inhoud configureren met de Azure-portal. Ook wordt uitgelegd hoe u dynamische versleuteling toepassen op uw assets.


> [!NOTE]
> Als u de klassieke Azure portal gebruikt voor het maken van beleid voor gegevensbeveiliging, het beleid mogelijk niet in de [Azure-portal](https://portal.azure.com/). Echter alle beleidsregels van de oude, nog bestaan. U kunt deze met Azure Media Services .NET SDK controleren of de [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) hulpprogramma (overzicht van het beleid met de rechtermuisknop op de asset -> weergave informatie (F4) -> Klik op het tabblad inhoud sleutels -> Klik op de sleutel). 
> 
> Als u versleutelen van uw asset met behulp van nieuwe beleidsregels wilt, configureert u deze met de Azure-portal, klik op Opslaan en dynamische versleuteling toepassen. 
> 
> 

## <a name="start-configuring-content-protection"></a>Beginnen met de configuratie van de beveiliging van inhoud
Als u de portal wilt beginnen met de configuratie van de beveiliging van inhoud, globale op uw AMS-account, het volgende doen:

1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. Selecteer **instellingen** > **Content protection**.

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Autorisatiebeleid voor de sleutel/licentie
AMS ondersteunt meerdere manieren van gebruikers die sleutel of licentie-aanvragen te verifiëren. Het autorisatiebeleid voor inhoudssleutels moet worden geconfigureerd door u en voldaan door de client om de sleutel/licentie worden delived naar de client. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben: **openen** of **token** beperking.

De Azure-portal kunt u een maken **sleutel/licentie autorisatiebeleid** voor alle typen versleutelingen.

### <a name="open"></a>Open
Open beperking betekent dat het systeem de sleutel voor iedereen die een sleutel aanvraag indient levert. Deze beperking kan handig zijn voor testdoeleinden. 

### <a name="token"></a>Token
Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door Secure Token Service (STS). Media Services ondersteunt tokens in de indeling Simple Web Tokens (SWT) en JSON Web Token (JWT)-indeling. Media Services biedt geen Token Services beveiligen. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS voor het probleem van tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. De Media Services sleutellevering-service wordt de aangevraagde sleutel (of licentie) naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel (of licentie) geretourneerd.

Wanneer beleid voor het configureren van het token worden beperkt, moet u de verificatie van de primaire sleutel, uitgever en doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met, certificaatverlener is de secure token service die de token uitgeeft. De doelgroep (ook wel bereik genoemd) beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady-rechtensjabloon
Zie voor gedetailleerde informatie over de rechtensjabloon PlayReady [Media Services PlayReady licentie sjabloon overzicht](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Niet-permanente
Als u de licentie als niet-permanente configureert, wordt deze alleen bewaard in het geheugen terwijl de licentie wordt gebruikt door Windows media player.  

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Permanente
Als u de licentie als permanente configureert, wordt deze opgeslagen in de permanente opslag op de client.

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine-rechtensjabloon
Zie voor gedetailleerde informatie over de rechtensjabloon Widevine [Widevine-licentie sjabloon overzicht](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Basic
Wanneer u selecteert **Basic**, de sjabloon wordt gemaakt met alle standaardwaarden waarden.

### <a name="advanced"></a>Geavanceerd
Zie voor gedetailleerde uitleg over geavanceerde optie Widevine configuraties [dit](media-services-widevine-license-template-overview.md) onderwerp.

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay-configuratie
FairPlay om versleuteling te schakelen, moet u het certificaat van de App en de toepassing geheime sleutel (vraag) bieden via de optie FairPlay-configuratie. Zie voor gedetailleerde informatie over de vereisten en FairPlay configuratie [dit](media-services-protect-hls-with-fairplay.md) artikel.

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dynamische versleuteling toepassen op uw asset
Om te profiteren van dynamische versleuteling, moet u het bronbestand coderen in een set adaptive bitrate MP4-bestanden.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Selecteer een asset die u wilt versleutelen
Overzicht van alle uw assets selecteert **instellingen** > **activa**.

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Versleutelen met AES of DRM
Indrukken **versleutelen** op een actief, krijgt u twee opties: **AES** of **DRM**. 

#### <a name="aes"></a>AES
AES wissen sleutelcodering wordt ingeschakeld voor alle protocollen voor streaming: Smooth Streaming, HLS en MPEG-DASH.

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
Wanneer u het tabblad DRM selecteert, krijgt u verschillende mogelijkheden van het beleid van de beveiliging van inhoud (die u moet nu hebt geconfigureerd) + een reeks protocollen voor streaming.

* **PlayReady en Widevine met MPEG-DASH** -wordt uw MPEG-DASH-stream met PlayReady en Widevine DRM's dynamisch versleutelen.
* **PlayReady en Widevine met MPEG-DASH + FairPlay met HLS** -wordt dynamisch versleutelen u MPEG-DASH-stroom met PlayReady en Widevine DRM's. Uw HLS-streams met FairPlay zal ook worden versleuteld.
* **PlayReady alleen met Smooth Streaming, HLS en MPEG-DASH** -wordt dynamisch versleutelen Smooth Streaming, HLS, MPEG-DASH-streams met PlayReady DRM.
* **Widevine alleen met MPEG-DASH** -wordt dynamisch versleutelen u MPEG-DASH met Widevine DRM.
* **FairPlay alleen met HLS** -wordt uw HLS-stream met FairPlay dynamisch versleutelen.

FairPlay om versleuteling te schakelen, moet u het certificaat van de App en de toepassing geheime sleutel (vraag) via de optie FairPlay configuratie van de beveiliging van inhoud-instellingenblade opgeven.

![Inhoud beschermen](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Wanneer u de versleutelingsselectie hebt gemaakt, drukt u op **toepassen**.

>[!NOTE] 
>Als u van plan bent om af te spelen een AES HLS in Safari versleuteld, Zie [deze blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

