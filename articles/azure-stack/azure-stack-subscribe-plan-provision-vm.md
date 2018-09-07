---
title: Abonneren op een aanbieding in Azure Stack | Microsoft Docs
description: Abonnementen voor aanbiedingen maken in Azure Stack
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
ms.date: 09/05/2018
ms.author: brenduns
ms.openlocfilehash: b35e75d7cfcaa46da46d2edcb80fe37c112a66a3
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44025568"
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Abonnementen voor aanbiedingen maken in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Nadat u [maken van een aanbieding](azure-stack-create-offer.md), gebruikers moeten een abonnement op de aanbieding voordat deze kan worden gebruikt. Er zijn twee manieren dat gebruikers kunnen u geabonneerd op een aanbieding:

- Als een cloud-operator, kunt u een abonnement voor een gebruiker uit binnen de beheerdersportal maken. U maakt-abonnementen kunnen worden voor zowel openbare als persoonlijke aanbiedingen.
- Als de gebruiker van een tenant, kunt u abonneren op een openbare aanbieding wanneer u de gebruikersportal.  

## <a name="create-a-subscription-as-a-cloud-operator"></a>Maken van een abonnement als een cloud-operator

Cloudoperators kunnen de beheerportal gebruiken om een abonnement op een aanbieding voor een gebruiker te maken.  U kunt abonnementen voor leden van uw eigen directory-tenant maken.  Wanneer [multitenancy](azure-stack-enable-multitenancy.md) is ingeschakeld, kunt u ook abonnementen maken voor gebruikers in de extra directory-tenants.

Als niet wilt dat uw tenants te maken van hun eigen abonnementen, privé maken van uw aanbiedingen en klikt u vervolgens abonnementen voor uw tenants te maken. Deze aanpak is gebruikelijk bij het integreren van Azure Stack met externe facturering of service catalogussystemen.

Nadat u een abonnement voor een gebruiker maakt, kunnen ze zich aanmelden bij de gebruikersportal en zien dat ze bent geabonneerd op de aanbieding.  

### <a name="to-create-a-subscription-for-a-user"></a>Een abonnement voor een gebruiker maken

1. In het beheerportal, gaat u naar **gebruikersabonnementen.**
2. Selecteer **Toevoegen**. Onder **nieuw gebruikersabonnement**, voer de volgende informatie:  

   - **Weergavenaam** : een beschrijvende naam voor het identificeren van het abonnement dat wordt weergegeven als de *naam van gebruikersabonnement*.
   - **Gebruiker** – Geef een gebruiker vanuit een beschikbare directory-tenant voor dit abonnement. Naam van de gebruiker wordt weergegeven als *eigenaar*.  De indeling van de naam van de gebruiker, is afhankelijk van uw oplossing voor identiteit. Bijvoorbeeld:

     - **Azure AD:** `<user1>@<contoso.onmicrosoft.com>`

     - **AD FS:** `<user1>@<azurestack.local>` 

   - **Directory-tenant** : Selecteer de directory-tenant waar het gebruikersaccount behoort. Als u meerdere tenants nog niet hebt ingeschakeld, vindt u alleen uw lokale directory-tenant.

3. Selecteer **bieden**. Onder **biedt**, kiest u een **bieden** voor dit abonnement. Omdat u het abonnement voor een gebruiker maakt, selecteert u **persoonlijke** als de status van de toegankelijkheid.

4. Selecteer **maken** om het abonnement te maken. Ziet u het nieuwe abonnement onder **gebruikersabonnement**. Wanneer de gebruiker zich aanmeldt bij de gebruikersportal aanmeldt, zien ze kunnen details van het abonnement.

### <a name="to-make-an-add-on-plan-available"></a>Een aanvullende plan om beschikbaar te maken

Een cloud-operator kunt u een aanvullende plan toevoegen aan een eerder gemaakte abonnement op elk gewenst moment:

1. Selecteer in de beheerportal **alle Services** en klik vervolgens onder de **BEHEERDERSRESOURCES** categorie, selecteer **gebruikersabonnementen**. Selecteer het abonnement dat u wilt wijzigen.

2. Selecteer **invoegtoepassingen** en selecteer vervolgens **+ toevoegen**.  

3. Onder **toevoegen plan**, selecteert u het gewenste abonnement als een invoegtoepassing.

## <a name="create-a-subscription-as-a-user"></a>Een abonnement als een gebruiker maken

Als een gebruiker, kunt u zich aanmelden bij de gebruikersportal aanmeldt om te zoeken en Abonneer u op openbare aanbiedingen en aanvullende plannen voor uw directory-tenant (organisatie).

>[!NOTE]
>Als uw Azure Stack-omgeving ondersteunt [multitenancy](azure-stack-enable-multitenancy.md) u kunt ook abonneren op aanbiedingen van een externe directory-tenant.

### <a name="to-subscribe-to-an-offer"></a>Om u te abonneren op een aanbieding

1. [Meld u aan](azure-stack-connect-azure-stack.md) bij de gebruikersportal van Azure Stack (https://portal.local.azurestack.external) en selecteer **Neem een abonnement**.

   ![Een abonnement nemen](media/azure-stack-subscribe-plan-provision-vm/image01.png)
  
2. Onder **Neem een abonnement**, geef de beschrijvende naam van het abonnement in **weergavenaam**. Selecteer **bieden** en klikt u onder **Kies een aanbieding**, kies een aanbieding. Selecteer **maken** om het abonnement te maken.

   ![Een aanbieding maken](media/azure-stack-subscribe-plan-provision-vm/image02.png)
  
3. Nadat u zich op een aanbieding abonneert, vernieuw de portal om te zien welke services deel uitmaken van het nieuwe abonnement.
4. Als u wilt zien van het abonnement dat u hebt gemaakt, selecteert u **alle services** en klik vervolgens onder de **algemene** categorie selecteren **abonnementen**. Selecteer het abonnement om de details van het abonnement te bekijken.  

### <a name="to-subscribe-to-an-add-on-plan"></a>Abonneren op een aanvullende-plan

Als een aanbieding een aanvullende plan heeft, kunt u het abonnement kunt toevoegen aan uw abonnement op elk gewenst moment.  

1. Selecteer in de gebruikersportal **alle services**. Vervolgens onder de **algemene** categorie selecteren **abonnementen**, en selecteer vervolgens het abonnement dat u wijzigen wilt. Als er aanvullende plannen beschikbaar, **+ toevoegen plan** actief is en er is een tegel voor **aanvullende plannen**.

   >[!NOTE]
   >Als **+ toevoegen plan** is niet actief is, wordt er geen eventuele aanvullende plannen voor de aanbieding die zijn gekoppeld aan dat abonnement.

1. Selecteer **+ toevoegen plan** of de **aanvullende plannen** tegel. Onder **aanvullende plannen**, selecteert u het abonnement dat u wilt toevoegen.

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine inrichten](azure-stack-provision-vm.md)
