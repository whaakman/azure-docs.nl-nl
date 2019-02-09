---
title: Verwijderen van persoonlijke gegevens - Azure Active Directory-toepassingsproxy | Microsoft Docs
description: Verwijderen van persoonlijke gegevens van connectoren voor Azure Active Directory-toepassingsproxy op apparaten geïnstalleerd.
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 1a4f5db66e5bcfa8b77930165534b943ed725209
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964913"
---
# <a name="remove-personal-data-for-azure-active-directory-application-proxy"></a>Verwijderen van persoonlijke gegevens voor Azure Active Directory-toepassingsproxy  

Azure Active Directory Application Proxy is vereist dat u connectors op uw apparaten installeren, wat betekent dat er persoonsgegevens op uw apparaten worden mogelijk. Dit artikel bevat stappen voor het verwijderen van persoonlijke gegevens ter verbetering van de privacy. 


## <a name="where-is-the-personal-data"></a>Waar is de persoonlijke gegevens?
Is het mogelijk voor de toepassingsproxy persoonlijke gegevens schrijven naar de volgende typen logboeken:

- Connector-gebeurtenislogboeken
- Windows-gebeurtenislogboeken

## <a name="remove-personal-data-from-windows-event-logs"></a>Persoonlijke gegevens worden verwijderd uit Windows-gebeurtenislogboeken

Zie voor meer informatie over het configureren van het bewaren van gegevens voor de Windows-gebeurtenislogboeken [instellingen voor gebeurtenislogboeken](https://technet.microsoft.com/library/cc952132.aspx). Zie voor meer informatie over Windows-gebeurtenislogboeken, [met behulp van Windows-gebeurtenislogboek](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="remove-personal-data-from-connector-event-logs"></a>Persoonlijke gegevens van gebeurtenislogboeken van de Connector verwijderd

Om te controleren of de logboeken van de Application Proxy geen persoonlijke gegevens bevatten, kunt u:

- Verwijderen of weergeven van gegevens wanneer dat nodig is, of
- Logboekregistratie uitschakelen

Gebruik de volgende secties om te verwijderen van persoonlijke gegevens uit de gebeurtenislogboeken connector. U moet de verwijdering voltooien voor alle apparaten waarop de connector is geïnstalleerd.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-or-export-specific-data"></a>Weergeven of specifieke gegevens exporteren

Als u wilt weergeven of specifieke gegevens exporteren, zoeken naar verwante vermeldingen in elk van de connector-gebeurtenislogboeken. De logboeken bevinden zich op `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. 

Omdat de logboekbestanden tekstbestanden zijn, kunt u [findstr](https://docs.microsoft.com/windows-server/administration/windows-commands/findstr) te zoeken naar tekstvermeldingen die betrekking hebben op een gebruiker.  

Zoeken om te vinden van persoonlijke gegevens, logboekbestanden voor gebruikers-id. 

Als u persoonlijke gegevens die zijn geregistreerd door een toepassing die gebruikmaakt van beperkte Kerberos-delegering zoekt, zoeken naar deze onderdelen van het type gebruikersnaam:

- On-premises user principal name
- Het deel van de user principal name dat de gebruikersnaam omvat
- Het deel van de on-premises user principal name dat de gebruikersnaam omvat
- On-premises security accounts manager (SAM)-accountnaam 


### <a name="delete-specific-data"></a>Specifieke gegevens verwijderen

Specifieke om gegevens te verwijderen:

1. Start de service Microsoft Azure AD Application Proxy Connector voor het genereren van een nieuw logboekbestand. Het nieuwe logboekbestand kunt u niet verwijderen of wijzigen van de oude logbestanden. 
2. Ga als volgt de [weergave- of exportbewerking specifieke gegevens](#view-or-export-specific-data) proces eerder beschreven informatie te vinden die moet worden verwijderd. Zoeken in alle logboeken van de connector.
3. Verwijder de relevante logboekbestanden of selectief verwijderen van de velden die persoonlijke gegevens bevatten. U kunt ook alle oude logboekbestanden verwijderen als u ze niet meer nodig.

### <a name="turn-off-connector-logs"></a>Logboeken van de connector uitschakelen

Een optie om te controleren of de connector-logboeken bevatten geen persoonlijke gegevens is de logboekgeneratie uitschakelen. Als u wilt stoppen met het genereren van Logboeken van de connector, verwijdert u de volgende gemarkeerde regel uit `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config`. 

![Configuratie](./media/application-proxy-remove-personal-data/01.png)


## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van Application Proxy [het bieden van veilige externe toegang tot on-premises toepassingen](application-proxy.md).

