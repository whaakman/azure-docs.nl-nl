---
title: Het opsporen van fouten met de Wizard Azure Active Directory-verbinding
description: De wizard active directory-verbinding een incompatibel verificatietype gedetecteerd
services: active-directory
documentationcenter: 
author: kraigb
manager: mtillman
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/05/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 186bb1ede11c869b1352906b7ebafe57025f4f09
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connection-wizard"></a>Oplossen van problemen met de Wizard Azure Active Directory-verbinding
De wizard gedetecteerd tijdens het detecteren van eerdere verificatiecode op te geven, een incompatibel verificatietype.   

## <a name="what-is-being-checked"></a>Wat wordt gecontroleerd?
**Opmerking:** om te detecteren correct vorige verificatiecode op te geven in een project, het project moet worden gebouwd.  Als u deze fout is opgetreden en er geen een vorige verificatiecode op te geven in uw project, opnieuw en probeer het opnieuw.

### <a name="project-types"></a>Projecttypen
De wizard controleert of het type project dat u ontwikkelt zodat deze de juiste verificatielogica in het project invoeren kunt.  Als er domeincontroller die is afgeleid van `ApiController` in het project, het project wordt beschouwd als een WebAPI-project.  Als er alleen domeincontrollers die zijn afgeleid van `MVC.Controller` in het project, het project wordt beschouwd als een MVC-project.  Overige wordt niet ondersteund door de wizard.

### <a name="compatible-authentication-code"></a>Compatibel verificatiecode op te geven
De wizard controleert ook of de verificatie-instellingen die eerder zijn geconfigureerd met de wizard of compatibel zijn met de wizard.  De instellingen weergegeven als alle instellingen aanwezig zijn, dit wordt beschouwd als een herhaalde aanvraag en de wizard wordt geopend.  Als er slechts enkele van de instellingen aanwezig zijn, wordt het beschouwd als een foutaanvraag.

De wizard controleert of in een MVC-project voor een van de volgende instellingen die het gevolg van eerdere gebruik van de wizard:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Daarnaast de wizard controleert of een van de volgende instellingen in een Web API-project die het gevolg van eerdere gebruik van de wizard:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a>Niet-compatibele verificatiecode op te geven
Ten slotte probeert de wizard voor het detecteren van versies van de verificatiecode op te geven die zijn geconfigureerd met eerdere versies van Visual Studio. Als u deze fout ontvangen, betekent dit dat het project bevat een incompatibel verificatietype. De wizard detecteert de volgende soorten verificatie van eerdere versies van Visual Studio:

* Windows-verificatie 
* Afzonderlijke gebruikersaccounts 
* Organisatieaccounts 

Om te detecteren Windows-verificatie in een MVC-project, de wizard zoekt de `authentication` element uit uw **web.config** bestand.

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Om te detecteren Windows-verificatie in een Web API-project, de wizard zoekt de `IISExpressWindowsAuthentication` element van uw project **.csproj** bestand:

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

Om te detecteren afzonderlijke gebruikersaccounts verificatie, de wizard zoekt naar het element van het pakket uit uw **Packages.config** bestand.

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

Om te detecteren een oude vorm van verificatie van de organisatie-Account, de wizard zoekt naar het volgende element van **web.config**:

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Als u wilt het verificatietype wijzigt, verwijder het verificatietype dat niet compatibel en voer de wizard opnieuw.

Zie voor meer informatie [verificatie scenario's voor Azure AD](active-directory-authentication-scenarios.md).

#<a name="next-steps"></a>Volgende stappen
- [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md) (Verificatiescenario's voor Azure AD)