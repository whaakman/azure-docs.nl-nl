---
title: 'Azure Active Directory B2C: Multi-Factor Authentication | Microsoft Docs'
description: Het inschakelen van multi-factor Authentication in consumententoepassingen beveiligd door Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 62ec48ab067cf02bc8409aca6da704a5418ec270
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Multi-Factor Authentication in uw consumententoepassingen inschakelen
Azure Active Directory (Azure AD) B2C wordt rechtstreeks geïntegreerd met [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) zodat u een tweede beveiligingslaag aan de ervaringen van registreren en aanmelden in uw consumententoepassingen toevoegen kunt. En u kunt dit doen zonder een één regel code te schrijven. Wij ondersteunen momenteel verificatie voor telefoongesprekken en SMS-bericht. Als u zich kunnen registreren en aanmelden beleid hebt gemaakt, kunt u nog steeds multi-factor Authentication inschakelen.

> [!NOTE]
> Multi-factor Authentication kan ook worden ingeschakeld wanneer u zich kunnen registreren en aanmelden beleidsregels, niet alleen bestaande beleidsregels bewerken.
> 
> 

Deze functie helpt bij het verwerken van scenario's zoals de volgende toepassingen:

* U multi-factor Authentication toegang tot een toepassing niet vereist, maar u hoeft deze toegang krijgt tot een andere naam. Bijvoorbeeld, de consument kunt aanmelden bij een automatische verzekering toepassing met een sociale of lokale account, maar het telefoonnummer moet verifiëren voordat toegang tot de thuis verzekering toepassing geregistreerd in dezelfde map.
* U multi-factor Authentication toegang tot een toepassing in het algemeen niet vereist, maar u deze toegang krijgt tot gevoelige gedeelten binnen dit vereist. Bijvoorbeeld, de consument kunt aanmelden bij een toepassing bankieren met een sociale of lokale account en het saldo van controle, maar het telefoonnummer moet verifiëren voordat u probeert een overschrijving.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Wijzigen van het registratiebeleid voor multi-factor Authentication inschakelen
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in de Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **Registratiebeleid**.
3. Klik op het registratiebeleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen.
4. Klik op **multi-factor authentication** en schakel de **status** naar **ON**. Klik op **OK**.
5. Klik op **opslaan** boven aan de blade.

U kunt de functie 'Nu uitvoeren' op het beleid gebruiken om te controleren of de consumer-ervaring. Bevestig het volgende:

Een consumentenaccount wordt gemaakt in uw directory voordat de multi-factor Authentication-stap plaatsvindt. Tijdens de stap wordt de gebruiker gevraagd om zijn of haar telefoonnummer opgeven en deze verifiëren. Als controle geslaagd is, wordt het telefoonnummer dat is gekoppeld aan het consumentenaccount voor later gebruik. Zelfs als de consument wordt geannuleerd of wegvalt, kan hij of zij worden gevraagd om te controleren of een telefoonnummer opnieuw tijdens de volgende keer aanmelden (met multi-factor Authentication is ingeschakeld).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Wijzigen van uw beleid voor aanmelden voordat u multi-factor Authentication inschakelen
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in de Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **aanmelden beleid**.
3. Klik op de aanmeldingspagina beleid (bijvoorbeeld ' B2C_1_SiIn') om deze te openen. Klik op **bewerken** boven aan de blade.
4. Klik op **multi-factor authentication** en schakel de **status** naar **ON**. Klik op **OK**.
5. Klik op **opslaan** boven aan de blade.

U kunt de functie 'Nu uitvoeren' op het beleid gebruiken om te controleren of de consumer-ervaring. Bevestig het volgende:

Wanneer de gebruiker zich aanmeldt (met behulp van een sociale of lokale account) als een geverifieerde telefoonnummer in dat is gekoppeld aan het account van verbruiker, hij of zij is gevraagd te verifiëren. Als er geen telefoonnummer dat is gekoppeld, worden de consument wordt gevraagd om opgeeft en bevestig dit. Op een geslaagde verificatie is het telefoonnummer dat is gekoppeld aan het consumentenaccount voor later gebruik.

## <a name="multi-factor-authentication-on-other-policies"></a>Multi-factor Authentication op andere beleidsregels
Zoals wordt beschreven voor bovenstaande beleid voor aanmelden en aanmelden, het is ook mogelijk om in te schakelen van meervoudige authenticatie aanmelding of beleidsregels voor aanmelden en het wachtwoord opnieuw instellen beleidsregels. Deze zijn beschikbaar snel op profiel bewerken van beleid.

