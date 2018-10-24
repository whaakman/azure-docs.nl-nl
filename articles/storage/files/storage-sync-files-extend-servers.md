---
title: Zelfstudie uitbreiden Windows-bestandsservers met Azure File Sync | Microsoft Docs
description: Informatie over hoe op bestandsservers Windows uitbreiden met Azure File Sync, van begin tot einde.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 10/23/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 4c3ceead792f60ceac7c5eddb64dc4644d662f83
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49960440"
---
<!---Customer intent: As a IT Administrator, I want see how to extend Windows file servers with Azure File Sync, so I can evaluate the process for extending storage capacity of my Windows Servers.
--->

# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Zelfstudie: Windows-bestandsservers met Azure File Sync uitbreiden
U gebruikt Azure File Sync te centraliseren bestandsshares van uw organisatie en uit te breiden de opslagcapaciteit van een on-premises bestandsserver. Azure File Sync transformeert Windows Server naar een snelle cache van uw Azure-bestandsshare.

In deze zelfstudie leert de basisstappen voor het uitbreiden van de opslagcapaciteit van een Windows-Server met behulp van Azure File Sync. We maken gebruik van een Windows Server Azure VM voor deze zelfstudie, maar u kunt het proces doorgaans doen voor uw on-premises servers. Als u klaar bent om Azure File Sync implementeren in uw eigen omgeving, gebruikt u de [Azure File Sync implementeren](storage-sync-files-deployment-guide.md) artikel in plaats daarvan.

> [!div class="checklist"]
> * De Opslagsynchronisatieservice implementeren
> * Voorbereiden van Windows Server voor gebruik met Azure File Sync
> * De Azure File Sync-agent installeren
> * WindowsServer registreren bij de Opslagsynchronisatieservice
> * Een groep voor synchronisatie en een cloudeindpunt maken
> * Een servereindpunt maken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="prepare-your-environment-for-the-tutorial"></a>Uw omgeving voorbereiden voor de zelfstudie
Er zijn enkele dingen die u opstellen voor deze zelfstudie wilt voordat u Azure File Sync implementeren. Samen met het maken van een Azure Storage-account en het bestand delen, maakt een virtuele machine met Windows Server 2016 Datacenter en u die server voorbereiden voor Azure File Sync.

### <a name="create-a-folder-and-txt-file"></a>Maak een map en .txt-bestand

Maak op uw lokale computer, een nieuwe map met de naam *FilesToSync* en toevoegen van een tekstbestand met de naam *mytestdoc.txt*. u gaat dat bestand uploaden naar de bestandsshare later in deze zelfstudie.

### <a name="create-a-storage-account"></a>Create a storage account

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Een bestandsshare maken
Vervolgens maakt maken u een bestandsshare.

1. Wanneer de implementatie van Azure storage-account voltooid is, klikt u op **naar de resource gaan**.
1. Klik op **bestanden** in het deelvenster storage-account.

    ![Klik op bestanden](./media/storage-sync-files-extend-servers/click-files.png)

