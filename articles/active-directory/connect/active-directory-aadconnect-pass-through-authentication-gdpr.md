---
title: Privacy van gebruikers en Azure Active Directory-Pass through-verificatie | Microsoft Docs
description: In dit artikel behandelt de naleving van Azure Active Directory (Azure AD) Pass through-verificatie en GDPR.
services: active-directory
keywords: Azure AD Connect Pass-through-verificatie, GDPR, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: swkrish
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
ms.custom: seohack1
ms.openlocfilehash: 3343cebb85124f19fe773822e296312abad53d96
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591171"
---
# <a name="user-privacy-and-azure-active-directory-pass-through-authentication"></a>Privacy van gebruikers en Azure Active Directory-Pass through-verificatie


[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Overzicht

Azure AD Pass-through-verificatie worden gemaakt van de volgende Logboektype dat persoonlijke gegevens kan bevatten:

- Azure AD Connect logboekbestanden voor tracering.
- Verificatie-Agent traceerlogboekbestanden.
- Logboekbestanden van het Windows-gebeurtenis.

De privacy van gebruikers voor Pass-through-verificatie op twee manieren:

1.  Gegevens ophalen voor een persoon op verzoek en gegevens van die persoon de installaties verwijderen.
2.  Zorg ervoor dat er geen gegevens behouden blijven na 48 uur.

Wij raden de tweede optie omdat dit eenvoudiger te implementeren en onderhouden. Hieronder vindt u de instructies voor elk Logboektype:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Verwijderen van de logboekbestanden voor tracering van Azure AD Connect

Controleer de inhoud van **%ProgramData%\AADConnect** map en verwijder de tracering Meld inhoud (**trace -\*.log** bestanden) van deze map binnen 48 uur na het installeren of upgraden van Azure AD Connect of wijzigen van de configuratie van Pass through-verificatie, als deze actie wordt gedekt door GDPR gegevens gemaakt.

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

### <a name="delete-authentication-agent-event-logs"></a>De gebeurtenislogboeken van de verificatie-Agent verwijderen

Dit product kan ook maken **Windows-gebeurtenislogboeken**. Lees voor meer informatie, [in dit artikel](https://msdn.microsoft.com/library/windows/desktop/aa385780(v=vs.85).aspx).

Om Logboeken te raadplegen betrekking hebben op de Agent van Pass through-verificatie, opent u de **logboeken** toepassing op de server en controleer onder **toepassing en Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin** .

### <a name="delete-authentication-agent-trace-log-files"></a>Logboekbestanden voor tracering van verificatie-Agent verwijderen

Controleer regelmatig de inhoud van **%ProgramData%\Microsoft\Azure AD Authentication Agent\Trace verbinding\**  en verwijder de inhoud van deze map om 48 uur. 

>[!IMPORTANT]
>Als de verificatie-Agent-service wordt uitgevoerd, kunt u zult niet verwijderen van het huidige logboekbestand in de map. Stop de service voordat u opnieuw probeert. Gebruiker aanmelden om fouten te voorkomen, u moet al hebt geconfigureerd Pass through-verificatie voor [hoge beschikbaarheid](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

U kunt bekijken en deze bestanden verwijderen met Windows Verkenner, of kunt u het volgende script uit te voeren de nodige maatregelen:

```
$Files = ((Get-childitem -Path "$env:programdata\microsoft\azure ad connect authentication agent\trace" -Recurse).VersionInfo).FileName 
 
Foreach ($file in $files) { 
    {Remove-Item -Path $File -Force} 
}
```

Als u dit script uit te voeren plannen om 48 uur als volgt:

1.  Sla het script in een bestand met de '. Ps1 ' extensie.
2.  Open **Configuratiescherm** en klik op **systeem en beveiliging**.
3.  Onder de **Systeembeheer** kop, klik op '**taken plant**'.
4.  In **Task Scheduler**, met de rechtermuisknop op '**taak schema bibliotheek**'en klik op'**maken Basic taak...** '.
5.  Voer de naam voor de nieuwe taak en klikt u op **volgende**.
6.  Selecteer "**dagelijkse**' voor de **taak is niet geactiveerd** en klik op **volgende**.
7.  Het terugkeerpatroon ingesteld op twee dagen en klik op **volgende**.
8.  Selecteer "**een programma Start**' als de actie en klik op **volgende**.
9.  Type '**PowerShell**'in het vak voor het programma/script en in het vak met het label'**argumenten toevoegen (optioneel)**', voert u het volledige pad naar het script dat u eerder hebt gemaakt en klik vervolgens op **volgende**.
10. Het volgende scherm toont een overzicht van de taak die u gaat maken. Controleer de waarden en klik op **voltooien** om de taak te maken:
 
### <a name="note-about-domain-controller-logs"></a>Houd er rekening mee over Domain controller Logboeken

Als logboekregistratie is ingeschakeld, kan dit product beveiligingslogboeken genereren voor uw domeincontrollers. Lees voor meer informatie over het configureren van controlebeleid dit [artikel](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Volgende stappen
* [Het beleid voor Microsoft Privacy op Vertrouwenscentrum bekijken](https://www.microsoft.com/trustcenter)
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
