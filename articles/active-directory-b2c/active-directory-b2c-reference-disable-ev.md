---
title: E-mailverificatie tijdens registratie in Azure Active Directory B2C consumer uitschakelen | Microsoft Docs
description: Een onderwerp het uitschakelen van e-mailverificatie tijdens registratie in Azure Active Directory B2C consumer te demonstreren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 24242054ea4af3797fbeca1ed48bb698e4f4296b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712008"
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Schakel e-mailverificatie tijdens registratie consumer
Wanneer dit is ingeschakeld, kunt Azure Active Directory (Azure AD) B2C een consument zich aanmelden voor toepassingen door een e-mailadres en een lokaal account maken. Azure AD B2C, zorgt u ervoor geldige e-mailadressen doordat consumenten om te controleren of ze tijdens het registratieproces. Dit voorkomt ook dat een kwaadwillende geautomatiseerd proces valse accounts voor de toepassingen te genereren.

Sommige toepassingsontwikkelaars liever overslaan e-mailverificatie tijdens het registratieproces en hebt in plaats daarvan consumenten later controleren van het e-mailadres. Ter ondersteuning hiervan, kan Azure AD B2C worden geconfigureerd voor het e-controle niet uitschakelen. Dit leidt tot een soepeler aanmeldingsproces maakt en biedt ontwikkelaars de flexibiliteit om te onderscheiden van consumenten die hun e-mailadres van consumenten die nog niet hebt geverifieerd.

Registratie beleidsregels hebben standaard e-mailverificatie ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. [Volg deze stappen om te navigeren naar de blade B2C-functies in de Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **aanmelding beleid** of **registreren of aanmelden beleid** afhankelijk van wat u hebt geconfigureerd voor aanmelding.
3. Klik op het beleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen. Klik op **bewerken** boven aan de blade.
4. Klik op **pagina UI-aanpassing**.
5. Klik op **aanmeldpagina voor lokaal account**.
6. Klik op **e-mailadres** in de **naam** kolom onder de **registratiekenmerken** sectie.
7. Schakelen tussen de **verificatie vereisen** optie naar **Nee**.
8. Klik op **OK** onder totdat u de **beleid bewerken** blade.
9. Klik op **opslaan** boven aan de blade. U bent nu klaar!

> [!NOTE]
> Het uitschakelen van e-mailverificatie in het aanmeldingsproces kan leiden tot spam. Als u de standaardtabel uitschakelt, wordt u aangeraden het toevoegen van uw eigen-verificatiesysteem.
> 
> 

We kunnen worden altijd feedback en suggesties! Als u problemen met dit onderwerp ondervindt of aanbevelingen voor het verbeteren van de inhoud hebben, zouden we stellen uw feedback onder aan de pagina. Voor functieaanvragen, voeg deze toe aan [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
