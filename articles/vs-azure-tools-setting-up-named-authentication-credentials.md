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
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 953b1aa459ddf5b7be00b9d32432e6dda97143e1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-named-authentication-credentials"></a>Benoemde verificatiereferenties instellen
Voor het publiceren van een toepassing in Azure vanuit Visual Studio of een bestaande cloudservice bewaken, moet u de referenties die Visual Studio gebruiken kunt voor verificatie van aanvragen naar Azure opgeven. Er zijn verschillende plaatsen in Visual Studio, waar u zich aanmelden kunt in deze referenties op te geven. In Server Explorer kunt u bijvoorbeeld het snelmenu voor openen de **Azure** uit en selecteer **verbinding maken met Microsoft Azure-abonnement**. Wanneer u zich aanmeldt, wordt de informatie over het abonnement dat is gekoppeld aan uw Azure-account is beschikbaar in Visual Studio. Er is niets die meer u hoeft te doen.

Azure-hulpprogramma's biedt ook ondersteuning voor een oudere manier van het bieden van referenties: met behulp van de abonnementsbestand (.publishsettings-bestand). Deze methode wordt nog steeds ondersteund in de Azure SDK 2.2 beschreven.

De volgende items zijn vereist voor verificatie naar Azure:

* Uw abonnements-ID
* Een geldig X.509 v3-certificaat

> [!NOTE]
> De lengte van het X.509 v3-certificaat-sleutel moet ten minste 2048 bits. Azure weigert alle certificaten die niet voldoen aan deze eis of die is niet geldig.
>
>

Visual Studio gebruikt uw abonnements-ID samen met de gegevens van het certificaat als referenties. De juiste referenties wordt verwezen in het abonnementsbestand (.publishsettings-bestand) dat met een openbare sleutel voor het certificaat. Het abonnementsbestand kan referenties voor meer dan één abonnement bevatten.

Kunt u de informatie over het abonnement van de **nieuw abonnement** of **abonnement bewerken** in het dialoogvenster zoals verderop in dit artikel wordt uitgelegd.

Als u maken van een certificaat wilt, raadpleegt u de instructies in [maken en uploaden een beheer van het certificaat voor Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) en upload het certificaat moet handmatig de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!NOTE]
> Deze referenties die Visual Studio vereist voor het beheren van uw cloudservices zijn niet dezelfde referenties die nodig zijn voor het verifiëren van een aanvraag in voor de Azure storage-services.
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a>Importeren, instellen of bewerken van referenties voor verificatie in Visual Studio

1. Open in Server Explorer het snelmenu voor het **Azure** uit en selecteer **beheren en Filter abonnementen**.
2. Selecteer de **certificaten** tabblad en gebruik vervolgens een van de volgende methoden:

    * Selecteer **importeren** openen de **importeren Microsoft Azure-abonnementen** in het dialoogvenster. Daar kunt u het bestand abonnementen downloaden voor de momenteel geladen abonnement, bladert u naar de downloadlocatie en vervolgens importeren voor gebruik bij verificatie.
    * Selecteer **nieuw** openen de **nieuw abonnement** in het dialoogvenster. Daar kunt u een nieuw abonnement voor gebruik bij verificatie instellen.
    * Selecteer **bewerken** (nadat u uw actief abonnement hebt gekozen) om te openen de **abonnement bewerken** in het dialoogvenster. Daar kunt u een bestaand abonnement voor gebruik bij verificatie bewerken. 

De volgende procedure wordt ervan uitgegaan dat de **nieuw abonnement** dialoogvenster is geopend.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Voor het instellen van referenties voor verificatie in Visual Studio
1. In de **Selecteer een bestaand certificaat voor verificatie** kiest u een certificaat.
2. Selecteer de **kopiëren van het volledige pad** koppeling. Het pad voor het certificaat (.cer-bestand) is naar het Klembord gekopieerd.

   > [!IMPORTANT]
   > U moet dit certificaat uploaden voor het publiceren van uw Azure-toepassing vanuit Visual Studio de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   >
   >
3. Het certificaat uploaden naar de Azure-portal:

   a. Open de [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   
   b. Als u wordt gevraagd, meld u aan bij de portal en blader vervolgens naar **instellingen** > **Beheercertificaten**.
   
   c. In de **beheercertificaten** deelvenster **uploaden**.
   
   d. Selecteer uw Azure-abonnement, plakt u het volledige pad van het cer-bestand dat u zojuist hebt gemaakt en selecteer **uploaden**.

## <a name="next-steps"></a>Volgende stappen
* [Algemeen overzicht van Web-Apps](https://docs.microsoft.com/azure/app-service/)
* [Implementeer uw app in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-deploy-local-git) 
* [WebJobs implementeren met Visual Studio](https://docs.microsoft.com/en-us/azure/app-service/websites-dotnet-deploy-webjobs)
* [Maken en implementeren van een service in de cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)