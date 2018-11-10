---
title: E-mailverificatie tijdens de registratie in Azure Active Directory B2C consument uitschakelen | Microsoft Docs
description: Een onderwerp laat zien hoe u e-mailverificatie tijdens de registratie in Azure Active Directory B2C consument uitschakelen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e36dd19aa020b8cb2a623cda904cf7fa8a0b26da
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004588"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Tijdens de registratie in Azure Active Directory B2C consument e-mailverificatie uitschakelen 
Wanneer dit is ingeschakeld, biedt Azure Active Directory (Azure AD) B2C een gebruiker de mogelijkheid om u te registreren voor toepassingen door een e-mailadres en het maken van een lokaal account. Azure AD B2C zorgt ervoor dat geldige e-mailadressen doordat gebruikers om te controleren of ze tijdens het registratieproces. Dit voorkomt ook dat een kwaadwillende geautomatiseerd proces valse accounts voor de toepassingen die worden gegenereerd.

Sommige toepassingsontwikkelaars liever het overslaan van e-mailverificatie tijdens het registratieproces en hebt in plaats daarvan het e-mailadres later controleren of consumers. Ter ondersteuning hiervan, kan Azure AD B2C worden geconfigureerd voor het e-mailverificatie uitschakelen. In dat geval maakt u een soepeler aanmeldingsproces en biedt ontwikkelaars de flexibiliteit om te onderscheiden van de consumenten die hun e-mailadres van deze consumenten die nog niet hebt geverifieerd.

Standaard hebben registratiebeleid e-mailverificatie is ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. Klik op **registratiebeleid** of **beleid voor registreren of aanmelden** , afhankelijk van wat u hebt geconfigureerd voor registratie.
2. Klik op het beleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen. 
3. Klik op **bewerken** aan de bovenkant van de blade.
4. Klik op **aanpassing van de gebruikersinterface van de pagina**.
5. Klik op **pagina voor het registreren van lokale account**.
6. Klik op **e-mailadres** in de **naam** kolom onder de **registratiekenmerken** sectie.
7. In-/ uitschakelen de **verificatie vereisen** optie naar **Nee**.
8. Klik op **OK** onder totdat de **beleid bewerken** blade.
9. Klik op **Opslaan** boven aan de blade. U bent klaar.

> [!NOTE]
> Uitschakelen van e-mailverificatie in het registratieproces kan leiden tot ongewenste e-mail. Als u de standaardwaarde uitschakelt, kunt u het beste uw eigen verificatiesysteem toevoegen.
> 
>