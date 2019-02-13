---
title: Azure AD Connect meerdere domeinen
description: Dit document beschrijft het instellen en configureren van meerdere domeinen op hoogste niveau met Office 365 en Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/31/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00cde6f9d17eb44fefae10d8694a89abf51540a5
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182051"
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Ondersteuning voor meerdere domeinen voor federatie met Azure AD
De volgende documentatie bevat richtlijnen over het gebruik van meerdere op het hoogste niveau domeinen en subdomeinen wanneer Federatie met Office 365 of Azure AD-domeinen.

## <a name="multiple-top-level-domain-support"></a>Ondersteuning voor meerdere domeinen op het hoogste niveau
Op het hoogste niveau domeinen met Azure AD federeren meerdere, is echter enkele aanvullende configuratiestappen die niet is vereist bij federeren met één op het hoogste niveau domein vereist.

Wanneer een domein is gefedereerd met Azure AD, worden verschillende eigenschappen worden ingesteld op het domein in Azure.  Een belangrijke gebeurtenis wordt IssuerUri.  Deze eigenschap is een URI die wordt gebruikt door Azure AD voor het identificeren van het domein dat het token is gekoppeld.  De URI hoeft niet te worden omgezet in alles, maar dat dit moet een geldige URI.  Standaard Azure AD stelt de URI op de waarde van de federatieservice-id in uw on-premises AD FS configureren.

> [!NOTE]
> De federatieservice-id is een URI die een federation-service wordt aangeduid.  De federation-service is een exemplaar van AD FS die werkt als de service voor beveiligingstokens.
>
>

