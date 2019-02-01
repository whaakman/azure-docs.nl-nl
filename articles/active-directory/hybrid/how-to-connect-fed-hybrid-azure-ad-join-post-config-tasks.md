---
title: 'Azure AD Connect: Taken na configuratie van hybride Azure AD join | Microsoft Docs'
description: De Documentdetails van dit post-configuratietaken die nodig zijn voor uitvoeren de hybride Azure AD join
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: billmath
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: a8c3a586835a0eb8cdcfffaddaa771ffc161b7fc
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495561"
---
# <a name="post-configuration-tasks-for-hybrid-azure-ad-join"></a>Taken na configuratie voor hybride Azure AD-koppeling

Nadat u Azure AD Connect voor het configureren van uw organisatie voor de hybride Azure AD join hebt uitgevoerd, zijn er nog een paar extra stappen die moeten worden voltooid als u wilt dat de installatie te voltooien.  Alleen de stappen die van toepassing voor uw apparaten zijn te verrichten.

## <a name="1-configure-controlled-rollout-optional"></a>1. Configureren van beheerde implementatie (optioneel)
Alle domein-apparaten met Windows 10 en Windows Server 2016 automatisch registreren bij Azure AD wanneer alle configuratiestappen voltooid zijn. Als u liever een gecontroleerde implementatie in plaats van deze automatische registratie, kunt u Groepsbeleid selectief inschakelen of uitschakelen van automatische implementatie.  Dit groepsbeleid moet worden ingesteld voordat u begint met de andere configuratie stappen: Azure AD
* Maak een group policy object in uw Active Directory.
* Noem het (ex-Hybrid Azure AD join).
* Ga naar & bewerken:  Computerconfiguratie > beleid > Beheersjablonen > Windows-onderdelen > Device Registration service.

>[!NOTE]
>Voor 2012R2 de beleidsinstellingen zijn op **Computerconfiguratie > beleid > Beheersjablonen > Windows-onderdelen > Workplace Join > automatisch workplace join-clientcomputers**

* Deze instelling uitschakelen:  Domeincomputers als apparaten registreren.
* Toepassen en klik op OK.
* Koppel het groepsbeleidsobject aan de locatie van uw keuze (organisatie-eenheid, beveiliging of op het domein voor alle apparaten).

## <a name="2-configure-network-with-device-registration-endpoints"></a>2. Netwerk configureren met device registration-eindpunten
Zorg ervoor dat de volgende URL's toegankelijk vanaf computers in uw organisatie-netwerk voor registratie met Azure AD zijn:

* https://enterpriseregistration.windows.net
* https://login.microsoftonline.com
* https://device.login.microsoftonline.com 

## <a name="3-implement-wpad-for-windows-10-devices"></a>3. WPAD voor Windows 10-apparaten implementeren
Als uw organisatie toegang heeft tot Internet via een uitgaande proxy, Web Proxy Auto-Discovery (WPAD) om in te schakelen van Windows 10-computers te registreren bij Azure AD te implementeren.

## <a name="4-configure-the-scp-in-any-forests-that-were-not-configured-by-azure-ad-connect"></a>4. De SCP in alle forests die niet zijn geconfigureerd door Azure AD Connect configureren 

De service connection point (SCP) bevat uw Azure AD-tenant-gegevens die worden gebruikt door uw apparaten voor automatische registratie.  Voer het PowerShell-script, ConfigureSCP.ps1, die u hebt gedownload van Azure AD Connect.

## <a name="5-configure-any-federation-service-that-was-not-configured-by-azure-ad-connect"></a>5. Configureren van een federation-service die niet is geconfigureerd door Azure AD Connect

