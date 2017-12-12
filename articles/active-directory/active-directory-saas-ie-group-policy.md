---
title: Azure Access Configuratiescherm extensie voor IE met behulp van een groepsbeleidsobject implementeren | Microsoft Docs
description: Het gebruik van Groepsbeleid voor het implementeren van de invoegtoepassing Internet Explorer voor de portal mijn Apps.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 7c2d49c8-5be0-4e7e-abac-332f9dfda736
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a203548575eacb2d0eb0d09a4aaf239b11caad3c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>De extensie van het Configuratiescherm toegang voor Internet Explorer met behulp van Groepsbeleid implementeren
Deze zelfstudie laat zien hoe het gebruik van Groepsbeleid op afstand installeren van de extensie Toegangspaneel voor Internet Explorer op uw gebruikers-machines. Deze uitbreiding is vereist voor Internet Explorer-gebruikers hoeven zich aanmeldt bij apps die zijn geconfigureerd met [op basis van wachtwoorden eenmalige aanmelding](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

Het is raadzaam dat beheerders de implementatie van deze extensie automatiseren. Gebruikers hebben anders downloaden en installeren van de extensie zelf, die is kwetsbaar voor gebruikersfouten en zijn beheerdersrechten vereist. Deze zelfstudie bevat één methode voor het software-implementaties automatiseren met behulp van Groepsbeleid. [Meer informatie over Groepsbeleid.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

De extensie Toegangspaneel is ook beschikbaar voor [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) en [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), geen van beide vereist beheerdersmachtigingen om te installeren.

## <a name="prerequisites"></a>Vereisten
* U hebt ingesteld [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), en u hebt uw gebruikers machines toegevoegd aan uw domein.
* U moet de machtiging 'Instellingen bewerken' voor het bewerken van het groepsbeleidsobject (GPO) hebben. Standaard hebben leden van de volgende beveiligingsgroepen deze machtiging: Domeinadministrators, Ondernemingsadministrators en Maker Eigenaar Groepsbeleid. [Meer informatie.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a>Stap 1: Het distributiepunt maken
Eerst moet u het installer-pakket plaatsen op een netwerklocatie die toegankelijk zijn voor de machines die u de extensie wilt op afstand te installeren. Voer de volgende stappen uit om dit te doen:

1. Meld u aan bij de server als een beheerder
2. In de **Serverbeheer** venster, gaat u naar **bestanden- en opslagservices**.
   
    ![Geopende bestanden- en opslagservices](./media/active-directory-saas-ie-group-policy/files-services.png)
3. Ga naar de **Shares** tabblad. Klik vervolgens op **taken** > **nieuwe Share...**
   
    ![Geopende bestanden- en opslagservices](./media/active-directory-saas-ie-group-policy/shares.png)
4. Voltooi de **Wizard Nieuwe Share** en stel de machtigingen om ervoor te zorgen dat deze kan worden geopend op uw gebruikers-machines. [Meer informatie over shares.](https://technet.microsoft.com/library/cc753175.aspx)
5. De volgende Microsoft Windows Installer-pakket (MSI-bestand) te downloaden: [toegang Configuratiescherm Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)
6. Kopieer het installatiepakket op de gewenste locatie op de share.
   
    ![Kopieer het MSI-bestand naar de share.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. Controleer of uw clientcomputers toegang kunnen krijgen tot het installatiepakket van de share. 

## <a name="step-2-create-the-group-policy-object"></a>Stap 2: Het groepsbeleidsobject maken
1. Meld u bij de server die als host fungeert voor de installatie van Active Directory Domain Services (AD DS).
2. In Serverbeheer gaat u naar **extra** > **Group Policy Management**.
   
    ![Ga naar Extra > beleid Management groep](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. Klik in het linkerdeelvenster van de **Group Policy Management** venster weergeven van uw hiërarchie van organisatie-eenheid (OE) en bepalen bij welke bereik wilt u het groepsbeleid van toepassing. Bijvoorbeeld, u kiest een kleine organisatie-eenheid voor het implementeren van een paar gebruikers voor het testen van besluiten of kiest u een site op het hoogste organisatie-eenheid om te implementeren voor uw hele organisatie.
   
   > [!NOTE]
   > Als u wilt maken of bewerken van uw organisatie-eenheden (OE's), Ga terug naar de Server Manager en gaat u naar **extra** > **Active Directory: gebruikers en Computers**.
   > 
   > 
4. Nadat u een organisatie-eenheid hebt geselecteerd, met de rechtermuisknop en selecteer **een groepsbeleidsobject in dit domein maken en hier een koppeling...**
   
    ![Een nieuw groepsbeleidsobject maken](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. In de **nieuw groepsbeleidsobject** modus, typt u een naam voor het nieuwe groepsbeleidsobject.
   
    ![De naam van het nieuwe groepsbeleidsobject](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. Met de rechtermuisknop op het groepsbeleidsobject dat u maakte en selecteer **bewerken**.
   
    ![Het nieuwe groepsbeleidsobject bewerken](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-the-installation-package"></a>Stap 3: Het installatiepakket toewijzen
1. Bepalen of u dat wilt voor het implementeren van de extensie op basis van **Computerconfiguratie** of **Gebruikersconfiguratie**. Wanneer u [Computerconfiguratie](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), de extensie wordt geïnstalleerd op de computer ongeacht welke gebruikers zich aanmelden bij deze. Met [Gebruikersconfiguratie](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), gebruikers hebben de extensie die is geïnstalleerd voor deze ongeacht welke computers ze zich aanmelden.
2. Klik in het linkerdeelvenster van de **Editor voor Groepsbeleidsbeheer** venster, gaat u naar een van de volgende paden voor mappen, afhankelijk van welk type van de configuratie die u hebt gekozen:
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. Met de rechtermuisknop op **Software-installatie**, selecteer daarna **nieuw** > **pakket...**
   
    ![Maak een nieuw pakket van de software-installatie](./media/active-directory-saas-ie-group-policy/new-package.png)
4. Ga naar de gedeelde map met het installer-pakket van [stap 1: maken van het distributiepunt](#step-1-create-the-distribution-point), selecteert u het .msi-bestand en klik op **Open**.
   
   > [!IMPORTANT]
   > Als de share bevindt zich op deze server, controleert u of u de .msi opent via het netwerk-bestandspad in plaats van het lokale bestandspad.
   > 
   > 
   
    ![Selecteer het installatiepakket uit de gedeelde map.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. In de **Software distribueren** vragen, selecteer **toegewezen** voor uw implementatiemethode. Klik vervolgens op **OK**.
   
    ![Selecteer toegewezen en klik op OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

De extensie is nu geïmplementeerd op de organisatie-eenheid die u hebt geselecteerd. [Meer informatie over Software-installatie.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Stap 4: Automatisch inschakelen voor de uitbreiding voor Internet Explorer
Naast het installatieprogramma wordt uitgevoerd, moet alle uitbreidingen voor Internet Explorer ingeschakeld zijn voordat deze kan worden gebruikt. Volg onderstaande stappen voor het inschakelen van de uitbreiding van het Configuratiescherm toegang met behulp van Groepsbeleid:

1. In de **Editor voor Groepsbeleidsbeheer** venster, gaat u naar een van de volgende paden, afhankelijk van welk type van de configuratie die u kiest in [stap 3: het installatiepakket toewijzen](#step-3-assign-the-installation-package):
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. Met de rechtermuisknop op **lijst met invoegtoepassingen**, en selecteer **bewerken**.
    ![Invoegtoepassing lijst niet bewerken.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)
3. In de **lijst met invoegtoepassingen** Selecteer **ingeschakeld**. Klik vervolgens onder de **opties** sectie, klikt u op **weergeven...** .
   
    ![Klik op inschakelen en klik vervolgens op weergeven...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. In de **inhoud weergeven** venster de volgende stappen uitvoeren:
   
   1. Voor de eerste kolom (de **waardenaam** veld), kopiëren en plakken van de volgende klasse-ID:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   2. Voor de tweede kolom (de **waarde** veld), typt u de volgende waarde:`1`
   3. Klik op **OK** sluiten de **inhoud weergeven** venster.
      
      ![Vul de parameterwaarden als is beschreven.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. Klik op **OK** naar uw wijzigingen toepassen op en sluit de **lijst met invoegtoepassingen** venster.

De extensie moet nu worden ingeschakeld voor de machines in de geselecteerde OE. [Meer informatie over het gebruik van Groepsbeleid inschakelen of uitschakelen van invoegtoepassingen voor Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>Stap 5 (optioneel): 'Wachtwoord onthouden' Prompt uitschakelen
Wanneer gebruikers aanmelden naar websites met de extensie van het Configuratiescherm toegang, Internet Explorer op de volgende gevraagd 'Wilt u uw wachtwoord opslaan?' kan tonen

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Als u voorkomen dat uw gebruikers deze vraag te zien wilt, klikt u vervolgens de stappen hieronder om te voorkomen dat automatisch aanvullen van onthouden wachtwoorden:

1. In de **Editor voor Groepsbeleidsbeheer** venster, gaat u naar het pad hieronder vermeld. Deze configuratieinstelling is alleen beschikbaar onder **Gebruikersconfiguratie**.
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. De instelling met de naam vinden **Schakel de functie voor automatisch aanvullen voor gebruikersnamen en wachtwoorden op formulieren**.
   
   > [!NOTE]
   > Eerdere versies van Active Directory kunnen deze instellingen met de naam van de lijst **niet toestaan dat wachtwoorden op te slaan automatisch aanvullen**. De configuratie voor de instelling verschilt van de instelling die wordt beschreven in deze zelfstudie.
   > 
   > 
   
    ![Vergeet niet om te zoeken voor dit onder gebruikersinstellingen.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. Klik met de rechtermuisknop op de instelling hierboven en selecteer **bewerken**.
4. In het venster met de titel **Schakel de functie voor automatisch aanvullen voor gebruikersnamen en wachtwoorden op formulieren**, selecteer **uitgeschakelde**.
   
    ![Selecteer uitschakelen](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. Klik op **OK** deze wijzigingen toepassen en sluit het venster.

Gebruikers wordt niet langer hun referenties of automatisch aanvullen gebruiken voor toegang tot de eerder opgeslagen referenties op te slaan. Dit beleid echter dat gebruikers om te blijven gebruiken automatisch aanvullen voor andere soorten formuliervelden, zoals zoeken.

> [!WARNING]
> Als dit beleid is ingeschakeld, nadat gebruikers hebt gekozen voor het opslaan van sommige referenties, dit beleid wordt *niet* schakelt u de referenties die al zijn opgeslagen.
> 
> 

## <a name="step-6-testing-the-deployment"></a>Stap 6: De implementatie testen
De volgende stappen om te controleren of de implementatie van de uitbreiding geslaagd is:

1. Als u hebt geïmplementeerd met behulp van **Computerconfiguratie**, meld u aan bij een clientcomputer die deel uitmaakt van de organisatie-eenheid die u hebt geselecteerd in [stap 2: het groepsbeleidsobject maken](#step-2-create-the-group-policy-object). Als u hebt geïmplementeerd met behulp van **Gebruikersconfiguratie**, zorg ervoor dat u zich aanmelden als een gebruiker die lid is van organisatie-eenheid.
2. Het duurt een paar aanmelding modules voor het groepsbeleid wijzigt volledig bijwerken met deze machine. Als u wilt dat de update, opent u een **opdrachtprompt** venster en voer de volgende opdracht:`gpupdate /force`
3. De computer voor de installatie te kunnen uitvoeren, moet u opnieuw. Wanneer kan aanzienlijk langer duren dan normaal tijdens de uitbreiding wordt geïnstalleerd.
4. Start opnieuw op en open **Internet Explorer**. Klik in de rechterbovenhoek van het venster op **extra** (het tandwielpictogram pictogram), en selecteer vervolgens **invoegtoepassingen beheren**.
   
    ![Ga naar Extra > invoegtoepassingen beheren](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. In de **invoegtoepassingen beheren** venster controleren of de **Configuratiescherm-uitbreiding voor toegang tot** is geïnstalleerd en dat de **Status** is ingesteld op **ingeschakeld**.
   
    ![Controleer of de uitbreiding van het Configuratiescherm toegang is geïnstalleerd en is ingeschakeld.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Verwante artikelen
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [De uitbreiding van het deelvenster toegang tot het oplossen van problemen voor Internet Explorer](active-directory-saas-ie-troubleshooting.md)

