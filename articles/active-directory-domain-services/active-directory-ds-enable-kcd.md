---
title: 'Azure Active Directory Domain Services: Kerberos-beperkte delegatie inschakelen | Microsoft Docs'
description: Kerberos-beperkte overdracht op beheerde domeinen voor Azure Active Directory Domain Services inschakelen
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: f36f16a7bb00ace9fd5164eb38ba77f015f22f5c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Kerberos-beperkte delegatie (KCD) configureren op een beheerd domein
Veel toepassingen moeten toegang tot bronnen in de context van de gebruiker. Active Directory ondersteunt een mechanisme voor Kerberos-overdracht, waardoor deze gebruiksvoorbeeld. U kunt bovendien delegering beperken zodat alleen bepaalde resources kunnen worden geopend in de context van de gebruiker. Azure AD Domain Services beheerde domeinen zijn anders dan traditionele Active Directory-domeinen, omdat ze veiliger zijn vergrendeld.

In dit artikel leest u hoe kerberos-beperkte overdracht configureren op een beheerd domein van Azure AD Domain Services.

## <a name="kerberos-constrained-delegation-kcd"></a>Kerberos-beperkte delegatie (KCD)
Kerberos-overdracht kunt een account aan een andere beveiligings-principal (zoals een gebruiker) toegang tot bronnen te imiteren. U kunt een webtoepassing die toegang heeft tot een back-end-web-API in de context van een gebruiker. In dit voorbeeld wordt in de webtoepassing (uitgevoerd in de context van een serviceaccount of een computer/computeraccount) de gebruiker imiteert bij toegang tot de bron (back-end web-API). Kerberos-overdracht is niet veilig omdat deze wordt niet beperkt de imitatie account in de context van de gebruiker toegang heeft tot resources.

Kerberos-beperkte delegatie (KCD) Hiermee beperkt u de services/resources waartoe de opgegeven server namens een gebruiker kan fungeren. Traditionele KCD vereist bevoegdheden voor het configureren van een domeinaccount voor een service van een domeinadministrator en het het account op één domein beperkt.

Traditionele KCD heeft ook een aantal aspecten die zijn gekoppeld. In eerdere besturingssystemen waar de domeinadministrator de service hebt geconfigureerd, kon de servicebeheerder geen handige manier achterhalen welke front-end-services overgedragen aan de bronservices die of zij eigenaar was. En elke front-endservice die kon worden overgedragen naar een bronservice vertegenwoordigd een potentiële aanvallen met zich mee. Als een server die als host van een front-end-service is geknoeid en deze is geconfigureerd voor het overgedragen aan bronservices, kunnen ook de bronservices gevaar.

> [!NOTE]
> Op een beheerd domein van Azure AD Domain Services er geen domeinadministratorrechten. Daarom **traditionele KCD kan niet worden geconfigureerd op een beheerd domein**. Gebruik KCD op basis van bronnen zoals beschreven in dit artikel. Dit mechanisme is ook veiliger.
>
>

## <a name="resource-based-kerberos-constrained-delegation"></a>Op basis van bronnen kerberos-beperkte overdracht
In Windows Server 2012 R2 en Windows Server 2012, is de mogelijkheid voor het configureren van beperkte delegering voor de service van de domeinbeheerder doorgegeven aan de servicebeheerder. Op deze manier kan de back-end-servicebeheerder toestaan of weigeren front-end-services. Dit model wordt ook wel **resources gebaseerde kerberos-beperkte overdracht**.

KCD op basis van een bron is geconfigureerd met behulp van PowerShell. U de Set-ADComputer of Set-ADUser-cmdlets, afhankelijk van de imitatie account een computeraccount of de account-/ serviceaccount voor een gebruiker.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>KCD op basis van bronnen voor een computeraccount configureren op een beheerd domein
Wordt ervan uitgegaan dat u hebt een web-app uitgevoerd op de computer ' contoso100-webapp.contoso100.com'. Deze toegang nodig heeft tot de resource (een web-API uit te voeren op ' contoso100-api.contoso100.com') in de context van Domeingebruikers. Dit is hoe u resources gebaseerde KCD voor dit scenario zou ingesteld.

```
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>KCD op basis van bronnen voor een gebruikersaccount configureren op een beheerd domein
Wordt ervan uitgegaan dat u hebt een web-app wordt uitgevoerd als een service-account 'appsvc' en deze moet toegang tot de resource (een web-API wordt uitgevoerd als een serviceaccount - 'backendsvc') in de context van Domeingebruikers. Dit is hoe u resources gebaseerde KCD voor dit scenario zou ingesteld.

```
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Kerberos-beperkte overdracht overzicht](https://technet.microsoft.com/library/jj553400.aspx)
