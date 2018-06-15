---
title: Abonneren op een aanbieding in Azure-Stack | Microsoft Docs
description: Abonnementen voor aanbiedingen maken in Azure-Stack
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/11/2018
ms.author: brenduns
ms.openlocfilehash: 9153649774a67533649fb62da83a3f50abd592da
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295207"
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Abonnementen op aanbiedingen maken in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Nadat u [maken van een aanbieding](azure-stack-create-offer.md), gebruikers moeten een abonnement op deze aanbieding voordat deze kan worden gebruikt. Er zijn twee manieren dat gebruikers kunnen ophalen geabonneerd met een aanbieding:

- Als een cloud-operator, kunt u een abonnement voor een gebruiker uit binnen de beheerdersportal. Abonnementen die u maakt kunnen zijn voor openbare en persoonlijke aanbiedingen.
- Als een gebruiker tenant kunt u abonneren op een openbare aanbieding wanneer u de gebruikersportal.  

## <a name="create-a-subscription-as-a-cloud-operator"></a>Een abonnement als een cloud-operator maken

Cloudoperators kunnen de-beheerportal gebruiken om een abonnement met een aanbieding voor een gebruiker te maken.  U kunt abonnementen voor leden van uw eigen directory-tenant maken.  Wanneer [multi-tenancymodus](azure-stack-enable-multitenancy.md) is ingeschakeld, u kunt ook abonnementen maken voor gebruikers in extra directory-tenants.

Als niet wilt dat uw tenants hun eigen abonnementen maken en maak vervolgens de abonnementen voor uw tenants uw aanbiedingen privé maken. Deze aanpak is gebruikelijk bij Azure Stack integratie met externe facturerings- of -catalogus systemen.

Nadat u een abonnement voor een gebruiker hebt gemaakt, kunnen ze aanmelden bij de gebruikersportal en Zie dat ze bent geabonneerd op de aanbieding.  

### <a name="to-create-a-subscription-for-a-user"></a>Een abonnement voor een gebruiker maken

1. Ga in de beheerportal naar **gebruikersabonnementen.**
2. Selecteer **Toevoegen**. Onder **nieuwe gebruikerabonnement**, voer de volgende informatie:  

   - **Weergavenaam** – een beschrijvende naam voor het identificeren van het abonnement dat wordt weergegeven als de *abonnement gebruikersnaam*.
   - **Gebruiker** – Geef een gebruiker vanuit een beschikbare directory-tenant voor dit abonnement. De gebruikersnaam wordt weergegeven als *eigenaar*.  De indeling van de gebruikersnaam is afhankelijk van uw oplossing voor identiteit. Bijvoorbeeld:

     - **Azure AD:***&lt;gebruiker1 > @&lt;contoso.onmicrosoft.com >* 

     - **AD FS:***&lt;gebruiker1 > @&lt;azurestack.local >* 

   - **Directory-tenant** – Selecteer de directory-tenant waar het gebruikersaccount behoort. Als u multi-tenancymodus nog niet hebt ingeschakeld, is alleen de tenant van uw lokale directory beschikbaar.

3. Selecteer **bieden**. Onder **biedt**, kies een **bieden** voor dit abonnement. Omdat u het abonnement voor een gebruiker maakt, selecteert u **persoonlijke** als de status van de toegankelijkheid.

4. Selecteer **maken** om het abonnement te maken. Ziet u het nieuwe abonnement onder **gebruikerabonnement**. Wanneer de gebruiker zich aanmeldt bij de gebruikersportal zien ze de details van abonnement.

### <a name="to-make-an-add-on-plan-available"></a>Om een plan invoegtoepassing beschikbaar maken

Een cloud-operator kunt u een plan invoegtoepassing toevoegen aan een eerder gemaakte abonnement op elk gewenst moment:

1. Selecteer in het beheerportal **meer Services** > **gebruikersabonnementen**. Selecteer het abonnement dat u wilt wijzigen.

2. Selecteer **invoegtoepassingen** en selecteer vervolgens **+ toevoegen**.  

3. Onder **toevoegen plan**, het plan dat u wilt selecteren als een invoegtoepassing.

## <a name="create-a-subscription-as-a-user"></a>Een abonnement als een gebruiker maken

Als een gebruiker, kunt u zich aanmelden bij de gebruikersportal om te zoeken en zich abonneren op openbare aanbiedingen en plannen van de invoegtoepassing voor uw directory-tenant (organisatie).

>[!NOTE]
>Als uw Azure-Stack-omgeving ondersteunt [multi-tenancymodus](azure-stack-enable-multitenancy.md) u kunt zich ook abonneren op aanbiedingen van een externe directory-tenant.

### <a name="to-subscribe-to-an-offer"></a>Abonneren op een aanbieding

1. [Meld u aan](azure-stack-connect-azure-stack.md) bij de gebruikersportal van Azure-Stack (https://portal.local.azurestack.external) en selecteer **Neem een abonnement op**.

   ![Een abonnement nemen](media/azure-stack-subscribe-plan-provision-vm/image01.png)
  
2. Onder **Neem een abonnement op**, geef de beschrijvende naam van het abonnement in **weergavenaam**. Selecteer **bieden** en klikt u onder **kiest u een aanbieding**, kies een aanbieding. Selecteer **maken** om het abonnement te maken.

   ![Een aanbieding maken](media/azure-stack-subscribe-plan-provision-vm/image02.png)
  
3. Nadat u zich op een aanbieding abonneert, vernieuw de portal om te zien welke services deel uitmaken van het nieuwe abonnement.
4. Overzicht van het abonnement dat u hebt gemaakt, selecteert u **meer services** en selecteer vervolgens **abonnementen**. Selecteer het abonnement kunt u de details van abonnement.  

### <a name="to-subscribe-to-an-add-on-plan"></a>Abonneren op een plan voor de invoegtoepassing

Als een aanbieding een invoegtoepassing plan heeft, kunt u dat plan kunt toevoegen aan uw abonnement op elk gewenst moment.  

1. Selecteer in de gebruikersportal **meer services** > **abonnementen**, en selecteer vervolgens het abonnement dat u wijzigen wilt. Als er extra schema's beschikbaar zijn, **+ toevoegen plan** actief is en er is een tegel voor **invoegtoepassing plannen**.

   >[!NOTE]
   >Als **+ toevoegen plan** is niet actief is, wordt er geen invoegtoepassing plannen voor de aanbieding die zijn gekoppeld aan dat abonnement.

1. Selecteer **+ toevoegen plan** of de **invoegtoepassing plannen** tegel. Onder **invoegtoepassing plannen**, selecteer het plan dat u wilt toevoegen.

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine inrichten](azure-stack-provision-vm.md)
