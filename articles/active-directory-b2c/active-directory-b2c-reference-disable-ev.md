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
ms.openlocfilehash: e008fb87b57b92f8f7e914e6b4344b52d42f9ef8
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263926"
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Disable e-mailverificatie tijdens de consumer registreren
Wanneer dit is ingeschakeld, biedt Azure Active Directory (Azure AD) B2C een gebruiker de mogelijkheid om u te registreren voor toepassingen door een e-mailadres en het maken van een lokaal account. Azure AD B2C zorgt ervoor dat geldige e-mailadressen doordat gebruikers om te controleren of ze tijdens het registratieproces. Dit voorkomt ook dat een kwaadwillende geautomatiseerd proces valse accounts voor de toepassingen die worden gegenereerd.

Sommige toepassingsontwikkelaars liever het overslaan van e-mailverificatie tijdens het registratieproces en hebt in plaats daarvan het e-mailadres later controleren of consumers. Ter ondersteuning hiervan, kan Azure AD B2C worden geconfigureerd voor het e-mailverificatie uitschakelen. In dat geval maakt u een soepeler aanmeldingsproces en biedt ontwikkelaars de flexibiliteit om te onderscheiden van de consumenten die hun e-mailadres van deze consumenten die nog niet hebt geverifieerd.

Standaard hebben registratiebeleid e-mailverificatie is ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. [Volg deze stappen om te navigeren naar de blade B2C-functies in Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **registratiebeleid** of **beleid voor registreren of aanmelden** , afhankelijk van wat u hebt geconfigureerd voor registratie.
3. Klik op het beleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen. 
4. Klik op **bewerken** aan de bovenkant van de blade.
5. Klik op **aanpassing van de gebruikersinterface van de pagina**.
6. Klik op **pagina voor het registreren van lokale account**.
7. Klik op **e-mailadres** in de **naam** kolom onder de **registratiekenmerken** sectie.
8. In-/ uitschakelen de **verificatie vereisen** optie naar **Nee**.
9. Klik op **OK** onder totdat de **beleid bewerken** blade.
10. Klik op **opslaan** aan de bovenkant van de blade. U bent klaar.

> [!NOTE]
> Uitschakelen van e-mailverificatie in het registratieproces kan leiden tot ongewenste e-mail. Als u de standaardwaarde uitschakelt, kunt u het beste uw eigen verificatiesysteem toevoegen.
> 
> 

We zijn altijd geopend zodat u kunt feedback en suggesties. Als u problemen met dit onderwerp ondervindt of aanbevelingen hebt voor het verbeteren van de inhoud, zouden we waarderen uw feedback aan de onderkant van de pagina. Voor functieaanvragen, voeg deze toe aan [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
