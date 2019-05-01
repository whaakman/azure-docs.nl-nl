---
title: Maak een nieuwe toegang-pakket in Azure AD recht management (Preview) - Azure Active Directory
description: Informatie over het maken van een nieuw pakket voor toegang tot resources die u wilt delen in Azure Active Directory waar u recht op management (Preview).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/24/2019
ms.author: rolyon
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ad6570a3f30e40e4074502a8ce85bf739f58d3f
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64866421"
---
# <a name="create-a-new-access-package-in-azure-ad-entitlement-management-preview"></a>Maak een nieuwe toegang-pakket in beheer van de rechten van een Azure AD (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Een pakket toegang kunt u een eenmalige installatie van resources en beleidsregels die automatisch worden beheerd door de toegang voor de levensduur van het pakket toegang. In dit artikel wordt beschreven hoe u een nieuwe toegang-pakket maken.

## <a name="overview"></a>Overzicht

Alle toegang tot pakketten moeten in een container met de naam een catalogus worden geplaatst. Een catalogus definieert welke resources u aan uw pakket toegang toevoegen kunt. Als u een catalogus niet opgeeft, wordt uw pakket toegang in de algemene catalogus worden geplaatst. U kunt een bestaand pakket met toegang op dit moment niet verplaatsen naar een andere catalogus.

Alle toegang tot pakketten moeten ten minste één beleid hebben. Beleidsregels opgeven die de toegang tot pakket en ook instellingen voor goedkeuring en de vervaldatum kan aanvragen. Wanneer u een nieuwe toegang-pakket maakt, kunt u een eerste beleid voor gebruikers in uw directory, voor gebruikers die niet in uw directory, voor de beheerder alleen directe toewijzingen of u kunt het beleid voor een later tijdstip maken.

Het volgende diagram toont het proces op hoog niveau om een nieuwe toegang-pakket te maken.

![Een toegangsproces-pakket maken](./media/entitlement-management-access-package-create/access-package-process.png)

## <a name="start-new-access-package"></a>Start de nieuwe toegang-pakket

**Vereiste rol:** Gebruikerbeheerder of eigenaar van catalogus

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Klik op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten**.

    ![Beheer van rechten in de Azure-portal](./media/entitlement-management-shared/elm-access-packages.png)

1. Klik op **nieuwe toegang-pakket**.

## <a name="basics"></a>Basics

Op de **basisbeginselen** tabblad u het pakket met toegang tot een naam geven en een opgeven welke catalogus voor het maken van de access-pakket in.

1. Voer een weergavenaam en beschrijving voor het pakket toegang. Gebruikers zien deze informatie bij het indienen van een aanvraag voor het pakket toegang.

1. In de **catalogus** vervolgkeuzelijst, selecteer de catalogus die u wilt maken van de toegang tot het pakket in. Bijvoorbeeld, mogelijk hebt u de eigenaar van een catalogus die u beheert de marketing resources dat kunnen worden aangevraagd. In dit geval kunt u de marketing-catalogus selecteren.

    U ziet alleen catalogi u gemachtigd bent om toegang tot pakketten in te maken. Toegang om pakket te maken in een bestaande-catalogus, moet u ten minste een beheerder van de gebruiker, de eigenaar van catalogus of Pakketbeheer voor toegang.

    ![Toegang tot de pakket - basisbeginselen](./media/entitlement-management-access-package-create/basics.png)

    Als u uw access-pakket in een nieuwe catalogus maken wilt, klikt u op **nieuw**. Voer de naam van de gegevenscatalogus en beschrijving en klik vervolgens op **maken**.

    Het pakket toegang die u wilt maken en alle resources in is opgenomen wordt toegevoegd aan de nieuwe catalogus. Bovendien wordt u automatisch de eerste eigenaar van de catalogus. U kunt extra catalogus eigenaren toevoegen.

    Voor het maken van een nieuwe catalogus, moet u ten minste een beheerder of maker van de catalogus.

1. Klik op **volgende**.

## <a name="resource-roles"></a>Resourcerollen

Op de **resourcerollen** tabblad, selecteert u de resources wilt opnemen in de access-pakket.

1. Klik op het resourcetype die u wilt toevoegen (**groepen**, **toepassingen**, of **SharePoint-sites**).

1. Selecteer een of meer resources in de lijst in het deelvenster selecteren die wordt weergegeven.

    ![Toegang tot package - Resource-rollen](./media/entitlement-management-access-package-create/resource-roles.png)

    Als u het pakket toegang in de algemene of een nieuwe catalogus maakt, wordt u mogelijk zijn om op te halen van elke bron uit de map waarvan u eigenaar bent. U moet de Gebruikersbeheerder van ten minste een of Maker-catalogus.

    Als u het pakket toegang in een bestaande catalogus maakt, kunt u een resource die is al in de catalogus zonder die eigenaar is deze selecteren.

    Als u een beheerder of cataloguseigenaar van de bent, hebt u de extra optie van het selecteren van jou resources die niet nog in de catalogus zijn. Als u resources op dit moment niet in de geselecteerde catalogus selecteert, worden deze resources wordt ook toegevoegd aan de catalogus voor andere catalogusbeheerders om toegang tot pakketten met te bouwen. Als u alleen wilt selecteren van resources die zich momenteel in de geselecteerde catalogus, Controleer de **zien alleen** selectievakje aan de bovenkant van de optie pannen.

1. Nadat u de resources hebt geselecteerd de **rol** , selecteert u de rol die u wilt dat gebruikers worden toegewezen voor de resource.

    ![Toegang tot package - Resource een cloudrol kiezen](./media/entitlement-management-access-package-create/resource-roles-role.png)

1. Klik op **volgende**.

## <a name="policy"></a>Beleid

Op de **beleid** tabblad, maakt u het eerste beleid om op te geven die de toegang tot pakket en ook instellingen voor goedkeuring en de vervaldatum kan aanvragen. U kunt later meer beleidsregels zodat extra groepen van gebruikers te vragen van de access-pakket met hun eigen instellingen voor goedkeuring en de vervaldatum maken. U kunt ook het beleid voor een later tijdstip maken.

1. Stel de **eerste beleid maken** overzet naar **nu** of **Later**.

    ![Toegang tot de pakket - beleid](./media/entitlement-management-access-package-create/policy.png)

1. Als u selecteert **Later**, gaat u naar beneden naar de [revisie + maken](#review--create) sectie om uw access-pakket te maken.

1. Als u selecteert **nu**, volg de stappen in een van de volgende secties van beleid.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="review--create"></a>Beoordelen en maken

Op de **bekijken + maken** tabblad kunt u uw instellingen en Controleer eventuele validatiefouten bekijken.

1. Controleer de instellingen van de access-pakket

    ![Toegang tot package - beleid maken voor de beleidsinstelling](./media/entitlement-management-access-package-create/review-create.png)

1. Klik op **maken** om de toegang-pakket te maken.

    Het nieuwe toegang-pakket wordt weergegeven in de lijst met toegang tot pakketten.

## <a name="next-steps"></a>Volgende stappen

- [Bewerken en beheren van een bestaand pakket met toegang](entitlement-management-access-package-edit.md)
- [Maken en beheren van een catalogus](entitlement-management-catalog-create.md)
