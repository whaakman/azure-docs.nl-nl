---
title: Basislijn-beleid MFA vereisen voor servicebeheer (preview) - Azure Active Directory
description: Beleid voor voorwaardelijke toegang om MFA te vereisen voor Azure Resource Manager
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 203b752f9da67ebf60e373fe7ce0893b4fd7fcb5
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560966"
---
# <a name="baseline-policy-require-mfa-for-service-management-preview"></a>Beleid: MFA vereisen voor servicebeheer (preview)

Mogelijk dat u een verscheidenheid aan Azure-services worden gebruikt in uw organisatie. Deze services kunnen worden beheerd via Azure Resource Manager-API:

* Azure Portal
* Azure PowerShell
* Azure-CLI

Met Azure Resource Manager om uw services te beheren, is een bijzondere rechten actie. Azure Resource Manager kunt wijzigen en tenant-brede configuraties, zoals service-instellingen en abonnement facturering. Er is één-factor authentication kwetsbaar voor tal van aanvallen zoals phishing en het wachtwoord spray. Het is daarom belangrijk om te controleren of de identiteit van gebruikers die toegang tot Azure Resource Manager en bijwerken van configuraties met meervoudige verificatie alvorens toegang vereisen.

**MFA vereisen voor servicebeheer** is een [Basisbeleid](concept-baseline-protection.md) dat MFA moet voor elke gebruiker toegang tot Azure portal, Azure PowerShell of Azure CLI. Dit beleid is van toepassing op alle gebruikers toegang hebben tot Azure Resource Manager, ongeacht als ze een beheerder bent.

Zodra dit beleid is ingeschakeld in een tenant, worden alle gebruikers die zich aanmeldt bij Azure-beheerresources wordt gevraagd multi-factor Authentication. Als de gebruiker is niet geregistreerd voor MFA, wordt de gebruiker moet registreren met behulp van de Microsoft Authenticator-App om door te gaan.

Om uit te voeren interactieve aanmelding met [Azure Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps), gebruikt u de [Connect AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet.

```PowerShell
Connect-AzAccount
```

Wanneer deze cmdlet wordt uitgevoerd, vertegenwoordigt deze een tokenreeks. Als u wilt aanmelden, kopieert u deze tekenreeks en plak deze in [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin)  in een browser. Uw PowerShell-sessie wordt geverifieerd om verbinding te maken met Azure.

Om uit te voeren interactieve aanmelding met [Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest), voert de [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) opdracht.

```azurecli
az login
```

Als de CLI uw standaardbrowser kan openen, gebeurt dat ook en wordt er een aanmeldingspagina gedownload. Anders moet u een browserpagina openen en volg de instructies op de opdrachtregel voor het invoeren van een autorisatiecode na het navigeren naar [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) in uw browser. Daarna, meld u aan met de referenties van uw account in de browser.

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Omdat de **MFA vereisen voor servicebeheer** beleid wordt toegepast op alle gebruikers van Azure Resource Manager, verschillende overwegingen hoeven te worden aangebracht in een probleemloze implementatie. Deze overwegingen zijn het identificeren van gebruikers en principes van de service in Azure AD die niet kunnen of moeten niet worden uitgevoerd, MFA, evenals toepassingen en clients die worden gebruikt door uw organisatie die geen ondersteuning voor moderne verificatie.

## <a name="enable-the-baseline-policy"></a>De basislijn-beleid inschakelen

Het beleid **Basisbeleid: MFA vereisen voor servicebeheer (preview)** wordt al geconfigureerd geleverd en worden weergegeven aan de bovenkant wanneer u gaat u naar de blade voor voorwaardelijke toegang in Azure portal.

Dit beleid inschakelt en beveiligen van uw beheerders:

1. Aanmelden bij de **Azure-portal** als hoofdbeheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **voorwaardelijke toegang**.
1. Selecteer in de lijst met beleidsregels **Basisbeleid: MFA vereisen voor servicebeheer (preview)** .
1. Stel **beleid inschakelen** naar **beleid direct gebruiken**.
1. Klik op **opslaan**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

* [Beleid voor voorwaardelijke toegang basislijn-beveiliging](concept-baseline-protection.md)
* [Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten](../../security/azure-ad-secure-steps.md)
* [Wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)