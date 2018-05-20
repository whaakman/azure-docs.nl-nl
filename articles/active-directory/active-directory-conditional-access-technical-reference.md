---
title: Azure Active Directory-naslaginformatie voor voorwaardelijke toegang | Microsoft Docs
description: Hiermee krijgt u een overzicht van de ondersteunde instellingen in een beleid voor voorwaardelijke toegang van Azure Active Directory.
services: active-directory.
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 341142222fb1440cf5118e6d10f5a254e73d53d9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="azure-active-directory-conditional-access-settings-reference"></a>Azure Active Directory-naslaginformatie voor voorwaardelijke toegang

U kunt [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) om te bepalen hoe geautoriseerde gebruikers kunnen toegang tot uw resources.   

Dit artikel vindt u informatie over ondersteuning voor de volgende configuratieopties in een beleid voor voorwaardelijke toegang: 

- Toewijzingen voor cloud-toepassingen

- Apparaat platform voorwaarde 

- Voorwaarde voor client-toepassingen

- Vereisten voor toepassingsimplementatie goedgekeurde client


Als dit niet de informatie die u zoekt, laat u een opmerking aan het einde van dit artikel.

## <a name="cloud-apps-assignments"></a>Toewijzingen voor cloud-apps

Met beleid voor voorwaardelijke toegang u bepalen hoe uw gebruikers toegang tot uw [cloud-apps](active-directory-conditional-access-conditions.md#cloud-apps). Wanneer u een beleid voor voorwaardelijke toegang configureert, moet u ten minste één cloud-app selecteren. 

![Selecteer de cloud-apps voor uw beleid](./media/active-directory-conditional-access-technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Microsoft cloud-toepassingen

U kunt beleid voor voorwaardelijke toegang toewijzen aan de volgende cloud-apps van Microsoft:

- Azure Information Protection - [meer informatie](https://docs.microsoft.com/information-protection/get-started/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)

- Azure RemoteApp

- Microsoft Dynamics 365

- Microsoft Office 365 Yammer

- Microsoft Office 365 Exchange Online

- Microsoft Office 365 SharePoint Online (inclusief OneDrive voor bedrijven en Project Online)

- Microsoft Power BI 

- Microsoft Visual Studio teamservices

- Microsoft Teams


### <a name="other-applications"></a>Andere toepassingen 

Naast de Microsoft cloud-apps kunt u een voorwaardelijk toegangsbeleid toewijzen aan de volgende typen van cloud-apps:

- Azure AD-toepassingen

- Vooraf geïntegreerde federatieve toepassing software als een dienst (SaaS)

- Toepassingen die gebruikmaken van wachtwoord eenmalige aanmelding (SSO)

- Line-of-business-toepassingen

- Toepassingen die gebruikmaken van Azure AD-toepassingsproxy


## <a name="device-platform-condition"></a>Apparaat platform voorwaarde

U kunt de voorwaarde voor het platform van apparaat voor het koppelen van het beleid voor het besturingssysteem op een client te configureren in een beleid voor voorwaardelijke toegang. Voorwaardelijke toegang van Azure AD ondersteunt de volgende apparaatplatforms:

- Android

- iOS

- Windows Phone

- Windows

- macOS


![Het toegangsbeleid naar de client OS koppelen](./media/active-directory-conditional-access-technical-reference/41.png)





## <a name="client-apps-condition"></a>Voorwaarde voor client-apps 

In uw beleid voor voorwaardelijke toegang configureert u de [client-apps](active-directory-conditional-access-conditions.md#client-apps) voorwaarde voor het koppelen van het beleid naar de clientapp die een toegangspoging heeft gestart. De client-apps probleem verlenen of toegang blokkeren als er een toegangspoging wordt uitgevoerd vanaf de volgende typen van de client-apps instellen:

- Browser
- Mobiele apps en bureaublad-apps

![Toegang beheren voor client-apps](./media/active-directory-conditional-access-technical-reference/03.png)

### <a name="supported-browsers"></a>Ondersteunde browsers 

In uw beleid voor voorwaardelijke toegang, kunt u **Browsers** als client-app.

![Toegang beheren voor ondersteunde browsers](./media/active-directory-conditional-access-technical-reference/05.png)

Deze instelling werkt met alle browsers. Echter, om te voldoen aan een apparaatbeleid, zoals een vereiste compatibel apparaat de volgende besturingssystemen en browsers worden ondersteund:


| OS                     | Browsers                            | Ondersteuning     |
| :--                    | :--                                 | :-:         |
| Windows 10             | Internet Explorer, rand Chrome     | ![Selecteren][1] |
| Windows 8 / 8.1        | Internet Explorer, Chrome           | ![Selecteren][1] |
| Windows 7              | Internet Explorer, Chrome           | ![Selecteren][1] |
| iOS                    | Safari, Intune Managed Browser      | ![Selecteren][1] |
| Android                | Chrome, Intune Managed Browser      | ![Selecteren][1] |
| Windows Phone          | Internet Explorer rand             | ![Selecteren][1] |
| Windows Server 2016    | Internet Explorer rand             | ![Selecteren][1] |
| Windows Server 2016    | Chrome                              | Binnenkort beschikbaar |
| Windows Server 2012 R2 | Internet Explorer, Chrome           | ![Selecteren][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome           | ![Selecteren][1] |
| macOS                  | Chrome, Safari                      | ![Selecteren][1] |



#### <a name="chrome-support"></a>Chrome-ondersteuning

Ondersteuning voor Chrome in **Windows 10 auteurs Update (versie 1703)** of later installeren [deze extensie](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

Om automatisch te implementeren voor deze extensie aan Chrome-browser, maken de volgende registersleutel:

|    |    |
|--- | ---|
|Pad | HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\ExtensionInstallForcelist |
|Naam | 1 |
|Type | REG_SZ (tekenreeks) |
|Gegevens | ppnbnpeolgkicgegkbkbjmhlideopiji;https://clients2.google.com/service/update2/crx

Ondersteuning voor Chrome in **Windows 8.1 en 7**, maken de volgende registersleutel:

|    |    |
|--- | ---|
|Pad | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
|Naam | 1 |
|Type | REG_SZ (tekenreeks) |
|Gegevens | {'patroon': 'https://device.login.microsoftonline.com',' filter': {"VERLENER": {'CN': 'MS organisatie toegang'}}}|

Deze browsers ondersteuning voor verificatie van apparaten, waardoor het apparaat kan worden geïdentificeerd en gevalideerd op basis van een beleid. De apparaat-controle mislukt als de browser wordt uitgevoerd in de privé-modus. 


### <a name="supported-mobile-applications-and-desktop-clients"></a>Ondersteunde mobiele toepassingen en bureaublad-clients

In uw beleid voor voorwaardelijke toegang, kunt u **mobiele apps en bureaublad-clients** als client-app.


![Toegang beheren voor ondersteunde mobiele apps of bureaubladclients](./media/active-directory-conditional-access-technical-reference/06.png)


Deze instelling heeft gevolgen op toegangspogingen tot van de volgende apps voor mobiele en bureaublad-clients: 


|Client-apps|Target-Service|Platform|
|---|---|---|
|Azure RemoteApp|Azure RemoteApp-service|Windows 10, Windows 8.1, Windows 7, iOS, Android en Mac OS X|
|Dynamics CRM-app|Dynamics CRM|Windows 10, Windows 8.1, iOS en Android|
|Agenda-mail/mensen app, Outlook 2016 Outlook 2013 (met moderne verificatie)|Office 365 Exchange Online|Windows 10|
|Beleid voor apps MFA en locatie. Apparaten op basis van beleid worden niet ondersteund. |Alle services van de app mijn Apps|Android en iOS|
|Services van Microsoft-Teams - Hiermee bepaalt u alle services die ondersteuning bieden voor Microsoft-Teams en alle bijbehorende Client-Apps - Windows-bureaublad, iOS, Android, WP en webclient|Microsoft Teams|Windows 10, Windows 8.1, Windows 7, iOS, Android en Mac OS |
|Apps van Office 2016, Office 2013 (met moderne verificatie), OneDrive synchroniseren client (Zie [notities](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|Office 365 SharePoint Online|Windows 8.1, Windows 7|
|Apps van Office 2016, Office Universal-apps, Office 2013 (met moderne verificatie), OneDrive sync-client (Zie [notities](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), Office-groepen ondersteuning voor de toekomst zijn gepland, ondersteuning voor SharePoint-app is gepland voor de toekomst|Office 365 SharePoint Online|Windows 10|
|Office 2016 voor Mac OS (Word, Excel, PowerPoint, OneNote alleen). OneDrive voor bedrijven ondersteuning voor de toekomst worden gepland|Office 365 SharePoint Online|Mac OS X|
|Office mobile-apps|Office 365 SharePoint Online|Android, iOS|
|De app Yammer Office|Office 365 Yammer|Windows 10, iOS, Android|
|Outlook 2016 (Office voor Mac OS)|Office 365 Exchange Online|Mac OS X|
|Outlook 2016, Outlook 2013 (met moderne verificatie), Skype voor bedrijven (met moderne verificatie)|Office 365 Exchange Online|Windows 8.1, Windows 7|
|Mobiele app voor Outlook|Office 365 Exchange Online|Android, iOS|
|Power BI-app|Power BI-service|Windows 10, Windows 8.1, Windows 7, Android en iOS|
|Skype voor Bedrijven|Office 365 Exchange Online|Android-, IOS |
|Visual Studio Team Services-app|Visual Studio Team Services|Windows 10, Windows 8.1, Windows 7, iOS en Android|



## <a name="approved-client-app-requirement"></a>Goedgekeurde app vereiste 

U kunt in uw beleid voor voorwaardelijke toegang vereisen dat een toegang wilt de geselecteerde cloud-apps moet worden gemaakt van een goedgekeurde client-app. 

![Toegang beheren voor goedgekeurde client-apps](./media/active-directory-conditional-access-technical-reference/21.png)

Deze instelling geldt voor de volgende ClientApps:


- Microsoft Intune Managed Browser
- Microsoft PowerBI
- Microsoft facturering
- Microsoft Launcher
- Microsoft Azure Information Protection
- Microsoft Excel
- Microsoft Kaizala 
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype voor bedrijven
- Microsoft StaffHub
- Microsoft Teams
- Microsoft Visio
- Microsoft Word



**Opmerkingen**

- De goedgekeurde apps ondersteuning voor de Intune mobile application management-functie.

- De **goedgekeurde app vereist** vereiste:

    - Biedt alleen ondersteuning voor het iOS- en Android voor [apparaat platform voorwaarde](#device-platforms-condition).


## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van voorwaardelijke toegang [voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md).
- Als u klaar bent voor het configureren van beleidsregels voor voorwaardelijke toegang in uw omgeving, raadpleegt u de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md).



<!--Image references-->
[1]: ./media/active-directory-conditional-access-technical-reference/01.png


