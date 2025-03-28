# 📘 AdventureWorks – Dokumentacja encji (Część 1)

> Opis tabel z bazy danych AdventureWorks.  
> Każda encja zawiera:
> - ✨ Opis funkcjonalny  
> - 🧱 Strukturę tabeli  
> - 🔗 Relacje z innymi encjami

---

## 🏠 `Person.Address`

### ✨ Opis funkcjonalny  
Przechowuje dane adresowe używane przez klientów, pracowników i dostawców.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `AddressID`        | `int` NOT NULL IDENTITY(1,1)          |
| `AddressLine1`     | `nvarchar(60)` NOT NULL               |
| `AddressLine2`     | `nvarchar(60)` NULL                   |
| `City`             | `nvarchar(30)` NOT NULL               |
| `StateProvinceID`  | `int` NOT NULL                        |
| `PostalCode`       | `nvarchar(15)` NOT NULL               |
| `SpatialLocation`  | `geography` NULL                      |
| `rowguid`          | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- `Person.StateProvince`  
- `Person.BusinessEntityAddress`

---

## 🗂️ `Person.AddressType`

### ✨ Opis funkcjonalny  
Definiuje typy adresów – np. domowy, firmowy, rozliczeniowy itp.

### 🧱 Struktura tabeli

| 🧩 Kolumna     | 📝 Typ danych / Definicja             |
|----------------|----------------------------------------|
| `AddressTypeID`| `int` NOT NULL IDENTITY(1,1)          |
| `Name`         | `nvarchar(50)` NOT NULL               |
| `rowguid`      | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate` | `datetime` NOT NULL                   |

### 🔗 Relacje  
- `Person.BusinessEntityAddress`

---

## 🧾 `Person.BusinessEntity`

### ✨ Opis funkcjonalny  
Główna encja reprezentująca osoby fizyczne, firmy, pracowników, klientów itd.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `BusinessEntityID` | `int` NOT NULL IDENTITY(1,1)          |
| `rowguid`          | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- `Person.Person`  
- `HumanResources.Employee`  
- `Sales.Customer`

---

## 🔗 `Person.BusinessEntityAddress`

### ✨ Opis funkcjonalny  
Łączy encje (osoby, firmy) z adresami i określa ich typ.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `BusinessEntityID` | `int` NOT NULL                        |
| `AddressID`        | `int` NOT NULL                        |
| `AddressTypeID`    | `int` NOT NULL                        |
| `rowguid`          | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- `Person.BusinessEntity`  
- `Person.Address`  
- `Person.AddressType`  
- Klucz główny: kombinacja trzech ID

---

## 👤 `Person.ContactType`

### ✨ Opis funkcjonalny  
Typy kontaktów używane np. w relacjach z osobami – np. technik, konsultant, osoba kontaktowa.

### 🧱 Struktura tabeli

| 🧩 Kolumna     | 📝 Typ danych / Definicja             |
|----------------|----------------------------------------|
| `ContactTypeID`| `int` NOT NULL IDENTITY(1,1)          |
| `Name`         | `nvarchar(50)` NOT NULL               |
| `ModifiedDate` | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Używana m.in. w `Person.Person`

---

## 🧑‍💼 `Person.CountryRegion`

### ✨ Opis funkcjonalny  
Zawiera listę krajów i regionów używanych w adresach i podatkach.

### 🧱 Struktura tabeli

| 🧩 Kolumna        | 📝 Typ danych / Definicja             |
|-------------------|----------------------------------------|
| `CountryRegionCode` | `nvarchar(3)` NOT NULL PRIMARY KEY  |
| `Name`             | `nvarchar(50)` NOT NULL              |
| `ModifiedDate`     | `datetime` NOT NULL                  |

### 🔗 Relacje  
- Używana przez `Sales.SalesTerritory`, `Person.StateProvince`, `Person.CountryRegionCurrency`

---

## 💱 `Person.CountryRegionCurrency`

### ✨ Opis funkcjonalny  
Łączy kraje/regiony z ich walutami.

### 🧱 Struktura tabeli

| 🧩 Kolumna            | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `CountryRegionCode`    | `nvarchar(3)` NOT NULL               |
| `CurrencyCode`         | `nchar(3)` NOT NULL                  |
| `ModifiedDate`         | `datetime` NOT NULL                  |

### 🔗 Relacje  
- Relacje z `Person.CountryRegion` oraz `Sales.Currency`  
- Klucz główny: kombinacja `CountryRegionCode`, `CurrencyCode`

---

## 💳 `Person.CreditCard`

### ✨ Opis funkcjonalny  
Przechowuje informacje o kartach kredytowych klientów.

### 🧱 Struktura tabeli

| 🧩 Kolumna      | 📝 Typ danych / Definicja             |
|-----------------|----------------------------------------|
| `CreditCardID`  | `int` NOT NULL IDENTITY(1,1)          |
| `CardType`      | `nvarchar(50)` NOT NULL               |
| `CardNumber`    | `nvarchar(25)` NOT NULL               |
| `ExpMonth`      | `tinyint` NOT NULL                    |
| `ExpYear`       | `smallint` NOT NULL                   |
| `ModifiedDate`  | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Używana w `Sales.ContactCreditCard` i `Sales.SalesOrderHeader`

---

## 🏢 `Person.StateProvince`

### ✨ Opis funkcjonalny  
Zawiera dane o województwach/prowincjach/krajach z podziałem terytorialnym.

### 🧱 Struktura tabeli

| 🧩 Kolumna            | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `StateProvinceID`      | `int` NOT NULL IDENTITY(1,1)          |
| `StateProvinceCode`    | `nchar(3)` NOT NULL                   |
| `CountryRegionCode`    | `nvarchar(3)` NOT NULL                |
| `IsOnlyStateProvinceFlag` | `bit` NOT NULL                  |
| `Name`                 | `nvarchar(50)` NOT NULL               |
| `TerritoryID`          | `int` NOT NULL                        |
| `rowguid`              | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`         | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Relacje z `Person.Address`, `Sales.SalesTaxRate`, `Sales.SalesTerritory`

