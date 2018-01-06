---
title: Toegang en gebruik rapporten voor Azure MFA | Microsoft Docs
description: Dit wordt beschreven hoe u de functie Azure multi-factor Authentication - rapporten.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 696f4ae3cb479a208e73e53a9a9a437caeabd294
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporten in Azure multi-factor Authentication

Azure multi-factor Authentication bevat verschillende rapporten die kunnen worden gebruikt door u en uw organisatie die toegankelijk zijn via de Azure-portal. De volgende tabel bevat de beschikbare rapporten:

| Rapport | Locatie | Beschrijving |
|:--- |:--- |:--- |
| Geschiedenis geblokkeerde gebruikers | Azure AD > MFA-Server > gebruikers blokkeren/blokkering opheffen | Toont de geschiedenis van aanvragen voor het blokkeren of deblokkeren van gebruikers. |
| Gebruiks- en fraude waarschuwingen | Azure AD > aanmeldingen | Bevat informatie over algemene gebruik Gebruikersoverzicht en gebruikersdetails; Als een geschiedenis van Fraudewaarschuwingen die zijn ingediend in het opgegeven datumbereik. |
| Gebruik voor on-premises onderdelen | Azure AD > MFA-Server > activiteitenrapport | Bevat informatie over algemene gebruik voor MFA via de uitbreiding NPS, AD FS, en de MFA-server. |
| Geschiedenis overgeslagen gebruikers | Azure AD > MFA-Server > eenmalig overslaan | Geeft een geschiedenis van aanvragen voor het overslaan van multi-factor Authentication voor een gebruiker. |
| Status van de server | Azure AD > MFA-Server > status van de Server | Geeft de status van de multi-factor Authentication-Servers die zijn gekoppeld aan uw account. |

## <a name="view-reports"></a>Rapporten weergeven 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **MFA-Server**.
3. Selecteer het rapport dat u wilt weergeven.

   <center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Melden van PowerShell

Gebruikers die zich hebben geregistreerd voor MFA met behulp van de volgende PowerShell identificeren.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Gebruikers identificeren die nog niet geregistreerd voor MFA met behulp van de volgende PowerShell.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Volgende stappen

* [Voor gebruikers](end-user/multi-factor-authentication-end-user.md)
* [Waar u wilt implementeren](multi-factor-authentication-get-started.md)
