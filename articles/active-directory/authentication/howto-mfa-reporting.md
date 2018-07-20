---
title: Toegang en gebruik rapporten voor Azure MFA | Microsoft Docs
description: Dit wordt beschreven hoe u de functie Azure multi-factor Authentication - rapporten.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: ead1c5a899057bb26154b45c75251e7d9e481147
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160889"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporten in de Azure multi-factor Authentication

Azure multi-factor Authentication biedt verschillende rapporten die kunnen worden gebruikt door u en uw organisatie die toegankelijk zijn via Azure portal. De volgende tabel bevat de beschikbare rapporten:

| Rapport | Locatie | Beschrijving |
|:--- |:--- |:--- |
| Geschiedenis geblokkeerde gebruikers | Azure AD > MFA-Server > gebruikers blokkeren/deblokkeren | Geeft de geschiedenis van aanvragen voor het blokkeren of deblokkeren van gebruikers. |
| Gebruik en fraude waarschuwingen | Azure AD >-aanmeldingen | Bevat informatie over algemene gebruik, overzicht van gebruikers en de details van de gebruiker; Als een geschiedenis van Fraudewaarschuwingen die zijn ingediend in het opgegeven datumbereik. |
| Gebruik voor on-premises onderdelen | Azure AD > MFA-Server > activiteitenrapport | Bevat informatie over algemene gebruik voor MFA via de NPS-extensie, ADFS, en de MFA-server. |
| Geschiedenis overgeslagen gebruikers | Azure AD > MFA-Server > eenmalige bypass | Biedt een geschiedenis van aanvragen voor het overslaan van multi-factor Authentication voor een gebruiker. |
| Serverstatus | Azure AD > MFA-Server > status van de Server | Hiermee wordt de status van multi-factor Authentication-Servers die zijn gekoppeld aan uw account. |

## <a name="view-reports"></a>Rapporten weergeven 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **MFA-Server**.
3. Selecteer het rapport dat u wilt weergeven.

   <center>![Cloud](./media/howto-mfa-reporting/report.png)</center>

## <a name="powershell-reporting"></a>Melden van PowerShell

Identificeer gebruikers die zich hebben geregistreerd voor MFA met behulp van PowerShell die volgt.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Gebruikers identificeren die niet hebt geregistreerd voor MFA met behulp van PowerShell die volgt.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Volgende stappen

* [Voor gebruikers](../user-help/multi-factor-authentication-end-user.md)
* [Waar u wilt implementeren](concept-mfa-whichversion.md)
