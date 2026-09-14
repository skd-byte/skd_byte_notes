## ARM Architecture Introduction

## What is cpu architecture? 
- contracty bw hw and sw
- it defines what cpu needs to do.
- how it does it, is deinfed by **microarchitecture**
- architecture means functional specification
- Ex A53 and A72 has same architecture, ARMv8-A but different microarchitecture
- Microarchitecture Includes:
    - pipeline length
    - number and sizes of caches
    - cycle count of inidividual instruction
    - which optional feature implemented


## ARM Brand Name
- Arm cortex, Neoverse, SecureCore

## Architecture Specification
- Base Standards
    - Hardware Specification 
    - Firmeare Specification
        - Architecture Reference Manual (ARM)

- Arm ARM speciﬁcations deﬁning standardized system components and their interfaces.
    - standard system components supporting the procesosr core GIC, SMMU core sight and debug trace
    - power management: a specialized subsystem featuring an embedded System Control Processor (SCP) for controlling and monitoring power across the SoC. This interfaces to the main system software using the System Control and Management Interface (SCMI).
    - AMBA: access to memory and memory mapped peripherals


- ARM Firmware interface specification
    - SMCCC, FF-A, CCA
    - PSCi
    - ACPI

- Base Root Requirement/Base system Architecture

- ARM configuration and integration Manual
- Arm software Optimization Guide



## Armv9-A is the latest version of the Arm Architecture for A-proﬁle. Armv9-A builds on Armv8-A and adds new features, including:

- Scalable Vector Extension, version 2 (SVE2)

- Scalable Matrix Extensions (SME, SME2)

-  Realm Management Extension (RME)

- Guarded Control Stack (GCS)

- Branch Record Buﬀer Extension (BRBE)

- Embedded Trace Extension (ETE)

- Trace Buﬀer Extension (TRBE)