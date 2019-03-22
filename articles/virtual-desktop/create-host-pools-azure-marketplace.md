---
title: Een host-pool maken met Azure Marketplace (preview) - Azure
description: Hoe u een virtuele Windows-bureaublad host-pool maken met Azure Marketplace.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: a1d172f219580b390ba4855928d6f6a68c8269b9
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318111"
---
# <a name="tutorial-create-a-host-pool-with-azure-marketplace-preview"></a>Zelfstudie: Een host-pool maken met Azure Marketplace (Preview)

Host-pools zijn een verzameling van een of meer identieke virtuele machines in virtuele Windows-bureaublad-tenant (preview)-omgevingen. Elke groep host kan een app-groep die gebruikers werken kunnen met net zoals op een fysieke bureaublad bevatten.

In dit artikel wordt beschreven hoe u een host-toepassingen in een virtuele Windows-bureaublad-tenant met behulp van een Microsoft Azure Marketplace-aanbieding te maken. Dit omvat het maken van een groep host in Windows virtuele bureaublad, het maken van een resourcegroep met de virtuele machines in een Azure-abonnement, deze virtuele machines toevoegen aan het Active Directory-domein en het registreren van de virtuele machines met virtuele Windows-bureaublad.

Voordat u begint, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op <https://portal.azure.com>.

## <a name="run-the-azure-marketplace-offering-to-provision-a-new-host-pool"></a>De Azure Marketplace-aanbieding voor het inrichten van een nieuwe groep van de host worden uitgevoerd

De Azure Marketplace-aanbieding voor het inrichten van een nieuwe groep van de host uitvoeren:

1. Selecteer **+** of **+ een resource maken**.
2. Voer **virtuele Windows-bureaublad** in het zoekvenster Marketplace.
3. Selecteer **Windows virtueel bureaublad - inrichten van een groep host**en selecteer vervolgens **maken**.

Volg de instructies om in te voeren van de gegevens voor de juiste blades.

### <a name="basics"></a>Basisbeginselen

Dit is wat u voor de blade grondbeginselen doen:

1. Voer een naam voor de host van toepassingen die uniek is binnen de tenant virtuele Windows-bureaublad.
2. Selecteer de gewenste optie voor persoonlijke bureaublad. Als u selecteert **Ja**, elke gebruiker die verbinding met deze groep van de host maakt wordt permanent worden toegewezen aan een virtuele machine.
3. Voer een door komma's gescheiden lijst van gebruikers die zich kan aanmelden bij de virtuele Windows-bureaublad-clients en toegang tot een bureaublad nadat de Azure Marketplace-aanbieding is voltooid. Bijvoorbeeld, als u wilt toewijzen user1@contoso.com en user2@contoso.com access, voer 'user1@contoso.com,user2@contoso.com. "
4. Selecteer **nieuw** en geef een naam voor de nieuwe resourcegroep.
5. Voor **locatie**, selecteert u de dezelfde locatie als het virtuele netwerk dat verbonden met de Active Directory-server is.
6. Selecteer **OK**.

### <a name="configure-virtual-machines"></a>Virtuele machines configureren

Voor de blade van het virtuele machines configureren:

1. Accepteer de standaardwaarden of aanpassen van het aantal en de grootte van de virtuele machines.
2. Voer een voorvoegsel voor de namen van de virtuele machines. Bijvoorbeeld, als u de naam 'voorvoegsel' invoert, wordt de virtuele machines aangeroepen "voorvoegsel-0," "voorvoegsel-1", enzovoort.
3. Selecteer **OK**.

### <a name="virtual-machine-settings"></a>Instellingen voor virtuele machines

Voor de virtuele machineblade:

1. Selecteer de **Afbeeldingsbron** en voer de juiste informatie kunt vinden en hoe deze worden opgeslagen. Als u ervoor geen beheerde schijven gebruiken kiest, selecteert u het opslagaccount met het VHD-bestand.
2. Voer de UPN-naam en het wachtwoord voor het domeinaccount waaraan de virtuele machines wordt toegevoegd aan het Active Directory-domein. Deze dezelfde gebruikersnaam en wachtwoord wordt op de virtuele machines worden gemaakt als een lokaal account. U kunt deze lokale accounts later opnieuw.
3. Selecteer het virtuele netwerk dat is verbonden met de Active Directory-server, en kies vervolgens een subnet voor het hosten van de virtuele machines.
4. Selecteer **OK**.

