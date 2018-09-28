---
title: Een toepassing verwijderen uit Azure Active Directory
description: Informatie over hoe u een toepassing verwijdert uit Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: 2cd77ee6df52940a800733209b7e384d72d98103
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962127"
---
# <a name="quickstart-remove-an-application-from-azure-active-directory"></a>Snelstart: Een toepassing verwijderen uit Azure Active Directory

Bedrijfsontwikkelaars en SaaS-providers (software-as-a-service) die toepassingen hebben geregistreerd bij Azure Active Directory (Azure AD) moeten mogelijk de registratie van een toepassing verwijderen uit hun Azure Active Directory-tenant.

In deze snelstart leert u het volgende:

* [Een toepassing verwijderen die is geschreven door uw organisatie](#removing-an-application-authored-by-your-organization)
* [Een toepassing met meerdere tenants verwijderen die is geautoriseerd door een andere organisatie](#removing-a-multi-tenant-application-authorized-by-another-organization)

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen, moet u een Azure Active Directory-tenant hebben waarvoor toepassingen zijn geregistreerd.

* Als u nog geen tenant hebt, vindt u [hier informatie over het verkrijgen van een tenant](quickstart-create-new-tenant.md).
* Zie [Een toepassing toevoegen aan Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md) voor meer informatie over het toevoegen aan en registreren van apps bij uw tenant.

## <a name="removing-an-application-authored-by-your-organization"></a>Een toepassing verwijderen die is geschreven door uw organisatie

Toepassingen die door uw organisatie zijn geregistreerd, worden weergegeven onder het filter **Mijn apps** op de hoofdpagina **App-registraties** van uw tenant. Deze toepassingen zijn de toepassingen die u handmatig hebt geregistreerd via Azure Portal of met behulp van een programma via PowerShell of de Microsoft Graph API. Om precies te zijn, worden deze toepassingen vertegenwoordigd door zowel een toepassing als het service-principal-object in uw tenant. Zie [Toepassingsobjecten en service-principal-objecten](app-objects-and-service-principals.md) voor meer informatie over deze objecten.

### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Een toepassing met één tenant uit uw directory verwijderen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Als u via uw account toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
1. Selecteer in het navigatiedeelvenster aan de linkerkant de **Azure Active Directory**-service en selecteer **App-registraties**. Zoek en selecteer vervolgens de toepassing die u wilt configureren.
    Hiermee gaat u naar de hoofdregistratiepagina van de toepassing, waarmee de pagina **Instellingen** voor de toepassing wordt geopend.
1. Selecteer **Verwijderen** op de hoofdregistratiepagina van de toepassing.
1. Selecteer **Ja** om te bevestigen dat u de toepassing wilt verwijderen.

### <a name="to-remove-a-multi-tenant-application-from-its-home-directory"></a>Een toepassing met meerdere tenants verwijderen uit de basisdirectory

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Als u via uw account toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
1. Selecteer in het navigatiedeelvenster aan de linkerkant de **Azure Active Directory**-service en selecteer **App-registraties**. Zoek en selecteer vervolgens de toepassing die u wilt configureren.
    Hiermee gaat u naar de hoofdregistratiepagina van de toepassing, waarmee de pagina **Instellingen** voor de toepassing wordt geopend.
1. Kies op de pagina **Instellingen** de optie **Eigenschappen** en wijzig de schakeloptie **Met meerdere tenants** in **Nee** om uw toepassing eerst te wijzigen in één tenant. Selecteer vervolgens **Opslaan**.
    De service-principal-objecten van de toepassing blijven in de tenants van alle organisaties die er al toestemming voor hebben gegeven.
1. Selecteer **Verwijderen** op de hoofdregistratiepagina van de toepassing.
1. Selecteer **Ja** om te bevestigen dat u de toepassing wilt verwijderen.

## <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Een toepassing met meerdere tenants verwijderen die is geautoriseerd door een andere organisatie

Een subset van de toepassingen die worden weergegeven onder het filter **Alle apps** (met uitzondering van de **Mijn apps**-registraties) op de hoofdpagina **App-registraties** van uw tenant, bestaat uit toepassingen met meerdere tenants.

In technische termen betekent dit dat deze toepassingen met meerdere tenants van een andere tenant zijn, en zijn geregistreerd bij uw tenant tijdens het toestemmingsproces. Om precies te zijn, worden deze toepassingen alleen vertegenwoordigd door een service-principal-object in uw tenant, zonder bijbehorend toepassingsobject. Zie [Toepassings- en service-principal-objecten in Azure Active Directory](app-objects-and-service-principals.md) voor meer informatie over de verschillen tussen toepassings- en service-principal-objecten.

Als u de toegang van een toepassing met meerdere tenants tot uw directory wilt verwijderen (nadat u toestemming hebt verleend), moet de bedrijfsbeheerder de service-principal van de toepassing verwijderen. De beheerder moet globale beheerderstoegang hebben en kan deze verwijderen via Azure Portal of de [Azure Active Directory PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) gebruiken.

## <a name="next-steps"></a>Volgende stappen

Lees meer over andere gerelateerde snelstarts voor app-beheer met behulp van het Azure Active Directory v1.0-eindpunt:

- [Een toepassing toevoegen aan Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md)
- [Een toepassing bijwerken in Azure Active Directory](quickstart-v1-update-azure-ad-app.md)
