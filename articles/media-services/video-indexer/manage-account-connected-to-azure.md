---
title: Een Video Indexer-account beheren
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe voor het beheren van een Video Indexer-account met Azure bent verbonden.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: 43e08298618f059ff706d11ce6bbb3d486642872
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292286"
---
# <a name="manage-a-video-indexer-account-connected-to-azure"></a>Een Video Indexer-account dat is verbonden met Azure beheren

In dit artikel ziet u hoe u een Video Indexer-account die is verbonden met uw Azure-abonnement en een Azure Media Services-account beheren.

> [!NOTE]
> U moet de eigenaar van de Video Indexer-account wilt account aanpassingen in de configuratie die in dit onderwerp wordt besproken.

## <a name="prerequisites"></a>Vereisten

Verbinding maken met uw Video Indexer-account naar Azure, zoals beschreven in [met Azure bent verbonden](connect-to-azure.md). 

Zorg ervoor dat u [vereisten](connect-to-azure.md#prerequisites) en bekijk [overwegingen met betrekking tot](connect-to-azure.md#considerations) in het artikel.

## <a name="examine-account-settings"></a>Accountinstellingen bekijken

In dit gedeelte worden de instellingen van uw Video Indexer-account.

Instellingen weergeven:

1. Klik op het pictogram van de gebruiker in de rechterbovenhoek en selecteer **instellingen**.

    ![Instellingen](./media/manage-account-connected-to-azure/select-settings.png)

2. Op de **instellingen** weergeeft, schakelt de **Account** tabblad.

Als uw video Indexer-account is verbonden met Azure, ziet u het volgende:

* De naam van de onderliggende Azure Media Services-account.
* Het aantal indexeertaken laten uitvoeren en in de wachtrij geplaatst.
* Het aantal en type van de toegewezen gereserveerde eenheden.

Als uw account aanpassen moet, ziet u relevante fouten en waarschuwingen over de configuratie van uw account op de **instellingen** pagina. De berichten bevatten koppelingen naar de exacte plaatsen in Azure portal waar u moet wijzigingen aanbrengen. Zie voor meer informatie de [fouten en waarschuwingen](#errors-and-warnings) volgende sectie.

## <a name="auto-scale-reserved-units"></a>Automatisch schalen, gereserveerde eenheden

De **instellingen** pagina kunt u de automatische schaalaanpassing van Media gereserveerde eenheden (RU) instellen. Als de optie **op**, kunt u het maximum aantal ru's toewijzen en zorg ervoor dat dat Video Indexer stoppen/starten ru's automatisch. Met deze optie niet extra geld te betalen voor niet-actieve tijd maar ook moet u niet wachten voor indexering van taken voor het voltooien van een lange tijd wanneer de indexering belasting hoog is.

Automatisch schalen schaalt niet 1 RU of hoger dan de standaardlimiet van het Media Services-account. Als u wilt de limiet te verhogen, moet u een serviceaanvraag maken. Zie voor meer informatie over quota en beperkingen en hoe u een ondersteuningsticket [quota en beperkingen](../../media-services/previous/media-services-quotas-and-limitations.md).

![Aanmelden](./media/manage-account-connected-to-azure/autoscale-reserved-units.png)

## <a name="errors-and-warnings"></a>Fouten en waarschuwingen

Als uw account aanpassen moet, ziet u relevante fouten en waarschuwingen over de configuratie van uw account op de **instellingen** pagina. De berichten bevatten koppelingen naar de exacte plaatsen in Azure portal waar u moet wijzigingen aanbrengen. Deze sectie vindt u meer informatie over de fout- en waarschuwingsberichten.

* Event Grid

    U hebt voor het registreren van de EventGrid resourceprovider met behulp van de Azure portal. In de [Azure-portal](https://portal.azure.com/), gaat u naar **abonnementen** > [abonnement] > **resourceproviders en** > **Microsoft.EventGrid**. Als dat niet in de **geregistreerde** staat, klikt u op **registreren**. Het duurt enkele minuten om te registreren. 

* Streaming-eindpunt

    Zorg ervoor dat de onderliggende Media Services-account is de standaardwaarde **Streaming-eindpunt** in de status van een gestart. Anders niet mogelijk om te bekijken van video's vanaf deze Media Services-account of in Video Indexer.

* Gereserveerde media-eenheden 

    U moet gereserveerde Media-eenheden toewijzen voor uw Media Service-resource in volgorde naar index video's. Voor optimale prestaties indexering, is het raadzaam om toe te wijzen ten minste 10 gereserveerde S3-eenheden. Voor informatie over prijzen, Zie de sectie Veelgestelde vragen over van de [prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/) pagina.   

## <a name="next-steps"></a>Volgende stappen

U kunt via programmacode communiceren met uw proefaccount en/of met uw Video Indexer-accounts die zijn verbonden met azure door de instructies in: [gebruik API's](video-indexer-use-apis.md).

U moet dezelfde Azure AD-gebruiker die u hebt gebruikt bij het verbinden met Azure.
