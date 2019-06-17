---
title: Lokale web UI-beheer voor Azure Data Box, Azure Data Box zware | Microsoft Docs
description: Wordt beschreven hoe u de lokale webgebruikersinterface voor het beheren van uw Data Box en gegevens in het zware-apparaten
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: bf8af37b0caf51966e336bcb4cea0c4ece5ca9c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66496250"
---
# <a name="use-the-local-web-ui-to-administer-your-data-box-and-data-box-heavy"></a>De lokale webgebruikersinterface gebruiken voor het beheren van uw Data Box en de gegevens in het zware

In dit artikel worden enkele van de configuratie en beheer taken die kunnen worden uitgevoerd op de Data Box en gegevens in het zware-apparaten beschreven. U kunt de gegevens in en gegevens in het zware apparaten via de gebruikersinterface van Azure portal en de lokale webgebruikersinterface voor het apparaat beheren. Dit artikel richt zich op de taken die u kunt uitvoeren met behulp van de lokale webinterface.

Lokale web-UI voor de Data Box en de gegevens in het zware wordt gebruikt voor de eerste configuratie van het apparaat. U kunt ook de lokale webgebruikersinterface afsluiten of opnieuw opstarten van het apparaat, Voer diagnostische tests uit, software bijwerken, logboeken kopiëren weergeven en genereer een logboek-pakket voor Microsoft Support. U kunt twee afzonderlijke lokale web-UI die overeenkomt met elk knooppunt van het apparaat te openen op een Data Box zware apparaat met twee onafhankelijke knooppunten.

Dit artikel bevat de volgende zelfstudies:

- Een ondersteuningspakket genereren
- Apparaat uitschakelen of opnieuw opstarten
- Stuklijst- of manifestbestanden downloaden
- Beschikbare capaciteit van het apparaat weergeven
- De controlesomvalidatie overslaan

## <a name="generate-support-package"></a>Ondersteuningspakket genereren

Als u problemen met het apparaat ondervindt, kunt u een ondersteuningspakket maken vanuit het systeemlogboek. Microsoft Ondersteuning gebruikt dit pakket om het probleem op te lossen. Voor het genereren van een ondersteuningspakket met, moet u de volgende stappen uitvoeren:

1. Ga in de lokale webinterface naar **Contact opnemen met de ondersteuning**  en klik op **Ondersteuningspakket maken**.

    ![Ondersteuningspakket maken 1](media/data-box-local-web-ui-admin/create-support-package-1.png)

2. Er wordt een ondersteuningspakket verzameld. Deze bewerking duurt enkele minuten.

    ![Ondersteuningspakket maken 2](media/data-box-local-web-ui-admin/create-support-package-2.png)

3. Nadat het maken van het ondersteuningspakket is voltooid, klikt u op **Ondersteuningspakket downloaden**. 

    ![Ondersteuningspakket maken 4](media/data-box-local-web-ui-admin/create-support-package-4.png)

4. Blader naar en kies de downloadlocatie. Open de map om de inhoud weer te geven.

    ![Ondersteuningspakket maken 5](media/data-box-local-web-ui-admin/create-support-package-5.png)


## <a name="shut-down-or-restart-your-device"></a>Apparaat uitschakelen of opnieuw opstarten

U kunt afsluiten of opnieuw opstarten van uw apparaat met behulp van de lokale webgebruikersinterface. We raden u aan de shares vóór de herstart offline te zetten op de host en vervolgens op het apparaat. Dit minimaliseert de kans op beschadigde gegevens. Zorg ervoor dat er geen gegevens worden gekopieerd wanneer u het apparaat afsluit.

Als u wilt afsluiten van uw apparaat, moet u de volgende stappen uitvoeren.

1. Ga in de lokale webinterface naar **Afsluiten of opnieuw opstarten**.
2. Klik op **Afsluiten**.

    ![Data Box afsluiten 1](media/data-box-local-web-ui-admin/shut-down-local-web-ui-1.png)

3. Klik op **OK** als u om bevestiging wordt gevraagd.

    ![Data Box afsluiten 2](media/data-box-local-web-ui-admin/shut-down-local-web-ui-2.png)

Nadat het apparaat is uitgeschakeld, gebruikt u de aan-uitknop op het voorpaneel om het apparaat in te schakelen.

Voer de volgende stappen uit om uw Data Box opnieuw op te starten.

1. Ga in de lokale webinterface naar **Afsluiten of opnieuw opstarten**.
2. Klik op **Opnieuw opstarten**.

    ![Data Box opnieuw opstarten 1](media/data-box-local-web-ui-admin/restart-local-web-ui-1.png)

3. Klik op **OK** als u om bevestiging wordt gevraagd.

   Het apparaat wordt afgesloten en opnieuw opgestart.

