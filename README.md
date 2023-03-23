# Stellar trash

This is the main repository for our START Hack challenge. It is divided into ### sections.

## Code

The repository with the code is available at https://github.com/Ubax/bulk-waste-calculator. It includes instructions how to setup the environment and build the final website. There is also basic description of code, future improvement ideas and instructions for implementing changes.

The website can be tested at https://ubax.github.io/stellar-trash/

## Our solution

Our solution consist of four subsystems working together to provide full solution.

```mermaid
flowchart TD
    Calc[Calculator] --> Payment[Payment system] --> Listing[Offers listing]
    Payment --> Collector[Collector application]
```

**Calculator** is already implemented part, that can be easily embedded into any existing website. It lets users estimate the number of stamps needed for their waste in a quick and efficient way.

**Payment system** will be an easy to use platform to buy digital stamps. Based on the previous calculations it will

**Offers listing** will be a website to check all the working objects that people want to throw away. It will present the address and the date for pickup.

**Collector application** will be mobile application used by the collectors to validate the digital stamps.

## State diagram of customer actions in the system

```mermaid
stateDiagram-v2
    state shop <<choice>>
    state list <<choice>>
    state wasListed <<choice>>
    state wasCollected <<choice>>
    Calculator: Opens calculator website and choses objects
    SustainableOrNot: Choses if want to list their object
    Details: Writes description and optionally adds photos
    Payment: Pays immediately
    PaymentDelayed: Payment is delayed (SEPA)
    DigitalStamp: Receives digital stamp code
    Outside: Puts object outside with code/stamp(s)
    MoneyTaken: Money is taken from the account
    PaymentCanceled: Delayed payment was canceled
    Physical: Goes to bulky waste stamp sales point
    BuyStamp: Buys stamp(s)
    [*] --> Calculator
    Calculator --> shop
    shop --> SustainableOrNot: Online shop
    shop --> Physical: Physical shop
    SustainableOrNot --> list
    list --> Details: Wants
    list --> Payment: Doesn't want
    Details --> PaymentDelayed
    PaymentDelayed --> DigitalStamp
    Payment --> DigitalStamp
    Physical --> BuyStamp
    BuyStamp --> Outside
    DigitalStamp --> Outside
    Outside --> wasListed
    wasListed --> wasCollected:Was listed
    wasListed --> [*]:Wasn't listed
    wasCollected --> PaymentCanceled: Was collected by other customer
    wasCollected --> MoneyTaken: Wasn't collected by other customer
    PaymentCanceled --> [*]
    MoneyTaken --> [*]
```

## State diagram of trash collector actions in the system

```mermaid
stateDiagram-v2
    state stamp <<choice>>
    state method <<choice>>
    state prompt <<choice>>
    state valid <<choice>>
    
    SeesTrash: Sees trash
    Reports: Reports illegal waste
    Collects: Collects trash
    SelectsTick: Presses ✅ on tablet
    TypesNumber: Types number
    
    [*] --> SeesTrash
    SeesTrash --> stamp
    stamp --> method: Has stamp
    method --> Collects: Physical stamp
    method --> prompt: Digital stamp (number)
    prompt --> SelectsTick: Number visible on tablet
    SelectsTick --> Collects
    prompt --> TypesNumber
    TypesNumber --> valid
    valid --> Collects: Number is valid
    valid --> Reports: Number is invalid
    stamp --> Reports: Doesn't have stamp
```

## State diagram of used item collector actions in the system

```mermaid
stateDiagram-v2
    Street: Finds item on street with digital stamp
    Online: Finds item on online listing
    Goes: Goes to location on date listed online
    Collects: Collects item
    
    [*] --> Street
    [*] --> Online
    Online --> Goes
    Goes --> Collects
    Street --> Collects
    Collects --> [*]
```