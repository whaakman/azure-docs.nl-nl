---
title: 'Account migratie van Cloud Partner-portal naar partner Center: commerciële Marketplace voor Azure'
description: Uw account migreren van CPP naar partner Center. -Commerciële Marketplace voor Azure
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 08/15/2019
ms.openlocfilehash: 9ab9a57641e6b34942ae6d4293311714177aa012
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533188"
---
# <a name="account-migration-from-cloud-partner-portal-to-partner-center"></a>Account migratie van Cloud Partner-portal naar het partner centrum

Als uw aanbiedingen worden gemigreerd van de Cloud Partner-portal (CPP) naar het partner centrum (PC), worden ze vergrendeld voor bewerking in CPP. Op dit moment moeten uw account instellingen worden gemigreerd naar het partner centrum.  Zowel uw account instellingen als uw aanbiedingen kunnen worden beheerd in het partner centrum.

## <a name="account-migration-process"></a>Account migratie proces

Wanneer aanbiedingen worden gemigreerd vanuit het CPP, wordt uw account geconfigureerd voor migratie. 
 
Als u een gebruiker bent met de rol owner in CPP voor een bepaald account, wordt een gele banner weer gegeven op de Publisher-profiel pagina.  U wordt gevraagd om uw account instellingen naar het partner centrum te verplaatsen. 

Klik op de banner om uw account migratie proces te initiëren. U wordt verwacht de volgende items in te voeren:

### <a name="work-e-mail-address"></a>**E-mail adres van werk**

In de meeste gevallen meldt u zich aan met het e-mail adres dat u gebruikt om u aan te melden bij CPP. In bepaalde gevallen moet een ander e-mail adres worden gebruikt:

* Micro soft-account: Als het CPP-account een micro soft-account is, moet u een geldig zakelijk e-mail adres invoeren dat is gekoppeld aan de Tenant, voor wie de MPN-ID is geregistreerd. 

* Niet-overeenkomende tenants: Als uw zakelijke e-mail adres niet hoort bij de Tenant die is gekoppeld aan de Microsoft Partner Network-ID die is opgegeven in uw CPP-account, wordt er een fout weer gegeven. Als u deze fout wilt verplaatsen, voert u een e-mail adres in dat is gekoppeld aan de Tenant. Er wordt een fout bericht weer gegeven met de naam van de Tenant. 

### <a name="sign-up-for-microsoft-partner-network-program"></a>Registreren voor Microsoft Partner Network programma

In het geval dat uw CPP-account geen Microsoft Partner Network-ID heeft of een certificaat heeft dat ongeldig is, moet u zich registreren voor het Microsoft Partner Network-programma als onderdeel van het activerings proces.

## <a name="account-activation-is-complete"></a>Account activering is voltooid

De account migratie moet slechts één keer plaatsvinden voor een bepaald account. Zodra een gegeven partner de migratie voor het account heeft voltooid, zien alle eigen aren dit gedrag op hun Publisher-profiel pagina:

1. U ziet de pagina partner instellingen in Microsoft Partner Network, waar u de account instellingen van de micro soft-partner kunt beheren. 
2. Zodra de account migratie is voltooid, wordt een gele banner op uw Publisher-profiel pagina weer gegeven aan gebruikers die deel uitmaken van de rol owner in CPP voor een bepaald account, om hen te vragen hun account instellingen te beheren in het partner centrum. 
3. De pagina account instellingen in CPP wordt vervolgens geconverteerd naar de modus alleen-lezen. 

## <a name="move-dynamics-365-based-solutions-to-partner-center"></a>Op Dynamics 365 gebaseerde oplossingen verplaatsen naar het partner centrum

Als u beschikt over Dynamics 365 voor klant betrokkenheid of Dynamics 365 voor Financiën en oplossingen in de GTM portal van een commerciële partner, **volgt u deze instructies op 31 augustus 2019** om deze oplossingen te verplaatsen naar het partner centrum.

> [!NOTE]
> Als uw account oorspronkelijk is gemaakt in het Partner Membership Center (PMC), meldt u zich aan bij het [partner centrum](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile) om te bevestigen dat uw account is gemigreerd voordat u de onderstaande stappen hebt voltooid. Als er een profiel scherm met uw MPN-ID wordt weer gegeven, bent u klaar om door te gaan. Als dat niet het geval is, moet u uw account migratie starten door de prompts in het [Partner Membership Center](https://partners.microsoft.com/partnerprogram/Welcome.aspx)te volgen. Als u hulp nodig hebt, gaat u naar [ondersteuning](https://partner.microsoft.com/support?issueid=100-0077).

1. Ga naar de [overzichts pagina voor commerciële Marketplace in partner centrum](https://partner.microsoft.com/dashboard/commercial-marketplace/overview). Als u ' commerciële Marketplace ' ziet in het navigatie deel venster aan de linkerkant, bent u Inge schreven en gaat u verder met de volgende stap. Als dat niet het geval is, moet u zich nu [inschrijven in de commerciële Marketplace](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/azureisv) .
2. Bevestig dat uw aanbiedingen zich in AppSource bevinden door te [zoeken naar uw aanbiedingen](https://appsource.microsoft.com/). Als uw aanbiedingen al AppSource zijn, gaat u verder met de volgende stap. Voor elke aanbieding die niet in AppSource is, maakt u een [nieuwe dynamics 365 voor klant engagement](create-new-customer-engagement-offer.md) of een [nieuwe aanbieding voor Dynamics 365 voor bewerkingen](create-new-operations-offer.md).
3. Controleer uw inschrijving in het Business Applications ISV Connect-programma:
  
   * Controleer op de pagina [overeenkomsten](https://partner.microsoft.com/dashboard/account/agreements) in partner centrum of u de **Business Applications ISV-addendum** hebt geaccepteerd om in het programma te registreren.
   * Geef uw facturerings gegevens op de pagina [account instellingen](https://partner.microsoft.com/dashboard/account/v3/accountsettings/billingprofile) op.

4. Verzend elke nieuwe en bestaande aanbieding voor certificering, zelfs als uw aanbiedingen eerder zijn gecertificeerd. **U wordt aangeraden zo snel mogelijk een aanvraag in te dienen om voldoende tijd te bieden voor goed keuring vóór 31 augustus 2019.**
5. Ga naar het [gtm-portal van de ene commerciële partner](https://msgtm.azurewebsites.net/en-US/Profile/SignIn) en voeg de URL van uw AppSource-vermelding toe in het gedeelte Marketplace-koppelingen. Als u hulp nodig hebt bij deze stap, kunt u cosell@microsoft.comons een e-mail sturen naar.

## <a name="next-steps"></a>Volgende stappen

- [Uw commerciële Marketplace-account beheren in het partner centrum](./manage-account.md) 
