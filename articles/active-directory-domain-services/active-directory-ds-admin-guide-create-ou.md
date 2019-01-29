---
title: 'Azure Active Directory Domain Services: Administration Guide | Microsoft Docs'
description: Maken van een organisatie-eenheid (OE) in Azure AD Domain Services beheerde domeinen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 33c4264d9aeffb721cd2d44df6d551126782f0e1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165707"
---
# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Maken van een organisatie-eenheid (OE) in een Azure AD Domain Services beheerde domein
Azure AD Domain Services beheerde domeinen bevatten twee ingebouwde containers respectievelijk 'AADDC Computers' en 'AADDC gebruikers' genoemd. De container 'AADDC Computers' heeft computerobjecten voor alle computers die zijn gekoppeld aan het beheerde domein. De container 'AADDC gebruikers' bevat gebruikers en groepen in de Azure AD-tenant. Af en toe kan het nodig zijn voor het maken van serviceaccounts in het beheerde domein om workloads te implementeren zijn. U kunt voor dit doel een aangepaste organisatie-eenheid (OE) in het beheerde domein maken en service-accounts binnen een organisatie-eenheid maken. Dit artikel leest u hoe een organisatie-eenheid maken in uw beheerde domein.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Een domein virtuele machine van waaruit u de Azure AD Domain Services beheerde domein beheren. Als u geen dergelijke een virtuele machine hebt, volgt u alle taken die worden beschreven in het artikel [Windows virtuele machine toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U moet de referenties van een **gebruikersaccount die behoren tot de groep 'AAD DC Administrators'** in uw directory, een aangepaste organisatie-eenheid maken op uw beheerde domein.

## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>AD-beheerprogramma's installeren op een virtuele machine domein voor beheer op afstand
Azure AD Domain Services beheerde domeinen kunnen worden beheerd op afstand met vertrouwde Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum (ADAC) of AD PowerShell. Tenantbeheerders geen bevoegdheden voor verbinding met de domeincontrollers in het beheerde domein via Extern bureaublad. Installeer de functie AD administration tools op een virtuele machine toegevoegd aan het beheerde domein voor het beheren van het beheerde domein. Raadpleeg het artikel met de titel [een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md) voor instructies.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Maken van een organisatie-eenheid in het beheerde domein
Nu dat de AD-beheerprogramma's zijn geïnstalleerd op de virtuele machine van een domein, we kunnen deze hulpprogramma's gebruiken voor het maken van een organisatie-eenheid in het beheerde domein. Voer de volgende stappen uit:

> [!NOTE]
> Alleen leden van de groep 'AAD DC Administrators' hebben de vereiste bevoegdheden voor het maken van een aangepaste organisatie-eenheid. Zorg ervoor dat u de volgende stappen uitvoeren als een gebruiker die deel uitmaakt van deze groep.
>
>

1. Klik vanuit het startscherm op **Systeembeheer**. Hier ziet u de AD beheerprogramma's geïnstalleerd op de virtuele machine.

    ![Beheerprogramma's geïnstalleerd op server](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Klik op **Active Directory-beheercentrum**.

    ![Active Directory-beheercentrum](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Als u het domein, klikt u op de naam van het domein in het linkerdeelvenster (bijvoorbeeld ' contoso100.com').

    ![Active Directory-Beheercentrum - domein weergeven](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)
4. Aan de rechterkant **taken** deelvenster, klikt u op **nieuw** onder de naam van knooppunt van het domein. In dit voorbeeld we klikt u op **nieuw** onder het knooppunt 'contoso100(local)' aan de rechterkant **taken** deelvenster.

    ![Active Directory-Beheercentrum - nieuwe organisatie-eenheid](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)
5. Hier ziet u de optie voor het maken van een organisatie-eenheid. Klik op **organisatie-eenheid** starten de **organisatie-eenheid maken** dialoogvenster.
6. In de **organisatie-eenheid maken** dialoogvenster, Geef een **naam** voor de nieuwe organisatie-eenheid. Geef een korte beschrijving voor de organisatie-eenheid. U kunt ook instellen de **beheerd door** veld voor de organisatie-eenheid. Klik op om de aangepaste organisatie-eenheid **OK**.

    ![Active Directory-Beheercentrum - dialoogvenster van de organisatie-eenheid maken](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)
7. De zojuist gemaakte organisatie-eenheid wordt nu weergegeven in het AD-beheercentrum (ADAC).

    ![Active Directory-Beheercentrum - organisatie-eenheid gemaakt](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="permissionssecurity-for-newly-created-ous"></a>Machtigingen/beveiliging voor de nieuwe organisatie-eenheden
Standaard wordt de gebruiker (lid van de groep 'AAD DC Administrators') die de aangepaste organisatie-eenheid gemaakt beheerdersbevoegdheden (volledig beheer) verleend via de organisatie-eenheid. De gebruiker kan vervolgens gaat u verder en bevoegdheden verlenen aan andere gebruikers of aan de groep 'AAD DC Administrators' naar wens. Zoals te zien is in de volgende schermopname is de gebruiker 'bob@domainservicespreview.onmicrosoft.com' die de nieuwe 'MyCustomOU' organisatie-eenheid hebt gemaakt, wordt de volledige controle over het worden verleend.

 ![Active Directory-Beheercentrum - nieuwe organisatie-eenheid-beveiliging](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)

## <a name="notes-on-administering-custom-ous"></a>Opmerkingen over het beheren van aangepaste organisatie-eenheden
Nu dat u een aangepaste organisatie-eenheid hebt gemaakt, kunt u direct verder gaan en gebruikers, groepen, computers en service-accounts in deze organisatie-eenheid maken. U kunt gebruikers of groepen van de 'AADDC gebruikers' organisatie-eenheid niet verplaatsen naar aangepaste organisatie-eenheden.

> [!WARNING]
> Gebruikersaccounts, groepen, service-accounts en computerobjecten die u onder aangepaste OE's maakt zijn niet beschikbaar in uw Azure AD-tenant. Met andere woorden weergeven deze objecten niet met behulp van de Azure AD Graph API of in de gebruikersinterface van Azure AD. Deze objecten zijn alleen beschikbaar in uw Azure AD Domain Services beheerde domein.
>
>

## <a name="related-content"></a>Gerelateerde inhoud
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Groepsbeleid configureren op een beheerd domein](active-directory-ds-admin-guide-administer-group-policy.md)
* [Active Directory-beheercentrum: Aan de slag](https://technet.microsoft.com/library/dd560651.aspx)
* [Stapsgewijze handleiding voor Service-Accounts](https://technet.microsoft.com/library/dd548356.aspx)