1. Klik op **+ bestandsshare**.

    ![Klik op de knop bestandsshare toevoegen](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Naam van de nieuwe bestandsshare *afsfileshare* en voer '1' voor de **quotum**, klikt u vervolgens op **maken**. Het quotum kan een maximum van 5 TiB, maar u hoeft alleen 1 GB voor deze zelfstudie.

    ![Geef een naam en de quota voor de nieuwe bestandsshare](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Selecteer de nieuwe bestandsshare en klik vervolgens op de bestandssharelocatie op **uploaden**.

    ![Bestand uploaden](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. Blader naar de *FilesToSync* map waarin u uw txt-bestand, selecteer gemaakt *mytestdoc.txt* en klikt u op **uploaden**.

    ![Bladeren-bestandsshare](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Op dit moment hebt u een Azure Storage-account en een bestandsshare gemaakt met één bestand in het in Azure. Nu maken u de Azure-VM met Windows Server 2016 Datacenter om weer te geven van de on-premises server in deze zelfstudie.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>Implementeer een virtuele machine en een gegevensschijf koppelen

1. Vervolgens, vouw het menu aan de linkerkant van de portal en kies **een resource maken** in de linkerbovenhoek van Azure portal.
1. In het zoekvak boven de lijst met **Azure Marketplace** resources, zoek en selecteer **Windows Server 2016 Datacenter**, en kies vervolgens **maken**.
1. In de **basisbeginselen** tabblad onder **projectdetails**, selecteer de resourcegroep die u voor deze zelfstudie hebt gemaakt.

   ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. Onder **exemplaar details**, geef de naam van een virtuele machine, zoals *myVM*.
1. Behoud de standaardinstellingen voor **regio**, **Beschikbaarheidsopties**, **installatiekopie**, en **grootte**.
1. Onder **Administrator-account**, bieden een **gebruikersnaam** en **wachtwoord** voor de virtuele machine.
1. Onder **Regels voor binnenkomende poort** kiest u **​​Geselecteerde poorten toestaan** en selecteert u **RDP (3389)** en **HTTP** in de vervolgkeuzelijst.

   Voordat u de virtuele machine maakt, moet u een gegevensschijf maken.

1. Klik op **volgende schijven:**

   ![Gegevensschijven toevoegen](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

1. In de **schijven** tabblad onder **schijf opties**, laat de standaardwaarden. 
1. Onder **GEGEVENSSCHIJVEN**, klikt u op **maken en koppelen van een nieuwe schijf**.

1. De standaardinstellingen laten staan, met uitzondering van wijziging de **grootte (GiB)** naar **1 GB** voor deze zelfstudie.

   ![Details van de gegevens-schijf](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

1. Klik op **OK**.
1. Klik op **Controleren + maken**.
1. Klik op **Create**.

   U kunt klikken op de **meldingen** pictogram om te kijken de **voortgang van implementatie**. Het maken van een nieuwe virtuele machine duurt een paar minuten om te voltooien.

1. Nadat uw VM-implementatie voltooid is, klikt u op **naar de resource gaan**.

   ![Ga naar resource](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

   Op dit moment hebt u een nieuwe virtuele machine gemaakt en een gegevensschijf die is gekoppeld. Nu moet u verbinding maken met de virtuele machine.

### <a name="connect-to-your-vm"></a>Verbinding maken met uw virtuele machine

1. Klik in de Azure-portal op **Connect** op de eigenschappenpagina van de virtuele machine.

   ![Verbinding maken met een Azure VM vanaf de portal](./media/storage-sync-files-extend-servers/connect-vm.png)

1. In de **verbinding maken met virtuele machine** pagina, bewaart de standaardopties verbinding maken met **IP-adres** meer dan poort 3389 en op **downloaden RDP-bestand**.

   ![Het RDP-bestand downloaden](./media/storage-sync-files-extend-servers/download-rdp.png)

1. Open het gedownloade RDP-bestand en klik op **Verbinden** wanneer dit wordt gevraagd.
1. Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Typ de gebruikersnaam als *localhost\username*, voert u het wachtwoord die u hebt gemaakt voor de virtuele machine en klik vervolgens op **OK**.

   ![Meer opties](./media/storage-sync-files-extend-servers/local-host2.png)

1. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om de verbinding te maken.

### <a name="prepare-the-windows-server"></a>De WindowsServer voorbereiden
Voor de **Windows Server 2016 Datacenter** -server uitschakelen **verbeterde beveiliging van Internet Explorer**. Deze stap is alleen vereist voor registratie van de oorspronkelijke server. U kunt deze opnieuw inschakelen nadat de server is geregistreerd.

In de **Windows Server 2016 Datacenter** VM, **Serverbeheer** wordt automatisch geopend.  Als **Serverbeheer** niet standaard, zoek het in Explorer wordt geopend.

1. In **Serverbeheer** klikt u op **lokale Server**.

   !['Lokale Server' aan de linkerkant van de Serverbeheer-UI](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. Op de **eigenschappen** subpane, selecteert u de koppeling voor **verbeterde beveiliging van Internet Explorer**.  

    ![Het deelvenster 'Internet Explorer Verbeterde beveiliging' in de Serverbeheer-UI](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. In de **verbeterde beveiliging van Internet Explorer** in het dialoogvenster, selecteer **uit** voor **beheerders** en **gebruikers**:  

    ![De verbeterde beveiliging van Internet Explorer-pop-upvenster met 'Uit' geselecteerd](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

   U kunt nu de gegevensschijf toevoegen aan de virtuele machine.

### <a name="add-the-data-disk"></a>De gegevensschijf toevoegen

1. Terwijl u nog steeds in de **Windows Server 2016 Datacenter** VM, klikt u op **bestanden- en opslagservices** > **Volumes** > **schijven** .

    ![Gegevensschijf](media/storage-sync-files-extend-servers/your-disk.png)

1. Met de rechtermuisknop op de schijf van 1 GB met de naam **Msft virtuele schijf** en klikt u op **nieuw volume**.
1. Voltooi de wizard verlaten van de standaardinstellingen aanwezig is, waarbij de toegewezen stationsletter, en klikt u op **maken**.
1. Klik op **Sluiten**.

   Op dit moment hebt u de schijf online gebracht en een volume gemaakt. U kunt controleren of de gegevensschijf toevoegen voltooid is door de Verkenner op de virtuele machine te openen en te bevestigen dat het nieuwe station aanwezig is.

1. Vouw in de Verkenner op de virtuele machine, **deze PC** en dubbelklikt u op het nieuwe station. Dit is het station F: in dit voorbeeld.
1. Met de rechtermuisknop op en selecteer **nieuw** > **map**. Naam van de map *FilesToSync*.
1. Dubbelklik op de **FilesToSync** map.
1. Met de rechtermuisknop op en selecteer **nieuw** > **tekstdocument**. Naam van het tekstbestand *MyTestFile*.

    ![Voeg een nieuw tekstbestand](media/storage-sync-files-extend-servers/new-file.png)

1. Sluiten **Explorer** en **Serverbeheer**.

### <a name="download-the-azurerm-powershell-module"></a>De AzureRM PowerShell-module downloaden
Vervolgens in de **Windows Server 2016 Datacenter** VM, installeer de **AzureRM PowerShell-module** op de server.

1. Open een verhoogde PowerShell-venster in de virtuele machine
1. Voer de volgende opdracht uit:

   ```powershell
   Install-Module -Name AzureRM -AllowClobber
   ```

   > [!NOTE]
   > Als u een versie van NuGet hebt die ouder is dan 2.8.5.201, wordt u gevraagd om de meest recente versie van NuGet te downloaden en installeren.

   PowerShell Gallery is standaard niet geconfigureerd als een vertrouwde opslagplaats voor PowerShellGet. De eerste keer dat u PSGallery gebruikt, ziet u het volgende bericht:

   ```output
   Untrusted repository

   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.

   Are you sure you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
   ```

1. Antwoord `Yes` of `Yes to All` om door te gaan met de installatie.

De `AzureRM`-module is een updatepakketmodule voor de Azure PowerShell-cmdlets. Installatie van deze downloads van alle beschikbare Azure Resource Manager-modules en hun cmdlets beschikbaar voor gebruik.

Op dit moment u klaar bent met instellen van uw omgeving voor de zelfstudie en u bent klaar om te beginnen met het implementeren van de **Opslagsynchronisatieservice**.

## <a name="deploy-the-storage-sync-service"></a>De Opslagsynchronisatieservice implementeren 
Azure File Sync implementeren begint met het plaatsen van een **Opslagsynchronisatieservice** resource in een resourcegroep voor het geselecteerde abonnement. De Opslagsynchronisatieservice neemt over machtigingen voor toegang van het abonnement en resourcegroep die u implementeert deze in.

1. Klik in de Azure-portal op **een resource maken** en zoek vervolgens **Azure File Sync**.
1. Selecteer in de lijst met zoekresultaten **Azure File Sync**.
1. Selecteer **maken** openen de **Opslagsynchronisatieservice implementeren** tabblad.

   ![Opslagsynchronisatie implementeren](media/storage-sync-files-extend-servers/afs-info.png)

   Voer in het deelvenster dat wordt geopend, de volgende informatie:

   | Waarde | Beschrijving |
   | ----- | ----- |
   | **Naam** | Een unieke naam (per abonnement) voor de Opslagsynchronisatieservice.<br><br>In deze zelfstudie gebruiken we *afssyncservice02*. |
   | **Abonnement** | Het abonnement dat u voor deze zelfstudie. |
   | **Resourcegroep** | De resourcegroep die u voor deze zelfstudie.<br><br>In deze zelfstudie gebruiken we *afsresgroup101918*. |
   | **Locatie** | US - oost |

1. Wanneer u klaar bent, selecteert u **maken** implementeren de **Opslagsynchronisatieservice**.
1. Klik op de **meldingen** tabblad > **naar de resource gaan**.

## <a name="install-the-azure-file-sync-agent"></a>De Azure File Sync-agent installeren
De Azure File Sync-agent is een downloadbare pakket waarmee Windows-Server kan worden gesynchroniseerd met een Azure-bestandsshare.

1. Ga terug naar de **Windows Server 2016 Datacenter** virtuele machine en open **Internet Explorer**
1. Ga naar de [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Schuif omlaag naar de **Azure File Sync-Agent** sectie en klikt u op **downloaden**.

   ![Synchronisatie-agent downloaden](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. Schakel het selectievakje voor **StorageSyncAgent_V3_WS2016. EXE** en klikt u op **volgende**.

   ![Selecteer agent](media/storage-sync-files-extend-servers/select-agent.png)

1. **Eenmaal toestaan** > **uitvoeren** > **Open** het bestand.
1. Als u dit nog niet hebt gedaan, sluit u het PowerShell-venster.
1. Accepteer de standaardwaarden in de **opslag Sync-Agent-installatiewizard**.
1. Klik op **Install**.
1. Klik op **Voltooien**.

U hebt geïmplementeerd met de Azure-Sync-Service en de agent hebt geïnstalleerd op de **Windows Server 2016 Datacenter** VM. Nu moet u registreren van de virtuele machine met de **Opslagsynchronisatieservice**.

## <a name="register-windows-server-with-storage-sync-service"></a>WindowsServer registreren bij de Opslagsynchronisatieservice
Uw Windows-Server registreren met een Opslagsynchronisatieservice brengt een vertrouwensrelatie tussen uw server (of cluster) en de Opslagsynchronisatieservice. Een server kan slechts aan één Opslagsynchronisatieservice worden geregistreerd en kunnen worden gesynchroniseerd met andere servers en Azure-bestandsshares die zijn gekoppeld aan de dezelfde Opslagsynchronisatieservice.

De gebruikersinterface van de registratie van Server wordt automatisch geopend na de installatie van de **Azure File Sync-agent**. Als dat niet het geval is, kunt u het handmatig openen vanuit de bestandslocatie: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.

1. Wanneer de gebruikersinterface van de registratie van Server wordt geopend in de virtuele machine, klikt u op **OK**.
1. Klik op **aanmelden** om te beginnen.
1. Meld u aan met de referenties van uw Azure-account en klikt u op **aanmelden**.
1. Geef de volgende informatie op:

   ![Een schermopname van de gebruikersinterface van de registratie van Server](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Waarde | Beschrijving |
   | **Azure-abonnement** | Het abonnement met de Opslagsynchronisatieservice voor deze zelfstudie. |
   | **Resourcegroep** | De resourcegroep die de Opslagsynchronisatieservice voor deze zelfstudie bevat. We hebben het gebruikt *afsresgroup101918* in deze zelfstudie. |
   | **Opslagsynchronisatieservice** | De naam van de Opslagsynchronisatieservice die u voor deze zelfstudie hebt gebruikt. We hebben het gebruikt *afssyncservice02* in deze zelfstudie. |

1. Klik op **registreren** om de serverregistratie te voltooien.
1. Als onderdeel van het registratieproces, wordt u gevraagd voor een aanvullende aanmelding. Meld u aan en klikt u op **volgende**.
1. Klik op **OK**

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Een groep voor synchronisatie en een cloudeindpunt maken
Een synchronisatiegroep definieert de synchronisatie-topologie voor een set bestanden. Een groep voor synchronisatie moet één cloudeindpunt, waarmee een Azure-bestandsshare en een of meer servereindpunten bevatten. Een servereindpunt vertegenwoordigt een pad op een geregistreerde server.

1. Maken van een synchronisatiegroep in de [Azure-portal](https://portal.azure.com/), selecteer **+ groep voor synchronisatie met** van de Opslagsynchronisatieservice die u voor deze zelfstudie hebt gemaakt. We hebben gebruikt *afssyncservice02* als in het voorbeeld in deze zelfstudie.

   ![Maak een nieuwe synchronisatiegroep in Azure portal](media/storage-sync-files-extend-servers/add-sync-group.png)

1. Voer de volgende informatie voor het maken van een groep voor synchronisatie met een cloudeindpunt in het deelvenster dat wordt geopend:

   | Waarde | Beschrijving |
   | ----- | ----- |
   | **Naam van synchronisatiegroep** | Deze naam kan moet uniek zijn binnen de Opslagsynchronisatieservice, maar elke naam zijn die logisch is voor u. In deze zelfstudie gebruiken we *afssyncgroup*.|
   | **Abonnement** | Het abonnement waar u de Opslagsynchronisatieservice voor deze zelfstudie hebt geïmplementeerd. |
   | **Opslagaccount** |Klik op **storage-account selecteren**. In het deelvenster dat wordt weergegeven, selecteert u het opslagaccount met de Azure-bestandsshare die u voor deze zelfstudie hebt gemaakt. We hebben gebruikt *afsstoracct101918*. |
   | **Azure-bestandsshare** | De naam van de Azure-bestandsshare die u voor deze zelfstudie hebt gemaakt. We hebben gebruikt *afsfileshare*. |

1. Klik op **Create**.

Als u de synchronisatiegroep selecteert, kunt u zien dat u nu een hebt **cloud-eindpunt**.

## <a name="add-a-server-endpoint"></a>Een servereindpunt toevoegen
Een servereindpunt vertegenwoordigt een specifieke locatie op een geregistreerde server, bijvoorbeeld een map op een volume van de server.

1. Een servereindpunt toevoegen, selecteert u de zojuist gemaakte synchronisatiegroep en selecteer vervolgens **servereindpunt toevoegen**.

   ![Een nieuw servereindpunt toevoegen in het deelvenster van de groep synchroniseren](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. In de **servereindpunt toevoegen** deelvenster, voer de volgende informatie voor het maken van een servereindpunt:

   | | |
   | ----- | ----- |
   | Waarde | Beschrijving |
   | **Geregistreerde server** | De naam van de server die u voor deze zelfstudie hebt gemaakt. We hebben gebruikt *afsvm101918* in deze zelfstudie ||
   | **Pad** | Het Windows Server-pad naar het station dat u voor deze zelfstudie hebt gemaakt. In ons voorbeeld is het *f:\filestosync*. |
   | **Cloud-Opslaglagen** | Laat de eigenschap uitgeschakeld voor deze zelfstudie. |
   | **Beschikbare volumeruimte** | Laat leeg voor deze zelfstudie. |

1. Klik op **Create**.

Uw bestanden worden nu gesynchroniseerd gehouden in uw Azure-bestandsshare en Windows Server.

![Azure Storage zijn gesynchroniseerd](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd de eenvoudige stappen om uit te breiden de opslagcapaciteit van een Windows-Server met behulp van Azure File Sync. Volg deze koppeling voor een uitgebreider overzicht van de planning voor de implementatie van een Azure File Sync.

> [!div class="nextstepaction"]
> [Azure File Sync-implementatie plannen](./storage-sync-files-planning.md)