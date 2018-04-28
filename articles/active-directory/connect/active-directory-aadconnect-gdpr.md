---
title: Azure AD Connect en gebruiker privacy | Microsoft Docs
description: Dit document beschrijft het verkrijgen van GDPR compliancy met Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: billmath
ms.openlocfilehash: a1fa7f58040b420bf52d89a57b1234416c2fb939
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="user-privacy-and-azure-ad-connect"></a>Privacy van gebruikers en Azure AD Connect 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>In dit artikel gaat over Azure AD Connect en gebruiker privacy.  Zie het artikel voor informatie over Azure AD Connect Health en gebruiker privacy [hier](../../active-directory/connect-health/active-directory-aadconnect-health-gdpr.md).

Privacy gebruikerscompatibiliteit voor Azure AD Connect-installaties, kan worden bereikt op twee manieren:

1.  Gegevens ophalen voor een persoon op verzoek en gegevens van die persoon de installaties verwijderen
2.  Zorg ervoor dat er geen gegevens behouden blijven na 48 uur.

Het team van Azure AD Connect raadt de tweede optie, omdat het is veel eenvoudiger te implementeren en onderhouden.

Een Azure AD Connect sync-server slaat de gegevens van de volgende privacy:
1.  Gegevens over een persoon in de **Azure AD Connect-database**
2.  Gegevens in de **Windows-gebeurtenislogboek** bestanden die informatie over een persoon bevatten kunnen
3.  Gegevens in de **logboekbestanden voor Azure AD Connect-installatie** die over een persoon kan bevatten

Azure AD Connect-klanten gebruik de volgende richtlijnen bij het verwijderen van gebruikersgegevens:
1.  Verwijder de inhoud van de map waarin de Azure AD Connect installatielogboekbestanden regelmatig: ten minste elke 48 uur
2.  Dit product kan ook gebeurtenislogboeken maken.  Raadpleeg voor meer informatie over gebeurtenislogboeken Logboeken, het [documentatie hier](https://msdn.microsoft.com/library/windows/desktop/aa385780.aspx).

Gegevens over een persoon worden automatisch verwijderd uit de database met Azure AD Connect wanneer gegevens van die persoon wordt verwijderd uit het bronsysteem waar deze afkomstig van is. Er is geen specifieke actie van administrators vereist GDPR compatibel zijn.  Het is echter vereist dat de gegevens van Azure AD Connect is gesynchroniseerd met uw gegevensbron ten minste elke twee dagen.

## <a name="delete-the-azure-ad-connect-installation-log-file-folder-contents"></a>Azure AD Connect-installatie Mapinhoud van het logboekbestand verwijderen
Regelmatig controleren en verwijder de inhoud van **c:\programdata\aadconnect** map – behalve voor de **PersistedState.Xml** bestand. Dit bestand houdt de status van de vorige installatie van Azure A Connect en wordt gebruikt wanneer u een upgrade-installatie wordt uitgevoerd. Dit bestand bevat niet alle gegevens over een persoon en mag niet worden verwijderd.

>[!IMPORTANT]
>Verwijder het bestand PersistedState.xml niet.  Dit bestand bevat geen gebruikersinformatie en onderhoudt de status van de vorige installatie.

U kunt bekijken en deze bestanden verwijderen met Windows Verkenner, of kunt u een script als volgt de benodigde acties uit te voeren:


```
$Files = ((Get-childitem -Path "$env:programdata\aadconnect" -Recurse).VersionInfo).FileName
Foreach ($file in $files) {
If ($File.ToUpper() -ne "$env:programdata\aadconnect\PERSISTEDSTATE.XML".toupper()) # Do not delete this file
    {Remove-Item -Path $File -Force}
    } 
```

### <a name="schedule-this-script-to-run-every-48-hours"></a>Dit script uit te voeren om 48 uur plannen
Gebruik de volgende stappen uit om te plannen dat het script uitvoeren om 48 uur.

1.  Sla het script in een bestand met de extensie  **&#46;PS1**, open het Configuratiescherm en klik op **systemen en beveiliging**.
    ![Systeem](media\active-directory-aadconnect-gdpr\gdpr2.png)

2.  Klik op onder de kop Systeembeheer **taken plant**.
    ![Taak](media\active-directory-aadconnect-gdpr\gdpr3.png)
3.  In Taakplanner, klik met de rechtermuisknop op **taak schema bibliotheek** en klik op **taak Basic maken...**
4.  Voer de naam voor de nieuwe taak en klikt u op **volgende**.
5.  Selecteer **dagelijkse** voor de taak is niet geactiveerd en klik op **volgende**.
6.  Het terugkeerpatroon ingesteld op **2 dagen** en klik op **volgende**.
7.  Selecteer **een programma Start** als de actie en klik op **volgende**.
8.  Type **PowerShell** in het vak voor het programma/script en in het vak met het label **argumenten toevoegen (optioneel)**, voert u het volledige pad naar het script dat u eerder hebt gemaakt en klik vervolgens op **volgende**.
9.  Het volgende scherm toont een overzicht van de taak die u gaat maken. Controleer de waarden en klik op **voltooien** om de taak te maken.



## <a name="next-steps"></a>Volgende stappen
* [Het beleid voor Microsoft Privacy op Vertrouwenscentrum bekijken](https://www.microsoft.com/trustcenter)
- [Azure AD Connect Health en Privacy van gebruikers](../../active-directory/connect-health/active-directory-aadconnect-health-gdpr.md)
