---
title: De werking van eenmalige aanmelding met on-premises bronnen in Azure AD gekoppelde apparaten | Microsoft Docs
description: Lees hoe u hybride Azure Active Directory-gekoppelde apparaten kunt configureren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 871302a93fadd7c5931ad9403370badf0d0673af
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452697"
---
# <a name="how-sso-to-on-premises-resources-works-on-azure-ad-joined-devices"></a>De werking van eenmalige aanmelding met on-premises bronnen in Azure AD gekoppelde apparaten

Het is waarschijnlijk niet een verrassing dat een apparaat dat lid is Azure Active Directory (Azure AD) u een ervaring voor eenmalige aanmelding (SSO) voor cloud-apps van uw tenant biedt. Als uw omgeving heeft een on-premises Active Directory (AD), kunt u de SSO-ervaring op deze apparaten toe kunt uitbreiden.

In dit artikel wordt uitgelegd hoe dit werkt.

## <a name="how-it-works"></a>Hoe werkt het? 

Omdat u nodig hebt om te onthouden slechts één één gebruikersnaam en wachtwoord, wordt eenmalige aanmelding vereenvoudigt de toegang tot uw resources en verbetert de beveiliging van uw omgeving. Met een Azure AD toegevoegd apparaat hebben uw gebruikers al een SSO-ervaring voor de cloud-apps in uw omgeving. Als uw omgeving een Azure AD heeft en een on-premises AD, wilt u waarschijnlijk om uit te breiden het bereik van de SSO-ervaring aan uw on-premises regel van Business (LOB)-toepassingen, bestandsshares en printers.  


Azure AD gekoppelde apparaten hebben geen kennis over uw on-premises AD-omgeving omdat ze niet zijn gekoppeld aan deze. U kunt echter aanvullende informatie opgeven over uw on-premises AD op deze apparaten met Azure AD Connect.
Een omgeving waarin beide, een Azure AD en een on-premises AD, is ook bekend is hybride omgeving. Als u een hybride omgeving hebt, is het waarschijnlijk dat u al hebt met Azure AD Connect geïmplementeerd om te synchroniseren van uw on-premises id-informatie naar de cloud. Als onderdeel van het proces van synchronisatie synchroniseert Azure AD Connect on-premises domeininformatie naar Azure AD. Wanneer een gebruiker zich aanmeldt met een Azure AD toegevoegde apparaat in een hybride omgeving:

1. Azure AD stuurt dat de naam van de on-premises domein de gebruiker lid is van terug naar het apparaat. 

2. De lokale security authority (LSA) service kan Kerberos-verificatie op het apparaat.

Tijdens een poging tot toegang tot een resource in het on-premises domein van de gebruiker, het apparaat:

1. Maakt gebruik van de domein-informatie te zoeken naar een domeincontroller (DC). 

2. De on-premises verzendt domein en de gebruikersreferenties naar de domeincontroller bevinden om op te halen van de geverifieerde gebruiker.

3. Ontvangt van een Kerberos [Ticket-Granting Ticket (TGT)](https://docs.microsoft.com/windows/desktop/secauthn/ticket-granting-tickets) die wordt gebruikt voor toegang tot AD-join-bronnen.

Alle apps die zijn geconfigureerd voor **geïntegreerde Windows-verificatie** SSO naadloos ontvangen wanneer een gebruiker probeert toegang tot deze.  

Windows Hello voor bedrijven is aanvullende configuratie vereist om in te schakelen van on-premises eenmalige aanmelding van een Azure AD gekoppelde apparaat. Zie voor meer informatie, [configureren Azure AD gekoppelde apparaten voor On-premises eenmalige aanmelding over het gebruik van Windows Hello voor bedrijven](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso-base). 

## <a name="what-you-get"></a>Wat u krijgt

Met eenmalige aanmelding, toegevoegde op een Azure AD apparaat die u kunt: 

- Toegang tot een UNC-pad op een lidserver AD

- Toegang tot een AD lid webserver die is geconfigureerd voor geïntegreerde Windows-beveiliging 



Als u beheren van uw wilt on-premises AD vanaf een Windows-apparaat, installeert de [Remote Server Administration Tools voor Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=45520).

U kunt gebruiken:

- De Active Directory: gebruikers en Computers (ADUC)-module voor het beheer van alle AD-objecten. U moet echter het domein dat u verbinding maken wilt met handmatig opgeven.

- De DHCP-module voor het beheren van een AD-join DHCP-server. U moet echter mogelijk opgeven van de DHCP-servernaam of het adres.

 
## <a name="what-you-should-know"></a>Wat u moet weten

Mogelijk moet u pas uw [filteren op basis van een domein](../hybrid/how-to-connect-sync-configure-filtering.md#domain-based-filtering) in Azure AD Connect om ervoor te zorgen dat de gegevens over de vereiste domeinen worden gesynchroniseerd.

Apps en resources die afhankelijk van Active Directory machine verificatie niet werkt zijn omdat Azure AD gekoppelde apparaten geen een computerobject in AD. 

U kunt geen bestanden delen met andere gebruikers in een Azure AD join-apparaat.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [wat is Apparaatbeheer in Azure Active Directory?](overview.md) 
