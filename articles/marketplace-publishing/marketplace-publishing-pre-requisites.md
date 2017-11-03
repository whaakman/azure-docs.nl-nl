---
title: Niet-technische vereisten voor het maken van een aanbieding voor Azure Marketplace | Microsoft Docs
description: Begrip van de vereisten voor het maken en implementeren van een aanbieding voor Azure Marketplace voor anderen om aan te schaffen.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio
ms.openlocfilehash: 4f86d444a2f2b97fd8605d480db358813bc39fd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Algemene vereisten voor het maken van een aanbieding voor Azure Marketplace
Inzicht in het algemeen, zakelijke proces gericht vereisten die nodig zijn om te gaan met een proces voor het maken van de aanbieding.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Zorg ervoor dat u bent geregistreerd als een verkoper met Microsoft
Voor gedetailleerde instructies over het registreren van een account met de verkoper met Microsoft, gaat u naar [accountaanmaking en registratie](marketplace-publishing-accounts-creation-registration.md).

* **Als uw bedrijf is al geregistreerd als een verkoper in het Dev Center en u wilt maken van een nieuwe aanbieding** en meld u aan bij het publiceren met dezelfde e-mail-id met welk ontwikkelaarscentrum registratie is uitgevoerd. Deze stap is vereist zodat de portal Dev Center en publiceren met elkaar zijn gekoppeld.
* **Als uw bedrijf is al geregistreerd als een verkoper in het Dev Center en u wilt bewerken van een bestaande aanbieding** vervolgens ofwel aanmelding bij het publiceren portal met de admin-account of met een account dat wordt toegevoegd als een co-beheerder bij het publiceren portal. Stappen voor het toevoegen van een co-beheerdersaccount is hieronder.

## <a name="steps-to-add-a-co-admin-in-the-publishing-portal"></a>Stappen voor het toevoegen van een co-beheerder in de Portal publiceren
Beheerders van de publicatie portal de andere leden van het bedrijf die aan de toepassing werkt, kunt toevoegen als een co-beheerder bij het publiceren portal. **Ervan uitgaande dat u de beheerder bent** hieronder worden de stappen voor het toevoegen van een co-beheerder.

> [!NOTE]
> Voor nieuwe gebruikers voordat u toevoegen co-beheerder bij het publiceren portal, zorg ervoor dat u ten minste één toepassing hebt gemaakt in de publicatie van de portal. Dit is vereist als de **UITGEVERS** tabblad weergegeven nadat u ten minste één toepassing bij het publiceren portal.
> 
> 

1. Zorg ervoor dat de co-beheerder e-id een Microsoft-account(MSA) is. Als dat niet registreren als een MSA gebruik van deze [koppeling](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Zorg ervoor dat er ten minste één toepassing onder de beheeraccount voordat u probeert toe te voegen een co-beheerder.
3. Nadat de bovenstaande stappen zijn gedaan, meld u aan bij het publiceren met de id van co-beheerder e- en afmelden.
4. Nu aanmelden bij het publiceren met de beheerder e-mail-id.
5. Navigeer naar uitgevers -> uw account -> Selecteer beheerders -> toevoegen de co-beheerder (schermafbeelding hieronder)
   
    ![tekenen](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Zorg ervoor dat e-id's opgegeven in de verschillende stadia van het publicatieproces (bijvoorbeeld Dev Center, portal publiceren) worden gecontroleerd op communicatie van Microsoft.
7. Voor registratie van de Dev Center, worden voorkomen met een account die is gekoppeld aan één persoon. Dit wordt voorgesteld voor het verwijderen van afhankelijkheid van een persoon.
8. Als u eventuele problemen met de registratie van de Dev Center geconfronteerd, vervolgens Verhoog een ticket gebruik van deze [koppeling](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-co-admin-in-the-publishing-portal"></a>Stappen voor het verwijderen van een co-beheerder bij het publiceren portal
**Ervan uitgaande dat u de beheerder bent** hieronder worden de stappen voor het verwijderen van een co-beheerder.

1. Meld u aan bij het publiceren met de beheerder e-mail-id.
2. Navigeer naar **uitgevers** -> uw account -> Selecteer **beheerders** -> **Co-beheerders**.
3. Klik op de **X** knop naast de co-beheerder die u wilt dat tot verwijderen (schermafbeelding hieronder).
   
    ![tekenen](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> U hebt niet de btw en bank bedrijfsgegevens voltooien als u van plan bent alleen gratis aanbiedingen publiceren (of breng uw eigen licentie).
> 
> De bedrijfsregistratie moet worden uitgevoerd om te beginnen. Echter, terwijl uw bedrijf op de belasting en bank informatie in het Microsoft Developer-account werkt, de ontwikkelaars kunnen gaan werken over het maken van de installatiekopie van de virtuele machine in de [Publishing Portal](https://publish.windowsazure.com), om deze gecertificeerd en testen van het in de Azure-testomgeving. U moet de goedkeuring van de account volledige verkoper alleen voor de laatste stap van uw aanbieding publiceren naar Azure Marketplace.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Een 'betalen naar gebruik' Azure-abonnement aanschaffen
Dit is het abonnement dat u gebruikt voor het maken van uw VM-installatiekopieën en via de afbeeldingen aan de hand de [Azure Marketplace](https://azure.microsoft.com/marketplace/). Als u een bestaand abonnement niet hebt, vervolgens moet zich aanmelden op https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>'Verkopen van' landen
> [!WARNING]
> Om uw services op Azure Marketplace verkopen, moet u ervoor zorgen dat uw geregistreerde entiteit afkomstig van een van de landen goedgekeurde 'verkopen-van is'. Deze beperking is voor toekenning en belasting redenen. We actief op zoek naar deze lijst met landen in de nabije toekomst uitgebreid, dus Bezoek ons regelmatig. Zie voor de volledige lijst 1b sectie van de [Azure Marketplace deelname beleid](http://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zodra de niet-technische vereisten is voldaan, vervolgens zijn de aanbieding specifieke technische vereisten. Klik op de koppeling naar het artikel voor het aanbiedingstype respectieve die u wilt maken voor Azure Marketplace.

* [De technische vereisten VM](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Oplossing sjabloon technische vereisten](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Zie ook
* [Aan de slag: hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)

