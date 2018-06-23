---
title: Gemiddeld met aangepaste term lijsten in Azure inhoud beheerder | Microsoft Docs
description: Het gemiddelde met aangepaste term geeft een lijst met Azure inhoud beheerder SDK voor .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/11/2018
ms.author: sajagtap
ms.openlocfilehash: 6da72ad070d9c3a6be38e24626dff77b52fed852
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344562"
---
# <a name="moderate-with-custom-term-lists-in-net"></a>Gemiddelde met aangepaste term lijsten in .NET

De standaard globale lijst met termen in Azure inhoud beheerder is voldoende voor de meest inhoud toezicht behoeften. Mogelijk moet u echter scherm van de termen die specifiek voor uw organisatie zijn. Bijvoorbeeld, kan u concurrent labelnamen voor verder onderzoek. 

Maken van aangepaste lijsten met voorwaarden voor gebruik met de tekst toezicht API kunt u de inhoud beheerder SDK voor .NET.

> [!NOTE]
> Er is een maximumlimiet van **geeft een lijst van 5 term** met elke lijst **niet meer dan 10.000 voorwaarden**.
>

Dit artikel bevat informatie en codevoorbeelden om u te helpen aan de slag met de inhoud beheerder SDK voor .NET aan:
- Een lijst maken.
- Voorwaarden toevoegen aan een lijst.
- Scherm voorwaarden op basis van de voorwaarden in een lijst.
- Voorwaarden verwijdert uit een lijst.
- Een lijst te verwijderen.
- Lijst met gegevens bewerken.
- De index vernieuwen, zodat de wijzigingen in de lijst zijn opgenomen in een nieuwe scan.

In dit artikel wordt ervan uitgegaan dat u al bekend met Visual Studio en C# bent.

## <a name="sign-up-for-content-moderator-services"></a>Aanmelden voor inhoud beheerder services

Voordat u inhoud beheerder services via de REST-API of de SDK gebruiken kunt, moet u een abonnementssleutel.

In het Dashboard inhoud beheerder vindt u de abonnementssleutel van uw in **instellingen** > **referenties** > **API**  >  **Proefversie Ocp-Apim-Subscription-Key**. Zie voor meer informatie [overzicht](overview.md).

## <a name="create-your-visual-studio-project"></a>Visual Studio-project maken

1. Voeg een nieuwe **Console-app (.NET Framework)** project om uw oplossing.

1. Noem het project **TermLists**. Selecteer dit project als opstartproject één voor de oplossing.

