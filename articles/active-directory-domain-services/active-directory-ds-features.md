---
title: 'Azure Active Directory Domain Services: Functies | Microsoft Docs'
description: Functies van Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: e0f38ed8367f076c9f9c4da77440bb3e9603d514
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234198"
---
# <a name="features-provided-by-azure-active-directory-domain-services"></a>Functies van Azure Active Directory Domain Services

De volgende functies zijn beschikbaar in Azure AD Domain Services beheerde domeinen.

* **Eenvoudige implementatie-ervaring:** U kunt Azure AD Domain Services voor uw Azure AD-adres lijst inschakelen met slechts enkele klikken. Uw beheerde domein bevat alleen Cloud gebruikers accounts en gebruikers accounts die zijn gesynchroniseerd vanuit een on-premises Directory.
* **Ondersteuning voor domein deelname:** U kunt eenvoudig domein computers toevoegen aan het virtuele netwerk van Azure dat uw beheerde domein beschikbaar is in. De ervaring voor domein deelname op Windows-besturings systemen voor clients en servers werkt probleemloos voor domeinen die worden onderhouden door Azure AD Domain Services. U kunt ook gebruikmaken van het hulp programma voor automatische domein deelname aan deze domeinen.
* **Eén domein exemplaar per Azure AD-map:** U kunt één Active Directory domein maken voor elke Azure AD-adres lijst.
* **Domeinen maken met aangepaste namen:** U kunt met behulp van Azure AD Domain Services domeinen maken met aangepaste namen (bijvoorbeeld ' contoso100.com '). U kunt geverifieerde of niet-geverifieerde domein namen gebruiken. U kunt eventueel ook een domein met het ingebouwde domein achtervoegsel (*. onmicrosoft.com) maken dat wordt aangeboden door uw Azure AD-adres lijst.
* **Geïntegreerd met Azure AD:** U hoeft geen replicatie naar Azure AD Domain Services te configureren of te beheren. Gebruikers accounts, groepslid maatschappen en gebruikers referenties (wacht woorden) van uw Azure AD-adres lijst zijn automatisch beschikbaar in Azure AD Domain Services. Nieuwe gebruikers, groepen of wijzigingen in kenmerken van uw Azure AD-Tenant of uw on-premises Directory worden automatisch gesynchroniseerd naar Azure AD Domain Services.
* **NTLM-en Kerberos-verificatie:** Met ondersteuning voor NTLM-en Kerberos-verificatie kunt u toepassingen implementeren die afhankelijk zijn van geïntegreerde Windows-authenticatie.
* **Uw bedrijfs referenties/wacht woorden gebruiken:** Wacht woorden voor gebruikers in uw Azure AD-Tenant werken met Azure AD Domain Services. Gebruikers kunnen hun bedrijfs referenties gebruiken om computers toe te voegen aan een domein, interactief of via extern bureau blad aan te melden en te verifiëren op basis van het beheerde domein.
* **Lees ondersteuning LDAP-binding & LDAP:** U kunt toepassingen die afhankelijk zijn van LDAP-bindingen gebruiken om gebruikers te verifiëren in domeinen die worden onderhouden door Azure AD Domain Services. Daarnaast kunnen toepassingen die gebruikmaken van LDAP-lees bewerkingen voor het opvragen van gebruikers-en computer kenmerken vanuit de Directory ook werken met Azure AD Domain Services.
* **Secure LDAP (LDAPS):** U kunt toegang tot de Directory via secure LDAP (LDAPS) inschakelen. Secure LDAP toegang is standaard beschikbaar in het virtuele netwerk. U kunt ook optioneel beveiligde LDAP-toegang via Internet inschakelen.
* **Groepsbeleid:** U kunt één ingebouwd groeps beleidsobject voor de containers gebruikers en computers gebruiken om naleving af te dwingen van het vereiste beveiligings beleid voor gebruikers accounts en computers die lid zijn van een domein. U kunt ook uw eigen aangepaste groeps beleidsobjecten maken en deze toewijzen aan aangepaste organisatie-eenheden om [groeps beleid te beheren](manage-group-policy.md).
* **DNS beheren:** Leden van de groep AAD DC-Administrators kunnen DNS beheren voor uw beheerde domein met behulp van vertrouwde hulpprogram ma's voor DNS-beheer, zoals de MMC-module voor DNS-beheer.
* **Aangepaste organisatie-eenheden maken:** Leden van de groep AAD DC Administrators kunnen aangepaste organisatie-eenheden maken in het beheerde domein. Deze gebruikers krijgen volledige beheerders bevoegdheden via aangepaste organisatie-eenheden, zodat ze service accounts, computers, groepen enzovoort kunnen toevoegen aan of verwijderen uit deze aangepaste organisatie-eenheden.
* **Beschikbaar in veel algemene Azure-regio's:** Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure AD Domain Services beschikbaar is.
* **Hoge Beschik baarheid:** Azure AD Domain Services biedt een hoge Beschik baarheid voor uw domein. Deze functie biedt de garantie voor een hogere uptime van de service en de flexibiliteit van storingen. Ingebouwde status bewaking biedt automatisch herstel van fouten door nieuwe instanties te laten oplopen om mislukte instanties te vervangen en om te zorgen voor een voortdurende service voor uw domein.
* **AD-account vergrendelings beveiliging:** Gebruikers accounts worden 30 minuten geblokkeerd als vijf ongeldige wacht woorden binnen twee minuten worden gebruikt. Accounts worden na 30 minuten automatisch ontgrendeld.
* **Gebruik vertrouwde beheer Programma's:** U kunt vertrouwde Windows Server Active Directory-beheer Programma's zoals de Active Directory-beheercentrum of Active Directory Power shell gebruiken voor het beheren van beheerde domeinen.
