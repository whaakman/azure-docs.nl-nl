---
title: Toevoegen of verwijderen van groepseigenaren - Azure Active Directory | Microsoft Docs
description: Groepseigenaren van instructies over het toevoegen of verwijderen met behulp van Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: d259be0c04af0fcf3628a9f296730749404610cb
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562088"
---
# <a name="add-or-remove-group-owners-in-azure-active-directory"></a>Toevoegen of verwijderen van eigenaren van groepen in Azure Active Directory
Azure Active Directory (Azure AD)-beveiligingsgroepen zijn eigendom van en beheerd door eigenaren van groepen. Groeps eigenaren kunnen gebruikers of service-principals zijn en kunnen de groep met inbegrip van lidmaatschap beheren. Alleen bestaande groeps eigenaren of groep-Beheerders kunnen groeps onwers toewijzen. Groepseigenaren hoeven te worden leden van de groep.

Wanneer een groep geen eigenaar heeft, kunnen beheerders van groeps beheer de groep blijven beheren.

## <a name="add-an-owner-to-a-group"></a>Eigenaar toevoegen aan een groep
Hieronder vindt u instructies voor het toevoegen van een gebruiker als een eigenaar aan een groep met behulp van de Azure AD-Portal. Als u een Service-Principal als eigenaar van een groep wilt toevoegen, volgt u de instructies om dit te doen met behulp van [Power shell](https://docs.microsoft.com/powershell/module/Azuread/Add-AzureADGroupOwner?view=azureadps-2.0).

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
