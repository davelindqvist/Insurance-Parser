# Insurance Eligibility Parser

Parses 271 eligibility responses from Change Healthcare’s API and consolidates benefit information scattered across nodes into a structure a non-technical person can read, validated against the sandbox test cases below.

## Description

This is a project to parse and sort out insurance eligibility information returned by [Change Healthcare's Eligibility API](https://developers.changehealthcare.com/eligibilityandclaims/reference/medical-network-eligibility-v3-overview). Examples of them are under `src/eligibilities` with descriptions found [here](#from-change-healthcares-sandbox-api-values-and-test-responses).

What this solves:

1. Dispersion of the same benefit information and characteristic on different nodes.
2. Organization of insurance information that would make more sense to a person.
3. Takes away redundant properties
4. After parsing, it leaves a O(1) look up instead of O(M x N)

## Example 1 
000055 - (Too many deductible at service levels)

<details>

Input
```json
{
  "meta": {
    "senderId": "APIM_Marketplace_SBX_Native_A2vFYhTtiVkqdwsx",
    "applicationMode": "sandbox",
    "traceId": "eab6898a-8da8-fa17-9c02-418fff193323"
  },
  "controlNumber": "000000001",
  "reassociationKey": "000000001",
  "tradingPartnerServiceId": "000055",
  "provider": {
    "entityIdentifier": "Provider",
    "entityType": "Person",
    "serviceProviderNumber": "CHCELIGIBILITY001"
  },
  "subscriber": {
    "entityIdentifier": "Insured or Subscriber",
    "entityType": "Person",
    "groupNumber": "092001"
  },
  "subscriberTraceNumbers": [
    {
      "traceTypeCode": "1",
      "traceType": "Current Transaction Trace Numbers",
      "referenceIdentification": "000000001",
      "originatingCompanyIdentifier": "9EMDEON999"
    }
  ],
  "payer": {
    "entityIdentifier": "Payer",
    "entityType": "Non-Person Entity",
    "name": "Unknown",
    "payorIdentification": "000055"
  },
  "planInformation": {
    "groupNumber": "092001",
    "groupDescription": "Medicare Mc"
  },
  "planDateInformation": {
    "plan": "20160914",
    "planBegin": "20160801",
    "eligibilityEnd": "20991231"
  },
  "planStatus": [
    {
      "statusCode": "1",
      "status": "Active Coverage",
      "serviceTypeCodes": ["1", "48"]
    },
    {
      "statusCode": "1",
      "status": "Active Coverage",
      "planDetails": "See Schedule at WWW.TEST.COM",
      "serviceTypeCodes": ["35"]
    },
    {
      "statusCode": "1",
      "status": "Active Coverage",
      "planDetails": "Medicare Choice",
      "serviceTypeCodes": ["30"]
    }
  ],
  "benefitsInformation": [
    {
      "code": "1",
      "name": "Active Coverage",
      "serviceTypeCodes": ["30"],
      "serviceTypes": ["Health Benefit Plan Coverage"],
      "planCoverage": "Medicare Choice",
      "headerLoopIdentifierCode": "2120",
      "trailerLoopIdentifierCode": "2120",
      "benefitsRelatedEntity": {
        "entityIdentifier": "Primary Care Provider",
        "entityType": "Person",
        "entityName": "TEST",
        "entityFirstname": "TEST",
        "contactInformation": {
          "contacts": [
            {
              "communicationMode": "Telephone",
              "communicationNumber": "1111111111"
            }
          ]
        }
      },
      "benefitsRelatedEntities": [
        {
          "entityIdentifier": "Primary Care Provider",
          "entityType": "Person",
          "entityName": "TEST",
          "entityFirstname": "TEST",
          "contactInformation": {
            "contacts": [
              {
                "communicationMode": "Telephone",
                "communicationNumber": "1111111111"
              }
            ]
          }
        }
      ],
      "additionalInformation": [
        {
          "description": "Medicare Choice (MC) w/ Rx"
        },
        {
          "description": "HMO type plan for Medicare members. Selection of a PCP is required. Offered in Miami-Dade and Broward Counties. Offers access to AvMed Medicare Choice network. (MC)"
        }
      ]
    },
    {
      "code": "G",
      "name": "Out of Pocket (Stop Loss)",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["30"],
      "serviceTypes": ["Health Benefit Plan Coverage"],
      "planCoverage": "Out Of Pocket Max-In Network Preferred Individual",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "5000",
      "quantityQualifierCode": "99",
      "quantityQualifier": "Quantity Used",
      "benefitQuantity": "155",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "G",
      "name": "Out of Pocket (Stop Loss)",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["30"],
      "serviceTypes": ["Health Benefit Plan Coverage"],
      "planCoverage": "Out Of Pocket Max-In Network Preferred Individual",
      "timeQualifierCode": "29",
      "timeQualifier": "Remaining",
      "benefitAmount": "4845",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["98"],
      "serviceTypes": ["Professional (Physician) Visit - Office"],
      "planCoverage": "Primary Care Visit-In Network Preferred",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["98"],
      "serviceTypes": ["Professional (Physician) Visit - Office"],
      "planCoverage": "Primary Care Visit-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "No Out of Network 2nd opinions."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["98"],
      "serviceTypes": ["Professional (Physician) Visit - Office"],
      "planCoverage": "Specialist Visit-In Network Preferred",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "10",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["98"],
      "serviceTypes": ["Professional (Physician) Visit - Office"],
      "planCoverage": "Specialist Visit-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "HPN; No Out of Network 2nd opinions."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["88"],
      "serviceTypes": ["Pharmacy"],
      "planCoverage": "Generic Drugs-In Network Preferred",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["88"],
      "serviceTypes": ["Pharmacy"],
      "planCoverage": "Generic Drugs-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["88"],
      "serviceTypes": ["Pharmacy"],
      "planCoverage": "Preferred Brand Drugs-In Network Preferred",
      "benefitAmount": "35",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["88"],
      "serviceTypes": ["Pharmacy"],
      "planCoverage": "Preferred Brand Drugs-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["88"],
      "serviceTypes": ["Pharmacy"],
      "planCoverage": "Non Preferred Brand Drugs-In Network Preferred",
      "benefitAmount": "70",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["88"],
      "serviceTypes": ["Pharmacy"],
      "planCoverage": "Non Preferred Brand Drugs-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "A",
      "name": "Co-Insurance",
      "serviceTypeCodes": ["88"],
      "serviceTypes": ["Pharmacy"],
      "planCoverage": "Specialty Drugs-In Network Preferred",
      "benefitPercent": "33",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["88"],
      "serviceTypes": ["Pharmacy"],
      "planCoverage": "Specialty Drugs-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "coinsurance (retail)"
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["50"],
      "serviceTypes": ["Hospital - Outpatient"],
      "planCoverage": "Outpatient Facility Fee-In Network Preferred",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "75",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["50"],
      "serviceTypes": ["Hospital - Outpatient"],
      "planCoverage": "Outpatient Facility Fee-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "No additional copay for Cataract surgery."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["50"],
      "serviceTypes": ["Hospital - Outpatient"],
      "planCoverage": "Outpatient Facility Fee-In Network Preferred",
      "benefitAmount": "175",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["50"],
      "serviceTypes": ["Hospital - Outpatient"],
      "planCoverage": "Outpatient Facility Fee-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "for hospital affiliated OP facility.  No additional copay for Cataract surgery."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["86"],
      "serviceTypes": ["Emergency Services"],
      "planCoverage": "Emergency Room Services-In Network Preferred",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "75",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["86"],
      "serviceTypes": ["Emergency Services"],
      "planCoverage": "Emergency Room Services-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Copay waived if admitted within 24 hours."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["86"],
      "serviceTypes": ["Emergency Services"],
      "planCoverage": "Emergency Transportation-In Network Preferred",
      "benefitAmount": "100",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["86"],
      "serviceTypes": ["Emergency Services"],
      "planCoverage": "Emergency Transportation-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay per one way trip."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["UC"],
      "serviceTypes": ["Urgent Care"],
      "planCoverage": "Urgent Care-In Network Preferred",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "25",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["UC"],
      "serviceTypes": ["Urgent Care"],
      "planCoverage": "Urgent Care-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Copay waived if admitted within 24 hours."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["UC"],
      "serviceTypes": ["Urgent Care"],
      "planCoverage": "Urgent Care-Out Of Network",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "25",
      "inPlanNetworkIndicatorCode": "N",
      "inPlanNetworkIndicator": "No"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["UC"],
      "serviceTypes": ["Urgent Care"],
      "planCoverage": "Urgent Care-Out Of Network",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "N",
      "inPlanNetworkIndicator": "No",
      "additionalInformation": [
        {
          "description": "Copay waived if admitted within 24 hours."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["47"],
      "serviceTypes": ["Hospital"],
      "planCoverage": "Hospital Stay Facility Fee-In Network Preferred",
      "benefitAmount": "80",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["47"],
      "serviceTypes": ["Hospital"],
      "planCoverage": "Hospital Stay Facility Fee-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "$0 per day thereafter."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["47"],
      "serviceTypes": ["Hospital"],
      "planCoverage": "Hospital Stay Facility Fee-In Network Preferred",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["47"],
      "serviceTypes": ["Hospital"],
      "planCoverage": "Hospital Stay Facility Fee-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["47"],
      "serviceTypes": ["Hospital"],
      "planCoverage": "Hospital Stay Facility Fee-In Network Preferred",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["47"],
      "serviceTypes": ["Hospital"],
      "planCoverage": "Hospital Stay Facility Fee-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Copay $0, days 21-90 per day and thereafter."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["MH"],
      "serviceTypes": ["Mental Health"],
      "planCoverage": "Mental/Behavioral Health Outpatient Services",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "15",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["MH"],
      "serviceTypes": ["Mental Health"],
      "planCoverage": "Mental/Behavioral Health Outpatient Services",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "$15 per day for Partial Hosp Stay; Medicare limits apply."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["A7"],
      "serviceTypes": ["Psychiatric - Inpatient"],
      "planCoverage": "Mental/Behavioral Health Inpatient Services",
      "timeQualifierCode": "7",
      "timeQualifier": "Day",
      "benefitAmount": "150",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["A7"],
      "serviceTypes": ["Psychiatric - Inpatient"],
      "planCoverage": "Mental/Behavioral Health Inpatient Services",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Copay applied per day to day 1 through day 9 per admission;"
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["A7"],
      "serviceTypes": ["Psychiatric - Inpatient"],
      "planCoverage": "Mental/Behavioral Health Inpatient Services",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["A7"],
      "serviceTypes": ["Psychiatric - Inpatient"],
      "planCoverage": "Mental/Behavioral Health Inpatient Services",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay for day 10 through day 90."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["AI"],
      "serviceTypes": ["Substance Abuse"],
      "planCoverage": "Substance Use Disorder Outpatient Services",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "15",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["AI"],
      "serviceTypes": ["Substance Abuse"],
      "planCoverage": "Substance Use Disorder Outpatient Services",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "$15 per day for Partial Hosp Stay; Medicare limits apply."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["AI"],
      "serviceTypes": ["Substance Abuse"],
      "planCoverage": "Substance Use Disorder Inpatient Services",
      "benefitAmount": "80",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["AI"],
      "serviceTypes": ["Substance Abuse"],
      "planCoverage": "Substance Use Disorder Inpatient Services",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["AI"],
      "serviceTypes": ["Substance Abuse"],
      "planCoverage": "Substance Use Disorder Inpatient Services",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["AI"],
      "serviceTypes": ["Substance Abuse"],
      "planCoverage": "Substance Use Disorder Inpatient Services",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "for each admission"
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["AI"],
      "serviceTypes": ["Substance Abuse"],
      "planCoverage": "Substance Use Disorder Inpatient Services",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["AI"],
      "serviceTypes": ["Substance Abuse"],
      "planCoverage": "Substance Use Disorder Inpatient Services",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "per day thereafter days 21 - 90."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["42"],
      "serviceTypes": ["Home Health Care"],
      "planCoverage": "Home Health Care-In Network Preferred",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["42"],
      "serviceTypes": ["Home Health Care"],
      "planCoverage": "Home Health Care-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Medicare limits apply."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["A9"],
      "serviceTypes": ["Rehabilitation"],
      "planCoverage": "Rehabilitation Services-In Network Preferred",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "15",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["A9"],
      "serviceTypes": ["Rehabilitation"],
      "planCoverage": "Rehabilitation Services-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Medicare limits apply."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["AG"],
      "serviceTypes": ["Skilled Nursing Care"],
      "planCoverage": "Skilled Nursing Care-In Network Preferred",
      "benefitAmount": "135",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["AG"],
      "serviceTypes": ["Skilled Nursing Care"],
      "planCoverage": "Skilled Nursing Care-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "per day for days 21-100 per admission."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["AG"],
      "serviceTypes": ["Skilled Nursing Care"],
      "planCoverage": "Skilled Nursing Care-In Network Preferred",
      "timeQualifierCode": "7",
      "timeQualifier": "Day",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["AG"],
      "serviceTypes": ["Skilled Nursing Care"],
      "planCoverage": "Skilled Nursing Care-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "per admission."
        }
      ]
    },
    {
      "code": "A",
      "name": "Co-Insurance",
      "serviceTypeCodes": ["18"],
      "serviceTypes": ["Durable Medical Equipment Rental"],
      "planCoverage": "Durable Medical Equipment-In Network Preferred",
      "benefitPercent": "20",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["18"],
      "serviceTypes": ["Durable Medical Equipment Rental"],
      "planCoverage": "Durable Medical Equipment-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Medicare limits apply."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["45"],
      "serviceTypes": ["Hospice"],
      "planCoverage": "Hospice Service-In Network Preferred",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["45"],
      "serviceTypes": ["Hospice"],
      "planCoverage": "Hospice Service-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "You must get care from a Medicare certified hospice. Your plan will pay for a consultative visit before you select hospice."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["AL"],
      "serviceTypes": ["Vision (Optometry)"],
      "planCoverage": "Eye Exam-In Network Preferred",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "5",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["AL"],
      "serviceTypes": ["Vision (Optometry)"],
      "planCoverage": "Eye Exam-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay/visit"
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["AL"],
      "serviceTypes": ["Vision (Optometry)"],
      "planCoverage": "Vision Rider Eye Exam-In Network Preferred",
      "benefitAmount": "5",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["AL"],
      "serviceTypes": ["Vision (Optometry)"],
      "planCoverage": "Vision Rider Eye Exam-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Copay/visit"
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["CP"],
      "serviceTypes": ["Eyewear and Eyewear Accessories"],
      "planCoverage": "Vision Rider Glasses-In Network Preferred",
      "benefitAmount": "100",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["CP"],
      "serviceTypes": ["Eyewear and Eyewear Accessories"],
      "planCoverage": "Vision Rider Glasses-In Network Preferred",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "allowance for glasses or contacts per year"
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Complex",
      "benefitAmount": "75",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Complex",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay for non-hospital affiliate."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Radiation Therapy",
      "benefitAmount": "60",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Radiation Therapy",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Radiation Therapy $60 copay for hosp affiliate."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "benefitAmount": "5",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay for each Medicare-covered chiropractic visit."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "5",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay/visit for Hearing Evaluation."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "benefitAmount": "5",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay for each Medicare-covered podiatry visit."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "benefitAmount": "5",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Medicare limits apply."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Radiation Therapy",
      "benefitAmount": "35",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Radiation Therapy",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "Radiation Therapy $35 copay for non-hosp affiliate."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["98"],
      "serviceTypes": ["Professional (Physician) Visit - Office"],
      "planCoverage": "Specialist Visit-In Network ",
      "timeQualifierCode": "27",
      "timeQualifier": "Visit",
      "benefitAmount": "30",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["98"],
      "serviceTypes": ["Professional (Physician) Visit - Office"],
      "planCoverage": "Specialist Visit-In Network ",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "No Out of Network 2nd opinions."
        }
      ]
    },
    {
      "code": "A",
      "name": "Co-Insurance",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Pet Scans",
      "benefitPercent": "20",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Pet Scans",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "PET scans 20% coinsurance."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Complex",
      "benefitAmount": "175",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["62"],
      "serviceTypes": ["MRI/CAT Scan"],
      "planCoverage": "Imaging-In Network Preferred Complex",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay for hospital affiliate."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "benefitAmount": "10",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["33"],
      "serviceTypes": ["Chiropractic"],
      "planCoverage": "Other Practitioner Visit",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "copay per Allergy Skin Testing.  Medicare limits apply."
        }
      ]
    },
    {
      "code": "B",
      "name": "Co-Payment",
      "serviceTypeCodes": ["5"],
      "serviceTypes": ["Diagnostic Lab"],
      "planCoverage": "Diagnostic Tests-In Network Preferred Lab",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes"
    },
    {
      "code": "C",
      "name": "Deductible",
      "coverageLevelCode": "IND",
      "coverageLevel": "Individual",
      "serviceTypeCodes": ["5"],
      "serviceTypes": ["Diagnostic Lab"],
      "planCoverage": "Diagnostic Tests-In Network Preferred Lab",
      "timeQualifierCode": "23",
      "timeQualifier": "Calendar Year",
      "benefitAmount": "0",
      "inPlanNetworkIndicatorCode": "Y",
      "inPlanNetworkIndicator": "Yes",
      "additionalInformation": [
        {
          "description": "$0 copay for each Medicare covered lab test"
        }
      ]
    },
    {
      "code": "1",
      "name": "Active Coverage",
      "serviceTypeCodes": ["1"],
      "serviceTypes": ["Medical Care"]
    },
    {
      "code": "1",
      "name": "Active Coverage",
      "serviceTypeCodes": ["35"],
      "serviceTypes": ["Dental Care"],
      "planCoverage": "See Schedule at WWW.TEST.COM"
    },
    {
      "code": "1",
      "name": "Active Coverage",
      "serviceTypeCodes": ["48"],
      "serviceTypes": ["Hospital - Inpatient"],
      "additionalInformation": [
        {
          "description": "Medicare Advantage members who visit HPN designated specialty physicians will incur a $0 copayment. Members visiting non-HPN in-network specialty physicians will incur a $25 copayment."
        }
      ]
    }
  ],
  "x12": "ISA*00*          *01*SomePwd   *ZZ*EMDEON         *ZZ*TPG00000       *240220*0533*^*00501*000000001*0*T*:~GS*HB*MTEXE*LLX1210001*20131015*2219*000000001*X*005010X279A1~ST*271*127704768*005010X279A1~BHT*0022*11*eab6898a-8da8-fa17-9c02-418fff193323*20240220*0533~HL*1**20*1~NM1*PR*2*Unknown*****PI*000055~HL*2*1*21*1~NM1*1P*1******SV*CHCELIGIBILITY001~HL*3*2*22*0~TRN*1*000000001*9EMDEON999*~NM1*IL*1*******01234567890~REF*6P*092001*Medicare Mc~N3*~N4***~DMG*D8**~DTP*346*D8*20160801~DTP*357*D8*20991231~DTP*291*D8*20160914~EB*1**30**Medicare Choice~MSG*Medicare Choice (MC) w/ Rx~MSG*HMO type plan for Medicare members. Selection of a PCP is required. Offered in Miami-Dade and Broward Counties. Offers access to AvMed Medicare Choice network. (MC)~LS*2120~NM1*P3*1*TEST*TEST~PER*IC**TE*1111111111~LE*2120~EB*G*IND*30**Out Of Pocket Max-In Network Preferred Individual*23*5000**99*155**Y~EB*G*IND*30**Out Of Pocket Max-In Network Preferred Individual*29*4845*****Y~EB*B**98**Primary Care Visit-In Network Preferred*27*0*****Y~EB*C*IND*98**Primary Care Visit-In Network Preferred*23*0*****Y~MSG*No Out of Network 2nd opinions.~EB*B**98**Specialist Visit-In Network Preferred*27*10*****Y~EB*C*IND*98**Specialist Visit-In Network Preferred*23*0*****Y~MSG*HPN; No Out of Network 2nd opinions.~EB*B**88**Generic Drugs-In Network Preferred**0*****Y~EB*C*IND*88**Generic Drugs-In Network Preferred*23*0*****Y~EB*B**88**Preferred Brand Drugs-In Network Preferred**35*****Y~EB*C*IND*88**Preferred Brand Drugs-In Network Preferred*23*0*****Y~EB*B**88**Non Preferred Brand Drugs-In Network Preferred**70*****Y~EB*C*IND*88**Non Preferred Brand Drugs-In Network Preferred*23*0*****Y~EB*A**88**Specialty Drugs-In Network Preferred***33****Y~EB*C*IND*88**Specialty Drugs-In Network Preferred*23*0*****Y~MSG*coinsurance (retail)~EB*B**50**Outpatient Facility Fee-In Network Preferred*27*75*****Y~EB*C*IND*50**Outpatient Facility Fee-In Network Preferred*23*0*****Y~MSG*No additional copay for Cataract surgery.~EB*B**50**Outpatient Facility Fee-In Network Preferred**175*****Y~EB*C*IND*50**Outpatient Facility Fee-In Network Preferred*23*0*****Y~MSG*for hospital affiliated OP facility.  No additional copay for Cataract surgery.~EB*B**86**Emergency Room Services-In Network Preferred*27*75*****Y~EB*C*IND*86**Emergency Room Services-In Network Preferred*23*0*****Y~MSG*Copay waived if admitted within 24 hours.~EB*B**86**Emergency Transportation-In Network Preferred**100*****Y~EB*C*IND*86**Emergency Transportation-In Network Preferred*23*0*****Y~MSG*copay per one way trip.~EB*B**UC**Urgent Care-In Network Preferred*27*25*****Y~EB*C*IND*UC**Urgent Care-In Network Preferred*23*0*****Y~MSG*Copay waived if admitted within 24 hours.~EB*B**UC**Urgent Care-Out Of Network*27*25*****N~EB*C*IND*UC**Urgent Care-Out Of Network*23*0*****N~MSG*Copay waived if admitted within 24 hours.~EB*B**47**Hospital Stay Facility Fee-In Network Preferred**80*****Y~EB*C*IND*47**Hospital Stay Facility Fee-In Network Preferred*23*0*****Y~MSG*$0 per day thereafter.~EB*B**47**Hospital Stay Facility Fee-In Network Preferred**0*****Y~EB*C*IND*47**Hospital Stay Facility Fee-In Network Preferred*23*0*****Y~EB*B**47**Hospital Stay Facility Fee-In Network Preferred**0*****Y~EB*C*IND*47**Hospital Stay Facility Fee-In Network Preferred*23*0*****Y~MSG*Copay $0, days 21-90 per day and thereafter.~EB*B**MH**Mental/Behavioral Health Outpatient Services*27*15*****Y~EB*C*IND*MH**Mental/Behavioral Health Outpatient Services*23*0*****Y~MSG*$15 per day for Partial Hosp Stay; Medicare limits apply.~EB*B**A7**Mental/Behavioral Health Inpatient Services*7*150*****Y~EB*C*IND*A7**Mental/Behavioral Health Inpatient Services*23*0*****Y~MSG*Copay applied per day to day 1 through day 9 per admission;~EB*B**A7**Mental/Behavioral Health Inpatient Services**0*****Y~EB*C*IND*A7**Mental/Behavioral Health Inpatient Services*23*0*****Y~MSG*copay for day 10 through day 90.~EB*B**AI**Substance Use Disorder Outpatient Services*27*15*****Y~EB*C*IND*AI**Substance Use Disorder Outpatient Services*23*0*****Y~MSG*$15 per day for Partial Hosp Stay; Medicare limits apply.~EB*B**AI**Substance Use Disorder Inpatient Services**80*****Y~EB*C*IND*AI**Substance Use Disorder Inpatient Services*23*0*****Y~EB*B**AI**Substance Use Disorder Inpatient Services**0*****Y~EB*C*IND*AI**Substance Use Disorder Inpatient Services*23*0*****Y~MSG*for each admission~EB*B**AI**Substance Use Disorder Inpatient Services**0*****Y~EB*C*IND*AI**Substance Use Disorder Inpatient Services*23*0*****Y~MSG*per day thereafter days 21 - 90.~EB*B**42**Home Health Care-In Network Preferred**0*****Y~EB*C*IND*42**Home Health Care-In Network Preferred*23*0*****Y~MSG*Medicare limits apply.~EB*B**A9**Rehabilitation Services-In Network Preferred*27*15*****Y~EB*C*IND*A9**Rehabilitation Services-In Network Preferred*23*0*****Y~MSG*Medicare limits apply.~EB*B**AG**Skilled Nursing Care-In Network Preferred**135*****Y~EB*C*IND*AG**Skilled Nursing Care-In Network Preferred*23*0*****Y~MSG*per day for days 21-100 per admission.~EB*B**AG**Skilled Nursing Care-In Network Preferred*7*0*****Y~EB*C*IND*AG**Skilled Nursing Care-In Network Preferred*23*0*****Y~MSG*per admission.~EB*A**18**Durable Medical Equipment-In Network Preferred***20****Y~EB*C*IND*18**Durable Medical Equipment-In Network Preferred*23*0*****Y~MSG*Medicare limits apply.~EB*B**45**Hospice Service-In Network Preferred**0*****Y~EB*C*IND*45**Hospice Service-In Network Preferred*23*0*****Y~MSG*You must get care from a Medicare certified hospice. Your plan will pay for a consultative visit before you select hospice.~EB*B**AL**Eye Exam-In Network Preferred*27*5*****Y~EB*C*IND*AL**Eye Exam-In Network Preferred*23*0*****Y~MSG*copay/visit~EB*B**AL**Vision Rider Eye Exam-In Network Preferred**5*****Y~EB*C*IND*AL**Vision Rider Eye Exam-In Network Preferred*23*0*****Y~MSG*Copay/visit~EB*B**CP**Vision Rider Glasses-In Network Preferred**100*****Y~EB*C*IND*CP**Vision Rider Glasses-In Network Preferred*23*0*****Y~MSG*allowance for glasses or contacts per year~EB*B**62**Imaging-In Network Preferred Complex**75*****Y~EB*C*IND*62**Imaging-In Network Preferred Complex*23*0*****Y~MSG*copay for non-hospital affiliate.~EB*B**62**Imaging-In Network Preferred Radiation Therapy**60*****Y~EB*C*IND*62**Imaging-In Network Preferred Radiation Therapy*23*0*****Y~MSG*Radiation Therapy $60 copay for hosp affiliate.~EB*B**33**Other Practitioner Visit**5*****Y~EB*C*IND*33**Other Practitioner Visit*23*0*****Y~MSG*copay for each Medicare-covered chiropractic visit.~EB*B**33**Other Practitioner Visit*27*5*****Y~EB*C*IND*33**Other Practitioner Visit*23*0*****Y~MSG*copay/visit for Hearing Evaluation.~EB*B**33**Other Practitioner Visit**5*****Y~EB*C*IND*33**Other Practitioner Visit*23*0*****Y~MSG*copay for each Medicare-covered podiatry visit.~EB*B**33**Other Practitioner Visit**5*****Y~EB*C*IND*33**Other Practitioner Visit*23*0*****Y~MSG*Medicare limits apply.~EB*B**62**Imaging-In Network Preferred Radiation Therapy**35*****Y~EB*C*IND*62**Imaging-In Network Preferred Radiation Therapy*23*0*****Y~MSG*Radiation Therapy $35 copay for non-hosp affiliate.~EB*B**98**Specialist Visit-In Network *27*30*****Y~EB*C*IND*98**Specialist Visit-In Network *23*0*****Y~MSG*No Out of Network 2nd opinions.~EB*A**62**Imaging-In Network Preferred Pet Scans***20****Y~EB*C*IND*62**Imaging-In Network Preferred Pet Scans*23*0*****Y~MSG*PET scans 20% coinsurance.~EB*B**62**Imaging-In Network Preferred Complex**175*****Y~EB*C*IND*62**Imaging-In Network Preferred Complex*23*0*****Y~MSG*copay for hospital affiliate.~EB*B**33**Other Practitioner Visit**10*****Y~EB*C*IND*33**Other Practitioner Visit*23*0*****Y~MSG*copay per Allergy Skin Testing.  Medicare limits apply.~EB*B**5**Diagnostic Tests-In Network Preferred Lab**0*****Y~EB*C*IND*5**Diagnostic Tests-In Network Preferred Lab*23*0*****Y~MSG*$0 copay for each Medicare covered lab test~EB*1**1~EB*1**35**See Schedule at WWW.TEST.COM~EB*1**48~MSG*Medicare Advantage members who visit HPN designated specialty physicians will incur a $0 copayment. Members visiting non-HPN in-network specialty physicians will incur a $25 copayment.~SE*154*127704768~GE*1*000000001~IEA*1*000000001~"
}
```

Result
```json
(a) - UNPARSED LEFTOVER
  []
  LENGTH:  0

(b) - BENEFIT SUMMARY
  {
    'Not Applicable': {},
    'In-Network': {
      Individual: {
        'Health Benefit Plan Coverage': {
          'Out of Pocket (Stop Loss)': {
            planCoverage: 'Out Of Pocket Max-In Network Preferred Individual',
            'Calendar Year': { 'Quantity Used': '155' },
            Remaining: '4845'
          }
        },
        'Professional (Physician) Visit - Office': {
          Deductible: {
            planCoverage: 'Specialist Visit-In Network ',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'No Out of Network 2nd opinions.' } ]
            }
          }
        },
        Pharmacy: {
          Deductible: {
            planCoverage: 'Specialty Drugs-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'coinsurance (retail)' } ]
            }
          }
        },
        'Hospital - Outpatient': {
          Deductible: {
            planCoverage: 'Outpatient Facility Fee-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: 'for hospital affiliated OP facility.  No additional copay for Cataract surgery.'
                }
              ]
            }
          }
        },
        'Emergency Services': {
          Deductible: {
            planCoverage: 'Emergency Transportation-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'copay per one way trip.' } ]
            }
          }
        },
        'Urgent Care': {
          Deductible: {
            planCoverage: 'Urgent Care-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: 'Copay waived if admitted within 24 hours.'
                }
              ]
            }
          }
        },
        Hospital: {
          Deductible: {
            planCoverage: 'Hospital Stay Facility Fee-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: 'Copay $0, days 21-90 per day and thereafter.'
                }
              ]
            }
          }
        },
        'Mental Health': {
          Deductible: {
            planCoverage: 'Mental/Behavioral Health Outpatient Services',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: '$15 per day for Partial Hosp Stay; Medicare limits apply.'
                }
              ]
            }
          }
        },
        'Psychiatric - Inpatient': {
          Deductible: {
            planCoverage: 'Mental/Behavioral Health Inpatient Services',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'copay for day 10 through day 90.' } ]
            }
          }
        },
        'Substance Abuse': {
          Deductible: {
            planCoverage: 'Substance Use Disorder Inpatient Services',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'per day thereafter days 21 - 90.' } ]
            }
          }
        },
        'Home Health Care': {
          Deductible: {
            planCoverage: 'Home Health Care-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'Medicare limits apply.' } ]
            }
          }
        },
        Rehabilitation: {
          Deductible: {
            planCoverage: 'Rehabilitation Services-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'Medicare limits apply.' } ]
            }
          }
        },
        'Skilled Nursing Care': {
          Deductible: {
            planCoverage: 'Skilled Nursing Care-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'per admission.' } ]
            }
          }
        },
        'Durable Medical Equipment Rental': {
          Deductible: {
            planCoverage: 'Durable Medical Equipment-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'Medicare limits apply.' } ]
            }
          }
        },
        Hospice: {
          Deductible: {
            planCoverage: 'Hospice Service-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: 'You must get care from a Medicare certified hospice. Your plan will pay for a consultative visit before you select hospice.'
                }
              ]
            }
          }
        },
        'Vision (Optometry)': {
          Deductible: {
            planCoverage: 'Vision Rider Eye Exam-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'Copay/visit' } ]
            }
          }
        },
        'Eyewear and Eyewear Accessories': {
          Deductible: {
            planCoverage: 'Vision Rider Glasses-In Network Preferred',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: 'allowance for glasses or contacts per year'
                }
              ]
            }
          }
        },
        'MRI/CAT Scan': {
          Deductible: {
            planCoverage: 'Imaging-In Network Preferred Complex',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [ { description: 'copay for hospital affiliate.' } ]
            }
          }
        },
        Chiropractic: {
          Deductible: {
            planCoverage: 'Other Practitioner Visit',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: 'copay per Allergy Skin Testing.  Medicare limits apply.'
                }
              ]
            }
          }
        },
        'Diagnostic Lab': {
          Deductible: {
            planCoverage: 'Diagnostic Tests-In Network Preferred Lab',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: '$0 copay for each Medicare covered lab test'
                }
              ]
            }
          }
        }
      },
      Other: {
        'Professional (Physician) Visit - Office': {
          'Co-Payment': { planCoverage: 'Specialist Visit-In Network ', Visit: '30' }
        },
        Pharmacy: {
          'Co-Payment': {
            planCoverage: 'Non Preferred Brand Drugs-In Network Preferred',
            benefitAmount: '70'
          },
          'Co-Insurance': {
            planCoverage: 'Specialty Drugs-In Network Preferred',
            benefitPercent: '33'
          }
        },
        'Hospital - Outpatient': {
          'Co-Payment': {
            planCoverage: 'Outpatient Facility Fee-In Network Preferred',
            Visit: '75',
            benefitAmount: '175'
          }
        },
        'Emergency Services': {
          'Co-Payment': {
            planCoverage: 'Emergency Transportation-In Network Preferred',
            Visit: '75',
            benefitAmount: '100'
          }
        },
        'Urgent Care': {
          'Co-Payment': {
            planCoverage: 'Urgent Care-In Network Preferred',
            Visit: '25'
          }
        },
        Hospital: {
          'Co-Payment': {
            planCoverage: 'Hospital Stay Facility Fee-In Network Preferred',
            benefitAmount: '0'
          }
        },
        'Mental Health': {
          'Co-Payment': {
            planCoverage: 'Mental/Behavioral Health Outpatient Services',
            Visit: '15'
          }
        },
        'Psychiatric - Inpatient': {
          'Co-Payment': {
            planCoverage: 'Mental/Behavioral Health Inpatient Services',
            Day: '150',
            benefitAmount: '0'
          }
        },
        'Substance Abuse': {
          'Co-Payment': {
            planCoverage: 'Substance Use Disorder Inpatient Services',
            Visit: '15',
            benefitAmount: '0'
          }
        },
        'Home Health Care': {
          'Co-Payment': {
            planCoverage: 'Home Health Care-In Network Preferred',
            benefitAmount: '0'
          }
        },
        Rehabilitation: {
          'Co-Payment': {
            planCoverage: 'Rehabilitation Services-In Network Preferred',
            Visit: '15'
          }
        },
        'Skilled Nursing Care': {
          'Co-Payment': {
            planCoverage: 'Skilled Nursing Care-In Network Preferred',
            benefitAmount: '135',
            Day: '0'
          }
        },
        'Durable Medical Equipment Rental': {
          'Co-Insurance': {
            planCoverage: 'Durable Medical Equipment-In Network Preferred',
            benefitPercent: '20'
          }
        },
        Hospice: {
          'Co-Payment': {
            planCoverage: 'Hospice Service-In Network Preferred',
            benefitAmount: '0'
          }
        },
        'Vision (Optometry)': {
          'Co-Payment': {
            planCoverage: 'Vision Rider Eye Exam-In Network Preferred',
            Visit: '5',
            benefitAmount: '5'
          }
        },
        'Eyewear and Eyewear Accessories': {
          'Co-Payment': {
            planCoverage: 'Vision Rider Glasses-In Network Preferred',
            benefitAmount: '100'
          }
        },
        'MRI/CAT Scan': {
          'Co-Payment': {
            planCoverage: 'Imaging-In Network Preferred Complex',
            benefitAmount: '175'
          },
          'Co-Insurance': {
            planCoverage: 'Imaging-In Network Preferred Pet Scans',
            benefitPercent: '20'
          }
        },
        Chiropractic: {
          'Co-Payment': {
            planCoverage: 'Other Practitioner Visit',
            benefitAmount: '10',
            Visit: '5'
          }
        },
        'Diagnostic Lab': {
          'Co-Payment': {
            planCoverage: 'Diagnostic Tests-In Network Preferred Lab',
            benefitAmount: '0'
          }
        }
      }
    },
    'Out-of-Network': {
      Other: {
        'Urgent Care': {
          'Co-Payment': { planCoverage: 'Urgent Care-Out Of Network', Visit: '25' }
        }
      },
      Individual: {
        'Urgent Care': {
          Deductible: {
            planCoverage: 'Urgent Care-Out Of Network',
            'Calendar Year': {
              benefitAmount: '0',
              additionalInformation: [
                {
                  description: 'Copay waived if admitted within 24 hours.'
                }
              ]
            }
          }
        }
      }
    },
    'Other Payors': {},
    Unknown: {
      Other: {
        'Health Benefit Plan Coverage': {
          'Active Coverage': {
            planCoverage: 'Medicare Choice',
            benefitsRelatedEntity: {
              entityIdentifier: 'Primary Care Provider',
              entityType: 'Person',
              entityName: 'TEST',
              entityFirstname: 'TEST',
              contactInformation: {
                contacts: [
                  {
                    communicationMode: 'Telephone',
                    communicationNumber: '1111111111'
                  }
                ]
              }
            },
            benefitsRelatedEntities: [
              {
                entityIdentifier: 'Primary Care Provider',
                entityType: 'Person',
                entityName: 'TEST',
                entityFirstname: 'TEST',
                contactInformation: {
                  contacts: [
                    {
                      communicationMode: 'Telephone',
                      communicationNumber: '1111111111'
                    }
                  ]
                }
              }
            ]
          }
        },
        'Medical Care': { 'Active Coverage': {} },
        'Dental Care': {
          'Active Coverage': { planCoverage: 'See Schedule at WWW.TEST.COM' }
        },
        'Hospital - Inpatient': { 'Active Coverage': {} }
      }
    }
  }
```
</details>

## Example 2
00006 - Payer system error with an AAA segment

<details>

Input
```json
{
  "meta": {
    "senderId": "APIM_Marketplace_SBX_Native_A2vFYhTtiVkqdwsx",
    "applicationMode": "sandbox",
    "traceId": "2b76816e-89ff-f80b-fc0c-764ee69cad4a"
  },
  "controlNumber": "000000001",
  "reassociationKey": "000000001",
  "tradingPartnerServiceId": "00006",
  "payer": {
    "entityIdentifier": "Payer",
    "entityType": "Non-Person Entity",
    "name": "Unknown",
    "payorIdentification": "00006"
  },
  "errors": [
    {
      "field": "AAA",
      "code": "42",
      "description": "Unable to Respond at Current Time",
      "followupAction": "Resubmission Allowed",
      "location": "Loop 2000A",
      "possibleResolutions": "Is what we call the catch-all error.First place to start is look at PIRs to see if payer is having issues processing. If no PIR for payer call Command Center and have them check the system see if they are seeing trouble with the payer.Follow standard troubleshooting flow if no issue happening with payer."
    }
  ],
  "x12": "ISA*00*          *01*SomePwd   *ZZ*EMDEON         *ZZ*TPG00000       *240220*0522*^*00501*000000001*0*T*:~GS*HB*MTEXE*LLX1210001*20131015*2219*000000001*X*005010X279A1~ST*271*0001*005010X279A1~BHT*0022*11*2b76816e-89ff-f80b-fc0c-764ee69cad4a*20240220*0522~HL*1**20*1~AAA*Y**42*R~NM1*PR*2*Unknown*****PI*00006~SE*6*0001~GE*1*000000001~IEA*1*000000001~"
}
```

Result
```json
[
  {
    field: 'AAA',
    code: '42',
    description: 'Unable to Respond at Current Time',
    followupAction: 'Resubmission Allowed',
    location: 'Loop 2000A',
    possibleResolutions: 'Is what we call the catch-all error.First place to start is look at PIRs to see if payer is having issues processing. If no PIR for payer call Command Center and have them check the system see if they are seeing trouble with the payer.Follow standard troubleshooting flow if no issuehappening with payer.'
  }
]
```
</details>

## Installation

- NOTE: Requires node installed

1. Clone this repo.
2. `cd` into `Insurance-Parser` and `npm install`.
3. Change ID of eligibility on line 1 of `src/app.ts` to desired Change Healthcare Eligibility example, save, then run `npm run test` in terminal for results.
4. There should be two console log outputs for review: `(a) - UNPARSED LEFTOVER` and `(b) - BENEFIT SUMMARY`

   a. `(a) - UNPARSED LEFTOVER` - An internal validating tool. While the function runs, it assures if the properties were looked and parsed from the Change Healthcare JSON. Any objects leftover is a sign that there are gaps within the `(b) - BENEFIT SUMMARY` and should update the function to include what's in the unparsed leftover.

   b. `(b) - BENEFIT SUMMARY` - The data model is outputted on the basis of this order: `Benefit summary` -> `In-Network / Out-of-Network` -> `Coverage Level` -> `Service Type` -> `Eligibility or Benefit Information`.

## From Change Healthcare's "[Sandbox API Values and Test Responses](https://developers.changehealthcare.com/eligibilityandclaims/docs/eligibility-sandbox-api-values-and-test-responses)"

| ID     | Description                                                                                                                                                                           |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 00001  | This is a canned response that returns a single coverage plan. This will work for any payer.                                                                                          |
| 000002 | This is a canned response that returns a badly formatted 271. This will work for any payer.                                                                                           |
| 00003  | This is a canned response that returns a good 271 that contains maxed MSG01 field (AN..264) and EB03 repeating data element (99 repeats). This will work for any payer.               |
| 00004  | This is a canned response that returns AAA Not Eligible For Inquiries. This will work for any payer.                                                                                  |
| 00005  | This is a canned response that contains non-printable characters which we need to make sure we can parse. This will work for any payer.                                               |
| 00006  | A system error from the Payer.&nbsp; AAA segment in the 2000A Information Source Loop with AAA01 = Y, AAA03 = 42 and AAA04 = R.                                                       |
| 00007  | This is a canned response that returns a single coverage plan with unused fields. This will work for any payer.                                                                       |
| 000008 | This is a canned response that returns a 271 that contains data in deprecated fields. It is used to test our parser's handling of data in these fields. This will work for any payer. |
| 00009  | This returns a canned 271 response where the patient is a dependent.                                                                                                                  |
| 00036  | Florida Medicaid: Sample response.                                                                                                                                                    |
| 000039 | Different Deductible at 30 and other STC.                                                                                                                                             |
| 000040 | EB 1 with no benefits (Active Coverage) and EB I (Non Covered).                                                                                                                       |
| 000041 | Test EB 6 (Inactive) and EB W (Other Source of Data).                                                                                                                                 |
| 000042 | EB R — other additional payer. Sample use case for standard Medicaid benefit.                                                                                                         |
| 000043 | EB W response (other source of Data).                                                                                                                                                 |
| 000044 | High Out-of-Pocket Remaining but no deductible in response.                                                                                                                           |
| 000045 | Low Deductible High Premium — many different copayments.                                                                                                                              |
| 000046 | Low Deductible High Premium with no coinsurance.                                                                                                                                      |
| 000047 | Low Deductible High Premium.                                                                                                                                                          |
| 000048 | Low Deductible with no copayment.                                                                                                                                                     |
| 000049 | Multiple Services.                                                                                                                                                                    |
| 000050 | No Deductible, High Out-of-Pocket Remaining, only co-pay and co-insurance.                                                                                                            |
| 000051 | No Deductible, only copay for 33,98, UC.                                                                                                                                              |
| 000052 | No Deductible, No copayment.                                                                                                                                                          |
| 000053 | Plan with active coverage but no patient responsibility.                                                                                                                              |
| 000054 | Response with AD time period 25.                                                                                                                                                      |
| 000055 | Too many deductibles at service levels.                                                                                                                                               |
| 000056 | Uniquely formatted EB segment.                                                                                                                                                        |
| 000067 | Sample use case from PAA.                                                                                                                                                             |
| 000068 | Sample use case 2 from PAA.                                                                                                                                                           |
| 000069 | Sample Use case for EB V (Cannot Process).                                                                                                                                            |
| 000070 | Sample Use case for EB U (contact Following Entity for Eligibility or Benefit Information).                                                                                           |
| 000074 | Sample Use case for Connecticut Medicaid.                                                                                                                                             |
| 000081 | Sample response for WellCare.                                                                                                                                                         |
| 000082 | Sample response for Blue Cross Blue Shield Georgia.                                                                                                                                   |
| 000083 | Sample response for Humana.                                                                                                                                                           |
| ABHFL  | Sample use case for Aetna Better Health of Florida.                                                                                                                                   |
| ABHKY  | Sample use case for Aetna Better Health of Kentucky.                                                                                                                                  |
| ABHLA  | Sample use case for Aetna Better Health of Louisiana.                                                                                                                                 |
| ABHMO  | Sample use case for Aetna Better Health of Missouri.                                                                                                                                  |
| AETNX  | Sample response for AETNA.                                                                                                                                                            |
| BCCTC  | Sample use case for Blue Cross Blue Shield Connecticut.                                                                                                                               |
| BCNJC  | Sample response for BCBS of New Jersey (Horizon).                                                                                                                                     |
| CIGNA  | A sample response for CIGNA for dependent.                                                                                                                                            |
| CNTCR  | Sample use case for Connecticare Inc.                                                                                                                                                 |
| COVON  | Sample response for Coventry.                                                                                                                                                         |
| CT     | Sample use case for Connecticut Medicaid.                                                                                                                                             |
| DENTAL | Sample response for benefits from Dental Payer.                                                                                                                                       |
| HUM    | Sample response for Humana.                                                                                                                                                           |
| ILMSA  | Sample use case for Aetna Better Health of Illinois.                                                                                                                                  |
| ISCAM  | Sample response for Medi-CAL Portal connection.                                                                                                                                       |
| MA/MB  | Sample response for Medicare Part A/Medicare Part B.                                                                                                                                  |
| MEDX   | Sample response for MEDEX.                                                                                                                                                            |
| MMSI   | Sample use case for Mayo.                                                                                                                                                             |
| TRICE  | Sample response for Tricare.                                                                                                                                                          |
| TX     | Sample response for Texas Medicaid.                                                                                                                                                   |
| TXBCBS | Sample response for Blue Advantage HMO.                                                                                                                                               |
| UHC    | Sample response for United Healthcare.                                                                                                                                                |

### Notes

- `trailerLoopIdentifierCode` and `headerLoopIdentifierCode` is noise since there are two 2120 ([C - Subscriber Benefit Related Entity Name Loop](https://www.stedi.com/app/guides/view/hipaa/health-care-eligibility-benefit-response-x279a1/01GS66YHZPB37ABF34DBPSR213#properties.detail.properties.information_source_level_HL_loop.items.properties.information_receiver_level_HL_loop.items.properties.subscriber_level_HL_loop.items.properties.subscriber_name_NM1_loop.properties.subscriber_eligibility_or_benefit_information_EB_loop.items.properties.subscriber_benefit_related_entity_name_NM1_loop) / [D - Dependent Benefit Related Entity Name Loop](https://www.stedi.com/app/guides/view/hipaa/health-care-eligibility-benefit-response-x279a1/01GS66YHZPB37ABF34DBPSR213#properties.detail.properties.information_source_level_HL_loop.items.properties.information_receiver_level_HL_loop.items.properties.subscriber_level_HL_loop.items.properties.dependent_level_HL_loop.items.properties.dependent_name_NM1_loop.properties.dependent_eligibility_or_benefit_information_EB_loop.items.properties.dependent_benefit_related_entity_name_NM1_loop)), but Change Healthcare's API doesn't specify which 2120 loop it is.

## Reflection

**What I've learned:**

1.  Even with the unparsed leftover as an internal validator, it's not perfect. I've come across many edge cases that come from looking manually at the results. At the least, it's another method to help me find the gaps.
2.  Recently read a post about 10 software engineering golden rules, and the one I resonated with most regarding this project is this: `"There are no universal solutions. Evaluate tradeoffs for each context".`
3.  Going off from (2), there are so many variations of what a benefit input looks like. That's also why within the function, it appears to have redundant if/else statements when in actuality, they all serve a purpose due to the edge cases.
4.  As a former healthcare worker, I had to use my experience working with this project to implement the nuances as it's meant to be while making it simpler for the end user to understand what's being outputted.


**Can improve with:**

1.  Zod schema for integrity of consistent data type and formatting
2.  Incorporate database
3.  Consistent capitalization of keys
4.  Testing framework such as Jest
5.  DRY'ing within the parsing function
6.  Further type-checking
7.  Incorporating Docker
8.  Frontend component


#

#### Authored by: Dave Lindqvist
