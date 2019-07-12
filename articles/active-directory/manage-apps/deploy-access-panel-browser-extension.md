---
title: Deelvenster-extensie voor Azure-toegang voor Internet Explorer met behulp van een groepsbeleidsobject implementeren | Microsoft Docs
description: Het gebruik van Groepsbeleid voor het implementeren van de Internet Explorer-invoegtoepassing voor de portal mijn Apps.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/08/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71c342ede77349b3f6c22093e5877ad5f5ce6549
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807688"
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Procedure: De extensie van het toegangsvenster voor Internet Explorer met behulp van Groepsbeleid implementeren

Deze zelfstudie laat zien hoe u kunt Groepsbeleid gebruiken om de extensie van het toegangsvenster voor Internet Explorer op afstand installeren op uw gebruikers virtuele machines. Deze uitbreiding is vereist voor Internet Explorer-gebruikers die u moeten zich aanmelden bij apps die zijn geconfigureerd met behulp van [wachtwoord gebaseerde eenmalige aanmelding](what-is-single-sign-on.md#password-based-sso).

Het verdient aanbeveling dat beheerders de implementatie van deze extensie wilt automatiseren. Gebruikers hebben anders downloaden en installeren van de extensie zelf, dat is kwetsbaar voor en zijn beheerdersmachtigingen vereist. Deze zelfstudie bestaat uit één methode voor het software-implementaties automatiseren met behulp van Groepsbeleid. [Meer informatie over Groepsbeleid.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

De extensie van het toegangsvenster is ook beschikbaar voor [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) en [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), geen van beide vereist beheerdersmachtigingen om te installeren.

## <a name="prerequisites"></a>Vereisten

* U hebt ingesteld [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), en u hebt uw gebruikers machines toegevoegd aan uw domein.
* U moet de machtiging 'Instellingen bewerken' voor het bewerken van het groepsbeleidsobject (GPO) hebben. Standaard hebben leden van de volgende beveiligingsgroepen deze machtiging: Domeinadministrators, Ondernemingsadministrators en beveiligingsgroep Maker Eigenaar Groepsbeleid. [Meer informatie.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a>Stap 1: Maken van het distributiepunt.

Eerst moet u het installer-pakket plaatsen op een netwerklocatie die toegankelijk is voor de machines die u de extensie wilt op afstand te installeren. Voer de volgende stappen uit om dit te doen:

1. Meld u aan bij de server als beheerder.
1. In de **Serverbeheer** venster, Ga naar **bestanden- en opslagservices**.

    ![Geopende bestanden en Storage-Services](./media/deploy-access-panel-browser-extension/files-services.png)

1. Ga naar de **Shares** tabblad. Klik vervolgens op **taken** > **nieuwe Share...**

    ![Schermafbeelding ziet u waar u kunt nieuwe Share te vinden in het venster taken](./media/deploy-access-panel-browser-extension/shares.png)

1. Voltooi de **Wizard Nieuwe Share** en instellen van machtigingen om ervoor te zorgen dat deze van uw gebruikers-machines kan worden geopend. [Meer informatie over de shares.](https://technet.microsoft.com/library/cc753175.aspx)
1. Download het volgende pakket voor Microsoft Windows Installer (MSI-bestand): [Toegang tot deelvenster Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)
1. Kopieer het installatiepakket op de gewenste locatie op de share.

    ![Het MSI-bestand kopiëren naar de share](./media/deploy-access-panel-browser-extension/copy-package.png)

1. Controleer of uw clientcomputers toegang hebben tot het installatiepakket van de share.

## <a name="step-2-create-the-group-policy-object"></a>Stap 2: De group policy object maken

1. Aanmelden bij de server die als host fungeert voor de installatie van Active Directory Domain Services (AD DS).
1. In Serverbeheer, gaat u naar **extra** > **Group Policy Management**.

    ![Ga naar Extra > Groepsbeleidsbeheer](./media/deploy-access-panel-browser-extension/tools-gpm.png)

1. In het linkerdeelvenster van de **Group Policy Management** venster weergeven van uw hiërarchie van organisatie-eenheid (OE) en te bepalen bij welke bereik wilt u het groepsbeleid van toepassing. Bijvoorbeeld, u besluiten om te kiezen van een kleine organisatie-eenheid om te implementeren op een paar gebruikers voor het testen of kiest u een op het hoogste niveau organisatie-eenheid om te implementeren voor uw hele organisatie.

   > [!NOTE]
   > Als u wilt maken of bewerken van uw organisatie-eenheden (OU's), Ga terug naar de Server Manager en gaat u naar **extra** > **Active Directory: gebruikers en Computers**.

1. Nadat u een organisatie-eenheid hebt geselecteerd, met de rechtermuisknop en selecteer **een groepsbeleidsobject in dit domein maken en hier een koppeling...**

    ![Schermafbeelding ziet u de maken een nieuwe GPO-optie](./media/deploy-access-panel-browser-extension/create-gpo.png)

1. In de **nieuw groepsbeleidsobject** prompt, typ een naam voor de nieuwe Group Policy Object.
1. Met de rechtermuisknop op het groepsbeleidsobject dat u maakte en selecteer **bewerken**.

## <a name="step-3-assign-the-installation-package"></a>Stap 3: Het installatiepakket toewijzen

1. Bepalen of u implementeren de extensie wilt op basis van **Computerconfiguratie** of **Gebruikersconfiguratie**. Bij het gebruik van [Computerconfiguratie](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), de extensie wordt geïnstalleerd op de computer, ongeacht welke gebruikers zich aanmelden bij deze. Met [Gebruikersconfiguratie](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), gebruikers hebben de extensie voor hen, ongeacht welke computers ze zich op om te aanmelden worden geïnstalleerd.
1. In het linkerdeelvenster van de **Editor voor Groepsbeleidsbeheer** venster, Ga naar een van de volgende mappaden, afhankelijk van welk type configuratie die u hebt gekozen:

   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`

1. Met de rechtermuisknop op **Software-installatie**en selecteer vervolgens **nieuw** > **pakket...**
1. Ga naar de gedeelde map met het installatiepakket uit [stap 1: Maken van het distributiepunt](#step-1-create-the-distribution-point), selecteert u het MSI-bestand en klik op **Open**.

   > [!IMPORTANT]
   > Als de share zich op deze server bevindt, controleert u of dat u het MSI-bestand via het bestandspad van het netwerk, in plaats van het lokale bestandspad openen wilt.

    ![Selecteer het installatiepakket van de gedeelde map](./media/deploy-access-panel-browser-extension/select-package.png)

1. In de **Software implementeren** vragen, selecteer **toegewezen** voor uw implementatie. Klik vervolgens op **OK**.

De extensie is nu geïmplementeerd in de organisatie-eenheid die u hebt geselecteerd. [Meer informatie over Software-installatie in Groepsbeleid.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Stap 4: Automatisch inschakelen de extensie voor Internet Explorer

Naast het installatieprogramma uitvoert, moet elke uitbreiding voor Internet Explorer ingeschakeld zijn voordat deze kan worden gebruikt. Volg de stappen hieronder om het deelvenster-extensie voor toegang met behulp van Groepsbeleid inschakelen:

1. In de **Editor voor Groepsbeleidsbeheer** venster, Ga naar een van de volgende paden, afhankelijk van welk type configuratie u hebt gekozen in [stap 3: Toewijzen van het installatiepakket](#step-3-assign-the-installation-package):

   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

1. Met de rechtermuisknop op **lijst met invoegtoepassingen**, en selecteer **bewerken**.

    ![Klik met de rechtermuisknop 'Lijst met invoegtoepassingen' en selecteer 'Bewerken'](./media/deploy-access-panel-browser-extension/edit-add-on-list.png)

1. In de **lijst met invoegtoepassingen** venster **ingeschakeld**. Klik vervolgens onder de **opties** sectie, klikt u op **weergeven...** .

    ![Klik op inschakelen en klik vervolgens op weergeven...](./media/deploy-access-panel-browser-extension/edit-add-on-list-window.png)

1. In de **inhoud weergeven** venster de volgende stappen uitvoeren:

   1. Voor de eerste kolom (de **waardenaam** veld), kopieer en plak de volgende klasse-ID: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   1. Voor de tweede kolom (de **waarde** veld), typt u de volgende waarde: `1`
   1. Klik op **OK** sluiten de **inhoud weergeven** venster.

      ![Vul de waarden die zijn opgegeven in de vorige stap](./media/deploy-access-panel-browser-extension/show-contents.png)

1. Klik op **OK** aan uw wijzigingen toe te passen op en sluit de **lijst met invoegtoepassingen** venster.

De extensie moet nu worden ingeschakeld voor de machines in de geselecteerde organisatie-eenheid. [Meer informatie over het gebruik van Groepsbeleid inschakelen of uitschakelen van invoegtoepassingen voor Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>Stap 5 (optioneel): 'Wachtwoord onthouden' prompt uitschakelen

Wanneer gebruikers zich aanmelden met behulp van de uitbreiding van het deelvenster toegang tot websites, kan Internet Explorer het volgende bericht waarin wordt gevraagd 'Wilt u dat voor het opslaan van uw wachtwoord?' weergeven

![Geeft de 'Wilt u voor het opslaan van uw wachtwoord...' prompt](./media/deploy-access-panel-browser-extension/remember-password-prompt.png)

Als u voorkomen dat uw gebruikers zien deze prompt wilt, klikt u vervolgens de volgende stappen om te voorkomen dat automatisch aanvullen van onthouden wachtwoorden:

1. In de **Editor voor Groepsbeleidsbeheer** venster, Ga naar het onderstaande pad. Deze configuratie-instelling is alleen beschikbaar onder **Gebruikersconfiguratie**.

   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
1. Ga naar de instelling met de naam **Schakel de functie automatisch aanvullen voor gebruikersnamen en wachtwoorden op formulieren**.

   > [!NOTE]
   > Eerdere versies van Active Directory kunnen deze instelling met de naam van de lijst **staan niet automatisch aanvullen wachtwoorden op te slaan**. De configuratie voor de betreffende instelling verschilt van de instelling die wordt beschreven in deze zelfstudie.

    ![Vergeet niet om te zoeken naar dit onder gebruikersinstellingen](./media/deploy-access-panel-browser-extension/disable-auto-complete.png)

1. De instelling van de bovenstaande Klik met de rechtermuisknop en selecteer **bewerken**.
1. In het venster met de titel **Schakel de functie automatisch aanvullen voor gebruikersnamen en wachtwoorden op formulieren**, selecteer **uitgeschakelde**.

    ![Selecteer de "Uitgeschakeld" optie voor het inschakelen van de functie automatisch aanvullen](./media/deploy-access-panel-browser-extension/disable-passwords.png)

1. Klik op **OK** deze wijzigingen toepassen en sluit het venster.

Gebruikers wordt niet langer hun referenties of gebruik automatisch aanvullen voor toegang tot de eerder opgeslagen referenties op te slaan. Dit beleid staat echter toe dat gebruikers om door te gaan met automatisch aanvullen voor andere typen velden, zoals zoekvelden.

> [!WARNING]
> Als dit beleid wordt ingeschakeld nadat gebruikers hebt gekozen voor het opslaan van sommige referenties, dit beleid wordt *niet* schakelt u de referenties die al zijn opgeslagen.

## <a name="step-6-testing-the-deployment"></a>Stap 6: De implementatie testen

Volg de stappen hieronder om te controleren of als de implementatie van de uitbreiding geslaagd is:

1. Als u hebt geïmplementeerd met behulp van **Computerconfiguratie**, meld u aan bij een clientcomputer die deel uitmaakt van de organisatie-eenheid die u hebt geselecteerd in [stap 2: Maken van de Group Policy Object](#step-2-create-the-group-policy-object). Als u hebt geïmplementeerd met behulp van **Gebruikersconfiguratie**, zorg ervoor dat u zich aanmeldt als een gebruiker die lid is van organisatie-eenheid.
1. Het duurt een paar aanmeldingen betreft voor de groep beleidswijzigingen volledig bijwerken met deze machine. Als u wilt afdwingen dat de update, opent u een **opdrachtprompt** venster en voer de volgende opdracht: `gpupdate /force`
1. U moet de computer voor de installatie kan plaatsvinden opnieuw opstarten. Opstartstatus kan aanzienlijk langer duren dan normaal tijdens de extensie wordt geïnstalleerd.
1. Na het opnieuw opstarten, open **Internet Explorer**. Klik in de rechterbovenhoek van het venster op **extra** (het tandwielpictogram), en selecteer vervolgens **invoegtoepassingen beheren**.
1. In de **invoegtoepassingen beheren** venster, Controleer de **Configuratiescherm-extensie voor toegang** is geïnstalleerd en dat de **Status** is ingesteld op **ingeschakeld**.

   ![Controleer of de uitbreiding van het deelvenster toegang tot is geïnstalleerd en ingeschakeld](./media/deploy-access-panel-browser-extension/verify-install.png)

## <a name="learn-more"></a>Meer informatie

* [Toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](what-is-single-sign-on.md)
* [Het oplossen van het Configuratiescherm-extensie voor toegang voor Internet Explorer](manage-access-panel-browser-extension.md)
