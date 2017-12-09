---
title: Een Azure-abonnement of management group - vinden Azure | Microsoft Docs
description: Navigeren tussen meerdere mappen voor een overzicht van de beheergroepen en abonnementen
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: f1b9c1ec2af8240ff71f6907516d8894c36ac9c3
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="find-an-azure-subscription-or-management-group"></a>Een Azure-abonnement of management-groep vinden

Als u problemen bij het zoeken van een abonnement of de beheergroep in Azure ondervindt, kijkt u mogelijk in de verkeerde directory. Deze situatie kan optreden wanneer uw account in meerdere Azure Active Directory's bestaat. Elke [active directory is onafhankelijk](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-directory-independence) en toegang wordt niet overgenomen op mappen.      

![Switch Directory Menu](media/billing-enterprise-mgmt-groups/mgempty.png)



## <a name="switch-directories"></a>Switch-mappen 
U kunt eenvoudig mappen overschakelen in de Azure portal.
1.  Meld u aan bij de [Azure Portal](https://portal.azure.com).
2.  Selecteer uw naam in de rechterbovenhoek van het scherm. 
3.  De onderkant van het menu geeft een lijst met alle mappen die u toegang tot hebt.
4.  Selecteer de naam van een map voor toegang tot. 

![Switch Directory Menu](media/billing-enterprise-mgmt-groups/switch-directory.png)

## <a name="asset-is-unavailable"></a>Asset is niet beschikbaar? 
Als u een foutbericht ontvangt 'dit activum is niet beschikbaar' wanneer een abonnement of management groep, dan hebt u toegang wilt tot hoeft niet de juiste rol voor dit item weergeven.  

![Asset niet is gevonden](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Neem contact op met het beheren van het abonnement of management groepen om toegang te krijgen.  
* Voor abonnementen, verwijzen naar [gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) document voor help waarop de rol is vereist.
* Voor-beheergroepen RBAC toegang is niet beschikbaar en is binnenkort beschikbaar. Neem contact op met uw enterprise portal beheren voor toegang is toegewezen.   

## <a name="improve-your-experience-with-management-groups-and-subscriptions-in-the-same-directory"></a>Om uw ervaring te verbeteren met beheergroepen en abonnementen in dezelfde map 
U kunt uw abonnementen of uw beheergroepen overbrengen naar de map die u kiest, zodat alles op dezelfde plaats bestaat.  Consolidatie van abonnementen en beheergroepen in dezelfde map vermindert de noodzaak overschakelen van mappen en toestaan dat het beleid om te worden overgenomen.  


### <a name="transfer-your-subscriptions"></a>Overdracht van uw abonnementen 
U kunt een abonnement verplaatsen naar de map die is gekoppeld aan de beheergroepen. Verplaatsing kan worden bereikt door de beheerder van uw inschrijving overdracht van uw abonnement met een account in de doelmap. Voor meer informatie, moet u zich aanmelden bij de [Enterprise portal](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription).


 






