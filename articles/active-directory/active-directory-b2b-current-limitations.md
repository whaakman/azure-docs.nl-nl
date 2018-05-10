---
title: Beperkingen van Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Huidige beperkingen voor Azure Active Directory B2B-samenwerking
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/23/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: d4efa6ad1d2768a41a670fae4c490942b5a814ad
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Beperkingen van Azure AD B2B-samenwerking
Azure Active Directory (Azure AD) B2B-samenwerking is momenteel onderworpen aan de beperkingen die in dit artikel wordt beschreven.

## <a name="possible-double-multi-factor-authentication"></a>Mogelijke dubbele multi-factor authentication-server
Met Azure AD B2B, kunt u multi-factor authentication op de bronorganisatie (de organisatie uitnodigen) afdwingen. De redenen voor deze benadering zijn aangegeven in [voorwaardelijke toegang voor gebruikers voor B2B-samenwerking](active-directory-b2b-mfa-instructions.md). Als een partner al meervoudige verificatie instellen en het systeem is, wordt hun gebruikers wellicht één keer de verificatie uitvoeren binnen hun organisatie thuis en vervolgens opnieuw in jouw e-mailadres.

## <a name="instant-on"></a>Instant op
In het stroomt B2B-samenwerking we gebruikers toevoegen aan de map en ze dynamisch bijwerken tijdens uitnodiging inwisseling en app-toewijzing. De updates- en schrijfbewerkingen normaal gebeuren in een directory-exemplaar en in alle exemplaren moeten worden gerepliceerd. Replicatie is voltooid zodra alle exemplaren worden bijgewerkt. Soms wanneer het object wordt geschreven of bijgewerkt in één exemplaar en de aanroep voor het ophalen van dit object naar een ander exemplaar is, kunnen de replicatielatentie optreden. Als dat gebeurt, vernieuwen of probeer om te helpen. Als u een app met behulp van onze API schrijft, is opnieuw wordt geprobeerd met sommige terug uit een goede, defensive procedure voor het verlichten van dit probleem.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen over Azure AD B2B-samenwerking:

- [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [B2bB samenwerking uitnodigingen delegeren](active-directory-b2b-delegate-invitations.md)

