---
title: Verbinding maken met een account met Google Cloud Platform naar Azure kosten Management | Microsoft Docs
description: Verbinding maken met een account Google Cloud Platform kosten weergeven en gebruiksgegevens in kostenbeheer repots.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/26/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: b7141d028c496b75b061c17aa19ad6c73c0588c0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="connect-a-google-cloud-platform-account"></a>Verbinding maken met een account met Google Cloud Platform

U kunt uw bestaande Google Cloud Platform-account verbinding maken met Azure kosten Management. Nadat u uw account verbinding met kostenbeheer, is kosten en gebruiksgegevens beschikbaar in kosten-rapporten. In dit artikel helpt u bij het configureren en verbinden van uw Google-account kostenbeheer.

## <a name="collect-project-information"></a>Projectinformatie verzamelen

Begint u met het verzamelen van gegevens over uw project.

1. Aanmelden bij de console van Google Cloud Platform op [ https://console.cloud.google.com ](https://console.cloud.google.com).
2. De project-informatie die u voorbereiden voor beheer van kosten en opmerking wilt de **projectnaam** en de **Project-ID**. De gegevens voor de volgende stappen bij de hand houden.  
    ![Google Cloud Platform-console](./media/connect-google-account/gcp-console01.png)
3. Als facturering niet is ingeschakeld en is gekoppeld aan uw project, maakt u een rekening. Zie voor meer informatie [Maak een nieuwe factureringsaccount](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create\_a\_new\_billing\_account).

## <a name="enable-storage-bucket-billing-export"></a>Opslag bucket facturering exporteren inschakelen

Beheer van kosten haalt uw Google gegevens uit een opslag-bucket facturering. Houd de **Bucketnaam** en **rapport voorvoegsel** informatie handig voor later gebruik tijdens de registratie van beheer van kosten.

Minimale kosten leidt ertoe dat Google Cloud-opslag gebruiken voor het opslaan van gebruiksrapporten. Zie voor meer informatie [prijzen van Cloud-opslag](https://cloud.google.com/storage/pricing).

1. Als facturering exporteren naar een bestand niet is ingeschakeld, volgt u de instructies op de [het inschakelen van facturering exporteren naar een bestand](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file). U kunt de exportindeling van de JSON- of CSV-facturering.
2. Anders wordt in de console van Google Cloud Platform, navigeer naar **facturering** > **facturering export**. Houd er rekening mee uw facturering **Bucketnaam** en **rapport voorvoegsel**.  
    ![Facturering exporteren](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Google Cloud Platform-API 's

Voor het verzamelen van informatie over het gebruik en activa-informatie moet kostenbeheer de volgende Google Cloud Platform API's ingeschakeld:

- BigQuery API
- Google Cloud SQL
- Google Cloud Datastore API
- Google Cloud-opslag
- Google Cloud-opslag JSON-API
- Google Compute Engine-API

### <a name="enable-or-verify-apis"></a>In- of Controleer of API 's

1. Selecteer het project dat u wilt registreren met kosten Management in de console van Google Cloud Platform.
2. Navigeer naar **API's en Services** > **bibliotheek**.
3. Zoeken gebruiken om dat elke API voor de eerder vermelde.
4. Controleer voor elke API **API ingeschakeld** wordt weergegeven. Klik anders op **inschakelen**.

## <a name="add-a-google-cloud-account-to-cost-management"></a>Een Google Cloud-account toevoegen aan kostenbeheer

1. De portal Cloudyn openen vanuit de Azure-portal of Ga naar [ https://azure.cloudyn.com ](https://azure.cloudyn.com/) en zich aanmelden.
2. Klik op **instellingen** (tandwiel symbol) en selecteer vervolgens **Accounts in de Cloud**.
3. In **accountbeheer**, selecteer de **Google-Accounts** tabblad en klik vervolgens op **nieuwe toevoegen +**.
4. In **Google-accountnaam**, voer het e-mailadres voor de facturering account en klik vervolgens op **volgende**.
5. In het dialoogvenster voor Google-verificatie selecteren of geef een Google-account en vervolgens **toestaan** cloudyn.com toegang tot uw account.
6. De gegevens van de aanvraag-project toevoegen dat moest u de vorige hebt genoteerd. Ze bevatten **Project-ID**, **Project** naam **facturering** Bucketnaam, en **facturering bestand** voorvoegsel rapporteren en klik vervolgens op  **Sla**.  
    ![Google project toevoegen](./media/connect-google-account/add-project.png)

Uw Google-account wordt weergegeven in de lijst van accounts en moet er **geverifieerde**. Klik hieronder moet uw Google-projectnaam en -ID worden weergegeven en een groen vinkje symbool hebben. Status van de account moet spreken **voltooid**.

Binnen een paar uur rapporten kostenbeheer met informatie over de kosten en informatie over het gebruik van Google.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over het beheer van Azure kosten, blijven de [gebruik en kosten bekijken](./tutorial-review-usage.md) zelfstudie voor het beheer van kosten.
