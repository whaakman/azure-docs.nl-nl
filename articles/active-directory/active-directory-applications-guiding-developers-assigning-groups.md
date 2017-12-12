---
title: Groepen toewijzen aan Azure AD-apps | Microsoft-Docs
description: Klik hier voor meer informatie over het implementeren van de groepstoewijzing van de voor Azure-toepassingen.
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: 471099c58a7fc1dfd520e693873a76f64c0fbf0a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="assign-azure-active-directory-groups-to-an-application"></a>Azure Active Directory-groepen toewijzen aan een toepassing
Voordat u gebruikers en groepen aan een toepassing toewijzen kunt, moet u de toewijzing van gebruiker vereisen. Zie voor informatie over het vereisen van de gebruiker is toegewezen, de [Gebruikerstoewijzing vereisen](active-directory-applications-guiding-developers-requiring-user-assignment.md) artikel.

In dit artikel wordt ervan uitgegaan dat u groepen in de active directory die u voor deze toepassing gebruikt al hebt gemaakt.

## <a name="assigning-groups-to-an-application"></a>Groepen toewijzen aan een toepassing
1. Aanmelden bij de Azure-portal met een administratoraccount.
2. Klik op de **alle Items** item in het hoofdmenu.
3. Kies de map die u voor de toepassing gebruikt.
4. Klik op de **toepassingen** tabblad.
5. Selecteer de toepassing uit de lijst met toepassingen die zijn gekoppeld aan deze map.
6. Klik op de **gebruikers en groepen** tabblad.
7. De lijst met groepen in uw active directory filteren met behulp van de **groepen** vervolgkeuzelijst.
8. Selecteer de groep.
9. Klik op **toewijzen**.
10. Klik op **Ja** wanneer u wordt gevraagd.

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