## <a name="download-bom-or-manifest-files"></a>Stuklijst- of manifestbestanden downloaden

De factuur van materiaal (BOM) of de manifest-bestanden bevatten de lijst van de bestanden die zijn gekopieerd naar de Data Box of de gegevens in het zware. Deze bestanden worden gegenereerd wanneer het voorbereiden van het apparaat te verzenden.

Voordat u begint, zorg ervoor dat uw apparaat is voltooid **voorbereiding voor verzending** stap. Volg deze stappen om stuklijst- of manifestbestanden te downloaden:

1. Ga naar de lokale webgebruikersinterface van uw apparaat. U ziet dat het apparaat de voorbereiding voor verzending is voltooid. Nadat de voorbereiding van het apparaat is voltooid, wordt de status van het apparaat bijgewerkt naar **Gereed voor verzending**.

    ![Apparaat gereed voor verzending](media/data-box-portal-admin/ready-to-ship.png)

2. Klik op **Lijst met bestanden downloaden** om de lijst met bestanden die naar uw Data Box zijn gekopieerd te downloaden.

    ![Klik op Lijst met bestanden downloaden](media/data-box-portal-admin/download-list-of-files.png)

3. In Windows Verkenner ziet u dat er afzonderlijke lijsten met bestanden worden gegenereerd, afhankelijk van het protocol dat wordt gebruikt om verbinding te maken met het apparaat en het gebruikte type Azure Storage.

    ![Bestanden voor opslagtype en verbindingsprotocol](media/data-box-portal-admin/files-storage-connection-type.png)

   In de volgende tabel ziet u welke bestandsnamen horen bij welk type Azure Storage en gebruikt verbindingsprotocol.

    |Bestandsnaam  |Azure Storage-type  |Gebruikt verbindingsprotocol |
    |---------|---------|---------|
    |databoxe2etest_BlockBlob.txt     |Blok-blobs         |SMB/NFS         |
    |databoxe2etest_PageBlob.txt     |Pagina-blobs         |SMB/NFS         |
    |databoxe2etest_AzFile-BOM.txt    |Azure Files         |SMB/NFS         |
    |databoxe2etest_PageBlock_Rest-BOM.txt     |Pagina-blobs         |REST        |
    |databoxe2etest_BlockBlock_Rest-BOM.txt    |Blok-blobs         |REST         |
    |mydbmdrg1_MDisk-BOM.txt    |Managed Disk         |SMB/NFS         |
    |mydbmdrg2_MDisk-BOM.txt     |Managed Disk         |SMB/NFS         |

U kunt deze lijst gebruiken om de bestanden die zijn geüpload naar het Azure Storage-account te controleren nadat de Data Box is geretourneerd naar het Azure-datacenter. Hieronder ziet u een voorbeeld van een manifestbestand.

> [!NOTE]
> Op een Data Box intensief zijn twee sets met de lijst met bestanden (BOM) aanwezig overeenkomt met de twee knooppunten op het apparaat.

