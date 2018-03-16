---
title: Het opsporen van fouten met de Azure Active Directory verbonden service
description: De service active directory verbonden een incompatibel verificatietype gedetecteerd
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: ab81c3385479a96fbfa7e68c4e81129ff327ed4b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Oplossen van problemen met de Azure Active Directory verbonden Service

Tijdens het detecteren van eerdere verificatiecode op te geven, de Azure Active Directory connect-server heeft een incompatibel verificatietype gedetecteerd.

Om te detecteren correct vorige verificatiecode op te geven in een project, moet het project worden gebouwd.  Als u deze fout is opgetreden en er geen een vorige verificatiecode op te geven in uw project, opnieuw en probeer het opnieuw.

## <a name="project-types"></a>Projecttypen

De gekoppelde service controleert het type project dat u ontwikkelt zodat deze de juiste verificatielogica in het project invoeren kunt. Als er domeincontroller die is afgeleid van `ApiController` in het project, het project wordt beschouwd als een WebAPI-project. Als er alleen domeincontrollers die zijn afgeleid van `MVC.Controller` in het project, het project wordt beschouwd als een MVC-project. De gekoppelde service biedt geen ondersteuning voor een ander projecttype.

## <a name="compatible-authentication-code"></a>Compatibel verificatiecode op te geven

Verificatie-instellingen die eerder zijn geconfigureerd of compatibel zijn met de service controleert ook de gekoppelde service. Alle instellingen zijn, dit als een herhaalde aanvraag wordt beschouwd als de gekoppelde service, verschijnt de instellingen worden weergegeven.  Als er slechts enkele van de instellingen aanwezig zijn, wordt het beschouwd als een foutaanvraag.

In een MVC-project controleert de gekoppelde service voor een van de volgende instellingen die het gevolg van eerdere gebruik van de service:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

De gekoppelde service controleert bovendien voor een van de volgende instellingen in een Web API-project die het gevolg van eerdere gebruik van de service:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

## <a name="incompatible-authentication-code"></a>Niet-compatibele verificatiecode op te geven

Ten slotte probeert de gekoppelde service voor het detecteren van versies van de verificatiecode op te geven die zijn geconfigureerd met eerdere versies van Visual Studio. Als u deze fout ontvangen, betekent dit dat het project bevat een incompatibel verificatietype. De gekoppelde service detecteert de volgende soorten verificatie van eerdere versies van Visual Studio:

* Windows-verificatie
* Afzonderlijke gebruikersaccounts
* Organisatieaccounts

Om te detecteren Windows-verificatie in een MVC-project, de verbonden zoekt de `authentication` -element in uw `web.config` bestand.

```xml
<configuration>
    <system.web>
        <span style="background-color: yellow"><authentication mode="Windows" /></span>
    </system.web>
</configuration>
```

Om te detecteren Windows-verificatie in een Web API-project, de gekoppelde service zoekt de `IISExpressWindowsAuthentication` element in uw project `.csproj` bestand:

```xml
<Project>
    <PropertyGroup>
        <span style="background-color: yellow"><IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication></span>
    </PropertyGroup>
</Project>
```

Om te detecteren afzonderlijke gebruikersaccounts verificatie, de gekoppelde service zoekt naar het element van het pakket in uw `packages.config` bestand.

```xml
<packages>
    <span style="background-color: yellow"><package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /></span>
</packages>
```

Om te detecteren een oude vorm van verificatie van de organisatie-Account, de gekoppelde service zoekt naar het volgende element in`web.config`:

```xml
<configuration>
    <appSettings>
        <span style="background-color: yellow"><add key="ida:Realm" value="***" /></span>
    </appSettings>
</configuration>
```

Het verificatietype wijzigen, verwijdert u het verificatietype dat niet compatibel en probeer het opnieuw toe te voegen de gekoppelde service.

Zie voor meer informatie [verificatie scenario's voor Azure AD](active-directory-authentication-scenarios.md).