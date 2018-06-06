---
title: Azure AD Connect meerdere domeinen
description: Dit document beschrijft instellen en configureren van meerdere topleveldomeinen met O365 en Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2f596f64041b3d429b99db482cd635ab441bf468
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801504"
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Ondersteuning voor meerdere domeinen voor federatie met Azure AD
De volgende documentatie biedt richtlijnen voor het gebruik van meerdere op het hoogste niveau domeinen en subdomeinen wanneer Federatie met Office 365 of Azure AD-domeinen.

## <a name="multiple-top-level-domain-support"></a>Ondersteuning voor meerdere domeinen op het hoogste niveau
Federeren van meerdere topleveldomeinen met Azure AD die is niet vereist voor federatie met één topleveldomein aanvullende configuratie vereist.

Wanneer een domein is gefedereerd met Azure AD, worden verschillende eigenschappen ingesteld voor het domein in Azure.  Een is belangrijk IssuerUri.  Deze eigenschap is een URI die door Azure AD wordt gebruikt voor het identificeren van het domein waaraan het token is gekoppeld.  De URI hoeft niet omzetten naar een andere waarde dan dat het moet een geldige URI.  Standaard Azure AD stelt de URI met de waarde van de federation service-id in uw on-premises AD FS configuration.

> [!NOTE]
> De federation service-id is een URI die een unieke identificatie van een federation-service.  De federation-service is een exemplaar van AD FS die functies als de service voor beveiligingstokens.
>
>

