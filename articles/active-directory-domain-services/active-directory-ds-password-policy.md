---
title: 'Azure Active Directory Domain Services: Wachtwoordbeleid | Microsoft Docs'
description: Wachtwoordbeleid op beheerde domeinen begrijpen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: ergreenl
ms.openlocfilehash: 0bb3ccb3841b60f6c94ac1d6126bd2b0d44af092
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54852691"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Wachtwoord- en accountvergrendelingsbeleid op beheerde domeinen
In dit artikel wordt uitgelegd dat de standaard-wachtwoordbeleid in een beheerd domein. Ook wordt beschreven hoe u dit beleid kunt configureren.

## <a name="fine-grained-password-policies-fgpp"></a>Goed korrelig wachtwoordbeleid (FGPP)
Fijnmazig wachtwoordbeleid gebruiken om op te geven veel wachtwoordbeleidsregels binnen één domein. FGPP kunt u verschillende beperkingen voor wachtwoord- en Accountvergrendelingsbeleid toepassen op verschillende sets van gebruikers in een domein. U kunt bijvoorbeeld strikte wachtwoordinstellingen toepassen op accounts met bevoegdheden.

U kunt de volgende wachtwoordinstellingen met behulp van FGPP configureren:
* Minimale wachtwoordlengte
* Geschiedenis van apparaatwachtwoorden
* Wachtwoorden moeten voldoen aan complexiteitsvereisten
* Minimale gebruiksduur wachtwoord
* Maximale wachtwoordduur
* Accountvergrendelingsbeleid
    * Account vergrendelen gedurende
    * Aantal mislukte aanmeldingspogingen toegestaan
    * Mislukte aanmeldingspogingen opnieuw instellen wordt tellen na


## <a name="default-fine-grained-password-policy-settings-on-a-managed-domain"></a>Standaard instellingen voor goed korrelig wachtwoordbeleid in een beheerd domein
De volgende schermafbeelding ziet u het standaardbeleid voor goed korrelig dat wachtwoorden die worden geconfigureerd op een beheerd domein van Azure AD Domain Services.

![Standaard goed korrelig wachtwoordbeleid](./media/how-to/default-fgpp.png)

> [!NOTE]
> U kan wijzigen of verwijderen van de standaard ingebouwde goed korrelig wachtwoordbeleid. Leden van de groep 'AAD DC Administrators' kunnen aangepaste FGPP maken en configureren om op te heffen (hebben voorrang op) de standaard ingebouwde FGPP.
>
>

## <a name="password-policy-settings"></a>Instellingen voor het beleid
Standaard worden de volgende wachtwoordbeleid in een beheerd domein geconfigureerd:
* Minimale wachtwoordlengte (tekens): 7
* Maximale wachtwoordduur (levensduur): 90 dagen
* Wachtwoorden moeten voldoen aan complexiteitsvereisten

### <a name="account-lockout-settings"></a>Accountvergrendeling
Het volgende beleid voor accountvergrendeling zijn standaard geconfigureerd op een beheerd domein:
* Account vergrendelen gedurende: 30
* Het aantal mislukte aanmeldingspogingen toegestaan: 5
* Opnieuw instellen van aanmeldingspogingen mislukte tellen na: 30 minuten

Effectief, zijn gebruikersaccounts vergrendeld voor 30 minuten als vijf ongeldige wachtwoorden worden gebruikt binnen twee minuten. Accounts zijn automatisch ontgrendelen na 30 minuten.


## <a name="create-a-custom-fine-grained-password-policy-fgpp-on-a-managed-domain"></a>Een aangepaste goed korrelig wachtwoordbeleid (FGPP) maken in een beheerd domein
U kunt een aangepaste FGPP maken en toepassen op specifieke groepen in uw beheerde domein. Deze configuratie is effectief voorrang boven de standaard-FGPP geconfigureerd voor het beheerde domein.

> [!TIP]
> Alleen leden van de **'AAD DC Administrators'** groep hebben de machtigingen voor het maken van aangepaste goed korrelig wachtwoordbeleid.
>
>

U kunt daarnaast ook aangepaste goed korrelig wachtwoordbeleid maken en deze toepassen op alle aangepaste OE's die u in het beheerde domein maakt.

U kunt een aangepaste FGPP configureren voor de volgende redenen:
* Een ander account lockout beleid instellen.
* Het configureren van een standaardinstelling voor het levensduur van wachtwoord voor het beheerde domein.

Een aangepaste FGPP maken op uw beheerde domein:
1. Aanmelden bij de Windows-VM die u gebruikt voor het beheren van uw beheerde domein. Als u niet hebt, volgt u de instructies voor het [een beheerd domein beheren](active-directory-ds-admin-guide-administer-domain.md)
2. Start de **Active Directory-beheercentrum** op de virtuele machine.
3. Klik op de naam van het domein (bijvoorbeeld ' contoso100.com').
4. Dubbelklik op **System** openen van de container.
5. Dubbelklik op **Password Settings Container**.
6. U ziet dat de standaard ingebouwde FGPP voor het beheerde domein met de naam **AADDSSTFPSO**. U kunt deze ingebouwde FGPP niet wijzigen. U kunt echter een nieuwe aangepaste FGPP onderdrukking van de standaard FGPP maken.
7. Op de **taken** deelvenster in de juiste, op **nieuw** en klikt u op **wachtwoordinstellingen**.
8. In de **wachtwoordinstellingen maken** dialoogvenster, geef de wachtwoordinstellingen op aangepaste om toe te passen als onderdeel van de aangepaste FGPP. Vergeet niet om in te stellen de voorrang op de juiste wijze het standaard FGPP.

  ![Aangepaste FGPP maken](./media/how-to/custom-fgpp.png)

  > [!TIP]
  > **Houd er rekening mee de beveiligen tegen onopzettelijk verwijderen optie uitschakelen.** Als deze optie is ingeschakeld, kan de FGPP kan niet worden opgeslagen.
  >
  >

9. In **rechtstreeks van toepassing op**, klikt u op de **toevoegen** knop. In de **gebruikers of groepen selecteren** dialoogvenster, klikt u op de **locaties** knop.

  ![Gebruikers en groepen selecteren](./media/how-to/fgpp-applies-to.png)

10. In de **locaties** dialoogvenster, vouwt u de domeinnaam en op **AADDC gebruikers**. U kunt nu een groep selecteren van de ingebouwde gebruikers organisatie-eenheid waaraan de FGPP toepassen.

  ![Selecteer de organisatie-eenheid die groep behoort](./media/how-to/fgpp-container.png)

11. Typ de naam van de groep en klikt u op de **namen controleren** knop voor het valideren van de groep bestaat.

  ![Selecteer de groep om toe te passen FGPP](./media/how-to/fgpp-apply-group.png)

12. De naam van de groep wordt weergegeven in **rechtstreeks van toepassing op** sectie. Klik op de **OK** knop deze wijzigingen op te slaan.

  ![FGPP toegepast](./media/how-to/fgpp-applied.png)

> [!TIP]
> **Toepassen van beleid voor aangepaste wachtwoorden voor gebruikersaccounts in een aangepaste organisatie-eenheid:** Goed korrelig wachtwoordbeleid kunnen worden alleen toegepast op groepen. Voor het configureren van een aangepaste wachtwoordbeleid alleen voor gebruikers van een aangepaste organisatie-eenheid, een groep met gebruikers in de organisatie-eenheid te maken.
>
>

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over Active Directory goed korrelig wachtwoordbeleid](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [Goed korrelig wachtwoordbeleid met behulp van AD-beheercentrum configureren](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)