### <a name="windows-virtual-desktop-tenant-information"></a>Informatie over virtuele Windows-bureaublad-tenant

Voor de blade van de informatie in de virtuele Windows-bureaublad-tenant:

1. Voer de **virtuele Windows-bureaublad-tenantnaam groep** voor de tenant-groep met uw tenant. Als u geen naam van een specifieke tenant gepland, laat u het als standaardwaarde.
2. Voer de **virtuele Windows-bureaublad-tenantnaam** voor de tenant maakt u deze groep host in.
3. Geef het type van de referenties die u wilt gebruiken om te verifiëren als de virtuele Windows-bureaublad-tenant de eigenaar van de extern bureaublad-services. Als u selecteert **Service-principal**, moet u ook opgeven de **Azure AD-tenant-ID** die zijn gekoppeld aan de service-principal.
4. Voer een van beide de referenties voor het beheerdersaccount van de tenant. Alleen service-principals met de wachtwoordreferenties van een worden ondersteund.
5. Selecteer **OK**.

## <a name="complete-setup-and-create-the-virtual-machine"></a>Installatie is voltooid en de virtuele machine maken

Voor de laatste twee blades:

1. In de **samenvatting** blade, Controleer de configuratie-informatie. Als u wijzigen iets wilt, gaat u terug naar de juiste blade en breng uw wijziging voordat u doorgaat. Als de informatie klopt, selecteert u **OK**.
2. In de **kopen** blade, lees de aanvullende informatie over uw aankoop van Azure Marketplace.
3. Selecteer **maken** implementeren van uw groep host.

Dit proces kan, afhankelijk van hoeveel virtuele machines dat u maakt, 30 minuten of langer duren.

## <a name="optional-assign-additional-users-to-the-desktop-application-group"></a>(Optioneel) Extra gebruikers aan de groep bureaubladtoepassing toewijzen

Nadat de Azure Marketplace-aanbieding is voltooid, kunt u extra gebruikers toewijzen aan de groep bureaubladtoepassing Voordat u begint met het testen van de volledige sessie bureaubladen op uw virtuele machines. Als u al gebruikers hebt toegevoegd in de Azure Marketplace-aanbieding en niet wilt toevoegen, kunt u deze sectie overslaan.

Als u wilt gebruikers toewijzen aan de groep bureaubladtoepassing, moet u eerst een PowerShell-venster te openen. Hierna moet u de volgende twee cmdlets invoeren.

Voer de volgende cmdlet om aan te melden bij de virtuele Windows-bureaublad-omgeving:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Stelt de context op de virtuele Windows-bureaublad tenant-groep die u hebt opgegeven in de Azure Marketplace bieden met de volgende cmdlet. Als u de tenant virtuele Windows-bureaublad waarde als de standaardwaarde in de Azure Marketplace-aanbieding, kunt u deze stap overslaan.

```powershell
Set-RdsContext -TenantGroupName <tenantgroupname>
```

Nadat u hebt deze twee dingen gedaan, kunt u gebruikers kunt toevoegen aan de groep bureaubladtoepassing met deze cmdlet:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

UPN van de gebruiker moet overeenkomen met de identiteit van de gebruiker in Azure Active Directory (bijvoorbeeld user1@contoso.com). Als u wilt dat meerdere gebruikers toevoegt, moet u deze cmdlet voor elke gebruiker uitvoeren.

Nadat u deze stappen hebt voltooid, kunnen gebruikers die zijn toegevoegd aan de groep bureaubladtoepassing aanmelden bij virtuele Windows-bureaublad met ondersteunde extern bureaublad-clients en ziet u een resource voor een sessie-desktop.

Hier volgen de huidige ondersteunde clients:

- [Extern-bureaubladclient voor Windows 7 en Windows 10](connect-windows-7-and-10.md)
- [Virtuele Windows-bureaublad-webclient](connect-web.md)

## <a name="next-steps"></a>Volgende stappen

Nu dat u kunt een host hebt aangebracht, toepassingen en toegewezen gebruikers toegang tot het bureaublad, kunt u ook uw host-pool met RemoteApps invullen. Zie voor meer informatie over het beheren van apps in een virtuele Windows-bureaublad, de zelfstudie voor groepen beheren-Apps.

> [!div class="nextstepaction"]
> [Zelfstudie voor app-groepen beheren](./manage-app-groups.md)