---

## 📇 `Person.Contact`

_(Uwaga: w nowszych wersjach może być zastąpiona przez `Person.Person`)_

### ✨ Opis funkcjonalny  
Przechowuje dane kontaktowe osób (np. e-mail, nazwisko, stanowisko).

### 🧱 Struktura tabeli

| 🧩 Kolumna        | 📝 Typ danych / Definicja             |
|-------------------|----------------------------------------|
| `ContactID`       | `int` NOT NULL IDENTITY(1,1)          |
| `NameStyle`       | `bit` NOT NULL                        |
| `Title`           | `nvarchar(8)` NULL                    |
| `FirstName`       | `nvarchar(50)` NOT NULL               |
| `MiddleName`      | `nvarchar(50)` NULL                   |
| `LastName`        | `nvarchar(50)` NOT NULL               |
| `Suffix`          | `nvarchar(10)` NULL                   |
| `EmailAddress`    | `nvarchar(50)` NULL                   |
| `EmailPromotion`  | `int` NOT NULL                        |
| `Phone`           | `nvarchar(25)` NULL                   |
| `PasswordHash`    | `varchar(128)` NOT NULL               |
| `PasswordSalt`    | `varchar(10)` NOT NULL                |
| `AdditionalContactInfo` | `xml` NULL                     |
| `rowguid`         | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`    | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Wykorzystywana przez `Sales.Individual`, `Sales.ContactCreditCard`, `Sales.SalesOrderHeader`

---

## 👨‍👩‍👧 `Person.Person`

### ✨ Opis funkcjonalny  
Reprezentuje osoby fizyczne w systemie – zarówno pracowników, klientów, jak i dostawców.

### 🧱 Struktura tabeli

| 🧩 Kolumna           | 📝 Typ danych / Definicja               |
|----------------------|------------------------------------------|
| `BusinessEntityID`   | `int` NOT NULL                          |
| `PersonType`         | `nchar(2)` NOT NULL                     |
| `NameStyle`          | `bit` NOT NULL                          |
| `Title`              | `nvarchar(8)` NULL                      |
| `FirstName`          | `nvarchar(50)` NOT NULL                 |
| `MiddleName`         | `nvarchar(50)` NULL                     |
| `LastName`           | `nvarchar(50)` NOT NULL                 |
| `Suffix`             | `nvarchar(10)` NULL                     |
| `EmailPromotion`     | `int` NOT NULL                          |
| `AdditionalContactInfo` | `xml` NULL                         |
| `Demographics`       | `xml` NULL                              |
| `rowguid`            | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`       | `datetime` NOT NULL                     |

### 🔗 Relacje  
- Centralna tabela dla danych osobowych  
- Połączona z `Person.EmailAddress`, `HumanResources.Employee`, `Sales.Customer`

---

## 📬 `Person.EmailAddress`

### ✨ Opis funkcjonalny  
Przechowuje adresy e-mail osób z tabeli `Person.Person`.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `BusinessEntityID` | `int` NOT NULL                        |
| `EmailAddressID`   | `int` NOT NULL IDENTITY(1,1)          |
| `EmailAddress`     | `nvarchar(50)` NULL                   |
| `rowguid`          | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `Person.Person` (wiele adresów dla jednej osoby)

---

## 📞 `Person.PersonPhone`

