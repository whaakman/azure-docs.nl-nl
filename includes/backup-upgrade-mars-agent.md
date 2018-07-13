---
title: De Azure Backup-agent bijwerken
description: Deze informatie wordt uitgelegd waarom de Azure backup-agent upgraden en het downloaden van de upgrade.
services: backup
cloud: ''
suite: ''
author: markgalioto
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 3/16/2018
ms.author: markgal
ms.openlocfilehash: 4142f88c3ccb376acbdd2af07c5cfdf8fd275780
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38750327"
---
## <a name="upgrade-the-mars-agent"></a>De MARS-agent bijwerken
Versies van de Agent van Microsoft Azure Recovery Service (MARS) hieronder 2.0.9083.0 hebben een afhankelijkheid op de Azure Access Control service (ACS). In 2018 Azure wordt [afschaffen van de Azure Access Control service (ACS)](../articles/active-directory/develop/active-directory-acs-migration.md). Vanaf 19 maart 2018, krijgen alle versies van de MARS-agent hieronder 2.0.9083.0 mislukte back-ups. Om te voorkomen of oplossen van back-upfouten [uw MARS-Agent bijwerken naar de meest recente versie](https://go.microsoft.com/fwlink/?linkid=229525). Voor het identificeren van servers waarvoor een upgrade van de MARS-Agent, volg de stappen in de [back-up-blog voor het upgraden van Azure Backup-agents](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/). De MARS-agent wordt gebruikt om back-up van de volgende gegevenstypen naar Azure:
- Bestanden 
- Gegevens over de systeemstatus
- back-ups van System Center DPM naar Azure
- Azure Backup-server (MABS)
