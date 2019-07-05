---
title: Azure-resource rolinstellingen in PIM - Azure Active Directory configureren | Microsoft Docs
description: Informatie over het configureren van instellingen voor de sitesysteemrol Azure-resource in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6da92ccd50707e676a1f5d583fe22ae8b3d8b73
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476322"
---
# <a name="configure-azure-resource-role-settings-in-pim"></a>Azure-resource rolinstellingen in PIM configureren

Wanneer u Azure-resource serverfunctie-instellingen configureert, definieert u de standaardinstellingen die worden toegepast op Azure-resource-roltoewijzingen in Azure Active Directory (Azure AD) Privileged Identity Management (PIM). Gebruik de volgende procedures voor het configureren van de werkstroom voor goedkeuring en opgeven wie kan goedkeuren of weigeren.

## <a name="open-role-settings"></a>Rolinstellingen openen

Volg deze stappen om de instellingen voor de rol van een Azure-resource te openen.

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) met een gebruiker die lid is van de [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

1. Klik op de resource die u beheren wilt, zoals een abonnement of beheergroep-groep.

    ![Pagina weergegeven van de resources die kunnen worden beheerd met Azure-resources](./media/pim-resource-roles-configure-role-settings/resources-list.png)

1. Klik op **rolinstellingen**.

    ![Rolinstellingen pagina aanbieding Azure-resourcerollen](./media/pim-resource-roles-configure-role-settings/resources-role-settings.png)

1. Klik op de rol waarvan u de instellingen u wilt configureren.

    ![Pagina rol instelling details weergeven van verschillende instellingen voor toewijzing en activering](./media/pim-resource-roles-configure-role-settings/resources-role-setting-details.png)

1. Klik op **bewerken** om de rol instellingen deelvenster te openen.

    ![Pagina van de functie-instellingen met opties voor het bijwerken van de toewijzing en activering instellingen bewerken](./media/pim-resource-roles-configure-role-settings/resources-role-settings-edit.png)

    Voor elke rol in het deelvenster van de instelling rol zijn er enkele instellingen die u kunt configureren.

## <a name="assignment-duration"></a>Duur van de toewijzing

U kunt kiezen uit twee opties voor toewijzing duur voor elk toewijzingstype (in aanmerking komende en actieve) bij het configureren van instellingen voor een rol. Deze opties worden de standaard maximale duur voor wanneer een lid is toegewezen aan de rollen in PIM.

U kunt een van deze **in aanmerking komende** duur toewijzingsopties:

| | |
| --- | --- |
| **Permanent in aanmerking komende toewijzing toestaan** | Resource-beheerders kunnen permanent in aanmerking komende lidmaatschap toewijzen. |
| **In aanmerking komende toewijzing na verlopen** | Resource-beheerders kunnen vereisen dat voor dat alle in aanmerking komende toewijzingen van een opgegeven begin- en datum hebben. |

En u kunt een van deze **active** duur toewijzingsopties:

| | |
| --- | --- |
| **Permanente actieve toewijzing toestaan** | Resource-beheerders kunnen permanent actief MSDN toewijzen. |
| **Actieve toewijzing na verlopen** | Resource-beheerders kunnen vereisen dat voor dat alle actieve toewijzingen van een opgegeven begin- en datum hebben. |

> [!NOTE] 
> Alle toewijzingen van met een opgegeven datum kunnen worden vernieuwd door beheerders van de resource. Bovendien leden Self-service aanvragen om te kunnen initiëren [verlengen of vernieuwen van roltoewijzingen](pim-resource-roles-renew-extend.md).

## <a name="require-multi-factor-authentication"></a>Meervoudige verificatie vereisen

PIM biedt optionele afdwinging van Azure multi-factor Authentication (MFA) voor twee verschillende scenario's.

### <a name="require-multi-factor-authentication-on-active-assignment"></a>Meervoudige verificatie vereisen bij actieve toewijzing

In sommige gevallen is het raadzaam om toe te wijzen een lid aan een rol voor een korte periode (bijvoorbeeld één dag). In dit geval hoeft ze niet de toegewezen leden aan de aanvraag voor activering. In dit scenario kan geen PIM MFA afdwingen wanneer het lid maakt gebruik van hun roltoewijzing, omdat ze al actief zijn in de rol vanaf het moment dat ze zijn toegewezen.

Om ervoor te zorgen dat de beheerder van de resource die voldoen aan de toewijzing is wie ze beweren te zijn, kunt u MFA afdwingen op actieve toewijzing door het controleren van de **multi-factor Authentication vereisen bij actieve toewijzing** vak.

### <a name="require-multi-factor-authentication-on-activation"></a>Meervoudige verificatie vereisen bij activering

U kunt vereisen dat in aanmerking komende leden van een rol om uit te voeren van MFA voor ze kunnen activeren. Dit proces zorgt ervoor dat de gebruiker die vraagt om de activering is wie ze beweren te zijn met redelijke zekerheid. Afdwingen van deze optie worden beveiligd op kritieke resources in situaties wanneer het gebruikersaccount dat mogelijk is aangetast.

Om te vereisen dat een in aanmerking komend lid om uit te voeren van MFA voor activering, Controleer de **overeenkomende multi-factor Authentication bij activering** vak.

Zie voor meer informatie, [multi-factor authentication (MFA) en PIM](pim-how-to-require-mfa.md).

## <a name="activation-maximum-duration"></a>Maximale activeringsduur

Gebruik de **maximale activeringsduur** schuifregelaar om in te stellen de maximale tijd in uren, die een rol actief blijft voordat het verloopt. Deze waarde kan liggen tussen 1 en 24 uur.

## <a name="require-justification"></a>Reden vereisen

U kunt vereisen dat leden een reden op actieve toewijzing of wanneer ze activeren invoeren. Om te vereisen dat reden, Controleer de **reden vereisen bij actieve toewijzing** vak of de **reden vereisen bij activering** vak.

## <a name="require-approval-to-activate"></a>Goedkeuring vereisen voor activeren

Als u wilt de goedkeuring van een rol wilt activeren, volgt u deze stappen.

1. Controleer de **goedkeuring vereisen voor het activeren** selectievakje.

1. Klik op **fiatteurs selecteren** Selecteer een lid of een groep om deelvenster te openen.

    ![Selecteer een lid of een groep deelvenster fiatteurs selecteren](./media/pim-resource-roles-configure-role-settings/resources-role-settings-select-approvers.png)

1. Selecteer ten minste één lid of de groep en klik vervolgens op **Selecteer**. U kunt een willekeurige combinatie van leden en groepen toevoegen. U moet ten minste één goedkeurder selecteren. Er zijn geen Standaard fiatteurs.

    Kies de gewenste opties wordt weergegeven in de lijst met geselecteerde goedkeurders.

1. Nadat u uw al uw functie-instellingen hebt opgegeven, klikt u op **Update** uw wijzigingen op te slaan.

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
- [Beveiligingswaarschuwingen voor Azure resource-rollen in PIM configureren](pim-resource-roles-configure-alerts.md)
