---
title: Maak een back-up van een SAP HANA-database naar Azure met Azure Backup | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u back-up van een SAP HANA-database naar Azure met de Azure Backup-service.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: raynew
ms.openlocfilehash: a16ed7134fc9f3c159715f58f116de3fb30e8aca
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481110"
---
# <a name="back-up-an-sap-hana-database"></a>Back-up van een SAP HANA-database

[Azure Backup](backup-overview.md) biedt ondersteuning voor de back-up van SAP HANA-databases naar Azure.

> [!NOTE]
> Deze functie is momenteel beschikbaar als openbare preview-versie. Het is momenteel niet klaar voor productie en u beschikt niet over een gegarandeerde SLA. 

## <a name="scenario-support"></a>Scenario-ondersteuning

**Ondersteuning** | **Details**
--- | ---
**Ondersteunde geografische gebieden** | Australië-Zuidoost, Australië Oost <br> Brazilië - zuid <br> Canada Central, Canada East <br> Zuidoost-Azië, Oost-Azië <br> VS-Oost, VS-Oost 2, West-Centraal VS, VS-West, VS-West 2, Noord-centraal VS, VS-midden, VS Zuid-centraal<br> India-centraal, India-Zuid <br> Japan (oost), Japan (west)<br> Korea Centraal, Korea Zuid <br> Europa - noord, Europa - west <br> UK-Zuid, UK-West
**Ondersteunde besturingssystemen voor VM** | SLES 12 SP2 of SP3.
**Ondersteunde versies van HANA** | SDC op HANA 1.x, MDC op HANA 2.x < = SPS03

### <a name="current-limitations"></a>Huidige beperkingen

- U kunt alleen back-up van SAP HANA-databases op Azure Virtual machines worden uitgevoerd.
- U kunt alleen back-up van SAP HANA in Azure portal configureren. De functie kan niet worden geconfigureerd met PowerShell, CLI of de REST-API.
- U kunt alleen back-up van databases in de modus voor Scale-Up.
- U kunt back-up van databaselogboeken om de 15 minuten. Logboekback-ups wordt alleen beginnen met flow nadat een geslaagde volledige back-up voor de database is voltooid.
- U kunt volledige en differentiële back-ups op te nemen. Incrementele back-up wordt momenteel niet ondersteund.
- U kunt het back-upbeleid niet wijzigen nadat u deze voor back-ups van SAP HANA toepast. Als u back wilt-up maken met verschillende instellingen, een nieuw beleid maken of toewijzen van een ander beleid.
  - Als u wilt een nieuw beleid hebt gemaakt, klik in de kluis op **beleid** > **back-upbeleid** >  **+ toevoegen** > **SAP HANA in Azure-VM**, en geeft u beleidsinstellingen.
  - Klik op de naam van het huidige beleid om te wijzen aan een ander beleid, in de eigenschappen van de virtuele machine waarop de database wordt uitgevoerd. Klik op de **back-upbeleid** pagina kunt u een ander beleid moet worden gebruikt voor de back-up selecteren.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u het volgende doen voordat u back-ups configureren:

1. Op de virtuele machine uitgevoerd van de SAP HANA-database, het officiële Microsoft installeren [.NET Core Runtime 2.1](https://dotnet.microsoft.com/download/linux-package-manager/sles/runtime-current) pakket. Houd rekening met het volgende:
    - U hoeft alleen de **dotnet-runtime-2.1** pakket. U hoeft geen **aspnetcore-runtime-2.1**.
    - Als de virtuele machine geen internet toegang krijgen tot, mirror of een offline cache bieden voor dotnet-runtime-2.1 (en alle afhankelijke rpm's) van het pakket voor Microsoft-feed opgegeven in de pagina.
    - Tijdens de installatie van het pakket, wordt u mogelijk gevraagd een optie opgeven. Als dit het geval is, geeft u **oplossing 2**.

        ![Pakket-installatieoptie](./media/backup-azure-sap-hana-database/hana-package.png)

2. Op de virtuele machine, installeren en inschakelen van ODBC-stuurprogramma-pakketten uit de officiële SLES pakket/media met zypper, als volgt:

    ```unix
    sudo zypper update
    sudo zypper install unixODBC
    ```

3. Een verbinding toestaan van de virtuele machine met het internet, zodat het Azure, bereiken kan, zoals beschreven in de procedure [hieronder](#set-up-network-connectivity).

4. Voer het script vóór registratie op de virtuele machine waarop HANA als een hoofdgebruiker is geïnstalleerd. Het script wordt geleverd [in de portal](#discover-the-databases) in de stroom en is vereist voor het instellen van de [rechts machtigingen](backup-azure-sap-hana-database-troubleshoot.md#setting-up-permissions).

### <a name="set-up-network-connectivity"></a>Verbinding met het netwerk instellen

Voor alle bewerkingen moet de SAP HANA-VM verbinding met Azure openbare IP-adressen. VM-bewerkingen (databasedetectie configureren van back-ups, back-ups plannen, herstelpunten herstellen, enzovoort) werkt niet zonder connectiviteit. Verbinding door het toestaan van toegang tot de Azure-datacenter-IP-adresbereiken: 

- U kunt downloaden de [IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653) voor Azure-datacenters, en vervolgens toegang tot deze IP-adressen toe te staan.
- Als u netwerkbeveiligingsgroepen (nsg's), kunt u de AzureCloud [servicetag](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) om toe te staan alle Azure openbare IP-adressen. U kunt de [cmdlet Set-AzureNetworkSecurityRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0) NSG-regels te wijzigen.

## <a name="onboard-to-the-public-preview"></a>Onboarding van de openbare preview

Onboarding van de openbare preview-versie als volgt te werk:

- In de portal, registreert u uw abonnements-ID met de Recovery Services-provider door [in dit artikel te volgen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal). 
- Voor PowerShell, moet u deze cmdlet uitvoert. Het moet uitvoeren als 'Registered'.

    ```powershell
    PS C:>  Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```



[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>De databases detecteren

1. In de kluis in **aan de slag**, klikt u op **back-up**. In **waar wordt uw werkbelasting uitgevoerd?** , selecteer **SAP HANA in virtuele Azure-machine**.
2. Klik op **detectie starten**. Hiermee initieert detectie van niet-beveiligde Linux-machines in de kluisregio.

   - Na de detectie, niet-beveiligde virtuele machines worden weergegeven in de portal, gesorteerd op naam en resourcegroep.
   - Als een virtuele machine niet wordt weergegeven zoals verwacht, controleert u of deze al gemaakt om in een kluis.
   - Meerdere virtuele machines kunnen dezelfde naam hebben, maar ze deel uitmaken van verschillende resourcegroepen bevinden.

3. In **virtuele Machines selecteren**, klikt u op de koppeling om te downloaden van het script dat voorziet in machtigingen voor de Azure Backup-service voor toegang tot de SAP HANA-VM's voor databasedetectie
4. Voer het script uit op elke virtuele machine die als host fungeert voor SAP HANA-databases die u back wilt-up.
5. Nadat het script is uitgevoerd op de virtuele machines, in **virtuele Machines selecteren**, selecteert u de virtuele machines. Klik vervolgens op **DB's detecteren**.
6. Azure Backup detecteert alle SAP HANA-databases op de virtuele machine. Azure Backup wordt de virtuele machine geregistreerd bij de kluis en een uitbreiding op de virtuele machine wordt geïnstalleerd tijdens de detectie. Geen agent is geïnstalleerd op de database.

    ![SAP HANA-databases detecteren](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Back-up configureren  

Nu back-up inschakelen.

1. Klik in stap 2, **back-up configureren**.
2. In **items back-up selecteren**, selecteer de databases die u wilt beveiligen > **OK**.
3. In **back-upbeleid** > **back-upbeleid kiezen**, maak een nieuwe back-upbeleid voor de databases en in overeenstemming met de onderstaande instructies.
4. Na het maken van het beleid op de **back-up** menu, klikt u op **back-up inschakelen**.
5. De back-upconfiguratie voortgang volgen in de **meldingen** gebied van de portal.

### <a name="create-a-backup-policy"></a>Maak een back-upbeleid

Een back-upbeleid definieert wanneer back-ups worden gemaakt en hoe lang ze worden bewaard.

- Een beleid wordt gemaakt op kluisniveau.
- U kunt hetzelfde back-upbeleid gebruiken voor meerdere kluizen, maar u moet het back-upbeleid toepassen op elke kluis.

Geef de beleidsinstellingen als volgt te werk:

1. Geef bij **Beleidsnaam** een naam voor het nieuwe beleid op.
2. In **Beleid voor een volledige back-up** selecteert u een **back-upfrequentie** en kiest u **Dagelijks** of **Wekelijks**.
   - **Dagelijkse**: Selecteer het uur en de tijdzone waarin de back-uptaak wordt gestart.
   
       - U moet een volledige back-up uitvoeren. U kunt deze optie niet uitschakelen.
       - Klik op **Volledige back-up** om het beleid te bekijken.
       - U kunt geen differentiële back-ups maken voor dagelijkse volledige back-ups.
       
   - **Wekelijkse**: Selecteer de dag van de week, uur en tijdzone waarin de back-uptaak wordt uitgevoerd.
3. In **bewaartermijn**, configureren van instellingen voor het bewaren voor de volledige back-up.
    - Standaard worden alle opties geselecteerd. Schakel alle limieten voor het bereik van bewaren die u niet wilt gebruiken, en stel die u doen.
    - De minimale bewaarperiode voor elk type back-up (volledige/differentiële/logboek) is zeven dagen.
    - Herstelpunten worden getagd voor retentie op basis van de bewaarperiode. Als u een dagelijkse volledige back-up selecteert, wordt slechts één volledige back-up per dag geactiveerd.
    - De back-up voor een specifieke dag is gemarkeerd en wordt bewaard op basis van het wekelijks bewaartermijn en de instelling.
    - De maandelijkse en jaarlijkse bewaarperioden werken op soortgelijke wijze.

4. In de **volledige back-upbeleid** menu, klikt u op **OK** te accepteren van de instellingen.
5. Selecteer **differentiële back-up** differentiële beleid toe te voegen.
6. In **Beleid voor een differentiële back-up** selecteert u **Inschakelen** om de frequentie- en bewaarinstellingen te openen.
    - U kunt maximaal één differentiële back-up per dag activeren.
    - Differentiële back-ups kunnen maximaal 180 dagen worden bewaard. Als dat voor u te kort is, moet u volledige back-ups gebruiken.

    > [!NOTE]
    > Incrementele back-ups worden momenteel niet ondersteund. 

7. Klik op **OK** slaat u het beleid en terugkeren naar de hoofdpagina **back-upbeleid** menu.
8. Selecteer **logboekback-up** om toe te voegen een back-upbeleid transactionele log
    - In **logboekback-up**, selecteer **inschakelen**.
    - Stel de frequentie en retentie-besturingselementen.

    > [!NOTE]
    > Logboekback-ups wordt alleen beginnen met flow nadat een geslaagde volledige back-up is voltooid.

9. Klik op **OK** slaat u het beleid en terugkeren naar de hoofdpagina **back-upbeleid** menu.
10. Nadat u klaar bent met de back-upbeleid definiëren, klikt u op **OK**.


## <a name="run-an-on-demand-backup"></a>Een on-demand back-up uitvoeren

Back-ups uitgevoerd volgens de planning voor beleid. U kunt als volgt een back-up op aanvraag uitvoeren:


1. Klik in het kluismenu op **back-up items**.
2. In **back-Upitems**, selecteer de virtuele machine met de SAP HANA-database en klik vervolgens op **back-up nu**.
3. In **nu back-up**, het kalenderbesturingselement gebruiken om te selecteren van de laatste dag waarop het herstelpunt dat moet worden bewaard. Klik vervolgens op **OK**.
4. Controleer de portal. U kunt de voortgang van de taak op het kluisdashboard bewaken > **back-uptaken** > wordt uitgevoerd. Afhankelijk van de grootte van uw database duren het maken van de eerste back-up even.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>SAP HANA Studio back-up uitgevoerd op een database met Azure Backup ingeschakeld

Als u wilt een lokale back-up (met behulp van HANA Studio) van een database die back-up met Azure Backup, het volgende doen:

1. Wachttijd voor een volledige of logboekback-ups voor de database om te voltooien. Controleer de status in SAP HANA Studio.
2. Logboekback-ups uitschakelen en instellen van de back-catalogus naar het bestandssysteem voor de betreffende database.
3. U doet dit door te dubbelklikken op **systemdb** > **configuratie** > **Database selecteren** > **Filter (logboek)** .
4. Stel **enable_auto_log_backup** naar **Nee**.
5. Stel **log_backup_using_backint** naar **False**.
6. Neemt een volledige ad-hoc back-up van de database.
7. Wacht tot de volledige back-up- en back-catalogus te voltooien.
8. De vorige instellingen terug naar die voor Azure herstellen:
    - Stel **enable_auto_log_backup** naar **Ja**.
    - Stel **log_backup_using_backint** naar **waar**.



## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](backup-azure-sap-hana-database-troubleshoot.md) over het oplossen van veelvoorkomende fouten tijdens het gebruik van back-up van SAP HANA in virtuele Azure-machines.
[Meer informatie over](backup-azure-arm-vms-prepare.md) back-ups van virtuele Azure-machines.