U kunt de IssuerUri weergeven met behulp van de PowerShell-opdracht `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Een probleem ontstaat wanneer u meer dan één site op het hoogste domein toevoegt.  Bijvoorbeeld, Stel dat u een federatieve tussen Azure AD hebt ingesteld en uw on-premises omgeving.  Voor dit document, het domein, wordt bmcontoso.com gebruikt.  Nu een tweede, op het hoogste niveau domein, bmfabrikam.com is toegevoegd.

![Domeinen](./media/active-directory-multiple-domains/domains.png)

Wanneer u probeert te converteren van het domein bmfabrikam.com gefedereerd, is een fout optreedt.  De reden is dat Azure AD heeft een beperking waarvoor de eigenschap IssuerUri hebben dezelfde waarde voor meer dan één domein niet is toegestaan.  

![Federation-fout](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>De SupportMultipleDomain Parameter
U kunt deze beperking omzeilen, moet u een ander IssuerUri dat u doen met behulp van kunt toevoegen de `-SupportMultipleDomain` parameter.  Deze parameter wordt gebruikt met de volgende cmdlets:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Deze parameter kunt u Azure AD de IssuerUri zodanig configureren dat deze is gebaseerd op de naam van het domein.  De IssuerUri wordt uniek zijn in mappen in Azure AD.  Met de parameter, kunt de PowerShell-opdracht te voltooien.

![Federation-fout](./media/active-directory-multiple-domains/convert.png)

De instellingen voor het domein bmfabrikam.com ziet u het volgende:

![Federation-fout](./media/active-directory-multiple-domains/settings.png)

`-SupportMultipleDomain` de andere eindpunten die nog steeds worden geconfigureerd om te verwijzen naar de federation-service adfs.bmcontoso.com niet gewijzigd.

Een andere ding die `-SupportMultipleDomain` biedt is het zorgt ervoor dat het systeem AD FS de juiste waarde van de verlener in de tokens die zijn uitgegeven voor Azure AD opgenomen. Deze waarde is ingesteld door te nemen het domeingedeelte van de UPN van gebruikers en ingesteld als het domein in de IssuerUri, dat wil zeggen https://{upn achtervoegsel} / services-adfs-vertrouwen.

Dus tijdens de verificatie met Azure AD of Office 365, het IssuerUri-element in het token van de gebruiker wordt gebruikt voor het domein niet vinden in Azure AD.  Als een overeenkomst kan niet worden gevonden, mislukt de verificatie.

Bijvoorbeeld, als een gebruiker de UPN is bsimon@bmcontoso.com, het IssuerUri-element in het token, problemen met AD FS wordt zo ingesteld dat http://bmcontoso.com/adfs/services/trust. Dit element komt overeen met de Azure AD-configuratie en verificatie slaagt.

Hier volgt de aangepaste claimregel die deze logica implementeert:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Als u wilt gebruiken bij een poging de switch - SupportMultipleDomain toevoegen aan nieuwe of bestaande domeinen converteren, moet uw federatieve vertrouwensrelatie al is ingesteld om ze te ondersteunen.
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Het bijwerken van de vertrouwensrelatie tussen AD FS en Azure AD
Als u niet de federatieve vertrouwensrelatie tussen AD FS en uw exemplaar van Azure AD hebt ingesteld, moet u wellicht opnieuw maken van deze vertrouwensrelatie.  De reden is wanneer het oorspronkelijk is stellen zonder de `-SupportMultipleDomain` parameter, de IssuerUri is ingesteld met de standaardwaarde.  In de onderstaande schermafbeelding ziet u de IssuerUri is ingesteld op https://adfs.bmcontoso.com/adfs/services/trust.

Als u een nieuw domein in de Azure AD-portal hebt toegevoegd en vervolgens probeert te converteren met behulp van `Convert-MsolDomaintoFederated -DomainName <your domain>`, krijgt u de volgende fout.

![Federation-fout](./media/active-directory-multiple-domains/trust1.png)

Als u probeert toe te voegen de `-SupportMultipleDomain` overschakelen, ontvangt u de volgende fout:

![Federation-fout](./media/active-directory-multiple-domains/trust2.png)

Gewoon probeert uit te voeren `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` op het oorspronkelijke domein wordt ook leiden tot een fout opgetreden.

![Federation-fout](./media/active-directory-multiple-domains/trust3.png)

Gebruik de volgende stappen uit om toe te voegen een extra domein op het hoogste niveau.  Als u al een domein hebt toegevoegd en niet gebruikt de `-SupportMultipleDomain` parameter, beginnen met de stappen voor het verwijderen en bijwerken van uw oorspronkelijke domein.  Als u nog niet een topleveldomein hebt toegevoegd, kunt u starten met de stappen voor het toevoegen van een domein met behulp van PowerShell van Azure AD Connect.

Gebruik de volgende stappen voor het verwijderen van de vertrouwensrelatie van de Microsoft Online en bijwerken van uw oorspronkelijke domein.

1. Op de AD FS-federatieserver open **AD FS-beheer.**
2. Vouw op de links **vertrouwensrelaties** en **Relying Party-vertrouwensrelaties**
3. Aan de rechterkant verwijderen de **Identiteitsplatform van Microsoft Office 365** vermelding.
   ![Verwijder Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
4. Op een computer die is [Azure Active Directory-Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) geïnstalleerd op het volgende uitvoeren: `$cred=Get-Credential`.  
5. Geef de gebruikersnaam en het wachtwoord van een globale beheerder voor de door u Federatie met Azure AD-domein.
6. Voer in PowerShell `Connect-MsolService -Credential $cred`
7. Voer in PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Deze update is voor het oorspronkelijke domein.  Met behulp van de bovenstaande domeinen die het normaal zou zijn:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

Gebruik de volgende stappen om toe te voegen van de nieuwe topleveldomein met behulp van PowerShell

1. Op een computer die is [Azure Active Directory-Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) geïnstalleerd op het volgende uitvoeren: `$cred=Get-Credential`.  
2. Voer de gebruikersnaam en het wachtwoord van een globale beheerder voor de door u Federatie met Azure AD-domein
3. Voer in PowerShell `Connect-MsolService -Credential $cred`
4. Voer in PowerShell `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Gebruik de volgende stappen uit om toe te voegen van de nieuwe topleveldomein met Azure AD Connect.

