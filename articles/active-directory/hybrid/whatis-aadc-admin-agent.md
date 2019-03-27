---
title: Wat is de Azure AD Connect-beheerder Agent - Azure AD Connect | Microsoft Docs
description: Hier worden de hulpprogramma's beschreven die worden gebruikt om uw on-premises omgeving met Azure AD te synchroniseren en bewaken.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17b99b7b03898ad72b4d9c7cc2ba9154855475ef
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488281"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>Wat is de Azure AD Connect-beheerder-Agent? 
De Azure AD Connect beheer-Agent is een nieuw onderdeel van Azure Active Directory Connect die is geïnstalleerd op een Azure Active Directory Connect-server. Deze wordt gebruikt om specifieke gegevens verzamelen over uw Active Directory-omgeving waarmee u een Microsoft ondersteuningstechnicus kunt oplossen van problemen wanneer u een ondersteuningsaanvraag openen.

Wanneer geïnstalleerd, ondersteuning de Azure AD Connect beheer Agent wacht op specifieke aanvragen voor gegevens uit Azure Active Directory, de aangevraagde gegevens worden opgehaald uit de synchronisatie-omgeving, en verzendt ze naar Azure Active Directory, waar deze wordt weergegeven aan het Microsoft engineer.

De informatie die de Azure AD Connect beheer-Agent wordt opgehaald uit uw omgeving wordt niet opgeslagen op geen enkele manier: dit wordt alleen weergegeven voor de ondersteuningstechnicus van Microsoft om u te helpen bij het onderzoeken en het oplossen van de Azure Active Directory Connect gerelateerde ondersteuningsaanvraag die u hebt geopend

## <a name="how-is-the-azure-ad-connect-admin-agent-installed-on-the-azure-ad-connect-server"></a>Hoe is de Azure AD Connect-beheerder-Agent geïnstalleerd op de Azure AD Connect-server? 
Nadat de beheerder-Agent is geïnstalleerd, ziet u de volgende twee nieuwe programma's in de lijst 'Programma's toevoegen/verwijderen' in het Configuratiescherm van de server: 

![beheerder-agent](media/whatis-aadc-admin-agent/adminagent1.png)

Verwijder of ongedaan maken de programma's installeren, omdat ze een belangrijk onderdeel van uw Azure AD Connect-installatie zijn niet.

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>Welke gegevens in mijn Sync-service wordt weergegeven aan de Microsoft-technicus?
Wanneer u een ondersteuningscase, de Microsoft-medewerker opent kunnen zien, voor een bepaalde gebruiker, de relevante gegevens in Active Directory, de Active Directory-connectorgebied in de Azure Active Directory Connect-server, de Azure Active Directory-connectorgebied in Azure Active Directory Connect-server en de Metaverse in de Azure Active Directory Connect-server.

De ondersteuningstechnicus van Microsoft kan gegevens niet wijzigen in uw systeem en de wachtwoorden niet kan zien.

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>Wat gebeurt er als ik wil niet de ondersteuningstechnicus van Microsoft voor toegang tot mijn gegevens? 
 
Als u niet wilt dat Bel de Microsoft service engineer toegang tot uw gegevens voor een telefoongesprek met de ondersteuning kunt u dit opgeven wanneer u een ondersteuningsaanvraag openen in de portal: 

  1.    Open **C:\Program Files\Microsoft Azure AD Connect beheer Agent\AzureADConnectAdministrationAgentService.exe.config** in Kladblok.
  2.    Uitschakelen **UserDataEnabled** instellen zoals hieronder wordt weergegeven. Als **UserDataEnabled** instelling bestaat en is ingesteld op true, wordt deze ingesteld op false. Als de instelling niet bestaat nog, voegt u de instelling zoals hieronder wordt weergegeven.    
  `
 <appSettings>
   <add key="TraceFilename" value="ADAdministrationAgent.log" />
   <add key="UserDataEnabled" value="false" />
  </appSettings>
  `
  3.    Sla het configuratiebestand.
  4.    Azure AD Connect beheer Agent-service opnieuw starten, zoals hieronder wordt weergegeven

![beheerder-agent](media/whatis-aadc-admin-agent/adminagent2.png)

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).