### ✨ Opis funkcjonalny  
Zawiera numery telefonów przypisane do osób.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `BusinessEntityID` | `int` NOT NULL                        |
| `PhoneNumber`      | `nvarchar(25)` NOT NULL               |
| `PhoneNumberTypeID`| `int` NOT NULL                        |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Połączenie z `Person.Person` i `Person.PhoneNumberType`  
- Klucz główny: kombinacja `BusinessEntityID`, `PhoneNumber`, `PhoneNumberTypeID`

---

## 📞 `Person.PhoneNumberType`

### ✨ Opis funkcjonalny  
Typy numerów telefonów – np. komórkowy, domowy, służbowy.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `PhoneNumberTypeID`| `int` NOT NULL IDENTITY(1,1)          |
| `Name`             | `nvarchar(50)` NOT NULL               |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `Person.PersonPhone`

---

## 💼 `Production.Culture`

### ✨ Opis funkcjonalny  
Zawiera kody kulturowe (języki, regiony) wykorzystywane w tłumaczeniach produktów.

### 🧱 Struktura tabeli

| 🧩 Kolumna     | 📝 Typ danych / Definicja             |
|----------------|----------------------------------------|
| `CultureID`    | `nchar(6)` NOT NULL PRIMARY KEY       |
| `Name`         | `nvarchar(50)` NOT NULL               |
| `ModifiedDate` | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `Production.ProductModelProductDescriptionCulture`

---

## 🧾 `Production.Document`

### ✨ Opis funkcjonalny  
Przechowuje dokumenty (pliki, instrukcje, plany techniczne) związane z produkcją.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `DocumentNode`     | `hierarchyid` PRIMARY KEY             |
| `DocumentLevel`    | `smallint` NULL                       |
| `Title`            | `nvarchar(50)` NOT NULL               |
| `Owner`            | `int` NOT NULL                        |
| `FolderFlag`       | `bit` NOT NULL                        |
| `FileName`         | `nvarchar(400)` NOT NULL              |
| `FileExtension`    | `nvarchar(8)` NOT NULL                |
| `Revision`         | `nchar(5)` NOT NULL                   |
| `ChangeNumber`     | `int` NOT NULL                        |
| `Status`           | `tinyint` NOT NULL                    |
| `DocumentSummary`  | `nvarchar(max)` NULL                  |
| `Document`         | `varbinary(max)` NULL                 |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Używana przez `Production.ProductDocument`

---

## 🏗️ `Production.Illustration`

### ✨ Opis funkcjonalny  
Zawiera grafiki/ilustracje produktów.

### 🧱 Struktura tabeli

| 🧩 Kolumna     | 📝 Typ danych / Definicja             |
|----------------|----------------------------------------|
| `IllustrationID` | `int` NOT NULL IDENTITY(1,1)        |
| `Diagram`      | `xml` NULL                            |
| `ModifiedDate` | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `Production.ProductModelIllustration`

---

## 📐 `Production.Location`

### ✨ Opis funkcjonalny  
Definiuje fizyczne lokalizacje w magazynach (np. półki, sekcje).

### 🧱 Struktura tabeli

| 🧩 Kolumna       | 📝 Typ danych / Definicja             |
|------------------|----------------------------------------|
| `LocationID`     | `smallint` NOT NULL IDENTITY(1,1)     |
| `Name`           | `nvarchar(50)` NOT NULL               |
| `CostRate`       | `smallmoney` NOT NULL                 |
| `Availability`   | `decimal(8,2)` NOT NULL               |
| `ModifiedDate`   | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Używana w `Production.ProductInventory` i `Production.WorkOrderRouting`

---

## ⚙️ `Production.Product`

### ✨ Opis funkcjonalny  
Główna tabela przechowująca dane o produktach.

### 🧱 Struktura tabeli

