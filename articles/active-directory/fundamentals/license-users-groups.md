---
title: Wijs toe of verwijder licenties - Azure Active Directory | Microsoft Docs
description: Instructies over het toewijzen of verwijderen van licenties voor Azure Active Directory van uw gebruikers of groepen.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10346ea0aa292be33f820bd4e92434e2c58a89ab
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194305"
---
# <a name="assign-or-remove-licenses-using-the-azure-active-directory-portal"></a>Wijs toe of verwijder licenties met behulp van de Azure Active Directory-portal
Veel services van Azure Active Directory (Azure AD) moeten u een Azure AD-product te activeren en licenties voor elk van uw gebruikers of groepen (en bijbehorende leden) voor dat product. Alleen gebruikers met actieve licenties kunnen openen en gebruiken de gelicentieerde Azure AD-services.

## <a name="available-product-editions"></a>Beschikbare product-edities
Er zijn verschillende versies zijn beschikbaar voor de Azure AD-product.

- Azure AD Free

- Azure AD Basic

- Azure AD Premium 1 (Azure AD P1)

- Azure AD Premium 2 (Azure AD P2)

Zie voor meer informatie over elke editie van het product en de bijbehorende licentie details [welke-licentie heb ik nodig?](../authentication/concept-sspr-licensing.md).

## <a name="view-your-product-edition-and-license-details"></a>De details van uw product editie en licentie weergeven
U kunt uw beschikbare producten, met inbegrip van de afzonderlijke licenties weergeven voor alle in behandeling vervaldatums en het aantal beschikbare toewijzingen controleren.

### <a name="to-find-your-product-and-license-details"></a>Voor uw product en licentie-informatie
1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **licenties**.

    De **licenties** pagina wordt weergegeven.

    ![Licenties pagina, met het aantal aangeschafte producten en toegewezen licenties](media/license-users-groups/license-details-blade.png)
    
3. Selecteer de **aangeschafte producten** koppeling om weer te geven de **producten** pagina en ziet u de **toegewezen**, **beschikbaar**, en  **Verloopt binnenkort** details voor elke specifieke product-editie.

    ![Pagina van de producten, met de productedities en de bijbehorende licentie-informatie](media/license-users-groups/license-products-blade-with-products.png)

4. Selecteer de naam van een product editie om te zien van de gelicentieerde gebruikers en groepen.

## <a name="assign-licenses-to-users-or-groups"></a>Licenties toewijzen aan gebruikers of groepen
Zorg ervoor dat iedereen die gebruik van een gelicentieerde Azure AD-service de juiste licentie heeft. Het is aan u of u wilt toevoegen de licentierechten naar afzonderlijke gebruikers of een hele groep.

>[!Note]
>Groepslicenties is een openbare preview-functie van Azure AD en is beschikbaar met een betaald abonnement voor Azure AD-licentie. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.<br><br>Zie voor gedetailleerde informatie over het toevoegen van gebruikers [toevoegen of verwijderen van gebruikers in Azure Active Directory](add-users-azure-active-directory.md). Zie voor gedetailleerde informatie over het maken van groepen en leden toevoegen [een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md).

### <a name="to-assign-a-license-to-a-specific-user"></a>Een licentie toewijzen aan een specifieke gebruiker
1. Op de **producten** pagina, selecteert u de naam van de editie die u wilt toewijzen aan de gebruiker. Bijvoorbeeld, _Azure Active Directory Premium Plan 2_.

    ![Pagina van de producten, met gemarkeerde product-editie](media/license-users-groups/license-products-blade-with-product-highlight.png)