U kunt de IssuerUri weergeven met behulp van de PowerShell-opdracht `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/MsolDomainFederationSettings.png)

Een probleem voordoet wanneer u meer dan één op het hoogste niveau domein toevoegt.  Bijvoorbeeld: Stel dat u een federatie tussen Azure AD hebt ingesteld en uw on-premises omgeving.  Voor dit document, het domein, wordt bmcontoso.com gebruikt.  Nu een tweede, op het hoogste niveau domein bmfabrikam.com is toegevoegd.

![Domeinen](./media/how-to-connect-install-multiple-domains/domains.png)

Wanneer u probeert te converteren van de bmfabrikam.com te federeren domein, wordt er een fout optreedt.  De reden is dat Azure AD heeft een beperking die niet toe de eigenschap IssuerUri dat staat hebben dezelfde waarde voor meer dan één domein.  

![Federation-fout](./media/how-to-connect-install-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain Parameter
U kunt deze beperking omzeilen, die u wilt toevoegen van een andere IssuerUri, die kan worden uitgevoerd met behulp van de `-SupportMultipleDomain` parameter.  Deze parameter wordt gebruikt met de volgende cmdlets:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Deze parameter kunt u Azure AD de IssuerUri zo configureren dat deze is gebaseerd op de naam van het domein.  De IssuerUri wordt uniek zijn in mappen in Azure AD.  De parameter wordt gebruikt, kunt de PowerShell-opdracht is voltooid.

![Federation-fout](./media/how-to-connect-install-multiple-domains/convert.png)

De instellingen voor het domein bmfabrikam.com ziet u het volgende:

![Federation-fout](./media/how-to-connect-install-multiple-domains/settings.png)

`-SupportMultipleDomain` de andere eindpunten die nog steeds worden geconfigureerd om te verwijzen naar de federation-service op adfs.bmcontoso.com wordt niet worden gewijzigd.

Een andere dingen die `-SupportMultipleDomain` heeft is dat het zorgt ervoor dat de AD FS-systeem bevat van de juiste uitgeverwaarde in tokens die zijn uitgegeven voor Azure AD. Deze waarde is ingesteld door te nemen het domeingedeelte van de gebruikers-UPN en ingesteld als het domein in de IssuerUri, dat wil zeggen https://{upn achtervoegsel} / adfs/services/trust.

Dus tijdens de verificatie met Azure AD of Office 365, het element IssuerUri in token van de gebruiker is gebruikt voor het vinden van het domein in Azure AD.  Als een overeenkomst kan niet worden gevonden, mislukt de verificatie.

Bijvoorbeeld, als een gebruiker van het UPN is bsimon@bmcontoso.com, de IssuerUri-element in het token, problemen met AD FS wordt ingesteld op http://bmcontoso.com/adfs/services/trust. Dit element komt overeen met de Azure AD-configuratie en verificatie slaagt.

Hier volgt de aangepaste claimregel die deze logische implementeert:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Als u wilt gebruiken wanneer u probeert de switch - SupportMultipleDomain nieuwe toevoegen of bestaande domeinen converteren, moet uw federatieve vertrouwensrelatie al zijn ingesteld om ze te ondersteunen.
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Het bijwerken van de vertrouwensrelatie tussen AD FS en Azure AD
Als u niet de federatieve vertrouwensrelatie tussen AD FS en uw exemplaar van Azure AD hebt ingesteld, moet u mogelijk opnieuw maken van deze vertrouwensrelatie.  De reden is dat als het oorspronkelijk is stellen zonder het `-SupportMultipleDomain` parameter, de IssuerUri is ingesteld met de standaardwaarde.  In de onderstaande schermafbeelding ziet u de IssuerUri is ingesteld op https://adfs.bmcontoso.com/adfs/services/trust.

Als u een nieuw domein in de Azure AD-portal hebt toegevoegd en vervolgens probeert te converteren met behulp van `Convert-MsolDomaintoFederated -DomainName <your domain>`, krijgt u de volgende fout.

![Federation-fout](./media/how-to-connect-install-multiple-domains/trust1.png)

Als u probeert toe te voegen de `-SupportMultipleDomain` overschakelen, ontvangt u de volgende fout:

![Federation-fout](./media/how-to-connect-install-multiple-domains/trust2.png)

Gewoon probeert uit te voeren `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` op het oorspronkelijke domein ook resulteert in een fout.

![Federation-fout](./media/how-to-connect-install-multiple-domains/trust3.png)

Gebruik de volgende stappen uit om toe te voegen een extra domeincontroller op het hoogste niveau.  Als u al een domein hebt toegevoegd, en heeft niet de `-SupportMultipleDomain` parameter, beginnen met de stappen voor het verwijderen en bijwerken van het oorspronkelijke domein.  Als u nog niet een op het hoogste niveau domein hebt toegevoegd, kunt u beginnen met de stappen voor het toevoegen van een domein met behulp van PowerShell of Azure AD Connect.

Gebruik de volgende stappen voor het verwijderen van de Microsoft Online-vertrouwensrelatie en het oorspronkelijke domein bijwerken.

1. Op de AD FS-federatieserver open **AD FS-beheer.**
2. Vouw aan de linkerkant **vertrouwensrelaties** en **Relying Party-vertrouwensrelaties**
3. Aan de rechterkant, verwijder de **Identiteitsplatform van Microsoft Office 365** vermelding.
   ![Remove Microsoft Online](./media/how-to-connect-install-multiple-domains/trust4.png)
4. Op een computer die is [Azure Active Directory-Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) geïnstalleerd voert u het volgende: `$cred=Get-Credential`.  
5. Voer de gebruikersnaam en het wachtwoord van een globale beheerder voor de u Federatie met Azure AD-domein.
6. Voer in PowerShell `Connect-MsolService -Credential $cred`
7. Voer in PowerShell, `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Deze update is voor het oorspronkelijke domein.  Met behulp van de bovenstaande domeinen is die het normaal zou zijn:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

Gebruik de volgende stappen om toe te voegen van de nieuwe op het hoogste niveau domein met behulp van PowerShell

