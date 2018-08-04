---
title: 'Azure Active Directory Domain Services: DNS beheren in beheerde domeinen | Microsoft Docs'
description: DNS beheren in Azure Active Directory Domain Services beheerde domeinen
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
ms.openlocfilehash: f20b2859f72087e208e8963fb18b297c7c670f4f
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504281"
---
# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>DNS beheren in een Azure AD Domain Services beheerde domein
Azure Active Directory Domain Services bevat een DNS (Domain Name Resolution)-server waarmee de DNS-omzetting voor het beheerde domein. Af en toe moet u mogelijk DNS configureren in het beheerde domein. Mogelijk moet u DNS-records voor machines die niet zijn gekoppeld aan het domein maken, configureren van virtuele IP-adressen voor load balancers of instellen van externe DNS-doorstuurservers. Om deze reden zijn gebruikers die deel uitmaken van de groep 'AAD DC Administrators' bevoegdheden verleend voor DNS-beheer in het beheerde domein.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Voor de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Een **domein virtuele machine** van waaruit u de Azure AD Domain Services beheerde domein beheren. Als u geen dergelijke een virtuele machine hebt, volgt u alle taken die worden beschreven in het artikel [Windows virtuele machine toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U moet de referenties van een **gebruikersaccount die behoren tot de groep 'AAD DC Administrators'** in uw directory, voor het beheren van DNS voor uw beheerde domein.

<br>

## <a name="task-1---create-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Taak 1: Maak een virtuele machine domein voor het extern beheren van DNS voor het beheerde domein
Azure AD Domain Services beheerde domeinen kunnen worden beheerd op afstand met vertrouwde Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum (ADAC) of AD PowerShell. Op deze manier kan DNS voor het beheerde domein extern worden beheerd met de beheerhulpprogramma's voor DNS-Server.

Beheerders in uw Azure AD-directory geen bevoegdheden voor verbinding met de domeincontrollers in het beheerde domein via Extern bureaublad. Leden van de groep 'AAD DC Administrators' kunnen DNS beheren voor beheerde domeinen op afstand met behulp van hulpprogramma's voor DNS-Server vanaf een Windows-Server/client-computer die lid van het beheerde domein is. DNS-Server-hulpprogramma's maken deel uit van de optionele functie van Remote Server Administration Tools (RSAT).

De eerste taak is het maken van een Windows Server virtuele machine die is gekoppeld aan het beheerde domein. Voor instructies raadpleegt u het artikel [een Windows Server-machine toevoegen aan Azure AD Domain Services beheerde domein](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Taak 2 - hulpprogramma's voor DNS-Server installeren op de virtuele machine
De volgende stappen voor het installeren van de DNS-beheerprogramma's op de virtuele machine van een domein. Voor meer informatie over [installeren en gebruiken van Remote Server Administration Tools](https://technet.microsoft.com/library/hh831501.aspx), Zie Technet.

1. Navigeer naar de Azure-portal. Klik op **alle resources** in het linkerdeelvenster. Zoek en klik op de virtuele machine die u in taak 1 hebt gemaakt.
2. Klik op de **Connect** knop op het tabblad Overzicht. Een Remote Desktop Protocol (RDP)-bestand gemaakt en gedownload.

    ![Verbinding maken met Windows virtuele machine](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Gebruik de referenties van een gebruiker die behoort tot de groep 'AAD DC Administrators'. Bijvoorbeeld 'bob@domainservicespreview.onmicrosoft.com'. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op Ja of doorgaan om verbinding te maken.

4. Openen vanuit het startscherm **Serverbeheer**. Klik op **functies en onderdelen toevoegen** in het centrale deelvenster van het venster Server Manager.

    ![Start de Server Manager op de virtuele machine](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Op de **voordat u begint** pagina van de **functies Wizard Functies toevoegen en**, klikt u op **volgende**.

    ![Voordat u begint met pagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Op de **installatietype** pagina, laat u de **op basis van functie of onderdeel gebaseerde installatie** optie is ingeschakeld en klik op **volgende**.

    ![Pagina Type installatie](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Op de **serverselectie** pagina, selecteert u de huidige virtuele machine uit de servergroep en klik op **volgende**.

    ![Pagina voor het selecteren van server](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Op de **serverfuncties** pagina, klikt u op **volgende**.
9. Op de **functies** pagina, klikt u op om uit te breiden de **Remote Server Administration Tools** knooppunt en klik vervolgens op om uit te breiden de **functiebeheer** knooppunt. Selecteer **beheerhulpprogramma's voor DNS-** functie uit de lijst met beheerprogramma's voor rollen.

    ![Pagina onderdelen](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)
10. Op de **bevestiging** pagina, klikt u op **installeren** voor het installeren van de functie van de hulpprogramma's voor DNS-Server op de virtuele machine. Wanneer de functie-installatie is voltooid, klikt u op **sluiten** om af te sluiten de **functies en onderdelen toevoegen** wizard.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)

## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Taak 3: Start de console DNS-beheer voor het beheren van DNS
U kunt nu, hulpprogramma's voor Windows Server DNS gebruiken voor het beheren van DNS in het beheerde domein.

> [!NOTE]
> U moet een lid van de groep 'AAD DC Administrators' voor het beheren van DNS in het beheerde domein.
>
>

1. Klik vanuit het startscherm op **Systeembeheer**. U ziet de **DNS** -console is geïnstalleerd op de virtuele machine.

    ![Beheerprogramma's - DNS-Console](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)
2. Klik op **DNS** om te starten van de DNS-beheerconsole.
3. In de **verbinding maken met de DNS-Server** dialoogvenster, klikt u op **de volgende computer**, en voert u de DNS-domeinnaam van het beheerde domein (bijvoorbeeld ' contoso100.com').

    ![DNS-Console - verbinding maken met domein](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)
4. De DNS-Console verbinding met het beheerde domein.

    ![De Console DNS - domein beheren](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)
5. U kunt nu de DNS-console gebruiken om toe te voegen DNS-vermeldingen voor computers binnen het virtuele netwerk waarin u AAD Domain Services hebt ingeschakeld.

> [!WARNING]
> Wees voorzichtig bij het beheren van DNS voor het beheerde domein met behulp van hulpprogramma's voor het beheer van DNS. Zorg ervoor dat u **niet verwijderen of wijzigen van de ingebouwde DNS-records die worden gebruikt door Domain Services in het domein**. Ingebouwde DNS-records bevatten domein DNS-records, naamserverrecords en andere records die worden gebruikt voor de locatie van de domeincontroller. Als u deze records wijzigt, worden de domeinservices onderbroken in het virtuele netwerk.
>
>

Zie voor meer informatie over het beheren van DNS, de [hulpprogramma's voor DNS-artikel op Technet](https://technet.microsoft.com/library/cc753579.aspx).

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een Windows Server-machine toevoegen aan Azure AD Domain Services beheerde domein](active-directory-ds-admin-guide-join-windows-vm.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Beheerhulpprogramma's voor DNS](https://technet.microsoft.com/library/cc753579.aspx)
