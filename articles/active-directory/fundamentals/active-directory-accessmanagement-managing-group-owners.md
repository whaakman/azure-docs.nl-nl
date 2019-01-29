---
title: Toevoegen of verwijderen van groepseigenaren - Azure Active Directory | Microsoft Docs
description: Groepseigenaren van instructies over het toevoegen of verwijderen met behulp van Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.openlocfilehash: 887f376eafadfb556ce69611195301b25b24bbd5
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104249"
---
# <a name="add-or-remove-group-owners-in-azure-active-directory"></a>Toevoegen of verwijderen van eigenaren van groepen in Azure Active Directory
Azure Active Directory (Azure AD)-beveiligingsgroepen zijn eigendom van en beheerd door eigenaren van groepen. Eigenaren van groepen worden toegewezen voor het beheren van een groep en de bijbehorende leden van een resource-eigenaar (beheerder). Groepseigenaren hoeven te worden leden van de groep. Nadat de Groepseigenaar van een is toegewezen, kunt alleen de eigenaar van een resource toevoegen of eigenaren verwijderen.

In sommige gevallen besluiten u als de beheerder toe te wijzen een eigenaar van de groep. In dit geval kunt u de eigenaar van de groep worden. Bovendien kunnen eigenaren van andere eigenaren toewijzen aan de groep, tenzij u dit hebt beperkt in de instellingen voor de.

## <a name="add-an-owner-to-a-group"></a>Eigenaar toevoegen aan een groep
Aanvullende groepseigenaren toevoegen aan een groep met behulp van Azure AD.

### <a name="to-add-a-group-owner"></a>De Groepseigenaar van een toevoegen
1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, selecteer **groepen**, en selecteer vervolgens de groep waarvoor u wilt een eigenaar toevoegen (in dit voorbeeld *MDM-beleid - West*).

3. Op de **MDM-beleid - West-overzicht** weergeeft, schakelt **eigenaren**.

    ![Beleid voor MDM - West-overzichtspagina met eigenaren-optie is gemarkeerd](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. Op de **eigenaar van de Groepsbeleid - West - MDM** weergeeft, schakelt **eigenaren toevoegen**, en vervolgens zoekt en selecteert u de gebruiker die eigenaar van de nieuwe groep en kies vervolgens **Selecteer**.

    ![MDM - West - beleid eigenaars-pagina met de optie eigenaren toevoegen gemarkeerd](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Nadat u de nieuwe eigenaar selecteren, kunt u vernieuwen de **eigenaren** pagina en ziet de naam toegevoegd aan de lijst met eigenaren.

## <a name="remove-an-owner-from-a-group"></a>Een eigenaar verwijderen uit een groep
Een eigenaar verwijderen uit een groep met behulp van Azure AD.

### <a name="to-remove-an-owner"></a>Een eigenaar verwijderen
1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, selecteer **groepen**, en selecteer vervolgens de groep waarvoor u wilt een eigenaar wilt verwijderen (in dit voorbeeld *MDM-beleid - West*).

3. Op de **MDM-beleid - West-overzicht** weergeeft, schakelt **eigenaren**.

    ![Beleid voor MDM - West-overzichtspagina met eigenaren-optie is gemarkeerd](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. Op de **eigenaar van de Groepsbeleid - West - MDM** pagina, selecteert u de gebruiker die u wilt verwijderen als Groepseigenaar van een, kies **verwijderen** uit de pagina met informatie van de gebruiker en selecteer **Ja** om te bevestigen uw beslissing.

    ![Informatie-pagina van de gebruiker met de optie verwijderen gemarkeerd](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    Nadat u de eigenaar van de verwijdert, kunt u terugkeren naar de **eigenaren** pagina en ziet de naam is verwijderd uit de lijst met eigenaren.

## <a name="next-steps"></a>Volgende stappen
- [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)

- [Azure Active Directory cmdlets for configuring group settings](../users-groups-roles/groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)

- [Groepen gebruiken voor toegang tot een geïntegreerde SaaS-app toewijzen](../users-groups-roles/groups-saasapps.md)

- [Uw on-premises identiteiten integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md)

- [Azure Active Directory cmdlets for configuring group settings](../users-groups-roles/groups-settings-v2-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
