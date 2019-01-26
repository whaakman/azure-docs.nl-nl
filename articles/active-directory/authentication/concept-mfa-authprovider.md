---
title: Wanneer en hoe u Azure multi-factor Authentication-Provider?
description: Wanneer moet u een verificatieprovider gebruiken met Azure MFA?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 2d7156a1387184cff2eb4252648f0e844e8b1f50
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55079704"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Wanneer u een Azure multi-factor Authentication-Provider

Verificatie in twee stappen is standaard beschikbaar voor globale beheerders van Azure Active Directory- en Office 365-gebruikers. Als u echter wilt profiteren van [geavanceerde functies](howto-mfa-mfasettings.md), moet u de volledige versie van Azure Multi-Factor Authentication (MFA) aanschaffen.

Azure multi-factor Authentication-Provider wordt gebruikt om te profiteren van functies van Azure multi-factor Authentication voor gebruikers die **geen licenties**.

Als u licenties die betrekking hebben op alle gebruikers in uw organisatie hebt, klikt u vervolgens hoeft niet u Azure multi-factor Authentication-Provider. Maak een Azure multi-factor Authentication-Provider alleen als u moet ook opgeven de verificatie in twee stappen voor gebruikers die geen licenties.

> [!NOTE]
> Vanaf 1 September 2018 nieuwe auth-providers kunnen niet meer worden gemaakt. Bestaande auth-providers kunnen nog steeds worden gebruikt en bijgewerkt. Meervoudige verificatie blijft beschikbaar als een functie in Azure AD Premium-licenties.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Waarschuwingen met betrekking tot de Azure MFA-SDK

Houd er rekening mee de SDK is gedeprecieerd en wordt alleen blijven werken tot en met 14 November 2018. Na deze datum kan de SDK niet meer worden aangeroepen.

## <a name="what-is-an-mfa-provider"></a>Wat is een MFA-provider?

Er zijn twee soorten verificatieproviders en is het onderscheid te maken over hoe uw Azure-abonnement wordt in rekening gebracht. De optie Per verificatie berekent het aantal verificaties die in een maand op uw tenant worden uitgevoerd. Deze optie wordt aanbevolen als u een aantal gebruikers hebt die slechts sporadisch verifiëren. De optie Per gebruiker berekent het aantal personen in uw tenant die in een maand verificatie in twee stappen uitvoeren. Deze optie wordt aanbevolen als u een aantal gebruikers met licenties hebt, maar MFA wilt uitbreiden naar meer gebruikers buiten de restricties van uw licentie.

## <a name="manage-your-mfa-provider"></a>Uw MFA-provider beheren

U kunt het gebruiksmodel (per ingeschakelde gebruiker of per verificatie) niet wijzigen nadat een MFA-provider is gemaakt.

Als u voldoende licenties voor alle gebruikers die zijn ingeschakeld voor MFA hebt aangeschaft, kunt u de MFA-provider helemaal verwijderen.

Als de MFA-provider niet is gekoppeld aan een Azure AD-tenant, of als u de nieuwe MFA-provider koppelt aan een andere Azure AD-tenant, worden de gebruikersinstellingen en configuratieopties niet overgedragen. Bestaande Azure MFA-Servers moet ook worden geactiveerd met behulp van de activeringsreferenties die worden gegenereerd door de MFA-Provider. Opnieuw activeren van de MFA-Servers om ze te koppelen aan de MFA-Provider niet van invloed op telefonische oproepen en tekstberichtverificatie, maar de mobiele app-meldingen werken voor alle gebruikers niet meer totdat ze de mobiele app activeren.

## <a name="next-steps"></a>Volgende stappen

[Multi-Factor Authentication-instellingen configureren](howto-mfa-mfasettings.md)
