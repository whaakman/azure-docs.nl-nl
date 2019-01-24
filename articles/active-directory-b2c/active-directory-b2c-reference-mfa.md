---
title: Multi-factor Authentication in Azure Active Directory B2C | Microsoft Docs
description: Klik hier voor meer informatie over het inschakelen van multi-factor Authentication in consumentgerichte toepassingen die zijn beveiligd door Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9dd69c7dd43e7dd709a32c1d869252ed6a495666
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853677"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Schakel multi-factor authentication in Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C wordt rechtstreeks geïntegreerd met [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) zodat u een tweede beveiligingslaag aan zich kunnen registreren en aanmelden ervaringen in uw toepassingen toevoegen kunt. U kunt multi-factor authentication inschakelen zonder één regel code te schrijven. Als u al sign up hebt gemaakt en aanmelden gebruiker stromen, kunt u nog steeds met meervoudige verificatie inschakelen.

Deze functie helpt scenario's zoals de volgende toepassingen:

- U geen multi-factor authentication voor toegang tot een toepassing vereist, maar u nodig hebt tot een andere. Bijvoorbeeld, de klant kan zich aanmelden bij een automatische verzekering toepassing met een sociale- of lokale account, maar het telefoonnummer moet controleren voordat toegang tot de home verzekering toepassing geregistreerd in dezelfde map.
- U multi-factor authentication voor toegang tot een toepassing in het algemeen niet vereist, maar u nodig hebt tot de gevoelige delen binnen het. Bijvoorbeeld, de klant kan zich aanmelden bij een toepassing voor bankieren met een sociale of lokale account en controleer of het account in balans brengen, maar het telefoonnummer moeten controleren voordat u probeert een overschrijving.

## <a name="set-multi-factor-authentication"></a>Verificatie met meerdere factoren instellen

Wanneer u een beleid maakt, hebt u de optie voor het inschakelen van meervoudige verificatie.

![Verificatie met meerdere factoren instellen](./media/active-directory-b2c-reference-mfa/add-policy.png)

Stel **Multifactor-verificatie** naar **ingeschakeld**.

U kunt **gebruikersstroom uitvoeren** om te controleren of de ervaring. Controleer of de volgende scenario:

Een klantaccount is gemaakt in uw tenant voordat de stap met meervoudige verificatie plaatsvindt. Tijdens de stap wordt gevraagd de klant een telefoonnummer opgeven en controleren. Als controle geslaagd is, wordt het telefoonnummer dat is gekoppeld aan het account voor later gebruik. Zelfs als de klant annuleert of wegvalt, kan de klant worden gevraagd om te controleren of een telefoonnummer opnieuw tijdens de volgende keer aanmelden met multi-factor authentication is ingeschakeld.

## <a name="add-multi-factor-authentication"></a>Multi-factor authentication toevoegen

Het is mogelijk om in te schakelen van meervoudige verificatie voor een beleid dat u eerder hebt gemaakt. 

Meervoudige verificatie inschakelen:

1. De gebruikersstroom openen en selecteer vervolgens **eigenschappen**. 
2. Naast **Multifactor-verificatie**, selecteer **ingeschakeld**.
3. Klik bovenaan de pagina op **Opslaan**.


