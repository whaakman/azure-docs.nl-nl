---
title: Oplossen van problemen voor dynamisch lidmaatschap voor groepen - Azure Active Directory | Microsoft Docs
description: Tips voor probleemoplossing voor dynamisch lidmaatschap voor groepen in Azure AD.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0594d99874ea9bb83673013a9a03272edcd8ce0b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57897670"
---
# <a name="troubleshoot-and-resolve-groups-issues"></a>Problemen op te lossen groepen

## <a name="troubleshooting-group-creation-issues"></a>Het oplossen van problemen bij het maken van de groep
**Ik security group maken in Azure portal hebt uitgeschakeld, maar kunnen nog steeds groepen worden gemaakt via Powershell** de **gebruikers kan beveiligingsgroepen maken in Azure-portals** instellen in de Azure portal-controles of niet-beheerders gebruikers kunnen beveiligingsgroepen maken in het toegangsvenster of de Azure-portal. Dit heeft geen controle over beveiliging groep maken via Powershell.

Groep maken voor gebruikers van niet-beheerders in Powershell uitschakelen:
1. Controleer of dat niet-beheerders zijn toegestaan om groepen te maken:
   
   ```
   PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
   ```
  
2. Als het resultaat `UsersPermissionToCreateGroupsEnabled : True`, niet-beheerders groepen kunnen maken. Deze functie uitschakelen:
  
   ``` 
   Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
   ```

<br/>**Ik heb ontvangen een maximale groepen toegestaan fout bij het maken van een dynamische groep in Powershell**<br/>
Als u een in Powershell waarmee wordt aangegeven bericht _dynamische Groepsbeleid maximum toegestane groepen bereikt_, betekent dit dat u hebt de limiet bereikt voor dynamische groepen in uw tenant. Het maximale aantal dynamische groepen per tenant is 5.000.

Voor het maken van een nieuwe dynamische groep, moet u eerst enkele bestaande dynamische groepen te verwijderen. Er is geen manier om de limiet te verhogen.

## <a name="troubleshooting-dynamic-memberships-for-groups"></a>Oplossen van problemen met dynamische lidmaatschappen voor groepen

**Heb ik een regel voor een groep hebt geconfigureerd, maar er zijn geen lidmaatschappen worden bijgewerkt in de groep**<br/>
1. Controleer of de waarden voor de gebruiker of apparaatkenmerken in de regel. Controleer of er zijn gebruikers die voldoen aan de regel. Voor apparaten, controleert u de apparaateigenschappen om te controleren of alle gesynchroniseerde kenmerken bevatten de verwachte waarden.<br/>
2. Controleer het lidmaatschap van de verwerkingsstatus om te controleren als deze voltooid is. U kunt controleren de [lidmaatschap verwerkingsstatus](groups-create-rule.md#check-processing-status-for-a-rule) en de laatste datum bijgewerkt op de **overzicht** pagina voor de groep.

Als alles er goed uitziet, wacht u enige tijd voor de groep om in te vullen. Afhankelijk van de grootte van uw tenant, kan het de eerste keer of na een regelwijziging tot 24 uur duren voordat de groep is ingevuld.

**Heb ik een regel hebt geconfigureerd, maar nu de bestaande leden van de regel zijn verwijderd**<br/>Dit is normaal. Bestaande leden van de groep worden verwijderd wanneer een regel is ingeschakeld of gewijzigd. De gebruikers geretourneerd van de evaluatieversie van de regel worden toegevoegd als leden aan de groep.

**Ik zie niet met het lidmaatschap wordt gewijzigd direct wanneer ik toevoegen of wijzigen van een regel, waarom niet?**<br/>Evaluatie toegewezen lidmaatschap wordt periodiek uitgevoerd in een asynchrone achtergrondproces. Hoe lang duurt het proces wordt bepaald door het aantal gebruikers in uw directory en de grootte van de groep gemaakt als gevolg van de regel. Mappen met kleine aantallen gebruikers ziet doorgaans, wijzigingen in de groep het lidmaatschap in minder dan een paar minuten. Mappen met een groot aantal gebruikers kunnen 30 minuten of langer om in te vullen.

**Hoe kan ik afdwingen dat de groep moet nu worden verwerkt?**<br/>
Er is op dit moment geen manier voor het automatisch activeren van de groep om te worden verwerkt op verzoek. U kunt echter handmatig activeren de herverwerking door bij te werken van de lidmaatschapsregel om toe te voegen een spatie aan het einde.  

**Er treedt een fout bij verwerken regel**<br/>De volgende tabel bevat algemene dynamisch lidmaatschap regel fouten en aanwijzingen om ze te corrigeren.

| Fout in regel parser | Fout-gebruik | Gecorrigeerde syntaxis |
| --- | --- | --- |
| Fout: Het kenmerk niet ondersteund. |(user.invalidProperty -eq "Value") |(user.department -eq "value")<br/><br/>Zorg ervoor dat het kenmerk is ingesteld op de [eigenschappenlijst ondersteund](groups-dynamic-membership.md#supported-properties). |
| Fout: Operator wordt niet ondersteund op kenmerk. |(user.accountEnabled-bevat waar) |(user.accountEnabled -eq true)<br/><br/>De operator die wordt gebruikt, wordt niet ondersteund voor de eigenschapstype (in dit voorbeeld-bevat kan niet worden gebruikt voor het type boolean). Gebruik de juiste operators voor het eigenschapstype. |
| Fout: Compilatiefout voor query. | 1. (user.department -eq "Sales") (user.department -eq "Marketing")<br>2.  (user.userPrincipalName -match "*@domain.ext") | 1. Ontbrekende operator. Gebruik de - en - of twee join-predicaten<br>(user.department -eq "Sales") -or (user.department -eq "Marketing")<br>2. Fout in reguliere expressie gebruikt in combinatie met - komt overeen met<br>(user.userPrincipalName -match ".*@domain.ext")<br>of u kunt ook: (user.userPrincipalName-overeenkomen met "@domain.ext$") |

## <a name="next-steps"></a>Volgende stappen

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Application Management in Azure Active Directory](../manage-apps/what-is-application-management.md) (Toepassingsbeheer in Azure Active Directory)
* [Wat is Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
