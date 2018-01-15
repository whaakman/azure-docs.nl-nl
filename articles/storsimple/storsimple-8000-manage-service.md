---
title: De service Manager voor StorSimple-apparaat in Azure implementeren | Microsoft Docs
description: Wordt uitgelegd hoe u maken en verwijderen van de service Manager voor StorSimple-apparaat in de Azure portal en wordt beschreven hoe u de serviceregistratiesleutel beheren.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/04/2017
ms.author: alkohli
ms.openlocfilehash: 96dcda25cde2473387842fd01421b6bb619e4ece
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>De service Manager voor StorSimple-apparaat voor StorSimple 8000 series apparaten implementeren

## <a name="overview"></a>Overzicht

De StorSimple-apparaat Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple-apparaten. Nadat u de service hebt gemaakt, kunt u deze kunt gebruiken voor het beheren van de apparaten die zijn verbonden met de service Manager voor StorSimple-apparaat vanaf een centrale locatie, waardoor administratieve last voor het minimaliseren.

Deze zelfstudie worden de stappen beschreven voor het maken, verwijderen, migratie van de service en het beheer van de serviceregistratiesleutel nodig. De informatie in dit artikel is alleen van toepassing op StorSimple 8000 series apparaten. Voor meer informatie over virtuele StorSimple-matrices, gaat u naar [implementeren van een service Manager voor StorSimple-apparaat voor uw virtuele StorSimple-matrix](storsimple-virtual-array-manage-service.md).

