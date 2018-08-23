---
title: Hoe het configureren van hybride Azure Active Directory apparaten gekoppelde | Microsoft Docs
description: Informatie over het configureren van hybride Azure Active Directory verbonden apparaten.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: fbc40d0768a7cf1cb83a3e78dd524fac26254bea
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058729"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Hoe u uw hybride Azure Active Directory join-implementatie plannen

Een apparaat wordt op een soortgelijke manier aan een gebruiker een andere identiteit die u wilt beveiligen en ook gebruiken voor het beveiligen van uw resources op elk moment en de locatie. U kunt dit doel te bereiken door uw apparaten met identiteiten met Azure AD met behulp van een van de volgende methoden:

- Azure AD join
- Hybrid Azure AD Join
- Azure AD-registratie

Door uw apparaten meebrengen naar Azure AD, kunt u uw gebruikers productiviteit door middel van eenmalige aanmelding (SSO) in uw cloud en on-premises resources maximaliseren. Op hetzelfde moment, kunt u veilige toegang tot uw cloud en on-premises resources met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md).

Als u een on-premises Active Directory-omgeving hebt en u wilt deelnemen aan uw domein apparaten naar Azure AD, kunt u dit doen met hybride Azure AD gekoppelde apparaten configureren. Dit artikel vindt u met de bijbehorende stappen voor het implementeren van een hybride Azure AD join in uw omgeving. 


## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent de [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md).


## <a name="plan-your-implementation"></a>Uw implementatie plannen

Als u wilt uw hybride Azure AD-implementatie te plannen, moet u vertrouwd raken met:

|   |   |
|---|---|
|![Selecteren][1]|Apparaten controleren die worden ondersteund|
|![Selecteren][1]|Lees wat die u moet weten|
|![Selecteren][1]|Selecteer uw scenario|


 


## <a name="review-supported-devices"></a>Apparaten controleren die worden ondersteund 

Hybride Azure AD join ondersteunt een breed bereik van de Windows-apparaten. Omdat de configuratie voor apparaten met oudere versies van Windows voor aanvullende of andere stappen vereist, worden de ondersteunde apparaten gegroepeerd in twee categorieën:

**Huidige Windows-apparaten**

- Windows 10
    
- Windows Server 2016


Voor apparaten met het besturingssysteem voor Windows-bureaublad, de ondersteunde versie is de Windows 10 Jubileumupdate (versie 1607) of hoger. Als een best practice, een upgrade uitvoeren naar de nieuwste versie van Windows 10.



 **Windows downlevel-apparaten**

- Windows 8.1
 
- Windows 7

- Windows Server 2012 R2
 
- Windows Server 2012 
 
- Windows Server 2008 R2 


U moet als eerste stap planning uw omgeving controleren en bepalen of u nodig hebt om Windows downlevel-apparaten te ondersteunen.



## <a name="review-things-you-should-know"></a>Lees wat die u moet weten

U kunt een hybride Azure AD join niet gebruiken als uw omgeving bestaat uit één forest die identiteitsgegevens in meer dan één Azure AD-tenant gesynchroniseerd.

Als u het hulpprogramma voor systeemvoorbereiding (Sysprep) zijn afhankelijk, zorg er dan voor dat u installatiekopieën maken van een installatie van Windows die niet is geconfigureerd voor de hybride Azure AD join.

Als u al op de momentopname van een virtuele Machine (VM vertrouwen,) te maken van aanvullende virtuele machines, zorg er dan voor dat u een momentopname van een virtuele machine die niet is geconfigureerd voor de hybride Azure AD join.

De registratie van Windows downlevel-apparaten wordt niet ondersteund voor apparaten die zijn geconfigureerd voor gebruiker profiel roaming of zwervende referenties. Als u zwervende profielen of instellingen vertrouwen, gebruikt u Windows 10.

- De registratie van Windows downlevel-apparaten **is** in omgevingen met niet-gefedereerde via naadloze eenmalige aanmelding ondersteund [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 
 
- De registratie van Windows downlevel-apparaten **is niet** ondersteund bij het gebruik van Azure AD Pass-through-verificatie zonder naadloze eenmalige aanmelding.

- De registratie van Windows downlevel-apparaten **is niet** ondersteund voor apparaten die gebruikmaken van zwervende profielen. Als u zwervende profielen of instellingen vertrouwen, gebruikt u Windows 10.


De registratie van Windows Server met de rol van domeincontroller (DC) wordt niet ondersteund.

Als uw organisatie toegang tot Internet via een geverifieerde uitgaande proxy vereist, moet u ervoor zorgen dat uw Windows 10-computers aan de uitgaande proxy verifiëren kunnen. Omdat Windows 10-computers worden uitgevoerd met behulp van de context van de computer het registreren van apparaten, is het nodig zijn om uitgaande proxy-verificatie met behulp van de context van de computer te configureren.


Hybride Azure AD join is een proces naar uw on-premises domein apparaten automatisch wordt geregistreerd bij Azure AD. Er zijn gevallen waarin u niet wilt dat al uw apparaten automatisch te registreren. Als dit het geval is, raadpleegt u [over het beheren van de hybride Azure AD join van uw apparaten](hybrid-azuread-join-control.md).



## <a name="select-your-scenario"></a>Selecteer uw scenario

U kunt lid worden van hybride Azure AD voor de volgende scenario's kunt configureren:

- Beheerde domeinen
- Federatieve domeinen  



Als uw omgeving heeft beheerde domeinen, ondersteunt hybride Azure AD join:

- Doorgeven van verificatie (PTA) met naadloze eenmalige aanmelding (SSO) 

- Wachtwoord is gesynchroniseerd (WHS) met naadloze eenmalige aanmelding (SSO) 

Vanaf versie 1.1.819.0, biedt Azure AD Connect u de wizard een hybride Azure AD join configureren. De wizard kunt u het configuratieproces aanzienlijk te vereenvoudigen. Zie voor meer informatie:

- [Hybride Azure Active Directory-deelname configureren voor federatieve domeinen](hybrid-azuread-join-federated-domains.md)

- [Hybride Azure Active Directory-deelname configureren voor beheerde domeinen](hybrid-azuread-join-managed-domains.md)


 Als het installeren van de vereiste versie van Azure AD Connect kan niet worden gebruikt voor u, raadpleegt u [het handmatig configureren van device Registration service](../device-management-hybrid-azuread-joined-devices-setup.md). 






## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Configure hybride Azure Active Directory join voor federatieve domeinen](hybrid-azuread-join-federated-domains.md)
> [configureren hybride Azure Active Directory join voor beheerde domeinen](hybrid-azuread-join-managed-domains.md)




<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
