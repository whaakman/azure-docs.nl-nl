---
title: Volgende stappen voor het toegangsbeheer van met behulp van groepen - Azure AD | Microsoft Docs
description: Hoe geavanceerde-om de van voor het beheren van beveiligingsgroepen en hoe u kunt deze groepen gebruiken voor het beheren van toegang tot een resource.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro
ms.openlocfilehash: d84c3b556f27366c249382a37e0a8227726110cf
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714681"
---
# <a name="managing-owners-for-a-group"></a>Eigenaars voor een groep beheren
Wanneer een resource-eigenaar heeft toegang tot een resource aan een Azure AD-groep worden toegewezen, wordt het lidmaatschap van de groep wordt beheerd door de eigenaar van de groep. De eigenaar van de resource delegeert effectief de machtiging voor het toewijzen van gebruikers aan de resource toe aan de eigenaar van de groep.

## <a name="add-an-owner-to-a-group"></a>Eigenaar toevoegen aan een groep

1. In de [Azure AD-beheercentrum](https://aad.portal.azure.com), selecteer **gebruikers en groepen**.
2. Selecteer **alle groepen**, en open vervolgens de groep die u wilt dat eigenaren aan toe te voegen.
3. Selecteer **eigenaren toevoegen**.
4. Op de **eigenaren toevoegen** pagina, selecteert u de gebruiker die u wilt toevoegen als de eigenaar van deze groep en zorg ervoor dat deze naam wordt toegevoegd aan de **geselecteerde** deelvenster.

## <a name="remove-an-owner-from-a-group"></a>Een eigenaar verwijderen uit een groep

1. In de [Azure AD-beheercentrum](https://aad.portal.azure.com), selecteer **gebruikers en groepen**.
2. Selecteer **alle groepen**, en open vervolgens de groep van waaruit u wilt verwijderen van eigenaren.
3. Selecteer de **eigenaren** tabblad.
4. Selecteer de eigenaar die u wilt verwijderen uit deze groep, en selecteer vervolgens **verwijderen**.

## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Azure Active Directory cmdlets for configuring group settings](../users-groups-roles/groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Toepassingsbeheer in Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Wat is Azure Active Directory?](active-directory-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](../connect/active-directory-aadconnect.md)
