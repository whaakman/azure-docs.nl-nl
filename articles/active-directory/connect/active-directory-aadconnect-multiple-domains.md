---
title: Azure AD Connect meerdere domeinen
description: Dit document beschrijft instellen en configureren van meerdere topleveldomeinen met O365 en Azure AD.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: db4cfe91b8d27b5336763eff7c6f22f0f345caf2
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Ondersteuning voor meerdere domeinen voor federatie met Azure AD
De volgende documentatie biedt richtlijnen voor het gebruik van meerdere op het hoogste niveau domeinen en subdomeinen wanneer Federatie met Office 365 of Azure AD-domeinen.

## <a name="multiple-top-level-domain-support"></a>Ondersteuning voor meerdere domeinen op het hoogste niveau
Federeren van meerdere topleveldomeinen met Azure AD die is niet vereist voor federatie met één topleveldomein aanvullende configuratie vereist.

Wanneer een domein is gefedereerd met Azure AD, worden verschillende eigenschappen ingesteld voor het domein in Azure.  Een is belangrijk IssuerUri.  Dit is een URI die door Azure AD wordt gebruikt voor het identificeren van het domein waaraan het token is gekoppeld.  De URI hoeft niet omzetten naar een andere waarde dan dat het moet een geldige URI.  Standaard Azure AD stelt dit aan de waarde van de federation service-id in uw on-premises AD FS configuration.

> [!NOTE]
> De federation service-id is een URI die een unieke identificatie van een federation-service.  De federation-service is een exemplaar van AD FS die functies als de service voor beveiligingstokens.
>
>

