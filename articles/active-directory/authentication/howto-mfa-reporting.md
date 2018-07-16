---
title: Toegang en gebruik rapporten voor Azure MFA | Microsoft Docs
description: Dit wordt beschreven hoe u de functie Azure multi-factor Authentication - rapporten.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 146a86058adc73626e532f33e9fdbc83d9cf27e8
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048984"
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
