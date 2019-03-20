---
title: 'Azure Active Directory Domain Services: Een Windows Server-VM toevoegen aan een beheerd domein | Microsoft Docs'
description: Een Windows Server virtuele machine toevoegen aan Azure AD DS
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: ergreenl
ms.openlocfilehash: f18d3409d53123733d7f423c77864cd0739765eb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57900587"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Een virtuele Windows Server-machine toevoegen aan een beheerd domein
In dit artikel laat zien hoe een Windows Server-machine implementeren met behulp van de Azure-portal. Vervolgens wordt uitgelegd hoe u de virtuele machine koppelen aan een beheerd domein van Azure Active Directory Domain Services (Azure AD DS).

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="step-1-create-a-windows-server-virtual-machine"></a>Stap 1: Een virtuele machine voor Windows Server maken
Voor het maken van een Windows virtuele machine die lid is van het virtuele netwerk waarin u Azure AD DS hebt ingeschakeld, voert u de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer boven in het linkerdeelvenster **nieuw**.
3. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**.

    ![De Windows Server 2016 Datacenter-koppeling](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. In de **basisbeginselen** deelvenster van de wizard de basisinstellingen voor de virtuele machine configureren.

    ![De grondbeginselen van deelvenster](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > De gebruikersnaam en het wachtwoord die u hier opgeeft, zijn voor een lokale administrator-account dat wordt gebruikt voor aanmelding bij de virtuele machine. Kies een sterk wachtwoord voor het beveiligen van de virtuele machine op basis van wachtwoord brute-force-aanvallen. Voer een domeingebruikersaccount referenties hier niet.
    >

5. Selecteer een **grootte** voor de virtuele machine. Als u meer groottes, selecteer **weergeven van alle** of wijzig de **ondersteund schijftype** filter.

    ![Het deelvenster 'Kies een grootte'](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. In de **instellingen** deelvenster, selecteert u het virtuele netwerk waarin u uw Azure AD DS-beheerde domein is geïmplementeerd. Kies een ander subnet dan de versie die uw beheerde domein wordt geïmplementeerd in. Behoud de standaardinstellingen voor de overige instellingen en selecteer vervolgens **OK**.

    ![Virtuele-netwerkinstellingen voor de virtuele machine](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Kies het juiste virtuele netwerk en subnet.**
    >
    > Selecteer het virtuele netwerk waarin uw beheerde domein is geïmplementeerd of een virtueel netwerk dat is verbonden met behulp van virtual network-peering. Als u een verbonden virtueel netwerk selecteert, kunt u de virtuele machine kan niet deelnemen aan het beheerde domein.
    >
    > Het is raadzaam om uw beheerde domein implementeert in een toegewezen subnet. Kies daarom niet het subnet waarin u uw beheerde domein hebt ingeschakeld.

7. Behoud de standaardinstellingen voor de overige instellingen en selecteer vervolgens **OK**.
8. Op de **aankoop** pagina, Controleer de instellingen en selecteer vervolgens **OK** de virtuele machine wilt implementeren.
9. De VM-implementatie is vastgemaakt aan het dashboard van de Azure portal.

    ![Klaar](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
10. Nadat de implementatie is voltooid, kunt u informatie over de virtuele machine bekijken op de **overzicht** pagina.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-by-using-the-local-administrator-account"></a>Stap 2: Verbinding maken met de Windows Server-machine met behulp van het lokale administrator-account
Vervolgens verbinding maken met de zojuist gemaakte Windows Server virtuele machine aan het domein gekoppeld. Gebruik de lokale administrator-referenties die u hebt opgegeven tijdens het maken van de virtuele machine.

Voor verbinding met de virtuele machine, moet u de volgende stappen uitvoeren:

1. In de **overzicht** venster **Connect**.  
    Een Remote Desktop Protocol (RDP)-bestand gemaakt en gedownload.

    ![Verbinding maken met Windows virtuele machine](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd.
3. Voer uw **lokale beheerdersreferenties**, dat u hebt opgegeven tijdens het maken van de virtuele machine (bijvoorbeeld *localhost\mahesh*).
4. Als u een certificaatwaarschuwing tijdens het aanmeldingsproces ziet, selecteert u **Ja** of **doorgaan** om verbinding te maken.

Op dit moment, moet u zijn aangemeld op de zojuist gemaakte virtuele machine voor Windows met uw lokale beheerdersreferenties. De volgende stap is het toevoegen van de virtuele machine aan het domein.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-azure-ad-ds-managed-domain"></a>Stap 3: De Windows Server-machine toevoegen aan de Azure AD DS beheerd domein
Als u wilt deelnemen aan de Windows Server-machine met Azure AD DS-beheerd domein, voert u de volgende stappen uit:

1. Verbinding maken met de Windows Server-VM, zoals in "Stap 2." Op de **Start** scherm openen **Serverbeheer**.
2. In het linkerdeelvenster van de **Serverbeheer** venster **lokale Server**.

    ![Het venster Server Manager op de virtuele machine](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Onder **eigenschappen**, selecteer **werkgroep**.
4. In de **Systeemeigenschappen** venster **wijziging** aan het domein.

    ![Het venster Eigenschappen](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

5. In de **domein** vak, geef de naam van uw Azure AD DS-beheerd domein en selecteer vervolgens **OK**.

    ![Geef het domein zal worden gekoppeld](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

6. U wordt gevraagd uw referenties voor deelname aan het domein in te voeren. Gebruik de referenties voor een *gebruiker die deel uitmaakt van de Azure AD-DC-beheerdersgroep*. Alleen leden van deze groep hebben bevoegdheden om machines te koppelen aan het beheerde domein.

    ![Het venster Windows-beveiliging voor het opgeven van referenties](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

7. U kunt referenties opgeven in een van de volgende manieren:

   * **UPN-indeling**: (Aanbevolen) Geef het achtervoegsel van de user principal name (UPN) voor het gebruikersaccount, zoals geconfigureerd in Azure AD. In dit voorbeeld wordt het UPN-achtervoegsel van de gebruiker *bob* is *bob\@domainservicespreview.onmicrosoft.com*.

   * **SAMAccountName-indeling**: U kunt de accountnaam opgeven in de SAMAccountName-indeling. In dit voorbeeld wordt de gebruiker *bob* moet invoeren *CONTOSO100\bob*.

     > [!TIP]
     > **U wordt aangeraden met behulp van de indeling voor UPN-referenties opgeven.**
     >
     > Als het voorvoegsel van de UPN van een gebruiker is te lang (bijvoorbeeld *joehasareallylongname*), de SAMAccountName mogelijk automatisch gegenereerd. Als meerdere gebruikers het voorvoegsel met dezelfde UPN hebben (bijvoorbeeld *bob*) in uw Azure AD-tenant de SAMAccountName-indeling kan worden automatisch gegenereerd door de service. In dergelijke gevallen kan de UPN-indeling op betrouwbare wijze worden gebruikt voor aanmelding bij het domein.
     >

8. Nadat u hebt een domein is toegevoegd, wordt u door het volgende bericht geïnteresseerd in het domein.

    ![Welkom bij het domein](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

9. Als u wilt toevoegen van het domein hebt voltooid, start u de virtuele machine.

## <a name="troubleshoot-joining-a-domain"></a>Deelnemen aan een domein oplossen
### <a name="connectivity-issues"></a>Connectiviteitsproblemen
Als de virtuele machine niet gevonden van het domein is, probeert u de volgende stappen:

* Controleer of dat de virtuele machine is verbonden met Azure AD DS is ingeschakeld in hetzelfde virtuele netwerk. Anders is de virtuele machine kan geen verbinding maken met of toegevoegd aan het domein.

* Controleer of de virtuele machine op een virtueel netwerk dat op zijn beurt is verbonden met het virtuele netwerk dat in Azure AD DS is ingeschakeld.

* Pingt de DNS-domeinnaam van het beheerde domein (bijvoorbeeld *ping contoso100.com*). Als u niet om dit te doen, probeert te pingen van de IP-adressen voor het domein dat wordt weergegeven op de pagina waar u Azure AD DS ingeschakeld (bijvoorbeeld *ping 10.0.0.4*). Als u het IP-adres, maar niet het domein pingen kunt, DNS mogelijk niet juist geconfigureerd. Controleer of de IP-adressen van het domein zijn geconfigureerd als DNS-servers voor het virtuele netwerk.

* Probeer de DNS-resolver cache op de virtuele machine (*ipconfig/flushdns*).

Als een venster waarin u wordt gevraagd om referenties weergegeven aan het domein, hoeft u geen problemen met de netwerkverbinding.

### <a name="credentials-related-issues"></a>Problemen met betrekking tot referenties
Als u hebt met referenties problemen en kan niet deelnemen aan het domein, probeert u de volgende stappen:

* Gebruik de indeling voor UPN-referenties opgeven. Als er veel gebruikers met hetzelfde voorvoegsel UPN in uw tenant zijn of als het UPN-voorvoegsel te lang is, kan de SAMAccountName voor uw account worden automatisch gegenereerd. In dergelijke gevallen kan de SAMAccountName-indeling voor uw account afwijken van wat u verwacht of gebruikt in uw on-premises domein.

* Gebruik de referenties van een gebruikersaccount die deel uitmaakt van de *AAD DC Administrators* groep.

* Controleer of u hebt [ingeschakeld Wachtwoordsynchronisatie](active-directory-ds-getting-started-password-sync.md) met uw beheerde domein.

* Controleer of u de UPN van de gebruiker hebt gebruikt zoals geconfigureerd in Azure AD (bijvoorbeeld *bob\@domainservicespreview.onmicrosoft.com*) aan te melden.

* Wacht lang genoeg voor Wachtwoordsynchronisatie moeten worden uitgevoerd, zoals opgegeven in de introductiehandleiding.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD DS aan de slag](active-directory-ds-getting-started.md)
* [Een Azure AD DS-beheerd domein beheren](active-directory-ds-admin-guide-administer-domain.md)
