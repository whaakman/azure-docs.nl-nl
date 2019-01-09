---
title: 'Zelfstudie: Windows-bestandsservers uitbreiden met Azure File Sync | Microsoft Docs'
description: Leer hoe u Windows-bestandsservers kunt uitbreiden met Azure File Sync, van begin tot eind.
services: storage
author: wmgries
ms.service: storage
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 3ebf450f4e84fed572307a18f20f36013e32c7a5
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630696"
---
# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Zelfstudie: Windows-bestandsservers uitbreiden met Azure File Sync
In deze zelfstudie worden de basisstappen getoond voor het uitbreiden van de opslagcapaciteit van een Windows-server met Azure File Sync. Hoewel in deze zelfstudie een Windows Server-VM wordt gebruikt, voert u dit proces gewoonlijk uit voor uw on-premises servers. Als u klaar bent om Azure File Sync in uw eigen omgeving te implementeren, lees dan het artikel [Azure File Sync implementeren](storage-sync-files-deployment-guide.md).

> [!div class="checklist"]
> * Opslagsynchronisatieservice implementeren
> * Windows Server voorbereiden voor gebruik met Azure File Sync
> * Azure File Sync-agent installeren
> * Windows Server registreren met opslagsynchronisatieservice
> * Synchronisatiegroep en cloudeindpunt maken
> * Servereindpunt maken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="prepare-your-environment"></a>Uw omgeving voorbereiden
Voor deze zelfstudie dient u een paar dingen in te stellen voordat u Azure File Sync implementeert. Samen met een Azure Storage-account en een bestandsshare maakt u een Windows Server 2016 Datacenter-VM en bereidt u die server voor op Azure File Sync.

### <a name="create-a-folder-and-txt-file"></a>Map en TXT-bestand maken

Maak op de lokale computer een nieuwe map met de naam *FilesToSync* en voeg het tekstbestand *mytestdoc.txt* toe. Later in deze zelfstudie uploadt u dat bestand naar de bestandsshare.

### <a name="create-a-storage-account"></a>Create a storage account

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Een bestandsshare maken
Vervolgens gaat u een bestandsshare maken.

1. Als de implementatie van het Azure-opslagaccount is voltooid, klikt u op **Go to resource**.
1. Klik in het deelvenster met het opslagaccount op **Bestanden**.

    ![Op Bestanden klikken](./media/storage-sync-files-extend-servers/click-files.png)

