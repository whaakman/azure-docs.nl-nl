---
title: Terraform-modules testen in Azure met behulp van Terratest
description: Informatie over het gebruik van Terratest voor het testen van uw Terraform-modules.
services: terraform
ms.service: terraform
keywords: terraform, devops, opslagaccount, azure, terratest, eenheidstest, integratie testen
author: JunyiYi
manager: jeconnoc
ms.author: junyi
ms.topic: tutorial
ms.date: 10/19/2018
ms.openlocfilehash: 7feee063c7b311934f7d157a9dff62d803a041b0
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638704"
---
# <a name="test-terraform-modules-in-azure-using-terratest"></a>Terraform-modules testen in Azure met behulp van Terratest

Terraform-modules worden gebruikt om herbruikbare, samenstelbare en testbare onderdelen te maken. Ze maken gebruik van inkapseling binnen de context van 'infrastructure as code'.

Net als bij andere softwareonderdelen speelt kwaliteitscontrole een belangrijke rol in Terraform-modules. Er is helaas weinig documentatie beschikbaar waarin wordt uitgelegd hoe eenheidstests en integratietests in Terraform-modules kunnen worden uitgevoerd. In deze zelfstudie maakt u kennis met een infrastructuur voor testen en best practices die we hebben gebruikt tijdens het opbouwen onze [Azure Terraform-modules](https://registry.terraform.io/browse?provider=azurerm).

Na alle populairste testinfrastructuren te hebben bekeken, kozen we voor [Terratest](https://github.com/gruntwork-io/terratest). Terratest wordt geïmplementeerd als een Go-bibliotheek. Deze infrastructuur biedt een verzameling ondersteunende functies en patronen voor algemene testtaken, zoals het maken van HTTP-aanvragen en SSH naar een bepaalde virtuele machine. Dit zijn enkele van de grote voordelen van Terratest:

- **Terratest biedt handige hulpprogramma's om de infrastructuur te controleren.** Deze functie is handig om uw bestaande infrastructuur in een echte omgeving te controleren.
- **De mapstructuur is duidelijk ingedeeld.** Uw testcases worden duidelijk geordend en volgen de [standaard mapstructuur voor Terraform-modules](https://www.terraform.io/docs/modules/create.html#standard-module-structure).
- **Alle testcases worden geschreven in Go.** Aangezien de meeste van de Terraform-ontwikkelaars ook al Go-ontwikkelaars zijn, is het niet nodig om weer een andere programmeertaal te leren. Bovendien zijn Go en Terraform de enige afhankelijkheden die nodig zijn om testcases in Terratest uit te voeren.
- **Deze infrastructuur kan in hoge mate worden uitgebreid.** Het is niet moeilijk om boven op Terratest extra functies te gebruiken, zoals Azure-specifieke functies.

## <a name="prerequisites"></a>Vereisten

Deze praktische handleiding is platformonafhankelijk en kan worden uitgevoerd op Windows, Linux of MacOS. Voordat u verdergaat, moet u de volgende software installeren:

- **De Go-programmeertaal**: de Terraform-testscenario's zijn geschreven in [Go](https://golang.org/dl/).
- **dep**: [dep](https://github.com/golang/dep#installation) is een tool voor afhankelijkheidsbeheer voor Go.
- **Azure CLI**: de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) is een opdrachtregelprogramma voor het beheren van Azure-resources. (Terraform biedt ondersteuning voor verificatie bij Azure via een Service Principal of [via de Azure CLI](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html).)
- **mage**: we gebruiken de [mage executable](https://github.com/magefile/mage/releases) om te leren hoe de Terratest-scenario's kunnen worden vereenvoudigd. 

## <a name="create-a-static-webpage-module"></a>Een statische webpaginamodule maken

In deze zelfstudie maakt u een Terraform-module waarmee een statische webpagina wordt ingericht door een enkel HTML-bestand te uploaden naar de Azure Blob-opslag. Met deze module kunnen gebruikers van over de hele wereld toegang krijgen tot deze webpagina via een URL die door de module wordt geretourneerd.

> [!NOTE]
> Alle bestanden die in deze sectie worden beschreven, moeten worden gemaakt onder uw [GOPATH](https://github.com/golang/go/wiki/SettingGOPATH).

Maak eerst een nieuwe map met de naam `staticwebpage` in de map `src` van uw GoPath. De algehele mappenstructuur van deze zelfstudie wordt hieronder weergegeven. (Het gaat in deze sectie met name over de bestanden die zijn gemarkeerd met een sterretje `(*)`.)

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf      (*)
   ├ 📄 outputs.tf   (*)
   └ 📄 variables.tf (*)
```

De module voor de statische webpagina accepteert drie items als invoer, die worden gedeclareerd in `./variables.tf`:

```hcl
variable "location" {
  description = "The Azure region in which all resources will be created."
}

variable "website_name" {
  description = "The website name which will be used to create a bunch of related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static homepage HTML in your local filesystem."
  default     = "index.html"
}
```

Zoals eerder vermeld, voert deze module ook een URL uit die is gedeclareerd in `./outputs.tf`:

```hcl
output "homepage_url" {
  value = "${azurerm_storage_blob.homepage.url}"
}
```

Daarmee zijn we aanbeland bij de voornaamste logica van deze module. In totaal worden deze vier resources ingericht:
- Een resourcegroep waarvan de naam de `website_name`-invoer is die wordt toegevoegd door `-staging-rg`.
- Een opslagaccount waarvan de naam de `website_name`-invoer is die wordt toegevoegd door `data001`. Maar om te voldoen aan de naamgevingsbeperkingen voor opslagaccounts, verwijdert de module alle speciale tekens en wordt de volledige naam omgezet in kleine letters.
- Een container met de vaste naam `wwwroot` gemaakt in het bovenstaande opslagaccount.
- Er wordt één HTML-bestand gelezen uit de `html_path`-invoer en geüpload naar `wwwroot/index.html`.

De modulelogica van de statische webpagina wordt geïmplementeerd in `./main.tf`:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "${var.website_name}-staging-rg"
  location = "${var.location}"
}

resource "azurerm_storage_account" "main" {
  name                     = "${lower(replace(var.website_name, "/[[:^alnum:]]/", ""))}data001"
  resource_group_name      = "${azurerm_resource_group.main.name}"
  location                 = "${azurerm_resource_group.main.location}"
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "main" {
  name                  = "wwwroot"
  resource_group_name   = "${azurerm_resource_group.main.name}"
  storage_account_name  = "${azurerm_storage_account.main.name}"
  container_access_type = "blob"
}

resource "azurerm_storage_blob" "homepage" {
  name                   = "index.html"
  resource_group_name    = "${azurerm_resource_group.main.name}"
  storage_account_name   = "${azurerm_storage_account.main.name}"
  storage_container_name = "${azurerm_storage_container.main.name}"
  source                 = "${var.html_path}"
  type                   = "block"
  content_type           = "text/html"
}
```

### <a name="unit-test"></a>Eenheidstest

Terratest is van oorsprong een hulpprogramma dat is ontworpen voor integratietests, wat betekent dat hiermee werkelijke resources in een echte omgeving worden ingericht. Soms kunnen dergelijke taken uitzonderlijk groot worden, vooral wanneer u grote hoeveelheden resources hebt die moeten worden ingericht. De logica voor naamgevingsconversie van opslagaccounts in de vorige sectie is daarvan een goed voorbeeld: het is niet echt nodig resources in te richten, we willen alleen maar controleren of de logica voor naamgevingsconversie klopt.

Dankzij de flexibiliteit van Terratest is dat eenvoudig te doen door gebruik te maken van eenheidstests. Eenheidstests zijn lokaal uitgevoerde testcases (hoewel toegang tot internet wel is vereist) die worden geactiveerd door het uitvoeren van de opdrachten `terraform init` en `terraform plan`, waarbij de testcases de uitvoer van `terraform plan` parseren en zoeken naar de te vergelijken kenmerkwaarden.

In de rest van deze sectie wordt beschreven hoe we Terratest gebruiken voor het implementeren van een eenheidstest om ervoor te zorgen dat de logica voor naamgevingsconversie van opslagaccounts juist is. We zijn alleen geïnteresseerd in de bestanden die zijn gemarkeerd met een sterretje `(*)`.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html                (*)
   │   │       └ 📄 main.tf                   (*)
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go (*)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Om te beginnen is een leeg HTML-bestand `./test/fixtures/storage-account-name/empty.html` niet meer dan een tijdelijke aanduiding.

Het bestand `./test/fixtures/storage-account-name/main.tf` vormt de basis van het testscenario. Het bestand accepteert één `website_name` als invoer en deze is tevens de invoer van de eenheidstests. De logica ervan wordt hier weergegeven:

```hcl
variable "website_name" {
  description = "The name of your static website."
}

module "staticwebpage" {
  source       = "../../../"
  location     = "West US"
  website_name = "${var.website_name}"
  html_path    = "empty.html"
}
```

Tot slot is het belangrijkste onderdeel de implementatie van de eenheidstests: `./test/storage_account_name_unit_test.go`

Als u een Go-ontwikkelaar bent, zult u herkennen dat dit overeenkomt met de handtekening van een klassieke Go-testfunctie waarbij een argument van het type `*testing.T` wordt geaccepteerd.

In de hoofdtekst van de eenheidstest zijn er in totaal vijf cases gedefinieerd in variabele `testCases` (sleutel als invoer, met de waarde als verwachte uitvoer). Voor elke eenheidstest voeren we eerst `terraform init` uit op de map test/fixtures (`./test/fixtures/storage-account-name/`). 

Vervolgens wordt met een `terraform plan`-opdracht met specifieke invoer voor een testscenario (kijk eens naar de definitie van `website_name` in `tfOptions`) het resultaat opgeslagen in `./test/fixtures/storage-account-name/terraform.tfplan` (wat niet wordt vermeld in de algehele mappenstructuur).

Dit resultatenbestand wordt vervolgens geparseerd naar een voor code leesbare structuur met behulp van de officiële Terraform-planparser.

We gaan nu naar de kenmerken kijken waarin we geïnteresseerd zijn (in dit geval de `name` van het `azurerm_storage_account`) en deze vergelijken met de verwachte uitvoer.

```go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    terraformCore "github.com/hashicorp/terraform/terraform"
)

func TestUT_StorageAccountName(t *testing.T) {
    t.Parallel()

    // Test cases for storage account name conversion logic
    testCases := map[string]string{
        "TestWebsiteName": "testwebsitenamedata001",
        "ALLCAPS":         "allcapsdata001",
        "S_p-e(c)i.a_l":   "specialdata001",
        "A1phaNum321":     "a1phanum321data001",
        "E5e-y7h_ng":      "e5ey7hngdata001",
    }

    for input, expected := range testCases {
        // Specify test case folder and "-var" options
        tfOptions := &terraform.Options{
            TerraformDir: "./fixtures/storage-account-name",
            Vars: map[string]interface{}{
                "website_name": input,
            },
        }

        // Terraform init and plan only
        tfPlanOutput := "terraform.tfplan"
        terraform.Init(t, tfOptions)
        terraform.RunTerraformCommand(t, tfOptions, terraform.FormatArgs(tfOptions.Vars, "plan", "-out="+tfPlanOutput)...)

        // Read and parse the plan output
        f, err := os.Open(path.Join(tfOptions.TerraformDir, tfPlanOutput))
        if err != nil {
            t.Fatal(err)
        }
        defer f.Close()
        plan, err := terraformCore.ReadPlan(f)
        if err != nil {
            t.Fatal(err)
        }

        // Validate the test result
        for _, mod := range plan.Diff.Modules {
            if len(mod.Path) == 2 && mod.Path[0] == "root" && mod.Path[1] == "staticwebpage" {
                actual := mod.Resources["azurerm_storage_account.main"].Attributes["name"].New
                if actual != expected {
                    t.Fatalf("Expect %v, but found %v", expected, actual)
                }
            }
        }
    }
}
```

Voor het uitvoeren van de eenheidstests moet u de volgende stappen in de opdrachtregel uitvoeren.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestUT_StorageAccountName
```

De traditionele Go-testresultaten worden na ongeveer een minuut weergegeven.

### <a name="integration-test"></a>Integratietest

In tegenstelling tot eenheidstests zijn integratietests vereist voor de inrichting van resources in een echte omgeving gezien vanuit een end-to-end perspectief. Terratest is daarvoor prima geschikt. Als voor een optimale toepassing van de Terraform-module ook wordt aangeraden om in de map `examples` enkele end-to-end voorbeelden op te nemen, waarom zou u die voorbeelden dan niet gewoon als integratietests testen? In deze sectie kijken we naar drie bestanden, die elk gemarkeerd zijn met een sterretje `(*)`.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html              (*)
   │       └ 📄 main.tf                 (*)
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go (*)
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Laten we eerst beginnen met de voorbeelden. Er wordt nieuwe map met de naam `hello-world/` gemaakt in de map `./examples/`. We geven hier een eenvoudige HTML-pagina die moet worden geüpload `./examples/hello-world/index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demostrate Terratest.</p>
</body>
</html>
```

Het Terraform-voorbeeld `./examples/hello-world/main.tf` is vergelijkbaar met het voorbeeld uit de eenheidstest, met maar één grote verschil: nu wordt ook de URL van de geüploade HTML-code met de naam `homepage` afgedrukt.

```hcl
variable "website_name" {
  description = "The name of your static website."
  default     = "Hello-World"
}

module "staticwebpage" {
  source       = "../../"
  location     = "West US"
  website_name = "${var.website_name}"
}

output "homepage" {
  value = "${module.staticwebpage.homepage_url}"
}
```

Terratest en de klassieke Go-testfunctie worden opnieuw weergegeven in het bestand van de integratietest `./test/hello_world_example_test.go`.

In tegenstelling tot eenheidstests, worden met integratietests echte resources in Azure gemaakt, reden waarom u zorgvuldig moet handelen om te voorkomen dat er naamconflicten ontstaan. (Besteed speciale aandacht aan bepaalde globaal unieke namen, zoals de naam van het opslagaccount). De eerste stap van de testlogica is het genereren van een willekeurige `websiteName` met behulp van de `UniqueId()`-functie die wordt geleverd door TerraTest. Deze functie genereert een willekeurige naam die kleine letters, hoofdletters en cijfers kan bevatten. Met `tfOptions` worden alle Terraform-opdrachten gericht op de map `./examples/hello-world/` en wordt `website_name` ingesteld op de willekeurige `websiteName`.

Vervolgens worden `terraform init`, `terraform apply` en `terraform output` een voor een uitgevoerd. We hebben een andere door Terratest geleverde helpfunctie, `HttpGetWithCustomValidation()`, gebruikt om ervoor te zorgen dat HTML wordt geüpload naar de uitvoer `homepage`-URL die wordt geretourneerd door `terraform output` door de HTTP Get- statuscode te vergelijken met `200` en te zoeken naar bepaalde trefwoorden in de HTML-inhoud. Ten slotte wordt "toegezegd" dat `terraform destroy` wordt uitgevoerd door gebruik te maken van de `defer`-functie van Go.

```go
package test

import (
    "fmt"
    "strings"
    "testing"

    "github.com/gruntwork-io/terratest/modules/http-helper"
    "github.com/gruntwork-io/terratest/modules/random"
    "github.com/gruntwork-io/terratest/modules/terraform"
)

func TestIT_HelloWorldExample(t *testing.T) {
    t.Parallel()

    // Generate a random website name to prevent naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demostrate Terratest.")
    })
}
```

Voor het uitvoeren van de integratietests moet u de volgende stappen in de opdrachtregel uitvoeren.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestIT_HelloWorldExample
```

De traditionele Go-testresultaten worden na ongeveer twee minuten weergegeven. Natuurlijk kunt u ook zowel eenheidstests als integratietests uitvoeren door de volgende opdracht te gebruiken:

```shell
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ go test
```

U kunt zich voorstellen dat integratietests veel meer tijd in beslag nemen dan eenheidstests (twee minuten voor één integratiescenario tegenover één minuut voor vijf eenheidsscenario's). Maar het blijft nog altijd uw beslissing wanneer u eenheidstests en wanneer integratietests wilt gebruiken. Normaal gesproken geven we de voorkeur aan eenheidstests voor complexe logica met gebruik van Terraform HCL-functies. Integratietests daarentegen worden vanuit het end-to-end perspectief van een gebruiker uitgevoerd.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Gebruik mage om het uitvoeren van Terratest-cases te vereenvoudigen 

Zoals u hebt gezien, is het uitvoeren van testcases in de shell geen eenvoudige taak omdat u moet navigeren naar verschillende mappen en diverse opdrachten moet uitvoeren. Daarom introduceren we het buildsysteem in dit project. In deze sectie gebruiken we een mage van het Go-buildsysteem om die taak te verrichten.

Het enige wat mage nodig heeft, is een `magefile.go` in de hoofdmap van uw project (gemarkeerd door `(+)` in de volgende afbeelding).

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 magefile.go (+)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Hier volgt een voorbeeld van `./magefile.go`. In dit buildscript, geschreven in Go, hebben we vijf buildstappen geïmplementeerd:
- `Clean`: met deze stap worden alle gegenereerde/tijdelijke bestanden tijdens de testuitvoeringen verwijderd.
- `Format`: met deze stap worden `terraform fmt` en `go fmt` uitgevoerd om uw codebasis op te maken.
- `Unit`: met deze stap worden alle eenheidstests uitgevoerd (met behulp van de functienaamconventie `TestUT_*`) onder de map `./test/`.
- `Integration`: vergelijkbaar met `Unit`, maar in plaats van eenheidstests worden integratietests (`TestIT_*`) uitgevoerd.
- `Full`: met deze stap worden achtereenvolgens `Clean`, `Format`, `Unit', and `Integration' uitgevoerd.

```go
// +build mage

// Build script to format and run tests of a Terraform module project.
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// Default target when execute `mage` in shell
var Default = Full

// A build step that runs Clean, Format, Unit and Integration in sequence
func Full() {
    mg.Deps(Unit)
    mg.Deps(Integration)
}

// A build step that runs unit tests
func Unit() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running unit tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestUT_", "-v")
}

// A build step that runs integration tests
func Integration() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running integration tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestIT_", "-v")
}

// A build step that formats both Terraform code and Go code
func Format() error {
    fmt.Println("Formatting...")
    if err := sh.RunV("terraform", "fmt", "."); err != nil {
        return err
    }
    return sh.RunV("go", "fmt", "./test/")
}

// A build step that removes temporary build/test files
func Clean() error {
    fmt.Println("Cleaning...")
    return filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
        if err != nil {
            return err
        }
        if info.IsDir() && info.Name() == "vendor" {
            return filepath.SkipDir
        }
        if info.IsDir() && info.Name() == ".terraform" {
            os.RemoveAll(path)
            fmt.Printf("Removed \"%v\"\n", path)
            return filepath.SkipDir
        }
        if !info.IsDir() && (info.Name() == "terraform.tfstate" ||
            info.Name() == "terraform.tfplan" ||
            info.Name() == "terraform.tfstate.backup") {
            os.Remove(path)
            fmt.Printf("Removed \"%v\"\n", path)
        }
        return nil
    })
}
```

Net als bij de voorgaande stappen kunt u de volgende opdrachten gebruiken om een volledig testpakket uit te voeren:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in magefile or test cases
GoPath/src/staticwebpage$ go fmt      # Only requied when you change the magefile
GoPath/src/staticwebpage$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage$ mage
```

U kunt de laatste opdrachtregel gerust vervangen door mage-stappen, zoals `mage unit` of `mage clean`. Nu denkt u misschien dat er hier nog steeds veel opdrachtregels zijn en dat het een goed idee is om zowel `dep`-opdrachten als `az login` in het mage-bestand in te sluiten. Maar we gaan de code daarvan hier niet weergeven. Een ander voordeel van mage is dat de stappen kunnen worden gedeeld met behulp van het Go-systeem. Dat betekent dat mage-bestanden in al uw modules kunnen worden vereenvoudigd door alleen maar te verwijzen naar een gezamenlijke implementatie en door afhankelijkheden te declareren (`mg.Deps()`).

> [!NOTE]
> **Optie: omgevingsvariabelen van de service-principal instellen op het uitvoeren van acceptatietests**
> 
> In plaats van `az login` uit te voeren voordat u tests uitvoert, kunt u zorgen voor Azure-verificatie door omgevingsvariabelen van de service-principal in te stellen. Terraform publiceert [een lijst met namen van omgevingsvariabelen](https://www.terraform.io/docs/providers/azurerm/index.html#testing). (Alleen de eerste vier van deze omgevingsvariabelen zijn vereist.) Terraform publiceert ook uitvoerige instructies waarin wordt uitgelegd hoe u [de waarde van deze omgevingsvariabelen kunt ophalen.](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Terratest kunt u vinden op de bijbehorende [GitHub-pagina](https://github.com/gruntwork-io/terratest). U kunt nuttige informatie over mage vinden in de [GitHub-pagina](https://github.com/magefile/mage) en [startpagina](https://magefile.org/) van mage.