| 🧩 Kolumna             | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `ProductID`            | `int` NOT NULL IDENTITY(1,1)          |
| `Name`                 | `nvarchar(50)` NOT NULL               |
| `ProductNumber`        | `nvarchar(25)` NOT NULL               |
| `MakeFlag`             | `bit` NOT NULL                        |
| `FinishedGoodsFlag`    | `bit` NOT NULL                        |
| `Color`                | `nvarchar(15)` NULL                   |
| `SafetyStockLevel`     | `smallint` NOT NULL                   |
| `ReorderPoint`         | `smallint` NOT NULL                   |
| `StandardCost`         | `money` NOT NULL                      |
| `ListPrice`            | `money` NOT NULL                      |
| `Size`                 | `nvarchar(5)` NULL                    |
| `SizeUnitMeasureCode`  | `nchar(3)` NULL                       |
| `WeightUnitMeasureCode`| `nchar(3)` NULL                       |
| `Weight`               | `decimal(8,2)` NULL                   |
| `DaysToManufacture`    | `int` NOT NULL                        |
| `ProductLine`          | `nchar(2)` NULL                       |
| `Class`                | `nchar(2)` NULL                       |
| `Style`                | `nchar(2)` NULL                       |
| `ProductSubcategoryID` | `int` NULL                            |
| `ProductModelID`       | `int` NULL                            |
| `SellStartDate`        | `datetime` NOT NULL                   |
| `SellEndDate`          | `datetime` NULL                       |
| `DiscontinuedDate`     | `datetime` NULL                       |
| `rowguid`              | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`         | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Sercowa tabela dla produkcji i sprzedaży  
- Połączona z `Sales.SalesOrderDetail`, `Production.ProductInventory`, `Production.ProductCostHistory`, itd.

---


## 🧪 `Production.ProductCategory`

### ✨ Opis funkcjonalny  
Grupuje produkty w szerokie kategorie, np. "Rowery", "Akcesoria".

### 🧱 Struktura tabeli

| 🧩 Kolumna       | 📝 Typ danych / Definicja             |
|------------------|----------------------------------------|
| `ProductCategoryID` | `int` NOT NULL IDENTITY(1,1)       |
| `Name`           | `nvarchar(50)` NOT NULL               |
| `rowguid`        | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`   | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączona z `Production.ProductSubcategory`

---

## 🧵 `Production.ProductCostHistory`

### ✨ Opis funkcjonalny  
Zawiera historię kosztów standardowych produktów.

### 🧱 Struktura tabeli

| 🧩 Kolumna       | 📝 Typ danych / Definicja             |
|------------------|----------------------------------------|
| `ProductID`      | `int` NOT NULL                        |
| `StartDate`      | `datetime` NOT NULL                   |
| `EndDate`        | `datetime` NULL                       |
| `StandardCost`   | `money` NOT NULL                      |
| `ModifiedDate`   | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Z `Production.Product`  
- Klucz główny: `ProductID`, `StartDate`

---

## 🔧 `Production.ProductDescription`

### ✨ Opis funkcjonalny  
Opis tekstowy produktu – używany w różnych językach/kulturach.

### 🧱 Struktura tabeli

| 🧩 Kolumna           | 📝 Typ danych / Definicja             |
|----------------------|----------------------------------------|
| `ProductDescriptionID` | `int` NOT NULL IDENTITY(1,1)        |
| `Description`        | `nvarchar(400)` NOT NULL              |
| `rowguid`            | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`       | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączona z `ProductModelProductDescriptionCulture`

---

## 🧬 `Production.ProductDocument`

### ✨ Opis funkcjonalny  
Relacja między produktami a dokumentacją techniczną.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `ProductID`        | `int` NOT NULL                        |
| `DocumentNode`     | `hierarchyid` NOT NULL                |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączy `Production.Product` z `Production.Document`

---

## 📂 `Production.ProductInventory`

### ✨ Opis funkcjonalny  
Śledzi stany magazynowe produktów w różnych lokalizacjach.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `ProductID`        | `int` NOT NULL                        |
| `LocationID`       | `smallint` NOT NULL                   |
| `Shelf`            | `nvarchar(10)` NOT NULL               |
| `Bin`              | `tinyint` NOT NULL                    |
| `Quantity`         | `smallint` NOT NULL                   |
| `rowguid`          | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Z `Production.Product` i `Production.Location`  
- Klucz główny: `ProductID`, `LocationID`

---

## 🧮 `Production.ProductListPriceHistory`

### ✨ Opis funkcjonalny  
Zawiera historię zmian ceny katalogowej (List Price) produktów.

### 🧱 Struktura tabeli

| 🧩 Kolumna      | 📝 Typ danych / Definicja             |
|-----------------|----------------------------------------|
| `ProductID`     | `int` NOT NULL                        |
| `StartDate`     | `datetime` NOT NULL                   |
| `EndDate`       | `datetime` NULL                       |
| `ListPrice`     | `money` NOT NULL                      |
| `ModifiedDate`  | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Połączona z `Production.Product`  
- Klucz główny: `ProductID`, `StartDate`

---

## 🖼️ `Production.ProductModel`

### ✨ Opis funkcjonalny  
Model produktu – definicja cech i zachowania rodziny produktów.

### 🧱 Struktura tabeli

| 🧩 Kolumna          | 📝 Typ danych / Definicja             |
|---------------------|----------------------------------------|
| `ProductModelID`    | `int` NOT NULL IDENTITY(1,1)          |
| `Name`              | `nvarchar(50)` NOT NULL               |
| `CatalogDescription`| `xml` NULL                            |
| `Instructions`      | `xml` NULL                            |
| `rowguid`           | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`      | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Połączona z `Production.Product`, `ProductModelIllustration`, `ProductModelProductDescriptionCulture`

