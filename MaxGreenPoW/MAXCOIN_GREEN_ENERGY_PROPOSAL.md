# MAXCOIN GREEN ENERGY PROPOSAL

## LICENSE

```
Copyright (C)  DECENTRALIZED CLIMATE FOUNDATION A.C.
Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3
or any later version published by the Free Software Foundation;
with no Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts.
A copy of the license is included in the section entitled "GNU
Free Documentation License". 
```

## DESCRIPTION

A system which can be a functional low scale proof of concept, by tracing real green energy data and representing it as tokens plus a proof of work that ensures the security and online operation consistency in an embedded hardware attached to a solar panel, we intend to achieve the original Satoshi vision (decentralization, and avoid double counting) and thus tokenize measured data with a small range of error.

The goal the following document is a proposal for building a proof of concept system with both hardware and software in a small scale model, this work is not only limited to that but also making some proposals that consider 3rd generation blockchains developments such as the blockchain trilemma , DAOs for management of trust and verification of Green Energy datasets based on embedded open hardware technology, Liquidity Pools and Energy pools (Batteries) with real parity between the real energy and tokens representations of energy.

Following a fully free software as following the main philosophies of the Maxcoin Project (Max Current) and Decentralized Climate Foundation, and finally taking the DECA 2.0 protocol and Maxcoin’s Blockchain as base technologies and other second layer and multichain solutions, as base technologies that will ensure the security and scalability of the system.

## MVP GOALS

## ELECTRICAL SCHEMATIC




## USE CASE

```plantuml
@startuml
title MAXCOIN GREEN ENERGY PROPOSAL USE CASES
left to right direction
skinparam packageStyle rect
skinparam actorStyle awesome

actor "MultiChain Miner" as MM
actor "Technical DAO" as TDAO

rectangle "DECA2 DAPP" as DAPP {
  usecase "Gets MultiChain Miner Request" as DAPP1
  usecase "Approves MultiChain Miner Request " as DAPP2
  usecase "Set MultiChain Miner SBT attributes" as DAPP3
  usecase "Gets mint request" as DAPP4
  usecase "Approved Minting (Baliot)" as DAPP5
  usecase "Gets Payment Request Order" as DAPP6
  usecase "Gets Energy Measurement Data" as DAPP7
  usecase "Verifies Payment Request Order" as DAPP8
  usecase  "Gets SBT" as DAPP9
  usecase "Verify all data and vote" as DAPP10
  usecase "Not Approved" as DAPP11 
  usecase "Votes module update" as DAPP12
  usecase "Mints DECA" as DAPP13
  usecase "Mints Energy Token" as DAPP14
  usecase "Fills Energy Request" as DAPP15

  rectangle DAO_Vaults{
    usecase "Stores DECA2" as DV1
  }
}
MM -right- DAPP1
TDAO -left- DAPP2 
MM -right- DAPP3
MM -right- DAPP4
MM -right- DAPP15
DAPP2 .down.> DAPP1 : <<extend>>
DAPP2 .down.> DAPP3 : <<include>>
DAPP3 .down.> DAPP9 : <<include>>
TDAO -left- DAPP4 
DAPP4 .down.> DAPP10: <<include>>
DAPP5 .down.> DAPP10: <<extend>>
DAPP11 .down.> DAPP10: <<extend>>
DAPP5 .down.> DAPP13: <<includes>>
DAPP5 .down.> DAPP14: <<includes>>
DAPP13 .down.> DV1: <<includes>>
MM -right- DAPP6
TDAO -left- DAPP8 


rectangle "DECA2 DistributedDB" as DOD {
  usecase "Stores Energy Measurements" as ODB1
  usecase "Updates Measured Sent Energy" as ODB2
  usecase "Stores Received Energy Data" as ODB3
  usecase "Updates Measured Received Energy" as ODB4
  usecase "Verifies Data Integrity Reltated to the Miner" as ODB5
  usecase "Provides Access to the DECA2 DAPP" as ODB6
  usecase ODB7 as "Stores SBT Miner Data:
  --
  Mining Pool Data
  Miner Hardware Status"
  usecase "Updates Mining Pool Data" as ODB8
  usecase "Updates Miner HW Status" as ODB9
}

note right of ODB3: Verify Transmision Loss

MM -right- ODB1
MM -right- ODB2
MM -right- ODB4
TDAO -left- ODB5
DAPP7 .> ODB6: <<include>>
DAPP3 .> ODB7: <<include>>
ODB8 .down.> ODB7: <<extend>>
ODB9 .down.> ODB7: <<extend>>
DAPP9 .up.> ODB7: <<includes>>

TDAO -left- DAPP12 
MM -right- DAPP12 
TDAO -left- DAPP10 

rectangle "Node Storage Energy" as NSE {
  usecase "Accepts Energy Storage Request" as NSE1
  usecase "Update Measured Received Energy" as NSE2
  usecase "Accepts Energy Take Out Request" as NSE3
  usecase "Transfers Energy to the Miner" as NSE4
}
MM -right- NSE1 
MM -right- NSE4
NSE3 .> NSE4: <<includes>>
NSE2 .> ODB3 : <<include>>
NSE3 .> DAPP15: <<extend>>

rectangle "MaxCoin Green Mining Pool" as MMP {
  usecase "Stores Online Miner Time + Hashes" as MMP1
  usecase "Uploads Data" as MMP2
  usecase "Logs in with SBT" as MMP3
}
MMP2 .> ODB7
MM -- MMP3
MMP3 .> MMP1: <<include>>

rectangle "Liquidity Pool" as LP{
  usecase "Gets % per transaction" as LP1
  usecase "Swaps Tokens" as LP2
  usecase "Provides liquidity DECA2/Energy Tokens" as LP3
  usecase "Gets Energy Tokens" as LP4
}

MM -right- LP1
MM -right- LP2
MM -right- LP4
DV1.> LP1 : <<include>>
DV1.> LP3 : <<include>>
DAPP14.> LP3 : <<include>>

note right of LP
  Paired with DECA2
  Paired with Carbon Token
end note
@enduml
```

