---
title: Azure AD SSPR vanuit het aanmeldingsscherm van Windows 10 | Microsoft Docs
description: Azure AD-wachtwoordherstel vanuit het aanmeldingsscherm van Windows 10 configureren en Ik ben mijn pincode vergeten
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: d38d2790ae73d147209bb0ccdbbbe20bf880daa4
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="azure-ad-password-reset-from-the-login-screen"></a>Azure AD-wachtwoord opnieuw instellen vanuit het aanmeldingsscherm

U hebt self-service voor wachtwoordherstel (SSPR) voor Azure AD al geïmplementeerd, maar uw gebruikers bellen nog steeds met de helpdesk wanneer ze hun wachtwoord zijn vergeten. Ze bellen de helpdesk omdat ze geen toegang hebben tot een webbrowser om toegang te krijgen tot SSPR.

Met de nieuwe Windows 10 Fall Creators Update zien gebruikers met apparaten die zijn toegevoegd aan Azure AD, de koppeling 'Wachtwoord opnieuw instellen' op hun aanmeldingsscherm. Wanneer ze op deze koppeling klikken, worden ze omgeleid naar dezelfde selfservice voor wachtwoordherstel (SSPR) als waarmee ze al bekend zijn.

Als u wilt dat gebruikers hun Azure AD-wachtwoord opnieuw kunnen instellen via het aanmeldingsscherm van Windows 10, moet aan de volgende vereisten worden voldaan:

* Windows 10, versie 1709, of nieuwere client die is [toegevoegd aan Azure AD](device-management-azure-portal.md).
* Self-service voor wachtwoordherstel voor Azure AD moet zijn ingeschakeld.
* U kunt de instelling die de koppeling 'Wachtwoord opnieuw instellen' inschakelt, configureren en implementeren met een van de volgende methoden:
   * [Intune-apparaatconfiguratieprofiel](active-directory-passwords-login.md#configure-reset-password-link-using-intune)
   * [Registersleutel](active-directory-passwords-login.md#configure-reset-password-link-using-the-registry)

## <a name="configure-reset-password-link-using-intune"></a>De koppeling Wachtwoord opnieuw instellen configureren met Intune

### <a name="create-a-device-configuration-policy-in-intune"></a>Een beleid voor apparaatconfiguratie maken in Intune

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en klik op **Intune**.
2. Maak een nieuw apparaatconfiguratieprofiel door te gaan naar **Apparaatconfiguratie** > **Profielen** > **Profiel maken**
   * Geef een beschrijvende naam op voor het profiel
   * Geef eventueel een duidelijke beschrijving van het profiel op
   * Platform **Windows 10 en hoger**
   * Profieltype **Aangepast**

   ![Profiel maken][CreateProfile]

3. **Instellingen** configureren
   * **Voeg de volgende OMA-URI-instelling toe** om de koppeling Wachtwoord opnieuw instellen in te schakelen
      * Geef een beschrijvende naam op om uit te leggen wat de instelling doet
      * Geef eventueel een duidelijke beschrijving van de instelling
      * **OMA-URI** ingesteld op `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      * **Gegevenstype** ingesteld op **Geheel getal**
      * **Waarde** ingesteld op **1**
      * Klik op **OK**
   * Klik op **OK**
4. Klik op **Maken**.

### <a name="assign-a-device-configuration-policy-in-intune"></a>Een beleid voor apparaatconfiguratie toewijzen in Intune

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>Een groep maken waarop het beleid voor apparaatconfiguratie moet worden toegepast

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en klik op **Azure Active Directory**.
2. Blader naar **Gebruikers en groepen** > **Alle groepen** > **Nieuwe groep**
3. Geef een naam op voor de groep en kies onder **Type lidmaatschap** de optie **Toegewezen** 
   * Kies onder **Leden** de aan Azure AD toegevoegde Windows 10-apparaten waarop u het beleid wilt toepassen.
   * Klik op **Selecteren**.
4. Klik op **Maken**.

Meer informatie over het maken van groepen vindt u in het artikel [Toegang tot resources beheren met Azure Active Directory-groepen](active-directory-manage-groups.md).

#### <a name="assign-device-configuration-policy-to-device-group"></a>Beleid voor apparaatconfiguratie toewijzen aan een apparaatgroep

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en klik op **Intune**.
2. Zoek het apparaatconfiguratieprofiel dat u eerder hebt gemaakt door te gaan naar **Apparaatconfiguratie** > **Profielen** en te klikken op het profiel
3. Het profiel toewijzen aan een groep apparaten 
   * Klik op **Toewijzingen** > onder **Opnemen** > **Groepen selecteren die moeten worden opgenomen**
   * Selecteer de groep die u eerder hebt gemaakt en klik op **Selecteren**
   * Klik op **Opslaan**

   ![Toewijzing][Assignment]

U hebt nu een beleid voor apparaatconfiguratie gemaakt en toegewezen om de koppeling Wachtwoord opnieuw instellen op het aanmeldingsscherm in te schakelen met Intune.

## <a name="configure-reset-password-link-using-the-registry"></a>De koppeling Wachtwoord opnieuw instellen configureren met het register

Het wordt aanbevolen deze methode alleen te gebruiken om de gewijzigde instelling te testen.

1. Meld u met administratorreferenties aan op het aan Azure AD toegevoegde apparaat
2. Voer **regedit** uit als administrator
3. Stel de volgende registersleutel in
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Wat gebruikers zien

Welke wijzigingen zien de gebruikers nadat het beleid is geconfigureerd en toegewezen? Hoe weten ze dat ze hun wachtwoord opnieuw kunnen instellen via het aanmeldingsscherm?

![Aanmeldingsscherm][LoginScreen]

Wanneer gebruikers zich proberen aan te melden, zien ze nu de koppeling Wachtwoord opnieuw instellen die toegang biedt tot de self-service voor wachtwoordherstel vanuit het aanmeldingsscherm. Deze functionaliteit maakt het mogelijk dat gebruikers hun wachtwoord opnieuw instellen zonder dat ze een ander apparaat moeten gebruiken om toegang te krijgen tot een webbrowser.

Uw gebruikers vinden hulp voor het gebruik van deze functie in [Uw wachtwoord voor werk of school opnieuw instellen](active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in)

## <a name="common-issues"></a>Algemene problemen

Wanneer u deze functie test met Hyper-V, wordt de koppeling 'Wachtwoord opnieuw instellen' niet weergegeven.

* Ga naar de VM die u voor de test gebruikt, klik op **Weergave** en schakel **Uitgebreide sessie** uit.

Wanneer u deze functie test met Extern bureaublad, wordt de koppeling 'Wachtwoord opnieuw instellen' niet weergegeven.

* Wachtwoordherstel wordt momenteel niet ondersteund vanaf een extern bureaublad.

## <a name="next-steps"></a>Volgende stappen
De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD

* [Hoe kan ik SSPR implementeren?](active-directory-passwords-best-practices.md)
* [Hoe kan ik inschakelen dat de pincode opnieuw kan worden ingesteld vanaf het aanmeldingsscherm?](https://docs.microsoft.com/intune/device-windows-pin-reset)
* [Meer informatie over MDM-verificatiebeleid](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-authentication)

[CreateProfile]: ./media/active-directory-passwords-login/create-profile.png "Een Intune-apparaatconfiguratieprofiel maken om de koppeling Wachtwoord opnieuw instellen op het aanmeldingsscherm van Windows 10 in te schakelen"
[Assignment]: ./media/active-directory-passwords-login/profile-assignment.png "Intune-beleid voor apparaatconfiguratie toewijzen aan een groep Windows 10-apparaten"
[LoginScreen]: ./media/active-directory-passwords-login/logon-reset-password.png "De koppeling Wachtwoord opnieuw instellen op het aanmeldingsscherm van Windows 10"
