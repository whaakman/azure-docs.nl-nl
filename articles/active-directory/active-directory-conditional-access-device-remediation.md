---
title: Problemen met toegang tot Azure Active Directory oplossen | Microsoft Docs
description: Meer informatie over stappen die u kunt uitvoeren om toegangsproblemen met onlineresources van uw organisatie op te lossen.
services: active-directory
keywords: voorwaardelijke toegang op basis van een apparaat, apparaatregistratie, apparaatregistratie inschakelen, apparaatregistratie en MDM
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/24/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: fbabf6f2e1e588ba509c4da84ab1700b1b5d4f87
ms.openlocfilehash: ad9f9a8c5b370ffa916b9089ef3ce523fe0266c7


---
# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Problemen met toegang tot Azure Active Directory oplossen
U probeert toegang te krijgen tot het SharePoint Online-intranet van uw organisatie en het foutbericht 'Toegang geweigerd' wordt weergegeven. Wat kunt u doen?


In dit artikel vindt u stappen die u kunt uitvoeren om toegangsproblemen met onlineresources van uw organisatie op te lossen.

Voor hulp bij het oplossen van toegangsproblemen met Azure Active Directory (Azure AD), raadpleegt u de sectie in het artikel die betrekking heeft op uw apparaatplatform:

* Windows-apparaat
* iOS-apparaat (Binnenkort vindt u hier hulp voor iPhones en iPads.)
* Android-apparaat (Binnenkort vindt u hier hulp voor Android-telefoons en -tablets.)

## <a name="access-from-a-windows-device"></a>Toegang vanaf een Windows-apparaat
Als op uw apparaat een van de volgende platforms wordt uitgevoerd, zoekt u in de volgende secties naar het foutbericht dat wordt weergegeven wanneer u probeert toegang te krijgen tot een toepassing of service:

* Windows 10
* Windows 8.1
* Windows 8
* Windows 7
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Apparaat is niet geregistreerd
Als uw apparaat niet is geregistreerd bij Azure AD en de toepassing is beveiligd met een beleid op basis van apparaten, wordt mogelijk een pagina met een van de volgende foutberichten weergegeven:

![Scenario](./media/active-directory-conditional-access-device-remediation/01.png "Berichten over ontoegankelijke toepassingen voor niet-geregistreerde apparaten")

Probeer het volgende als het apparaat is toegevoegd aan een Active Directory-domein in uw organisatie:

1. Zorg dat u bij Windows bent aangemeld met uw werkaccount (uw Active Directory-account).
2. Maak via een VPN (virtueel particulier netwerk) of DirectAccess verbinding met uw bedrijfsnetwerk.
3. Wanneer u verbinding hebt gemaakt, drukt u op de toets met het Windows-logo + L om uw Windows-sessie te vergrendelen.
4. Voer de referenties voor uw werkaccount in om de Windows-sessie te ontgrendelen.
5. Wacht even en probeer dan opnieuw of u de toepassing of service kunt openen.
6. Als dezelfde pagina wordt weergegeven, klikt u op de koppeling **Meer details** en neemt u met deze gegevens contact op met de beheerder.

Als uw apparaat niet is verbonden met een domein en Windows 10 op het apparaat wordt uitgevoerd, hebt u twee opties:

* Azure AD Join uitvoeren
* Uw werk- of schoolaccount toevoegen aan Windows

Zie [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md) (Windows 10-apparaten gebruiken op uw werkplek) voor informatie over de verschillen tussen deze opties.

Als u Azure AD Join wilt uitvoeren, volgt u de stappen voor het platform waarop uw apparaat wordt uitgevoerd. (Azure AD Join is niet beschikbaar op Windows Phones.)

**Windows 10 Jubileumupdate**

1. Start de app **Instellingen**.
2. Klik op **Accounts** > **Toegang via werk of school**.
3. Klik op **Verbinden**.
4. Klik op **Dit apparaat toevoegen aan Azure AD**.
5. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
6. Meld u af en meld u weer aan met uw werkaccount.
7. Probeer opnieuw toegang te krijgen tot de toepassing.

**Windows 10-update van november 2015**

1. Start de app **Instellingen**.
2. Klik op **Systeem** > **Info**.
3. Klik op **Lid worden van Azure AD**.
4. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
5. Meld u af en meld u weer aan met uw werkaccount (uw Azure AD-account).
6. Probeer opnieuw toegang te krijgen tot de toepassing.

Voer de volgende stappen uit om uw werk- of schoolaccount toe te voegen:

**Windows 10 Jubileumupdate**

1. Start de app **Instellingen**.
2. Klik op **Accounts** > **Toegang via werk of school**.
3. Klik op **Verbinden**.
4. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
5. Probeer opnieuw toegang te krijgen tot de toepassing.

**Windows 10-update van november 2015**

1. Start de app **Instellingen**.
2. Klik op **Accounts** > **Uw accounts**.
3. Klik op **Werk- of schoolaccount toevoegen**.
4. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
5. Probeer opnieuw toegang te krijgen tot de toepassing.

Als uw apparaat niet in een domein is opgenomen en Windows 8.1 op het apparaat wordt uitgevoerd, kunt u de volgende stappen uitvoeren om een Workplace Join uit te voeren en het apparaat in te schrijven bij Microsoft Intune:

1. Open **Pc-instellingen**.
2. Klik op **Netwerk** > **Werkplek**.
3. Klik op **Deelnemen**.
4. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
5. Klik op **Inschakelen**.
6. Probeer opnieuw toegang te krijgen tot de toepassing.

### <a name="browser-is-not-supported"></a>Browser wordt niet ondersteund
Misschien hebt u geen toegang als u een toepassing of service probeert te openen met een van de volgende browsers:

* Chrome, Firefox of een andere browser dan Microsoft Edge of Microsoft Internet Explorer in Windows 10 of Windows Server 2016
* Firefox in Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2

Er wordt een foutpagina weergegeven die er ongeveer als volgt uitziet:

![Scenario](./media/active-directory-conditional-access-device-remediation/02.png "Berichten over ontoegankelijke toepassingen voor niet-ondersteunde browsers")

De enige oplossing is een browser te gebruiken die de toepassing ondersteunt voor uw apparaatplatform.

## <a name="next-steps"></a>Volgende stappen
[Voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access.md)




<!--HONumber=Jan17_HO4-->


