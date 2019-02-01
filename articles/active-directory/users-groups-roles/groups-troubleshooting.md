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
ms.openlocfilehash: 7d157595e5a3168f7807f015441fbcb770612e71
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55510405"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Oplossen van problemen met dynamische lidmaatschappen voor groepen

**Heb ik een regel voor een groep hebt geconfigureerd, maar er zijn geen lidmaatschappen worden bijgewerkt in de groep**<br/>Controleer of de waarden voor kenmerken van de gebruiker op de regel: Er zijn van gebruikers die voldoen aan de regel? Als alles er goed uitziet, wacht u enige tijd voor de groep om in te vullen. Afhankelijk van de grootte van uw tenant, kan het de eerste keer of na een regelwijziging tot 24 uur duren voordat de groep is ingevuld.

**Heb ik een regel hebt geconfigureerd, maar nu de bestaande leden van de regel zijn verwijderd**<br/>Dit is normaal. Bestaande leden van de groep worden verwijderd wanneer een regel is ingeschakeld of gewijzigd. De gebruikers geretourneerd van de evaluatieversie van de regel worden toegevoegd als leden aan de groep.

**Ik zie niet met het lidmaatschap wordt gewijzigd direct wanneer ik toevoegen of wijzigen van een regel, waarom niet?**<br/>Evaluatie toegewezen lidmaatschap wordt periodiek uitgevoerd in een asynchrone achtergrondproces. Hoe lang duurt het proces wordt bepaald door het aantal gebruikers in uw directory en de grootte van de groep gemaakt als gevolg van de regel. Mappen met kleine aantallen gebruikers ziet doorgaans, wijzigingen in de groep het lidmaatschap in minder dan een paar minuten. Mappen met een groot aantal gebruikers kunnen 30 minuten of langer om in te vullen.

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
