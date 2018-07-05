---
title: Multi-factor Authentication in Azure Active Directory B2C | Microsoft Docs
description: Klik hier voor meer informatie over het inschakelen van multi-factor Authentication in consumentgerichte toepassingen die zijn beveiligd door Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3d18e1b2e45aba4e83989e29c533cfc7bf5033fc
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442705"
---
# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Multi-Factor Authentication in uw consumententoepassingen inschakelen
Azure Active Directory (Azure AD) B2C wordt rechtstreeks geïntegreerd met [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) zodat u een tweede beveiligingslaag aan zich kunnen registreren en aanmelden ervaringen in uw consumententoepassingen toevoegen kunt. En u kunt dit doen zonder één regel code te schrijven. We ondersteunen momenteel telefonische oproepen en SMS-bericht-verificatie. Als u beleid voor aanmelden en meld u al hebt gemaakt, kunt u nog steeds multi-factor Authentication inschakelen.

> [!NOTE]
> Multi-factor Authentication kan ook worden ingeschakeld wanneer u zich kunnen registreren en aanmelden beleidsregels, maakt niet alleen met het bewerken van bestaand beleid.
> 
> 

Deze functie helpt scenario's zoals de volgende toepassingen:

* U kunt multi-factor Authentication voor toegang tot een toepassing niet vereist, maar u nodig hebt tot een andere naam. Bijvoorbeeld, de consumenten kan zich aanmelden bij een automatische verzekering toepassing met een sociale- of lokale account, maar het telefoonnummer moet controleren voordat toegang tot de home verzekering toepassing geregistreerd in dezelfde map.
* U multi-factor Authentication voor toegang tot een toepassing in het algemeen niet vereist, maar u nodig hebt tot de gevoelige delen binnen het. Bijvoorbeeld, de consumenten kan zich aanmelden bij een toepassing voor bankieren met een sociale of lokale account en het saldo van selectievakje, maar het telefoonnummer moet controleren voordat u probeert een overschrijving.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Wijzigen van het registratiebeleid voor multi-factor Authentication inschakelen
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **Registratiebeleid**.
3. Klik op het registratiebeleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen.
4. Klik op **multi-factor authentication** en schakelt u de **status** naar **op**. Klik op **OK**.
5. Klik op **opslaan** aan de bovenkant van de blade.

U kunt de functie 'Nu uitvoeren' van het beleid gebruiken om te controleren of de ervaring van consumenten. Bevestig het volgende:

Een consumentenaccount wordt gemaakt in uw directory voordat de multi-factor Authentication-stap plaatsvindt. Tijdens de stap wordt de consument gevraagd opnieuw te zijn of haar telefoonnummer opgeven. Als controle geslaagd is, wordt het telefoonnummer dat is gekoppeld aan het consumentenaccount voor later gebruik. Zelfs als de consument wordt geannuleerd of wegvalt, kan hij of zij worden gevraagd om te controleren of een telefoonnummer opnieuw tijdens de volgende aanmelding (met meervoudige verificatie is ingeschakeld).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Wijzigen van uw beleid aanmelden voordat u multi-factor Authentication inschakelen
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **aanmeldingsbeleid**.
3. Klik op het beleid aanmelden (bijvoorbeeld ' B2C_1_SiIn') om deze te openen. Klik op **bewerken** aan de bovenkant van de blade.
4. Klik op **multi-factor authentication** en schakelt u de **status** naar **op**. Klik op **OK**.
5. Klik op **opslaan** aan de bovenkant van de blade.

U kunt de functie 'Nu uitvoeren' van het beleid gebruiken om te controleren of de ervaring van consumenten. Bevestig het volgende:

Wanneer de gebruiker zich aanmeldt (met behulp van een sociale- of lokale account), als een geverifieerde telefoonnummer in dat is gekoppeld aan het consumentenaccount, hij of zij gevraagd om het te verifiëren. Als er geen telefoonnummer dat is gekoppeld, wordt de consument gevraagd opnieuw te leveren. Op een geslaagde verificatie worden het telefoonnummer dat is gekoppeld aan de consumentenaccount voor later gebruik.

## <a name="multi-factor-authentication-on-other-policies"></a>Meervoudige verificatie voor andere beleidsregels
Zoals wordt beschreven voor registratie en aanmelding bovenstaande beleid, het is ook mogelijk om in te schakelen van meervoudige verificatie voor aanmelding bij of beleid voor aanmelden en het wachtwoord opnieuw instellen van beleidsregels. Dit wordt binnenkort op beleid voor profielbewerking beschikbaar zijn.

