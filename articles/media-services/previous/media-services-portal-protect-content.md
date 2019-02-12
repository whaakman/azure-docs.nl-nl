---
title: Content protection-beleid configureren met behulp van Azure portal | Microsoft Docs
description: In dit artikel ziet u hoe u de Azure portal gebruiken voor het configureren van beleid voor beveiliging van inhoud. Ook wordt uitgelegd hoe u dynamische versleuteling inschakelen voor uw activa.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: dc5439c98b5abc69b6cb1ba144e015426bb887ce
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996931"
---
# <a name="configure-content-protection-policies-by-using-the-azure-portal"></a>Content protection-beleid configureren met behulp van Azure portal
 U kunt uw media vanaf het moment dat het verlaten van uw computer via opslag, verwerking en levering beveiligen met Azure Media Services. U kunt Media Services gebruiken om uw inhoud dynamisch met het Advanced Encryption Standard (AES) versleuteld met behulp van 128-bits versleutelingssleutels te leveren. Ook kunt u deze met algemene versleuteling (CENC) met behulp van PlayReady en/of Widevine digital rights management (DRM) en Apple FairPlay. 

Media Services biedt een service voor het leveren van DRM-licenties en AES clear sleutels naar geautoriseerde clients. De Azure-portal kunt u een autorisatiebeleid voor sleutels/licenties voor alle typen coderingen maken.

In dit artikel ziet u hoe u een beleid voor beveiliging van inhoud configureren via de portal. Ook wordt uitgelegd hoe u dynamische versleuteling toepassen op uw assets.

## <a name="start-to-configure-content-protection"></a>Beginnen met de beveiliging van inhoud configureren
Als u de portal wilt algemene inhoudsbeveiliging configureren met behulp van Media Services-account, moet u de volgende stappen uitvoeren:

1. In de [portal](https://portal.azure.com/), selecteert u uw Media Services-account.

1. Selecteer **instellingen** > **Content protection**.

    ![Inhoudsbeveiliging](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Autorisatiebeleid voor sleutels/licenties
Media Services ondersteunt meerdere manieren om gebruikers die sleutel of licentie aanvragen te verifiëren. U moet het autorisatiebeleid voor de inhoudssleutel configureren. De client vervolgens moet voldoen aan het beleid voordat de sleutels/licenties kan worden geleverd. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben: beperking voor openen of tokenbeperkingen.

U kunt de portal gebruiken om te maken van een autorisatiebeleid voor sleutels/licenties voor alle typen coderingen.

### <a name="open-authorization"></a>Open autorisatie
Open beperking betekent dat het systeem de sleutel levert aan iedereen die sleutels aanvragen. Deze beperking kan handig zijn voor test-doeleinden. 

### <a name="token-authorization"></a>Token autorisatie
Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in de eenvoudige webtoken (SWT) en JSON Web Token (JWT). Media Services biedt geen een STS. U kunt een aangepaste STS maken of gebruik van Azure Access Control Service om probleem tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de tokenbeperking. Als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel (of een licentie), retourneert de Media Services-sleutelleveringsservice de aangevraagde sleutel (of een licentie) naar de client.

Wanneer u de tokenbeperking beleid configureert, moet u de primaire verificatiesleutel, uitgever en doelgroep parameters opgeven. De primaire verificatiesleutel bevat de sleutel die het token is ondertekend. De uitgever wordt de secure token service die de token uitgeeft. De doelgroep (ook wel bereik) beschrijft de intentie van de token of de resource voor het token wordt toegang tot geautoriseerd. De Media Services-sleutelleveringsservice valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

![Autorisatiebeleid voor sleutels/licenties](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>PlayReady-licentiesjabloon
De PlayReady-licentiesjabloon Hiermee stelt u de functionaliteit die is ingeschakeld op uw PlayReady-licentie. Zie voor meer informatie over de PlayReady-licentiesjabloon de [overzicht van sjablonen voor Media Services PlayReady-licentie](media-services-playready-license-template-overview.md).

### <a name="nonpersistent"></a>Niet-persistente
Als u een licentie als niet-persistente configureert, deze alleen vast als Windows media player maakt gebruik van de licentie ondergebracht in het geheugen.  

![Niet-persistente inhoudsbeveiliging](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Permanent
Als u een licentie als permanente configureert, wordt deze opgeslagen in een permanente opslag op de client.

![Permanente beveiliging van inhoud](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Widevine-licentiesjabloon
De Widevine-licentiesjabloon Hiermee stelt u de functionaliteit die is ingeschakeld op uw Widevine-licenties.

### <a name="basic"></a>Basic
Wanneer u selecteert **Basic**, de sjabloon is gemaakt met alle standaardwaarden.

### <a name="advanced"></a>Geavanceerd
Zie voor meer informatie over de Widevine-rechtensjabloon, de [overzicht van Widevine-licentiesjablonen](media-services-widevine-license-template-overview.md).

![Geavanceerde beveiliging van inhoud](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay-configuratie
Als u FairPlay-versleuteling, schakelt **FairPlay-configuratie**. Selecteer vervolgens de **App certificate** en voer de **geheime Toepassingssleutel**. Zie voor meer informatie over de vereisten en FairPlay-configuratie [beveiligen van uw inhoud met Apple FairPlay of Microsoft PlayReady HLS](media-services-protect-hls-with-FairPlay.md).

![FairPlay-configuratie](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dynamische versleuteling toepassen op uw asset
Als u wilt profiteren van dynamische versleuteling, moet u het bronbestand coderen in een set adaptive bitrate MP4-bestanden.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Selecteer een asset die u wilt versleutelen
Al uw activa Selecteer **instellingen** > **activa**.

![Activa-optie](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Coderen met AES of DRM
Wanneer u selecteert **versleutelen** voor een asset, ziet u twee opties: **AES** of **DRM**. 

#### <a name="aes"></a>AES
AES clear key-versleuteling is ingeschakeld voor alle streamingprotocollen: Smooth Streaming, HLS en MPEG-DASH.

![Versleutelingsconfiguratie](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
1. Nadat u hebt geselecteerd **DRM**, ziet u andere inhoud beveiligingsbeleid voor apps (die moeten worden geconfigureerd door dit punt) en een set protocollen voor streaming:

    a. **PlayReady en Widevine met MPEG-DASH** dynamisch versleutelt uw MPEG-DASH-stream met PlayReady en Widevine DRM's.

    b. **PlayReady en Widevine met MPEG-DASH + FairPlay met HLS** uw MPEG-DASH-stream met PlayReady en Widevine DRM's dynamisch wordt versleuteld. Deze optie versleutelt ook uw HLS-streams met FairPlay.

    c. **PlayReady alleen met Smooth Streaming, HLS en MPEG-DASH** dynamisch versleutelt Smooth Streaming, HLS en MPEG-DASH-streams met PlayReady DRM.

    d. **Widevine alleen met MPEG-DASH** dynamisch versleutelt uw MPEG-DASH met Widevine DRM.
    
    e. **FairPlay alleen met HLS** dynamisch versleutelt uw HLS-stream met FairPlay.

1. FairPlay-versleuteling inschakelen voor de **Content Protection globale instellingen** Selecteer **FairPlay-configuratie**. Selecteer vervolgens de **App certificate**, en voer de **geheime Toepassingssleutel**.

    ![Versleutelingstype](./media/media-services-portal-content-protection/media-services-content-protection009.png)

1. Nadat u de versleutelingsselectie, selecteer **toepassen**.

>[!NOTE] 
>Als u van plan bent een met AES versleutelde HLS afspelen in Safari, Zie het blogbericht [versleutelde HLS in Safari](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

