---
title: bestand opnemen
description: include-bestand met de vereisten
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 66b5f8e2-e08d-43c8-b460-6204aecda8f7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.custom: include file
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: 1fba8cc9ae40cf5539016bbd73de65f557a64136
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66111389"
---
> [!IMPORTANT]
> **Synchronisatie van wachtwoordhashes met Azure AD Domain Services inschakelen voordat u de taken in dit artikel hebt voltooid.**
>
> Volg de instructies hieronder, afhankelijk van het type gebruikers in uw Azure AD-directory. Voltooi beide sets instructies als u een combinatie van alleen in de cloud en gesynchroniseerde gebruikersaccounts in uw Azure AD-directory. Kunt u mogelijk niet de volgende bewerkingen uitvoeren als u probeert een B2B-Gast-account (bijvoorbeeld uw gmail- of MSA van een andere id-provider waarmee we) te gebruiken omdat er geen het wachtwoord voor deze gebruikers die zijn gesynchroniseerd met het beheerde domein als deze Gast-account in de map zijn. De volledige informatie over deze accounts met inbegrip van hun wachtwoord zou buiten Azure AD en als deze gegevens niet in Azure AD zijn daarom deze komt niet zelfs ophalen die zijn gesynchroniseerd met het beheerde domein. 
> - [Instructies voor gebruikersaccounts die alleen in de cloud](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)
> - [Instructies voor gebruikersaccounts die zijn gesynchroniseerd vanuit een on-premises directory](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant.md)
