---
title: 'Azure Active Directory Domain Services: Wachtwoord beleid | Microsoft Docs'
description: Wachtwoord beleid in beheerde domeinen begrijpen
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: 8829f16e580d0b926781ce0a3e9f8e6a63cf3110
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68853777"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Wacht woord-en account vergrendelings beleid in beheerde domeinen
In dit artikel wordt het standaard wachtwoord beleid voor een beheerd domein uitgelegd. Ook wordt beschreven hoe u dit beleid kunt configureren.

## <a name="fine-grained-password-policies-fgpp"></a>Verfijnd wachtwoord beleid (FGPP)
Gebruik een nauw keurig wachtwoord beleid om veel wachtwoord beleid binnen één domein op te geven. Met FGPP kunt u verschillende beperkingen voor wacht woord-en account vergrendelings beleid Toep assen op verschillende groepen gebruikers in een domein. U kunt bijvoorbeeld strikte wachtwoord instellingen Toep assen op bevoegde accounts.

U kunt de volgende wachtwoord instellingen configureren met behulp van FGPP:
* Minimale wachtwoord lengte
* Wachtwoord geschiedenis
* Wacht woorden moeten voldoen aan complexiteits vereisten
* Minimale wachtwoord duur
* Maximale wachtwoord duur
* Account vergrendelings beleid
    * Account vergrendelings duur
    * Aantal mislukte aanmeldings pogingen toegestaan
    * Aantal mislukte aanmeldings pogingen opnieuw instellen na


## <a name="default-fine-grained-password-policy-settings-on-a-managed-domain"></a>Standaard instellingen voor het wachtwoord beleid in een beheerd domein
De volgende scherm afbeelding illustreert het standaard beleid voor verfijnd gekorrelde wacht woorden dat is geconfigureerd op een Azure AD Domain Services beheerd domein.

![Standaard verfijnd wachtwoord beleid](./media/how-to/default-fgpp.png)

> [!NOTE]
> U kunt het ingebouwde standaard wachtwoord beleid niet wijzigen of verwijderen. Leden van de groep ' AAD DC Administrators ' kunnen aangepaste FGPP maken en deze configureren voor onderdrukking (prioriteit hebben boven) de standaard ingebouwde FGPP.
>
>

## <a name="password-policy-settings"></a>Instellingen voor wachtwoord beleid
In een beheerd domein worden standaard de volgende wachtwoord beleidsregels geconfigureerd:
* Minimale wachtwoord lengte (tekens): 7
* Maximale wachtwoord duur (levens duur): 90 dagen
* Wacht woorden moeten voldoen aan complexiteits vereisten

### <a name="account-lockout-settings"></a>Instellingen voor account vergrendeling
In een beheerd domein worden standaard de volgende beleids regels voor account vergrendeling geconfigureerd:
* Account vergrendelings duur: 30
* Aantal mislukte aanmeldings pogingen toegestaan: 5
* Aantal mislukte aanmeldings pogingen opnieuw instellen na: 30 minuten

In feite worden gebruikers accounts 30 minuten vergrendeld als vijf ongeldige wacht woorden binnen twee minuten worden gebruikt. Accounts worden na 30 minuten automatisch ontgrendeld.


## <a name="create-a-custom-fine-grained-password-policy-fgpp-on-a-managed-domain"></a>Een aangepast verfijnd wachtwoord beleid (FGPP) maken in een beheerd domein
U kunt een aangepaste FGPP maken en deze Toep assen op specifieke groepen in uw beheerde domein. Deze configuratie heeft een onjuiste prioriteit voor de standaard-FGPP die voor het beheerde domein zijn geconfigureerd.

> [!TIP]
> Alleen leden van de groep **' Aad DC Administrators '** hebben de machtigingen voor het maken van aangepaste beleids regels met een verfijnd wacht woord.
>
>

Daarnaast kunt u ook aangepaste, verfijnde wachtwoord beleidsregels maken en deze Toep assen op aangepaste organisatie-eenheden die u maakt in het beheerde domein.

U kunt een aangepaste FGPP configureren om de volgende redenen:
* Om een ander account vergrendelings beleid in te stellen.
* Voor het configureren van een standaard instelling voor het wacht woord levens duur voor het beheerde domein.

Een aangepast FGPP maken op uw beheerde domein:
1. Meld u aan bij de Windows-VM die u gebruikt voor het beheren van uw beheerde domein (moet mini maal Windows Server 2012 R2 zijn). Als u er nog geen hebt, volgt u de instructies voor het [beheren van een Azure AD Domain Services domein](manage-domain.md).
2. Start de **Active Directory-beheercentrum** op de VM.
3. Klik op de domein naam (bijvoorbeeld ' contoso100.com ').
4. Dubbel klik op **systeem** om de systeem container te openen.
5. Dubbel klik op **Password Settings Container**.
6. U ziet de standaard ingebouwde FGPP voor het beheerde domein met de naam **AADDSSTFPSO**. U kunt deze ingebouwde FGPP niet wijzigen. U kunt echter een nieuwe aangepaste FGPP maken, waarbij u de standaard FGPP overschrijft.
7. Klik in het deel venster **taken** aan de rechter kant op **Nieuw** en klik op **wachtwoord instellingen**.
8. Geef in het dialoog venster **wacht woord instellingen maken** de aangepaste wachtwoord instellingen op die moeten worden toegepast als onderdeel van de aangepaste FGPP. Zorg ervoor dat u de prioriteit op de juiste wijze instelt om de standaard FGPP te onderdrukken.

   ![Aangepaste FGPP maken](./media/how-to/custom-fgpp.png)

   > [!TIP]
   > **Vergeet niet om de optie beveiligen tegen onopzettelijk verwijderen uit te vinken.** Als deze optie is geselecteerd, kan de FGPP niet worden opgeslagen.
   >
   >

9. Klik in **direct van toepassing**op op de knop **toevoegen** . Klik in het dialoog venster **gebruikers of groepen selecteren** op de knop **locaties** .

   ![Gebruikers en groepen selecteren](./media/how-to/fgpp-applies-to.png)

10. Vouw in het dialoog venster **locaties** de domein naam uit en klik op **AADDC-gebruikers**. U kunt nu een groep selecteren in de organisatie-eenheid met ingebouwde gebruikers, waarop de FGPP moet worden toegepast.

    ![Selecteer de organisatie-eenheid waarvan de groep deel uitmaakt](./media/how-to/fgpp-container.png)

11. Typ de naam van de groep en klik op de knop **Namen controleren** om te controleren of de groep bestaat.

    ![Selecteer de groep die u wilt Toep assen FGPP](./media/how-to/fgpp-apply-group.png)

12. De naam van de groep wordt weer gegeven in de sectie **direct van toepassing op** . Klik op de knop **OK** om deze wijzigingen op te slaan.

    ![FGPP toegepast](./media/how-to/fgpp-applied.png)

> [!TIP]
> **Aangepaste wachtwoord beleid Toep assen op gebruikers accounts in een aangepaste organisatie-eenheid:** Verfijnd wachtwoord beleid kan alleen worden toegepast op groepen. Als u een aangepast wachtwoord beleid alleen voor gebruikers van een aangepaste OE wilt configureren, maakt u een groep die gebruikers bevat in die organisatie-eenheid.
>
>

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over Active Directory verfijnd wachtwoord beleid](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [Verfijnd wachtwoord beleid configureren met het AD-beheer centrum](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)
