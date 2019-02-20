---
title: Upgrade van het Collector-apparaat in Azure Migrate | Microsoft Docs
description: Bevat informatie over upgrades voor het Azure Migrate Collector-apparaat.
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: f29556c8f2a5aa727ce80632eaccf5e1ed6d7c1a
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415541"
---
# <a name="collector-update-release-history"></a>Releasegeschiedenis van collector bijwerken

In dit artikel bevat een overzicht van de upgrade-informatie voor de Collector-apparaat in [Azure Migrate](migrate-overview.md).

De Azure Migrate Collector is een lichtgewicht apparaat dat wordt gebruikt voor het detecteren van een on-premises vCenter-omgeving voor evaluatiedoeleinden vóór de migratie naar Azure. [Meer informatie](concepts-collector.md).

## <a name="continuous-discovery-upgrade-versions"></a>Continue detectie: Upgrade-versies

Geen upgrade voor het apparaat continue detectie is nog beschikbaar.

## <a name="one-time-discovery-deprecated-now-previous-upgrade-versions"></a>Eenmalige discovery (nu afgeschaft): Vorige versies van de upgrade

> [!NOTE]
> Ondersteuning van het apparaat voor eenmalige detectie is nu beëindigd omdat deze methode gebaseerd was op statistiekinstellingen van vCenter Server voor de beschikbaarheid van prestatiegegevenspunten en gemiddelde prestatiemeteritems verzamelde, wat leidde tot een te voorzichtige schaling van virtuele machines voor migratie naar Azure.

### <a name="version-10916-released-on-10292018"></a>Versie 1.0.9.16 (uitgebracht op 29-10-2018)

Bevat oplossingen voor problemen bij het instellen van het toestel PowerCLI.

Hash-waarden voor de upgrade [1.0.9.16 pakket](https://aka.ms/migrate/col/upgrade_9_16)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | d2c53f683b0ec7aaf5ba3d532a7382e1
SHA1 | e5f922a725d81026fa113b0c27da185911942a01
SHA256 | a159063ff508e86b4b3b7b9a42d724262ec0f2315bdba8418bce95d973f80cfc

### <a name="version-10914"></a>Versie 1.0.9.14

Hash-waarden voor de upgrade [1.0.9.14 pakket](https://aka.ms/migrate/col/upgrade_9_14)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

### <a name="version-10913"></a>Versie 1.0.9.13

Hash-waarden voor de upgrade [1.0.9.13 pakket](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e


## <a name="run-an-upgrade"></a>Een upgrade uitvoert

U kunt de Collector upgraden naar de meest recente versie zonder het ova-bestand opnieuw te downloaden.

1. U downloaden het meest recente upgradepakket in de onderstaande lijst.
2. Om ervoor te zorgen dat de gedownloade hotfix veilig is, opent u het opdrachtvenster beheerder en voer de volgende opdracht voor het genereren van de hash voor het ZIP-bestand. De gegenereerde hash moet overeenkomen met de hash op basis van de specifieke versie vermeld:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    Voorbeeld: **C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)**
3. Kopieer het zip-bestand naar de virtuele machine van de Collector-apparaat.
4. Met de rechtermuisknop op het zip-bestand > **Alles uitpakken**.
5. Met de rechtermuisknop op **Setup.ps1** > **uitvoeren met PowerShell**, en volgt u de installatie-instructies.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](concepts-collector.md) over het Collector-apparaat.
- [Voer een evaluatie uit](tutorial-assessment-vmware.md) voor VMware-VM's.
