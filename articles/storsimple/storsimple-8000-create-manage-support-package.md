---
title: Maken van een ondersteuningspakket van de StorSimple 8000-reeks | Microsoft Docs
description: Informatie over het maken, te ontsleutelen en te bewerken van een ondersteuningspakket voor de StorSimple 8000-apparaat.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: dfc2d8d763a1eb64a37af73e03992f2d948a6856
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254363"
---
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a>Maken en beheren van een ondersteuningspakket met voor StorSimple 8000-serie

## <a name="overview"></a>Overzicht

Een StorSimple-ondersteuning-pakket is een mechanisme voor eenvoudig te gebruiken die worden verzameld van alle relevante Logboeken om te helpen van Microsoft Support bij het oplossen van problemen met StorSimple-apparaat. De verzamelde logboeken worden versleuteld en gecomprimeerd.

Deze zelfstudie bevat stapsgewijze instructies voor het maken en beheren van het ondersteuningspakket voor de StorSimple 8000-apparaat. Als u met een StorSimple Virtual Array werkt, gaat u naar [genereren van een pakket log](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="create-a-support-package"></a>Een ondersteuningspakket maken

In sommige gevallen moet u handmatig maken van het ondersteuningspakket via Windows PowerShell voor StorSimple. Bijvoorbeeld:

* Als u verwijderen van gevoelige informatie uit uw logboekbestanden wilt vóór delen met Microsoft Support.
* Als u problemen bij het uploaden van het pakket vanwege verbindingsproblemen ondervindt.

U kunt uw handmatig gegenereerde ondersteuningspakket delen met Microsoft Support via e-mail. De volgende stappen uitvoeren om een ondersteuningspakket maken in Windows PowerShell voor StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Een ondersteuningspakket maken in Windows PowerShell voor StorSimple

1. Als u wilt een Windows PowerShell-sessie starten als beheerder op de externe computer die verbinding maken met uw StorSimple-apparaat wordt gebruikt, voer de volgende opdracht:
   
    `Start PowerShell`
2. In de Windows PowerShell-sessie verbinding maken met de SSAdmin Console van uw apparaat:
   
   1. Voer in de opdrachtregel in:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. Voer in het dialoogvenster dat wordt geopend, het beheerderswachtwoord voor het apparaat. Is het standaardwachtwoord _Wachtwoord1_.
     
      ![In het dialoogvenster van PowerShell-referentie](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. Selecteer **OK**.
   4. Voer in de opdrachtregel in:
     
      `Enter-PSSession $MS`
3. Voer in de sessie die wordt geopend, de juiste opdracht.
   
   * Voer voor netwerkshares die beveiligd met een wachtwoord zijn in:
     
       `Export-HcsSupportPackage -Path <\\IP address\location of the shared folder> -Include Default -Credential domainname\username`
     
       U wordt gevraagd een wachtwoord en een wachtwoordzin voor versleuteling (omdat het ondersteuningspakket is versleuteld). Een ondersteuningspakket wordt vervolgens gemaakt in de standaardmap (apparaatnaam toegevoegd met de huidige datum en tijd).
   * Voor bestandsshares die niet beveiligd met een wachtwoord zijn, hoeft u niet de `-Credential` parameter. Voer het volgende in:
     
       `Export-HcsSupportPackage`
     
       Het ondersteuningspakket is voor beide controllers in de standaardmap gemaakt. Het pakket is een versleutelde gecomprimeerde bestand dat kan worden verzonden naar Microsoft Support voor het oplossen van problemen. Zie voor meer informatie, [contact opnemen met Microsoft ondersteuning](storsimple-8000-contact-microsoft-support.md).

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>De parameters van de cmdlet Export-HcsSupportPackage

Met de cmdlet Export-HcsSupportPackage kunt u de volgende parameters.

| Parameter | Vereist/optioneel | Beschrijving |
| --- | --- | --- |
| `-Path` |Vereist |Gebruik voor de locatie van de gedeelde netwerkmap waarin het ondersteuningspakket wordt geplaatst. |
| `-EncryptionPassphrase` |Vereist |Gebruik een wachtwoordzin in om u te helpen bij het versleutelen van het ondersteuningspakket. |
| `-Credential` |Optioneel |Gebruiken om op te geven van de referenties voor toegang voor de gedeelde netwerkmap. |
| `-Force` |Optioneel |Gebruik de versleuteling wachtwoordzin bevestigingsstap overslaan. |
| `-PackageTag` |Optioneel |Gebruiken om op te geven van een map onder *pad* in dat het ondersteuningspakket wordt geplaatst. De standaardwaarde is [apparaatnaam]-[huidige datum en time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |Optioneel |Geef als **Cluster** (standaard) te maken van een ondersteuningspakket met voor beide controllers. Als u maken van een pakket alleen voor de huidige controller wilt, geeft u **Controller**. |

## <a name="edit-a-support-package"></a>Een ondersteuningspakket bewerken

Nadat u een ondersteuningspakket hebt gegenereerd, moet u mogelijk om te bewerken van het pakket om gevoelige informatie te verwijderen. Dit kunnen bijvoorbeeld volumenamen, apparaat-IP-adressen en namen van back-up van de logboekbestanden.

> [!IMPORTANT]
> U kunt alleen een ondersteuningspakket die is gegenereerd via Windows PowerShell voor StorSimple bewerken. U kunt een pakket gemaakt in de Azure portal met StorSimple Device Manager-service niet bewerken.

Als u wilt een ondersteuningspakket bewerken voordat u deze op de website Microsoft Support uploadt, het ondersteuningspakket met voor het eerst ontsleutelen, de bestanden bewerken en vervolgens opnieuw versleutelen. Voer de volgende stappen uit.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Het bewerken van een ondersteuningspakket in Windows PowerShell voor StorSimple

1. Genereer een ondersteuningspakket zoals is beschreven eerder in [een ondersteuningspakket maken in Windows PowerShell voor StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Download het script](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokaal op de client.
3. De Windows PowerShell-module importeren. Geef het pad naar de lokale map waarin u het script hebt gedownload. Voer het volgende voor het importeren van de module:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Alle bestanden zijn *.aes* bestanden die worden gecomprimeerd en versleuteld. Decomprimeren en bestanden ontsleutelen, invoeren:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Houd er rekening mee dat de werkelijke bestandsextensies nu voor alle bestanden worden weergegeven.
   
    ![Ondersteuningspakket bewerken](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. Wanneer u wordt gevraagd of u voor de wachtwoordzin voor versleuteling, typt u de wachtwoordzin op die u hebt gebruikt bij het ondersteuningspakket is gemaakt.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Blader naar de map met de logboekbestanden. Omdat de logboekbestanden worden nu gecomprimeerd en ontsleuteld, heeft deze oorspronkelijke bestandsextensies. Wijzigen van deze bestanden als u wilt verwijderen van eventuele klantspecifieke informatie, zoals volumenamen en IP-adressen van apparaat, en sla de bestanden.
7. Sluit de bestanden te comprimeren zodat ze met gzip en versleuteld met AES-256. Dit is voor snelheid en beveiliging bij het overbrengen van het ondersteuningspakket via een netwerk. Als u wilt comprimeren en versleutelen van bestanden, voert u het volgende:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Ondersteuningspakket bewerken](./media/storsimple-8000-create-manage-support-package/IC750707.png)
8. Geef desgevraagd een wachtwoordzin voor versleuteling voor het gewijzigde ondersteuningspakket.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. Noteer de nieuwe wachtwoordzin, zodat u deze met Microsoft Support delen kunt wanneer aangevraagd.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Voorbeeld: Bestanden bewerken in een ondersteuningspakket op een beveiligde share

Het volgende voorbeeld ziet hoe u ontsleutelen, bewerken en opnieuw versleutelen een ondersteuningspakket.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [informatie verzameld in het pakket met ondersteuning](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)
* Meer informatie over het [voor ondersteuningspakketten en logboeken van het apparaat gebruiken om op te lossen van de implementatie van uw apparaten](storsimple-8000-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

