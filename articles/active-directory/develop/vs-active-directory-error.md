---
title: Het opsporen van fouten met de Azure Active Directory verbonden service
description: De verbonden active directory-service heeft een niet-compatibele verificatietype gedetecteerd
services: active-directory
author: ghogen
manager: douge
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 82449c3a8154142a64aa264f72d2dec75fe2f23c
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42055690"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Diagnose van fouten met de Azure Active Directory Connected Service

Tijdens het detecteren van vorige verificatiecode op te geven, de Azure Active Directory connect-server heeft een niet-compatibele verificatietype dat wordt gedetecteerd.

Als u wilt detecteren correct vorige verificatiecode op te geven in een project, moet het project worden gebouwd.  Als u deze fout is opgetreden en u geen een vorige verificatiecode op te geven in uw project, opnieuw en probeer het opnieuw.

## <a name="project-types"></a>Projecttypen

De gekoppelde service controleert het type project dat u ontwikkelt, zodat deze de juiste verificatielogica in het project invoeren kunt. Als er een domeincontroller die is afgeleid van `ApiController` in het project, het project wordt beschouwd als een WebAPI-project. Als er alleen domeincontrollers die zijn afgeleid van `MVC.Controller` in het project, het project wordt beschouwd als een MVC-project. De gekoppelde service biedt geen ondersteuning voor een ander projecttype.

## <a name="compatible-authentication-code"></a>Compatibel verificatiecode op te geven

Verificatie-instellingen die eerder zijn geconfigureerd of compatibel zijn met de service controleert ook de gekoppelde service. Alle instellingen aanwezig zijn, dit als een herhaalde aanvraag wordt beschouwd als de gekoppelde service, verschijnt de instellingen worden weergegeven.  Als er slechts enkele van de instellingen aanwezig zijn, wordt deze beschouwd als een foutaanvraag.

In een MVC-project controleert de gekoppelde service voor het gebruik van de volgende instellingen, die het resultaat zijn van vorige gebruik van de service:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Bovendien worden de verbonden service voor het gebruik van de volgende instellingen in een Web-API-project die het resultaat zijn van vorige gebruik van de service controleert:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

## <a name="incompatible-authentication-code"></a>Niet-compatibele verificatiecode op te geven

Ten slotte, probeert de gekoppelde service voor het detecteren van versies van de verificatiecode op te geven die zijn geconfigureerd met eerdere versies van Visual Studio. Als u deze fout ontvangen, betekent dit dat het project bevat een niet-compatibele verificatietype. De gekoppelde service detecteert de volgende typen verificatie van eerdere versies van Visual Studio:

* Windows-verificatie
* Afzonderlijke gebruikersaccounts
* Organisatie-Accounts

Voor het detecteren van Windows-verificatie in een MVC-project, het verbonden zoekt de `authentication` -element in uw `web.config` bestand.

```xml
<configuration>
    <system.web>
        <span style="background-color: yellow"><authentication mode="Windows" /></span>
    </system.web>
</configuration>
```

Voor het detecteren van Windows-verificatie in een Web-API-project, de gekoppelde service zoekt de `IISExpressWindowsAuthentication` -element in van uw project `.csproj` bestand:

```xml
<Project>
    <PropertyGroup>
        <span style="background-color: yellow"><IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication></span>
    </PropertyGroup>
</Project>
```

Voor het detecteren van afzonderlijke gebruikersaccounts verificatie, de gekoppelde service zoekt de pakket-element in uw `packages.config` bestand.

```xml
<packages>
    <span style="background-color: yellow"><package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /></span>
</packages>
```

Voor het detecteren van een oude vorm van verificatie van de organisatie-Account, de gekoppelde service naar het volgende element in zoekt`web.config`:

```xml
<configuration>
    <appSettings>
        <span style="background-color: yellow"><add key="ida:Realm" value="***" /></span>
    </appSettings>
</configuration>
```

Als u wilt het verificatietype wijzigen, verwijder de niet-compatibele verificatietype en probeer de gekoppelde service opnieuw toe te voegen.

Zie voor meer informatie, [Verificatiescenario's voor Azure AD](authentication-scenarios.md).