1. Voeg een verwijzing naar de **ModeratorHelper** project assembly die u hebt gemaakt in de [inhoud beheerder client helper Quick Start](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>De vereiste pakketten installeren

De volgende NuGet-pakketten voor het project TermLists installeren:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Update van het programma de using-instructies

Wijzig het programma de using-instructies.

    using System;
    using System.Threading;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;

### <a name="add-private-properties"></a>Privé-eigenschappen toevoegen

De volgende persoonlijke eigenschappen toevoegen aan de naamruimte TermLists, klasse Program.

    /// <summary>
    /// The language of the terms in the term lists.
    /// </summary>
    private const string lang = "eng";

    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Content Moderator APIs.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against a the list.
    /// </summary>
    private const double latencyDelay = 0.5;

## <a name="create-a-term-list"></a>Maak een lijst van de termijn

U maakt een lijst van de termijn met **ContentModeratorClient.ListManagementTermLists.Create**. De eerste parameter **maken** is een tekenreeks met een MIME-type ' application/json' moet zijn. Zie voor meer informatie de [API-referentiemateriaal](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). De tweede parameter is een **instantie** object met een naam en beschrijving voor de nieuwe termijn-lijst.

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

> [!NOTE]
> De sleutel van uw inhoud beheerder service heeft een aanvragen per frequentielimiet van tweede (RPS) en als u de limiet overschrijdt, de SDK er een uitzondering gegenereerd met een 429 foutcode. 
>
> Een sleutel gratis laag heeft een limiet van de frequentie waarmee een RPS.

    /// <summary>
    /// Creates a new term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The term list ID.</returns>
    static string CreateTermList (ContentModeratorClient client)
    {
        Console.WriteLine("Creating term list.");

        Body body = new Body("Term list name", "Term list description");
        TermList list = client.ListManagementTermLists.Create("application/json", body);
        if (false == list.Id.HasValue)
        {
            throw new Exception("TermList.Id value missing.");
        }
        else
        {
            string list_id = list.Id.Value.ToString();
            Console.WriteLine("Term list created. ID: {0}.", list_id);
            Thread.Sleep(throttleRate);
            return list_id;
        }
    }

## <a name="update-term-list-name-and-description"></a>Update term lijstnaam en beschrijving

Bijwerken van de informatie van de termijn lijst met **ContentModeratorClient.ListManagementTermLists.Update**. De eerste parameter **Update** is de term lijst-ID. De tweede parameter is een MIME-type ' application/json' moet zijn. Zie voor meer informatie de [API-referentiemateriaal](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). De derde parameter is een **instantie** object, dat de nieuwe naam en beschrijving bevat.

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

    /// <summary>
    /// Update the information for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="name">The new name for the term list.</param>
    /// <param name="description">The new description for the term list.</param>
    static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
    {
        Console.WriteLine("Updating information for term list with ID {0}.", list_id);
        Body body = new Body(name, description);
        client.ListManagementTermLists.Update(list_id, "application/json", body);
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-term-to-a-term-list"></a>Een term kunt toevoegen aan een lijst term

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

    /// <summary>
    /// Add a term to the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="term">The term to add to the term list.</param>
    static void AddTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
        client.ListManagementTerm.AddTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

## <a name="get-all-terms-in-a-term-list"></a>Alle termen in een lijst van de termijn ophalen

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

    /// <summary>
    /// Get all terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to get all terms.</param>
    static void GetAllTerms(ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
        Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
        TermsData data = terms.Data;
        foreach (TermsInList term in data.Terms)
        {
            Console.WriteLine(term.Term);
        }
        Thread.Sleep(throttleRate);
    }

## <a name="add-code-to-refresh-the-search-index"></a>Code toevoegen om te vernieuwen van de search-index

Nadat u wijzigingen in een lijst van de termijn aanbrengt, vernieuw de search-index om de wijzigingen te worden opgenomen de volgende keer dat u de lijst term om tekst op het scherm te gebruiken. Dit is vergelijkbaar met hoe een zoekmachine op het bureaublad (indien ingeschakeld) of een zoekmachine voortdurend vernieuwd de index als nieuwe bestanden of pagina's wilt opnemen.

Vernieuwen van een term lijst search-index met **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

    /// <summary>
    /// Refresh the search index for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to refresh.</param>
    static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
        client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
        Thread.Sleep((int)(latencyDelay * 60 * 1000));
    }

## <a name="screen-text-using-a-term-list"></a>Tekst op het scherm met behulp van een lijst van de termijn

Scherm van tekst met behulp van een lijst van de termijn met **ContentModeratorClient.TextModeration.ScreenText**, die de volgende parameters zijn vereist.

- De taal van de voorwaarden in de lijst van de termijn.
- Een MIME-type, ' text/html', ' text/xml', ' text/markdown' of ' text/plain worden kan'.
- De tekst op het scherm.
- Een Booleaanse waarde. Dit veld ingesteld op **true** AutoCorrectie-de tekst vóór het gescreend.
- Een Booleaanse waarde. Dit veld ingesteld op **true** voor het detecteren van persoonlijk herleidbare informatie (PII) in de tekst.
- De term lijst-ID.

Zie voor meer informatie de [API-referentiemateriaal](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** retourneert een **scherm** -object met een **voorwaarden** eigenschap met een lijst met alle voorwaarden die inhoud beheerder gedetecteerd in de bestandsgroepen. Houd er rekening mee dat als inhoud beheerder eventuele voorwaarden die u tijdens de controle niet gedetecteerd de **voorwaarden** eigenschap heeft een waarde **null**.

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

    /// <summary>
    /// Screen the indicated text for terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to use to screen the text.</param>
    /// <param name="text">The text to screen.</param>
    static void ScreenText (ContentModeratorClient client, string list_id, string text)
    {
        Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
        Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
        if (null == screen.Terms)
        {
            Console.WriteLine("No terms from the term list were detected in the text.");
        }
        else
        {
            foreach (DetectedTerms term in screen.Terms)
            {
                Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
            }
        }
        read.Sleep(throttleRate);
    }

## <a name="delete-terms-and-lists"></a>Voorwaarden en lijsten verwijderen

Het is eenvoudig een term of een lijst te verwijderen. U de SDK gebruiken voor de volgende taken uitvoeren:

- Een term verwijderen. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- De voorwaarden in een lijst verwijderen zonder te verwijderen van de lijst. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Een lijst en alle inhoud verwijderd. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### <a name="delete-a-term"></a>Een term verwijderen

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

    /// <summary>
    /// Delete a term from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete the term.</param>
    /// <param name="term">The term to delete.</param>
    static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
        client.ListManagementTerm.DeleteTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-all-terms-in-a-term-list"></a>Alle termen in een lijst term verwijderen

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

    /// <summary>
    /// Delete all terms from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete all terms.</param>
    static void DeleteAllTerms (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
        client.ListManagementTerm.DeleteAllTerms(list_id, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-a-term-list"></a>Een term lijst verwijderen

De volgende methodedefinitie toevoegen aan naamruimte TermLists, klasse Program.

    /// <summary>
    /// Delete the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to delete.</param>
    static void DeleteTermList (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Deleting term list with ID {0}.", list_id);
        client.ListManagementTermLists.Delete(list_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Kort samengevat

Voeg de **Main** methodedefinitie naamruimte TermLists, klasse Program. Tot slot sluit de klasse Program en de naamruimte TermLists.

    static void Main(string[] args)
    {
        using (var client = Clients.NewClient())
        {
            string list_id = CreateTermList(client);

            UpdateTermList(client, list_id, "name", "description");
            AddTerm(client, list_id, "term1");
            AddTerm(client, list_id, "term2");

            GetAllTerms(client, list_id);

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            string text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteTerm(client, list_id, "term1");

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteAllTerms(client, list_id);
            DeleteTermList(client, list_id);

            Console.WriteLine("Press ENTER to close the application.");
            Console.ReadLine();
        }
    }

## <a name="run-the-application-to-see-the-output"></a>Voer de toepassing om te zien van de uitvoer

De uitvoer worden uitgevoerd in de volgende regels, maar de gegevens kan verschillen.

    Creating term list.
    Term list created. ID: 252.
    Updating information for term list with ID 252.
    
    Adding term "term1" to term list with ID 252.
    Adding term "term2" to term list with ID 252.
    
    Getting terms in term list with ID 252.
    term1
    term2
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term1" from list ID 252 at index 32.
    Found term: "term2" from list ID 252 at index 46.
    
    Removed term "term1" from term list with ID 252.
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term2" from list ID 252 at index 46.
    
    Removing all terms from term list with ID 252.
    Deleting term list with ID 252.
    Press ENTER to close the application.
    
## <a name="next-steps"></a>Volgende stappen

[Downloaden van de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere inhoud beheerder snelstartgidsen voor .NET, en op uw integratie aan de slag.
