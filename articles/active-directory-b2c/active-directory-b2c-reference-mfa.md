---
title: Multi-factor Authentication in Azure Active Directory B2C | Microsoft Docs
description: Klik hier voor meer informatie over het inschakelen van multi-factor Authentication in consumentgerichte toepassingen die zijn beveiligd door Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: eabae0f3575719c6cb93affefe0a393dd13d1439
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014003"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Schakel multi-factor authentication in Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C wordt rechtstreeks geïntegreerd met [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) zodat u een tweede beveiligingslaag aan zich kunnen registreren en aanmelden ervaringen in uw toepassingen toevoegen kunt. U kunt multi-factor authentication inschakelen zonder één regel code te schrijven. Als u al hebt gemaakt sign up en beleid voor aanmelden, kunt u nog steeds inschakelen met meervoudige verificatie.

Deze functie helpt scenario's zoals de volgende toepassingen:

- U geen multi-factor authentication voor toegang tot een toepassing vereist, maar u nodig hebt tot een andere. Bijvoorbeeld, de klant kan zich aanmelden bij een automatische verzekering toepassing met een sociale- of lokale account, maar het telefoonnummer moet controleren voordat toegang tot de home verzekering toepassing geregistreerd in dezelfde map.
- U multi-factor authentication voor toegang tot een toepassing in het algemeen niet vereist, maar u nodig hebt tot de gevoelige delen binnen het. Bijvoorbeeld, de klant kan zich aanmelden bij een toepassing voor bankieren met een sociale of lokale account en controleer of het account in balans brengen, maar het telefoonnummer moeten controleren voordat u probeert een overschrijving.

## <a name="set-multi-factor-authentication"></a>Verificatie met meerdere factoren instellen

Wanneer u een beleid maakt, hebt u de optie voor het inschakelen van meervoudige verificatie.

![Verificatie met meerdere factoren instellen](./media/active-directory-b2c-reference-mfa/add-policy.png)

Stel **status** naar **op**.

U kunt **nu uitvoeren** op het beleid om te controleren of de ervaring. Controleer of de volgende scenario:

Een klantaccount is gemaakt in uw tenant voordat de stap met meervoudige verificatie plaatsvindt. Tijdens de stap wordt gevraagd de klant een telefoonnummer opgeven en controleren. Als controle geslaagd is, wordt het telefoonnummer dat is gekoppeld aan het account voor later gebruik. Zelfs als de klant annuleert of wegvalt, kan de klant worden gevraagd om te controleren of een telefoonnummer opnieuw tijdens de volgende keer aanmelden met multi-factor authentication is ingeschakeld.

## <a name="add-multi-factor-authentication"></a>Multi-factor authentication toevoegen

Het is mogelijk om in te schakelen van meervoudige verificatie voor een beleid dat u eerder hebt gemaakt. 

Meervoudige verificatie inschakelen:

1. Open het beleid en selecteer vervolgens **bewerken**. 
2. Selecteer **multi-factor authentication**
3. Stel **status** naar **op**.
4. Klik bovenaan de pagina op **Opslaan**.


