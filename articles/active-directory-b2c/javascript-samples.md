---
title: JavaScript-voorbeelden voor gebruik in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het gebruik van JavaScript in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4a8d036ff2f36b75df17029c8f00edce25c49e65
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994314"
---
# <a name="javascript-samples-for-use-in-azure-active-directory-b2c"></a>JavaScript-voorbeelden voor gebruik in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

U kunt uw eigen code van de client-side JavaScript toevoegen aan uw Azure Active Directory (Azure AD) B2C-toepassingen. Dit artikel wordt beschreven hoe kunt u uw [aangepast beleid](active-directory-b2c-overview-custom.md) om in te schakelen van de uitvoering van het script.

## <a name="prerequisites"></a>Vereisten

Selecteer een [pagina contract](page-contract.md) voor de gebruikersinterface-elementen van uw toepassing. Als u van plan bent JavaScript gebruiken, moet u de versie van een contract voor al uw inhoudsdefinities in het aangepaste beleid definiëren.

## <a name="add-the-scriptexecution-element"></a>Het element ScriptExecution toevoegen

U de uitvoering van script inschakelen door toe te voegen de **ScriptExecution** element op de [RelyingParty](relyingparty.md) element.

1. Open het bestand aangepast beleid. Bijvoorbeeld, *SignUpOrSignin.xml*.
2. Voeg de **ScriptExecution** element op de **UserJourneyBehaviors** element van **RelyingParty**:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="B2CSignUpOrSignInWithPassword" />
      <UserJourneyBehaviors>
        <ScriptExecution>Allow</ScriptExecution>
      </UserJourneyBehaviors>
      ...
    </RelyingParty>
    ```
3. Opslaan en upload het bestand.

## <a name="guidelines-for-using-javascript"></a>Richtlijnen voor het gebruik van JavaScript

Volg deze richtlijnen wanneer u de interface van uw toepassing met behulp van JavaScript aanpast:

- Een gebeurtenis niet binden op `<a>` HTML-elementen. 
- Geen duren voordat een afhankelijkheid van Azure AD B2C-code of opmerkingen hebt.
- De volgorde of de hiërarchie van Azure AD B2C-HTML-elementen worden niet gewijzigd. Azure AD B2C-beleid gebruiken voor het beheren van de volgorde van de UI-elementen.
- U kunt een RESTful-service met deze overwegingen aanroepen:
    - Mogelijk moet u uw RESTful-service CORS waarmee HTTP-aanroepen aan clientzijde instellen.
    - Zorg ervoor dat uw RESTful-service is beveiligd en alleen het HTTPS-protocol wordt gebruikt.
    - Gebruik geen JavaScript rechtstreeks naar Azure AD B2C-eindpunten worden aangeroepen.
- U kunt uw JavaScript insluiten of u kunt koppelen aan externe JavaScript-bestanden. Wanneer u een bestand met externe JavaScript, zorg ervoor dat u de absolute URL zijn en niet een relatieve URL.
- JavaScript-frameworks:
    - Azure AD B2C maakt gebruik van een specifieke versie van jQuery. Een andere versie van jQuery zijn niet opgenomen. Met behulp van meer dan één versie op dezelfde pagina zorgt ervoor dat de problemen.
    - Met behulp van RequireJS wordt niet ondersteund.
    - De meeste JavaScript-frameworks worden niet ondersteund door Azure AD B2C.
- Azure AD B2C-instellingen kunnen worden gelezen door het aanroepen van `window.SETTINGS`, `window.CONTENT` objecten, zoals de huidige taal van de gebruikersinterface. De waarde van deze objecten niet te wijzigen.
- Voor het aanpassen van het foutbericht van de Azure AD B2C, lokalisatie in een beleid te gebruiken.
- Als alles kan worden bereikt met behulp van een beleid, in het algemeen is de aanbevolen manier.

## <a name="javascript-samples"></a>JavaScript-voorbeelden

### <a name="show-or-hide-a-password"></a>Weergeven of verbergen van een wachtwoord

Er is een veelgebruikte manier om te helpen uw klanten bij hun aanmelding geslaagd zodat ze om te zien wat zij hun wachtwoord hebt ingevoerd. Deze optie kan gebruikers zich aanmelden door zodat ze eenvoudig bekijken en correcties aan hun wachtwoord indien nodig. Elk veld van het type wachtwoord heeft een selectievakje met een **Show wachtwoord** label.  Dit kan de gebruiker om te zien van het wachtwoord in tekst zonder opmaak. Dit codefragment in uw sjabloon registreren of aanmelden voor een zelf-gecontroleerde pagina opnemen:

```Javascript
function makePwdToggler(pwd){
  // Create show-password checkbox
  var checkbox = document.createElement('input');
  checkbox.setAttribute('type', 'checkbox');
  var id = pwd.id + 'toggler';
  checkbox.setAttribute('id', id);

  var label = document.createElement('label');
  label.setAttribute('for', id);
  label.appendChild(document.createTextNode('show password'));

  var div = document.createElement('div');
  div.appendChild(checkbox);
  div.appendChild(label);

  // Add show-password checkbox under password input
  pwd.insertAdjacentElement('afterend', div);

  // Add toggle password callback
  function toggle(){
    if(pwd.type === 'password'){
      pwd.type = 'text';
    } else {
      pwd.type = 'password';
    }
  }
  checkbox.onclick = toggle;
  // For non-mouse usage
  checkbox.onkeydown = toggle;
}

function setupPwdTogglers(){
  var pwdInputs = document.querySelectorAll('input[type=password]');
  for (var i = 0; i < pwdInputs.length; i++) {
    makePwdToggler(pwdInputs[i]);
  }
}

setupPwdTogglers();
```

### <a name="add-terms-of-use"></a>Gebruiksrechtovereenkomst toevoegen 

De volgende code opnemen in uw pagina waar u wilt opnemen in een **gebruiksvoorwaarden** selectievakje. Dit selectievakje is meestal nodig in uw lokale account aanmelden en sociale account aanmeldingspagina's.  

```Javascript
function addTermsOfUseLink() {
    // find the terms of use label element
    var termsOfUseLabel = document.querySelector('#api label[for="termsOfUse"]');
    if (!termsOfUseLabel) {
        return;
    }
      
    // get the label text
    var termsLabelText = termsOfUseLabel.innerHTML;
      
    // create a new <a> element with the same inner text 
    var termsOfUseUrl = 'https://docs.microsoft.com/legal/termsofuse';
    var termsOfUseLink = document.createElement('a');
    termsOfUseLink.setAttribute('href', termsOfUseUrl);
    termsOfUseLink.setAttribute('target', '_blank');
    termsOfUseLink.appendChild(document.createTextNode(termsLabelText));

    // replace the label text with the new element 
    termsOfUseLabel.replaceChild(termsOfUseLink, termsOfUseLabel.firstChild);
}
```

In de code, Vervang `terms-of-use-url` met de koppeling naar de voorwaarden van de gebruiksrechtovereenkomst. Maak een nieuw gebruikerskenmerk met de naam voor uw directory **termsOfUse** en neemt u **termsOfUse** als een gebruikerskenmerk voor de gebruikersstroom.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u de gebruikersinterface van uw toepassingen kunt aanpassen [aanpassen van de gebruikersinterface van uw toepassing met behulp van een aangepast beleid in Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).

