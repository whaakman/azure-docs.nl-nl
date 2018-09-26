---
title: Upgrade van het Collector-apparaat in Azure Migrate | Microsoft Docs
description: Bevat informatie over upgrades voor het Azure Migrate Collector-apparaat.
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: b6552714326c51c7e1ed0cbda124b68b7faa159b
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47180475"
---
# <a name="collector-appliance-upgrades"></a>Upgrades van collector-apparaat

In dit artikel bevat een overzicht van de upgrade-informatie voor de Collector-apparaat in [Azure Migrate](migrate-overview.md).

De Azure Migrate Collector is een lichtgewicht apparaat dat wordt gebruikt voor het detecteren van een on-premises vCenter-omgeving voor evaluatiedoeleinden vóór de migratie naar Azure. [Meer informatie](concepts-collector.md).


## <a name="upgrade-versions"></a>Upgrade-versies

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

### <a name="version-10911"></a>Versie 1.0.9.11

Hash-waarden voor de upgrade [1.0.9.11 pakket](https://aka.ms/migrate/col/upgrade_9_11)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 0e36129ac5383b204720df7a56b95a60
SHA1 | aa422ef6aa6b6f8bc88f27727e80272241de1bdf
SHA256 | 5f76dbbe40c5ccab3502cc1c5f074e4b4bcbf356d3721fd52fb7ff583ff2b68f

### <a name="version-1097"></a>Versie 1.0.9.7

Hash-waarden voor de upgrade [pakket 1.0.9.7](https://aka.ms/migrate/col/upgrade_9_7)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6


## <a name="run-an-upgrade"></a>Een upgrade uitvoert

U kunt de Collector upgraden naar de meest recente versie zonder het ova-bestand opnieuw te downloaden.

1. U downloaden het meest recente upgradepakket in de onderstaande lijst.
2. Om ervoor te zorgen dat de gedownloade hotfix veilig is, opent u het opdrachtvenster beheerder en voer de volgende opdracht voor het genereren van de hash voor het ZIP-bestand. De gegenereerde hash moet overeenkomen met de hash op basis van de specifieke versie vermeld:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    Voorbeeld: **C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)**
3. Kopieer het zip-bestand naar de virtuele machine van de Collector-apparaat.
4. Met de rechtermuisknop op het zip-bestand > **Alles uitpakken**.
5. Met de rechtermuisknop op **Setup.ps1** > **uitvoeren met PowerShell**, en volgt u de installatie-instructies.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](concepts-collector.md) over het Collector-apparaat.
- [Voer een evaluatie uit](tutorial-assessment-vmware.md) voor VMware-VM's.
