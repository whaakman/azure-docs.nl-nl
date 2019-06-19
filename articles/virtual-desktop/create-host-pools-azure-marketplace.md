---
title: Een Windows Virtual Desktop Preview host-pool maken met behulp van de Azure Marketplace - Azure
description: Het maken van een pool van de host Windows Virtual Desktop Preview met behulp van de Azure Marketplace.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: f692303140db1441aa34aacef62523d7f596dba1
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204735"
---
# <a name="tutorial-create-a-host-pool-by-using-the-azure-marketplace"></a>Zelfstudie: Een host-pool maken met behulp van de Azure Marketplace

Host-pools zijn een verzameling van een of meer identieke virtuele machines in Windows Virtual Desktop Preview tenant omgevingen. Elke groep host kan een app-groep die gebruikers werken kunnen met net zoals op een fysieke bureaublad bevatten.

In deze zelfstudie wordt beschreven hoe u een groep host in een virtuele Windows-bureaublad-tenant maken met behulp van een Microsoft Azure Marketplace-aanbieding. De taken zijn:

> [!div class="checklist"]
> * Maak een adresgroep host in een virtuele Windows-bureaublad.
> * Een resourcegroep maken met virtuele machines in een Azure-abonnement.
> * De virtuele machines toevoegen aan het Active Directory-domein.
> * Registreer de virtuele machines met virtuele Windows-bureaublad.

Voordat u begint, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="run-the-azure-marketplace-offering-to-provision-a-new-host-pool"></a>De Azure Marketplace-aanbieding voor het inrichten van een nieuwe groep van de host worden uitgevoerd

De Azure Marketplace-aanbieding voor het inrichten van een nieuwe groep van de host uitvoeren:

1. Selecteer **+** of **+ een resource maken**.
2. Voer **virtuele Windows-bureaublad** in het zoekvenster Marketplace.
3. Selecteer **Windows virtueel bureaublad - inrichten van een groep host**, en selecteer vervolgens **maken**.

Volg de instructies om in te voeren van de gegevens voor de juiste blades.

### <a name="basics"></a>Basics

Dit is wat u doet voor de **basisbeginselen** blade:

1. Voer een naam voor de host van toepassingen die uniek is binnen de tenant virtuele Windows-bureaublad.
2. Selecteer de gewenste optie voor een persoonlijke bureaublad. Als u selecteert **Ja**, elke gebruiker die verbinding met deze groep van de host maakt wordt permanent worden toegewezen aan een virtuele machine.
3. Voer een door komma's gescheiden lijst van gebruikers die zich kan aanmelden bij de virtuele Windows-bureaublad-clients en toegang tot een desktop nadat de Azure Marketplace-aanbieding is voltooid. Bijvoorbeeld, als u wilt toewijzen aan user1@contoso.com en user2@contoso.com access, voer 'user1@contoso.com,user2@contoso.com. "
4. Selecteer **nieuw** en geef een naam voor de nieuwe resourcegroep.
5. Voor **locatie**, selecteert u de dezelfde locatie als het virtuele netwerk dat verbonden met de Active Directory-server is.
6. Selecteer **OK**.

### <a name="configure-virtual-machines"></a>Virtuele machines configureren

Voor de **configureren van virtuele machines** blade:

1. Accepteer de standaardwaarden of aanpassen van het aantal en de grootte van de virtuele machines.
2. Voer een voorvoegsel voor de namen van de virtuele machines. Bijvoorbeeld, als u de naam 'voorvoegsel' invoert, wordt de virtuele machines aangeroepen "voorvoegsel-0," "voorvoegsel-1", enzovoort.
3. Selecteer **OK**.

### <a name="virtual-machine-settings"></a>Instellingen voor virtuele machines

Voor de **instellingen voor virtuele machines** blade:

