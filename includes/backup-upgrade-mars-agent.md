---
title: Upgrade de Azure Backup agent
description: Deze informatie wordt uitgelegd waarom de Azure Backup agent upgraden en downloaden van de upgrade.
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
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
ms.locfileid: "29973296"
---
## <a name="upgrade-the-mars-agent"></a>Werk de MARS-agent
Versies van de Microsoft Azure Recovery Service (MARS) Agent hieronder 2.0.9083.0 hebben een afhankelijkheid op de Azure Access Control-service (ACS). In 2018 Azure wordt [afschaffen van de Azure Access Control-service (ACS)](../articles/active-directory/develop/active-directory-acs-migration.md). Vanaf 19 maart 2018, ervaren alle versies van de agent MARS hieronder 2.0.9083.0 mislukte back-ups. Om te voorkomen of oplossen van de back-upfouten [uw MARS-Agent bijwerken naar de nieuwste versie](https://go.microsoft.com/fwlink/?linkid=229525). Om te bepalen welke-servers waarvoor u de upgrade van een MARS-Agent, volg de stappen in de [back-up-blog voor Azure Backup-agents bijwerken](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/). De MARS-agent wordt gebruikt voor back-up van de volgende gegevenstypen naar Azure:
- Bestanden 
- Gegevens over de systeemstatus
- back-ups van System Center DPM naar Azure
- Azure Backup-server (MABS)
