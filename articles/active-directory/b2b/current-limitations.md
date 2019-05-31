---
title: 'Beperkingen van B2B-samenwerking: Azure Active Directory | Microsoft Docs'
description: Huidige beperkingen voor Azure Active Directory B2B-samenwerking
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: d1466614dfeb8fa4d3d095bae070d01c6503c5bb
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357245"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Beperkingen van Azure AD B2B-samenwerking
Azure Active Directory (Azure AD) B2B-samenwerking is momenteel onderhevig aan de beperkingen die in dit artikel wordt beschreven.

## <a name="possible-double-multi-factor-authentication"></a>Mogelijke dubbele meervoudige verificatie
Met Azure AD B2B, kunt u multi-factor authentication op de bronorganisatie (de uitnodigende organisatie) afdwingen. De redenen voor deze methode worden beschreven in [voorwaardelijke toegang voor gebruikers van B2B-samenwerking](conditional-access.md). Als een partner al multi-factor authentication instellen en afgedwongen, hun gebruikers mogelijk moeten de verificatie één keer uitgevoerd in hun organisatie die thuis en vervolgens opnieuw in kunt kiezen.

## <a name="instant-on"></a>Chatberichten op
In de B2B-samenwerking stromen, we gebruikers toevoegen aan de directory en deze dynamisch bijgewerkt tijdens de uitnodiging inwisselen, app-toewijzing, enzovoort. De updates- en schrijfbewerkingen gewoonlijk gebeuren in een directory-exemplaar en over alle exemplaren moeten worden gerepliceerd. Replicatie is voltooid zodra alle exemplaren worden bijgewerkt. Soms wanneer het object wordt geschreven of bijgewerkt in één exemplaar en de aanroep om op te halen van dit object naar een andere instantie is, kunnen replicatielatentie optreden. Als dit gebeurt, vernieuwen of probeer om te helpen. Als u een app met behulp van onze API schrijft, wordt nieuwe pogingen met sommige uitstel goed, verdediging om te verlichten van dit probleem.

## <a name="azure-ad-directories"></a>Azure AD-directory 's
Azure AD B2B is onderworpen aan Azure AD-directory limieten service. Voor meer informatie over het aantal mappen een gebruiker kunt maken en het aantal mappen aan die een gebruiker of een gastgebruiker deel uitmaken kan, Zie [limieten en beperkingen voor Azure AD-service](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions).

## <a name="national-clouds"></a>Nationale clouds
[Nationale clouds](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud) zijn fysiek geïsoleerd exemplaren van Azure. B2B-samenwerking wordt niet ondersteund grenzen voor nationale cloud. Als uw Azure-tenant in de cloud openbare, wereldwijde, niet kan u bijvoorbeeld een gebruiker waarvan het account zich in een nationale cloud uitnodigen. Als u wilt samenwerken met de gebruiker, vraag om een ander e-mailadres of een lid-gebruikersaccount maken voor deze in uw directory.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen op Azure AD B2B-samenwerking:

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [B2B-samenwerking uitnodigingen delegeren](delegate-invitations.md)

