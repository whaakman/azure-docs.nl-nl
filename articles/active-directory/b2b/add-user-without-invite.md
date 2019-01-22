---
title: Gebruikers van B2B-samenwerking met Azure Active Directory toevoegen zonder uitnodiging | Microsoft Docs
description: U kunt een gastgebruiker andere gastgebruikers toevoegen aan uw Azure AD zonder het inwisselen van een uitnodiging in Azure Active Directory B2B-samenwerking.
services: active-directory
documentationcenter: ''
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 7ddd5c0195bdd59deac453fe324b6d8c6898ec2e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434203"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>B2B-samenwerking gastgebruikers toevoegen zonder uitnodiging

U kunt nu gastgebruikers uitnodigen door het verzenden van een directe koppeling naar een gedeelde app. Met deze methode wilt gastgebruikers ook kunnen niet meer gebruiken de uitnodiging per e-mail, behalve in sommige gevallen speciale. Een gastgebruiker klikt op de app-koppeling, beoordeelt en accepteert de voorwaarden van de privacyverklaring en vervolgens naadloos toegang heeft tot de app. Zie voor meer informatie, [B2B-samenwerking uitnodiging inwisselen](redemption-experience.md).   

Voordat deze nieuwe methode beschikbaar is, kan u gastgebruikers uitnodigen zonder de uitnodiging per e-mail een uitnodiging antwoorden (van uw organisatie of van een andere organisatie) toevoegt aan de **gastuitnodiging** directory-rol, en vervolgens met de afzender van de uitnodiging gastgebruikers toevoegen aan de directory, groepen of toepassingen via de gebruikersinterface of via PowerShell. (Als u PowerShell gebruikt, u kunt onderdrukken de uitnodiging per e-mail kan worden overgeslagen). Bijvoorbeeld:

1. Een gebruiker in de organisatie van de host (bijvoorbeeld WoodGrove) nodigt één gebruiker uit de andere organisatie (bijvoorbeeld Sam@litware.com) als gast.
2. De beheerder in de organisatie van de host [beleid heeft ingesteld](delegate-invitations.md) waarmee Sam om te bepalen en andere gebruikers van de partnerorganisatie (Litware) toevoegen. (Sam moet worden toegevoegd aan de **gastuitnodiging** rol.)
3. Nu kunt Sam toevoegen andere gebruikers van Litware aan de directory WoodGrove, groepen of toepassingen zonder uitnodigingen na ontvangst worden ingewisseld. Als de juiste opsomming Sam in Litware beschikt, wordt dit automatisch gebeurt.
 
Deze oorspronkelijke methode werkt nog steeds. Er is echter een klein verschil in gedrag. Als u PowerShell gebruikt, zult u merken dat een uitgenodigde Gast-account nu heeft een **PendingAcceptance** status in plaats van direct **geaccepteerde**. Hoewel de status in behandeling is, kunt nog steeds de gastgebruiker zich aanmelden en toegang tot de app zonder te klikken op de koppeling van een e-uitnodiging. De status in behandeling betekent dat de gebruiker heeft geen nog doorlopen de [toestemming geven ervaring](redemption-experience.md#privacy-policy-agreement), waar ze de voorwaarden van de privacyverklaring van de uitnodigende organisatie accepteren. De gastgebruiker ziet dit toestemmingsscherm wanneer ze zich aanmelden voor de eerste keer. 

Als u een gebruiker naar de map uitnodigen, de gastgebruiker moet toegang tot de Azure portal voor resource-tenantspecifieke URL rechtstreeks (zoals https://portal.azure.com/ *resourcetenant*. onmicrosoft.com) om te bekijken en ga akkoord met de voorwaarden van de privacyverklaring.

### <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [B2B-samenwerking uitnodiging inwisselen](redemption-experience.md)
- [Uitnodigingen delegeren voor B2B-samenwerking in Azure Active Directory](delegate-invitations.md)
- [Hoe voeg informatiemedewerkers gebruikers van B2B-samenwerking toe?](add-users-information-worker.md)

