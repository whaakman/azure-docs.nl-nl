---
title: Azure AD Connect en gebruiker privacy | Microsoft Docs
description: Dit document wordt beschreven hoe u kunt verkrijgen van GDPR in overeenstemming is met Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/21/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f5d3125b7b77e8ce7a943f640c44615049ab160
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58096347"
---
# <a name="user-privacy-and-azure-ad-connect"></a>Gebruikersprivacy en Azure AD Connect 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>In dit artikel behandelt de Azure AD Connect en gebruiker privacy.  Zie het artikel voor meer informatie over Azure AD Connect Health en gebruiker privacy [hier](reference-connect-health-user-privacy.md).

Verbeter de gebruikersprivacy voor Azure AD Connect-installatie op twee manieren:

1.  Op aanvraag kunnen gegevens voor een persoon ophalen en verwijderen van gegevens uit die persoon de installaties
2.  Zorg ervoor dat er geen gegevens worden bewaard na 48 uur.

Het team van Azure AD Connect raadt de tweede optie, omdat het is veel eenvoudiger te implementeren en onderhouden.

Een Azure AD Connect-synchronisatieserver slaat de gegevens van de volgende privacy:
1.  Gegevens over een persoon in de **Azure AD Connect-database**
2.  Gegevens in de **Windows-gebeurtenislogboek** bestanden die informatie over een persoon bevatten kunnen
3.  Gegevens in de **logboekbestanden van de Azure AD Connect-installatie** die over een persoon kan bevatten

Azure AD Connect-klanten moeten de volgende richtlijnen gebruiken bij het verwijderen van gebruikersgegevens:
1.  Verwijder de inhoud van de map met de Azure AD Connect-installatielogboekbestanden op gezette tijden: ten minste elke 48 uur
2.  Dit product kan ook gebeurtenislogboeken maken.  Raadpleeg voor meer informatie over gebeurtenislogboeken Logboeken, het [documentatie die hier beschikbaar](https://msdn.microsoft.com/library/windows/desktop/aa385780.aspx).

Gegevens over een persoon automatisch worden verwijderd uit de Azure AD Connect-database als de gegevens van die persoon wordt verwijderd uit het bronsysteem waarvan deze afkomstig is uit. Geen specifieke actie van de beheerders is vereist om te voldoen aan het beleid aan gdpr.  Het is echter vereist dat de gegevens van Azure AD Connect is gesynchroniseerd met uw gegevensbron ten minste elke twee dagen.

## <a name="delete-the-azure-ad-connect-installation-log-file-folder-contents"></a>Azure AD Connect-installatie Mapinhoud van het logboekbestand verwijderen
Regelmatig controleren en verwijderen van de inhoud van **c:\programdata\aadconnect** map –, behalve voor de **PersistedState.Xml** bestand. Dit bestand houdt de status van de vorige installatie van Azure een verbinding en wordt gebruikt wanneer de installatie van een upgrade wordt uitgevoerd. Dit bestand bevat geen gegevens over een persoon en mag niet worden verwijderd.

>[!IMPORTANT]
>Het bestand PersistedState.xml niet verwijderen.  Dit bestand bevat geen gebruikersgegevens en onderhoudt de status van de vorige installatie.

U kunt controleren en verwijderen van deze bestanden met behulp van Windows Explorer of kunt u een script als volgt de vereiste acties uit te voeren:


```
$Files = ((Get-childitem -Path "$env:programdata\aadconnect" -Recurse).VersionInfo).FileName
Foreach ($file in $files) {
If ($File.ToUpper() -ne "$env:programdata\aadconnect\PERSISTEDSTATE.XML".toupper()) # Do not delete this file
    {Remove-Item -Path $File -Force}
    } 
```

### <a name="schedule-this-script-to-run-every-48-hours"></a>Met dit script om uit te voeren om 48 uur plannen
Gebruik de volgende stappen uit om te plannen dat het script om uit te voeren om 48 uur.

1.  Sla het script in een bestand met de extensie  **&#46;PS1**, open het Configuratiescherm en klik op **systemen en beveiliging**.
    ![Systeem](./media/reference-connect-user-privacy/gdpr2.png)

2.  Klik onder de kop Systeembeheer op **taken plant**.
    ![Taak](./media/reference-connect-user-privacy/gdpr3.png)
3.  In Task Scheduler, klik met de rechtermuisknop op **taak planning bibliotheek** en klikt u op **taak Basic maken...**
4.  Voer de naam voor de nieuwe taak en klik op **volgende**.
5.  Selecteer **dagelijkse** voor de taaktrigger en klik op **volgende**.
6.  Het terugkeerpatroon ingesteld op **2 dagen** en klikt u op **volgende**.
7.  Selecteer **begint met een programma** als de actie en klikt u op **volgende**.
8.  Type **PowerShell** in het vak voor het programma/script en in het vak met de tekst **argumenten toevoegen (optioneel)**, voert u het volledige pad naar het script dat u eerder hebt gemaakt en klik vervolgens op **volgende**.
9.  Het volgende scherm bevat een overzicht van de taak die u gaat maken. Controleer of de waarden en klik op **voltooien** om de taak te maken.



## <a name="next-steps"></a>Volgende stappen
* [De Microsoft Privacy controlebeleid op Vertrouwenscentrum](https://www.microsoft.com/trustcenter)
* [Azure AD Connect Health en Privacy van gebruikers](reference-connect-health-user-privacy.md)
