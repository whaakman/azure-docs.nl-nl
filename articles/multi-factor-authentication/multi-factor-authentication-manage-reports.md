---
title: Toegang en gebruik rapporten voor Azure MFA | Microsoft Docs
description: Dit wordt beschreven hoe u de functie Azure multi-factor Authentication - rapporten.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: joflore
ms.reviewer: alexwe
ms.openlocfilehash: 77d6742faadfaf3d7afccfbe888b910c80278737
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporten in Azure multi-factor Authentication

Azure multi-factor Authentication bevat verschillende rapporten die kunnen worden gebruikt door u en uw organisatie. Deze rapporten zijn toegankelijk via de multi-factor Authentication-beheerportal. De volgende tabel bevat de beschikbare rapporten:

| Rapport | Beschrijving |
|:--- |:--- |
| Gebruik |Het gebruik rapporten informatie weergeven over algemene gebruik Gebruikersoverzicht en details van gebruiker. |
| Status van de server |Dit rapport geeft de status van de multi-factor Authentication-Servers die zijn gekoppeld aan uw account. |
| Geschiedenis van geblokkeerde gebruikers |Deze rapporten tonen de geschiedenis van aanvragen voor het blokkeren of deblokkeren van gebruikers. |
| Geschiedenis van overgeslagen gebruikers |Geeft de geschiedenis van aanvragen voor het overslaan van multi-factor Authentication voor het telefoonnummer van een gebruiker. |
| Fraudewaarschuwing |Geeft een historie weer van Fraudewaarschuwingen die zijn ingediend in het datumbereik dat u hebt opgegeven. |
| In de wachtrij |Een lijst met rapporten in de wachtrij voor verwerking en hun status. Een koppeling om te downloaden of het rapport weergeven is opgegeven als het rapport voltooid is. |

## <a name="view-reports"></a>Rapporten weergeven

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer links Active Directory.
3. Ga als volgt op een van deze twee opties, afhankelijk van of u Auth-Providers gebruiken:
   * **Optie 1**: klik op het tabblad multi-factor Auth-Providers. Selecteer de MFA-provider en klik op de **beheren** knop onderaan.
   * **Optie 2**: Selecteer uw directory en Ga naar de **configureren** tabblad. Selecteer in de sectie Multi-Factor Authentication de optie **Service-instellingen beheren**. Klik op de naar de portal-koppeling onder aan de pagina MFA-Service-instellingen.
4. Selecteer het type van het gewenste rapport in de Azure multi-factor Authentication-beheerportal, de **een rapport weergeven** sectie in het linkernavigatievenster.

<center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Melden van PowerShell

Gebruikers die zich hebben geregistreerd voor MFA met behulp van de volgende Powershell identificeren.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Gebruikers identificeren die nog niet geregistreerd voor MFA met behulp van de volgende Powershell.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

**Aanvullende resources**

* [Voor gebruikers](end-user/multi-factor-authentication-end-user.md)
* [Azure multi-factor Authentication op MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