---

## 🎨 `Production.ProductModelIllustration`

### ✨ Opis funkcjonalny  
Łączy modele produktów z ilustracjami.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `ProductModelID`   | `int` NOT NULL                        |
| `IllustrationID`   | `int` NOT NULL                        |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączy `ProductModel` z `Illustration`  
- Klucz główny: `ProductModelID`, `IllustrationID`

---

## 🌐 `Production.ProductModelProductDescriptionCulture`

### ✨ Opis funkcjonalny  
Zawiera opisy produktów powiązane z modelami i kulturami językowymi.

### 🧱 Struktura tabeli

| 🧩 Kolumna             | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `ProductModelID`       | `int` NOT NULL                        |
| `ProductDescriptionID` | `int` NOT NULL                        |
| `CultureID`            | `nchar(6)` NOT NULL                   |
| `ModifiedDate`         | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `ProductModel`, `ProductDescription`, `Culture`  
- Klucz główny: wszystkie 3 kolumny razem

---

## 🧭 `Production.ProductPhoto`

### ✨ Opis funkcjonalny  
Zawiera zdjęcia produktów – miniatury i duże wersje.

### 🧱 Struktura tabeli

| 🧩 Kolumna          | 📝 Typ danych / Definicja             |
|---------------------|----------------------------------------|
| `ProductPhotoID`    | `int` NOT NULL IDENTITY(1,1)          |
| `ThumbNailPhoto`    | `varbinary(max)` NULL                 |
| `ThumbnailPhotoFileName` | `nvarchar(50)` NULL             |
| `LargePhoto`        | `varbinary(max)` NULL                 |
| `LargePhotoFileName`| `nvarchar(50)` NULL                  |
| `ModifiedDate`      | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączona z `Production.ProductProductPhoto`

---


## 🖼️ `Production.ProductProductPhoto`

### ✨ Opis funkcjonalny  
Tabela pośrednicząca między produktami a ich zdjęciami.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `ProductID`        | `int` NOT NULL                        |
| `ProductPhotoID`   | `int` NOT NULL                        |
| `Primary`          | `bit` NOT NULL                        |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączy `Product` z `ProductPhoto`  
- Klucz główny: `ProductID`, `ProductPhotoID`

---

## 🧩 `Production.ProductReview`

### ✨ Opis funkcjonalny  
Zawiera recenzje produktów dodawane przez klientów.

### 🧱 Struktura tabeli

| 🧩 Kolumna       | 📝 Typ danych / Definicja             |
|------------------|----------------------------------------|
| `ProductReviewID`| `int` NOT NULL IDENTITY(1,1)          |
| `ProductID`      | `int` NOT NULL                        |
| `ReviewerName`   | `nvarchar(50)` NOT NULL               |
| `ReviewDate`     | `datetime` NOT NULL                   |
| `EmailAddress`   | `nvarchar(50)` NOT NULL               |
| `Rating`         | `int` NOT NULL                        |
| `Comments`       | `nvarchar(3850)` NULL                 |
| `ModifiedDate`   | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `Production.Product`

---

## 🏷️ `Production.ProductSubcategory`

### ✨ Opis funkcjonalny  
Podkategorie produktów, np. "Rowery górskie" w kategorii "Rowery".

### 🧱 Struktura tabeli

