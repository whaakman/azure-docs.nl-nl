---
title: 'Azure AD Connect: Wanneer u al hebt Azure AD | Microsoft Docs'
description: Dit onderwerp wordt beschreven hoe u verbinding maken wanneer u een bestaande Azure AD-tenant.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: fa264487c68ea5403300d9b5b9978934a639a2a4
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Azure AD Connect: Wanneer u hebt een bestaande tenant
De meeste van de onderwerpen over het gebruik van Azure AD Connect wordt ervan uitgegaan dat u begint met een nieuwe Azure AD-tenant en dat er geen gebruikers of er andere objecten zijn. Maar als u hebt gestart met een Azure AD-tenant gevuld zijn met gebruikers en andere objecten, en wilt verbinden, gebruik vervolgens dit onderwerp is voor u.

## <a name="the-basics"></a>De basisbeginselen
Een object in Azure AD is ofwel gemaakt in de cloud (Azure AD) of on-premises. Voor een enkel object, kunt u bepaalde kenmerken on-premises en sommige andere kenmerken niet beheren in Azure AD. Elk object heeft een vlag die aangeeft waar het object wordt beheerd.

U kunt sommige gebruikers on-premises en andere beheren in de cloud. Een veelvoorkomend scenario voor deze configuratie is een organisatie met een combinatie van accounting werknemers en verkoop. De accounting werknemers hebben een on-premises AD-account, maar de verkoop werknemers niet, ze hebben een account in Azure AD. Sommige gebruikers on-premises en sommige in Azure AD beheren.

Als u gestart voor het beheren van gebruikers in Azure AD die zich ook in on-premises AD en later wilt verbinding maken, worden er enkele aanvullende problemen moet u rekening houden.

## <a name="sync-with-existing-users-in-azure-ad"></a>Synchroniseren met bestaande gebruikers in Azure AD
Wanneer u Azure AD Connect installeert en u begint met het synchroniseren, wordt de Azure AD sync-service (in Azure AD) biedt een controle op elk nieuw object en probeer het vinden van een bestaand object moet worden gezocht. Er zijn drie kenmerken die worden gebruikt voor dit proces: **userPrincipalName**, **proxyAddresses**, en **sourceAnchor**/**onveranderbare id genoemd**. Een overeenkomst op **userPrincipalName** en **proxyAddresses** wordt ook wel een **zachte overeen**. Een overeenkomst op **sourceAnchor** staat bekend als **harde overeen**. Voor de **proxyAddresses** alleen de waarde met het kenmerk **SMTP:**, die het primaire e-mailadres wordt gebruikt voor de evaluatie.

De overeenkomst wordt alleen voor nieuwe objecten die afkomstig zijn van Connect geëvalueerd. Als u een bestaand object wijzigen zodat deze met een van deze kenmerken overeen komt, klikt u vervolgens ziet u een fout in plaats daarvan.

Als Azure AD vindt een object waar de kenmerkwaarden zijn hetzelfde voor een object dat afkomstig is van Connect en die is al aanwezig in Azure AD, wordt klikt u vervolgens het object in Azure AD overgenomen Connect. Het object eerder cloud beheerd is gemarkeerd als lokale beheerd. Alle kenmerken in Azure AD met een waarde in de lokale AD overschreven met de lokale waarde. De uitzondering is wanneer een kenmerk heeft een **NULL** lokale waarde. In dit geval wordt kunt nog steeds de waarde in Azure AD blijft, maar u deze alleen wijzigen lokale in iets anders.

> [!WARNING]
> Aangezien alle kenmerken in de Azure AD worden overschreven door de waarde van de lokale gaat, moet u goed gegevens lokaal. Bijvoorbeeld zo hebt u alleen e-mailadres in Office 365 beheerd en niet bewaard bijgewerkt on-premises AD DS en u eventuele waarden in Azure AD/Office 365 niet aanwezig is in AD DS verliest.

> [!IMPORTANT]
> Als u Wachtwoordsynchronisatie die altijd door expresinstellingen wordt gebruikt, wordt het wachtwoord in Azure AD wordt overschreven met het wachtwoord in de lokale AD. Als uw gebruikers worden gebruikt om verschillende wachtwoorden te beheren, moet u laat ze weten dat ze de on-premises wachtwoord gebruiken moeten wanneer u verbinding hebt geïnstalleerd.

De vorige sectie en een waarschuwing moeten worden overwogen in de planning. Als u veel wijzigingen hebt aangebracht in Azure AD niet doorgevoerd in lokale AD DS, moet u plannen hoe u AD DS met de bijgewerkte waarden opgeeft voordat u uw objecten met Azure AD Connect synchroniseert.

Als u uw objecten met een soft-overeenkomst, komt overeen met de **sourceAnchor** is toegevoegd aan het object in Azure AD, zodat een vaste overeenkomst later kan worden gebruikt.

### <a name="hard-match-vs-soft-match"></a>Harde-match tegenover Soft-match
Er is geen praktische verschil tussen een soft- en een vaste overeenkomst voor een nieuwe installatie van Connect. Het verschil is in een situatie met een herstel na noodgevallen. Als uw server met Azure AD Connect is verbroken, kunt u een nieuw exemplaar opnieuw installeren zonder verlies van gegevens. Een object met een sourceAnchor is verzonden naar Connect tijdens de eerste installatie. De overeenkomst kan vervolgens worden geëvalueerd door de client (Azure AD Connect), veel sneller is dan hetzelfde doen in Azure AD. Een vaste overeenkomst wordt geëvalueerd door Connect zowel door Azure AD. Een voorlopig overeenkomst wordt alleen geëvalueerd door Azure AD.

### <a name="other-objects-than-users"></a>Andere objecten dan gebruikers
Voor groepen met e-mailfunctionaliteit en contactpersonen, u kunt soft-overeenkomst op basis van proxyAddresses. Harde-match is niet van toepassing omdat u kunt alleen de sourceAnchor/onveranderbare id genoemd (met PowerShell) voor gebruikers alleen bijwerken. Voor groepen die niet e-mailfunctionaliteit zijn, is er momenteel geen ondersteuning voor soft-match of harde-overeenkomst.

## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Een nieuwe lokale Active Directory gemaakt van gegevens in Azure AD
Sommige klanten beginnen met een cloud-only-oplossing met Azure AD en hebben geen een on-premises AD. Later ze willen verbruiken lokale bronnen en wilt maken van een on-premises AD op basis van Azure AD-gegevens. Azure AD Connect helpen u niet voor dit scenario. Er wordt geen gebruikers lokale gemaakt en heeft geen mogelijkheid om in te stellen de wachtwoord on-premises naar hetzelfde als in Azure AD.

Als de enige reden waarom u van plan bent om toe te voegen on-premises AD dat is voor de ondersteuning van LOB's (Line-of-Business-apps), wordt mogelijk kunt u overwegen gebruik [Azure AD domain services](../../active-directory-domain-services/index.md) in plaats daarvan.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
