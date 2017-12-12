---
title: 'Azure Active Directory Domain Services: Een virtuele machine van Windows Server toevoegen aan een beheerd domein | Microsoft Docs'
description: Virtuele machine met Windows Server toevoegen aan Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: 1ea3f7271bd165bf42d520e4a0267a80dcca58d5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Een virtuele Windows Server-machine toevoegen aan een beheerd domein
In dit artikel laat zien hoe een Windows Server-virtuele machine met de Azure portal implementeren. Vervolgens ziet u hoe de virtuele machine toevoegen aan een beheerd domein van Azure AD Domain Services.

## <a name="step-1-create-the-windows-server-virtual-machine"></a>Stap 1: Maak de virtuele machine van Windows Server
Voer de volgende stappen uit voor het maken van een virtuele Windows-computer die lid zijn van het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld.

1. Meld u bij Azure Portal aan via [http://portal.azure.com](http://portal.azure.com).
2. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
3. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**.

    ![Selecteer de installatiekopie](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. Configureer de basisinstellingen voor de virtuele machine op de **basisbeginselen** pagina van de wizard.

    ![Basisinstellingen voor de virtuele machine configureren](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > De gebruikersnaam en wachtwoord die u hier opgeeft, zijn voor een lokale administrator-account gebruikt voor aanmelding bij de virtuele machine. Kies een sterk wachtwoord voor het beveiligen van de virtuele machine met wachtwoord beveiligingsaanvallen. Geef geen referenties voor een domeingebruikersaccount hier.
    >

5. Selecteer een **grootte** voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**.

    ![Grootte voor virtuele machine selecteren](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. Op de **instellingen** pagina van de wizard selecteert u het virtuele netwerk waarin uw Azure AD Domain Services beheerd domein wordt geïmplementeerd. Kies een ander subnet dan een uw beheerde domein wordt geïmplementeerd in. Behoud de standaardinstellingen voor de overige instellingen en klik op **OK**.

    ![Virtueel netwerk voor virtuele machine selecteren](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Kies de juiste virtuele netwerk en subnet.**
    > Selecteer het virtuele netwerk waarin uw beheerde domein wordt geïmplementeerd of een virtueel netwerk dat is verbonden met de virtuele netwerk peering. Als u een niet-verbonden virtueel netwerk selecteert, kunt u de virtuele machine kan niet toevoegen aan het beheerde domein.
    > Het is raadzaam om uw beheerde domein implementeren in een subnet toegewezen. Kies daarom niet het subnet waarin u uw beheerde domein hebt ingeschakeld.

7. Op de **aankoop** pagina, controleert u de instellingen en klik op **OK** voor het implementeren van de virtuele machine.
8. De implementatie van de VM is vastgemaakt aan de Azure-portaldashboard.

    ![Klaar](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
9. Nadat de implementatie is voltooid, ziet u informatie over de virtuele machine in de **overzicht** pagina.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Stap 2: Verbinding maken met de Windows Server-virtuele machine met behulp van het lokale administrator-account
Nu verbinding maken met de nieuwe Windows Server virtuele machine, aan het domein toevoegen. Gebruik de lokale administrator-referenties die u hebt opgegeven bij het maken van de virtuele machine.

Voer de volgende stappen uit verbinding maken met de virtuele machine.

1. Klik op de **Connect** knop op de **overzicht** pagina. Een Remote Desktop Protocol (RDP)-bestand is gemaakt en gedownload.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**.
3. Voer bij de aanmeldingsprompt uw **lokale beheerdersreferenties**, die u hebt opgegeven tijdens het maken van de virtuele machine. Bijvoorbeeld, hebben we 'localhost\mahesh' in dit voorbeeld gebruikt.
4. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op Ja of doorgaan om door te gaan met de verbinding.

Op dit moment moet u worden aangemeld bij de zojuist gemaakte virtuele Windows-computer met lokale Administrator-referenties. De volgende stap is de virtuele machine toevoegen aan het domein.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Stap 3: Join, de virtuele machine van Windows Server aan de beheerde AAD-DS-domein
Voer de volgende stappen voor de virtuele machine met Windows Server toevoegen aan de beheerde AAD-DS-domein.

1. Verbinding maken met de Windows-Server zoals u in stap 2. Open in het startscherm **Serverbeheer**.
2. Klik op **lokale Server** in het linkerdeelvenster van het venster Server Manager.

    ![Serverbeheer starten op de virtuele machine](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Klik op **werkgroep** onder de **eigenschappen** sectie. In de **Systeemeigenschappen** eigenschappenpagina, klikt u op **wijziging** aan het domein.

    ![De pagina eigenschappen van systeem](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Geef de domeinnaam van uw beheerde domein van Azure AD Domain Services in de **domein** tekstvak en klik op **OK**.

    ![Geef het domein worden toegevoegd](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. U wordt gevraagd uw referenties voor deelname aan het domein op te geven. Zorg ervoor dat u **geeft u de referenties voor een gebruiker die horen bij de AAD-DC-beheerders** groep. Alleen leden van deze groep hebben bevoegdheden voor machines toevoegen aan het beheerde domein.

    ![Geef de referenties voor domein](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. U kunt referenties opgeven in een van de volgende manieren:

   * **UPN-indeling: (aanbevolen)** het UPN-achtervoegsel voor de gebruikersaccount opgeven, zoals geconfigureerd in Azure AD. In dit voorbeeld is het het UPN-achtervoegsel van de gebruiker "bob" 'bob@domainservicespreview.onmicrosoft.com'.
   * **SAMAccountName indeling:** kunt u de accountnaam in de indeling SAMAccountName. In dit voorbeeld moet de gebruiker "bob" zou 'CONTOSO100\bob' invoeren.

     > [!TIP]
     > **U kunt het beste de UPN-indeling gebruikt referenties opgeven.**
     > Als de UPN-voorvoegsel van een gebruiker is te lang (bijvoorbeeld 'joehasareallylongname'), mogelijk de SAMAccountName automatisch wordt gegenereerd. Als meerdere gebruikers het voorvoegsel met dezelfde UPN (bijvoorbeeld "bob") in uw Azure AD-tenant hebben, mogelijk de indeling van hun SAMAccountName automatisch gegenereerd door de service. In dergelijke gevallen kan de UPN-indeling op betrouwbare wijze worden gebruikt voor aanmelding bij het domein.
     >

7. Nadat het domein is geslaagd, ziet u het volgende bericht Verwelkom u aan het domein. Start opnieuw op de virtuele machine voor het domein join-bewerking te voltooien.

    ![Welkom bij het domein](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Het oplossen van problemen aan domein toevoegen
### <a name="connectivity-issues"></a>Connectiviteitsproblemen
Als de virtuele machine niet gevonden van het domein is, raadpleegt u de volgende stappen:

* Zorg ervoor dat de virtuele machine is verbonden met hetzelfde virtuele netwerk als u Domain Services in hebt ingeschakeld. Zo niet, de virtuele machine is geen verbinding maken met het domein en daarom kan niet aan het domein.
* Zorg ervoor dat de virtuele machine zich op een virtueel netwerk dat op zijn beurt is verbonden met het virtuele netwerk waarin u Domain Services hebt ingeschakeld.
* Probeer te pingen van het domein met de domeinnaam van het beheerde domein (bijvoorbeeld ping contoso100.com). Als u niet lukt om dit te doen, probeert te pingen van de IP-adressen voor het domein dat wordt weergegeven op de pagina waarop u Azure AD Domain Services hebt ingeschakeld (bijvoorbeeld ' ping 10.0.0.4'). Als u kunnen ping het IP-adres, maar niet het domein bent, DNS mogelijk niet juist geconfigureerd. Controleer of de IP-adressen van het domein zijn geconfigureerd als DNS-servers voor het virtuele netwerk.
* Probeer de DNS-resolver cache op de virtuele machine (' ipconfig/flushdns).

Als u in het dialoogvenster waarin u wordt gevraagd om referenties krijgt aan het domein, hebt u geen problemen met de netwerkverbinding.

### <a name="credentials-related-issues"></a>Problemen met de referenties
Raadpleeg de volgende stappen uit als u problemen met de referenties ondervindt en kan niet deelnemen aan het domein zijn.

* Probeer de UPN-indeling met referenties opgeven. Als er meerdere gebruikers met hetzelfde voorvoegsel UPN in uw tenant zijn of als de UPN-voorvoegsel te lang is, mogelijk de SAMAccountName voor uw account automatisch wordt gegenereerd. Daarom kan de indeling SAMAccountName voor uw account afwijken van wat u verwacht, of gebruik in uw lokale domein.
* Probeer de referenties van een gebruikersaccount die deel uitmaakt van de groep 'AAD DC Administrators' gebruiken.
* Zorg ervoor dat u [wachtwoordsynchronisatie hebt ingeschakeld](active-directory-ds-getting-started-password-sync.md) volgens de stappen beschreven in de handleiding.
* Zorg dat u de UPN van de gebruiker gebruikt zoals geconfigureerd in Azure AD (bijvoorbeeld 'bob@domainservicespreview.onmicrosoft.com') aan te melden.
* Zorg ervoor dat u lang genoeg voor Wachtwoordsynchronisatie voltooid zoals opgegeven in de handleiding hebt gewacht.

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
