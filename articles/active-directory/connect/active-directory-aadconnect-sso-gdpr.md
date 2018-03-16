---
title: 'Azure AD Connect: Naadloze Single Sign-On - GDPR naleving | Microsoft Docs'
description: In dit artikel behandelt de naleving van Azure Active Directory (Azure AD) naadloze eenmalige aanmelding en GDPR.
services: active-directory
keywords: Wat is Azure AD Connect, GDPR, onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: billmath
ms.openlocfilehash: 0c7ed376accb1eed01106358491e925d3b8126c5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="azure-ad-seamless-single-sign-on-gdpr-compliance"></a>Azure AD naadloze eenmalige aanmelding: GDPR naleving

## <a name="overview"></a>Overzicht

In mei 2018, een Europese privacywetgeving de [algemene Data Protection regelgeving (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm), vervalt kracht te laten worden. De GDPR legt nieuwe regels voor bedrijven, overheidsinstanties, non-profitorganisaties en andere organisaties aanbieding goederen en diensten naar mensen in de Europese Unie, of dat verzamelen en analyseren van gegevens die zijn gekoppeld aan de EU inwoners. De GDPR is van toepassing ongeacht waar u zich bevindt. 

Microsoft-producten en services zijn vandaag beschikbaar waarmee u voldoen aan de vereisten GDPR. Meer informatie over het privacybeleid van Microsoft voor op [Vertrouwenscentrum](https://www.microsoft.com/trustcenter).

Azure AD naadloze eenmalige aanmelding wordt gemaakt van de volgende Logboektype die EUII kan bevatten:

- Azure AD Connect logboekbestanden voor tracering.

GDPR naleving voor naadloze eenmalige aanmelding kan worden bereikt op twee manieren:

1.  Gegevens ophalen voor een persoon op verzoek en gegevens van die persoon de installaties verwijderen.
2.  Zorg ervoor dat er geen gegevens behouden blijven na 48 uur.

Wij raden de tweede optie omdat dit eenvoudiger te implementeren en onderhouden. Zie de volgende instructies voor elk Logboektype:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Verwijderen van de logboekbestanden voor tracering van Azure AD Connect

Controleer de inhoud van **%ProgramData%\AADConnect** map en verwijder de tracering Meld inhoud (**trace -\*.log** bestanden) van deze map binnen 48 uur na het installeren of upgraden van Azure AD Connect of wijzigen van de configuratie naadloze eenmalige aanmelding, als deze actie wordt gedekt door GDPR gegevens gemaakt.

>[!IMPORTANT]
>Verwijder niet de **PersistedState.xml** in deze map, bestand zoals dit bestand wordt gebruikt om de status van de vorige installatie van Azure AD Connect te behouden en wordt gebruikt als een upgrade-installatie is voltooid. Dit bestand bevat geen gegevens over een persoon nooit en nooit moet worden verwijderd.

U kunt bekijken en verwijderen van deze logboekbestanden voor tracering met Windows Verkenner, of kunt u het volgende PowerShell-script de benodigde acties uit te voeren:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Sla het script in een bestand met de '. Ps1 ' extensie. Dit script uitvoert, indien nodig.

Voor meer informatie over gerelateerde vereisten voor Azure AD Connect GDPR, Zie [in dit artikel](active-directory-aadconnect-gdpr.md).

### <a name="note-about-domain-controller-logs"></a>Houd er rekening mee over Domain controller Logboeken

Als logboekregistratie is ingeschakeld, kan dit product beveiligingslogboeken genereren voor uw domeincontrollers. Lees voor meer informatie over het configureren van controlebeleid dit [artikel](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Volgende stappen

- [**Problemen met** ](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.
