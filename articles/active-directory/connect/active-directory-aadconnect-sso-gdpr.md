---
title: Gebruikersprivacy en Azure AD naadloze eenmalige aanmelding | Microsoft Docs
description: In dit artikel behandelt de naadloze eenmalige aanmelding voor Azure Active Directory (Azure AD) en de GDPR-naleving.
services: active-directory
keywords: Wat is Azure AD Connect, Avg, onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 50c97ce7a492c934e15634622d86bf587ffb3fb7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915721"
---
# <a name="user-privacy-and-azure-ad-seamless-single-sign-on"></a>Gebruikersprivacy en Azure AD naadloze eenmalige aanmelding

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Overzicht


Azure AD naadloze eenmalige aanmelding wordt gemaakt van de volgende Logboektype die persoonlijke gegevens bevat: 

- Azure AD Connect logboekbestanden voor tracering.

De privacy van gebruikers verbeteren voor naadloze eenmalige aanmelding op twee manieren:

1.  Gegevens ophalen voor een persoon op aanvraag, en gegevens verwijderen van die persoon de installaties.
2.  Zorg ervoor dat er geen gegevens worden bewaard na 48 uur.

Wij raden de tweede optie als het is eenvoudiger te implementeren en onderhouden. Zie de instructies volgen voor elk type:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Logboekbestanden voor tracering van Azure AD Connect verwijderen

Controleer de inhoud van **%ProgramData%\AADConnect** map en verwijder de tracering het foutenlogboek van inhoud (**trace -\*.log** bestanden) van deze map binnen 48 uur na het installeren of upgraden van Azure AD Connect of wijzigen van naadloze eenmalige aanmelding-configuratie, als deze actie kan maken van gegevens AVG vallen.

>[!IMPORTANT]
>Verwijder niet de **PersistedState.xml** in deze map, bestand, zoals dit bestand wordt gebruikt om de status van de vorige installatie van Azure AD Connect te behouden en wordt gebruikt wanneer de installatie van een upgrade is voltooid. Dit bestand bevat geen gegevens over een persoon nooit en moet nooit worden verwijderd.

U kunt controleren en verwijderen van deze logboekbestanden voor tracering met behulp van Windows Explorer of u kunt de volgende PowerShell-script gebruiken om uit te voeren van de nodige maatregelen:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Sla het script in een bestand met de '. Ps1 "extensie. Voer dit script zo nodig.

Voor meer informatie over gerelateerde vereisten voor Azure AD Connect Avg, Zie [in dit artikel](active-directory-aadconnect-gdpr.md).

### <a name="note-about-domain-controller-logs"></a>Houd er rekening mee over de Domain controller Logboeken

Als logboekregistratie is ingeschakeld, kan dit product-logboeken genereren voor uw domeincontrollers. Lees voor meer informatie over het configureren van controlebeleid, dit [artikel](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Volgende stappen
* [De Microsoft Privacy controlebeleid op Vertrouwenscentrum](https://www.microsoft.com/trustcenter)
- [**Problemen oplossen** ](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen in te dienen.