| 🧩 Kolumna             | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `ProductSubcategoryID` | `int` NOT NULL IDENTITY(1,1)          |
| `ProductCategoryID`    | `int` NOT NULL                        |
| `Name`                 | `nvarchar(50)` NOT NULL               |
| `rowguid`              | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`         | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `ProductCategory`  
- Używana przez `Product`

---

## 🛠️ `Production.ProductVendor`

### ✨ Opis funkcjonalny  
Łączy produkty z ich dostawcami (vendorami).

### 🧱 Struktura tabeli

| 🧩 Kolumna             | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `ProductID`            | `int` NOT NULL                        |
| `BusinessEntityID`     | `int` NOT NULL                        |
| `AverageLeadTime`      | `int` NOT NULL                        |
| `StandardPrice`        | `money` NOT NULL                      |
| `LastReceiptCost`      | `money` NULL                          |
| `LastReceiptDate`      | `datetime` NULL                       |
| `MinOrderQty`          | `int` NOT NULL                        |
| `MaxOrderQty`          | `int` NOT NULL                        |
| `OnOrderQty`           | `int` NULL                            |
| `UnitMeasureCode`      | `nchar(3)` NOT NULL                   |
| `ModifiedDate`         | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `Product`, `Vendor`, `UnitMeasure`

---

## 📊 `Production.ScrapReason`

### ✨ Opis funkcjonalny  
Zawiera powody odrzucenia materiału w produkcji.

### 🧱 Struktura tabeli

| 🧩 Kolumna      | 📝 Typ danych / Definicja             |
|-----------------|----------------------------------------|
| `ScrapReasonID` | `smallint` NOT NULL IDENTITY(1,1)     |
| `Name`          | `nvarchar(50)` NOT NULL               |
| `ModifiedDate`  | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Używana w `Production.WorkOrder`

---

## 🛠️ `Production.UnitMeasure`

### ✨ Opis funkcjonalny  
Zawiera jednostki miary (np. sztuki, litry, gramy) wykorzystywane w produktach i produkcji.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `UnitMeasureCode`  | `nchar(3)` NOT NULL PRIMARY KEY       |
| `Name`             | `nvarchar(50)` NOT NULL               |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Wykorzystywana w `Product`, `ProductVendor`, `BillOfMaterials`, itd.

---

## 🧾 `Production.WorkOrder`

### ✨ Opis funkcjonalny  
Zawiera zlecenia produkcyjne – jakie produkty mają być wytworzone, w jakiej ilości, w jakim terminie.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `WorkOrderID`      | `int` NOT NULL IDENTITY(1,1)          |
| `ProductID`        | `int` NOT NULL                        |
| `OrderQty`         | `int` NOT NULL                        |
| `StockedQty`       | `int` NOT NULL                        |
| `ScrappedQty`      | `smallint` NOT NULL                   |
| `StartDate`        | `datetime` NOT NULL                   |
| `EndDate`          | `datetime` NULL                       |
| `DueDate`          | `datetime` NOT NULL                   |
| `ScrapReasonID`    | `smallint` NULL                       |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączy się z `Product`, `ScrapReason`  
- Powiązana z `WorkOrderRouting`

---

## 🔄 `Production.WorkOrderRouting`

### ✨ Opis funkcjonalny  
Definiuje trasę wykonania zlecenia produkcyjnego – krok po kroku.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `WorkOrderID`      | `int` NOT NULL                        |
| `ProductID`        | `int` NOT NULL                        |
| `OperationSequence`| `smallint` NOT NULL                   |
| `LocationID`       | `smallint` NOT NULL                   |
| `ScheduledStartDate`| `datetime` NOT NULL                 |
| `ScheduledEndDate` | `datetime` NOT NULL                   |
| `ActualStartDate`  | `datetime` NULL                       |
| `ActualEndDate`    | `datetime` NULL                       |
| `ActualResourceHrs`| `decimal(9,4)` NULL                   |
| `PlannedCost`      | `money` NOT NULL                      |
| `ActualCost`       | `money` NULL                          |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączona z `WorkOrder`, `Location`, `Product`

---

## 🏢 `Purchasing.Vendor`

### ✨ Opis funkcjonalny  
Zawiera informacje o dostawcach – firmach zaopatrujących w surowce i komponenty.

### 🧱 Struktura tabeli

| 🧩 Kolumna             | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `BusinessEntityID`     | `int` NOT NULL                        |
| `AccountNumber`        | `nvarchar(15)` NOT NULL               |
| `Name`                 | `nvarchar(50)` NOT NULL               |
| `CreditRating`         | `tinyint` NOT NULL                    |
| `PreferredVendorStatus`| `bit` NOT NULL                        |
| `ActiveFlag`           | `bit` NOT NULL                        |
| `PurchasingWebServiceURL` | `nvarchar(1024)` NULL             |
| `ModifiedDate`         | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `ProductVendor`, `PurchaseOrderHeader`

---

## 🧾 `Purchasing.PurchaseOrderDetail`

### ✨ Opis funkcjonalny  
Szczegóły pozycji w zamówieniu zakupu – co dokładnie zostało zamówione.

### 🧱 Struktura tabeli

| 🧩 Kolumna         | 📝 Typ danych / Definicja             |
|--------------------|----------------------------------------|
| `PurchaseOrderID`  | `int` NOT NULL                        |
| `PurchaseOrderDetailID` | `int` NOT NULL IDENTITY(1,1)     |
| `DueDate`          | `datetime` NOT NULL                   |
| `OrderQty`         | `smallint` NOT NULL                   |
| `ProductID`        | `int` NOT NULL                        |
| `UnitPrice`        | `money` NOT NULL                      |
| `LineTotal`        | `numeric(38,6)` COMPUTED              |
| `ReceivedQty`      | `decimal(8,2)` NOT NULL               |
| `RejectedQty`      | `decimal(8,2)` NOT NULL               |
| `StockedQty`       | `decimal(9,2)` COMPUTED               |
| `ModifiedDate`     | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `Purchasing.PurchaseOrderHeader`  
- Łączona z `Production.Product`

---

## 📦 `Purchasing.PurchaseOrderHeader`

### ✨ Opis funkcjonalny  
Główne informacje o zamówieniu zakupu – np. numer zamówienia, dostawca, status.

### 🧱 Struktura tabeli

| 🧩 Kolumna           | 📝 Typ danych / Definicja             |
|----------------------|----------------------------------------|
| `PurchaseOrderID`    | `int` NOT NULL IDENTITY(1,1)          |
| `RevisionNumber`     | `tinyint` NOT NULL                    |
| `Status`             | `tinyint` NOT NULL                    |
| `EmployeeID`         | `int` NOT NULL                        |
| `VendorID`           | `int` NOT NULL                        |
| `ShipMethodID`       | `int` NOT NULL                        |
| `OrderDate`          | `datetime` NOT NULL                   |
| `ShipDate`           | `datetime` NULL                       |
| `SubTotal`           | `money` NOT NULL                      |
| `TaxAmt`             | `money` NOT NULL                      |
| `Freight`            | `money` NOT NULL                      |
| `TotalDue`           | `money` COMPUTED                      |
| `ModifiedDate`       | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączona z `Vendor`, `Employee`, `ShipMethod`, `PurchaseOrderDetail`

---

## 🧍‍♂️ `HumanResources.Employee`

### ✨ Opis funkcjonalny  
Dane pracowników firmy – stanowisko, etat, menedżer, daty zatrudnienia.

### 🧱 Struktura tabeli

| 🧩 Kolumna           | 📝 Typ danych / Definicja             |
|----------------------|----------------------------------------|
| `BusinessEntityID`   | `int` NOT NULL                        |
| `NationalIDNumber`   | `nvarchar(15)` NOT NULL               |
| `LoginID`            | `nvarchar(256)` NOT NULL              |
| `OrganizationNode`   | `hierarchyid` NULL                    |
| `OrganizationLevel`  | `smallint` COMPUTED                   |
| `JobTitle`           | `nvarchar(50)` NOT NULL               |
| `BirthDate`          | `date` NOT NULL                       |
| `MaritalStatus`      | `nchar(1)` NOT NULL                   |
| `Gender`             | `nchar(1)` NOT NULL                   |
| `HireDate`           | `date` NOT NULL                       |
| `SalariedFlag`       | `bit` NOT NULL                        |
| `VacationHours`      | `smallint` NOT NULL                   |
| `SickLeaveHours`     | `smallint` NOT NULL                   |
| `CurrentFlag`        | `bit` NOT NULL                        |
| `rowguid`            | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`       | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `HumanResources.EmployeeDepartmentHistory`, `Sales.SalesPerson`

