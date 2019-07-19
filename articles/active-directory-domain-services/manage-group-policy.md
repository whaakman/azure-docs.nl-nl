---
title: 'Azure Active Directory Domain Services: groepsbeleid beheren | Microsoft Docs'
description: groepsbeleid op Azure Active Directory Domain Services beheerde domeinen beheren
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: iainfou
ms.openlocfilehash: c7b32885fdb3cf4f3e584c916d6b234fff54bfc4
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234032"
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>groepsbeleid op een Azure AD Domain Services beheerd domein beheren
Azure Active Directory Domain Services bevat ingebouwde groepsbeleid objecten (Gpo's) voor de containers ' AADDC users ' en ' AADDC computers '. U kunt deze ingebouwde Gpo's aanpassen voor het configureren van groepsbeleid op het beheerde domein. Daarnaast kunnen leden van de groep AAD DC-Administrators hun eigen aangepaste organisatie-eenheden maken in het beheerde domein. Ze kunnen ook aangepaste groeps beleidsobjecten maken en deze koppelen aan deze aangepaste organisatie-eenheden. Gebruikers die deel uitmaken van de groep ' AAD DC Administrators ' worden groepsbeleid beheerders rechten verleend op het beheerde domein.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Voor het uitvoeren van de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldig **Azure-abonnement**.
2. Een **Azure AD-Directory** : gesynchroniseerd met een on-premises Directory of een alleen-Cloud Directory.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-adres lijst. Als u dit nog niet hebt gedaan, volgt u alle taken die in de aan de slag- [hand leiding](create-instance.md)worden beschreven.
4. Een **virtuele machine die lid** is van een domein van waaruit u het Azure AD Domain Services beheerde domein beheert. Als u een dergelijke virtuele machine niet hebt, volgt u alle taken die worden beschreven in het artikel [een virtuele Windows-machine toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U hebt de referenties nodig van een **gebruikers account dat deel uitmaakt van de groep Aad DC** -Administrators in uw directory om Groepsbeleid voor uw beheerde domein te beheren.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-group-policy-for-the-managed-domain"></a>Taak 1: een virtuele machine die lid is van een domein inrichten voor het extern beheren van groepsbeleid voor het beheerde domein
Azure AD Domain Services beheerde domeinen kunnen extern worden beheerd met vertrouwde Active Directory beheer Programma's, zoals de Active Directory-beheercentrum (ADAC) of AD Power shell. Op dezelfde manier kan groepsbeleid voor het beheerde domein extern worden beheerd met behulp van de groepsbeleid-beheer hulpprogramma's.

Beheerders in uw Azure AD-adres lijst zijn niet gemachtigd om via Extern bureaublad verbinding te maken met domein controllers in het beheerde domein. Leden van de groep AAD DC Administrators kunnen groepsbeleid voor beheerde domeinen op afstand beheren. Ze kunnen gebruikmaken van groepsbeleid-hulpprogram ma's op een Windows Server/client-computer die is toegevoegd aan het beheerde domein. Groepsbeleid-hulpprogram ma's kunnen worden geïnstalleerd als onderdeel van de optionele functie voor groepsbeleid beheer op Windows Server-en client computers die zijn gekoppeld aan het beheerde domein.

De eerste taak is het inrichten van een virtuele Windows Server-machine die is gekoppeld aan het beheerde domein. Raadpleeg het artikel ' [een virtuele Windows Server-machine toevoegen aan een Azure AD Domain Services beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md)' voor instructies.

## <a name="task-2---install-group-policy-tools-on-the-virtual-machine"></a>Taak 2: groepsbeleid-hulpprogram ma's installeren op de virtuele machine
Voer de volgende stappen uit om de groepsbeleid-beheer hulpprogramma's te installeren op de virtuele machine die lid is van het domein.

1. Navigeer naar het Azure Portal. Klik op **alle resources** in het linkerdeel venster. Zoek en klik op de virtuele machine die u hebt gemaakt in taak 1.
2. Klik op de knop **verbinding maken** op het tabblad Overzicht. Er wordt een Remote Desktop Protocol-bestand (. RDP) gemaakt en gedownload.

    ![Verbinding maken met virtuele Windows-machine](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Gebruik bij de aanmeldings prompt de referenties van een gebruiker die deel uitmaakt van de groep AAD DC-Administrators. We gebruikenbob@domainservicespreview.onmicrosoft.combijvoorbeeld in ons geval. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op Ja of door gaan om door te gaan met de verbinding.
4. Open **Serverbeheer**in het Start scherm. Klik op **functies en onderdelen toevoegen** in het middelste deel venster van het venster Serverbeheer.

    ![Serverbeheer op virtuele machine starten](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Klik op de pagina **voordat u begint** van de **wizard functies en onderdelen toevoegen**op **volgende**.

    ![Voordat u begint met de pagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Zorg ervoor dat op de pagina **type installatie** de optie installatie die op de **functie of het onderdeel is gebaseerd** is ingeschakeld en klik op **volgende**.

    ![Pagina type installatie](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Selecteer op de pagina **selectie van servers** de huidige virtuele machine uit de Server groep en klik op **volgende**.

    ![Pagina selectie van servers](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Klik op de pagina **Server functies** op **volgende**. Deze pagina wordt overgeslagen omdat er geen rollen op de server worden geïnstalleerd.
9. Selecteer op de pagina **functies** de **Groepsbeleid beheer** functie.

    ![Pagina functies](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. Klik op de pagina **bevestiging** op **installeren** om de functie Groepsbeleid beheer op de virtuele machine te installeren. Wanneer de installatie van de functie is voltooid, klikt u op **sluiten** om de wizard **functies en onderdelen toevoegen** af te sluiten.

    ![Bevestigings pagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-the-group-policy-management-console-to-administer-group-policy"></a>Taak 3: de groepsbeleid-beheer console starten om groepsbeleid te beheren
U kunt de groepsbeleid-beheer console gebruiken op de virtuele machine die lid is van het domein om groepsbeleid te beheren op het beheerde domein.

> [!NOTE]
> U moet lid zijn van de groep AAD DC Administrators om groepsbeleid te beheren op het beheerde domein.
>
>

1. Klik in het Start scherm op **systeem beheer**. U ziet dat de **Groepsbeleid-beheer** console op de virtuele machine is geïnstalleerd.

    ![groepsbeleid beheer starten](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. Klik op **Groepsbeleid beheer** om de Groepsbeleid-beheer console te starten.

    ![groepsbeleid-console](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>Taak 4-ingebouwde groepsbeleid objecten aanpassen
Er zijn twee ingebouwde groepsbeleid objecten (Gpo's): één voor de containers AADDC computers en AADDC gebruikers in uw beheerde domein. U kunt deze groeps beleidsobjecten aanpassen voor het configureren van groeps beleid op het beheerde domein.

1. Klik in de **Groepsbeleid-beheer** console op het knoop punt **forest: contoso100.com** en **domeinen** om de groeps beleidsregels voor uw beheerde domein weer te geven.

    ![Ingebouwde Gpo's](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. U kunt deze ingebouwde Gpo's aanpassen voor het configureren van groeps beleid op uw beheerde domein. Klik met de rechter muisknop op het groeps beleidsobject en klik op **bewerken...** om het ingebouwde groeps beleidsobject aan te passen. Met het hulp programma groepsbeleid Configuration Editor kunt u het groeps beleidsobject aanpassen.

    ![Ingebouwd groeps beleidsobject bewerken](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. U kunt nu de **Groepsbeleidsbeheer-editor** -console gebruiken om het ingebouwde groeps beleidsobject te bewerken. De volgende scherm afbeelding laat bijvoorbeeld zien hoe u het ingebouwde groeps beleidsobject ' AADDC-computers ' aanpast.

    ![Groeps beleidsobject aanpassen](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>Taak 5: een aangepast groepsbeleid-object maken (GPO)
U kunt uw eigen aangepaste groeps beleidsobjecten maken of importeren. U kunt ook aangepaste groeps beleidsobjecten koppelen aan een aangepaste organisatie-eenheid die u hebt gemaakt in uw beheerde domein. Zie [een aangepaste organisatie-eenheid maken in een beheerd domein](create-ou.md)voor meer informatie over het maken van aangepaste organisatie-eenheden.

> [!NOTE]
> U moet lid zijn van de groep AAD DC Administrators om groepsbeleid te beheren op het beheerde domein.
>
>

1. Klik in de **Groepsbeleid-beheer** console om uw aangepaste organisatie-eenheid (OE) te selecteren. Klik met de rechter muisknop op de organisatie-eenheid en klik op **groeps beleidsobject in dit domein maken en hier een koppeling...** .

    ![Een aangepast groeps beleidsobject maken](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Geef een naam op voor het nieuwe groeps beleidsobject en klik op **OK**.

    ![Geef een naam op voor het groeps beleidsobject](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Er wordt een nieuw groeps beleidsobject gemaakt en gekoppeld aan uw aangepaste organisatie-eenheid. Klik met de rechter muisknop op het groeps beleidsobject en klik op **bewerken...** in het menu.

    ![Nieuw groeps beleidsobject](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. U kunt het nieuw gemaakte groeps beleidsobject aanpassen met behulp van de **Groepsbeleidsbeheer-editor**.

    ![Nieuw groeps beleidsobject aanpassen](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


Meer informatie over het gebruik van [console Groepsbeleidbeheer](https://technet.microsoft.com/library/cc753298.aspx) is beschikbaar op TechNet.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services aan de slag-hand leiding](create-instance.md)
* [Een virtuele machine met Windows Server toevoegen aan een door Azure AD Domain Services beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md)
* [Een Azure AD Domain Services domein beheren](manage-domain.md)
* [console Groepsbeleidbeheer](https://technet.microsoft.com/library/cc753298.aspx)