1. Op een computer die is [Azure Active Directory-Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) geïnstalleerd voert u het volgende: `$cred=Get-Credential`.  
2. Voer de gebruikersnaam en het wachtwoord van een globale beheerder voor de u Federatie met Azure AD-domein
3. Voer in PowerShell `Connect-MsolService -Credential $cred`
4. Voer in PowerShell `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Gebruik de volgende stappen uit om toe te voegen van de nieuwe op het hoogste niveau domein met Azure AD Connect.

1. Azure AD Connect vanaf het bureaublad te starten of het menu start
2. Kies 'Een extra Azure AD-domein toevoegen' ![toevoegen van een extra Azure AD-domein](./media/how-to-connect-install-multiple-domains/add1.png)
3. Voer uw Azure AD en Active Directory-referenties
4. Selecteer het tweede domein dat u wilt configureren voor Federatie.
   ![Toevoegen van een extra Azure AD-domein](./media/how-to-connect-install-multiple-domains/add2.png)
5. Klik op Installeren

### <a name="verify-the-new-top-level-domain"></a>Controleer of het nieuwe domein op het hoogste niveau
Met behulp van de PowerShell-opdracht `Get-MsolDomainFederationSettings -DomainName <your domain>`kunt u de bijgewerkte IssuerUri bekijken.  De onderstaande schermafbeelding ziet u de federatie instellingen zijn bijgewerkt op het oorspronkelijke domein http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/MsolDomainFederationSettings.png)

En de IssuerUri op het nieuwe domein is ingesteld op https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/settings2.png)

## <a name="support-for-subdomains"></a>Ondersteuning voor subdomeinen
Wanneer u een subdomein vanwege de manier waarop Azure AD verwerkt domeinen toevoegt, worden de instellingen van het bovenliggende item overgenomen.  Dus moet de IssuerUri overeenkomen met de objecten.

Zo kunt dat bijvoorbeeld dat ik bmcontoso.com hebt en voeg deze corp.bmcontoso.com.  De IssuerUri voor een gebruiker vanuit corp.bmcontoso.com moet  **http://bmcontoso.com/adfs/services/trust.**  Maar de standard-regel hierboven is geïmplementeerd voor Azure AD, wordt een token genereren met een verlener als  **http://corp.bmcontoso.com/adfs/services/trust.** waarde die niet overeenkomen met het domein nodig en mislukt de verificatie.

### <a name="how-to-enable-support-for-subdomains"></a>Het inschakelen van ondersteuning voor subdomeinen
De AD FS vertrouwensrelatie van relying party voor Microsoft Online moet worden bijgewerkt om dit probleem omzeilen.  Om dit te doen, moet u een aangepaste claimregel configureren zodat deze uit subdomeinen van UPN-achtervoegsel van de gebruiker ontdoet bij het maken van aangepaste waarde voor de verlener.

De volgende claim zal dit doen:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
Het laatste getal in de set met reguliere expressie is het aantal bovenliggende domeinen er zijn in uw-hoofddomein. Hier wordt bmcontoso.com gebruikt, zodat twee bovenliggende domeinen nodig zijn. Als drie bovenliggende domeinen moeten worden bewaard zijn (dat wil zeggen: corp.bmcontoso.com), en vervolgens het aantal drie zijn zou. Uiteindelijk kunnen een bereik worden aangegeven, de overeenkomst wordt altijd worden gemaakt zodat deze overeenkomt met het maximum van domeinen. "{2,3}" komt overeen met twee of drie domeinen (dat wil zeggen: bmfabrikam.com en corp.bmcontoso.com).

Gebruik de volgende stappen uit om toe te voegen een aangepaste claim voor de ondersteuning van subdomeinen.

1. Open AD FS-Management
2. Met de rechtermuisknop op de Microsoft Online RP-vertrouwensrelaties en kiest u claimregels bewerken
3. Selecteer de derde claimregel en vervang ![claim bewerken](./media/how-to-connect-install-multiple-domains/sub1.png)
4. Vervang de huidige claim:

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));

       with

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Claim vervangen](./media/how-to-connect-install-multiple-domains/sub2.png)

5. Klik op Ok.  Klik op toepassen.  Klik op Ok.  Sluit AD FS-beheer.

## <a name="next-steps"></a>Volgende stappen
Nu u Azure AD Connect geïnstalleerd hebt kunt u [de installatie verifiëren en licenties toewijzen](how-to-connect-post-installation.md).

Meer informatie over deze functies, die de installatie zijn ingeschakeld: [Automatische upgrade](how-to-connect-install-automatic-upgrade.md), [Onopzettelijke verwijderingen voorkomen](how-to-connect-sync-feature-prevent-accidental-deletes.md) en [Azure AD Connect Health](how-to-connect-health-sync.md).

Meer informatie over deze algemene onderwerpen: [scheduler and how to trigger sync](how-to-connect-sync-feature-scheduler.md).

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).