1. Klik op **+ Bestandsshare**.

    ![Op de knop Bestandsshare toevoegen klikken](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Geef de nieuwe bestandsshare de naam *afsfileshare* en voer 1 in bij **Quotum**. Klik vervolgens op **Maken**. Het quotum kan maximaal 5 TiB zijn, maar voor deze zelfstudie hebt u slechts 1 GB nodig.

    ![Een naam en een quotum opgeven voor de nieuwe bestandsshare](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Selecteer de nieuwe bestandsshare en klik vervolgens op de bestandssharelocatie op **Uploaden**.

    ![Bestand uploaden](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. Ga naar de map *FilesToSync*, waar u het TXT-bestand hebt gemaakt, selecteer *mytestdoc.txt* en klik op **Uploaden**.

    ![In bestandsshare bladeren](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

U hebt nu een Azure Storage-account gemaakt en een bestandsshare met één bestand in Azure. U gaat nu de Azure-VM maken met Windows Server 2016 Datacenter, die de on-premises server in deze zelfstudie representeert.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>Een VM implementeren en een gegevensschijf koppelen

1. Vouw vervolgens het menu aan de linkerkant van de portal uit en kies **Een resource maken** in linkerbovenhoek van de Azure-portal.
1. Zoek via het zoekvak boven de lijst met **Azure Marketplace**-resources naar **Windows Server 2016 Datacenter**, selecteer dit en kies **Maken**.
1. Op het tabblad **Basis**, onder **Projectdetails**, selecteert u de resourcegroep die u voor deze zelfstudie hebt gemaakt.

   ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. Geef onder **Exemplaardetails** een naam aan de VM, bijvoorbeeld *mijnVM*.
1. Behoud de standaardinstellingen voor **Regio**, **Beschikbaarheidsopties**, **Installatiekopie** en **Grootte**.
1. Geef onder **Administrator-account** een **gebruikersnaam** en een **wachtwoord** voor de VM op.
1. Onder **Regels voor binnenkomende poort** kiest u **​​Geselecteerde poorten toestaan** en selecteert u **RDP (3389)** en **HTTP** in de vervolgkeuzelijst.

   Voordat u de VM kunt maken, dient u en gegevensschijf te maken.

1. Klik op **Volgende: schijven**

   ![Gegevensschijven toevoegen](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

1. Behoud op het tabblad **Schijven**, onder **Schijfopties**, de standaardinstellingen.
1. Klik onder **GEGEVENSSCHIJVEN** op **Create and attach a new disk**.

1. Behoud de standaardinstellingen, maar wijzig voor deze zelfstudie **Grootte (GiB)** in **1 GB**.

   ![Details van gegevensschijf](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

1. Klik op **OK**.
1. Klik op **Controleren + maken**.
1. Klik op **Create**.

   Als u op het pictogram **Meldingen** klikt, kunt u de **Deployment progress** zien. Het duurt enkele minuten voordat de nieuwe VM is voltooid.

1. Zodra de implementatie van de VM is voltooid, klikt u op **Ga naar resource**.

   ![Ga naar resource](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

   U hebt nu een nieuwe virtuele machine gemaakt en een gegevensschijf gekoppeld. U dient nu verbinding te maken met de VM.

### <a name="connect-to-your-vm"></a>Verbinding maken met uw VM

1. Klik in de Azure-portal, op de eigenschappenpagina van de virtuele machine, op **Verbinding maken**.

   ![Verbinding maken met een Azure VM vanaf de portal](./media/storage-sync-files-extend-servers/connect-vm.png)

1. Laat op de pagina **Verbinding maken met virtuele machine** de standaardopties staan om via **IP-adres** verbinding te maken via poort 3389 en klik op **RDP-bestand downloaden**.

   ![Het RDP-bestand downloaden](./media/storage-sync-files-extend-servers/download-rdp.png)

1. Open het gedownloade RDP-bestand en klik op **Verbinden** wanneer dit wordt gevraagd.
1. Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Typ de gebruikersnaam als *localhost\username*, voer het wachtwoord in dat u voor de virtuele machine hebt gemaakt en klik vervolgens op **OK**.

   ![Meer keuzes](./media/storage-sync-files-extend-servers/local-host2.png)

1. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om de verbinding te maken.

### <a name="prepare-the-windows-server"></a>Windows Server voorbereiden
Schakel **Verbeterde beveiliging van Internet Explorer** uit voor de server **Windows Server 2016 Datacenter**. Deze step is alleen noodzakelijk als u de server voor het eerst registreert. U kunt de optie opnieuw inschakelen nadat de server is geregistreerd.

In de VM **Windows Server 2016 Datacenter** wordt **Serverbeheer** automatisch geopend.  Als **Serverbeheer** niet standaard wordt geopend, kunt u er naar zoeken in Verkenner.

1. Klik in **Serverbeheer** op **Lokale server**.

   ![Lokale server aan de linkerkant van de gebruikersinterface van Serverbeheer](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. Selecteer in het deelvenster **Eigenschappen** de koppeling naar **Verbeterde beveiliging van Internet Explorer**.  

    ![Het deelvenster Verbeterde beveiliging van Internet Explorer in de gebruikersinterface van Serverbeheer](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. Selecteer in het dialoogvenster **Verbeterde beveiliging van Internet Explorer** de optie **Uit** voor **Administrators** en **Gebruikers**.

    ![Het pop-upvenster Verbeterde beveiliging van Internet Explorer met de optie Uit geselecteerd](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

   U kunt nu de gegevensschijf aan de VM toevoegen.

### <a name="add-the-data-disk"></a>De gegevensschijf toevoegen

1. Terwijl u nog steeds in **Windows Server 2016 Datacenter** aanwezig bent, klikt u op **Files and storage services** > **Volumes** > **Schijven**.

    ![Gegevensschijf](media/storage-sync-files-extend-servers/your-disk.png)

1. Klik met de rechtermuisknop op de 1 GB-schijf met de naam **Msft Virtual Disk** en klik op **Nieuw volume**.
1. Voltooi de wizard met behoud van de standaardinstellingen, noteer de toegewezen stationsletter en klik op **Maken**.
1. Klik op **Sluiten**.

   U hebt nu de schijf online gebracht en een volume gemaakt. U kunt controleren of de gegevensschijf is geslaagd door Verkenner op de VM te openen en te controleren of het nieuwe station aanwezig is.

1. Vouw in Verkenner op de VM **Deze pc** uit en dubbelklik op het nieuwe station. In dit voorbeeld is dat het F-station.
1. Klik er met de rechtermuisknop op en selecteer **Nieuw** > **Map**. Geef de map de naam *FilesToSync*.
1. Dubbelklik op de map **FilesToSync**.
1. Klik er met de rechtermuisknop op en selecteer **Nieuw** > **Tekstdocument**. Geef het tekstbestand de naam *MyTestFile*.

    ![Een nieuw tekstbestand toevoegen](media/storage-sync-files-extend-servers/new-file.png)

1. Sluit **Verkenner** en **Serverbeheer**.

### <a name="download-the-azure-powershell-module"></a>Azure PowerShell-module downloaden
Installeer vervolgens in de VM **Windows Server 2016 Datacenter** de **Azure PowerShell-module** op de server.

1. Open in de VM een PowerShell-venster met verhoogde bevoegdheid
1. Voer de volgende opdracht uit:

   ```powershell
   Install-Module -Name Az -AllowClobber
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

De `Az`-module is een updatepakketmodule voor de Azure PowerShell-cmdlets. Wanneer u deze installeert, worden alle beschikbare Azure Resource Manager-modules gedownload en kunnen de cmdlets uit deze modules worden gebruikt.

U hebt nu uw omgeving ingesteld voor de zelfstudie en u kunt de **opslagsynchronisatieservice** gaan implementeren.

## <a name="deploy-the-service"></a>Implementeer de service 
Het implementeren van de Azure File Sync begint door een resource van een **opslagsynchronisatieservice** in een resourcegroep voor uw geselecteerde abonnement te plaatsen. De opslagsynchronisatieservice neemt toegangsmachtigingen over van het abonnement en de resourcegroep waar u de service in implementeert.

1. Klik in de Azure-portal op **Een resource maken** en zoek naar **Azure File Sync**.
1. Selecteer **Azure File Sync** in de lijst met zoekresultaten.
1. Selecteer **Maken** om het tabblad **Opslagsynchronisatie implementeren** te openen.

   ![Opslagsynchronisatie implementeren](media/storage-sync-files-extend-servers/afs-info.png)

   Voer de volgende gegevens in in het deelvenster dat verschijnt:

   | Waarde | Beschrijving |
   | ----- | ----- |
   | **Naam** | Een unieke naam (per abonnement) voor opslagsynchronisatieservice.<br><br>In deze zelfstudie wordt *afssyncservice02* gebruikt. |
   | **Abonnement** | Het abonnement dat u gebruikt voor deze zelfstudie. |
   | **Resourcegroep** | De resourcegroep die u gebruikt voor deze zelfstudie.<br><br>In deze zelfstudie wordt *afsresgroup101918* gebruikt. |
   | **Locatie** | US - oost |

1. Als u klaar bent, selecteert u **Maken** om de **opslagsynchronisatieservice** te implementeren.
1. Klik op het tabblad **Meldingen** > **Naar de resource gaan**.

## <a name="install-the-agent"></a>De agent installeren
De Azure File Sync-agent is een downloadbaar pakket waardoor Windows Server met een Azure-bestandsshare kan worden gesynchroniseerd.

1. Schakel terug naar de VM **Windows Server 2016 Datacenter** en open **Internet Explorer**.
1. Ga naar het [Microsoft Downloadcentrum](https://go.microsoft.com/fwlink/?linkid=858257). Schuif omlaag naar de sectie **Azure File Sync-agent** en klik op **Downloaden**.

   ![Downloaden van synchronisatieagent](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. Schakel het selectievakje voor **StorageSyncAgent_V3_WS2016.EXE** in en klik op **Volgende**.

   ![Agent selecteren](media/storage-sync-files-extend-servers/select-agent.png)

1. **Eenmaal toestaan** > **Uitvoeren** > **Openen** van het bestand.
1. Sluit het PowerShell-venster als u dat nog niet hebt gedaan.
1. Accepteer de standaardwaarden in de **Storage Sync Agent Setup Wizard**.
1. Klik op **Install**.
1. Klik op **Voltooien**.

U hebt de Azure Sync-service geïmplementeerd en de agent geïnstalleerd op de VM **Windows Server 2016 Datacenter**. U dient nu de VM te registreren voor de **opslagsynchronisatieservice**.

## <a name="register-windows-server"></a>Windows Server registreren
Als u Windows Server registreert voor een opslagsynchronisatieservice, wordt er een vertrouwensrelatie ingesteld tussen uw server (of cluster) en de opslagsynchronisatieservice. Een server kan slechts voor één opslagsynchronisatieservice worden geregistreerd en kan worden gesynchroniseerd met andere servers en Azure-bestandsshares die aan dezelfde opslagsynchronisatieservice zijn gekoppeld.

De gebruikersinterface van de serverregistratie moet automatisch worden geopend als de **Azure File Sync-agent** is geïnstalleerd. Als dat niet zo is, kunt u de interface handmatig openen vanaf de bestandslocatie: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.

1. Als de gebruikersinterface van de serverregistratie in de VM wordt geopend, klikt u op **OK**.
1. Klik op **Aanmelden** om te beginnen.
1. Meld u aan met uw Azure-accountreferenties en klik op **Aanmelden**.
1. Geef de volgende informatie op:

   ![Schermafbeelding van de gebruikersinterface van de serverregistratie](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Waarde | Beschrijving |
   | **Azure-abonnement** | Het abonnement dat de opslagsynchronisatieservice voor deze zelfstudie bevat. |
   | **Resourcegroep** | De resourcegroep die de opslagsynchronisatieservice voor deze zelfstudie bevat. In de hele zelfstudie is *afsresgroup101918* gebruikt. |
   | **Opslagsynchronisatieservice** | De naam van de opslagsynchronisatieservice die u voor deze zelfstudie hebt gebruikt. In de hele zelfstudie is *afssyncservice02* gebruikt. |

1. Klik op **Registreren** om de serverregistratie te voltooien.
1. Als deel van het registratieproces wordt u gevraagd u nogmaals aan te melden. Meld u aan en klik op **Volgende**.
1. Klik op **OK**.

## <a name="create-a-sync-group"></a>Synchronisatiegroep maken
Een synchronisatiegroep definieert de synchronisatietopologie voor een verzameling bestanden. Een synchronisatiegroep moet één cloudeindpunt bevatten, dat een Azure-bestandsshare en een of meer servereindpunten representeert. Een servereindpunt representeert een pad naar een geregistreerde server.

1. Als u een synchronisatiegroep wilt maken, selecteert u in de [Azure-portal](https://portal.azure.com/) de optie **+ Synchronisatiegroep** vanuit de opslagsynchronisatieservice die u voor deze zelfstudie hebt gemaakt. In deze zelfstudie is *afssyncservice02* als voorbeeld gebruikt.

   ![Een nieuwe synchronisatiegroep in de Azure-portal maken](media/storage-sync-files-extend-servers/add-sync-group.png)

1. Voer in het deelvenster dat verschijnt de volgende gegevens in om een synchronisatiegroep met een cloudeindpunt te maken:

   | Waarde | Beschrijving |
   | ----- | ----- |
   | **Naam synchronisatiegroep** | Deze naam moet uniek zijn binnen de opslagsynchronisatieservice, maar het mag een willekeurige naam zijn die u makkelijk kunt onthouden. In deze zelfstudie wordt *afssyncgroup* gebruikt.|
   | **Abonnement** | Het abonnement waar u de opslagsynchronisatieservice voor deze zelfstudie hebt geïmplementeerd. |
   | **Opslagaccount** |Klik op **Opslagaccount selecteren**. Selecteer in het deelvenster dat verschijnt het opslagaccount met de Azure-bestandsshare die u voor deze zelfstudie hebt gemaakt. Hier is *afsstoracct101918* gebruikt. |
   | **Azure-bestandsshare** | De naam van de Azure-bestandsshare die u voor deze zelfstudie hebt gemaakt. Hier is *afsfileshare* gebruikt. |

1. Klik op **Create**.

Als u uw synchronisatiegroep selecteert, ziet u dat u nu één **cloudeindpunt** hebt.

## <a name="add-a-server-endpoint"></a>Servereindpunt toevoegen
Een servereindpunt representeert een bepaalde locatie op een geregistreerde server, bijvoorbeeld een map op een servervolume.

1. U kunt een servereindpunt toevoegen door de nieuw gemaakte synchronisatiegroep te selecteren en vervolgens **Servereindpunt toevoegen** te selecteren.

   ![Nieuw servereindpunt toevoegen in het deelvenster met de synchronisatiegroep](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. Voer in het deelvenster **Servereindpunt toevoegen** de volgende gegevens in om een servereindpunt te maken:

   | | |
   | ----- | ----- |
   | Waarde | Beschrijving |
   | **Geregistreerde server** | De naam van de server die u voor deze zelfstudie hebt gemaakt. In deze zelfstudie is *afsvm101918* gebruikt |
   | **Pad** | Het Windows Server-pad naar het station dat u voor deze zelfstudie hebt gemaakt. In ons voorbeeld is dat *f:\filestosync*. |
   | **Cloudopslaglagen** | Voor deze zelfstudie uitgeschakeld laten. |
   | **Beschikbare volumeruimte** | Voor deze zelfstudie leeg laten. |

1. Klik op **Create**.

Uw bestanden zijn nu gesynchroniseerd met uw Azure-bestandsshare en Windows Server.

![Azure Storage is gesynchroniseerd](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u de basisstappen geleerd voor het uitbreiden van de opslagcapaciteit van een Windows-server met Azure File Sync. Klik op deze koppeling voor meer inzicht in het plannen van een Azure File Sync-implementatie.

> [!div class="nextstepaction"]
> [Planning voor de implementatie van Azure File Sync](./storage-sync-files-planning.md)