1. Azure AD Connect vanaf het bureaublad te starten of het menu start
2. Kies 'Een extra Azure AD-domein toevoegen' ![toevoegen van een extra Azure AD-domein](./media/active-directory-multiple-domains/add1.png)
3. Voer uw Azure AD en Active Directory-referenties
4. Selecteer het tweede domein dat u wilt configureren voor Federatie.
   ![Toevoegen van een extra Azure AD-domein](./media/active-directory-multiple-domains/add2.png)
5. Klik op Installeren

### <a name="verify-the-new-top-level-domain"></a>Controleer of het nieuwe domein op het hoogste niveau
Met behulp van de PowerShell-opdracht `Get-MsolDomainFederationSettings -DomainName <your domain>`kunt u de bijgewerkte IssuerUri weergeven.  De onderstaande schermafbeelding ziet u de federatie instellingen zijn bijgewerkt op de oorspronkelijke domein http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

En de IssuerUri in het nieuwe domein is ingesteld op https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-subdomains"></a>Ondersteuning voor subdomeinen
Wanneer u een subdomein vanwege de manier waarop Azure AD verwerkt domeinen toevoegt, worden overgenomen door de instellingen van het bovenliggende item.  Ja, de IssuerUri moet overeenkomen met de bovenliggende items.

In dat geval kunt dat bijvoorbeeld ik bmcontoso.com hebt en voeg vervolgens corp.bmcontoso.com.  De IssuerUri voor een gebruiker vanuit corp.bmcontoso.com moet  **http://bmcontoso.com/adfs/services/trust.**  Maar de standaard regel boven geïmplementeerd voor Azure AD, wordt een token genereren met een verlener als  **http://corp.bmcontoso.com/adfs/services/trust.** waarde die niet overeenkomt met het domein nodig en mislukt de verificatie.

### <a name="how-to-enable-support-for-subdomains"></a>Het inschakelen van ondersteuning voor subdomeinen
De AD FS vertrouwensrelatie van relying party voor Microsoft Online moet worden bijgewerkt om dit probleem omzeilen.  Om dit te doen, moet u een aangepaste claimregel configureren zodat deze uitschakelen subdomeinen van UPN-achtervoegsel van de gebruiker ontdoet bij het maken van de aangepaste waarde van de verlener.

De volgende claim wordt dit doen:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
De laatste waarde in de set reguliere expressie is er hoeveel bovenliggende-domeinen zich in het hoofddomein. Hier wordt bmcontoso.com gebruikt, dus er twee bovenliggende domeinen nodig zijn. Als u drie bovenliggende domeinen worden bewaard zijn (dat wil zeggen: corp.bmcontoso.com), en vervolgens het aantal drie zijn zou. Uiteindelijk kan een bereik worden aangegeven overeenkomst met de altijd worden aangebracht aan overeen met het maximum van domeinen. '{2,3}' komt overeen met twee of drie domeinen (dat wil zeggen: bmfabrikam.com en corp.bmcontoso.com).

Gebruik de volgende stappen uit een aangepaste claim ter ondersteuning van subdomeinen toevoegen.

1. Open AD FS-beheer
2. Met de rechtermuisknop op de vertrouwensrelatie van de Microsoft Online RP en kies claimregels bewerken
3. Selecteer de derde claimregel en vervang ![Edit claim](./media/active-directory-multiple-domains/sub1.png)
4. Vervang de huidige claim:

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));

       with

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Claim vervangen](./media/active-directory-multiple-domains/sub2.png)

5. Klik op Ok.  Klik op toepassen.  Klik op Ok.  Sluit AD FS-beheer.

## <a name="next-steps"></a>Volgende stappen
Nu u Azure AD Connect geïnstalleerd hebt, kunt u [de installatie verifiëren en licenties toewijzen](active-directory-aadconnect-whats-next.md).

Lees meer over deze functies, die tijdens de installatie zijn ingeschakeld: [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md), [Onopzettelijk verwijderen voorkomen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) en [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Lees meer over deze veelvoorkomende onderwerpen: [Scheduler en het activeren van de synchronisatie](active-directory-aadconnectsync-feature-scheduler.md).

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).