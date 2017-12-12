---
title: 'Azure Active Directory Domain Services: Beheerdershandleiding | Microsoft Docs'
description: Maken van een organisatie-eenheid (OE) in Azure AD Domain Services beheerd-domeinen
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 197696d737e56cbdc9fe925b6fa5b9e4134e1539
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Maken van een organisatie-eenheid (OE) op een beheerd domein van Azure AD Domain Services
Azure AD Domain Services beheerde domeinen bevatten twee ingebouwde containers respectievelijk 'AADDC Computers' en 'AADDC gebruikers' genoemd. De container 'AADDC Computers' heeft de computerobjecten voor alle computers die lid zijn van het beheerde domein. De container 'AADDC gebruikers' bevat gebruikers en groepen in de Azure AD-tenant. In sommige gevallen kan het mogelijk nodig serviceaccounts maken op het beheerde domein voor het implementeren van werkbelastingen. U kunt voor dit doel een aangepaste organisatie-eenheid (OE) op het beheerde domein maken en serviceaccounts binnen de organisatie-eenheid maken. In dit artikel leest u hoe een organisatie-eenheid maken in uw beheerde domein.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Een domein-virtuele machine van waaruit u de Azure AD Domain Services beheerd domein beheren. Als u een dergelijke virtuele machine geen hebt, volgt u alle taken die worden beschreven in het artikel [een virtuele Windows-computer toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U moet de referenties van een **gebruikersaccount van de ' AAD DC' beheerdersgroep** in uw directory voor het maken van een aangepaste organisatie-eenheid op uw beheerde domein.

## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>AD-beheerprogramma's installeren op een virtuele machine domein voor extern beheer
Azure AD Domain Services beheerde domeinen kunnen worden beheerd op afstand met vertrouwd Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum (ADAC) of AD PowerShell. Tenantbeheerders hoeft geen bevoegdheden voor verbinding met domeincontrollers op het beheerde domein via Extern bureaublad. Installeer de functie AD administration tools op een virtuele machine toegevoegd aan het beheerde domein voor het beheren van het beheerde domein. Raadpleeg het artikel [een Azure AD Domain Services beheerd domein beheren](active-directory-ds-admin-guide-administer-domain.md) voor instructies.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Maken van een organisatie-eenheid op het beheerde domein
Nu de AD-beheerprogramma's zijn geïnstalleerd op de virtuele machine van het domein, kunnen we deze hulpprogramma's gebruiken voor het maken van een organisatie-eenheid op het beheerde domein. De volgende stappen uitvoeren:

> [!NOTE]
> Alleen leden van de groep 'AAD DC Administrators' hebben de vereiste machtigingen voor het maken van een aangepaste organisatie-eenheid. Zorg ervoor dat u de volgende stappen uitvoeren als een gebruiker die aan deze groep behoort.
>
>

1. Klik vanuit het startscherm op **Systeembeheer**. Hier ziet u de AD-beheerprogramma's geïnstalleerd op de virtuele machine.

    ![Beheerprogramma's geïnstalleerd op server](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Klik op **Active Directory-beheercentrum**.

    ![Active Directory-beheercentrum](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Als u wilt weergeven in het domein, klikt u op de naam van het domein in het linkerdeelvenster (bijvoorbeeld, contoso100.com').

    ![Active Directory-Beheercentrum - domein weergeven](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)
4. Aan de rechterkant **taken** deelvenster, klikt u op **nieuw** onder de naam van knooppunt van het domein. In dit voorbeeld klikt raadzaam **nieuw** onder het knooppunt 'contoso100(local)' aan de rechterkant **taken** deelvenster.

    ![Active Directory-Beheercentrum - nieuwe organisatie-eenheid](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)
5. Hier ziet u de optie voor het maken van een organisatie-eenheid. Klik op **organisatie-eenheid** starten de **organisatie-eenheid maken** dialoogvenster.
6. In de **organisatie-eenheid maken** dialoogvenster, Geef een **naam** voor de nieuwe organisatie-eenheid. Geef een korte beschrijving voor de organisatie-eenheid. U kunt ook instellen de **beheerd door** veld voor de organisatie-eenheid. Klik op om de aangepaste organisatie-eenheid **OK**.

    ![Active Directory-Beheercentrum - dialoogvenster van de organisatie-eenheid maken](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)
7. De zojuist gemaakte organisatie-eenheid wordt nu weergegeven in de AD Administrative Center (ADAC).

    ![Active Directory-Beheercentrum - organisatie-eenheid gemaakt](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="permissionssecurity-for-newly-created-ous"></a>Machtigingen/beveiliging voor de zojuist gemaakte organisatie-eenheden
Standaard krijgt de gebruiker (lid van de groep 'AAD DC Administrators') die de aangepaste organisatie-eenheid gemaakt bevoegdheden als beheerder (volledig beheer) via de organisatie-eenheid. De gebruiker kan vervolgens opwekken en rechten verlenen aan andere gebruikers of aan de groep 'AAD DC Administrators' desgewenst. Zoals te zien is in de volgende schermafbeelding, de gebruiker 'bob@domainservicespreview.onmicrosoft.com' volledige controle over het maker van de organisatie-eenheid van de nieuwe 'MyCustomOU' is toegekend.

 ![Active Directory-Beheercentrum - nieuwe beveiliging van de organisatie-eenheid](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)

## <a name="notes-on-administering-custom-ous"></a>Opmerkingen over het beheren van aangepaste organisatie-eenheden
Nu dat u een aangepaste organisatie-eenheid hebt gemaakt, kunt u doorgaan en gebruikers, groepen, computers en serviceaccounts maken in deze organisatie-eenheid. U kunt gebruikers of groepen uit de 'AADDC gebruikers' organisatie-eenheid niet verplaatsen naar aangepaste organisatie-eenheden.

> [!WARNING]
> Gebruikersaccounts, groepen, service-accounts en computerobjecten die u op aangepaste OE's maakt zijn niet beschikbaar in uw Azure AD-tenant. Met andere woorden, weergeven deze objecten niet met behulp van de Azure AD Graph API of in de gebruikersinterface van Azure AD. Deze objecten zijn alleen beschikbaar in uw beheerde domein van Azure AD Domain Services.
>
>

## <a name="related-content"></a>Gerelateerde inhoud
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Groepsbeleid configureren op een beheerd domein](active-directory-ds-admin-guide-administer-group-policy.md)
* [Active Directory-beheercentrum: Aan de slag](https://technet.microsoft.com/library/dd560651.aspx)
* [Stapsgewijze handleiding voor Service-Accounts](https://technet.microsoft.com/library/dd548356.aspx)
