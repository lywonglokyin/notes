# Flow of information

How does information flow through machines?

## Parent-child relation

# Electronic trading

1. Human intervention (e.g UBS trading make the decison) (Out of scope)
2. No intervention (e.g client directly goes to market) (!!!)

## Technical information for hubs

e.g. Hub A and Hub B.

Order type (Tag13): 

DMA - Direct market access (directly sent to market)

(Matches buy and sell side)

DSA - Strategy Acess (Use algorithm to execute order) (Tag 21, Tag 6061)

(varying order?)

(Ways for client desired to trade in market)


Stop at Hub C?

- Algorithm states that the timing is not correct!

## Usage of algorithm

1. For huge buy or sell order, algorithms helps slice the order to ease the impact

### Tag 6000 series

Parameter of strategies (e.g. start time, end time, max quantity etc.)


## Example hubs

CL (A4): Client Layer (Hub for order to enter UBS), client would only communicate with UBS through CL. Translate incoming FIX message to standardize it. (e.g. tag35=D, tag11=A12)

->

CO (Client Sapphire - in Asia): Client order management layer. Would generate its own client ID (tag11). Maintain the state of order. (e.g. tag35=D, tag11=B12, tag526=A12). UBS people would look in CO are behaving normally. Also allocate orders to different region. Check client integrity (single order limit, etc).

MO (MKT Sapphire) (Market OMS): Different MO for different markets (different market has different rules)

(Algo) (SCALE): Algo built by UBS

EG (MOMS)(Exchange gateway): 1. check order validity (fat finger) (franchise protection check)

Market (HKEX)


CL(HK) ->

CL(HK) -> CO(HK) -> MO(HK)

CL(US) -> 

e.g. US client CL(US)->CO(Asia)->MO(HK) (single point of entry)

Example of orders:

1. Unsolicitated cancellation: Got accepted at first, then got rejected by machines afterwards. (usually refering to open state order)


### Open state

Order that has at least 1 open quantity.


#### Important tags

35, 11, 109, 60, 126

150 = 1, 2(partially filled or fully filled)

39

#### Search through ID

ID from ANY machine should suffice
