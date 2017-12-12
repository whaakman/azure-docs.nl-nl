---
title: 'Azure Active Directory Domain Services: DNS op de beheerde domeinen beheren | Microsoft Docs'
description: DNS op Azure Active Directory Domain Services beheerde domeinen beheren
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
ms.date: 09/23/2017
ms.author: maheshu
ms.openlocfilehash: 55b6368d55b5d0ad50d066a4963e74d8c44a2049
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>DNS op een beheerd domein van Azure AD Domain Services beheren
Azure Active Directory Domain Services bevat een DNS (Domain Name Resolution)-server met DNS-omzetting voor het beheerde domein. In sommige gevallen moet u wellicht DNS configureren op het beheerde domein. Mogelijk moet u DNS-records maken voor computers die niet zijn gekoppeld aan het domein, virtuele IP-adressen voor load balancers te configureren of instellen van externe DNS-doorstuurservers. Om deze reden krijgen gebruikers die deel uitmaken van de groep 'AAD DC-beheerders' DNS-beheer-machtigingen op het beheerde domein.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Een **domein virtuele machine** van waaruit u het beheerde domein van Azure AD Domain Services beheren. Als u een dergelijke virtuele machine geen hebt, volgt u alle taken die worden beschreven in het artikel [een virtuele Windows-computer toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U moet de referenties van een **gebruikersaccount van de ' AAD DC' beheerdersgroep** in uw directory voor het beheer van DNS voor uw beheerde domein.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Taak 1: een domein-virtuele machine voor het extern beheren van DNS voor het beheerde domein inrichten
Azure AD Domain Services beheerde domeinen kunnen worden beheerd op afstand met vertrouwd Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum (ADAC) of AD PowerShell. DNS voor het beheerde domein kan ook worden beheerd op afstand met DNS-Server administration tools.

Beheerders in uw Azure AD-directory geen bevoegdheden voor verbinding met domeincontrollers op het beheerde domein via Extern bureaublad. Leden van de groep 'AAD DC Administrators' beheren DNS voor de beheerde domeinen extern met hulpprogramma's voor DNS-Server vanaf een Windows-Server /-client-computer die is gekoppeld aan het beheerde domein. Hulpprogramma's voor DNS-Server kunnen worden geïnstalleerd als onderdeel van de optionele functie van Remote Server Administration Tools (RSAT) voor Windows Server en client-computers die zijn gekoppeld aan het beheerde domein.

De eerste taak is voor het inrichten van een virtuele machine van Windows Server die is toegevoegd aan het beheerde domein. Voor instructies raadpleegt u het artikel [virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Taak 2 - hulpprogramma's voor DNS-Server installeren op de virtuele machine
Voer de volgende stappen voor het installeren van de DNS-beheerprogramma's op de virtuele machine van verbonden met het domein. Voor meer informatie over [installeren en gebruiken van Remote Server Administration Tools](https://technet.microsoft.com/library/hh831501.aspx), Zie Technet.

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
9. Op de **functies** pagina, vouw de **Remote Server Administration Tools** knooppunt en klik vervolgens op uit te breiden de **functiebeheer** knooppunt. Selecteer **DNS-Server-hulpprogramma's** functie uit de lijst met hulpprogramma's voor functiebeheer.

    ![Pagina onderdelen](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)
10. Op de **bevestiging** pagina, klikt u op **installeren** het onderdeel van de hulpprogramma's voor DNS-Server installeren op de virtuele machine. Wanneer de functie-installatie is voltooid, klikt u op **sluiten** om af te sluiten de **functies en onderdelen toevoegen** wizard.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)

## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Taak 3: Start de console DNS-beheer voor het beheren van DNS
Nu dat de functie hulpprogramma's voor DNS-servers is geïnstalleerd op de virtuele machine van het domein, wordt de DNS-hulpprogramma's kunt gebruiken voor het beheer van DNS op het beheerde domein.

> [!NOTE]
> U moet lid zijn van de groep 'AAD DC-beheerders' voor het beheer van DNS op het beheerde domein.
>
>

1. Klik vanuit het startscherm op **Systeembeheer**. U ziet de **DNS** console is geïnstalleerd op de virtuele machine.

    ![Systeembeheer - DNS-Console](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)
2. Klik op **DNS** starten van de DNS-beheerconsole.
3. In de **verbinding maken met de DNS-Server** dialoogvenster, klikt u op de optie **de volgende computer**, en voert u de DNS-domeinnaam van het beheerde domein (bijvoorbeeld, contoso100.com').

    ![DNS-Console - verbinding maken met domein](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)
4. De DNS-Console verbindt met het beheerde domein.

    ![De Console DNS - domein beheren](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)
5. U kunt nu de DNS-console gebruiken om toe te voegen DNS-vermeldingen voor computers in het virtuele netwerk waarin u AAD Domain Services hebt ingeschakeld.

> [!WARNING]
> Wees voorzichtig met het beheren van DNS voor het beheerde domein met de DNS-beheerprogramma's. Zorg ervoor dat u **niet verwijderen of wijzigen van de ingebouwde DNS-records die worden gebruikt door Domain Services in het domein**. Ingebouwde DNS-records zijn domein-DNS-records, naamserverrecords en andere records gebruikt voor de locatie van de domeincontroller. Als u deze records wijzigt, worden de domeinservices verstoord op het virtuele netwerk.
>
>

Zie de [DNS-hulpprogramma's op Technet artikel](https://technet.microsoft.com/library/cc753579.aspx) voor meer informatie over het beheren van DNS.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [DNS-beheerprogramma 's](https://technet.microsoft.com/library/cc753579.aspx)
