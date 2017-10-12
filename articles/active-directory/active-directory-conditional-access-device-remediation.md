---
title: U kunt daar niet komen vanaf deze locatie in Azure Portal vanaf een Windows-apparaat | Microsoft Docs
description: Informatie over wat 'U kunt daar niet komen vanaf deze locatie' veroorzaakt en wat u kunt controleren om te voorkomen dat u dit dialoogvenster tegenkomt.
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
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: d86e47507f7a578e8a0affb690b6d96673b6b69a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a>U kunt daar niet komen vanaf deze locatie op een Windows-apparaat

Tijdens een poging om bijvoorbeeld het SharePoint Online-intranet van uw organisatie te openen, krijgt u een pagina te zien waarop staat dat *u geen toegang hebt*. U ziet deze pagina omdat de beheerder voorwaardelijk toegangsbeleid heeft geconfigureerd op basis waarvan de toegang tot resources in uw organisatie onder bepaalde omstandigheden wordt geblokkeerd. Uiteindelijk kan het nodig zijn om contact op te nemen met de helpdesk of beheerder, maar voordat u dit doet, is er een aantal dingen dat u eerst zelf kunt proberen.

Als u een **Windows**-apparaat gebruikt, controleert u het volgende:

- Gebruikt u een ondersteunde browser?

- Voert u een ondersteunde versie van Windows uit op het apparaat?

- Is het apparaat compatibel?






## <a name="supported-browser"></a>Ondersteunde browser

Als de beheerder voorwaardelijk toegangsbeleid heeft geconfigureerd, hebt u alleen toegang tot de resources van uw organisatie vanaf een ondersteunde browser. Op een Windows-apparaat worden alleen **Internet Explorer** en **Edge** ondersteund.

U kunt eenvoudig vaststellen of u geen toegang hebt tot een resource vanwege een niet-ondersteunde browser door op de foutpagina naar de sectie Details te gaan:

![Scenario](./media/active-directory-conditional-access-device-remediation/02.png "Berichten over ontoegankelijke toepassingen voor niet-ondersteunde browsers")

De enige oplossing is een browser te gebruiken die de toepassing ondersteunt voor uw apparaatplatform. Zie [Ondersteunde browsers](active-directory-conditional-access-supported-apps.md) voor een volledige lijst met ondersteunde browsers.  


## <a name="supported-versions-of-windows"></a>Ondersteunde versies van Windows

Het volgende moet waar zijn over het Windows-besturingssysteem op uw apparaat: 

- Als u een Windows-desktopbesturingssysteem uitvoert op het apparaat, moet dit Windows 7 of later zijn.
- Als u een Windows-serverbesturingssysteem uitvoert op het apparaat, moet dit Windows Server 2008 R2 of later zijn. 


## <a name="compliant-device"></a>Compatibel apparaat

Mogelijk heeft de beheerder voorwaardelijk toegangsbeleid geconfigureerd op basis waarvan toegang tot de resources van uw organisatie alleen mogelijk is vanaf compatibele apparaten. Het apparaat moet zijn gekoppeld aan uw on-premises Active Directory of aan uw Azure Active Directory om compatibel te zijn.

U kunt eenvoudig vaststellen of u geen toegang hebt tot een resource vanwege een apparaat dat niet compatibel is, door op de foutpagina naar de sectie Details te gaan:
 
![Scenario](./media/active-directory-conditional-access-device-remediation/01.png "Berichten over ontoegankelijke toepassingen voor niet-geregistreerde apparaten")


### <a name="is-your-device-joined-to-an-on-premises-active-directory"></a>Is het apparaat gekoppeld aan een on-premises Active Directory?

**Als het apparaat is gekoppeld aan een on-premises Active Directory in uw organisatie:**

