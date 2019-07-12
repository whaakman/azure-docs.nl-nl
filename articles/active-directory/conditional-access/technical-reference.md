---
title: Naslaginformatie over Azure Active Directory voor voorwaardelijke toegang | Microsoft Docs
description: Bekijk een overzicht van de ondersteunde instellingen in een beleid voor Azure Active Directory voor voorwaardelijke toegang.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 07/10/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 535842989ef49ee13a5ddee7c4349a3b819f741c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797847"
---
# <a name="azure-active-directory-conditional-access-settings-reference"></a>Naslaginformatie over Azure Active Directory voor voorwaardelijke toegang

U kunt [voorwaardelijke toegang van Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md) om te bepalen hoe gemachtigde gebruikers toegang hebben tot uw resources.

Dit artikel vindt u informatie over ondersteuning voor de volgende configuratieopties in een beleid voor voorwaardelijke toegang:

- Toewijzingen voor cloud-toepassingen
- Apparaat platform voorwaarde
- Voorwaarde voor client-toepassingen
- Vereiste voor goedgekeurde client-toepassing

Als dit niet de informatie die u zoekt, plaatst u een opmerking aan het einde van dit artikel.

## <a name="cloud-apps-assignments"></a>Toewijzingen voor cloud-apps

Met beleid voor voorwaardelijke toegang u bepalen hoe uw gebruikers toegang tot uw [cloud-apps](conditions.md#cloud-apps-and-actions). Wanneer u een beleid voor voorwaardelijke toegang configureert, moet u ten minste één cloud-app te selecteren. 

![De cloud-apps voor uw beleid selecteren](./media/technical-reference/09.png)

### <a name="microsoft-cloud-applications"></a>Microsoft cloud-toepassingen

U kunt een beleid voor voorwaardelijke toegang toewijzen aan de volgende cloud-apps van Microsoft:

- Azure Analysis Services
- Azure DevOps
- Azure SQL Database en datawarehouse - [meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-conditional-access)
- Dynamics CRM Online
- Microsoft Application Insights Analytics
- Microsoft Azure Information Protection - [meer informatie](https://docs.microsoft.com/azure/information-protection/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)
- Microsoft Azure Management - [meer informatie](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)
- Beheer van Microsoft Azure-abonnementen
- Microsoft Cloud App Security
- Microsoft Commerce hulpprogramma's van Access Control-beheerportal
- Authentication-Service van Microsoft Commerce-hulpprogramma 's
- Microsoft Flow
- Microsoft Forms
- Microsoft Intune
- Microsoft Intune-inschrijving
- Microsoft Planner
- Microsoft Power BI
- Microsoft PowerApps
- Microsoft zoeken in Bing
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Office 365 Exchange Online
- Office 365 SharePoint Online
- Office 365 Yammer
- Office Delve
- Office Sway
- Outlook Groups
- Project Online
- Skype voor Bedrijven Online
- Virtueel particulier netwerk (VPN)
- Visual Studio App Center
- Windows Defender ATP

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

| OS                     | Browsers                                      |
| :--                    | :--                                           |
| Windows 10             | Internet Explorer, Microsoft Edge, Chrome     |
| Windows 8 / 8.1        | Internet Explorer, Chrome                     |
| Windows 7              | Internet Explorer, Chrome                     |
| iOS                    | Safari, Microsoft Edge, Intune Managed Browser |
| Android                | Chrome, Microsoft Edge, Intune Managed Browser |
| Windows Phone          | Internet Explorer, Microsoft Edge             |
| Windows Server 2016    | Internet Explorer, Microsoft Edge             |
| Windows Server 2016    | Chrome                                        |
| Windows Server 2012 R2 | Internet Explorer, Chrome                     |
| Windows Server 2008 R2 | Internet Explorer, Chrome                     |
| macOS                  | Chrome, Safari                                |

#### <a name="why-do-i-see-a-certificate-prompt-in-the-browser"></a>Waarom zie ik een certificaat in de browser wordt gevraagd

Op Windows 7, iOS, Android en macOS identificeert Azure AD het apparaat met behulp van een clientcertificaat dat is ingericht als het apparaat is geregistreerd bij Azure AD.  Wanneer een gebruiker zich eerst via de browser wordt de gebruiker gevraagd om het certificaat te selecteren. De gebruiker moet dit certificaat selecteren voordat u de browser.

#### <a name="chrome-support"></a>Ondersteuning voor chrome

Ondersteuning voor Chrome in **Windows 10-Makersupdate (versie 1703)** of hoger, installeert u de [Windows 10-Accounts extensie](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji). Deze extensie is vereist wanneer een beleid voor voorwaardelijke toegang is vereist voor apparaat-specifieke details.

Voor het automatisch implementeren met deze extensie voor Chrome browsers, maken de volgende registersleutel:

|    |    |
| --- | --- |
| Path | HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\ExtensionInstallForcelist |
| Name | 1 |
| type | REG_SZ (String) |
| Data | ppnbnpeolgkicgegkbkbjmhlideopiji; https://clients2.google.com/service/update2/crx |

Ondersteuning voor Chrome in **Windows 8.1 en 7**, maken de volgende registersleutel:

|    |    |
| --- | --- |
| Path | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
| Name | 1 |
| type | REG_SZ (String) |
| Data | {"patroon": "https://device.login.microsoftonline.com ',' filter': {"VERLENER": {"CN":"MS-organisatie-toegang"}}} |

Deze browsers ondersteunen verificatie van apparaten, waardoor het apparaat kan worden geïdentificeerd en gevalideerd op basis van een beleid. De apparaat-controle mislukt als de browser wordt uitgevoerd in de privémodus.

### <a name="supported-mobile-applications-and-desktop-clients"></a>Ondersteunde mobiele toepassingen en bureaublad-clients

In het beleid voor voorwaardelijke toegang, kunt u **mobiele apps en bureaubladclients** als client-app.

![Toegangsbeheer voor ondersteunde mobiele apps of bureaublad-clients](./media/technical-reference/06.png)

Deze instelling heeft een invloed op een poging toegang van de volgende mobiele apps en bureaublad-clients:

| Client-apps | Target-Service | Platform |
| --- | --- | --- |
| Dynamics CRM-app | Dynamics CRM | Windows 10, Windows 8.1, iOS en Android |
| Agenda-e-mail/mensen app, Outlook 2016, Outlook 2013 (met moderne verificatie)| Office 365 Exchange Online | Windows 10 |
| Beleid voor meervoudige verificatie en locatiebeleid voor apps. Op basis van apparaatbeleid wordt niet ondersteund.| Elke mijn Apps van appservice| Android en iOS |
| Microsoft Teams-Services: Hiermee bepaalt u alle services die ondersteuning bieden voor Microsoft Teams en alle bijbehorende Client-Apps - Windows-bureaublad, iOS, Android, WP en webclient | Microsoft Teams | Windows 10, Windows 8.1, Windows 7, iOS, Android en macOS |
| Apps van Office 2016, Office 2013 (met moderne verificatie), OneDrive synchroniseren client (Zie [opmerkingen bij de](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)) | Office 365 SharePoint Online | Windows 8.1, Windows 7 |
| Apps van Office 2016, universele Office-apps, Office 2013 (met moderne verificatie), OneDrive-synchronisatieclient (Zie [opmerkingen bij de](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), ondersteuning voor Office-groepen is gepland voor de toekomst, ondersteuning voor SharePoint-app is gepland voor de toekomst | Office 365 SharePoint Online | Windows 10 |
| Office 2016 (Word, Excel, PowerPoint en OneNote alleen). OneDrive voor bedrijven ondersteuning voor de toekomst worden gepland| Office 365 SharePoint Online| macOS|
| Office 2019| Office 365 SharePoint Online | Windows 10, macOS |
| Mobiele Office-apps | Office 365 SharePoint Online | Android, iOS |
| Office Yammer-app | Office 365 Yammer | Windows 10, iOS, Android |
| Outlook 2019 | Office 365 SharePoint Online | Windows 10, macOS |
| Outlook 2016 (Office voor Mac OS) | Office 365 Exchange Online | macOS |
| Outlook 2016, Outlook 2013 (met moderne verificatie), Skype voor bedrijven (met moderne verificatie) | Office 365 Exchange Online | Windows 8.1, Windows 7 |
| Mobiele app van Outlook | Office 365 Exchange Online | Android, iOS |
| Power BI-app | Power BI-service | Windows 10, Windows 8.1, Windows 7, Android en iOS |
| Skype voor Bedrijven | Office 365 Exchange Online| Android, IOS |
| Visual Studio Team Services-app | Visual Studio Team Services | Windows 10, Windows 8.1, Windows 7, iOS en Android |

## <a name="support-for-legacy-authentication"></a>Ondersteuning voor legacy-verificatie

Door het selecteren van **andere clients**, kunt u een voorwaarde die van invloed is op apps die gebruikmaken van basisverificatie met e-mailprotocollen zoals IMAP, MAPI-, pop-server, SMTP en oudere Office-apps die moderne verificatie wordt niet gebruikt.  

![Andere clients](./media/technical-reference/11.png)

Zie voor meer informatie, [Client-apps](conditions.md#client-apps).

## <a name="approved-client-app-requirement"></a>Vereiste voor goedgekeurde client-app

U kunt vereisen dat een toegang willen de geselecteerde cloud-apps moet worden uitgevoerd vanaf een goedgekeurde client-app in uw beleid voor voorwaardelijke toegang. 

![Toegangsbeheer voor goedgekeurde client-apps](./media/technical-reference/21.png)

Deze instelling geldt voor de volgende client-apps:

- Microsoft Azure Information Protection
- Microsoft Bookings
- Microsoft Edge
- Microsoft Excel
- Microsoft Flow
- Microsoft Intune Managed Browser
- Facturering van Microsoft
- Microsoft Kaizala
- Microsoft Launcher
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerApps
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype voor Bedrijven
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Microsoft To-Do
- Microsoft Visio
- Microsoft Word
- Microsoft Yammer

**Opmerkingen**

- De goedgekeurde client-apps ondersteunen de Intune mobile application management-functie.
- De **goedgekeurde client-app vereisen** vereiste:
   - Biedt alleen ondersteuning voor iOS en Android for [apparaat platform voorwaarde](#device-platform-condition).

## <a name="app-protection-policy-requirement"></a>De vereiste van het beleid voor App-beveiliging 

U kunt in uw beleid voor voorwaardelijke toegang vereisen dat een beveiligingsbeleid voor apps zijn aanwezig zijn op de client-app voordat toegang beschikbaar voor de geselecteerde cloud-apps is. 

![Toegang beheren met app-beveiligingsbeleid](./media/technical-reference/22.png)

Deze instelling geldt voor de volgende client-apps:

- Microsoft OneDrive
- Microsoft Outlook

**Opmerkingen**

- Apps voor app-beveiligingsbeleid ondersteunen de Intune mobile application management-functie met beleid voor beveiliging.
- De **vereisen app-beveiligingsbeleid** vereisten:
    - Biedt alleen ondersteuning voor iOS en Android for [apparaat platform voorwaarde](#device-platform-condition).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van voorwaardelijke toegang, [wat is voorwaardelijke toegang in Azure Active Directory?](../active-directory-conditional-access-azure-portal.md)
- Als u klaar om te configureren van beleid voor voorwaardelijke toegang in uw omgeving bent, raadpleegt u de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md).

<!--Image references-->
[1]: ./media/technical-reference/01.png
