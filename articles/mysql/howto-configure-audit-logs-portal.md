---
title: Configureren en toegang tot de auditlogboeken voor Azure Database voor MySQL in Azure portal
description: Dit artikel wordt beschreven hoe u configureert en toegang krijgen tot de controlelogboeken in Azure Database for MySQL via Azure portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: dfa57b3f597ba1c01dc78421cc04074f7b6a858e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063048"
---
# <a name="configure-and-access-audit-logs-in-the-azure-portal"></a>Configureren en toegang tot de auditlogboeken in Azure portal

U kunt configureren dat de [Azure Database for MySQL-auditlogboeken](concepts-audit-logs.md) en diagnostische instellingen van de Azure-portal.

> [!IMPORTANT]
> Audit log functionaliteit is momenteel in preview.

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:

- [Azure Database voor MySQL-server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Audit-logboekregistratie configureren

Inschakelen en configureren van controlegebeurtenissen vastleggen wordt uitgevoerd.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer uw Azure Database voor MySQL-server.

1. Onder de **instellingen** sectie in de zijbalk, selecteer **serverparameters**.
    ![Serverparameters](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. Update de **audit_log_enabled** parameter op ON.
    ![Auditlogboeken inschakelen](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. Selecteer de gebeurtenissen die moeten worden vastgelegd door het bijwerken van de **audit_log_events** parameter.
    ![Gebeurtenissen voor beveiligingscontrole logboek](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. Toevoegen van een MySQL-gebruikers moeten worden uitgesloten van logboekregistratie door bij te werken de **audit_log_exclude_users** parameter. Geef gebruikers door te geven van de naam van de MySQL-gebruiker.
    ![Audit log uitgesloten gebruikers](./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png)

1. Nadat u de parameters hebt gewijzigd, kunt u klikken op **opslaan**. U kunt ook **negeren** uw wijzigingen.
    ![Opslaan](./media/howto-configure-audit-logs-portal/save-parameters.png)

## <a name="set-up-diagnostic-logs"></a>Diagnostische logboeken instellen

1. Onder de **bewaking** sectie in de zijbalk, selecteer **diagnostische instellingen**.

1. Klik op ' + diagnostische instelling toevoegen ' ![diagnostische instelling toevoegen](./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png)

1. Geef een naam voor de diagnostische instelling.

1. Geef op welke gegevens PUT voor het verzenden van de auditlogboeken (storage-account, event hub en/of Log Analytics-werkruimte).

1. Selecteer 'MySqlAuditLogs' als Logboektype.
![Diagnostische instelling configureren](./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png)

1. Wanneer u de gegevenssinks voor het doorgeven van de auditlogboeken voor hebt geconfigureerd, kunt u klikken op **opslaan**.
![Diagnostische instelling opslaan](./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png)

1. Toegang krijgen tot de auditlogboeken verkennen ze in de gegevenssinks die u hebt geconfigureerd. Het duurt maximaal tien minuten voor de logboeken worden weergegeven.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [auditlogboeken](concepts-audit-logs.md) in Azure Database for MySQL.