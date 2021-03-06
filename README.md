# Onboarding - account opening API Service

## Definitions ##

* [Entity Defintion](#definition_entity)
* [KYC Definition](#definition_kyc)
* [Account Definition](#definition_account)

#### <a id="definition_entity"></a> Define an Entity ####

An Entity can be both a Corporate (moral person) and an Individual (physical person) listed in the following couterparties:
* Lead: Basic form of an Entity.
* Customer: When coupled with a KYC (and validated by IbanFirst).
* Customer's Client: When coupled with a KYCC (and validated by IbanFirst)
* Shareholder: When coupled with a KYCS (and validated by IbanFirst).
* Users: When coupled with a KYCU (and validated by IbanFirst).
* Supplier: When coupled with an Account.
* Beneficiary: When coupled with an Account.

#### <a id="definition_kyc"></a> Define the KYC(s) ####

Know Your Customer (KYC) is the process identifying and verifying the identity of an Entity.
Several level of KYC may be required depending on the type of relationship between IbanFirst and the Entity:
* Know Your Customer (KYC): Standard Entity Identification Procedure for direct clients.
* Know your Customers' Shareholder (KYCS): Specific Entity Identification Procedure for shareholders of IbanFirst's direct clients. 
* Know your Customers' Users (KYCU): Specific Entity Identification Procedure for users of IbanFirst's direct clients. 
* Know Your Customers' Counterparty (KYCC) : Third-party Customer Identification Procedure for indirect clients of licensed IbanFirst's direct clients.

#### <a id="definition_account"></a> Define an Account ####

An Account is identifying the bank details of an Entity thats can be an IbanFirst's Client or not.
* IBAN account (IBAN): Standard Payment Account provided by IbanFirst for direct registered Customers or Customers' Clients.
* Escrow IBAN account (EIBAN): Specific Escrow Account provided by IbanFirst for capital deposit purpose.
* Liquidity account: TBD.
* Counterparty account: external bank account referenced with IbanFirst (typically a Beneficiary of a payment).


#### <a id="definition_kyt"></a> Define the KYT ####

Know Your transaction (KYT) is the process permitting to spot potential risky transactions and unusual behaviour on an account.

## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /entity/`](#post_entity) | Post a new entity |
| [`POST /entity/-{id}/kyc`](#post_entity) | Add a KYC to an entity |
| [`POST /entity/-{id}/kyc/documentation/`](#post_documentation) | Post a Documentation related to an entity |
| [`POST /entity/-{id}/kyc/organizationStructure/`](#post_organisationStructure) | Post an Organisation Structure related to an entity |
| [`POST /contact/`](#post_contact) | Post a new contact |

## Details ##

#### <a id="post_entity"></a> Create an Entity ####

```
Method: POST 
URL: /entity/
```
Create a new entity.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| registeredParentNumber | String(100) | Optional | The unique legal identifier of the entity's parent registered company. |
| registeredNumber | String(100) | Required |  The unique legal identifier of the entity opening the account. |
| registeredName | String(100) | Required |  The legal name of the entity. |
| commercialName | String(100) | Optional |  The commercial name of the entity. |
| tag | String(100) | Optional |  The customized name of the entity. |
| address | [Address Object](#address_object) | Required |  The entity address. |
| phoneNumber | [phone Object](#phone_object)  | Optional |  The phone number of the entity. |
| email | [email Object](#email_object)  | Optional |  The  email address of the entity. |
| primaryUrl | [email Object](#email_object)  | Optional |  The  email address of the entity. |

#### <a id="post_entity"></a> Add KYC informations to an Entity ####

```
Method: POST 
URL: /entity/-{id}/kyc/details
```
Add KYC informations to an Entity.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| activityCode | [NAF](#NAF_type) | Required |  The code identifying the type of business. |
| registrationDate | [Date](#type_date) | Required |  The legal date of creation of the entity. |
| legalForm | [legalForm](#legalForm) | Required |  The legal form of the entity. |
| authorizedCapital | [amount Object](#amount_object)  | Required |  The amount in shareholding capital. |

#### <a id="post_entity"></a> Add KYC documentation to an Entity ####

```
Method: POST 
URL: /entity/-{id}/kyc/documentation
```
Add KYC documentation to an Entity. 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| proofOfIncorporation | [page](#page) | Required |  The code identifying the type of business. |
| registrationDate | [Date](#type_date) | Required |  The legal date of creation of the entity. |
| legalForm | [legalForm](#legalForm) | Required |  The legal form of the entity. |
| authorizedCapital | [amount Object](#amount_object)  | Required |  The amount in shareholding capital. |

#### <a id="post_entity"></a> Add KYC Organization Structure to an Entity ####

```
Method: POST 
URL: /entity/-{id}/kyc/organisationStructure
```
This structure describes an organisation structure.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| entities | Array[[entities](#entities_object)] | Optional | List of the entities that own more than 10% of the entity we want to create. |
| contacts | Array[[contacts](#contacts_object)] | Optional | List of the contacts that own more than 10% of the entity we want to create. |

# API Objects  

* [Address Object](#address_object)
* [Entity Object](#entity_object)
* [entities](#entities_object)
* [Contact Object](#contact_object)
* [contacts](#contacts_object)
* [Account Object](#account_object)
* [Phone Object](#phone_object)
* [Individual Name Object](#individualName_object)
* [Amount Object](#amount_object)

## Details ##

#### <a id="entity_object"></a> Entity Object ####

What we call an Entity can be only an company incorporated in France or Belgium.
When an entity is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the entity. |
| registeredParentNumber | String(100) | The unique legal identifier of the entity mother house. |
| registeredNumber | String(100) | The unique legal identifier of the entity opening the account. |
| registeredName | String(100) | The legal name of the entity. |
| commercialName | String(100) | The commercial name of the entity. |
| tag | String(100) | The customized name of the entity. |
| address | [Address Object](#address_object) | The entity address. |
| activityCode | [NAFID](../conventions/formattingConventions.md#NAF) | The code identifying the type of business. |
| registrationDate | [Date](../conventions/formattingConventions.md#type_date) | The legal date of creation of the entity. |
| legalForm | [legalForm](../conventions/formattingConventions.md#legalForm) | The legal form of the entity. |
| authorizedCapital | [amount Object](#amount_object)  | The amount in shareholding capital. |
| phoneNumber | [phone Object](#phone_object)  | The phone number of the entity. |

**Example:**

```js
"entity": {
    "id": "ND4ue2",
    "registeredParentNumber": "	814455614",
    "registeredNumber": "81445561400010",
    "registeredName": "DJPAD",
    "commercialName": "null",
    "tag":"null",
    "address": {address},
    "activityCode":"6201Z",
    "registrationDate":"2015-11-04",
    "legalForm":"SARL unipersonnelle",
    "authorizedCapital":{amount},
    "phoneNumber":{phone},
}
```

<hr />


#### <a id="contact_object"></a> Contact Object ####

What we call a contact can be only an individual registered in France or Belgium.
When an entity is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the contact. |
| registeredNumber | String(100) | The unique legal identifier of the contact. |
| registeredNCountry| String(100) | The registering country of the contact. |
| registeredName | [Individual_Name Object](#individual_name_object) | The individual name of the contact. |
| tag | String(100) | The customized name of the contact. |
| address | [Address Object](#address_object) | The contact address. |
| birthDate | [Date](#type_date) | The birthdate of the contact. |
| phoneNumber | [phone Object](#phone_object)  | The phone number of the entity. |
| position | [position Object](#position_object)  | The position of the entity. |
| idProof | [idProof Object](#idProof_object)  | The url where the file is stored. |
| addressProof | [addressProof Object](#addressProof_object)  | The url where the file is stored. |

**Example:**

```js
"contact": {
    "id": "ND4ue2",
    "registeredNumber": "81445561400010",
    "registeredName": {individual_name},
    "tag":"null",
    "address": {address},
    "birthDate":"1980-11-04",
    "phoneNumber":{phone},
    "position":{position},
}
```

<hr />


#### <a id="account_object"></a> Account Object ####

When an Account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](#type_id) | The code identifying the account. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency of the account. |
| tag |  String(50) | Custom reference of the account. |
| accountNumber | String(40) | The code specifying the account (can be either an Iban or an account number). |
| correspondentBank | [ID](#type_id) | The intermediary bank details, used to reach the beneficiary bank. |
| holderBank | [ID](#type_id) | The recipient bank details, holding the account. |
| holder | [Entity Object](#entity_object) | The recipient details, owner of the account. |

**Example:**

```js
"account": {
    "id": "NT4edA",
    "currency": "EUR",
    "tag": "My wallet account EUR",
    "accountNumber": "516981638516313513",
    "correspondantBank":{correspondentBank}
    "holderBank":{beneficiaryBank}
    "holder":{beneficiary}
}
```

<hr />


#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| street1 | String(255) | The street for the address described. |
| street2 | String(255) | The continuation street for the address described. |
| postCode | String(15) | The ZIP/Post code for the address described. |
| city | String(35) | The city for the address described. |
| state | String(2) | The state code for the address described. This field could be required if the country use a state system, like United States or Canada. To see a full list of state code, please refer to [this site](http://www.mapability.com/ei8ic/contest/states.php). |
| country | String(2) | The two-letters abbreviation for the country, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166) for the address described. |

**Example:**

```js
"address": {
	"street": "4 NEW YORK PLAZA, FLOOR 15",
	"postCode": "10004",
	"city": "NEW YORK",
	"state": "NY",
	"country": "US"
}
```

<hr />

#### <a id="phone_object"></a> Phone Object ####

When a phone number is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| countryCode | String(4) | Numeric country code. Optional +, followed by 1-3 digits. |
| phoneNumber | String(15) | Country code and phone number. |

**Example:**

```js
"phone": {
    "countryCode": "+33",
    "phoneNumber": "81445561400010",
}
```

<hr />

#### <a id="individualName_object"></a> Individual Name Object ####

When a phone number is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| first | String(35) | The individual's first name. Truncated after the first 35 characters. |
| first | String(35) | The individual's middle name. Truncated after the first 35 characters. |
| last | String(35) | The individual's last name. Truncated after the first 35 characters. |

**Example:**

```js
"phone": {
    "first": "John",
    "last": "Doe",
}
```

<hr />

#### <a id="amount_object"></a> Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| value  | [QuotedDecimal](../conventions/formattingConventions.md#type_quoteddecimal) | The quantity of the currency. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency related to the amount. |

**Example:**

```js
"amount": {
	"value": "10000.00",
	"currency": "GBP"
}
```

<hr />

# Formatting Conventions #  

### <a id="type_date"></a> Date Type ###

The Date type represents a date with its year, month and day.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| Date | String | `^[0-9]{4}\-[0-9]{2}\-[0-9]{2}$` - `YYYY-MM-DD` | A String representing a date by its year, month and day in month. | `2015-07-14` |