```xml
<file size="52689" crc64="0x95a62e3f2095181e">\databox\media\data-box-deploy-copy-data\prepare-to-ship2.png</file>
<file size="22117" crc64="0x9b160c2c43ab6869">\databox\media\data-box-deploy-copy-data\connect-shares-file-explorer2.png</file>
<file size="57159" crc64="0x1caa82004e0053a4">\databox\media\data-box-deploy-copy-data\verify-used-space-dashboard.png</file>
<file size="24777" crc64="0x3e0db0cd1ad438e0">\databox\media\data-box-deploy-copy-data\prepare-to-ship5.png</file>
<file size="162006" crc64="0x9ceacb612ecb59d6">\databox\media\data-box-cable-options\cabling-dhcp-data-only.png</file>
<file size="155066" crc64="0x051a08d36980f5bc">\databox\media\data-box-cable-options\cabling-2-port-setup.png</file>
<file size="150399" crc64="0x66c5894ff328c0b1">\databox\media\data-box-cable-options\cabling-with-switch-static-ip.png</file>
<file size="158082" crc64="0xbd4b4c5103a783ea">\databox\media\data-box-cable-options\cabling-mgmt-only.png</file>
<file size="148456" crc64="0xa461ad24c8e4344a">\databox\media\data-box-cable-options\cabling-with-static-ip.png</file>
<file size="40417" crc64="0x637f59dd10d032b3">\databox\media\data-box-portal-admin\delete-order1.png</file>
<file size="33704" crc64="0x388546569ea9a29f">\databox\media\data-box-portal-admin\clone-order1.png</file>
<file size="5757" crc64="0x9979df75ee9be91e">\databox\media\data-box-safety\japan.png</file>
<file size="998" crc64="0xc10c5a1863c5f88f">\databox\media\data-box-safety\overload_tip_hazard_icon.png</file>
<file size="5870" crc64="0x4aec2377bb16136d">\databox\media\data-box-safety\south-korea.png</file>
<file size="16572" crc64="0x05b13500a1385a87">\databox\media\data-box-safety\taiwan.png</file>
<file size="999" crc64="0x3f3f1c5c596a4920">\databox\media\data-box-safety\warning_icon.png</file>
<file size="1054" crc64="0x24911140d7487311">\databox\media\data-box-safety\read_safety_and_health_information_icon.png</file>
<file size="1258" crc64="0xc00a2d5480f4fcec">\databox\media\data-box-safety\heavy_weight_hazard_icon.png</file>
<file size="1672" crc64="0x4ae5cfa67c0e895a">\databox\media\data-box-safety\no_user_serviceable_parts_icon.png</file>
<file size="3577" crc64="0x99e3d9df341b62eb">\databox\media\data-box-safety\battery_disposal_icon.png</file>
<file size="993" crc64="0x5a1a78a399840a17">\databox\media\data-box-safety\tip_hazard_icon.png</file>
<file size="1028" crc64="0xffe332400278f013">\databox\media\data-box-safety\electrical_shock_hazard_icon.png</file>
<file size="58699" crc64="0x2c411d5202c78a95">\databox\media\data-box-deploy-ordered\data-box-ordered.png</file>
<file size="46816" crc64="0x31e48aa9ca76bd05">\databox\media\data-box-deploy-ordered\search-azure-data-box1.png</file>
<file size="24160" crc64="0x978fc0c6e0c4c16d">\databox\media\data-box-deploy-ordered\select-data-box-option1.png</file>
<file size="115954" crc64="0x0b42449312086227">\databox\media\data-box-disk-deploy-copy-data\data-box-disk-validation-tool-output.png</file>
<file size="6093" crc64="0xadb61d0d7c6d4deb">\databox\data-box-cable-options.md</file>
<file size="6499" crc64="0x080add29add367d9">\databox\data-box-deploy-copy-data-via-nfs.md</file>
<file size="11089" crc64="0xc3ce6b13a4fe3001">\databox\data-box-deploy-copy-data-via-rest.md</file>
<file size="9126" crc64="0x820856b5a54321ad">\databox\data-box-overview.md</file>
<file size="10963" crc64="0x5e9a14f9f4784fd8">\databox\data-box-safety.md</file>
<file size="5941" crc64="0x8631d62fbc038760">\databox\data-box-security.md</file>
<file size="12536" crc64="0x8c8ff93e73d665ec">\databox\data-box-system-requirements-rest.md</file>
<file size="3220" crc64="0x7257a263c434839a">\databox\data-box-system-requirements.md</file>
<file size="2823" crc64="0x63db1ada6fcdc672">\databox\index.yml</file>
<file size="4364" crc64="0x62b5710f58f00b8b">\databox\data-box-local-web-ui-admin.md</file>
<file size="3603" crc64="0x7e34c25d5606693f">\databox\TOC.yml</file>
```

Dit bestand bevat de lijst van alle bestanden die zijn gekopieerd op de Data Box of de gegevens in het zware. In dit bestand is de *crc64*-waarde gerelateerd aan de controlesom die is gegenereerd voor het bijbehorende bestand.

## <a name="view-available-capacity-of-the-device"></a>Beschikbare capaciteit van het apparaat weergeven

U kunt het apparaatdashboard gebruiken om de beschikbare en gebruikte capaciteit van het apparaat weer te geven.

1. Ga in de lokale webinterface naar **Dashboard weergeven**.
2. Onder **Verbinding maken en kopiëren** wordt de beschikbare en gebruikte ruimte op het apparaat weergegeven.

    ![Beschikbare capaciteit weergeven](media/data-box-local-web-ui-admin/verify-used-space-dashboard.png)

## <a name="skip-checksum-validation"></a>De controlesomvalidatie overslaan

Er worden standaard controlesommen gegenereerd voor uw gegevens tijdens de voorbereiding voor verzending. In sommige zeldzame gevallen, afhankelijk van het gegevenstype (kleine bestandsgrootten), kan dit lang duren. In dergelijke gevallen kunt u de controlesom overslaan.

We raden u ten zeerste aan om de controlesom niet uit te schakelen, tenzij de prestaties te veel worden beïnvloed.

1. In de rechterbovenhoek van de lokale webgebruikersinterface van uw apparaat, gaat u naar **instellingen**.

    ![Controlesom uitschakelen](media/data-box-local-web-ui-admin/disable-checksum.png)

2. Controlesomvalidatie **Uitschakelen**
3. Klik op **Toepassen**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [de Data Box en de gegevens in het zware beheren via de Azure portal](data-box-portal-admin.md).

