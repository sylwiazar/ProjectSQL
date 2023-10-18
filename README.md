### Abstract:
The real estate office keeps a records of its clients - both landlords and people looking for apartments for rent. It also collects data on rental offers and contracts signed through the office. The client is the owner of a real estate office with high standards.

### The client's main requirements are: 
- The ability to search for deals that are available
- Control of currently concluded contracts

### The data contained in the database are:
- Customer data
- Description of apartments
- Rental offers
- Contracts

### The database does not contain data on:
- Office employees

### Assumptions:
- An office employee cannot make an offer to rent a particular apartment earlier than 2 months before the rental start date.
- Offer and contract entries are added chronologically. That is for offers there is a correlation between offer date and offer ID, for contracts there is a correlation between rental start date and contract ID.

### Example usage scenarios:
- An office worker searches for [rental housing listings](https://github.com/sylwiazar/ProjectSQL/blob/main/List_of_flats_for_rent) available in a particular city for a client who wants to rent an apartment.
- The office worker searches for [contracts coming to an end](https://github.com/sylwiazar/ProjectSQL/blob/main/Expiring_contracts) to offer the client a rental extension.
- The office owner calculates a [monthly commission X](https://github.com/sylwiazar/ProjectSQL/blob/main/Monthly_commission) on all contracts for the month.
- The office worker acquires a new client. The [new client's data](https://github.com/sylwiazar/ProjectSQL/blob/main/New_client) is recorded in the Person table.
  - If the client is the owner of a rental apartment, the [apartment data](https://github.com/sylwiazar/ProjectSQL/blob/main/New_flat_for_rent) are entered in the Flat table.
    - If for a given apartment the owner establishes a suitable [offer for rent](https://github.com/sylwiazar/ProjectSQL/blob/main/New_offer), the offer data are entered in the Offer table, respectively.
  - If the customer is a tenant and decides to rent the apartment from a given offer, the [contract data](https://github.com/sylwiazar/ProjectSQL/blob/main/New_contract) are entered in the Rent table.
 
### ERD
![ERD](https://github.com/sylwiazar/ProjectSQL/assets/60239530/abb14e10-a1e1-4686-810c-47b47641c54e)

### Entity Set Description:

| Person |
| :---: |
| It contains basic information about clients. A client can be an apartment owner, as well as a person who wants to rent an apartment. Entities are inserted into the collection when a client is reported to the real estate office, or is obtained by an employee of the office. The client's data is not removed from the collection, even if the client stops using the bureau's services. |
| personId - int - Person ID |
| firstName	- varchar(20)	- Customer's name |
| lastName	- varchar(40)	- Customer's last name |
| phoneNumber	- varchar(15)	- Contact number for the customer |

| Flat |
| :---: |
| It contains basic information about apartments for rent. Entities are inserted into the collection when a client presents a desire to advertise his apartment for rent. The apartment data is not removed from the collection if the owner no longer wishes to rent it. |
| flatId - int - Unique apartment ID |
| addressFlat - varchar(100) - The address at which the apartment is located |
| availabilityRent - bit - Availability of the apartment for rent, the owner may decide to sell the apartment, in which case the attribute takes the value 0 |
| rooms - tinyint - Number of rooms available in the apartment |
| area - tinyint - Area of the apartment |
| balcony - bit	- Information about having a balcony in the apartment |
| city - varchar(25) - The city in which the apartment is located |
| ownerId - int - The ID of the apartment owner |

| Offer |
| :---: |
| It contains information about proposed rental offers for a given apartment. Entities are inserted into the collection when a client decides to rent his apartment. Offers are not removed from the collection if the apartment is no longer available for rent. |
| offerId - int - OfferID |
| price - int - Rental price per month, the last offer before the rental date is the final offer for which the contract is concluded |
| offer_date - date - Date of issuance of the offer |
| flat_Id - int - The ID of the flat which offer describes |
| newestOffer - bit - It's 1 for the newest offer for the flat and 0 for the rest of the offers |

| Rent |
| :---: |
| It contains basic information of the contract signed for renting an apartment. Entites are inserted into the collection when the customer decides to rent the apartment in question. Contract data is not deleted from the collection if its validity ends. |
| contractID - int - Unique contract ID |
| dateStart - date - Rental start date |
| dateExp - date - End date of rental |
| tenantID - int - ID of the person renting |
| flatID - int - ID of the apartment which is the object of the contract |
| newestContract - bit - It's 1 for the newest contract for the flat and 0 for the rest of the contracts |

### Relationships Description:

| Name | Entity set 1 | Entity set 2 | Quantity | Description |
| :---: | :---: | :---: | :---: | :---: |
| Own	| Person	| Flat	| 1 : 0..n | The relationship assigns owners to the apartments they own. One person may have several apartments for rent, or be a person who wants to rent an apartment from someone and then owns no apartment at all. The apartment has only one owner. The association is formed when the owner shows a desire to enter his apartment in the real estate office's database. |
| Is_for_rent |	Offer	| Flat	| 0..n : 1 | The relationship assigns offers for a particular apartment for rent. Depending on market prices, offers for a given apartment may change, or the owner has not yet decided to list the apartment under a specific offer. A given offer always applies to a single apartment. The association is formed when the owner decides to issue a new offer for a given apartment. |
| Is_object	| Rent	| Flat	| 0..n : 1 | The relationship assigns a given apartment to a signed contract. A given apartment may not yet have any signed contract, or have historically had multiple tenants. A given contract is associated with only one apartment. The association is created when the customer signs the contract for rent. |
| Rent	| Person	| Rent	| 1 : 0..n | The relationship assigns the data of the renter to the contract. A person may historically have many signed contracts, or has not yet signed any contract. A given contract always applies to one person. The association is created when the customer signs the contract for rent. |

### Relational database schema:

- Person(personId, firstName, lastName, phoneNumber)
- Flat(flatId, addressFlat, availabilityRent, rooms, area, balcony, city, ownerId REF Person)
- Offer(offerId, price, offerDate, newestOffert, flatId REF Flat)
- Rent(contractId, dateStart, dateExp, newestContract, tenantId REF Person, flatId REF Flat) 