---

## 🏢 `HumanResources.Department`

### ✨ Opis funkcjonalny  
Zawiera listę działów firmy – np. produkcja, HR, księgowość.

### 🧱 Struktura tabeli

| 🧩 Kolumna     | 📝 Typ danych / Definicja             |
|----------------|----------------------------------------|
| `DepartmentID` | `smallint` NOT NULL IDENTITY(1,1)     |
| `Name`         | `nvarchar(50)` NOT NULL               |
| `GroupName`    | `nvarchar(50)` NOT NULL               |
| `ModifiedDate` | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Łączona z `EmployeeDepartmentHistory`

---

## 🕓 `HumanResources.EmployeeDepartmentHistory`

### ✨ Opis funkcjonalny  
Historia przypisań pracowników do działów i zmian stanowisk.

### 🧱 Struktura tabeli

| 🧩 Kolumna           | 📝 Typ danych / Definicja             |
|----------------------|----------------------------------------|
| `BusinessEntityID`   | `int` NOT NULL                        |
| `DepartmentID`       | `smallint` NOT NULL                   |
| `ShiftID`            | `tinyint` NOT NULL                    |
| `StartDate`          | `date` NOT NULL                       |
| `EndDate`            | `date` NULL                           |
| `ModifiedDate`       | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Połączenie `Employee`, `Department`, `Shift`

---


## 🕒 `HumanResources.Shift`

### ✨ Opis funkcjonalny  
Zawiera informacje o zmianach pracowniczych – poranna, popołudniowa, nocna.

### 🧱 Struktura tabeli

| 🧩 Kolumna     | 📝 Typ danych / Definicja             |
|----------------|----------------------------------------|
| `ShiftID`      | `tinyint` NOT NULL IDENTITY(1,1)      |
| `Name`         | `nvarchar(50)` NOT NULL               |
| `StartTime`    | `time(0)` NOT NULL                    |
| `EndTime`      | `time(0)` NOT NULL                    |
| `ModifiedDate` | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Wykorzystywana przez `EmployeeDepartmentHistory`

