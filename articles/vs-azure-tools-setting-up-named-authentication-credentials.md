---
title: Referenties voor verificatie met de naam instellen | Microsoft Docs
description: Leer hoe u referenties die Visual Studio gebruiken kunt voor het verifiëren van aanvragen naar Azure, zodat u kunt een toepassing in Azure vanuit Visual Studio publiceren of een bestaande cloudservice controleren op te geven.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 61570907-42a1-40e8-bcd6-952b21a55786
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: 48c410df1768c2cae7807aa1538d76867a581c71
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42055740"
---
# <a name="set-up-named-authentication-credentials"></a>Instellen met de naam van de referenties voor verificatie

Voor het publiceren van een toepassing naar Azure of voor het bewaken van een bestaande cloudservice moet Visual Studio referenties voor het verifiëren van aanvragen naar Azure, namelijk uw Azure-abonnement-ID en een geldig X.509 v3-certificaat met een sleutel van ten minste 2048 bits. U opgeven dat deze referenties via een van de volgende methoden:

- Klik in Visual Studio, selecteer **weergave > Server Explorer**, met de rechtermuisknop op de **Azure** knooppunt uit, selecteer **verbinding maken met Microsoft Azure-abonnement**, en meld u aan.
- Maak een abonnementsbestand (`.publishsettings`), die een openbare sleutel voor het certificaat bevat. De abonnementsbestand kan referenties voor meer dan één abonnement bevatten, zoals beschreven in dit artikel.

Opmerking: deze referenties zijn anders dan de referenties die worden gebruikt voor het verifiëren van aanvragen voor Azure storage-services.

## <a name="create-a-subscription-file"></a>Maak een abonnementsbestand

In Server Explorer met de rechtermuisknop op de **Azure** knooppunt en selecteert u **beheren en Filter abonnementen**. Selecteer vervolgens de **certificaten** tabblad en voert u een van de volgende acties:

- Selecteer **importeren** openen de **importeren van Microsoft Azure-abonnementen** in het dialoogvenster. Selecteer de **abonnement downloadbestand** koppelen en opslaan in de browser het gedownloade bestand naar een tijdelijke locatie. Terug in het dialoogvenster, blader naar de downloadlocatie en vervolgens importeren voor gebruik bij verificatie.
- Kies een actief abonnement en selecteer **bewerken**, die een dialoogvenster waarin u een bestaand abonnement voor gebruik bij verificatie bewerken wordt geopend.
- Selecteer **nieuw** openen de **nieuw abonnement** dialoogvenster vak en geef de vereiste gegevens. Het certificaat uploaden naar uw cloud service worden vermeld in het dialoogvenster, meld u aan bij de Azure-portal, gaat u naar uw cloudservice en selecteer **instellingen > Beheercertificaten**, selecteer **uploaden**, klikt u vervolgens Geef het pad op naar de `.cer` bestand.

Als u maken van een certificaat wilt, kunt u verwijzen naar de instructies in [maken en uploaden van een management certificate for Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) en upload het certificaat handmatig de [Azure-portal](https://portal.azure.com/).

## <a name="next-steps"></a>Volgende stappen

- [Algemeen overzicht van Web-Apps](https://docs.microsoft.com/azure/app-service/)
- [Uw app implementeren in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-deploy-local-git) 
- [WebJobs implementeren met Visual Studio](https://docs.microsoft.com/azure/app-service/websites-dotnet-deploy-webjobs)
- [Maken en implementeren van een service in de cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)