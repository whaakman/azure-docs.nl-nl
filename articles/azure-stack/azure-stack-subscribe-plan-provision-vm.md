---
title: Abonneren op een aanbieding in Azure-Stack | Microsoft Docs
description: Abonnementen voor aanbiedingen maken in Azure-Stack
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 9b35b497c264fab2b8352eda82fe6d4e1fc274e9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Abonnementen op aanbiedingen maken in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Nadat u [maken van een aanbieding](azure-stack-create-offer.md), gebruikers moeten een abonnement op deze aanbieding voordat deze kan worden gebruikt.   
- Als een cloud-operator, kunt u een abonnement voor een gebruiker uit binnen de beheerdersportal.  Abonnementen die u maakt kunnen zijn voor openbare en persoonlijke aanbiedingen.
- Als een gebruiker tenant kunt u abonneren op een openbare aanbieding wanneer u de gebruikersportal.  

De volgende secties bevatten richtlijnen voor cloudoperators bij het maken van abonnementen voor gebruikers en hoe u zich abonneert op een aanbieding als een gebruiker.

## <a name="create-a-subscription-as-a-cloud-operator"></a>Een abonnement als een cloud-operator maken
Cloudoperators kunnen de-beheerportal gebruiken om een abonnement met een aanbieding voor een gebruiker te maken.  U kunt abonnementen voor leden van uw eigen directory-tenant maken.  Wanneer [multi-tenancymodus](azure-stack-enable-multitenancy.md) is ingeschakeld, u kunt ook abonnementen maken voor gebruikers in extra directory-tenants.

U kunt abonnementen voor openbare en persoonlijke aanbiedingen maken.  Als niet wilt dat uw tenants hun eigen abonnementen maken, al uw persoonlijke aanbiedingen aanbrengen en vervolgens abonnementen namens uw tenants maken. Deze aanpak is gebruikelijk bij Azure Stack integratie met externe facturerings- of -catalogus systemen.

Nadat u een abonnement voor een gebruiker hebt gemaakt, wordt die gebruiker zich kan aanmelden bij de gebruikersportal en vindt dat ze de aanbieding bent geabonneerd.  

### <a name="to-create-the-subscription-for-a-user"></a>Het abonnement voor een gebruiker maken
1.  Ga in de beheerportal naar **gebruikersabonnementen.**
2.  Selecteer **toevoegen** openen de **nieuwe gebruikerabonnement** deelvenster. Hier kunt u de volgende details opgeven:  

  a. **Weergavenaam** – een beschrijvende naam voor het identificeren van het abonnement dat wordt weergegeven als de *abonnement gebruikersnaam*.

  b. **Gebruiker** – Geef een gebruiker vanuit een beschikbare directory-tenant voor dit abonnement. De gebruikersnaam wordt weergegeven als *eigenaar*.  De indeling van de gebruikersnaam is afhankelijk van uw oplossing voor identiteit. Bijvoorbeeld:   

     -  **Azure AD:***&lt;gebruiker1 > @&lt;contoso.onmicrosoft.com >* 

     -   **AD FS:**  *&lt;user1>@&lt;azurestack.local>*     

  c.    **Directory-tenant** – Selecteer de directory-tenant waar het gebruikersaccount behoort. Of multi-tenancymodus niet is ingeschakeld, wordt alleen de tenant van uw lokale directory beschikbaar is.

3.  Selecteer **bieden** openen de **biedt** deelvenster en kies vervolgens een **bieden** voor dit abonnement. Omdat u een abonnement voor een gebruiker maakt, kunt u een persoonlijke of openbare aanbieding.

4.  Selecteer **maken** om het abonnement te maken. De **gebruikersabonnementen** deelvenster nu ziet u het nieuwe abonnement.  Wanneer de gebruiker zich bij de gebruikersportal aanmeldt, kunnen ze later meer informatie over dit abonnement bekijken.

### <a name="to-make-an-add-on-plan-available"></a>Om een plan invoegtoepassing beschikbaar maken  
Een cloud-operator kunt u een plan invoegtoepassing toevoegen aan een eerder gemaakte abonnement op elk gewenst moment:   
1.  Ga in de beheerportal naar **meer Services** > **gebruikersabonnementen**, en selecteer vervolgens het abonnement dat u wilt wijzigen.   

2.  Selecteer **invoegtoepassingen** > **toevoegen** openen de **toevoegen plan** deelvenster.  

3.  Selecteer het plan dat u wilt toevoegen als een invoegtoepassing.  De abonnementen die zijn gekoppeld aan het abonnement wordt met de console weergegeven.




## <a name="create-a-subscription-as-a-user"></a>Een abonnement als een gebruiker maken
Als een gebruiker, kunt u zich aanmelden bij de gebruikersportal om te zoeken en zich abonneren op openbare aanbiedingen en plannen van de invoegtoepassing van uw directory-tenant (organisatie). Wanneer de Stack van Azure-omgeving ondersteunt [multi-tenancymodus](azure-stack-enable-multitenancy.md) kunt u zich abonneren op voorstellen van een externe directory-tenant.

### <a name="to-subscribe-to-an-offer"></a>Abonneren op een aanbieding
1. [Meld u aan](azure-stack-connect-azure-stack.md) naar de gebruikersportal van Azure-Stack (https://portal.local.azurestack.external) en klik op **Neem een abonnement op**.

   ![Een abonnement nemen](media/azure-stack-subscribe-plan-provision-vm/image01.png)
2. In de **weergavenaam** veld, typ een naam voor uw abonnement, klik op **bieden**, klikt u op een van de aanbiedingen in de **kiest u een aanbieding** deelvenster en klik vervolgens op  **Maak**.

   ![Een aanbieding maken](media/azure-stack-subscribe-plan-provision-vm/image02.png)
3. Als u wilt weergeven van het abonnement dat u hebt gemaakt, klikt u op **meer services**, klikt u op **abonnementen**, klik vervolgens op het nieuwe abonnement.  

Nadat u zich op een aanbieding abonneert, vernieuw de portal om te zien welke services deel uitmaken van het nieuwe abonnement.

### <a name="to-subscribe-to-an-add-on-plan"></a>Abonneren op een plan voor de invoegtoepassing
Als een aanbieding een invoegtoepassing plan heeft, kunt u dat plan kunt toevoegen aan uw abonnement op elk gewenst moment.  

1. Selecteer in de gebruikersportal **meer services** > **abonnementen**, en selecteer vervolgens het abonnement dat u wijzigen wilt.

2. Selecteer **Plan toevoegen** knop en selecteer vervolgens de invoegtoepassing-plan dat u wilt. Als **toevoegen plan** is niet beschikbaar is, klikt u vervolgens er zijn geen plannen invoegtoepassing voor de aanbieding die zijn gekoppeld aan dat abonnement.



## <a name="next-steps"></a>Volgende stappen
[Een virtuele machine inrichten](azure-stack-provision-vm.md)