## COMPONENT DIAGRAM

![Components Diagram](components_diagram.jpg)

## NETWORK DIAGRAM

![NETWORK Diagram](network_diagram.png)

## SEQUENCE DIAGRAM

```plantuml
@startuml
participant "Multichain Miner" as MM
participant "Technical DAO" as DAO
participant "MAXCoin Mining Pool" as MMP
participant "DECA2 DistributedDB" as DOD
participant "DECA2 DAPP" as DAPP
participant "Liquidity Pool" as LP
participant "Node Storage Energy" as NSE
MM->DAPP: Applies for getting a clean energy system (miner)
activate DAPP
DAO->DAPP: Verifies and Approves Applications
DAPP-->MM: Gets a SBT paired the clean energy system (miner)
deactivate DAPP
    loop Operation Process
        MM->MMP: Starts PoW the mining Process
        MM->DOD: Stores Energy Measurements 
        alt if "Sells Excess of Energy"
            MM->NSE: Start sending energy to the NSE
            MM->DOD: Updates Measured Sent Energy 
            NSE->DOD: Stores Received Energy (-looses)
            MM->DOD: Updates Measured Received Energy 
            MM->DAPP: Request Payment (Example Monthly)
            activate DAPP
            DAPP->DOD: Gets Measurments
            DAO->DAPP: Verifies Payment Request Order
            DAO->DOD: Verifies data integrity related to the Miner
            DAO->MMP: Verifies Continues MaxCoin Mining data
            DAO->MM: Verifies Miner Hardware Integrity
            DAO->DAPP: DAO Votes If Complies
            alt if "it gets approved"
                DAPP->LP: Mints DECA(EnergyToken to LP)
                DAPP->LP: Mints MaxCoin (Pay to LP)
                DAPP->DAO: Mints DECA/Maxcoin Payment to DAO
                deactivate DAPP
            else if "it does not"
                DAO-->DOD: Update Status
                DAO-->MM: Request Updates
            end
        else if "Buys Energy"
            '-------------------------TODO 
            LP-->MM: Gets Energy Tokens
            MM->DAPP: Sets Fill Energy Request
            NSE->DAPP: Accepts Requests
            NSE-->MM: Sends Energy to the Miner
        end
    end
    LP->LP: Generates % for\nthe Miner and the NSE
    alt if Miner Requests EnergyToken
        MM->LP: Request EnergyToken
        LP->MM: Gets EnergyToken
    else if Miner Requests Maxcoin
        MM->LP: Request Maxcoin
        LP->MM: Gets Maxcoin equivalent to EnergyToken owned
    end
@enduml
```

## GANTT DIAGRAM

## FUTURE ACHIEVABLES

## CONCLUSION

## REFERENCES

\[1\] Manisa Pipattanasomporn; Murat Kuzlu; Saifur Rahman, "A Blockchain-based Platform for Exchange of Solar Energy: Laboratory-scale Implementation", https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8635679, 2018.

\[2\] Joint Research Centre (JRC), "Energy system blockchain solutions", https://ses.jrc.ec.europa.eu/node/31977, October 2022.

\[3\] Anselma Wörner, Arne Meeuw, Liliane Ableitner, Felix Wortmann, Sandro Schopfer and
Verena Tiefenbeck, "Trading solar energy within the neighborhood: field implementation of a blockchain-based electricity market",https://energyinformatics.springeropen.com/track/pdf/10.1186/s42162-019-0092-0.pdf, September 2019.

\[4\] Merlinda Andoni, Valentin Robu, David Flynn, Simone Abram, Dale Geach, David Jenkins, Peter McCallum, Andrew Peacock, "Blockchain technology in the energy sector: A systematic review of challenges and opportunities", https://www.sciencedirect.com/science/article/pii/S1364032118307184?via%3Dihub,  February 2019.

\[5\] Subin Kwak, Joohyung Lee, Jangkyum Kim, and Hyeontaek Oh, "EggBlock: Design and Implementation of Solar Energy Generation and Trading Platform in Edge-Based IoT Systemswith Blockchain", https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8951093/pdf/sensors-22-02410.pdf, March 2022.

\[6\] Naiyu Wang, Xiao Zhou, Xin Lu, Zhitao Guan, "When Energy Trading meets Blockchain in ElectricalPower System: The State of the Art", https://arxiv.org/ftp/arxiv/papers/1902/1902.07233.pdf, 2018.

\[7\] Energypedia.info, "Blockchain Techologies For the Energy Access Sector", [https://energypedia.info/wiki/Blockchain\_Techologies\_For\_the\_Energy\_Access\_Sector#ImpactPPA](https://energypedia.info/wiki/Blockchain_Techologies_For_the_Energy_Access_Sector#ImpactPPA), 2022.


