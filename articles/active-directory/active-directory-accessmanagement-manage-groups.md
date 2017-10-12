---
title: Groepen beheren in Azure Active Directory | Microsoft Docs
description: Groepen maken en beheren om Azure-groepen te beheren met behulp van Azure Active Directory.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d1f5451c-3807-423c-8bac-2822d27b893f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/10/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: a29bc68e966a3706af557af0c626d369d04149aa
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="managing-groups-in-azure-active-directory"></a>Groepen beheren in Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Een van de functies van het gebruikersbeheer van Azure Active Directory (Azure AD) is de mogelijkheid voor het maken van groepen gebruikers. U gebruikt een groep om beheertaken uit te voeren zoals het toewijzen van licenties of machtigingen aan een aantal gebruikers tegelijk. Ook kunt u groepen gebruiken om toegangsmachtigingen toe te wijzen aan:

* Resources zoals objecten in de directory;
* Resources die zich buiten de directory bevinden, zoals SaaS-toepassingen, Azure-services, SharePoint-sites of on-premises resources.

Bovendien kan een resource-eigenaar de toegang tot een resource ook toewijzen aan een Azure AD-groep die eigendom is van iemand anders. Met deze toewijzing wordt leden van die groep toegang verleend tot de resource. Daarna kan de eigenaar van de groep het lidmaatschap van de groep beheren. Dit houdt in dat de eigenaar van de resource de machtiging voor het toewijzen van gebruikers aan de resource delegeert naar de eigenaar van de groep.

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen. Zie [Een groep maken en leden toevoegen in Azure Active Directory](active-directory-groups-create-azure-portal.md) voor informatie over het beheren van groepen in het Azure AD-beheercentrum.

## <a name="how-do-i-create-a-group"></a>Hoe maak ik een groep?
Afhankelijk van de services waarop uw organisatie is geabonneerd, kunt u een groep maken met behulp van een van de volgende opties:

* de klassieke Azure Portal
* de Office 365-accountportal
* de Windows Intune-accountportal

Taken worden hier beschreven zoals ze in de klassieke Azure Portal worden uitgevoerd. Zie [Uw Azure AD-directory beheren](active-directory-administer.md) voor meer informatie over het gebruik van andere portals dan Azure Portal voor het beheer van uw Azure AD-directory.

1. Selecteer in de [klassieke Azure Portal](https://manage.windowsazure.com) de optie **Active Directory** en vervolgens de naam van de directory van uw organisatie.
2. Selecteer de tab **Groepen**.
3. Selecteer **Groep toevoegen**.
4. Geef in het venster **Groep toevoegen** de naam en de beschrijving van een groep op.

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Hoe kan ik afzonderlijke gebruikers in een beveiligingsgroep toevoegen of verwijderen?
**Een afzonderlijke gebruiker toevoegen aan een groep**

1. Selecteer in de [klassieke Azure Portal](https://manage.windowsazure.com) de optie **Active Directory** en vervolgens de naam van de directory van uw organisatie.
2. Selecteer de tab **Groepen**.
3. Open de groep waaraan u leden wilt toevoegen. Open het tabblad **Leden** van de geselecteerde groep als deze nog niet is weergegeven.
4. Selecteer **Leden toevoegen**.
5. Selecteer op de pagina **Leden toevoegen** de naam van de gebruiker of een groep die u als lid van deze groep wilt toevoegen. Zorg ervoor dat deze naam wordt toegevoegd aan het deelvenster **Geselecteerd**.

**Een afzonderlijke gebruiker verwijderen uit een groep**

1. Selecteer in de [klassieke Azure Portal](https://manage.windowsazure.com) de optie **Active Directory** en vervolgens de naam van de directory van uw organisatie.
2. Selecteer de tab **Groepen**.
3. Open de groep waaruit u leden wilt verwijderen.
4. Selecteer op het tabblad **Leden** de naam van het lid dat u wilt verwijderen uit deze groep en klik op **Verwijderen**.
5. Bevestig dat u dit lid uit de groep wilt verwijderen wanneer u hierom wordt gevraagd.

## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Hoe kan ik het lidmaatschap van een groep dynamisch beheren?
In Azure AD kunt u gemakkelijk een eenvoudige regel instellen om te bepalen welke gebruikers leden van de groep moeten worden. Een eenvoudige regel is een regel die slechts één vergelijking maakt. Als een groep bijvoorbeeld aan een SaaS-toepassing is toegewezen , kunt u een regel instellen voor het toevoegen van gebruikers met de functienaam ‘Vertegenwoordiger’. Deze regel verleent dan alle gebruikers met deze functietitel in uw directory toegang tot deze SaaS-toepassing.

Wanneer kenmerken van een gebruiker worden gewijzigd, evalueert het systeem alle dynamische groepsregels in een directory om te zien of de wijziging van het kenmerk van de gebruiker leidt tot wijzigingen uit of toevoegingen aan de groep. Als een gebruiker voldoet aan een regel voor een groep, wordt hij of zij toegevoegd als een lid van die groep. Als gebruikers niet langer voldoen aan de regel van een groep waarvan ze deel uitmaken, worden ze verwijderd als lid van die groep.

> [!NOTE]
> U kunt een regel instellen voor dynamisch lidmaatschap voor beveiligingsgroepen of Office 365-groepen. Op dit moment wordt genest groepslidmaatschap niet ondersteund voor toewijzing aan een toepassing op basis van een groep.
>
> Bij dynamisch lidmaatschap voor groepen moet een Azure AD Premium-licentie worden toegewezen aan
>
> * De beheerder die de regel voor een groep beheert
> * Alle leden van de groep
>
>

**Dynamische lidmaatschap inschakelen voor een groep**

1. Selecteer in de [klassieke Azure Portal](https://manage.windowsazure.com) de optie **Active Directory** en vervolgens de naam van de directory van uw organisatie.
2. Selecteer de tab **Groepen** en open de groep die u wilt bewerken.
3. Selecteer de tab **Configureren** en stel vervolgens **Dynamische lidmaatschappen inschakelen** in op **Ja**.
4. Stel een eenvoudige regel in voor de groep om te bepalen hoe dynamisch lidmaatschap werkt voor deze groep. Zorg ervoor dat de optie **Gebruikers toevoegen als** is geselecteerd en selecteer vervolgens een gebruikerseigenschap in de lijst (bijvoorbeeld afdeling, functietitel, enzovoort),
5. Selecteer vervolgens een voorwaarde (niet gelijk aan, gelijk aan, begint niet met, begint met, bevat niet, bevat, geen overeenkomst, overeenkomst).
6. Geef een vergelijkingswaarde op voor de geselecteerde gebruikerseigenschap.

Zie [Using attributes to create advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md) (Engelstalig) voor meer informatie over het maken van *geavanceerde* regels (regels met meerdere vergelijkingen).

## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Azure Active Directory cmdlets for configuring group settings](active-directory-accessmanagement-groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [What is Azure Active Directory?](active-directory-whatis.md) (Wat is Azure Active Directory?)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
