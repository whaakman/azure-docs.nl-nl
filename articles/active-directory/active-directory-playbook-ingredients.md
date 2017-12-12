---
title: "Azure Active Directory implementatiemodel Playbook ingrediënten | Microsoft Docs"
description: Verkennen en snel implementeren scenario's voor Identity and Access Management
services: active-directory
keywords: Azure active directory, playbook, Proof-of-Concept, implementatiemodel
documentationcenter: 
author: dstefanMSFT
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: dstefan
ms.openlocfilehash: ff4a8601b619837d835ec25c26b1f7e69b46ae85
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a>Azure Active Directory-bewijs van Concept Playbook ingrediënten 

## <a name="theme"></a>Thema
Azure AD levert de oplossingen voor identiteits- en toegangsbeheer via meerdere gebieden in de onderneming. Classificeren van de scenario's in de volgende gebieden: 

* [Veel apps, één identiteit](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [De beveiliging verbeteren](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [Schaal met selfservice](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

Voor het definiëren van een thema wilt toevoegen aan de POC-fase zorgt de concentreren dat binnen met zakelijke doelstellingen die vaak is het mogelijk zijn de triggers van de interesse in een POC in de eerste plaats. 

## <a name="environment"></a>Omgeving

Het is belangrijk om te bepalen van de details van de omgeving waarin u de POC-fase zullen leveren. U kunt in het ideale geval bouwen voort op het nadat de POC-fase is voltooid. De doelomgeving is van cruciaal belang en vindt u de juiste balans tussen deze als echte mogelijk en de verwerkingstijd van de beperkingen of overwegingen zijn extra maken. De typische omgevingen voor POC's zijn:
* **Productie:** de scenario's zullen worden uitgevoerd in uw productieomgeving en Microsoft Cloud-services (productie AD, Office 365, Azure AD-tenant/SSO oplossing) al is geïmplementeerd. 
* **Gebruiker acceptatie testen (UAT) / Dev-omgeving:** u test-infrastructuur hebt (parallelle AD en mogelijk Azure AD-tenant/SSO oplossing) met testgegevens dat lijkt op productie. De testomgeving worden doorgaans verdeeld over meerdere projecten in de onderneming.

De meeste scenario's in deze handleiding zijn additieve aard. Hierdoor kunnen kunnen ze worden geïmplementeerd in de productie-tenant zonder gebruikers buiten de POC-fase. In dit document bellen we uit welke scenario's tenant wide gevolgen zou hebben. In deze gevallen is het raadzaam rekening houden met een niet-productieomgeving. 


## <a name="target-users"></a>Doelgebruikers

Het is belangrijk om te bepalen welke doel van gebruikers die de scenario's, oefent vooral wanneer de omgeving testomgeving is. De categorieën van doelgebruikers voor implementatiemodel zijn:
* **Testgebruikers:** echte gebruikers in de omgeving die de oplossing gaat gebruiken met het account dat ze voor hun dagelijkse taakfuncties gebruiken
* **Testgebruikers:** accounts die zijn gemaakt in de omgeving testen 

De meeste scenario's in deze handleiding kunnen worden uitgeoefend door testgebruikers. In dit document zullen we contact uit doel gebruiker overwegingen indien nodig.


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]