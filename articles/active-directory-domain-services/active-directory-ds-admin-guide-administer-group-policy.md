---
title: 'Azure Active Directory Domain Services: Beheer van Groepsbeleid op de beheerde domeinen | Microsoft Docs'
description: Groepsbeleid beheren in Azure Active Directory Domain Services beheerde domeinen
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: acdba45bef5407af4b96d8e5f805a828e10d2d61
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502213"
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>Groepsbeleid in een Azure AD Domain Services beheerde domein beheren
Azure Active Directory Domain Services bevat ingebouwde groepsbeleidsobjecten (GPO's) voor de 'AADDC gebruikers' en 'AADDC Computers' containers. U kunt deze ingebouwde groepsbeleidsobjecten om Groepsbeleid te configureren in het beheerde domein. Leden van de groep 'AAD DC Administrators' kunnen bovendien hun eigen aangepaste OE's maken in het beheerde domein. Ze kunnen ook aangepaste GPO's maken en deze koppelen aan deze aangepaste organisatie-eenheden. Group Policy administration bevoegdheden in het beheerde domein worden verleend voor gebruikers die deel uitmaken van de groep 'AAD DC Administrators'.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Een **domein virtuele machine** van waaruit u de Azure AD Domain Services beheerde domein beheren. Als u geen dergelijke een virtuele machine hebt, volgt u alle taken die worden beschreven in het artikel [Windows virtuele machine toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U moet de referenties van een **gebruikersaccount die behoren tot de groep 'AAD DC Administrators'** in uw directory, voor het beheren van Groepsbeleid voor uw beheerde domein.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-group-policy-for-the-managed-domain"></a>Taak 1: inrichten van een domein virtuele machine voor het extern beheren van Groepsbeleid voor het beheerde domein
Azure AD Domain Services beheerde domeinen kunnen worden beheerd op afstand met vertrouwde Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum (ADAC) of AD PowerShell. Groepsbeleid voor het beheerde domein kan op dezelfde manier worden beheerd op afstand met behulp van de beheerhulpprogramma's voor Groepsbeleid.

Beheerders in uw Azure AD-directory geen bevoegdheden voor verbinding met de domeincontrollers in het beheerde domein via Extern bureaublad. Leden van de groep 'AAD DC Administrators' kunnen op afstand Groepsbeleid voor de beheerde domeinen beheren. Ze kunnen hulpprogramma's voor Groepsbeleid gebruiken op een Windows-Server/client-computer toegevoegd aan het beheerde domein. Groepsbeleid beheerhulpprogramma's kunnen worden geïnstalleerd als onderdeel van de Group Policy Management optionele functie in Windows Server en client-computers die zijn toegevoegd aan het beheerde domein.

De eerste taak is het inrichten van een Windows Server virtuele machine die is gekoppeld aan het beheerde domein. Voor instructies raadpleegt u het artikel [een Windows Server-machine toevoegen aan Azure AD Domain Services beheerde domein](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-group-policy-tools-on-the-virtual-machine"></a>Taak 2 - hulpprogramma's voor installatie van Groepsbeleid op de virtuele machine
Voer de volgende stappen uit voor het installeren van de Group Policy Administration tools op de virtuele machine van een domein.

1. Navigeer naar de Azure-portal. Klik op **alle resources** in het linkerdeelvenster. Zoek en klik op de virtuele machine die u in taak 1 hebt gemaakt.
2. Klik op de **Connect** knop op het tabblad Overzicht. Een Remote Desktop Protocol (RDP)-bestand gemaakt en gedownload.

    ![Verbinding maken met Windows virtuele machine](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Aan de aanmeldingsprompt, gebruikt u de referenties van een gebruiker die behoort tot de groep 'AAD DC Administrators'. Bijvoorbeeld, gebruiken we 'bob@domainservicespreview.onmicrosoft.com' in ons geval. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op Ja of doorgaan om door te gaan met de verbinding.
4. Openen vanuit het startscherm **Serverbeheer**. Klik op **functies en onderdelen toevoegen** in het centrale deelvenster van het venster Server Manager.

    ![Start de Server Manager op de virtuele machine](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Op de **voordat u begint** pagina van de **functies Wizard Functies toevoegen en**, klikt u op **volgende**.

    ![Voordat u begint met pagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Op de **installatietype** pagina, laat u de **op basis van functie of onderdeel gebaseerde installatie** optie is ingeschakeld en klik op **volgende**.

    ![Pagina Type installatie](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Op de **serverselectie** pagina, selecteert u de huidige virtuele machine uit de servergroep en klik op **volgende**.

    ![Pagina voor het selecteren van server](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Op de **serverfuncties** pagina, klikt u op **volgende**. We slaat u deze pagina omdat we niet alle functies op de server wilt installeren.
9. Op de **functies** weergeeft, schakelt de **Group Policy Management** functie.

    ![Pagina onderdelen](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. Op de **bevestiging** pagina, klikt u op **installeren** de Group Policy Management-functie installeren op de virtuele machine. Wanneer de functie-installatie is voltooid, klikt u op **sluiten** om af te sluiten de **functies en onderdelen toevoegen** wizard.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-the-group-policy-management-console-to-administer-group-policy"></a>Taak 3: Start de Group Policy management console voor het beheren van Groepsbeleid
U kunt de Group Policy management console op de virtuele domein machine gebruiken voor het beheren van Groepsbeleid voor het beheerde domein.

> [!NOTE]
> U moet een lid van de groep 'AAD DC Administrators' voor het beheren van Groepsbeleid voor het beheerde domein.
>
>

1. Klik vanuit het startscherm op **Systeembeheer**. U ziet de **Group Policy Management** -console is geïnstalleerd op de virtuele machine.

    ![Groepsbeleidsbeheer starten](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. Klik op **Group Policy Management** om te starten van de console Groepsbeleidsbeheer.

    ![Group Policy Console](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>Taak 4: ingebouwde Group Policy Objects aanpassen
Er zijn twee ingebouwde groepsbeleidsobjecten (GPO's): één voor de 'AADDC Computers' en 'AADDC gebruikers' containers in uw beheerde domein. U kunt deze groepsbeleidsobjecten voor het configureren van instellingen voor Groepsbeleid in het beheerde domein.

1. In de **Group Policy Management** console, vouw de **Forest: contoso100.com** en **domeinen** knooppunten om te zien van het groepsbeleid voor uw beheerde domein.

    ![Ingebouwde GPO 's](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. U kunt deze ingebouwde groepsbeleidsobjecten voor het configureren van Groepsbeleid op uw beheerde domein. Met de rechtermuisknop op het groepsbeleidsobject en klik op **bewerken...**  om aan te passen van de ingebouwde GPO. Het hulpprogramma Configuratie van de Groepsbeleidsobjecteditor kunt u het aanpassen van het groepsbeleidsobject.

    ![Ingebouwde groepsbeleidsobject bewerken](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. U kunt nu de **Editor voor Groepsbeleidsbeheer** console om de ingebouwde groepsbeleidsobject te bewerken. De volgende schermafbeelding ziet u bijvoorbeeld over het aanpassen van de ingebouwde 'AADDC Computers'-GPO.

    ![Groepsbeleidsobject aanpassen](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>Taak 5: een aangepaste groepsbeleidsobject (GPO) maken
U kunt maken of importeren van uw eigen aangepaste groepsbeleidsobjecten. U kunt ook aangepaste groepsbeleidsobjecten koppelen aan een aangepaste OE die u hebt gemaakt in uw beheerde domein. Zie voor meer informatie over het maken van aangepaste organisatie-eenheden [een aangepaste organisatie-eenheid maken in een beheerd domein](active-directory-ds-admin-guide-create-ou.md).

> [!NOTE]
> U moet een lid van de groep 'AAD DC Administrators' voor het beheren van Groepsbeleid voor het beheerde domein.
>
>

1. In de **Group Policy Management** console, klikt u op uw aangepaste organisatie-eenheid (OE) selecteren. Met de rechtermuisknop op de organisatie-eenheid en klik op **een groepsbeleidsobject in dit domein maken en hier een koppeling...** .

    ![Een aangepaste groepsbeleidsobject maken](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Geef een naam voor het nieuwe groepsbeleidsobject en klik op **OK**.

    ![Geef een naam op voor het groepsbeleidsobject](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Een nieuw GPO wordt gemaakt en gekoppeld aan uw aangepaste organisatie-eenheid. Met de rechtermuisknop op het groepsbeleidsobject en klik op **bewerken...**  in het menu.

    ![Een nieuw gemaakt groepsbeleidsobject](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. U kunt de zojuist gemaakte GPO met behulp van de **Editor voor Groepsbeleidsbeheer**.

    ![Nieuw groepsbeleidsobject aanpassen](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


Meer informatie over het gebruik van [Group Policy Management Console](https://technet.microsoft.com/library/cc753298.aspx) is beschikbaar op Technet.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een Windows Server-machine toevoegen aan Azure AD Domain Services beheerde domein](active-directory-ds-admin-guide-join-windows-vm.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Console Groepsbeleidsbeheer](https://technet.microsoft.com/library/cc753298.aspx)
