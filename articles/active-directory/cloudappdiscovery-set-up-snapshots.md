---
title: Cloud App Discovery momentopname rapporten maken in Azure Active Directory | Microsoft Docs
description: Bevat informatie over het zoeken en beheren van toepassingen met Cloud App Discovery, wat zijn de voordelen en hoe het werkt.
services: active-directory
keywords: cloud app discovery, het beheren van toepassingen
documentationcenter: 
author: curtand
manager: mtillman
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: curtand
ms.reviewer: nigu
ms.custom: it-pro
ms.openlocfilehash: 69a633b39e68596c536700dbb2c7c8d35f3b44bb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-cloud-app-discovery-snapshot-reports"></a>Cloud App Discovery momentopname rapporten maken

Voordat u de automatische logboekverzamelaar instelt, een logboek handmatig uploaden en laat het parseren van Cloud App Security. Als u een logboek nog geen hebt en u een voorbeeld wilt van hoe uw logboek eruit moet zien, gebruikt u de onderstaande procedure voor het downloaden van een voorbeeldbestand logboek om te zien wat uw logboek moet eruitzien als.

## <a name="to-create-a-snapshot-report"></a>Een momentopname van een rapport maken

1. Verzamelen van logboekbestanden van uw firewall en proxy-server via welke gebruikers in uw organisatie toegang het Internet tot. Logboeken verzamelt tijdens piekverkeer die representatief voor de gebruikersactiviteit in uw organisatie zijn.
2. Op [de menubalk van Cloud App Security](https://portal.cloudappsecurity.com), selecteer **Discover**, en vervolgens **momentopname van een rapport maken**.
  
  ![Momentopname van een nieuw rapport maken](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-command.png)
3. Voer een **rapportnaam** en een **beschrijving**.
    
  ![Nieuwe momentopname van een rapport](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-form.png)
4. Selecteer de **gegevensbron** waaruit u wilt uploaden van de logboekbestanden.
5. Controleer of uw logboekindeling om ervoor te zorgen dat deze volgens het voorbeeld dat kunt u downloaden juist is opgemaakt. Klik op **weergeven en controleer of** en klik vervolgens op **downloaden voorbeeldlogboek**. Vergelijk uw logboek met het voorbeeld dat is opgegeven om te controleren of dat het compatibel is.
  
  ![Controleer of uw logboekindeling](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-verify.png)
  >  [!NOTE]
  > De FTP-voorbeeld-indeling wordt ondersteund in momentopnamen en geautomatiseerde uploaden terwijl syslog automatisch uploaden alleen wordt ondersteund. Een voorbeeldlogboek te downloaden, downloadt een FTP-voorbeeldlogboek.
6. **Kies de verkeerslogboeken** die u wilt uploaden. U kunt maximaal 20 bestanden tegelijk uploaden. Gecomprimeerd en gecomprimeerde bestanden worden ook ondersteund.
  
7. Klik op **Create**. Nadat het uploaden is voltooid, kan het even duren om te worden geparseerd en geanalyseerd. Als dit het geval is, verzendt Cloud App Discovery een e-mailbericht wanneer ze klaar bent.

8. Selecteer **momentopname rapporten beheren** en selecteer uw momentopname van een rapport.
  
  ![Beheer van momentopnamen rapport](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-manage.png)

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Cloud App Discovery in Azure AD](cloudappdiscovery-get-started.md)
* [Logboek automatisch uploaden voor het melden van continue configureren](https://docs.microsoft.com/cloud-app-security/discovery-docker)
* [Een aangepaste logboekparser gebruiken](https://docs.microsoft.com/cloud-app-security/custom-log-parser)