U kunt de IssuerUri weergeven met behulp van de PowerShell-opdracht `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Een probleem ontstaat wanneer we willen meer dan één site op het hoogste domein toevoegen.  Bijvoorbeeld, Stel dat u setup federation tussen Azure AD hebt en uw on-premises omgeving.  Ik gebruik bmcontoso.com voor dit document.  Nu ik heb een domein tweede, op het hoogste niveau toegevoegd bmfabrikam.com.

![Domeinen](./media/active-directory-multiple-domains/domains.png)

Wanneer we proberen te converteren van onze bmfabrikam.com domein dat gefedereerd, moet er een foutbericht ontvangt.  De reden hiervoor is dat Azure AD heeft een beperking waarvoor de eigenschap IssuerUri hebben dezelfde waarde voor meer dan één domein niet is toegestaan.  

![Federation-fout](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>De SupportMultipleDomain Parameter
Als tijdelijke oplossing, moeten we het toevoegen van een andere IssuerUri dat u doen met behulp van kunt de `-SupportMultipleDomain` parameter.  Deze parameter wordt gebruikt met de volgende cmdlets:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Deze parameter kunt u Azure AD de IssuerUri zodanig configureren dat deze is gebaseerd op de naam van het domein.  Dit is de unieke op mappen in Azure AD.  Met de parameter, kunt de PowerShell-opdracht te voltooien.

![Federation-fout](./media/active-directory-multiple-domains/convert.png)

De instellingen van onze nieuwe bmfabrikam.com domein ziet u het volgende:

![Federation-fout](./media/active-directory-multiple-domains/settings.png)

Houd er rekening mee dat `-SupportMultipleDomain` verandert niets aan de andere eindpunten die worden nog geconfigureerd om te verwijzen naar onze adfs.bmcontoso.com federation-service.

Een andere ding die `-SupportMultipleDomain` biedt is het zorgt ervoor dat het systeem AD FS de juiste waarde van de verlener in de tokens die zijn uitgegeven voor Azure AD opgenomen. Dit wordt door te nemen het domeingedeelte van de UPN van gebruikers en in te stellen als het domein in de IssuerUri, dat wil zeggen https://{upn achtervoegsel} / services-adfs-vertrouwen.

Dus tijdens de verificatie met Azure AD of Office 365, het IssuerUri-element in het token van de gebruiker wordt gebruikt voor het domein niet vinden in Azure AD.  Als er een overeenkomst wordt gevonden dat de verificatie mislukt.

Bijvoorbeeld, als een gebruiker de UPN is bsimon@bmcontoso.com, het IssuerUri-element in de problemen met het token AD FS wordt zo ingesteld dat http://bmcontoso.com/adfs/services/trust. Dit komt overeen met de Azure AD-configuratie en verificatie slaagt.

Hier volgt de aangepaste claimregel die deze logica implementeert:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Om de schakeloptie - SupportMultipleDomain gebruikt bij het toevoegen van domeinen bij het toevoegen of al converteren, moet u het installatieprogramma de federatieve vertrouwensrelatie worden oorspronkelijk ondersteund.  
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Het bijwerken van de vertrouwensrelatie tussen AD FS en Azure AD
Als u niet de federatieve vertrouwensrelatie tussen AD FS en uw exemplaar van Azure AD instellen hebt, moet u mogelijk om deze vertrouwensrelatie opnieuw te maken.  Dit is omdat, als het oorspronkelijk installatie zonder het `-SupportMultipleDomain` parameter, de IssuerUri is ingesteld met de standaardwaarde.  In de schermafbeelding hieronder u ziet dat de IssuerUri is ingesteld op https://adfs.bmcontoso.com/adfs/services/trust.

In dat geval nu, als er een nieuw domein in de Azure AD-portal hebt toegevoegd en vervolgens probeert te converteren met behulp van `Convert-MsolDomaintoFederated -DomainName <your domain>`, krijgen we de volgende fout.

![Federation-fout](./media/active-directory-multiple-domains/trust1.png)

Als u probeert toe te voegen de `-SupportMultipleDomain` switch we de volgende fout ontvangen:

![Federation-fout](./media/active-directory-multiple-domains/trust2.png)

Gewoon probeert uit te voeren `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` op het oorspronkelijke domein wordt ook leiden tot een fout opgetreden.

![Federation-fout](./media/active-directory-multiple-domains/trust3.png)

Gebruik de volgende stappen uit om toe te voegen een extra domein op het hoogste niveau.  Als u al een domein hebt toegevoegd en niet gebruikt de `-SupportMultipleDomain` parameter starten met de stappen voor het verwijderen en bijwerken van uw oorspronkelijke domein.  Als u een domein op het hoogste niveau niet hebt toegevoegd kunt nog u starten met de stappen voor het toevoegen van een domein met behulp van PowerShell van Azure AD Connect.

Gebruik de volgende stappen voor het verwijderen van de vertrouwensrelatie van de Microsoft Online en bijwerken van uw oorspronkelijke domein.

1. Op de AD FS-federatieserver open **AD FS-beheer.**
2. Vouw op de links **vertrouwensrelaties** en **Relying Party-vertrouwensrelaties**
3. Aan de rechterkant verwijderen de **Identiteitsplatform van Microsoft Office 365** vermelding.
   ![Verwijder Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
4. Op een computer die is [Azure Active Directory-Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) geïnstalleerd op het volgende uitvoeren: `$cred=Get-Credential`.  
5. Voer de gebruikersnaam en het wachtwoord van een globale beheerder voor de door u Federatie met Azure AD-domein
6. Voer in PowerShell`Connect-MsolService -Credential $cred`
7. Voer in PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Dit is voor het oorspronkelijke domein.  Met behulp van de bovenstaande domeinen die het normaal zou zijn:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

Gebruik de volgende stappen om toe te voegen van de nieuwe topleveldomein met behulp van PowerShell

1. Op een computer die is [Azure Active Directory-Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) geïnstalleerd op het volgende uitvoeren: `$cred=Get-Credential`.  
2. Voer de gebruikersnaam en het wachtwoord van een globale beheerder voor de door u Federatie met Azure AD-domein
3. Voer in PowerShell`Connect-MsolService -Credential $cred`
4. Voer in PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Gebruik de volgende stappen uit om toe te voegen van de nieuwe topleveldomein met Azure AD Connect.

1. Azure AD Connect vanaf het bureaublad te starten of het menu start
2. Kies 'Een extra Azure AD-domein toevoegen' ![toevoegen van een extra Azure AD-domein](./media/active-directory-multiple-domains/add1.png)
3. Voer uw Azure AD en Active Directory-referenties
4. Selecteer het tweede domein dat u wilt configureren voor Federatie.
   ![Toevoegen van een extra Azure AD-domein](./media/active-directory-multiple-domains/add2.png)
5. Klik op installeren

### <a name="verify-the-new-top-level-domain"></a>Controleer of het nieuwe domein op het hoogste niveau
Met behulp van de PowerShell-opdracht `Get-MsolDomainFederationSettings -DomainName <your domain>`kunt u de bijgewerkte IssuerUri weergeven.  De onderstaande schermafbeelding ziet u de federatie instellingen zijn bijgewerkt op de oorspronkelijke domein http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

En de IssuerUri in onze nieuwe domein is ingesteld op https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a>Ondersteuning voor subdomeinen
Wanneer u een subdomein vanwege de manier waarop Azure AD verwerkt domeinen toevoegt, worden overgenomen door de instellingen van het bovenliggende item.  Dit betekent dat de IssuerUri moet overeenkomen met de bovenliggende items.

Zo kunnen zeggen bijvoorbeeld ik bmcontoso.com hebt en voeg vervolgens corp.bmcontoso.com.  Dit betekent dat de IssuerUri voor een gebruiker vanuit corp.bmcontoso.com moet **http://bmcontoso.com/adfs/services/trust.**  Maar de standaard regel boven geïmplementeerd voor Azure AD, wordt een token genereren met een verlener als **http://corp.bmcontoso.com/adfs/services/trust.** waarde die niet overeenkomt met het domein nodig en mislukt de verificatie.

### <a name="how-to-enable-support-for-sub-domains"></a>Het inschakelen van ondersteuning voor subdomeinen
Relying party trust voor Microsoft Online moet worden bijgewerkt om dit probleem oplossen door de AD FS.  Om dit te doen, moet u een aangepaste claimregel configureren zodat deze uit alle subdomeinen van UPN-achtervoegsel van de gebruiker ontdoet bij het maken van de aangepaste waarde van de verlener.

De volgende claim wordt dit doen:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
De laatste waarde in de reguliere expressie instellen hoeveel bovenliggende domeinen zich in het hoofddomein. Ik heb hier bmcontoso.com zodat twee bovenliggende domeinen nodig zijn. Als u drie bovenliggende domeinen worden bewaard zijn (dat wil zeggen: corp.bmcontoso.com), en vervolgens het aantal drie zijn zou. Uiteindelijk kan een bereik worden aangegeven overeenkomst met de altijd worden aangebracht aan overeen met het maximum van domeinen. '{2,3}' komt overeen met twee of drie domeinen (dat wil zeggen: bmfabrikam.com en corp.bmcontoso.com).

Gebruik de volgende stappen uit een aangepaste claim ter ondersteuning van subdomeinen toevoegen.

1. Open AD FS-beheer
2. Klik met de rechtermuisknop op de vertrouwensrelatie van de Microsoft Online RP en kies claimregels bewerken
3. Selecteer de derde claimregel en vervang ![Edit claim](./media/active-directory-multiple-domains/sub1.png)
4. Vervang de huidige claim:

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));

       with

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Claim vervangen](./media/active-directory-multiple-domains/sub2.png)

5. Klik op Ok.  Klik op toepassen.  Klik op Ok.  Sluit AD FS-beheer.
