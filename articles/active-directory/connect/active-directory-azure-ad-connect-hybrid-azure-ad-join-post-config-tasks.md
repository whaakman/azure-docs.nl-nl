---
title: 'Azure AD Connect: Taken hybride Azure AD join na configuratie | Microsoft Docs'
description: De Documentdetails van dit post configuratietaken die nodig zijn voor voltooien de hybride Azure AD join
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: samueld
editor: billmath
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: anandyadavmsft
ms.openlocfilehash: 02f1cbd1f2b8f7b0bec2f8016a300ede1d6f0439
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="post-configuration-tasks-for-hybrid-azure-ad-join"></a>Post-configuratietaken voor hybride Azure AD join

Nadat u Azure AD Connect voor het configureren van uw organisatie voor hybride Azure AD join hebt uitgevoerd, zijn er een aantal extra stappen die u uitvoeren moet als u wilt dat de installatie te voltooien.  Alleen de stappen die voor uw apparaten gelden te verrichten.

## <a name="1-configure-controlled-rollout-optional"></a>1. Configureren van beheerde implementatie (optioneel)
Alle domein-apparaten met Windows 10 en Windows Server 2016 automatisch registreren met Azure AD als alle configuratiestappen voltooid zijn. Als u liever een gecontroleerde rollout in plaats van deze automatische registratie, kunt u Groepsbeleid in- of uitschakelen van automatische implementatie.  Dit groepsbeleid moet worden ingesteld voordat u de andere configuratie stappen: Azure AD
* Maak een groepsbeleidsobject in uw Active Directory.
* Naam (ex hybride Azure AD join).
* Ga naar & bewerken: Computerconfiguratie > beleidsregels > Beheersjablonen > Windows-onderdelen > apparaatregistratie.

>[!NOTE]
>Voor 2012R2 de beleidsinstellingen zijn op **Computerconfiguratie > beleidsregels > Beheersjablonen > Windows-onderdelen > werkplek koppelen > automatisch workplace join clientcomputers**

* Deze instelling uitschakelen: Domeincomputers als apparaten registreren.
* Toepassen en klik op OK.
* Koppel het groepsbeleidsobject aan de locatie van uw keuze (organisatie-eenheid, beveiliging of op het domein voor alle apparaten).

## <a name="2-configure-network-with-device-registration-endpoints"></a>2. Netwerk configureren met device registration-eindpunten
Zorg ervoor dat de volgende URL's toegankelijk vanaf computers in het netwerk van uw organisatie voor registratie bij Azure AD zijn:

* https://enterpriseregistration.windows.net
* https://login.microsoftonline.com
* https://device.login.microsoftonline.com 

## <a name="3-implement-wpad-for-windows-10-devices"></a>3. WPAD voor Windows 10-apparaten implementeren
Als uw organisatie toegang heeft tot Internet via een uitgaande proxyconfiguratie, implementeert u Web Proxy Auto-Discovery (WPAD) zodat Windows 10-computers te registreren bij Azure AD.

## <a name="4-configure-the-scp-in-any-forests-that-were-not-configured-by-azure-ad-connect"></a>4. Het SCP te configureren in alle forests die niet zijn geconfigureerd voor Azure AD Connect 

Het service connection point (SCP) bevat de informatie van uw Azure AD-tenant die wordt gebruikt door uw apparaten voor automatische registratie.  Voer de PowerShell-script, ConfigureSCP.ps1 die u hebt gedownload van Azure AD Connect.

## <a name="5-configure-any-federation-service-that-was-not-configured-by-azure-ad-connect"></a>5. Configureren van een federation-service is niet geconfigureerd voor Azure AD Connect

