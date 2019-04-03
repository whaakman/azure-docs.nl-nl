---
title: Definities van de cookie - Azure Active Directory B2C | Microsoft Docs
description: Bevat definities voor de cookies die worden gebruikt in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ac422a00a919903063c96ac096882036b99a63e3
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887226"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Definities van cookies voor Azure Active Directory B2C

De volgende tabel bevat de cookies die worden gebruikt in Azure Active Directory B2C.

| Name | Domain | Verlooptijd | Doel |
| ----------- | ------ | -------------------------- | --------- |
| x-ms-cpim-admin | main.b2cadmin.ext.azure.com | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Bevat de groepslidmaatschap gebruikersgegevens voor tenants. De tenants een gebruiker is lid van de en niveau van het lidmaatschap (beheerder of gebruiker). |
| x-ms-cpim-slice | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Gebruikt voor het routeren van aanvragen naar de juiste productie-instantie. |
| x-ms-cpim-trans | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Gebruikt voor het bijhouden van de transacties (aantal verificatieaanvragen naar Azure AD B2C) en de huidige transactie. |
| x-ms-cpim-sso:{Id} | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Gebruikt voor het onderhouden van de sessie voor eenmalige aanmelding. |
| x-ms-cpim-cache:{id}_n | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md), verificatie is geslaagd | Gebruikt voor het onderhouden van de status van de aanvraag. |
| x-ms-cpim-csrf | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Cross-Site aanvragen kunnen worden vervalst token dat wordt gebruikt voor CRSF beveiliging. |
| x-ms-cpim-dc | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Gebruikt voor de routering van Azure AD B2C. |
| x-ms-cpim-ctx | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Context |
| x-ms-cpim-rp | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Gebruikt voor het opslaan van gegevens betreffende het groepslidmaatschap voor de resource provider-tenant. |
| x-ms-cpim-rc | Login.microsoftonline.com, b2clogin.com, huisstijl domein | Einde van de [browsersessie](active-directory-b2c-token-session-sso.md) | Gebruikt voor het opslaan van de relay-cookie. |

