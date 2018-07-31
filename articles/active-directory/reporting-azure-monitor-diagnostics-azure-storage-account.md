---
title: 'Zelfstudie: Azure Active Directory-logboeken archiveren in een Azure-opslagaccount (preview) | Microsoft Docs'
description: Informatie over het instellen van Azure Diagnostics voor het pushen van Azure Active Directory-logboeken naar een opslagaccount (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ea26da91e2a0b88e8afa54a4210b7a2365e4949b
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240185"
---
# <a name="tutorial-archive-azure-active-directory-logs-to-an-azure-storage-account-preview"></a>Zelfstudie: Azure Active Directory-logboeken archiveren in een Azure-opslagaccount (preview)

In deze zelfstudie leert u diagnostische instellingen van Azure Monitor instellen voor het routeren van Azure Active Directory-logboeken naar een Azure-opslagaccount.

## <a name="prerequisites"></a>Vereisten 

U hebt de volgende zaken nodig:

* Een abonnement op Azure met een Azure-opslagaccount. Als u nog geen Azure-abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/).
* Een Azure Active Directory-tenant.
* Een gebruiker die een globale beheerder of beveiligingsbeheerder voor die tenant is.

## <a name="archive-logs-to-an-azure-storage-account"></a>Activiteitenlogboeken in een Azure-opslagaccount archiveren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Klik op **Azure Active Directory** -> **Activiteit** -> **Auditlogboeken**. 
3. Klik op **Exportinstellingen** om de blade Diagnostische instellingen te openen. Klik op **Instelling bewerken** als u bestaande instellingen wilt bewerken, of klik op **Diagnostische instelling toevoegen** als u een nieuwe wilt toevoegen. Er zijn maximaal drie instellingen mogelijk. 
    ![Exportinstellingen](./media/reporting-azure-monitor-diagnostics-azure-storage-account/ExportSettings.png "Exportinstellingen")

4. Geef de instelling een beschrijvende naam zodat u het doel ervan kunt onthouden. Bijvoorbeeld: Naar Azure-opslagaccount verzenden. 
5. Schakel het selectievakje **Archiveren naar een opslagaccount** in en klik op **Opslagaccount** om het Azure-opslagaccount te kiezen. 
6. Selecteer het Azure-abonnement en het opslagaccount waarnaar u de logboeken wilt routeren. Klik op **OK** om de configuratie af te sluiten.
7. Schakel het selectievakje **AuditLogs** in om auditlogboeken naar het opslagaccount te verzenden. 
8. Schakel het selectievakje **SignInLogs** in om aanmeldingsboeken naar het opslagaccount te verzenden.
9. Gebruik de schuifregelaar om retentie voor uw logboekgegevens in te stellen. Standaard is deze waarde nul en de logboeken worden voor onbepaalde tijd in het opslagaccount bewaard. U kunt ook een waarde instellen zodat gebeurtenissen ouder dan het opgegeven aantal dagen automatisch worden gewist.
10. Klik op **Opslaan** om de instelling op te slaan.
    ![Diagnostische instellingen](./media/reporting-azure-monitor-diagnostics-azure-storage-account/DiagnosticSettings.png "Diagnostische instellingen")

11. Na circa vijftien minuten controleert u of de logboeken naar uw opslagaccount worden gepusht. Ga naar Azure Portal, klik op **Opslagaccounts**, kies het opslagaccount dat u eerder hebt gebruikt en klik op **Blobs**. 
12. Klik voor **Auditlogboeken** op **insights-log-audit**. Klik voor **Aanmeldingslogboeken** op **insights-log-signin**.
    ![Opslagaccount](./media/reporting-azure-monitor-diagnostics-azure-storage-account/StorageAccount.png "Opslagaccount")

## <a name="next-steps"></a>Volgende stappen

* [Auditlogboekenschema interpreteren in Azure Monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Aanmeldingslogboekenschema interpreteren in Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Veelgestelde vragen en bekende problemen](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)