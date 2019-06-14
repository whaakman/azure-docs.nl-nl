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
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6f151251d76965cf1bc86216eac15a08f1adbc6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60296789"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Diagnose van fouten met de Azure Active Directory Connected Service

Tijdens het detecteren van vorige verificatiecode op te geven, de Azure Active Directory connect-server heeft een niet-compatibele verificatietype dat wordt gedetecteerd.

Als u wilt detecteren correct vorige verificatiecode op te geven in een project, moet het project worden gebouwd.  Als u deze fout ziet en u geen een vorige verificatiecode op te geven in uw project, opnieuw en probeer het opnieuw.

## <a name="project-types"></a>Projecttypen

De gekoppelde service controleert het type project dat u ontwikkelt, zodat deze de juiste verificatielogica in het project invoeren kunt. Als er een domeincontroller die is afgeleid van `ApiController` in het project, het project wordt beschouwd als een WebAPI-project. Als er alleen domeincontrollers die zijn afgeleid van `MVC.Controller` in het project, het project wordt beschouwd als een MVC-project. De gekoppelde service biedt geen ondersteuning voor een ander projecttype.

## <a name="compatible-authentication-code"></a>Compatibel verificatiecode op te geven

Verificatie-instellingen die eerder zijn geconfigureerd of compatibel zijn met de service controleert ook de gekoppelde service. Als alle instellingen aanwezig zijn, heeft deze een herhaalde aanvraag beschouwd en de instellingen worden weergegeven door de gekoppelde service wordt geopend.  Als er slechts enkele van de instellingen aanwezig zijn, is het geval is een fout beschouwd.

In een MVC-project controleert de gekoppelde service voor het gebruik van de volgende instellingen, die het resultaat zijn van vorige gebruik van de service:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

De gekoppelde service controleert ook voor het gebruik van de volgende instellingen in een Web-API-project die het resultaat zijn van vorige gebruik van de service:

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
        <authentication mode="Windows" />
    </system.web>
</configuration>
```

Voor het detecteren van Windows-verificatie in een Web-API-project, de gekoppelde service zoekt de `IISExpressWindowsAuthentication` -element in van uw project `.csproj` bestand:

```xml
<Project>
    <PropertyGroup>
        <IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication>
    </PropertyGroup>
</Project>
```

Voor het detecteren van afzonderlijke gebruikersaccounts verificatie, de gekoppelde service zoekt de pakket-element in uw `packages.config` bestand.

```xml
<packages>
    <package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" />
</packages>
```

Voor het detecteren van een oude vorm van verificatie van de organisatie-Account, de gekoppelde service naar het volgende element in zoekt`web.config`:

```xml
<configuration>
    <appSettings>
        <add key="ida:Realm" value="***" />
    </appSettings>
</configuration>
```

Als u wilt het verificatietype wijzigen, verwijder de niet-compatibele verificatietype en probeer de gekoppelde service opnieuw toe te voegen.

Zie voor meer informatie, [Verificatiescenario's voor Azure AD](authentication-scenarios.md).
