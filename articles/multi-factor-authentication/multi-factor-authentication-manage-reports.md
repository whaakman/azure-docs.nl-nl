---
title: Toegang en gebruik rapporten voor Azure MFA | Microsoft Docs
description: Dit wordt beschreven hoe u de functie Azure multi-factor Authentication - rapporten.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 32697eea410cb9afaa0e4347acd1a280fcf94289
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporten in Azure multi-factor Authentication

Azure multi-factor Authentication bevat verschillende rapporten die kunnen worden gebruikt door u en uw organisatie. Deze rapporten zijn toegankelijk via de multi-factor Authentication-beheerportal. De volgende tabel bevat de beschikbare rapporten:

| Rapport | Beschrijving |
|:--- |:--- |
| Gebruik |Het gebruik rapporten informatie weergeven over algemene gebruik Gebruikersoverzicht en details van gebruiker. |
| Serverstatus |Dit rapport geeft de status van de multi-factor Authentication-Servers die zijn gekoppeld aan uw account. |
| Geschiedenis geblokkeerde gebruikers |Deze rapporten tonen de geschiedenis van aanvragen voor het blokkeren of deblokkeren van gebruikers. |
| Geschiedenis overgeslagen gebruikers |Geeft de geschiedenis van aanvragen voor het overslaan van multi-factor Authentication voor het telefoonnummer van een gebruiker. |
| Fraudewaarschuwing |Geeft een historie weer van Fraudewaarschuwingen die zijn ingediend in het datumbereik dat u hebt opgegeven. |
| In wachtrij |Een lijst met rapporten in de wachtrij voor verwerking en hun status. Een koppeling om te downloaden of het rapport weergeven is opgegeven als het rapport voltooid is. |

## <a name="view-reports"></a>Rapporten weergeven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers** > **multi-factor Verificatie**.
3. Onder **multi-factorauthenticatie**, selecteer **service-instellingen**. Onderaan onder **beheren geavanceerde instellingen en rapporten weergeven**, selecteer **gaat u naar de portal**.
4. Selecteer het type van het gewenste rapport in de Azure multi-factor Authentication-beheerportal, de **een rapport weergeven** sectie in het linkernavigatievenster.

<center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Melden van PowerShell

Gebruikers die zich hebben geregistreerd voor MFA met behulp van de volgende PowerShell identificeren.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Gebruikers identificeren die nog niet geregistreerd voor MFA met behulp van de volgende PowerShell.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

**Aanvullende resources**

* [Voor gebruikers](end-user/multi-factor-authentication-end-user.md)
* [Azure multi-factor Authentication op MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
