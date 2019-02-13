---
title: 'Azure AD Connect: Wanneer u Azure AD al hebt | Microsoft Docs'
description: In dit onderwerp wordt beschreven hoe u verbinding maken wanneer u een bestaande Azure AD-tenant hebt.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6775f6e37a5b282afcfcdce7f93751e852923366
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56168355"
---
# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Azure AD Connect: Wanneer u een bestaande tenant hebt
De meeste van de onderwerpen over het gebruik van Azure AD Connect wordt ervan uitgegaan dat u begint met een nieuwe Azure AD-tenant en dat er geen gebruikers of er andere objecten zijn. Maar als u hebt gestart met een Azure AD-tenant, gevuld zijn met gebruikers en andere objecten en wilt u nu verbinding maken met de, en in dit onderwerp is voor u.

## <a name="the-basics"></a>De basisbeginselen
Een object in Azure AD is ofwel gemaakt in de cloud (Azure AD) of on-premises. Voor één enkel object, kunt u bepaalde kenmerken on-premises en enkele andere kenmerken niet beheren in Azure AD. Elk object heeft een vlag die aangeeft waar het object wordt beheerd.

U kunt sommige gebruikers on-premises en andere beheren in de cloud. Een veelvoorkomend scenario voor deze configuratie is een organisatie met een combinatie van accounting werknemers en verkoop. De accounting werknemers hebben een on-premises AD-account, maar de verkoop werknemers dat niet doet, ze hebben een account in Azure AD. Sommige gebruikers on-premises en soms in Azure AD beheren.

Als u aan de slag voor het beheren van gebruikers in Azure AD die zich ook in on-premises AD en later wilt verbinding maken met de, en er zijn enkele aanvullende problemen u moet overwegen.

## <a name="sync-with-existing-users-in-azure-ad"></a>Synchroniseren met bestaande gebruikers in Azure AD
Wanneer u Azure AD Connect installeert en u begint met het synchroniseren, wordt de Azure AD sync-service (in Azure AD) biedt een controle op elk nieuw object en proberen te vinden van een bestaand object zodat deze overeenkomt met. Er zijn drie kenmerken die worden gebruikt voor dit proces: **userPrincipalName**, **proxyAddresses**, en **sourceAnchor**/**immutableID** . Een overeenkomst op **userPrincipalName** en **proxyAddresses** wordt ook wel een **zachte match**. Een overeenkomst op **sourceAnchor** staat bekend als **harde overeenkomst**. Voor de **proxyAddresses** alleen de waarde met het kenmerk **SMTP:**, dat wil zeggen het primaire e-mailadres wordt gebruikt voor de evaluatie.

De overeenkomst wordt alleen beoordeeld voor nieuwe objecten die afkomstig zijn van Connect. Als u een bestaand object wijzigen zodat deze met een van deze kenmerken overeen komt, klikt u vervolgens ziet u een fout in plaats daarvan.

Als Azure AD vindt een object waar de kenmerkwaarden zijn hetzelfde voor een object die afkomstig zijn van Connect en of deze al aanwezig zijn in Azure AD, wordt opnieuw verbinding maken met het object in Azure AD gebruikt. Het object eerder door de cloud beheerd is gemarkeerd als on-premises worden beheerd. Alle kenmerken in Azure AD met een waarde in de on-premises AD worden overschreven met de on-premises-waarde. De uitzondering is wanneer een kenmerk heeft een **NULL** on-premises-waarde. In dit geval wordt kunt nog steeds de waarde in Azure AD blijven, maar u deze alleen wijzigen on-premises op iets anders.

> [!WARNING]
> Omdat alle kenmerken in Azure AD worden overschreven door de waarde van de on-premises gaat, zorg ervoor dat u goede gegevens on-premises hebt. Bijvoorbeeld, als u alleen e-mailadres in Office 365 beheerde en niet bewaard bijgewerkt on-premises AD DS, en vervolgens u alle waarden in Azure AD/Office 365 niet aanwezig is in AD DS verliest.

> [!IMPORTANT]
> Als u wachtwoord-sync, waarmee altijd door de express-instellingen gebruikt wordt, wordt het wachtwoord in Azure AD wordt overschreven met het wachtwoord in de on-premises AD. Als uw gebruikers worden gebruikt om verschillende wachtwoorden te beheren, moet u laat ze weten dat ze de on-premises wachtwoord gebruiken moeten wanneer u verbinding hebt geïnstalleerd.

De vorige sectie en de waarschuwing moeten worden beschouwd als in uw planning. Als u veel wijzigingen hebt aangebracht in Azure AD niet weerspiegeld in on-premises AD DS, moet u van plan bent voor het vullen van AD DS met de bijgewerkte waarden voordat u uw objecten met Azure AD Connect synchroniseert.

Als u overeenkomen met uw objecten met een zachte match, dan zal de **sourceAnchor** wordt toegevoegd aan het object in Azure AD, zodat een vaste overeenkomst kan later worden gebruikt.

>[!IMPORTANT]
> Microsoft raadt ten zeerste aan op basis van on-premises-accounts synchroniseren met bestaande beheerdersaccounts in Azure Active Directory.

### <a name="hard-match-vs-soft-match"></a>Harde-match vs zachte match
Er is geen praktische verschil tussen een voorlopig- en een vaste-overeenkomst voor een nieuwe installatie van Connect. Het verschil is in een herstel na noodgevallen situatie. Als u uw server met Azure AD Connect is verbroken, kunt u een nieuw exemplaar installeren zonder verlies van gegevens. Een object met een sourceAnchor wordt verzonden naar Connect tijdens de eerste installatie. De overeenkomst kan vervolgens worden geëvalueerd door de client (Azure AD Connect), die veel sneller dan om hetzelfde te doen in Azure AD. Een vaste overeenkomst wordt geëvalueerd door Connect zowel door Azure AD. Een voorlopig overeenkomst wordt alleen beoordeeld door Azure AD.

### <a name="other-objects-than-users"></a>Andere objecten dan gebruikers
Voor groepen met e-mail en contactpersonen, u kunt zachte match op basis van proxyAddresses. Harde-match is niet van toepassing omdat u alleen de sourceAnchor/immutableID (met behulp van PowerShell) voor de gebruikers alleen kunt bijwerken. Voor groepen die niet e-mail zijn, is er momenteel geen ondersteuning voor zachte match of harde-overeenkomst.

## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Maken van een nieuwe on-premises Active Directory van gegevens in Azure AD
Sommige klanten beginnen met een cloud-only-oplossing met Azure AD en ze hebben niet een on-premises AD. Later ze willen gebruiken, on-premises bronnen en wilt maken van een on-premises AD op basis van Azure AD-gegevens. Azure AD Connect helpen u niet voor dit scenario. Maakt geen gebruikers on-premises en heeft geen een mogelijkheid om in te stellen van de on-premises wachtwoord naar hetzelfde als in Azure AD.

Als de enige reden waarom u van plan bent om toe te voegen on-premises AD dat is voor de ondersteuning van LOB's (Line-of-Business-apps), misschien kunt u overwegen gebruik [Azure AD domain services](../../active-directory-domain-services/index.yml) in plaats daarvan.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).