>[!NOTE]
> Als u bent lid wordt van uw virtuele machines naar een Azure Active Directory Domain Services (Azure AD DS)-omgeving, controleert u of uw lid worden van domeingebruiker ook lid is van de [groep AAD DC Administrators](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-admingroup#task-3-configure-administrative-group).

1. Voor **Afbeeldingsbron**, selecteert u de bron en voer de juiste informatie kunt vinden en hoe deze worden opgeslagen. Als u ervoor geen beheerde schijven gebruiken kiest, selecteert u het opslagaccount waarin het VHD-bestand.
2. Voer de UPN-naam en het wachtwoord voor het domeinaccount waaraan de virtuele machines wordt toegevoegd aan het Active Directory-domein. Deze dezelfde gebruikersnaam en wachtwoord wordt op de virtuele machines worden gemaakt als een lokaal account. U kunt deze lokale accounts later opnieuw.
3. Selecteer het virtuele netwerk dat verbonden met de Active Directory-server is, en kies vervolgens een subnet voor het hosten van de virtuele machines.
4. Selecteer **OK**.

### <a name="windows-virtual-desktop-preview-tenant-information"></a>Informatie over Windows Virtual Desktop Preview-tenant

Voor de **informatie over virtuele Windows-bureaublad-tenant** blade:

1. Voor **virtuele Windows-bureaublad-tenantnaam groep**, voer de naam in voor de tenant-groep met uw tenant. Laat de standaardwaarde, tenzij u een specifieke zijn opgegeven groepsnaam van de tenant.
2. Voor **virtuele Windows-bureaublad-tenantnaam**, voer de naam van de tenant waar u maakt deze groep van de host.
3. Geef het type van de referenties die u wilt gebruiken om te verifiëren als de virtuele Windows-bureaublad-tenant de eigenaar van de extern bureaublad-services. Als u de [service-principals en roltoewijzingen maken met PowerShell zelfstudie](./create-service-principal-role-powershell.md), selecteer **Service-principal**. Wanneer **Azure AD-tenant-ID** wordt weergegeven, geef de ID voor de Azure Active Directory-exemplaar met de service-principal.
4. Voer de referenties voor het beheerdersaccount van de tenant. Alleen service-principals met de wachtwoordreferenties van een worden ondersteund.
5. Selecteer **OK**.

## <a name="complete-setup-and-create-the-virtual-machine"></a>Installatie is voltooid en de virtuele machine maken

Voor de laatste twee blades:

1. Op de **samenvatting** blade, Controleer de configuratie-informatie. Als u wijzigen iets wilt, gaat u terug naar de juiste blade en breng uw wijziging voordat u doorgaat. Als de informatie klopt, selecteert u **OK**.
2. Op de **kopen** blade, lees de aanvullende informatie over uw aankoop van de Azure Marketplace.
3. Selecteer **maken** implementeren van uw groep host.

Dit proces kan, afhankelijk van hoeveel virtuele machines dat u maakt, 30 minuten of langer duren.

## <a name="optional-assign-additional-users-to-the-desktop-application-group"></a>(Optioneel) Extra gebruikers aan de groep bureaubladtoepassing toewijzen

Nadat de Azure Marketplace-aanbieding is voltooid, kunt u meer gebruikers toewijzen aan de groep bureaubladtoepassing Voordat u begint met het testen van de volledige sessie bureaubladen op uw virtuele machines. Als u al gebruikers hebt toegevoegd in de Azure Marketplace-aanbieding en niet wilt toevoegen, kunt u deze sectie overslaan.

Als u wilt gebruikers toewijzen aan de groep bureaubladtoepassing, moet u eerst een PowerShell-venster te openen. Hierna moet u de volgende twee cmdlets invoeren.

Voer de volgende cmdlet om aan te melden bij de virtuele Windows-bureaublad-omgeving:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Gebruikers toevoegen aan de groep bureaubladtoepassing met behulp van deze cmdlet:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

UPN van de gebruiker moet overeenkomen met de identiteit van de gebruiker in Azure Active Directory (bijvoorbeeld user1@contoso.com). Als u wilt dat meerdere gebruikers toevoegt, moet u deze cmdlet voor elke gebruiker uitvoeren.

Nadat u deze stappen hebt voltooid, kunnen gebruikers die zijn toegevoegd aan de groep bureaubladtoepassing aanmelden bij virtuele Windows-bureaublad met ondersteunde extern bureaublad-clients en ziet u een resource voor een sessie-desktop.

Hier volgen de huidige ondersteunde clients:

- [Extern-bureaubladclient voor Windows 7 en Windows 10](connect-windows-7-and-10.md)
- [Virtuele Windows-bureaublad-webclient](connect-web.md)

>[!IMPORTANT]
>Als u wilt beveiligen uw virtuele Windows-bureaublad-omgeving in Azure, wordt aangeraden dat u binnenkomende poort 3389 op uw VM's niet openen. Virtuele Windows-bureaublad zijn vereist om een open binnenkomende poort 3389 voor gebruikers toegang krijgen tot virtuele machines van de host-pool. Als u poort 3389 voor het oplossen van problemen opent moet, raden wij aan u [just-in-time-VM-toegang](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).

## <a name="next-steps"></a>Volgende stappen

Nu dat u kunt een host hebt aangebracht, toepassingen en toegewezen gebruikers toegang tot het bureaublad, kunt u uw host-pool met RemoteApp-programma's kunt vullen. Zie voor meer informatie over het beheren van apps in een virtuele Windows-bureaublad, in deze zelfstudie:

> [!div class="nextstepaction"]
> [Zelfstudie voor app-groepen beheren](./manage-app-groups.md)
