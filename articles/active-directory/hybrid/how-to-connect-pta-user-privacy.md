---
title: Gebruikersprivacy en Azure Active Directory Pass through-verificatie | Microsoft Docs
description: In dit artikel behandelt de Pass through-verificatie voor Azure Active Directory (Azure AD) en de GDPR-naleving.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, Avg, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 52856ecaef06b367cca6edef0017b75f140f652d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149642"
---
# <a name="user-privacy-and-azure-active-directory-pass-through-authentication"></a>Gebruikersprivacy en Azure Active Directory Pass through-verificatie


[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Overzicht

Azure AD Pass-through-verificatie wordt gemaakt van de volgende Logboektype die persoonlijke gegevens bevat:

- Azure AD Connect logboekbestanden voor tracering.
- Verificatie-Agent trace-logboekbestanden.
- Windows Event log-bestanden.

De privacy van gebruikers verbeteren voor Pass through-verificatie op twee manieren:

1.  Gegevens ophalen voor een persoon op aanvraag, en gegevens verwijderen van die persoon de installaties.
2.  Zorg ervoor dat er geen gegevens worden bewaard na 48 uur.

Wij raden de tweede optie als het is eenvoudiger te implementeren en onderhouden. Hieronder vindt u de instructies voor elk type:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Logboekbestanden voor tracering van Azure AD Connect verwijderen

Controleer de inhoud van **%ProgramData%\AADConnect** map en verwijder de tracering het foutenlogboek van inhoud (**trace -\*.log** bestanden) van deze map binnen 48 uur na het installeren of upgraden van Azure AD Connect of wijzigen van Pass through-verificatie-configuratie, als deze actie kan maken van gegevens AVG vallen.

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

Voor meer informatie over gerelateerde vereisten voor Azure AD Connect Avg, Zie [in dit artikel](reference-connect-user-privacy.md).

### <a name="delete-authentication-agent-event-logs"></a>Verificatie-Agent-logboeken verwijderen

Dit product kan ook maken **Windows-gebeurtenislogboeken**. Lees voor meer informatie, [in dit artikel](https://msdn.microsoft.com/library/windows/desktop/aa385780(v=vs.85).aspx).

Als u logboeken met betrekking tot de Pass through-verificatie-Agent, opent u de **logboeken** toepassing op de server en controleer onder **toepassing en Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin** .

### <a name="delete-authentication-agent-trace-log-files"></a>Logboekbestanden voor tracering van verificatie-Agent verwijderen

Controleer regelmatig de inhoud van <strong>%ProgramData%\Microsoft\Azure AD verbinding maken met verificatie Agent\Trace\</ strong > en verwijder de inhoud van deze map om 48 uur. 

>[!IMPORTANT]
>Als de verificatie-Agent-service wordt uitgevoerd, wordt het niet mogelijk om te verwijderen van het huidige logboekbestand in de map. Stop de service voordat u doorgaat. Om te voorkomen dat een gebruiker aanmeldingen dat is toegestaan, u moet al zijn geconfigureerd voor Pass-through-verificatie [hoge beschikbaarheid](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability).

U kunt controleren en verwijderen van deze bestanden met behulp van Windows Explorer of kunt u het volgende script om uit te voeren van de noodzakelijke acties:

```
$Files = ((Get-childitem -Path "$env:programdata\microsoft\azure ad connect authentication agent\trace" -Recurse).VersionInfo).FileName 
 
Foreach ($file in $files) { 
    {Remove-Item -Path $File -Force} 
}
```

Als u deze uit te voeren script plannen om 48 uur als volgt:

1.  Sla het script in een bestand met de '. Ps1 "extensie.
2.  Open **Configuratiescherm** en klikt u op **systeem en beveiliging**.
3.  Onder de **Systeembeheer** kop, klikt u op '**taken plant**'.
4.  In **Task Scheduler**, met de rechtermuisknop op "**taak planning bibliotheek**'en klik op'**... in eenvoudige taak maken** '.
5.  Voer de naam voor de nieuwe taak en klik op **volgende**.
6.  Selecteer "**dagelijkse**' voor de **Taaktrigger** en klikt u op **volgende**.
7.  Het terugkeerpatroon ingesteld op twee dagen en klikt u op **volgende**.
8.  Selecteer "**begint met een programma**' als de actie en klik op **volgende**.
9.  Type "**PowerShell**"in het vak voor het programma/script en in het vak met de tekst"**argumenten toevoegen (optioneel)**', voert u het volledige pad naar het script dat u eerder hebt gemaakt en klik vervolgens op **volgende**.
10. Het volgende scherm bevat een overzicht van de taak die u gaat maken. Controleer of de waarden en klik op **voltooien** om de taak te maken:
 
### <a name="note-about-domain-controller-logs"></a>Houd er rekening mee over de Domain controller Logboeken

Als logboekregistratie is ingeschakeld, kan dit product-logboeken genereren voor uw domeincontrollers. Lees voor meer informatie over het configureren van controlebeleid, dit [artikel](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Volgende stappen
* [De Microsoft Privacy controlebeleid op Vertrouwenscentrum](https://www.microsoft.com/trustcenter)
- [**Problemen oplossen** ](tshoot-connect-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
