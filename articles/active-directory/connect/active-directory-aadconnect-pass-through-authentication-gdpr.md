---
title: Azure AD Connect - Pass through-verificatie - GDPR naleving | Microsoft Docs
description: In dit artikel behandelt de naleving van Azure Active Directory (Azure AD) Pass through-verificatie en GDPR.
services: active-directory
keywords: Azure AD Connect Pass-through-verificatie, GDPR, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
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
ms.custom: seohack1
ms.openlocfilehash: 21874c961163e3efba45c2ee8557c03135987f95
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="azure-active-directory-pass-through-authentication-gdpr-compliance"></a>Azure Active Directory Pass-through-verificatie: GDPR naleving

## <a name="overview"></a>Overzicht

In mei 2018, een Europese privacywetgeving de [algemene Data Protection regelgeving (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm), vervalt kracht te laten worden. De GDPR legt nieuwe regels voor bedrijven, overheidsinstanties, non-profitorganisaties en andere organisaties aanbieding goederen en diensten naar mensen in de Europese Unie, of dat verzamelen en analyseren van gegevens die zijn gekoppeld aan de EU inwoners. De GDPR is van toepassing ongeacht waar u zich bevindt. 

Microsoft-producten en services zijn vandaag beschikbaar waarmee u voldoen aan de vereisten GDPR. Meer informatie over het privacybeleid van Microsoft voor op [Vertrouwenscentrum](https://www.microsoft.com/trustcenter).

Azure AD Pass-through-verificatie worden gemaakt van de volgende typen van het logboek, die kunnen EUII bevatten:

- Azure AD Connect logboekbestanden voor tracering.
- Verificatie-Agent traceerlogboekbestanden.
- Logboekbestanden van het Windows-gebeurtenis.

GDPR naleving voor Pass-through-verificatie kan worden bereikt op twee manieren:

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
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
