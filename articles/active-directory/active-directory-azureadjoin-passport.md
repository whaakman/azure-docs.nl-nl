---
title: "Verifiëren van identiteiten zonder wachtwoorden via Windows Hello voor bedrijven en Azure AD | Microsoft Docs"
description: Biedt een overzicht van Windows Hello voor bedrijven en aanvullende informatie over het implementeren van Windows Hello voor bedrijven.
services: active-directory
documentationcenter: 
author: femila
manager: mtillman
editor: 
tags: azure-classic-portal
ms.assetid: f907bb90-8776-46ca-9e12-279949af66ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 22534cc5f7f2de235bc1f1212c63ea227083c5a4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="authenticating-identities-without-passwords-through-windows-hello-for-business"></a>Identiteiten zonder wachtwoorden via Windows Hello voor bedrijven verifiëren
De huidige methoden voor verificatie met wachtwoorden die alleen zijn niet voldoende om gebruikers te beschermen. Gebruikers gebruiken en vergeten wachtwoorden. Wachtwoorden zijn breachable, phishable, foutgevoelige scheuren en te raden. Ook krijgen ze moeilijk te onthouden en gevoelig voor aanvallen, zoals '[doorgeven van de hash](https://technet.microsoft.com/dn785092.aspx)'.

## <a name="about-windows-hello-for-business"></a>Over Windows Hello voor bedrijven
Windows Hello voor bedrijven is een persoonlijke/openbare sleutel of benadering voor verificatie op basis van certificaten voor organisaties en consumenten die zich verder uitstrekt dan wachtwoorden. Deze vorm van verificatie, afhankelijk van sleutelpaar-referenties die wachtwoorden kunnen vervangen en het bestand zijn tegen inbreuk, diefstal en phishing zijn.

 Windows Hello voor bedrijven kunt een gebruiker wordt geverifieerd bij een Microsoft-account, een Windows Server Active Directory-account, een Microsoft Azure Active Directory (Azure AD)-account of een niet-Microsoft-service die snel identiteit Online (FIDO) authenticatie ondersteunt. Na een initiële in twee stappen verificatie tijdens Windows Hello voor bedrijven-inschrijving, Windows Hello voor bedrijven is ingesteld op het apparaat van de gebruiker en de gebruiker een gebaar, kan dit Windows Hello of een PINCODE instelt. De gebruiker biedt de gebaar om hun identiteit te verifiëren. Windows gebruikt vervolgens Windows Hello voor bedrijven om te verifiëren van de gebruiker en sneller toegang tot beveiligde bronnen en services.

De persoonlijke sleutel wordt beschikbaar gesteld uitsluitend via een 'Gebaar van de gebruiker' zoals een PINCODE, biometrie of een extern apparaat zoals een smartcard die gebruikmaakt van de gebruiker zich aanmeldt bij het apparaat. Deze informatie is gekoppeld aan een certificaat of een asymmetrisch sleutelpaar. De persoonlijke sleutel is als het apparaat een Trusted Platform Module (TPM)-chip heeft Attestation hardware. De persoonlijke sleutel nooit het apparaat zal verlaten.

De openbare sleutel is geregistreerd bij Azure Active Directory en Windows Server Active Directory (voor on-premises). ID-Providers (IDPs) voor het valideren van de gebruiker door de openbare sleutel van de gebruiker toewijzen aan de persoonlijke sleutel en informatie aanmelden via één keer wachtwoord (OTP), PhoneFactor of een andere meldingsmechanisme bieden.

## <a name="why-enterprises-should-adopt-windows-hello-for-business"></a>Waarom ondernemingen dient vast te stellen Windows Hello voor bedrijven
Als u Windows Hello voor bedrijven, kunnen ondernemingen maken hun resources nog beter beveiligen door:

* Instellen van Windows Hello voor bedrijven met een hardware-voorkeurs-optie. Dit betekent dat sleutels op TPM 1.2 of TPM 2.0 zodra deze beschikbaar wordt gegenereerd. Wanneer er geen TPM beschikbaar is, wordt de software de sleutel gegenereerd.
* Het definiëren van de complexiteit en de lengte van de PINCODE en of gebruik Hallo is ingeschakeld in uw organisatie.
* Configureren van Windows Hello voor bedrijven ter ondersteuning van smartcard-achtige scenario's met behulp van de vertrouwensrelatie op basis van certificaten.

## <a name="how-windows-hello-for-business-works"></a>Hoe Windows Hello voor bedrijven werkt
1. Sleutels worden gegenereerd op de hardware door TPM of software. Veel apparaten hebben een ingebouwde TPM-chip die de hardware beveiligt door te integreren cryptografische sleutels in apparaten. TPM 1.2 of TPM 2.0 genereert sleutels of certificaten die zijn gemaakt van de gegenereerde sleutels.
2. De TPM verklaart deze sleutels hardware is gebonden.
3. Een gebaar van één ontgrendelen Hiermee ontgrendelt u het apparaat. Deze gebaar staat toegang tot meerdere resources als het apparaat lid van een domein of Azure is AD lid.

## <a name="how-the-windows-hello-for-business-lifecycle-works"></a>Hoe het Windows Hello voor bedrijven lifecycle werkt
![Windows Hello voor bedrijven levenscyclus](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Het voorgaande diagram illustreert het persoonlijke/openbare sleutelpaar en de validatie van de identiteitsprovider. Elk van deze stappen wordt uitgelegd in detail hier:

1. De gebruiker uw bewijs van hun identiteit via meerdere ingebouwde taalprogramma methoden (gebaren, fysieke smartcards, multi-factor authentication-Server) en stuurt deze gegevens naar een identiteit Provider (IDP) zoals Azure Active Directory of on-premises Active Directory.
2. Het apparaat vervolgens maakt de sleutel, verklaart de sleutel, duurt het openbare deel van deze sleutel, worden deze gekoppeld met station instructies, zich heeft aangemeld en verzendt het naar de IDP registreren van de sleutel.
3. Zodra het openbare deel van de sleutel wordt geregistreerd in de IDP, vraagt de IDP het apparaat om te ondertekenen met het persoonlijke gedeelte van de sleutel.
4. De IDP vervolgens valideert en verstrekt de verificatietoken waarmee de gebruikers en het apparaat toegang tot de beveiligde bronnen. IDPs kan schrijven platformoverschrijdende apps of browserondersteuning (via JavaScript/Webcrypto API's) maken en gebruiken Windows Hello voor bedrijven-referenties voor de gebruikers.

## <a name="the-deployment-requirements-for-windows-hello-for-business"></a>De vereisten voor de implementatie voor Windows Hello voor bedrijven
### <a name="at-the-enterprise-level"></a>Op het ondernemingsniveau van de
* Heeft een Azure-abonnement van de onderneming.

### <a name="at-the-user-level"></a>Op het gebruikersniveau van de
* Computer van de gebruiker wordt uitgevoerd voor Windows 10 Professional of Enterprise.

Zie voor gedetailleerde implementatie-instructies [inschakelen Windows Hello voor bedrijven in de organisatie](active-directory-azureadjoin-passport-deployment.md).

## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 for the enterprise: Ways to use devices for work](active-directory-azureadjoin-windows10-devices-overview.md) (Windows 10 voor de onderneming: manieren om apparaten voor werk te gebruiken)
* [Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md) (Cloudmogelijkheden uitbreiden naar Windows 10-apparaten met behulp van Azure Active Directory Join)
* [Learn about usage scenarios for Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md) (Gebruiksscenario’s voor Azure AD Join)
* [Connect domain-joined devices to Azure AD for Windows 10 experiences](active-directory-azureadjoin-devices-group-policy.md) (Apparaten die lid zijn van een domein verbinden met Azure AD voor Windows 10-ervaringen)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)