2. Op de **Azure Active Directory Premium Plan 2** weergeeft, schakelt **toewijzen**.

    ![Pagina van de producten, met de optie voor gemarkeerde toewijzen](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

3. Op de **toewijzen** weergeeft, schakelt **gebruikers en groepen**, en vervolgens zoekt en selecteert u de gebruiker die u bij het toewijzen van de licentie. Bijvoorbeeld, _Mary Parker_.

    ![Licentie-pagina met Selecteer opties voor gemarkeerde zoeken en toewijzen](media/license-users-groups/assign-license-blade-with-highlight.png)

4. Selecteer **toewijzingsopties**, zorg ervoor dat u hebt de juiste licentie-opties die zijn ingeschakeld en selecteer vervolgens **OK**.

    ![Licentie optie-pagina met alle van de beschikbare opties in de editie](media/license-users-groups/license-option-blade-assignments.png)

    De **licentie toewijzen** pagina updates om weer te geven dat een gebruiker is geselecteerd en of de toewijzingen zijn geconfigureerd.

    >[!NOTE]
    >Niet alle Microsoft-services zijn beschikbaar in alle locaties. Voordat een licentie kan worden toegewezen aan een gebruiker, moet u de **gebruikslocatie**. U kunt deze waarde instellen in de **Azure Active Directory &gt; gebruikers &gt; profiel &gt; instellingen** gebied in Azure AD.

5. Selecteer **Toewijzen**.

    De gebruiker wordt toegevoegd aan de lijst met gebruikers met een licentie en toegang heeft tot de Azure AD-services.

### <a name="to-assign-a-license-to-an-entire-group"></a>Een licentie toewijzen aan een hele groep
1. Op de **producten** pagina, selecteert u de naam van de editie die u wilt toewijzen aan de gebruiker. Bijvoorbeeld, _Azure Active Directory Premium Plan 2_.

    ![De blade producten, met een editie van het gemarkeerde](media/license-users-groups/license-products-blade-with-product-highlight.png)

2. Op de **Azure Active Directory Premium Plan 2** weergeeft, schakelt **toewijzen**.

    ![Pagina van de producten, met de optie voor gemarkeerde toewijzen](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

3. Op de **toewijzen** weergeeft, schakelt **gebruikers en groepen**, en vervolgens zoekt en selecteert u de groep die u bij het toewijzen van de licentie. Bijvoorbeeld, _MDM-beleid - West_.

    ![Licentie-pagina met Selecteer opties voor gemarkeerde zoeken en toewijzen](media/license-users-groups/assign-group-license-blade-with-highlight.png)

4. Selecteer **toewijzingsopties**, zorg ervoor dat u hebt de juiste licentie-opties die zijn ingeschakeld en selecteer vervolgens **OK**.

    ![Licentie optie-pagina met alle van de beschikbare opties in de editie](media/license-users-groups/license-option-blade-group-assignments.png)

    De **licentie toewijzen** pagina updates om weer te geven dat een gebruiker is geselecteerd en of de toewijzingen zijn geconfigureerd.

    >[!NOTE]
    >Niet alle Microsoft-services zijn beschikbaar in alle locaties. Voordat een licentie kan worden toegewezen aan een groep, moet u de **gebruikslocatie** voor alle leden. U kunt deze waarde instellen in de **Azure Active Directory &gt; gebruikers &gt; profiel &gt; instellingen** gebied in Azure AD. Een gebruiker waarvan gebruikslocatie is niet opgegeven, neemt de locatie van de tenant.

5. Selecteer **Toewijzen**.

    De groep wordt toegevoegd aan de lijst met groepen met licenties en alle leden van de hebben toegang tot de Azure AD-services.


## <a name="remove-a-license"></a>Een licentie verwijderen
U kunt een licentie verwijderen uit een gebruiker of een groep in de **licenties** pagina.

### <a name="to-remove-a-license-from-a-specific-user"></a>Verwijderen van een licentie van een specifieke gebruiker
1. Op de **gelicentieerde gebruikers** pagina voor de product-editie, selecteert u de gebruiker die de licentie niet meer hebt. Bijvoorbeeld, _Alain Charon_.

2. Selecteer **verwijderen licentie**.

    ![Gebruikers met een licentie-pagina met de optie licentie verwijderen gemarkeerd](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

### <a name="to-remove-a-license-from-a-group"></a>Een licentie van een groep verwijderen
1. Op de **groepen in licentie gegeven** pagina voor de product-editie, selecteert u de groep die de licentie niet meer hebt. Bijvoorbeeld, _MDM-beleid - West_.

2. Selecteer **verwijderen licentie**.

    ![Groepen met licenties pagina met de optie licentie verwijderen gemarkeerd](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)

>[!Important]
>Licenties die zijn overgenomen door een gebruiker uit een groep kunnen niet rechtstreeks worden verwijderd. In plaats daarvan moet u de gebruiker te verwijderen uit de groep waarvan ze de licentie bent overnemen.

## <a name="next-steps"></a>Volgende stappen
Nadat u uw licenties hebt toegewezen, kunt u de volgende processen uitvoeren:

- [Identificeren en oplossen van problemen toewijzing](../users-groups-roles/licensing-groups-resolve-problems.md)

- [Gelicentieerde gebruikers toevoegen aan een groep voor licentieverlening](../users-groups-roles/licensing-groups-migrate-users.md)

- [Scenario's, beperkingen en bekende problemen met behulp van groepen beheren in Azure Active Directory-licentieverlening](../users-groups-roles/licensing-group-advanced.md)

- [Toevoegen of wijzigen van de profielgegevens](active-directory-users-profile-azure-portal.md)
