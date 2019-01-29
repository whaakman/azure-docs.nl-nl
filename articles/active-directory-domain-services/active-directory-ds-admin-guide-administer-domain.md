---
title: 'Azure Active Directory Domain Services: Een beheerd domein beheren | Microsoft Docs'
description: Azure Active Directory Domain Services beheerde domeinen beheren
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: d4fdbc75-3e6b-4e20-8494-5dcc3bf2220a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: f1e3272bd8f0d353a7bf817d7f79d2d596277caa
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173810"
---
# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Een beheerd domein van Azure AD Domain Services beheren
Dit artikel ziet u hoe u een beheerd domein van Azure Active Directory (AD) Domain Services beheren.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Voor de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Een **domein virtuele machine** van waaruit u de Azure AD Domain Services beheerde domein beheren. Als u geen dergelijke een virtuele machine hebt, volgt u alle taken die worden beschreven in het artikel [Windows virtuele machine toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).
5. U moet de referenties van een **gebruikersaccount die behoren tot de groep 'AAD DC Administrators'** in uw directory, voor het beheren van uw beheerde domein.

<br>

## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Administratieve taken die u op een beheerd domein uitvoeren kunt
Leden van de groep 'AAD DC Administrators' worden rechten in het beheerde domein waarmee deze taken uit te voeren, zoals:

* Computers toevoegen aan het beheerde domein.
* De ingebouwde GPO voor de containers 'AADDC Computers' en 'AADDC Users' in het beheerde domein configureren.
* DNS beheren in het beheerde domein.
* Maken en beheren van aangepaste organisatie-eenheden (OU's) in het beheerde domein.
* Beheerderstoegang verkrijgen tot computers die aan het beheerde domein zijn gekoppeld.

## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Beheerdersbevoegdheden die u niet op een beheerd domein hebt
Het domein wordt beheerd door Microsoft, met inbegrip van activiteiten, zoals patching, bewaking en, waarbij back-ups. Het domein is vergrendeld en er geen bevoegdheden voor het uitvoeren van bepaalde administratieve taken op het domein. Enkele voorbeelden van taken die u kunt geen staan hieronder.

* U hebt geen domeinbeheerder of Ondernemingsadministrator-bevoegdheden in voor het beheerde domein.
* U kunt het schema van het beheerde domein niet uitbreiden.
* U kan geen verbinding maken met domeincontrollers voor het beheerde domein met extern bureaublad.
* U kunt domeincontrollers kan toevoegen aan het beheerde domein.

## <a name="task-1---create-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Taak 1: het maken van een domein Windows Server-machine voor het extern beheren van het beheerde domein
Azure AD Domain Services beheerde domeinen kunnen worden beheerd met vertrouwde Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum (ADAC) of AD PowerShell. Tenantbeheerders geen bevoegdheden voor verbinding met de domeincontrollers in het beheerde domein via Extern bureaublad. Leden van de groep 'AAD DC Administrators' kunnen beheren. beheerde domeinen op afstand met behulp van beheerprogramma's van de AD vanaf een Windows-Server/client-computer die lid van het beheerde domein is. AD-beheerprogramma's kunnen worden geïnstalleerd als onderdeel van de optionele functie van Remote Server Administration Tools (RSAT) voor Windows Server en client-computers die zijn toegevoegd aan het beheerde domein.

De eerste stap is het instellen van een Windows Server virtuele machine die is gekoppeld aan het beheerde domein. Voor instructies raadpleegt u het artikel [een Windows Server-machine toevoegen aan Azure AD Domain Services beheerde domein](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Het beheerde domein op afstand te beheren vanaf een clientcomputer (bijvoorbeeld Windows 10)
De instructies in dit artikel een Windows Server-machine voor het beheren van de AAD-DS beheerde domein. U kunt echter ook een virtuele machine voor Windows-client (bijvoorbeeld Windows 10) gebruiken om dit te doen.

U kunt [Remote Server Administration Tools (RSAT) installeren](https://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) op een Windows-client virtuele machine door de instructies te volgen op TechNet.

## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Taak 2: Install Active Directory-beheerprogramma's op de virtuele machine
De volgende stappen voor het installeren van de Active Directory-beheerprogramma's op de virtuele machine van een domein. Technet voor meer informatie Zie [informatie over het installeren en gebruiken van Remote Server Administration Tools](https://technet.microsoft.com/library/hh831501.aspx).

1. Navigeer naar de Azure-portal. Klik op **alle resources** in het linkerdeelvenster. Zoek en klik op de virtuele machine die u in taak 1 hebt gemaakt.
2. Klik op de **Connect** knop op het tabblad Overzicht. Een Remote Desktop Protocol (RDP)-bestand gemaakt en gedownload.

    ![Verbinding maken met Windows virtuele machine](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Gebruik de referenties van een gebruiker die behoort tot de groep 'AAD DC Administrators'. Bijvoorbeeld 'bob@domainservicespreview.onmicrosoft.com'. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op Ja of doorgaan om door te gaan met de verbinding.
4. Openen vanuit het startscherm **Serverbeheer**. Klik op **functies en onderdelen toevoegen** in het centrale deelvenster van het venster Server Manager.

    ![Start de Server Manager op de virtuele machine](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Op de **voordat u begint** pagina van de **functies Wizard Functies toevoegen en**, klikt u op **volgende**.

    ![Voordat u begint met pagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Op de **installatietype** pagina, laat u de **op basis van functie of onderdeel gebaseerde installatie** optie is ingeschakeld en klik op **volgende**.

    ![Pagina Type installatie](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Op de **serverselectie** pagina, selecteert u de huidige virtuele machine uit de servergroep en klik op **volgende**.

    ![Pagina voor het selecteren van server](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Op de **serverfuncties** pagina, klikt u op **volgende**.
9. Op de **functies** pagina, klikt u op om uit te breiden de **Remote Server Administration Tools** knooppunt en klik vervolgens op om uit te breiden de **functiebeheer** knooppunt. Selecteer **AD DS en AD LDS-hulpprogramma's** functie uit de lijst met beheerprogramma's voor rollen.

    ![Pagina onderdelen](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)
10. Op de **bevestiging** pagina, klikt u op **installeren** voor het installeren de advertentie en functie van AD LDS-hulpprogramma's op de virtuele machine. Wanneer de functie-installatie is voltooid, klikt u op **sluiten** om af te sluiten de **functies en onderdelen toevoegen** wizard.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)

## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Taak 3: verbinding maken met en Verken het beheerde domein
U kunt nu, Windows Server AD-beheerprogramma's gebruiken om te verkennen en beheren van het beheerde domein.

> [!NOTE]
> U moet een lid van de groep 'AAD DC Administrators' voor het beheer van het beheerde domein.
>
>

1. Klik vanuit het startscherm op **Systeembeheer**. Hier ziet u de AD beheerprogramma's geïnstalleerd op de virtuele machine.

    ![Beheerprogramma's geïnstalleerd op server](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Klik op **Active Directory-beheercentrum**.

    ![Active Directory-beheercentrum](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Als u wilt verkennen van het domein, klikt u op de naam van het domein in het linkerdeelvenster (bijvoorbeeld ' contoso100.com'). U ziet twee containers respectievelijk 'AADDC Computers' en 'AADDC gebruikers' genoemd.

    ![Active Directory-Beheercentrum - domein weergeven](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)
4. Klik op de container met de naam **AADDC gebruikers** om te zien alle gebruikers en groepen die tot het beheerde domein behoren. Ziet u gebruikersaccounts en groepen uit uw Azure AD tenant weergeven om in deze container. In dit voorbeeld, een gebruikersaccount voor de gebruiker met de naam "bob" en een groep met de naam 'AAD DC Administrators' zijn beschikbaar in deze container.

    ![Active Directory-Beheercentrum - domeingebruikers](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)
5. Klik op de container met de naam **AADDC Computers** om te zien van de computers die zijn gekoppeld aan dit beheerde domein. Hier ziet u een vermelding voor de huidige virtuele machine die is gekoppeld aan het domein. Computeraccounts voor alle computers die zijn gekoppeld aan het Azure AD Domain Services beheerde domein worden opgeslagen in deze container 'AADDC Computers'.

    ![Active Directory-Beheercentrum - domein lid zijn van computers](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een Windows Server-machine toevoegen aan Azure AD Domain Services beheerde domein](active-directory-ds-admin-guide-join-windows-vm.md)
* [Remote Server Administration Tools implementeren](https://technet.microsoft.com/library/hh831501.aspx)