Als uw organisatie gebruikmaakt van een federation-service zich aanmeldt bij Azure AD, moeten de claimregels in uw Azure AD-vertrouwensrelatie voor relying party verificatie van apparaten toestaan. Als u van Federatie met AD FS gebruikmaakt, gaat u naar [Help bij AD FS](https://aka.ms/aadrptclaimrules) voor het genereren van de claimregels. Als u een niet-Microsoft-federatieoplossing gebruikt, moet u contact op met die provider voor instructies.  

>[!NOTE]
>Als u Windows downlevel-apparaten hebt, moet de service ondersteuning voor de claims authenticationmethod en wiaormultiauthn verlenen bij het ontvangen van aanvragen naar de Azure AD-vertrouwensrelatie. In AD FS, moet u een regel voor het transformeren van uitgifte die passen via de methode voor netwerkauthenticatie toevoegen.

## <a name="6-enable-azure-ad-seamless-sso-for-windows-down-level-devices"></a>6. Azure AD naadloze eenmalige aanmelding voor Windows downlevel-apparaten inschakelen

Als uw organisatie gebruikmaakt van wachtwoord-Hashsynchronisatie of Pass through-verificatie te melden bij Azure AD, Azure AD naadloze eenmalige aanmelding inschakelen met deze methode aanmelden om Windows downlevel-apparaten te verifiëren: https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso. 

## <a name="7-set-azure-ad-policy-for-windows-down-level-devices"></a>7. Azure AD-beleid voor Windows downlevel-apparaten instellen

Voor het registreren van Windows downlevel-apparaten, moet u om ervoor te zorgen dat het beleid voor Azure AD kan gebruikers om apparaten te registreren. 

* Meld u aan uw account in Azure portal.
* Ga naar:  Azure Active Directory > apparaten > instellingen voor apparaten
* Ingesteld op ' Gebruikers kunnen hun apparaten registreren bij Azure AD' op Alles.
* Op Opslaan klikken

## <a name="8-add-azure-ad-endpoint-to-windows-down-level-devices"></a>8. Azure AD-eindpunt toevoegen aan Windows downlevel-apparaten

De Azure AD-apparaat verificatie-eindpunt toevoegen aan het lokale intranetzones op uw Windows downlevel-apparaten om te voorkomen dat certificaatmeldingen bij het verifiëren van de apparaten: https://device.login.microsoftonline.com 

Als u [naadloze eenmalige aanmelding](how-to-connect-sso.md), ook deze zone inschakelen 'Balk statusupdates via scripts toestaan' en voeg het volgende eindpunt toe: https://autologon.microsoftazuread-sso.com 

## <a name="9-install-microsoft-workplace-join-on-windows-down-level-devices"></a>9. Microsoft Workplace Join op Windows downlevel-apparaten installeren

Met dit installatieprogramma maakt een geplande taak op het apparaat-systeem dat wordt uitgevoerd in de context van de gebruiker. De taak wordt geactiveerd wanneer de gebruiker zich aanmeldt bij Windows. De taak op de achtergrond lid wordt van het apparaat met Azure AD met de referenties van de gebruiker na verificatie met behulp van geïntegreerde Windows-verificatie. Het download center loopt https://www.microsoft.com/download/details.aspx?id=53554. 

## <a name="10-configure-group-policy-to-allow-device-registration"></a>10. Groepsbeleid om toe te staan van device Registration service configureren

* Maak een group policy object in uw Active Directory - als nog niet is gemaakt.
* Noem het (ex-Hybrid Azure AD join).
* Ga naar & bewerken:  Computerconfiguratie > beleid > Beheersjablonen > Windows-onderdelen > Device Registration service
* Inschakelen:  Domeincomputers als apparaten registreren
* Toepassen en klik op OK.
* Koppel het groepsbeleidsobject aan de locatie van uw keuze (organisatie-eenheid, beveiliging of op het domein voor alle apparaten).

>[!NOTE]
>Voor 2012R2 de beleidsinstellingen zijn op **Computerconfiguratie > beleid > Beheersjablonen > Windows-onderdelen > Workplace Join > automatisch workplace join-clientcomputers**

## <a name="next-steps"></a>Volgende stappen
[Write-back van apparaat configureren](how-to-connect-device-writeback.md)
