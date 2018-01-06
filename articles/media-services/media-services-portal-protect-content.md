---
title: Content protection-beleid configureren met behulp van de Azure portal | Microsoft Docs
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
ms.openlocfilehash: 805e1246dbc984582528d2b351d2f14ab2e811fc
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="configure-content-protection-policies-by-using-the-azure-portal"></a>Content protection-beleid configureren met behulp van de Azure-portal
 U kunt uw media vanaf het moment dat het verlaten van uw computer via de opslag, verwerking en levering beveiligen met Azure Media Services. Media Services kunt u de inhoud die is versleuteld met de Advanced Encryption Standard (AES) dynamisch met behulp van 128-bits versleutelingssleutels bezorgen. Ook kunt u deze met common encryption (CENC) met behulp van PlayReady en/of Widevine beheer van digitale rechten (DRM) en FairPlay van Apple. 

Media Services biedt een service voor het leveren van DRM-licenties en AES-sleutels geautoriseerde clients wissen. U kunt de Azure-portal gebruiken voor het maken van een sleutel/licentie verificatiebeleid voor alle typen versleutelingen.

In dit artikel laat zien hoe een content protection-beleid configureren met behulp van de portal. Ook wordt uitgelegd hoe u dynamische versleuteling toepassen op uw assets.

## <a name="start-to-configure-content-protection"></a>Begint met het configureren van beveiliging van inhoud
Als u de portal wilt algemene inhoudsbeveiliging configureren met behulp van uw Media Services-account, moet u de volgende stappen uitvoeren:

1. In de [portal](https://portal.azure.com/), selecteert u uw Media Services-account.

2. Selecteer **instellingen** > **Content protection**.

    ![Content Protection](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Autorisatiebeleid voor sleutels/licenties
Media Services ondersteunt meerdere manieren van gebruikers die sleutel of licentie-aanvragen te verifiëren. U moet het autorisatiebeleid voor inhoudssleutels configureren. De client vervolgens moet voldoen aan het beleid voordat de sleutel/licentie kunnen worden aangeboden aan. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen, openen of token beperkingen hebben.

U kunt de portal gebruiken voor het maken van een sleutel/licentie verificatiebeleid voor alle typen versleutelingen.

### <a name="open-authorization"></a>Open autorisatie
Open beperking betekent dat het systeem de sleutel voor iedereen die een sleutel aanvraag indient levert. Deze beperking kan handig zijn voor testdoeleinden. 

### <a name="token-authorization"></a>Token autorisatie
Het beleid token beperkte vergezeld van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in de eenvoudige web token (SWT) en indelingen JSON Web Token (JWT). Media Services biedt niet een STS. U kunt een aangepaste STS maken of gebruiken van Azure Access Control Service voor het probleem van tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. Als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel (of licentie), stuurt de Media Services sleutellevering-service de aangevraagde sleutel (of licentie) naar de client.

Wanneer u het beleid token beperkt configureert, moet u de verificatie van de primaire sleutel, uitgever en doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met. De certificaatverlener is de secure token service die de token uitgeeft. De doelgroep (ook wel bereik genoemd) beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

![Autorisatiebeleid voor sleutels/licenties](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>PlayReady-licentiesjabloon
De sjabloon PlayReady-licenties Hiermee stelt u de functionaliteit die is ingeschakeld op uw PlayReady-licentie. Zie voor meer informatie over de sjabloon PlayReady-licentie, de [Media Services PlayReady licentie sjabloon overzicht](media-services-playready-license-template-overview.md).

### <a name="nonpersistent"></a>Niet-persistente
Als u een licentie als niet-persistente configureert, wordt deze alleen tijdens de speler maakt gebruik van de licentie bewaard in het geheugen.  

![Niet-persistente inhoudsbeveiliging](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Permanent
Als u een licentie als permanente configureert, wordt deze opgeslagen in de permanente opslag op de client.

![Permanente beveiliging van inhoud](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Sjabloon voor Widevine-licentie
De sjabloon voor Widevine-licentie wordt de functionaliteit die is ingeschakeld op uw Widevine-licenties.

### <a name="basic"></a>Basic
Wanneer u selecteert **Basic**, de sjabloon wordt gemaakt met alle standaardwaarden.

### <a name="advanced"></a>Geavanceerd
Zie voor meer informatie over de rechtensjabloon Widevine de [Widevine-licentie sjabloon overzicht](media-services-widevine-license-template-overview.md).

![Geavanceerde beveiliging van inhoud](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay-configuratie
Als u FairPlay versleuteling selecteert **FairPlay configuratie**. Selecteer vervolgens de **certificaat App** en voer de **toepassing geheime sleutel**. Zie voor meer informatie over de vereisten en FairPlay configuratie [beveiligen van uw inhoud met Apple FairPlay of Microsoft PlayReady HLS](media-services-protect-hls-with-FairPlay.md).

![FairPlay-configuratie](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dynamische versleuteling toepassen op uw asset
Om te profiteren van dynamische versleuteling, moet u het bronbestand coderen in een set adaptive bitrate MP4-bestanden.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Selecteer een asset die u wilt versleutelen
Overzicht van alle uw assets selecteert **instellingen** > **activa**.

![Activa-optie](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Versleutelen met AES of DRM
Wanneer u selecteert **versleutelen** voor een asset, ziet u twee opties: **AES** of **DRM**. 

#### <a name="aes"></a>AES
Schakel codering is ingeschakeld op alle protocollen voor streaming AES: Smooth Streaming, HLS en MPEG-DASH.

![Versleutelingsconfiguratie](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
1. Nadat u hebt geselecteerd **DRM**, ziet u andere inhoud-beveiligingsbeleid (die moeten worden geconfigureerd door dit punt) en een set protocollen voor streaming:

    a. **PlayReady en Widevine met MPEG-DASH** dynamisch versleutelt uw MPEG-DASH-stream met PlayReady en Widevine DRM's.

    b. **PlayReady en Widevine met MPEG-DASH + FairPlay met HLS** dynamisch versleutelen uw MPEG-DASH-stream met PlayReady en Widevine DRM's. Uw HLS-streams met FairPlay met deze optie ook versleuteld.

    c. **PlayReady alleen met Smooth Streaming, HLS en MPEG-DASH** Smooth Streaming, HLS en MPEG-DASH-streams met PlayReady DRM dynamisch worden versleuteld.

    d. **Widevine alleen met MPEG-DASH** dynamisch versleutelt uw MPEG-DASH met Widevine DRM.
    
    e. **FairPlay alleen met HLS** uw HLS-stream met FairPlay dynamisch worden versleuteld.

2. FairPlay-versleuteling inschakelen op de **Content Protection globale instellingen** blade Selecteer **FairPlay configuratie**. Selecteer vervolgens de **certificaat App**, en voer de **toepassing geheime sleutel**.

    ![Versleutelingstype](./media/media-services-portal-content-protection/media-services-content-protection009.png)

3. Nadat u de versleutelingsselectie, selecteer **toepassen**.

>[!NOTE] 
>Als u een HLS AES-versleuteling in Safari speelt wilt, Zie het blogbericht [HLS versleuteld in Safari](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

