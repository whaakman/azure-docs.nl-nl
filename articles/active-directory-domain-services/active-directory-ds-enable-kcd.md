---
title: 'Azure Active Directory Domain Services: Kerberos-beperkte delegatie inschakelen | Microsoft Docs'
description: Kerberos-beperkte overdracht op beheerde domeinen voor Azure Active Directory Domain Services inschakelen
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2018
ms.author: maheshu
ms.openlocfilehash: cd730d2224babe0c44d4c1d90106d44994536d29
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Kerberos-beperkte delegatie (KCD) configureren op een beheerd domein
Veel toepassingen moeten toegang tot bronnen in de context van de gebruiker. Active Directory ondersteunt een mechanisme voor Kerberos-overdracht, waardoor deze gebruiksvoorbeeld. U kunt bovendien delegering beperken zodat alleen bepaalde resources kunnen worden geopend in de context van de gebruiker. Azure AD Domain Services beheerde domeinen zijn anders dan traditionele Active Directory-domeinen, omdat ze veiliger zijn vergrendeld.

In dit artikel leest u hoe Kerberos-beperkte overdracht configureren op een beheerd domein van Azure AD Domain Services.

## <a name="kerberos-constrained-delegation-kcd"></a>Kerberos-beperkte delegatie (KCD)
Kerberos-overdracht kunt een account aan een andere beveiligings-principal (zoals een gebruiker) toegang tot bronnen te imiteren. U kunt een webtoepassing die toegang heeft tot een back-end-web-API in de context van een gebruiker. In dit voorbeeld wordt in de webtoepassing (uitgevoerd in de context van een serviceaccount of een computer/computeraccount) de gebruiker imiteert bij toegang tot de bron (back-end web-API). Kerberos-overdracht is niet veilig omdat deze wordt niet beperkt de imitatie account in de context van de gebruiker toegang heeft tot resources.

sKerberos beperkte delegatie (KCD) Hiermee beperkt u de services/resources waartoe de opgegeven server namens een gebruiker kan fungeren. Traditionele KCD vereist bevoegdheden voor het configureren van een domeinaccount voor een service van een domeinadministrator en het het account op één domein beperkt.

Traditionele KCD heeft ook een aantal aspecten die zijn gekoppeld. Als de domeinbeheerder geconfigureerd KCD op basis van een account voor de service, had de servicebeheerder in eerdere besturingssystemen, geen handige manier achterhalen welke front-end-services overgedragen aan de bronservices die of zij eigenaar was. En elke front-endservice die kon worden overgedragen naar een bronservice vertegenwoordigd een potentiële aanvallen met zich mee. Als een server die als host van een front-end-service is geknoeid en deze is geconfigureerd voor het overgedragen aan bronservices, kunnen ook de bronservices gevaar.

> [!NOTE]
> Op een beheerd domein van Azure AD Domain Services er geen domeinadministratorrechten. Daarom **traditionele KCD op basis van het account kan niet worden geconfigureerd op een beheerd domein**. Gebruik KCD op basis van bronnen zoals beschreven in dit artikel. Dit mechanisme is ook veiliger.
>
>

## <a name="resource-based-kcd"></a>KCD op basis van bronnen
Windows Server 2012 en hoger krijgen servicebeheerders de mogelijkheid voor het configureren van beperkte delegering voor de service. In dit model kan de back-end-servicebeheerder toestaan of weigeren van specifieke front-end-services KCD te gebruiken. Dit model wordt ook wel **KCD resources gebaseerde**.

KCD op basis van een bron is geconfigureerd met behulp van PowerShell. U gebruikt de `Set-ADComputer` of `Set-ADUser` cmdlets, afhankelijk van of het account voor imitatie een computeraccount of de account-/ serviceaccount voor een gebruiker is.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>KCD op basis van bronnen voor een computeraccount configureren op een beheerd domein
Wordt ervan uitgegaan dat u hebt een web-app uitgevoerd op de computer ' contoso100-webapp.contoso100.com'. Deze toegang nodig heeft tot de resource (een web-API uit te voeren op ' contoso100-api.contoso100.com') in de context van Domeingebruikers. Hier ziet u hoe u resources gebaseerde KCD voor dit scenario zou instellen:

1. [Maak een aangepaste organisatie-eenheid](active-directory-ds-admin-guide-create-ou.md). U kunt machtigingen voor het beheren van deze aangepaste organisatie-eenheid voor gebruikers binnen het beheerde domein delegeren.
2. Beide virtuele machines (het uitvoeren van de web-app en de met de web-API) toevoegen aan het beheerde domein. Deze computeraccounts in de aangepaste organisatie-eenheid maken.
3. Nu configureren op basis van een resource KCD met de volgende PowerShell-opdracht:

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> De computeraccounts voor de web-app en de web-API moeten in een aangepaste organisatie-eenheid waar u gemachtigd bent om te configureren op basis van een resource KCD. U kunt KCD op basis van bronnen voor een computeraccount niet configureren in de ingebouwde ' AAD DC' computercontainer.
>

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>KCD op basis van bronnen voor een gebruikersaccount configureren op een beheerd domein
Wordt ervan uitgegaan dat u hebt een web-app wordt uitgevoerd als een service-account 'appsvc' en deze moet toegang tot de resource (een web-API wordt uitgevoerd als een serviceaccount - 'backendsvc') in de context van Domeingebruikers. Dit is hoe u resources gebaseerde KCD voor dit scenario zou ingesteld.

1. [Maak een aangepaste organisatie-eenheid](active-directory-ds-admin-guide-create-ou.md). U kunt machtigingen voor het beheren van deze aangepaste organisatie-eenheid voor gebruikers binnen het beheerde domein delegeren.
2. Voeg de virtuele machine met het back-end-web-API/resourcegroep naar het beheerde domein. De computeraccount binnen de aangepaste organisatie-eenheid maken.
3. Het serviceaccount (bijvoorbeeld ' appsvc') gebruikt voor het uitvoeren van de web-app in de aangepaste organisatie-eenheid maken.
4. Nu configureren op basis van een resource KCD met de volgende PowerShell-opdracht:

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> Het computeraccount voor de back-end-web-API en de service-account moeten in een aangepaste organisatie-eenheid waar u gemachtigd bent om te configureren op basis van een resource KCD. U kunt KCD op basis van bronnen voor een account in de ingebouwde 'AAD DC Computers'-container of voor gebruikersaccounts niet configureren in de ingebouwde 'AAD DC-gebruikers'-container. U kunt gebruikersaccounts gesynchroniseerd met Azure AD dus niet gebruiken om in te stellen op basis van een resource KCD.
>

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Kerberos-beperkte overdracht overzicht](https://technet.microsoft.com/library/jj553400.aspx)