> [!NOTE]
> Alle klassieke StorSimple-apparaat managers zijn automatisch verplaatst naar de nieuwe Azure portal. Als u vragen hebt, raadpleegt u [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md). De Azure Service Management (ASM) PowerShell-cmdlets worden niet ondersteund na de overstap naar de nieuwe Azure portal. Bijwerken van de scripts voor het beheren van uw apparaten en gaat u naar de [scripts voor het beheren van StorSimple-apparaten op basis van gebruik Azure Resource Manager SDK](storsimple-8000-automation-azurerm-scripts.md) voor meer informatie. De nieuwe Azure portal ondersteunt apparaten met Update 5.0 of hoger. Als uw apparaat niet up-to-date te houden is, onmiddellijk te installeren Update 5. Ga voor meer informatie naar [installeren Update 5](storsimple-8000-install-update-5.md). Als u een StorSimple-Cloud-apparaat (8010/8020), kunt u een cloud-apparaat niet bijwerken. Gebruik de nieuwste versie van software voor het maken van een nieuwe cloud-apparaat met Update 5.0 en vervolgens een failover uitvoeren naar het nieuwe cloud toestel gemaakt. Alle apparaten met Update 4.0 of eerder merken [verminderd beheerfunctionaliteit](storsimple-8000-manage-service.md#supported-operations-on-devices-running-versions-prior-to-update-5.0). 

## <a name="create-a-service"></a>Een service maken
Voor het maken van een StorSimple-apparaat Manager-service moet u beschikken over:

* Een abonnement met een Enterprise Agreement
* Een actieve Microsoft Azure storage-account
* De informatie die wordt gebruikt voor toegangsbeheer

Alleen de abonnementen, met een Enterprise Agreement zijn toegestaan. Microsoft Sponsorship abonnementen die zijn toegestaan in de klassieke Azure portal worden niet ondersteund in de Azure portal. U ziet het volgende bericht weergegeven wanneer u een niet-ondersteunde abonnement:

![Abonnement is niet geldig](./media/storsimple-8000-manage-service/subscription-not-valid.jpg)

U kunt ook een standaardopslagaccount genereren bij het maken van de service.

Een enkele service kunt meerdere apparaten beheren. Een apparaat niet kan echter meerdere services omvatten. Een grote onderneming kan meerdere exemplaren van de service te werken met verschillende abonnementen behoren, organisaties of zelfs implementatie locaties hebben. 

> [!NOTE]
> U moet afzonderlijke exemplaren van StorSimple-apparaat Manager-service voor het beheren van apparaten voor StorSimple 8000 serie en virtuele StorSimple-matrices.

Voer de volgende stappen uit voor het maken van een service.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Voor elke service Manager voor StorSimple-apparaat bestaat de volgende kenmerken:

* **Naam** : de naam die is toegewezen aan uw StorSimple-apparaat Manager-service wanneer deze is gemaakt. **Naam van de service kan niet worden gewijzigd nadat de service is gemaakt. Dit geldt ook voor andere entiteiten, zoals apparaten, volumes, volumecontainers en back-upbeleid dat in de Azure portal kunnen niet worden gewijzigd.**
* **Status** – de status van de service, die kan worden **Active**, **maken**, of **Online**.
* **Locatie** – de geografische locatie op waarin het StorSimple-apparaat wordt geïmplementeerd.
* **Abonnement** – het Facturering abonnement dat is gekoppeld aan uw service.

## <a name="delete-a-service"></a>Een service verwijderen

Zorg voordat u een service hebt verwijderd, dat geen verbonden apparaten wordt gebruikt. Als de service gebruikt wordt, schakelt u verbonden apparaten. De bewerking deactiveren server de verbinding tussen het apparaat en de service, maar behouden van de apparaatgegevens in de cloud.

> [!IMPORTANT]
> Een service wordt verwijderd, de bewerking kan niet ongedaan worden gemaakt. Elk apparaat dat is met de service moet opnieuw worden ingesteld op standaardwaarden voordat deze kan worden gebruikt met een andere service. In dit scenario is de lokale gegevens op het apparaat, evenals de configuratie, gaan verloren.

Voer de volgende stappen uit om een service te verwijderen.

### <a name="to-delete-a-service"></a>Een service verwijderen

1. Zoek de service die u wilt verwijderen. Klik op **Resources** pictogram en geef de juiste voorwaarden om te zoeken. Klik in de zoekresultaten op de service die u wilt verwijderen.

    ![Search-service verwijderen](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Hiermee gaat u naar de blade van de service Manager voor StorSimple-apparaat. Klik op **Verwijderen**.

    ![Verwijderen van de service](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Klik op **Ja** in het bevestigingsbericht weergegeven. Duurt enkele minuten duren voordat de service moet worden verwijderd.

    ![Verwijdering bevestigen](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>De serviceregistratiesleutel ophalen

Nadat u een service hebt gemaakt, moet u uw StorSimple-apparaat registreren bij de service. Voor het registreren van uw eerste StorSimple-apparaat, moet u de serviceregistratiesleutel. Om extra apparaten registreren met een bestaande StorSimple-service, moet u zowel de registratiesleutel en de gegevensversleutelingssleutel van service (die is gegenereerd op de eerste apparaat tijdens de registratie). Zie voor meer informatie over de gegevensversleutelingssleutel van service [StorSimple security](storsimple-8000-security.md). U kunt de registratiesleutel ophalen door het openen van **sleutels** op de blade van uw StorSimple-Apparaatbeheer.

Voer de volgende stappen uit om de serviceregistratiesleutel ophalen.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Houd de serviceregistratiesleutel op een veilige locatie. U moet deze sleutel, evenals de gegevensversleutelingssleutel service, extra apparaten registreren bij deze service. Nadat de serviceregistratiesleutel, moet u uw apparaat via de Windows PowerShell voor StorSimple-interface configureren.

Zie voor meer informatie over het gebruik van deze registratiecode [stap 3: configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Opnieuw genereren van de serviceregistratiesleutel
U moet een serviceregistratiesleutel genereren als u nodig zijn voor sleutel rotatie uitgevoerd of als de lijst met servicebeheerders is gewijzigd. Wanneer u de sleutel opnieuw genereren, wordt de nieuwe sleutel alleen gebruikt voor latere apparaten registreren. De apparaten die al zijn geregistreerd, worden hierdoor niet beïnvloed door dit proces.

De volgende stappen voor het genereren van een serviceregistratiesleutel uitvoeren.

### <a name="to-regenerate-the-service-registration-key"></a>Opnieuw genereren van de serviceregistratiesleutel
1. In de **StorSimple Apparaatbeheer** blade, gaat u naar **Management &gt;**  **sleutels**.
    
    ![De blade Sleutels](./media/storsimple-8000-manage-service/regenregkey2.png)

2. In de **sleutels** blade, klikt u op **genereren**.

    ![Klik op opnieuw genereren](./media/storsimple-8000-manage-service/regenregkey3.png)
3. In de **opnieuw genereren serviceregistratiesleutel** blade, Controleer de actie vereist als de sleutels opnieuw worden gegenereerd. De volgende apparaten die zijn geregistreerd bij deze service gebruiken voor de nieuwe registratiesleutel. Klik op **genereren** om te bevestigen. U wordt gewaarschuwd als het opnieuw genereren voltooid is.

    ![Opnieuw genereren bevestigen](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Een nieuwe registratiecode van de service wordt weergegeven.

5. Kopieer deze sleutel en sla het voor het registreren van nieuwe apparaten aan deze service.



## <a name="change-the-service-data-encryption-key"></a>De gegevensversleutelingssleutel van service wijzigen
Versleutelingssleutels voor service-gegevens worden gebruikt voor het versleutelen van de vertrouwelijke klantgegevens, zoals opslagaccountreferenties, die van uw StorSimple Manager-service worden verzonden naar het StorSimple-apparaat. U moet deze sleutels regelmatig te wijzigen als uw IT-organisatie een beleid voor belangrijke draaien op de opslagapparaten heeft. Het proces voor sleutel kan zijn enigszins verschillen afhankelijk van of er is een apparaat met één of meerdere apparaten, beheerd door de StorSimple Manager-service. Ga voor meer informatie naar [StorSimple-beveiliging en gegevensbescherming](storsimple-8000-security.md).

Het wijzigen van de gegevensversleutelingssleutel van service is een proces 3-stappen:

1. Met Windows PowerShell-scripts voor Azure Resource Manager, de autorisatie voor een apparaat wijzigen van de gegevensversleutelingssleutel van service.
2. Met Windows PowerShell voor StorSimple, start de service gegevens versleuteling belangrijke wijziging.
3. Als u meer dan een StorSimple-apparaat hebt, werkt u de gegevensversleutelingssleutel van service op de andere apparaten.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>Stap 1: Gebruik Windows PowerShell-script voor het autoriseren van een apparaat wijzigen van de gegevensversleutelingssleutel van service
Normaal gesproken wordt de apparaatbeheerder vragen of de service-beheerder een apparaat wijzigen gegevensversleutelingssleutels service autoriseren. De servicebeheerder machtigen vervolgens het apparaat om de sleutel te wijzigen.

Deze stap wordt uitgevoerd met het script op basis van Azure Resource Manager. De servicebeheerder kunt selecteren van een apparaat dat in aanmerking komende worden geautoriseerd. Het apparaat is vervolgens gemachtigd om de service gegevens versleuteling wijzigingsproces te starten. 

Ga voor meer informatie over het gebruik van het script naar [autoriseren ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Welke apparaten kunnen worden geautoriseerd service gegevensversleutelingssleutels wijzigen?
Een apparaat moet de volgende criteria voldoen voordat deze kan worden geautoriseerd data encryption key servicewijzigingen initiëren:

* Het apparaat moet online in aanmerking komt voor service data encryption wijziging autorisatie.
* Als de wijziging is niet geïnitieerd, kunt u hetzelfde apparaat na 30 minuten opnieuw autoriseren.
* U kunt een ander apparaat autoriseren, mits de sleutel wijziging is niet geïnitieerd door de eerder gemachtigd apparaat. Nadat het nieuwe apparaat is verleend, kan de wijziging van het oude apparaat niet starten.
* U kunt een apparaat kan niet machtigen, terwijl de overschakeling van de gegevensversleutelingssleutel van de service uitgevoerd wordt.
* Wanneer enkele van de apparaten die zijn geregistreerd bij de service via de versleuteling hebt hersteld, terwijl andere gebruikers niet hebben, kunt u een apparaat autoriseren. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Stap 2: Gebruik Windows PowerShell voor StorSimple initiëren service data encryption key wijzigen
Deze stap wordt uitgevoerd in de Windows PowerShell voor StorSimple-interface op het geautoriseerde StorSimple-apparaat.

> [!NOTE]
> Er zijn geen bewerkingen kunnen in de Azure portal van uw StorSimple Manager-service worden uitgevoerd totdat de overschakeling van de sleutel is voltooid.
> 
> 

Als u de seriële console van het apparaat verbinding maken met de Windows PowerShell-interface gebruikt, moet u de volgende stappen uitvoeren.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Starten van service data encryption key wijzigen
1. Selecteer optie 1 om aan te melden met volledige toegang.
2. Typ het volgende achter de opdrachtprompt:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Nadat de cmdlet is voltooid, ontvangt u een nieuwe versleutelingssleutel van de service-gegevens. Kopieer en sla deze sleutel voor gebruik in stap 3 van dit proces. Deze sleutel wordt gebruikt om bij te werken van alle resterende apparaten die zijn geregistreerd bij de StorSimple Manager-service.
   
   > [!NOTE]
   > Dit proces moet worden gestart binnen vier uur na het autoriseren van een StorSimple-apparaat.
   > 
   > 
   
   Deze nieuwe sleutel is vervolgens naar de service moet worden gepusht naar alle apparaten die zijn geregistreerd bij de service verzonden. Een waarschuwing wordt vervolgens weergegeven op het servicedashboard. De service worden alle bewerkingen op de geregistreerde apparaten uitschakelen en vervolgens de apparaatbeheerder moet bijwerken van de gegevensversleutelingssleutel van service op de andere apparaten. De i/o's (hosts verzenden van gegevens naar de cloud) worden echter niet verstoord.
   
   Als u één apparaat geregistreerd met uw service hebt, de rollover is nu voltooid en kunt u de volgende stap overslaan. Als u meerdere apparaten die zijn geregistreerd met uw service hebt, gaat u verder met stap 3.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Stap 3: Bijwerken van de gegevensversleutelingssleutel van service op andere StorSimple-apparaten
Deze stappen moeten worden uitgevoerd in de Windows PowerShell-interface van uw StorSimple-apparaat als er meerdere apparaten zijn geregistreerd met uw StorSimple Manager-service. De sleutel die u hebt verkregen in stap 2 moet worden gebruikt om bij te werken van alle de resterende StorSimple-apparaat geregistreerd bij de StorSimple Manager-service.

Voer de volgende stappen uit voor het bijwerken van de versleuteling van de service-gegevens op uw apparaat.

#### <a name="to-update-the-service-data-encryption-key-on-physical-devices"></a>Bijwerken van de gegevensversleutelingssleutel van service op de fysieke apparaten
1. Windows PowerShell voor StorSimple gebruiken voor het verbinding maken met de console. Selecteer optie 1 om aan te melden met volledige toegang.
2. Typ het volgende achter de opdrachtprompt:`Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Geef de versleutelingssleutel van de service-gegevens die u hebt verkregen in [stap 2: gebruik Windows PowerShell voor StorSimple starten van de service gegevens versleuteling belangrijke wijziging](#to-initiate-the-service-data-encryption-key-change).

#### <a name="to-update-the-service-data-encryption-key-on-all-the-80108020-cloud-appliances"></a>Bijwerken van de gegevensversleutelingssleutel van service op alle apparaten van de 8010/8020 cloud
1. Downloaden en installeren [Update CloudApplianceServiceEncryptionKey.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Update-CloudApplianceServiceEncryptionKey.ps1) PowerShell-script. 
2. Open PowerShell en typ het volgende achter de opdrachtprompt:`Update-CloudApplianceServiceEncryptionKey.ps1 -SubscriptionId [subscription] -TenantId [tenantid] -ResourceGroupName [resource group] -ManagerName [device manager]`

Dit script zorgt ervoor dat gegevensversleutelingssleutel van service is ingesteld op de 8010/8020 cloud toestellen onder Apparaatbeheer.

## <a name="supported-operations-on-devices-running-versions-prior-to-update-50"></a>Ondersteunde bewerkingen op apparaten met eerdere versies Update 5.0
In de Azure portal, worden alleen de StorSimple-apparaten met Update 5.0 of hoger en ondersteund. De apparaten waarop oudere versies worden in beperkte mate ondersteunt. Nadat u hebt gemigreerd naar de Azure portal, gebruikt u de volgende tabel om te begrijpen welke bewerkingen worden ondersteund op apparaten met eerdere versies Update 5.0.

| Bewerking                                                                                                                       | Ondersteund      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Een apparaat registreren                                                                                                               | Ja            |
| Instellingen voor apparaten zoals algemene-, netwerk- en beveiliging configureren                                                                | Ja            |
| Scannen, downloaden en installeren van updates                                                                                             | Ja            |
| Apparaat deactiveren                                                                                                               | Ja            |
| Apparaat verwijderen                                                                                                                   | Ja            |
| Maken, wijzigen en verwijderen van een volumecontainer                                                                                   | Nee             |
| Maken, wijzigen en verwijderen van een volume                                                                                             | Nee             |
| Maken, wijzigen en verwijderen van een back-upbeleid                                                                                      | Nee             |
| Maak een handmatige back-up                                                                                                            | Nee             |
| Maak een geplande back-up                                                                                                         | Niet van toepassing |
| Herstellen vanuit een back-upset                                                                                                        | Nee             |
| Klonen op een apparaat met Update 3.0 en hoger <br> Het bronapparaat wordt versie vóór de Update 3.0 uitgevoerd.                                | Ja            |
| Klonen op een apparaat met eerdere versies Update 3.0                                                                          | Nee             |
| Failover als Bronapparaat <br> (van een apparaat met versie vóór de Update 3.0 op een apparaat met Update 3.0 en hoger)                                                               | Ja            |
| Failover als doelapparaat <br> (op een apparaat met softwareversie vóór de Update 3.0)                                                                                   | Nee             |
| Een waarschuwing wissen                                                                                                                  | Ja            |
| Back-upbeleid, back-upcatalogus, volumes, volumecontainers, bewaking grafieken, taken en waarschuwingen die zijn gemaakt in de klassieke portal weergeven | Ja            |
| In-of uitschakelen apparaatcontrollers                                                                                              | Ja            |


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [StorSimple-implementatieproces](storsimple-8000-deployment-walkthrough-u2.md).
* Meer informatie over [het beheren van uw StorSimple-opslagaccount](storsimple-8000-manage-storage-accounts.md).
* Meer informatie over het [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).
