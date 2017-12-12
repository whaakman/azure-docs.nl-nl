---
title: 'Azure Active Directory Domain Services: Groepsbeleid op de beheerde domeinen beheren | Microsoft Docs'
description: Groepsbeleid beheren op Azure Active Directory Domain Services beheerde domeinen
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maheshu
ms.openlocfilehash: 316ddc2cbd67cfafaf44318c5baebcd8da366f93
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>Groepsbeleid voor een Azure AD Domain Services beheerd domein beheren
Azure Active Directory Domain Services omvat ingebouwde groepsbeleidsobjecten (GPO's) voor de containers 'AADDC gebruikers' en 'AADDC Computers'. U kunt deze ingebouwde groepsbeleidsobjecten om Groepsbeleid te configureren op het beheerde domein. Leden van de groep 'AAD DC Administrators' kunnen bovendien hun eigen aangepaste OE's maken in het beheerde domein. Ze kunnen ook aangepaste GPO's maken en deze koppelen aan deze aangepaste organisatie-eenheden. Gebruikers die deel uitmaken van de groep 'AAD DC-beheerders' krijgen beheerprivileges heeft Groepsbeleid op het beheerde domein.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Een **domein virtuele machine** van waaruit u het beheerde domein van Azure AD Domain Services beheren. Als u een dergelijke virtuele machine geen hebt, volgt u alle taken die worden beschreven in het artikel [een virtuele Windows-computer toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U moet de referenties van een **gebruikersaccount van de ' AAD DC' beheerdersgroep** in uw directory voor het beheren van Groepsbeleid voor uw beheerde domein.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-group-policy-for-the-managed-domain"></a>Taak 1: een domein-virtuele machine voor het extern beheren van Groepsbeleid voor het beheerde domein inrichten
Azure AD Domain Services beheerde domeinen kunnen worden beheerd op afstand met vertrouwd Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum (ADAC) of AD PowerShell. Groepsbeleid voor het beheerde domein kan ook worden beheerd op afstand met Groepsbeleid administration tools.

Beheerders in uw Azure AD-directory geen bevoegdheden voor verbinding met domeincontrollers op het beheerde domein via Extern bureaublad. Leden van de groep 'AAD DC Administrators' kunnen extern Groepsbeleid voor beheerde domeinen beheren. Hulpprogramma's voor Groepsbeleid kan worden gebruikt op een Windows-Server /-client-computer toegevoegd aan het beheerde domein. Groepsbeleid's kunnen worden geïnstalleerd als onderdeel van de Group Policy Management optionele functie in Windows Server en client-computers die zijn gekoppeld aan het beheerde domein.

De eerste taak is voor het inrichten van een virtuele machine van Windows Server die is toegevoegd aan het beheerde domein. Voor instructies raadpleegt u het artikel [virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-group-policy-tools-on-the-virtual-machine"></a>Taak 2 - hulpprogramma's voor Groepsbeleid voor installatie op de virtuele machine
Voer de volgende stappen voor het installeren van de Groepsbeleid Administration tools op de virtuele machine van verbonden met het domein.

1. Navigeer naar de Azure-portal. Klik op **alle resources** in het deelvenster links. Zoek en klik op de virtuele machine die u in taak 1 hebt gemaakt.
2. Klik op de **Connect** knop op het tabblad Overzicht. Een Remote Desktop Protocol (RDP)-bestand is gemaakt en gedownload.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Gebruik de referenties van een gebruiker van de ' AAD DC' beheerdersgroep bij de aanmeldingsprompt. Bijvoorbeeld, gebruiken we 'bob@domainservicespreview.onmicrosoft.com' in ons geval. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op Ja of doorgaan om door te gaan met de verbinding.
4. Open in het startscherm **Serverbeheer**. Klik op **functies en onderdelen toevoegen** in het centrale deelvenster van het venster Server Manager.

    ![Serverbeheer starten op de virtuele machine](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Op de **voordat u begint** pagina van de **Wizard Functies toevoegen en onderdelen**, klikt u op **volgende**.

    ![Voordat u begint met pagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Op de **installatietype** pagina, laat u de **op basis van functie of onderdeel gebaseerde installatie** optie ingeschakeld en klik op **volgende**.

    ![Pagina Type installatie](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Op de **serverselectie** pagina, selecteert u de huidige virtuele machine uit de servergroep en klik op **volgende**.

    ![Pagina voor serverselectie](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Op de **serverfuncties** pagina, klikt u op **volgende**. We overslaan deze pagina omdat er geen rollen niet op de server installeert.
9. Op de **functies** pagina de **Group Policy Management** functie.

    ![Pagina onderdelen](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. Op de **bevestiging** pagina, klikt u op **installeren** voor het installeren van het onderdeel Groepsbeleidsbeheer op de virtuele machine. Wanneer de functie-installatie is voltooid, klikt u op **sluiten** om af te sluiten de **functies en onderdelen toevoegen** wizard.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-the-group-policy-management-console-to-administer-group-policy"></a>Taak 3: Start de console Groepsbeleidsbeheer voor het beheren van Groepsbeleid
U kunt de console Groepsbeleidsbeheer op de virtuele domein machine gebruiken voor het beheren van Groepsbeleid voor het beheerde domein.

> [!NOTE]
> U moet lid zijn van de groep 'AAD DC-beheerders' voor het beheren van Groepsbeleid voor het beheerde domein.
>
>

1. Klik vanuit het startscherm op **Systeembeheer**. U ziet de **Group Policy Management** console is geïnstalleerd op de virtuele machine.

    ![Groepsbeleidsbeheer starten](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. Klik op **Group Policy Management** starten van de console Groepsbeleidsbeheer.

    ![Group Policy Console](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>Taak 4: groepsbeleidsobjecten ingebouwde aanpassen
Er zijn twee ingebouwde groepsbeleidsobjecten (GPO's) - één voor de 'AADDC Computers' en 'AADDC gebruikers' containers in uw beheerde domein. U kunt deze groepsbeleidsobjecten om Groepsbeleid te configureren op het beheerde domein.

1. In de **Group Policy Management** console, vouw de **Forest: contoso100.com** en **domeinen** knooppunten om te zien van het groepsbeleid voor uw beheerde domein.

    ![Ingebouwde GPO 's](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. U kunt deze ingebouwde GPO's voor Groepsbeleid configureren op uw beheerde domein. Met de rechtermuisknop op het groepsbeleidsobject en klik op **bewerken...**  voor het aanpassen van de ingebouwde GPO. U kunt met het hulpprogramma Configuratie van de Groepsbeleidsobjecteditor aanpassen van het groepsbeleidsobject.

    ![Ingebouwde GPO bewerken](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. U kunt nu de **Editor voor Groepsbeleidsbeheer** console om de ingebouwde groepsbeleidsobject te bewerken. De volgende schermafbeelding ziet bijvoorbeeld het aanpassen van het groepsbeleidsobject van de ingebouwde AADDC-Computers.

    ![Groepsbeleidsobject aanpassen](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>Taak 5: Maak een aangepaste Object GPO (Group Policy)
U kunt maken of importeren van uw eigen aangepaste groepsbeleidsobjecten. U kunt ook aangepaste groepsbeleidsobjecten koppelen aan een aangepaste OU die u hebt gemaakt in uw beheerde domein. Zie voor meer informatie over het maken van aangepaste organisatie-eenheden [een aangepaste organisatie-eenheid maken op een beheerd domein](active-directory-ds-admin-guide-create-ou.md).

> [!NOTE]
> U moet lid zijn van de groep 'AAD DC-beheerders' voor het beheren van Groepsbeleid voor het beheerde domein.
>
>

1. In de **Group Policy Management** -console, klikt u op uw aangepaste organisatie-eenheid (OE) selecteren. Met de rechtermuisknop op de organisatie-eenheid en klik op **een groepsbeleidsobject in dit domein maken en hier een koppeling...** .

    ![Een aangepaste groepsbeleidsobject maken](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Geef een naam voor het nieuwe groepsbeleidsobject en klik op **OK**.

    ![Geef een naam voor de GPO](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Een nieuw groepsbeleidsobject is gemaakt en gekoppeld aan uw aangepaste organisatie-eenheid. Met de rechtermuisknop op het groepsbeleidsobject en klik op **bewerken...**  in het menu.

    ![Een nieuw gemaakt groepsbeleidsobject](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. U kunt de zojuist gemaakte GPO met behulp van de **Editor voor Groepsbeleidsbeheer**.

    ![Nieuw groepsbeleidsobject aanpassen](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


Meer informatie over het gebruik van [Group Policy Management Console](https://technet.microsoft.com/library/cc753298.aspx) is beschikbaar op Technet.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Console Groepsbeleidsbeheer](https://technet.microsoft.com/library/cc753298.aspx)
