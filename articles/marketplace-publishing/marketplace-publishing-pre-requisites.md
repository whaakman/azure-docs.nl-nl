---
title: Niet-technische vereisten voor het maken van een aanbieding voor Azure Marketplace | Microsoft Docs
description: Informatie over de vereisten voor het maken en implementeren van een aanbieding op Azure Marketplace voor anderen om aan te schaffen.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: 4b925522186d2d9ae537431c1d96d39b107ad967
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073166"
---
# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Algemene vereisten voor het maken van een aanbieding voor Azure Marketplace
Krijg inzicht in het algemeen, business-proces-georiënteerde vereisten die nodig zijn om te gaan met het proces voor het maken van aanbieding.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Zorg ervoor dat u bent geregistreerd als een verkoper met Microsoft
Voor gedetailleerde instructies over het registreren van een verkopersaccount met Microsoft, gaat u naar [van accountaanmaking en registratie](marketplace-publishing-accounts-creation-registration.md).

* Als uw bedrijf is al geregistreerd als een verkoper in het Dev Center en u wilt maken van een nieuwe aanbieding, meld u vervolgens op de publicatie met de dezelfde e-mail-ID met welke Dev Center registratie wordt uitgevoerd. Deze stap is vereist zodat het Dev Center en publiceren-portal met elkaar zijn gekoppeld.
* Als uw bedrijf is al geregistreerd als een verkoper in het Dev Center en u wilt bewerken van een bestaande aanbieding, wordt een Meld u aan bij de publicatie portal met de Administrator-account of met een account dat wordt toegevoegd als een coadmin in de publicatie portal. Stappen voor het toevoegen van een account coadmin hieronder.

## <a name="steps-to-add-a-coadmin-in-the-publishing-portal"></a>Stappen voor het toevoegen van een coadmin in de Portal voor publiceren
Beheerders van de publicatie portal de andere leden van het bedrijf die aan de toepassing werkt, kunt toevoegen als een coadmin in de publicatie portal. **Ervan uitgaande dat u de beheerder bent** hieronder worden de stappen voor het toevoegen van een coadmin.

> [!NOTE]
> Voor nieuwe gebruikers, voordat u toevoegen een co-beheerder in de publicatie-portal, zorg ervoor dat u ten minste één toepassing hebt gemaakt in de publicatie portal. Dit is vereist als de **UITGEVERS** tabblad worden weergegeven na het maken van ten minste één toepassing in de publicatie portal.
> 
> 

1. Zorg ervoor dat de coadmin e-mail-ID een Microsoft-account(MSA) is. Als dit niet het geval is, registreert u dit als een MSA met behulp van dit [koppeling](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Zorg ervoor dat er ten minste één toepassing onder de admin-account voordat u probeert toe te voegen een coadmin.
3. Nadat de bovenstaande stappen worden uitgevoerd, zich in de publicatie-portal met de coadmin e-mail-ID en vervolgens meldt u zich af.
4. Nu aanmelden met het publiceren met de beheerder e-ID.
5. Navigeer naar uitgevers -> Selecteer uw account -> beheerders -> toevoegen de coadmin (schermafbeelding hieronder genoemd)
   
    ![tekenen](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Zorg ervoor dat de e-mail-id's opgegeven in de verschillende stadia van het publicatieproces (bijvoorbeeld Dev Center, portal voor publiceren) voor alle communicatie van Microsoft worden bewaakt.
7. Voor registratie van Dev Center, te voorkomen dat met een account dat is gekoppeld aan één persoon. Deze suggesties verwijdert de afhankelijkheid van een enkele persoon.
8. Als u problemen ondervindt met de registratie van het Dev Center, klikt u vervolgens een ticket met behulp van dit verhogen [koppeling](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-coadmin-in-the-publishing-portal"></a>Stappen voor het verwijderen van een coadmin in de publicatie portal
**Ervan uitgaande dat u de beheerder bent** hieronder worden de stappen voor het verwijderen van een coadmin.

1. Meld u aan bij het publiceren met de beheerder e-ID.
2. Navigeer naar **uitgevers** -> Selecteer uw account -> **beheerders** -> **Co-beheerders**.
3. Klik op de **X** knop naast de coadmin die u wilt dat tot verwijderen (schermafbeelding hieronder).
   
    ![tekenen](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> U hoeft niet te belasting en bank bedrijfsgegevens te voltooien als u van plan bent alleen gratis aanbiedingen publiceren (of hun eigen licentie mee).
> 
> De bedrijfsregistratie moet worden uitgevoerd om te beginnen. Echter, terwijl uw bedrijf op de belasting en bank informatie in het Microsoft Developer-account werkt, de ontwikkelaars kunnen gaan over het maken van de installatiekopie van de virtuele machine in de [Portal voor publiceren](https://publish.windowsazure.com), aan het gecertificeerd en testen het is in de faseringsomgeving van Azure. De goedkeuring van de account volledige verkoper moet u alleen voor de laatste stap van de publicatie van uw aanbieding op Azure Marketplace.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Een 'betalen per gebruik' Azure-abonnement aanschaffen
Deze waarde is het abonnement die u gebruiken wilt voor het maken van uw VM-installatiekopieën en over de afbeeldingen aan de hand de [Azure Marketplace](https://azure.microsoft.com/marketplace/). Als u een bestaand abonnement hebt, klikt u vervolgens aanmelden bij https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>'Verkopen van'-landen
> [!WARNING]
> Als u wilt uw services op Azure Marketplace verkopen, moet u ervoor zorgen dat uw geregistreerde entiteit van een van de goedgekeurde 'verkoop-van'-landen is. Deze beperking is omwille van de betalings- en belasting. We zijn actief op zoek naar deze lijst van landen in de nabije toekomst uitvouwen, dus kom. Zie voor de volledige lijst 1b sectie van de [deelnamebeleid voor Azure Marketplace](https://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Volgende stappen
Als de niet-technische vereisten is voldaan, bent u naast de aanbieding-specifieke technische vereisten. Klik op de koppeling naar het artikel voor de betreffende aanbiedingtype dat u wilt maken voor de Azure Marketplace.

* [Technische vereisten voor virtuele machine](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Oplossing sjabloon technische vereisten](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Zie ook
* [Aan de slag: Een aanbieding publiceren op Azure Marketplace](marketplace-publishing-getting-started.md)