1. Zorg dat u bij Windows bent aangemeld met uw werkaccount (uw Active Directory-account).
2. Maak via een VPN (virtueel particulier netwerk) of DirectAccess verbinding met uw bedrijfsnetwerk.
3. Wanneer u verbinding hebt gemaakt, drukt u op de toets met het Windows-logo + L om uw Windows-sessie te vergrendelen.
4. Ontgrendel de Windows-sessie door de referenties voor uw werkaccount in te voeren.
5. Wacht even en probeer dan opnieuw of u de toepassing of service kunt openen.
6. Als dezelfde pagina wordt weergegeven, klikt u op de koppeling **Meer details** en neemt u met deze gegevens contact op met de beheerder.


### <a name="is-your-device-not-joined-to-an-on-premises-active-directory"></a>Is het apparaat niet gekoppeld aan een on-premises Active Directory?

Als het apparaat niet is gekoppeld aan een on-premises Active Directory en Windows 10 op het apparaat wordt uitgevoerd, hebt u twee opties:

* Azure AD Join uitvoeren
* Uw werk- of schoolaccount toevoegen aan Windows

Zie [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md) (Windows 10-apparaten gebruiken op uw werkplek) voor informatie over de verschillen tussen deze opties.  
Als het apparaat:

- Deel uitmaakt van uw organisatie, voert u Azure AD Join uit.
- Een persoonlijk apparaat of Windows Phone is, voegt u uw werk- of schoolaccount toe aan Windows 



#### <a name="azure-ad-join-on-windows-10"></a>Azure AD Join op Windows 10

De stappen voor het koppelen van een apparaat aan Azure AD zijn afhankelijk van de versie van Windows 10 die erop wordt uitgevoerd. Voer de opdracht **winver** uit om te bepalen welke versie van het Windows 10-besturingssysteem u hebt: 

![Windows-versie](./media/active-directory-conditional-access-device-remediation/03.png )


**Windows 10 Jubileumupdate (versie 1607)**

1. Start de app **Instellingen**.
2. Klik op **Accounts** > **Toegang via werk of school**.
3. Klik op **Verbinden**.
4. Klik op **Dit apparaat toevoegen aan Azure AD**.
5. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
6. Meld u af en meld u weer aan met uw werkaccount.
7. Probeer opnieuw toegang te krijgen tot de toepassing.

**Windows 10-update van november 2015 (versie 1511):**

1. Start de app **Instellingen**.
2. Klik op **Systeem** > **Info**.
3. Klik op **Lid worden van Azure AD**.
4. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
5. Meld u af en meld u weer aan met uw werkaccount (uw Azure AD-account).
6. Probeer opnieuw toegang te krijgen tot de toepassing.


#### <a name="workplace-join-on-windows-81"></a>Workplace Join op Windows 8.1

Als uw apparaat niet in een domein is opgenomen en Windows 8.1 op het apparaat wordt uitgevoerd, kunt u de volgende stappen uitvoeren om een Workplace Join uit te voeren en het apparaat in te schrijven bij Microsoft Intune:

1. Open **Pc-instellingen**.
2. Klik op **Netwerk** > **Werkplek**.
3. Klik op **Deelnemen**.
4. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
5. Klik op **Inschakelen**.
6. Probeer opnieuw toegang te krijgen tot de toepassing.



#### <a name="add-your-work-or-school-account-to-windows"></a>Uw werk- of schoolaccount toevoegen aan Windows 


**Windows 10 Jubileumupdate (versie 1607)**

1. Start de app **Instellingen**.
2. Klik op **Accounts** > **Toegang via werk of school**.
3. Klik op **Verbinden**.
4. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
5. Probeer opnieuw toegang te krijgen tot de toepassing.


**Windows 10-update van november 2015 (versie 1511):**

1. Start de app **Instellingen**.
2. Klik op **Accounts** > **Uw accounts**.
3. Klik op **Werk- of schoolaccount toevoegen**.
4. Verifieer uzelf bij uw organisatie, gebruik zo nodig Multi-Factor Authentication en voer vervolgens de stappen uit die hier worden weergegeven.
5. Probeer opnieuw toegang te krijgen tot de toepassing.





## <a name="next-steps"></a>Volgende stappen
[Voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access.md)