---

## 💼 `Sales.Customer`

### ✨ Opis funkcjonalny  
Reprezentuje klienta – może to być osoba fizyczna lub firma.

### 🧱 Struktura tabeli

| 🧩 Kolumna           | 📝 Typ danych / Definicja             |
|----------------------|----------------------------------------|
| `CustomerID`         | `int` NOT NULL IDENTITY(1,1)          |
| `PersonID`           | `int` NULL                            |
| `StoreID`            | `int` NULL                            |
| `TerritoryID`        | `int` NULL                            |
| `AccountNumber`      | `nvarchar(10)` NOT NULL               |
| `rowguid`            | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`       | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `Sales.SalesOrderHeader`, `Sales.Store`  
- Klient może być osobą (`Person`) lub firmą (`Store`)

---

## 🧾 `Sales.SalesOrderHeader`

### ✨ Opis funkcjonalny  
Zawiera nagłówki zamówień sprzedaży – kto zamówił, kiedy, ile zapłacił.

### 🧱 Struktura tabeli

| 🧩 Kolumna             | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `SalesOrderID`         | `int` NOT NULL IDENTITY(1,1)          |
| `RevisionNumber`       | `tinyint` NOT NULL                    |
| `OrderDate`            | `datetime` NOT NULL                   |
| `DueDate`              | `datetime` NOT NULL                   |
| `ShipDate`             | `datetime` NULL                       |
| `Status`               | `tinyint` NOT NULL                    |
| `OnlineOrderFlag`      | `bit` NOT NULL                        |
| `SalesOrderNumber`     | `nvarchar(25)` COMPUTED               |
| `PurchaseOrderNumber`  | `nvarchar(25)` NULL                   |
| `AccountNumber`        | `nvarchar(15)` NULL                   |
| `CustomerID`           | `int` NOT NULL                        |
| `SalesPersonID`        | `int` NULL                            |
| `TerritoryID`          | `int` NULL                            |
| `BillToAddressID`      | `int` NOT NULL                        |
| `ShipToAddressID`      | `int` NOT NULL                        |
| `ShipMethodID`         | `int` NOT NULL                        |
| `CreditCardID`         | `int` NULL                            |
| `CreditCardApprovalCode` | `varchar(15)` NULL                 |
| `CurrencyRateID`       | `int` NULL                            |
| `SubTotal`             | `money` NOT NULL                      |
| `TaxAmt`               | `money` NOT NULL                      |
| `Freight`              | `money` NOT NULL                      |
| `TotalDue`             | `money` COMPUTED                      |
| `Comment`              | `nvarchar(128)` NULL                  |
| `rowguid`              | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`         | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z: `Customer`, `SalesPerson`, `CreditCard`, `ShipMethod`, `Address`, `SalesOrderDetail`

---

## 📦 `Sales.SalesOrderDetail`

### ✨ Opis funkcjonalny  
Pozycje zamówienia – co konkretnie zostało kupione, w jakiej ilości i za ile.

### 🧱 Struktura tabeli

| 🧩 Kolumna             | 📝 Typ danych / Definicja             |
|------------------------|----------------------------------------|
| `SalesOrderID`         | `int` NOT NULL                        |
| `SalesOrderDetailID`   | `int` NOT NULL IDENTITY(1,1)          |
| `CarrierTrackingNumber`| `nvarchar(25)` NULL                   |
| `OrderQty`             | `smallint` NOT NULL                   |
| `ProductID`            | `int` NOT NULL                        |
| `SpecialOfferID`       | `int` NOT NULL                        |
| `UnitPrice`            | `money` NOT NULL                      |
| `UnitPriceDiscount`    | `money` NOT NULL                      |
| `LineTotal`            | `numeric(38,6)` COMPUTED              |
| `rowguid`              | `uniqueidentifier` ROWGUIDCOL NOT NULL |
| `ModifiedDate`         | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Powiązana z `SalesOrderHeader`, `Product`, `SpecialOffer`

---

## 💸 `Sales.Currency`

### ✨ Opis funkcjonalny  
Zawiera listę walut (kod ISO, nazwa) używanych w transakcjach.

### 🧱 Struktura tabeli

| 🧩 Kolumna     | 📝 Typ danych / Definicja             |
|----------------|----------------------------------------|
| `CurrencyCode` | `nchar(3)` NOT NULL PRIMARY KEY       |
| `Name`         | `nvarchar(50)` NOT NULL               |
| `ModifiedDate` | `datetime` NOT NULL                   |

### 🔗 Relacje  
- Używana w `Sales.CurrencyRate`, `CountryRegionCurrency`

---



