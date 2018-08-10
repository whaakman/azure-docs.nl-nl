---
title: Azure Active Directory-naslaginformatie voor voorwaardelijke toegang | Microsoft Docs
description: Bekijk een overzicht van de ondersteunde instellingen in een Azure Active Directory-beleid voor voorwaardelijke toegang.
services: active-directory.
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 0055968e4333f5ac7653a37fcd728b040024ee55
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39629844"
---
# <a name="azure-active-directory-conditional-access-settings-reference"></a>Azure Active Directory-instellingen voor voorwaardelijke toegang verwijst naar

U kunt [voorwaardelijke toegang van Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md) om te bepalen hoe gemachtigde gebruikers toegang hebben tot uw resources.   

Dit artikel vindt u informatie over ondersteuning voor de volgende configuratieopties in een beleid voor voorwaardelijke toegang: 

- Toewijzingen voor cloud-toepassingen

- Apparaat platform voorwaarde 

- Voorwaarde voor client-toepassingen

- Vereiste voor goedgekeurde client-toepassing


Als dit niet de informatie die u zoekt, plaatst u een opmerking aan het einde van dit artikel.

## <a name="cloud-apps-assignments"></a>Toewijzingen voor cloud-apps

Met beleid voor voorwaardelijke toegang u bepalen hoe uw gebruikers toegang tot uw [cloud-apps](conditions.md#cloud-apps). Wanneer u een beleid voor voorwaardelijke toegang configureert, moet u ten minste één cloud-app te selecteren. 

![De cloud-apps voor uw beleid selecteren](./media/technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Microsoft cloud-toepassingen

U kunt een beleid voor voorwaardelijke toegang toewijzen aan de volgende cloud-apps van Microsoft:

- Azure Information Protection - [meer informatie](/azure/information-protection/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)

- Azure RemoteApp

- Microsoft Dynamics 365

- Microsoft Office 365 Yammer

- Microsoft Office 365 Exchange Online

- Microsoft Office 365 SharePoint Online (OneDrive voor bedrijven en Project Online bevat)

- Microsoft Power BI 

- Microsoft Visual Studio teamservices

- Microsoft Teams


### <a name="other-applications"></a>Andere toepassingen 

Naast de Microsoft cloud-apps, kunt u een beleid voor voorwaardelijke toegang toewijzen aan de volgende typen van cloud-apps:

- Azure AD-toepassingen

- Vooraf geïntegreerde federatieve software als een dienst (SaaS)-toepassing

- Toepassingen die gebruikmaken van wachtwoord eenmalige aanmelding (SSO)

- Line-of-business-toepassingen

- Toepassingen die gebruikmaken van Azure AD-toepassingsproxy


## <a name="device-platform-condition"></a>Apparaat platform voorwaarde

U kunt de apparaat-platform voorwaarde om te koppelen van het beleid voor het besturingssysteem op een client configureren in een beleid voor voorwaardelijke toegang. Voorwaardelijke toegang van Azure AD ondersteunt de volgende apparaatplatformen:

- Android

- iOS

- Windows Phone

- Windows

- macOS


![Het toegangsbeleid voor de client-besturingssysteem koppelen](./media/technical-reference/41.png)





## <a name="client-apps-condition"></a>Voorwaarde voor client-apps 

In het beleid voor voorwaardelijke toegang, configureert u de [client-apps](conditions.md#client-apps) voorwaarde om te koppelen van het beleid naar de clientapp die een poging tot toegang heeft gestart. Stel de client apps voorwaarde om te verlenen of toegang blokkeren als er een poging tot toegang bestaat uit de volgende typen van de client-apps:

- Browser
- Mobiele apps en bureaublad-apps

![Toegangsbeheer voor client-apps](./media/technical-reference/03.png)

### <a name="supported-browsers"></a>Ondersteunde browsers 

In het beleid voor voorwaardelijke toegang, kunt u **Browsers** als client-app.

![Toegangsbeheer voor ondersteunde browsers](./media/technical-reference/05.png)

Deze instelling werkt met alle browsers. Echter, om te voldoen aan een apparaatbeleid, zoals een vereiste compatibel apparaat, de volgende besturingssystemen en browsers worden ondersteund:


| OS                     | Browsers                            | Ondersteuning     |
| :--                    | :--                                 | :-:         |
| Windows 10             | Internet Explorer, Edge, Chrome     | ![Selecteren][1] |
| Windows 8 / 8.1        | Internet Explorer, Chrome           | ![Selecteren][1] |
| Windows 7              | Internet Explorer, Chrome           | ![Selecteren][1] |
| iOS                    | Safari, Intune Managed Browser      | ![Selecteren][1] |
| Android                | Chrome, Intune Managed Browser      | ![Selecteren][1] |
| Windows Phone          | Internet Explorer, Edge             | ![Selecteren][1] |
| Windows Server 2016    | Internet Explorer, Edge             | ![Selecteren][1] |
| Windows Server 2016    | Chrome                              | Binnenkort beschikbaar |
| Windows Server 2012 R2 | Internet Explorer, Chrome           | ![Selecteren][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome           | ![Selecteren][1] |
| macOS                  | Chrome, Safari                      | ![Selecteren][1] |



#### <a name="chrome-support"></a>Ondersteuning voor chrome

Ondersteuning voor Chrome in **Windows 10-Makersupdate (versie 1703)** of later installeren [deze extensie](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

Voor het automatisch implementeren met deze extensie voor Chrome browsers, maken de volgende registersleutel:

|    |    |
|--- | ---|
|Pad | HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\ExtensionInstallForcelist |
|Naam | 1 |
|Type | REG_SZ (tekenreeks) |
|Gegevens | ppnbnpeolgkicgegkbkbjmhlideopiji; https://clients2.google.com/service/update2/crx

Ondersteuning voor Chrome in **Windows 8.1 en 7**, maken de volgende registersleutel:

|    |    |
|--- | ---|
|Pad | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
|Naam | 1 |
|Type | REG_SZ (tekenreeks) |
|Gegevens | {"patroon": "https://device.login.microsoftonline.com',' filter': {"VERLENER": {"CN":"MS-organisatie-toegang"}}}|

Deze browsers ondersteunen verificatie van apparaten, waardoor het apparaat kan worden geïdentificeerd en gevalideerd op basis van een beleid. De apparaat-controle mislukt als de browser wordt uitgevoerd in de privémodus. 


### <a name="supported-mobile-applications-and-desktop-clients"></a>Ondersteunde mobiele toepassingen en bureaublad-clients

In het beleid voor voorwaardelijke toegang, kunt u **mobiele apps en bureaubladclients** als client-app.


![Toegangsbeheer voor ondersteunde mobiele apps of bureaublad-clients](./media/technical-reference/06.png)


Deze instelling heeft een invloed op een poging toegang van de volgende mobiele apps en bureaublad-clients: 


|Client-apps|Target-Service|Platform|
|---|---|---|
|Azure RemoteApp|Azure RemoteApp-service|Windows 10, Windows 8.1, Windows 7, iOS, Android, en Mac OS X|
|Dynamics CRM-app|Dynamics CRM|Windows 10, Windows 8.1, iOS en Android|
|Agenda-e-mail/mensen app, Outlook 2016, Outlook 2013 (met moderne verificatie)|Office 365 Exchange Online|Windows 10|
|Beleid voor meervoudige verificatie en locatiebeleid voor apps. Op basis van apparaatbeleid wordt niet ondersteund. |Elke mijn Apps van appservice|Android en iOS|
|Microsoft Teams-Services: Hiermee bepaalt u alle services die ondersteuning bieden voor Microsoft Teams en alle bijbehorende Client-Apps - Windows-bureaublad, iOS, Android, WP en webclient|Microsoft Teams|Windows 10, Windows 8.1, Windows 7, iOS, Android en macOS |
|Apps van Office 2016, Office 2013 (met moderne verificatie), OneDrive synchroniseren client (Zie [opmerkingen bij de](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|Office 365 SharePoint Online|Windows 8.1, Windows 7|
|Apps van Office 2016, universele Office-apps, Office 2013 (met moderne verificatie), OneDrive-synchronisatieclient (Zie [opmerkingen bij de](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), ondersteuning voor Office-groepen is gepland voor de toekomst, ondersteuning voor SharePoint-app is gepland voor de toekomst|Office 365 SharePoint Online|Windows 10|
|Office 2016 voor Mac OS (Word, Excel, PowerPoint, OneNote alleen). OneDrive voor bedrijven ondersteuning voor de toekomst worden gepland|Office 365 SharePoint Online|Mac OS X|
|Mobiele Office-apps|Office 365 SharePoint Online|Android, iOS|
|Office Yammer-app|Office 365 Yammer|Windows 10, iOS, Android|
|Outlook 2016 (Office voor Mac OS)|Office 365 Exchange Online|Mac OS X|
|Outlook 2016, Outlook 2013 (met moderne verificatie), Skype voor bedrijven (met moderne verificatie)|Office 365 Exchange Online|Windows 8.1, Windows 7|
|Mobiele app van Outlook|Office 365 Exchange Online|Android, iOS|
|Power BI-app|Power BI-service|Windows 10, Windows 8.1, Windows 7, Android en iOS|
|Skype voor Bedrijven|Office 365 Exchange Online|Android, IOS |
|Visual Studio Team Services-app|Visual Studio Team Services|Windows 10, Windows 8.1, Windows 7, iOS en Android|



## <a name="approved-client-app-requirement"></a>Vereiste voor goedgekeurde client-app 

U kunt vereisen dat een toegang willen de geselecteerde cloud-apps moet worden uitgevoerd vanaf een goedgekeurde client-app in uw beleid voor voorwaardelijke toegang. 

![Toegangsbeheer voor goedgekeurde client-apps](./media/technical-reference/21.png)

Deze instelling geldt voor de volgende client-apps:


- Microsoft Intune Managed Browser
- Microsoft PowerBI
- Facturering van Microsoft
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

- De goedgekeurde client-apps ondersteunen de Intune mobile application management-functie.

- De **goedgekeurde client-app vereisen** vereiste:

    - Biedt alleen ondersteuning voor iOS en Android for [apparaat platform voorwaarde](#device-platforms-condition).


## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van voorwaardelijke toegang, [wat is voorwaardelijke toegang in Azure Active Directory?](../active-directory-conditional-access-azure-portal.md)
- Als u klaar om te configureren van beleid voor voorwaardelijke toegang in uw omgeving bent, raadpleegt u de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md).



<!--Image references-->
[1]: ./media/technical-reference/01.png


