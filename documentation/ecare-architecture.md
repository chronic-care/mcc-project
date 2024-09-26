# About the MCC eCare Applications
The eCarePlan patient/caregiver- and clinician-facing applications were developed as part of a broader Electronic Care (eCare) Planning for People with Multiple Chronic Conditions (MCC) project sponsored by the [National Institute of Diabetes and Digestive and Kidney Diseases](https://www.niddk.nih.gov/) (NIDDK) and the [Agency for Healthcare Research and Quality](https://www.ahrq.gov/) (AHRQ). The project also received funding from the Office of the Secretary Patient-Centered Outcomes Research Trust Fund (OS-PCORTF). A multidisciplinary team led by EMI Advisors spearheaded the application development process.

This code was developed from 2020-2024 by [EMI Advisors](https://www.emiadvisors.net/) and is undergoing pilot testing with patients and caregivers through January 2025 at [Oregon Health and Sciences University](https://www.ohsu.edu/), led and evaluated by [RTI International](https://www.rti.org/). To learn more about the e-Care Plan Project for MCC, visit: https://ecareplan.ahrq.gov/.

## Developer Links
The eCare apps consist of two web-based SMART on FHIR applications, along with a data services library and Supplemental Data Store (SDS) server. The four open-source GitHub repositories can be found here: 

- MyCarePlanner README: https://github.com/chronic-care/mycareplanner/blob/main/README.md 
- eCarePlanner README: https://github.com/chronic-care/eCarePlanner/blob/develop/README.md 
- Common Data Services README: https://github.com/chronic-care/e-care-common-data-services/blob/main/README.md 
- Supplemental Data Store (SDS) README: https://github.com/OHSUCMP/ecp-sds-hardfork/blob/main/README.md 

## System Architecture
The relationship and technical workflow between a patient/caregiver-facing application and a clinician application involve several key components and data pathways. The system supports authentication processes and allows patient/caregiver data to be saved into a third-party supplemental data store (SDS) that cannot be directly written into an EHR.

These diagrams outline the structure and relationships of the system’s components, including the eCare planning applications, EHR systems, the supplemental data store (SDS), and the data flows. The components include:

1. **Two web-based eCare Planning applications**: MyCarePlanner and eCarePlanner.
2. **A supplemental data store (SDS) server**: A FHIR data store designed to contain health data not supported by institutional EHRs, such as patient-authored data.
3. **EHR system(s)**: Pilot EHR systems such as Epic at OHSU and VA VistA.
4. **Data flows**: USCDI data elements represented with FHIR resources.

The MCC care plan technical architecture leverages an SDS to host patient-contributed content. Within the patient- and caregiver-facing application, users contribute to their care plans by setting health goals, adding health concerns, and completing questionnaires, then stored in the SDS. The patient/caregiver application also collected patient data from multiple third-party EHR systems where a patient receives care and writes all of these data to the SDS for sharing with clinicians at OHSU and for use in research. The current SDS design is restricted to storing patient-compartment data, i.e. resources directly related to an individual patient, not Practitioner or Medication resources.

The patient/caregiver application reads these data from multiple providers and presents the collected data in a meaningful and usable way for use by non-clinicians, featuring capabilities like categorizing diagnoses based on extensive value sets using Clinical Quality Language (CQL). Clinicians can access the aggregated patient data from their institution, allowing them to view comprehensive health information collected by patients from multiple providers, including the patient-contributed content stored in the SDS.

## Component Features
The U.S. 21st Century Cures Act empowers all patients with a right to access their healthcare data from each of their providers that uses an ONC-certified EHR system. These data include U.S. Core Data for Interoperability (USCDI) data elements based on the US Core FHIR implementation guide for interoperability.

Both applications incorporate the HL7 SMART on FHIR standard to launch the applications using OAuth2 security with their existing healthcare provider portal account; no additional user accounts are used or supported.

The Supplemental Data Store (SDS) is a FHIR R4 data repository server using OAuth2 for access security. The SDS holds patient/caregiver-authored content that cannot be written into the EHR, plus aggregated from all provider systems where a patient has access. These aggregated data support care coordination and shared decision-making by clinicians who are working with patients who use MyCarePlanner.

Two web-based SMART on FHIR apps:
- MyCarePlanner for patients and caregivers
  - https://github.com/chronic-care/mycareplanner 
- eCarePlanner for clinicians, launched from within Epic and includes a reusable JavaScript library called the “Common Data Services” which supports mapping FHIR data resources to UI view objects
  - https://github.com/chronic-care/eCarePlanner 
  - https://github.com/chronic-care/e-care-common-data-services
- Supplemental Data Store (SDS)
  - https://github.com/OHSUCMP/ecp-sds-hardfork


