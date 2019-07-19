---
title: 'Azure Active Directory Domain Services: Share point-gebruikers profiel service inschakelen | Microsoft Docs'
description: Azure Active Directory Domain Services beheerde domeinen configureren voor de ondersteuning van profiel synchronisatie voor share Point server
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: iainfou
ms.openlocfilehash: 4a9ee05b37a69927d70269dccef2b74a2c251722
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234104"
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>Een beheerd domein configureren voor de ondersteuning van profiel synchronisatie voor share Point server
Share Point server bevat een gebruikers profiel service die wordt gebruikt voor het synchroniseren van gebruikers profielen. Als u de gebruikers profiel service wilt instellen, moeten de juiste machtigingen worden verleend voor een Active Directory domein. Zie voor meer informatie [Active Directory Domain Services machtigingen verlenen voor profiel synchronisatie in share Point Server 2013](https://technet.microsoft.com/library/hh296982.aspx).

In dit artikel wordt uitgelegd hoe u Azure AD Domain Services beheerde domeinen kunt configureren om de synchronisatie service van de share Point server-gebruikers profielen te implementeren.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="the-aad-dc-service-accounts-group"></a>De groep ' AAD DC-service accounts '
Een beveiligings groep met de naam '**Aad DC-service accounts**' is beschikbaar binnen de organisatie-eenheid ' gebruikers ' op uw beheerde domein. U kunt deze groep zien in de MMC-module **Active Directory gebruikers en computers** in uw beheerde domein.

![Beveiligings groep voor AAD DC-service accounts](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Leden van deze beveiligings groep zijn gemachtigd om de volgende bevoegdheden:
- De bevoegdheid Directory wijzigingen repliceren in de hoofdmap DSE van het beheerde domein.
- De bevoegdheid Directory wijzigingen repliceren in de configuratie naamgevings context (CN = Configuration container) van het beheerde domein.

Deze beveiligings groep is ook lid van de ingebouwde groep **pre-Windows 2000-compatibele toegang**.

![Beveiligings groep voor AAD DC-service accounts](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>Uw beheerde domein inschakelen voor de ondersteuning van synchronisatie van gebruikers profielen van share Point server
U kunt het service account dat wordt gebruikt voor de synchronisatie van share point-gebruikers profielen toevoegen aan de groep **Aad DC-service accounts** . Als gevolg hiervan krijgt het synchronisatie account voldoende bevoegdheden om wijzigingen in de map te repliceren. Met deze configuratie stap kan de synchronisatie van share Point server-gebruikers profielen goed worden uitgevoerd.

![AAD DC-service accounts-leden toevoegen](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD DC-service accounts-leden toevoegen](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Gerelateerde inhoud
* [Technische Naslag informatie over het verlenen van Active Directory Domain Services machtigingen voor profiel synchronisatie in share Point server 2013](https://technet.microsoft.com/library/hh296982.aspx)
