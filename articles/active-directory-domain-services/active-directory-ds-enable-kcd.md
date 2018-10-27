---
title: 'Azure Active Directory Domain Services: Kerberos-beperkte overdracht inschakelen | Microsoft Docs'
description: Kerberos-beperkte overdracht op de beheerde domeinen voor Azure Active Directory Domain Services inschakelen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: da7909853d47f680726464d6d646ca87028fd311
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158779"
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Kerberos-beperkte delegatie (KCD) configureren op een beheerd domein
Veel toepassingen nodig voor toegang tot resources in de context van de gebruiker. Active Directory ondersteunt een mechanisme voor Kerberos-overdracht, waardoor deze use-case genoemd. U kunt bovendien delegering beperken zodat alleen bepaalde resources kunnen worden geopend in de context van de gebruiker. Azure AD Domain Services beheerde domeinen zijn anders dan traditionele Active Directory-domeinen, omdat ze veiliger zijn vergrendeld.

In dit artikel leest u hoe het Kerberos-beperkte overdracht configureren op een beheerd domein van Azure AD Domain Services.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="kerberos-constrained-delegation-kcd"></a>Kerberos-beperkte delegatie (KCD)
Kerberos-overdracht kunt een account te imiteren een andere beveiligings-principal (zoals een gebruiker) voor toegang tot resources. Houd rekening met een webtoepassing die toegang heeft tot een back-end-web-API in de context van een gebruiker. In dit voorbeeld wordt in de web-App (die wordt uitgevoerd in de context van een serviceaccount of een computer/apparaat) de gebruiker geïmiteerd bij het openen van de resource (back-end web-API). Kerberos-delegatie is onveilig omdat deze niet de resources die de imitatie account toegang heeft tot in de context van de gebruiker wordt beperkt.

Kerberos-beperkte delegatie (KCD) Hiermee beperkt u de services/resources waartoe de opgegeven server namens een gebruiker kan fungeren. Traditionele KCD vereist domein administrator-bevoegdheden voor het configureren van een domeinaccount voor een service en daardoor wordt het account beperkt tot één domein.

Traditionele KCD heeft ook enkele problemen die ermee verbonden zijn. Als beheerder van het domein geconfigureerd KCD op basis van een account voor de service, heeft de servicebeheerder in eerdere besturingssystemen, geen handige manier achterhalen welke front-end-services gedelegeerd naar de bronservices die hij of zij eigenaar. En elke front-endservice die kon worden overgedragen aan een resource-service een potentieel van de aanval weergegeven. Als een server die door een front-end-service wordt gehost is aangetast en deze is geconfigureerd om te worden overgedragen aan bronservices, kunnen ook de bronservices gevaar.

> [!NOTE]
> Op een Azure AD Domain Services beheerde domein, doet u domeinbeheerdersbevoegdheden hebt. Daarom **traditionele KCD op basis van een account kan niet worden geconfigureerd in een beheerd domein**. Gebruik op basis van een resource KCD zoals beschreven in dit artikel. Dit mechanisme is ook beter te beveiligen.
>
>

## <a name="resource-based-kcd"></a>KCD op basis van resource
Windows Server 2012 en hoger krijgen servicebeheerders de mogelijkheid om te configureren van beperkte delegering voor hun service. In dit model, kan de back-end-service-beheerder toestaan of weigeren van specifieke front-endservices KCD te gebruiken. Dit model wordt ook wel **op basis van een resource KCD**.

KCD op basis van een bron is geconfigureerd met behulp van PowerShell. U gebruikt de `Set-ADComputer` of `Set-ADUser` cmdlets, afhankelijk van of de imitatie-account een computeraccount of een gebruiker account/service-account.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>KCD op basis van een resource voor een computeraccount configureren op een beheerd domein
Stel dat u hebt een WebApp die wordt uitgevoerd op de computer ' contoso100-webapp.contoso100.com'. Deze moet toegang hebben tot de resource (een web-API die wordt uitgevoerd op ' contoso100-api.contoso100.com') in de context van gebruikers van een domein. Hier ziet hoe u resources gebaseerde KCD voor dit scenario zou instellen:

1. [Maak een aangepaste organisatie-eenheid](active-directory-ds-admin-guide-create-ou.md). U kunt overdragen van machtigingen voor het beheren van deze aangepaste organisatie-eenheid voor gebruikers binnen het beheerde domein.
2. Beide virtuele machines (het uitvoeren van de WebApp en de uitvoeren van de web-API) toevoegen aan het beheerde domein. Deze computeraccounts in de aangepaste organisatie-eenheid maken.
3. Nu configureren op basis van een resource KCD met behulp van de volgende PowerShell-opdracht:

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> De computeraccounts voor de web-app en de web-API moeten zich in een aangepaste organisatie-eenheid waar u gemachtigd bent om te configureren op basis van een resource KCD. U kunt KCD op basis van een resource voor een computeraccount niet configureren in de ingebouwde 'AAD DC Computers'-container.
>

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>KCD op basis van een resource voor een gebruikersaccount configureren op een beheerd domein
Stel dat u hebt een WebApp die wordt uitgevoerd als een serviceaccount 'appsvc' en er nodig voor toegang tot de resource (een web-API die wordt uitgevoerd als een service-account - 'backendsvc') in de context van Domeingebruikers. Hier ziet hoe u resources gebaseerde KCD voor dit scenario wilt instellen.

1. [Maak een aangepaste organisatie-eenheid](active-directory-ds-admin-guide-create-ou.md). U kunt overdragen van machtigingen voor het beheren van deze aangepaste organisatie-eenheid voor gebruikers binnen het beheerde domein.
2. Neem deel aan de virtuele machine met de back-end web API-resource met het beheerde domein. De computeraccount binnen de aangepaste organisatie-eenheid maken.
3. Het serviceaccount (bijvoorbeeld ' appsvc') gebruikt voor het uitvoeren van de web-app in de aangepaste organisatie-eenheid maken.
4. Nu configureren op basis van een resource KCD met behulp van de volgende PowerShell-opdracht:

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> Het computeraccount voor de back-end web-API en de service-account moeten zich in een aangepaste organisatie-eenheid waar u gemachtigd bent om te configureren op basis van een resource KCD. U configureren niet op basis van een resource KCD voor een account in de ingebouwde 'AAD DC Computers' container of voor gebruikersaccounts in de ingebouwde 'AAD DC-gebruikers'-container. U kunt de gebruikersaccounts die zijn gesynchroniseerd met Azure AD dus niet gebruiken om in te stellen op basis van een resource KCD.
>

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Kerberos-beperkte overdracht-overzicht](https://technet.microsoft.com/library/jj553400.aspx)
