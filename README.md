# Design Exercise Resources
[Design Exercise Resources]: #design-exercise-resources

This repository contains resources related to the **Providing Medicare Claims Data to Prescription Drug Plan Sponsors (PDPs)** request for proposal's (RFP) design exercise. It should be utilized in concert with the design exercise details contained in that RFP. If any instructions or information in this and the RFP conflict, the details here supercede those in the RFP.

## Table of Contents
[Table of Contents]: #table-of-contents

* [Opening](#design-exercise-resources)
* [Details and Instructions: Updates]
* [Differences from Real World]
* [Dictionary of Terms]
* [Input Data Files]
    * [Access Management System Authentication Data]
    * [Access Management System Authorization Data]
    * [Claims Data System Beneficiary Data]
    * [Claims Data System Coverage Data]
    * [Claims Data System Claims Data]

## Details and Instructions: Updates
[Details and Instructions: Updates]: #details-and-instructions

Please observe the following updates to the Design Exercise's details and instructions:

1. Additional data files are required for the Design Exercise, and have been included here:
    * [Claims Data System Beneficiary Data]
    * [Claims Data System Coverage Data]

## Differences from Real World
[Differences from Real World]: #differences-from-real-world

It's worth reiterating that this design exercise is simplified from the real-world problems that a successful Quoter will be solving. This simplification is primarily to limit the Deisgn Exercise's scope, such that it's reasonable for the amount of time available. In addition, though, CMS does not yet itself have a full understanding of the problems to be solved. One of the most urgent tasks facing a successful Quoter will be research to fully detail the problems that are to be solved.

Some known differences between this Design Exercise and the "real world" problems to be solved include (but are not limited to):

* The input data and associated systems in the real world are likely far more "messy" and unreliable.
* The real-world analogue to the Design Exercise's Access Management System is CMS' Health Plan Management System (HPMS).
    * The AMS is presented here as providing simple authentication and authorization data, but any interface with HPMS in reality will likely look very different.
    * Will HPMS need to be extended to support the proposed solutions?
    * Is the HPMS technology and team amenable to such extensions?
* The real-world solution will need to comply with all relevant laws and regulations.
    * Including the “Data Extract Content” section of the relevant Final Rule: <https://www.federalregister.gov/d/2019-06822/p-727>.

These, and many other, questions and differences will need to be addressed as part of the real-world project.

## Dictionary of Terms
[Dictionary of Terms]: #dictionary

* **User**: A user of your system, which in a more real-world scenario would be a PDP Plan representative, or an authorized vendor or consultant of theirs.
* **Plan**: A health plan that beneficiaries are members/subscribers of and that users are representatives of.
* **Beneficiary**: A health plan member, whose claims data will be shared with plans.
* **Claim**: A record of services received by a beneficiary.

## Input Data Files
[Input Data Files]: #input-data-files

### Access Management System Authentication Data: `input-data-ams-authn.csv`
[Access Management System Authentication Data]: #input-data-ams-authn

The `input-data-ams-authn.csv` file is a Comma-Separated Value (CSV) file containing authentication data from the (hypothetical) Access Management System. It contains one row for every user, with the following columns:

1. `ID`: The unique ID of this user.
2. `PASSWORD`: The password that can be used to authenticate this user.
3. `NAME`: The user's display name.
4. `EMAIL`: The user's email address.
5. `EXPIRES`: The timestamp at which this user's login ceases to be valid.

### Access Management System Authorization Data: `input-data-ams-authz.csv`
[Access Management System Authorization Data]: #input-data-ams-authz

The `input-data-ams-authz.csv` file is a Comma-Separated Value (CSV) file containing authorization data from the (hypothetical) Access Management System. Each row contains data for a single beneficiary, with the following columns:

1. `ID`: The authentication user ID authorized for the specified `CONTRACT_ID`.
2. `CONTRACT_ID`: The ID of a plan that the user is authorized for.
3. `ATTEST_DATE`: The timestamp for when this authorization record was created.

### Claims Data System Beneficiary Data: `input-data-cds-benes.ndjson`
[Claims Data System Beneficiary Data]: #input-data-cds-benes

The `input-data-cds-benes.ndjson` file is a Newline delimited JSON (NDJSON) file containing FHIR `Patient` resources from the (hypothetical) Claims Data System. Each record in this file is a `Patient` resource containing many fields, including the following (identified by JsonPath specifiers):

1. `$.id`: A unique ID for the beneficiary represented by this resource.
2. `$.identifier[?(@.system =~ /https:\/\/bluebutton.cms.gov\/resources\/identifier\/hicn/)].value`: The HICN(s) that CMS uses to identify this beneficiary.

### Claims Data System Coverage Data: `input-data-cds-coverage.ndjson`
[Claims Data System Coverage Data]: #input-data-cds-coverage

The `input-data-cds-coverage.ndjson` file is a Newline delimited JSON (NDJSON) file containing FHIR `Coverage` resources from the (hypothetical) Claims Data System. Each record in this file is a `Coverage` resource containing many fields, including the following (identified by JsonPath specifiers):

1. `$.beneficiary.reference`: A value of the form, `Patient/<patient_id>` where `patient_id` corresponds to `Patient.id` values in the [Claims Data Beneficiary Data] file, representing the beneficiary that this coverage record is for.
2. `$.grouping.subPlan`: Identifies whether this `Coverage` resource provides details on Part A, B, C, or D coverage.
3. `$.extension[?(@.url =~ /https:\/\/bluebutton.cms.gov\/resources\/variables\/ptdcntrct/)].valueCoding.code`: The `CONTRACT_ID` that the beneficiary represented by this resource is currently enrolled in. Only present for Part D.

### Claims Data System Claims Data: `input-data-cds-claims.ndjson`
[Claims Data System Claims Data]: #input-data-cds-claims

The `input-data-cds-claims.ndjson` file is a Newline delimited JSON (NDJSON) file containing FHIR `ExplanationOfBenefit` resources from the (hypothetical) Claims Data System. Each record in this file is an `ExplanationOfBenefit` resource containing many fields, including the following (identified by JsonPath specifiers):

1. `$.patient.reference`: A value of the form, `Patient/<patient_id>` where `patient_id` corresponds to `Patient.id` values in the [Claims Data Beneficiary Data] file, representing the beneficiary that this claim is for.
