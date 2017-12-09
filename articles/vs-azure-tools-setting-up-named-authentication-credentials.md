---
title: Instellen van referenties voor verificatie met de naam | Microsoft Docs
description: "Informatie over het opgeven van referenties die Visual Studio gebruiken kunt om aanvragen te verifiëren naar Azure, zodat u kunt een toepassing in Azure vanuit Visual Studio publiceren of een bestaande cloudservice bewaken."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2017
ms.author: kraigb
ms.openlocfilehash: fc6f88ee3b808e46e693de7c31b836be86728cd5
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="set-up-named-authentication-credentials"></a>Benoemde verificatiereferenties instellen

Voor het publiceren van een toepassing in Azure of een bestaande cloudservice bewaken vereist Visual Studio-referenties voor verificatie van aanvragen naar Azure, namelijk uw Azure-abonnement-ID en een geldig X.509 v3-certificaat met een sleutel van minstens 2048 bits. U opgeven dat deze referenties via een van de volgende methoden:

- Klik in Visual Studio, selecteer **weergave > Server Explorer**, met de rechtermuisknop op de **Azure** knooppunt **verbinding maken met Microsoft Azure-abonnement**, en meld u aan.
- Maak een abonnementsbestand (`.publishsettings`), die een openbare sleutel voor het certificaat bevat. Het abonnementsbestand kan referenties voor meer dan één abonnement bevatten zoals beschreven in dit artikel.

Opmerking: deze referenties zijn anders dan de referenties die worden gebruikt voor verificatie van aanvragen naar Azure storage-services.

## <a name="create-a-subscription-file"></a>Een abonnementsbestand maken

In Server Explorer met de rechtermuisknop op de **Azure** uit en selecteer **beheren en Filter abonnementen**. Selecteer vervolgens de **certificaten** tabblad en voer een van de volgende acties:

- Selecteer **importeren** openen de **importeren Microsoft Azure-abonnementen** in het dialoogvenster. Selecteer de **downloaden abonnementsbestand** koppelen en opslaan in de browser het gedownloade bestand naar een tijdelijke locatie. Blader naar de downloadlocatie terug in het dialoogvenster en vervolgens importeren voor gebruik bij verificatie.
- Kies een actief abonnement en selecteer **bewerken**, die een dialoogvenster waarin u een bestaand abonnement voor gebruik bij verificatie bewerken wordt geopend.
- Selecteer **nieuw** openen de **nieuw abonnement** dialoogvenster vak en geef de vereiste gegevens. Het certificaat uploaden naar uw cloud service worden vermeld in het dialoogvenster Aanmelden bij de Azure-portal, gaat u naar de cloudservice, selecteer **instellingen > Beheercertificaten**, selecteer **uploaden**, klikt u vervolgens Geef het pad naar de `.cer` bestand.

Als u maken van een certificaat wilt, raadpleegt u de instructies in [maken en uploaden een beheer van het certificaat voor Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) en upload het certificaat moet handmatig de [Azure-portal](https://portal.azure.com/).

## <a name="next-steps"></a>Volgende stappen

- [Algemeen overzicht van Web-Apps](https://docs.microsoft.com/azure/app-service/)
- [Implementeer uw app in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-deploy-local-git) 
- [WebJobs implementeren met Visual Studio](https://docs.microsoft.com/azure/app-service/websites-dotnet-deploy-webjobs)
- [Maken en implementeren van een service in de cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)