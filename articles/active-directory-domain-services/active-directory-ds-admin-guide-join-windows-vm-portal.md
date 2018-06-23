---
title: 'Azure Active Directory Domain Services: Een virtuele machine van Windows Server toevoegen aan een beheerd domein | Microsoft Docs'
description: Windows Server een virtuele machine toevoegen aan Azure AD DS
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: maheshu
ms.openlocfilehash: 2929f85b738171f7fb7f5b66af90e4e2ab54f5d0
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317167"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Een virtuele Windows Server-machine toevoegen aan een beheerd domein
In dit artikel laat zien hoe een virtuele machine van Windows Server implementeren met behulp van de Azure-portal. Vervolgens ziet u hoe de virtuele machine toevoegen aan een beheerd domein van Azure Active Directory Domain Services (Azure AD DS).

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="step-1-create-a-windows-server-virtual-machine"></a>Stap 1: Maak een virtuele machine van Windows Server
Voor het maken van een virtuele Windows-computer die is gekoppeld aan het virtuele netwerk waarin u Azure AD DS hebt ingeschakeld, moet u de volgende stappen uitvoeren:

1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Selecteer boven aan het linkerdeelvenster **nieuw**.
3. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**.

    ![De Windows Server 2016 Datacenter-koppeling](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. In de **basisbeginselen** deelvenster van de wizard de basisinstellingen voor de virtuele machine te configureren.

    ![Het deelvenster basisbeginselen](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > De gebruikersnaam en wachtwoord die u hier opgeeft, zijn voor een lokale administrator-account dat wordt gebruikt voor aanmelding bij de virtuele machine. Kies een sterk wachtwoord voor het beveiligen van de virtuele machine met wachtwoord beveiligingsaanvallen. Geef geen referenties voor een domeingebruikersaccount hier.
    >

5. Selecteer een **grootte** voor de virtuele machine. Als u wilt meer grootten weergeven, selecteert u **weergeven van alle** of wijzig de **ondersteund schijftype** filter.

    ![Het deelvenster 'Kies een grootte'](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. In de **instellingen** deelvenster, selecteer het virtuele netwerk waarin u uw Azure AD DS beheerd domein wordt geïmplementeerd. Kies een ander subnet bevindt dan de versie die in uw beheerde domein wordt geïmplementeerd. Behoud de standaardinstellingen voor de overige instellingen en selecteer vervolgens **OK**.

    ![Virtuele-netwerkinstellingen voor de virtuele machine](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Kies de juiste virtuele netwerk en subnet.**
    >
    > Selecteer het virtuele netwerk waarin uw beheerde domein wordt geïmplementeerd of een virtueel netwerk dat is verbonden met het met behulp van virtueel netwerk peering. Als u een niet-verbonden virtueel netwerk selecteert, kunt u de virtuele machine kan niet toevoegen aan het beheerde domein.
    >
    > Het is raadzaam om uw beheerde domein implementeren in een subnet toegewezen. Kies daarom niet het subnet waarin u uw beheerde domein hebt ingeschakeld.

7. Behoud de standaardinstellingen voor de overige instellingen en selecteer vervolgens **OK**.
8. Op de **aankoop** pagina, controleert u de instellingen en selecteer vervolgens **OK** voor het implementeren van de virtuele machine.
9. De implementatie van de VM is vastgemaakt aan de Azure-portaldashboard.

    ![Klaar](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
10. Nadat de implementatie is voltooid, kunt u informatie over de virtuele machine weergeven op de **overzicht** pagina.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-by-using-the-local-administrator-account"></a>Stap 2: Verbinding maken met de virtuele machine van Windows Server met behulp van het lokale administrator-account
Vervolgens verbinding maken met de zojuist gemaakte virtuele machine in Windows Server aan het domein toevoegen. Gebruik de lokale administrator-referenties die u hebt opgegeven toen u de virtuele machine hebt gemaakt.

Voor verbinding met de virtuele machine, moet u de volgende stappen uitvoeren:

1. In de **overzicht** deelvenster **Connect**.  
    Een Remote Desktop Protocol (RDP)-bestand is gemaakt en gedownload.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd.
3. Voer uw **lokale beheerdersreferenties**, die u hebt opgegeven toen u de virtuele machine hebt gemaakt (bijvoorbeeld *localhost\mahesh*).
4. Als u er een certificaatwaarschuwing weergegeven tijdens het aanmelden, selecteert u **Ja** of **doorgaan** verbinding maken.

Op dit moment moet u zijn aangemeld op de zojuist gemaakte Windows virtuele machine met de lokale beheerdersreferenties. De volgende stap is de virtuele machine toevoegen aan het domein.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-azure-ad-ds-managed-domain"></a>Stap 3: De virtuele machine met Windows Server naar de Azure AD DS beheerd domein toevoegen
Als u wilt deelnemen aan de virtuele machine van Windows Server naar de Azure AD DS beheerd domein, moet u de volgende stappen uitvoeren:

1. Verbinding maken met de virtuele machine van Windows Server, zoals in "Stap 2." Op de **Start** scherm open **Serverbeheer**.
2. Klik in het linkerdeelvenster van de **Serverbeheer** Selecteer **lokale Server**.

    ![Het venster in Serverbeheer op de virtuele machine](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Onder **eigenschappen**, selecteer **werkgroep**.
4. In de **Systeemeigenschappen** Selecteer **wijziging** aan het domein.

    ![Het venster Systeemeigenschappen](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

5. In de **domein** vak en selecteer vervolgens de naam van uw Azure AD DS beheerd domein opgeven **OK**.

    ![Geef het domein worden toegevoegd](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

6. U wordt gevraagd uw referenties voor deelname aan het domein op te geven. Gebruik de referenties voor een *gebruiker die bij de Azure AD-DC-beheerdersgroep hoort*. Alleen leden van deze groep hebben bevoegdheden voor machines toevoegen aan het beheerde domein.

    ![Het venster Windows-beveiliging voor het opgeven van referenties](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

7. U kunt referenties opgeven in een van de volgende manieren:

   * **UPN-indeling**: (aanbevolen) Geef de UPN (user Principal name)-achtervoegsel voor de gebruikersaccount, zoals geconfigureerd in Azure AD. In dit voorbeeld wordt het UPN-achtervoegsel van de gebruiker *bob* is *bob@domainservicespreview.onmicrosoft.com*.

   * **SAMAccountName indeling**: U kunt de accountnaam opgeven in de indeling SAMAccountName. In dit voorbeeld wordt de gebruiker *bob* zou moet invoeren *CONTOSO100\bob*.

     > [!TIP]
     > **U kunt het beste de UPN-indeling gebruikt referenties opgeven.**
     >
     > Als een gebruiker UPN-voorvoegsel overmatig lange (bijvoorbeeld *joehasareallylongname*), de SAMAccountName mogelijk automatisch wordt gegenereerd. Als meerdere gebruikers met hetzelfde voorvoegsel voor de UPN (bijvoorbeeld *bob*) in uw Azure AD-tenant, automatisch gegenereerd door de service van de indeling van hun SAMAccountName mogelijk. In dergelijke gevallen kan de UPN-indeling op betrouwbare wijze worden gebruikt voor aanmelding bij het domein.
     >

8. Nadat u hebt een domein is toegevoegd, het volgende bericht Welkom wordt geheten bij het domein.

    ![Welkom bij het domein](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

9. Als u lid worden van het domein, start opnieuw op de virtuele machine.

## <a name="troubleshoot-joining-a-domain"></a>Problemen met deelname aan een domein
### <a name="connectivity-issues"></a>Connectiviteitsproblemen
Als de virtuele machine niet gevonden van het domein is, probeert u de volgende stappen:

* Controleer of dat de virtuele machine is verbonden met Azure AD DS is ingeschakeld in hetzelfde virtuele netwerk. Anders is de virtuele machine kan geen verbinding maken met of deelnemen aan het domein.

* Controleer of de virtuele machine op een virtueel netwerk dat op zijn beurt is verbonden met het virtuele netwerk dat in Azure AD DS is ingeschakeld.

* Probeer te pingen van de DNS-domeinnaam van het beheerde domein (bijvoorbeeld *ping, contoso100.com*). Als u niet lukt om dit te doen, probeert te pingen van de IP-adressen voor het domein dat wordt weergegeven op de pagina waarop u Azure AD DS ingeschakeld (bijvoorbeeld *pingen 10.0.0.4*). Als u kunt de opdracht ping het IP-adres, maar niet het domein, DNS mogelijk niet juist geconfigureerd. Controleer of de IP-adressen van het domein zijn geconfigureerd als DNS-servers voor het virtuele netwerk.

* Probeer de DNS-resolver cache op de virtuele machine (*ipconfig/flushdns*).

Als een venster wordt weergegeven waarin u wordt gevraagd om referenties aan het domein, hoeft u geen problemen met de netwerkverbinding.

### <a name="credentials-related-issues"></a>Problemen met de referenties
Als u problemen met de referenties ondervindt en kan niet deelnemen aan het domein zijn, probeert u de volgende stappen:

* Probeer de UPN-indeling met referenties opgeven. Als er veel gebruikers met hetzelfde voorvoegsel UPN in uw tenant of als de UPN-voorvoegsel te lang is, mogelijk de SAMAccountName voor uw account automatisch wordt gegenereerd. In dergelijke gevallen kan de indeling SAMAccountName voor uw account afwijken van wat u verwacht, of gebruik in uw lokale domein.

* Gebruik de referenties van een gebruikersaccount dat hoort bij de *AAD DC beheerders* groep.

* Controleer of u hebt [ingeschakeld Wachtwoordsynchronisatie](active-directory-ds-getting-started-password-sync.md) naar uw beheerde domein.

* Controleer of u de UPN van de gebruiker hebt gebruikt zoals geconfigureerd in Azure AD (bijvoorbeeld *bob@domainservicespreview.onmicrosoft.com*) aan te melden.

* Wacht lang genoeg voor Wachtwoordsynchronisatie te worden uitgevoerd, zoals opgegeven in de introductiehandleiding.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD DS aan de slag](active-directory-ds-getting-started.md)
* [Een Azure AD DS beheerd domein beheren](active-directory-ds-admin-guide-administer-domain.md)
