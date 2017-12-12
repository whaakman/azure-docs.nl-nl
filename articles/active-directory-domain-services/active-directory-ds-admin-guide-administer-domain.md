---
title: 'Azure Active Directory Domain Services: Een beheerd domein beheren | Microsoft Docs'
description: Azure Active Directory Domain Services beheerde domeinen beheren
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d4fdbc75-3e6b-4e20-8494-5dcc3bf2220a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2017
ms.author: maheshu
ms.openlocfilehash: 7ffe3e54b891fd74e14666c136be49132d78590b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Een beheerd domein van Azure AD Domain Services beheren
In dit artikel laat zien hoe een Azure Active Directory (AD) Domain Services beheerd domein beheren.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Een **domein virtuele machine** van waaruit u het beheerde domein van Azure AD Domain Services beheren. Als u een dergelijke virtuele machine geen hebt, volgt u alle taken die worden beschreven in het artikel [een virtuele Windows-computer toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U moet de referenties van een **gebruikersaccount van de ' AAD DC' beheerdersgroep** in uw directory voor het beheren van uw beheerde domein.

<br>

## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Administratieve taken die u op een beheerd domein uitvoeren kunt
Leden van de groep 'AAD DC Administrators' krijgen bevoegdheden op het beheerde domein waarmee deze taken uit te voeren, zoals:

* Computers toevoegen aan het beheerde domein.
* De ingebouwde GPO voor de containers 'AADDC Computers' en 'AADDC Users' in het beheerde domein configureren.
* DNS beheren in het beheerde domein.
* Maak en beheer van aangepaste organisatie-eenheden (OE's) voor het beheerde domein.
* Beheerderstoegang verkrijgen tot computers die aan het beheerde domein zijn gekoppeld.

## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>U niet op een beheerd domein hebt beheerdersbevoegdheden
Het domein wordt beheerd door Microsoft, met inbegrip van activiteiten zoals patchen, bewaking en, back-ups maken. Daarom wordt het domein is vergrendeld en u bent niet bevoegd bepaalde beheertaken uitvoeren op het domein. Er zijn enkele voorbeelden van taken die u niet kunt uitvoeren onder.

* U domein- of Ondernemingsadministrator-bevoegdheden voor het beheerde domein niet krijgen.
* U kunt het schema van het beheerde domein niet uitbreiden.
* U kan geen verbinding met de domeincontrollers voor het beheerde domein met extern bureaublad.
* U kunt domeincontrollers kan toevoegen aan het beheerde domein.

## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Taak 1: een domein Windows Server-machine voor het extern beheren van het beheerde domein inrichten
Azure AD Domain Services beheerde domeinen kunnen worden beheerd met vertrouwd Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum (ADAC) of AD PowerShell. Tenantbeheerders hoeft geen bevoegdheden voor verbinding met domeincontrollers op het beheerde domein via Extern bureaublad. Leden van de groep 'AAD DC Administrators' beheren daarom beheerde domeinen op afstand met AD-beheerprogramma's van een Windows-Server /-client-computer die is toegevoegd aan het beheerde domein. AD-beheerprogramma's kunnen worden geïnstalleerd als onderdeel van het optionele onderdeel van Remote Server Administration Tools (RSAT) voor Windows Server en client-computers die zijn gekoppeld aan het beheerde domein.

De eerste stap is het instellen van een virtuele machine van Windows Server die is toegevoegd aan het beheerde domein. Voor instructies raadpleegt u het artikel [virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Het beheerde domein op afstand te beheren vanaf een clientcomputer (bijvoorbeeld Windows 10)
De instructies in dit artikel gebruik een virtuele machine van Windows Server voor het beheren van de AAD-DS beheerd domein. U kunt echter ook een virtuele machine van Windows-client (bijvoorbeeld Windows 10) gebruiken om dit te doen.

U kunt [Remote Server Administration Tools (RSAT) installeren](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) op een Windows-client virtuele machine door de instructies op TechNet.

## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Taak 2 - Install Active Directory-beheerprogramma's op de virtuele machine
Voer de volgende stappen voor het installeren van de Active Directory-beheerprogramma's op de virtuele machine van verbonden met het domein. Zie Technet voor meer [informatie over het installeren en gebruiken van Remote Server Administration Tools](https://technet.microsoft.com/library/hh831501.aspx).

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
9. Op de **functies** pagina, vouw de **Remote Server Administration Tools** knooppunt en klik vervolgens op uit te breiden de **functiebeheer** knooppunt. Selecteer **AD DS en AD LDS-hulpprogramma** functie uit de lijst met hulpprogramma's voor functiebeheer.

    ![Pagina onderdelen](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)
10. Op de **bevestiging** pagina, klikt u op **installeren** installeren de advertentie en functie van AD LDS-hulpprogramma's op de virtuele machine. Wanneer de functie-installatie is voltooid, klikt u op **sluiten** om af te sluiten de **functies en onderdelen toevoegen** wizard.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)

## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Taak 3: verbinding maken met en het beheerde domein verkennen
Nu de AD-beheerprogramma's zijn geïnstalleerd op de virtuele machine van het domein, kunnen we deze hulpprogramma's gebruiken om te verkennen en beheren van het beheerde domein.

> [!NOTE]
> U moet lid zijn van de groep 'AAD DC-beheerders' voor het beheer van het beheerde domein.
>
>

1. Klik vanuit het startscherm op **Systeembeheer**. Hier ziet u de AD-beheerprogramma's geïnstalleerd op de virtuele machine.

    ![Beheerprogramma's geïnstalleerd op server](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Klik op **Active Directory-beheercentrum**.

    ![Active Directory-beheercentrum](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Als u wilt verkennen van het domein, klikt u op de naam van het domein in het linkerdeelvenster (bijvoorbeeld, contoso100.com'). U ziet twee containers respectievelijk 'AADDC Computers' en 'AADDC gebruikers' genoemd.

    ![Active Directory-Beheercentrum - domein weergeven](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)
4. Klik op de container aangeroepen **AADDC gebruikers** om te zien alle gebruikers en groepen die tot het beheerde domein behoren. Ziet u gebruikersaccounts en groepen in uw Azure AD tenant weergeven om in deze container. In dit voorbeeld ziet, een gebruikersaccount voor de gebruiker met de naam "bob" en een groep genaamd 'AAD DC-beheerders' zijn beschikbaar in deze container.

    ![Active Directory-Beheercentrum - domeingebruikers](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)
5. Klik op de container aangeroepen **AADDC Computers** om te zien van de computers die zijn gekoppeld aan dit beheerde domein. Hier ziet u een vermelding voor de huidige virtuele machine, die is gekoppeld aan het domein. De computeraccounts voor alle computers die lid zijn van het beheerde domein van Azure AD Domain Services worden opgeslagen in deze container AADDC-Computers.

    ![Active Directory-Beheercentrum - computers die lid van domein](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Remote Server Administration Tools implementeren](https://technet.microsoft.com/library/hh831501.aspx)