Als uw organisatie gebruikmaakt van een federation-service aan te melden bij Azure AD, moeten de claimregels die u in uw Azure AD-vertrouwensrelatie voor relying party verificatie van apparaten toestaan. Als u van Federatie met AD FS gebruikmaakt, gaat u naar [Help bij AD FS](https://aka.ms/aadrptclaimrules) voor het genereren van de claimregels. Als u van een niet-Microsoft-federation-oplossing gebruikmaakt, neem dan contact op met de die provider voor instructies.  

>[!NOTE]
>Als u Windows downlevel-apparaten hebt, moet de service ondersteuning voor uitgifte van de claims authenticationmethod en wiaormultiauthn tijdens het ontvangen van aanvragen voor de Azure AD-vertrouwensrelatie. In AD FS, moet u een regel voor het transformeren van uitgifte die geeft via de verificatiemethode toevoegen.

## <a name="6-enable-azure-ad-seamless-sso-for-windows-down-level-devices"></a>6. Azure AD naadloze eenmalige aanmelding voor Windows downlevel-apparaten inschakelen

Als uw organisatie gebruikmaakt van synchronisatie van wachtwoordhash of Pass through-verificatie aan te melden bij Azure AD, Azure AD naadloze eenmalige aanmelding inschakelen met deze methode aanmelden om Windows downlevel-apparaten te verifiëren: https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso. 

## <a name="7-set-azure-ad-policy-for-windows-down-level-devices"></a>7. Azure AD-beleid voor Windows-downlevel-apparaten instellen

Voor het registreren van eerdere Windows-apparaten, moet u ervoor zorgen dat het Azure AD-beleid kan gebruikers registreren van apparaten. 

* Aanmelden bij uw account in de Azure-portal.
* Ga naar: Azure Active Directory > apparaten > apparaatinstellingen
* Ingesteld op ' Gebruikers kunnen hun apparaten registreren bij Azure AD' op Alles.
* Op Opslaan klikken

## <a name="8-add-azure-ad-endpoint-to-windows-down-level-devices"></a>8. Azure AD-eindpunt toevoegen aan Windows downlevel-apparaten

Het eindpunt voor authenticatie van Azure AD-apparaat toevoegen aan de lokale Intranet-zones op uw eerdere Windows-apparaten om te voorkomen dat certificaatmeldingen bij het verifiëren van de apparaten: https://device.login.microsoftonline.com 

Als u [naadloze eenmalige aanmelding](https://aka.ms/hybrid/sso), ook 'Balk statusupdates via script toestaan' inschakelt op die zone en voeg het volgende eindpunt toe: https://autologon.microsoftazuread-sso.com 

## <a name="9-install-microsoft-workplace-join-on-windows-down-level-devices"></a>9. Microsoft Workplace Join op downlevel-apparaten met Windows installeren

Dit installatieprogramma maakt een geplande taak op het apparaat-systeem dat wordt uitgevoerd in de context van de gebruiker. De taak wordt geactiveerd wanneer de gebruiker zich aanmeldt bij Windows. De taak achtergrond lid wordt van het apparaat met Azure AD met referenties van de gebruiker na verificatie met behulp van geïntegreerde Windows-verificatie. Het download center loopt https://www.microsoft.com/en-us/download/details.aspx?id=53554. 

## <a name="10-configure-group-policy-to-allow-device-registration"></a>10. Groepsbeleid configureren zodat het registreren van apparaten toestaan

* Een groepsbeleidsobject maken in uw Active Directory--als nog niet gemaakt.
* Naam (ex hybride Azure AD join).
* Ga naar & bewerken: Computerconfiguratie > beleidsregels > Beheersjablonen > Windows-onderdelen > apparaatregistratie
* Inschakelen: Domeincomputers als apparaten registreren
* Toepassen en klik op OK.
* Koppel het groepsbeleidsobject aan de locatie van uw keuze (organisatie-eenheid, beveiliging of op het domein voor alle apparaten).

>[!NOTE]
>Voor 2012R2 de beleidsinstellingen zijn op **Computerconfiguratie > beleidsregels > Beheersjablonen > Windows-onderdelen > werkplek koppelen > automatisch workplace join clientcomputers**

## <a name="next-steps"></a>Volgende stappen
[Write-back van apparaat configureren](./active-directory-aadconnect-feature-device-writeback.md)
