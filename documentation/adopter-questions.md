# Adopter Questions
These questions were asked by a health system looking to implement the ecare applications. The proof of concept development team provided the responses below which other adopters may find helpful.

Adoption of the eCare apps and architecture is highly customizable depending on the adopter organization’s priorities and requirements. Below are a few notes on the developmental priorities the NIDDK-AHRQ project had which informed the design and implementation of the initial proof-of-concept eCare Plan Applications and architecture.

* Initial priority was on the effective display of clinical data for Multiple Chronic Conditions (MCC) for use by patients, caregivers, and clinicians. Data retrieval and aggregation from multiple EHR systems was an essential part of that goal.
* Patients and caregivers must be able to contribute to the comprehensive care plan, including patient goals, health concerns, and patient-reported assessments and outcomes. None of these data can be saved into any EHR system at this time, so our Supplemental Data Store (SDS) must support saving these data outside of the EHR system, enabling patients to add new data elements, and supporting patient and clinician secure and protected access to these data as an integral part of the app design.
* The eCare Project developed two SMART applications, one for patients/caregivers ("MyCarePlanner") and a second for clinicians ("eCarePlanner"). An important objective is to enable and improve shared decision-making between patients and clinical care team members, including consideration of patient-expressed contributions.

Depending on the adopter organization’s goals, the adopter may implement a simplified or adapted architecture and components i.e., implement a simplified design for a Supplemental Data Store (SDS), e.g., one that does not include saving third party EHR data into the SDS, or a second clinician-facing SMART app that accesses this SDS and is launched embedded within Epic.

## Questions and Responses

### How do the eCarePlan apps store data on a server?

Data are stored in a FHIR repository using SMART launch with OAuth2 to authorize the eCare app user and FHIR R4 APIs for all read/write operations.

For the eCare pilot project at OHSU the “Supplemental Data Store,” or “SDS,” is a HAPI FHIR server with some custom code added for our needs. The SDS is backed by a PostgreSQL database. The customized FHIR server allows the SDS to share an OAuth2 access token from another FHIR endpoint, eliminating the need for eCare app users to have a separate login. The OHSU Epic MyChart login is used to authorize the SDS in our pilot project.

### What happens after the patient's data is pulled down and stored in the browser?

The MyCarePlanner app retains all patient data within app memory for as long as the app is open and active. The app includes a configurable timeout period, after which the app is terminated and all loaded data will be removed.

The app’s Home tab includes a link action that prompts the user to share his/her data to the Supplemental Data Store. This is an opt-in action that must be selected by a user.

### Is it stored somewhere more permanently?

The SDS would be the “more permanent” storage location.

See the appendix, Sync for Science, with a very brief summary of our understanding of how S4S may be adapted for use at an adopter organization if the adopter project does not include some of NIDDK-AHRQ’s requirements, e.g., patient-contributed content or a clinician-facing app for shared decision making.

### Does eCarePlan require use of a server such as the supplemental data store (SDS)?

No, the SDS isn’t required for the MyCarePlanner app to work. However, without the SDS, MyCarePlanner will not allow patients or caregivers to add their personal goals, health concerns, or to complete questionnaire assessments (since there is no where to save these data), and the aggregated clinical data will be lost when the app is exited.

### Can a patient-user upload their data to the server?

Yes, patients can share their data represented in MyCarePlanner (patient-contributed goals, concerns and questionnaires, as well as third-party EHR data) with the SDS, as described above.

### How are MCC data mapping/value sets implemented in eCarePlan?

We are using the MCC FHIR IG value sets to classify conditions in the patient/caregiver app (MyCarePlanner). This involves the use of Clinical Quality Language (CQL) to apply logic to the FHIR data based on the value sets. We use the CQL to group conditions and present patient/consumer-friendly names for conditions. Documentation for CQL use in MyCarePlanner can be found here: [Developer CQL](https://github.com/chronic-care/mycareplanner/blob/main/documentation/developer-cql.md). There is no data or terminology mapping in MyCarePlanner other than using CQL and/or app logic to summarize the FHIR data for display within the app views. eCarePlanner (clinician app) does not use CQL and does contain some hardcoded value sets. Documentation of these can be found here: [eCarePlanner Query Logic](https://github.com/chronic-care/eCarePlanner/blob/master/documentation/query-logic.md).

### Are endpoint directories available in the next version of eCarePlan?

An endpoint directory was not included in the scope of the NIDDK-AHRQ project. The pilot implementer site, OHSU, included a short list of provider endpoints embedded in the app code.

### Are unstructured note queries available in the next version of eCarePlan?

We do not retrieve any unstructured notes via FHIR queries. Our apps were designed to query FHIR data that can be displayed within the apps and we have no plans to expand the app UI to display unstructured notes.

### Is it possible to have the backend pull the data in directly?

Not at this time. eCarePlan depends on patient-mediated access at this time as enabled by the 21st Century Cures Act. A few possibilities that the adopter organization could consider for local enhancements:

* Implement a backend that saves a patient’s OAuth2 access token and includes support for OAuth2 refresh tokens for a period of time authorized by each patient. Plus, patient consent to save this access token and a means for each patient to retract that authorization.
* Point-to-point business and data use agreements with each provider where you would like to configure backend data retrieval, and use of SMART and OAuth2 backend data services.
* Looking a few years down the road, we are optimistic that TEFCA will make this capability more feasible.

### Can eCarePlan track which data sources a patient has previously connected?

There is no robust support for tracking data sources connected. MyCarePlanner integrates with a “log service” system built by OHSU which collects logs from MyCarePlanner use. These logs are deidentified and intended only to collect basic system usage metrics. 

### Can a single institutional instance support multiple versions of eCarePlan?

We assume you are asking: Can an institution install multiple different versions of eCarePlan applications within their network space? Yes, each eCarePlan application instance will require its own deployment space assets and DNS entries, such as a VM from where the eCarePlan container can be launched with a DNS entry that points to that location so users can launch it, and appropriate SMART-on-FHIR client configurations for multiple redirect URLs. Multiple instances of eCarePlan apps at a single institution could make use of the same SDS instance, since the SDS is a separate service.

### Can an organization use configuration profiles for each endpoint to address each vendor's rules which change over time?

We do not have configuration profiles for vendor-specific FHIR query parameters. A goal of our project is to align with, and test adequacy of, US Core FHIR IG query requirements that are part of EHR certification. An objective of ASTP/ONC is for third-party apps like ours to be portable across vendors using a required standard API. However, we did need to include a small number of variations in the app queries that depended on vendor differences, e.g., whether or not a vendor supports \_include query parameters.

## Appendix: Sync for Science (S4S)

* https://www.healthit.gov/topic/sync-science
* https://www.healthit.gov/buzz-blog/precision-medicine/empowering-patients-to-advance-precision-medicine-one-ehr-at-a-time
* S4S objectives appear to have significant overlap with eCare requirements for a patient to share their data for research purposes (from S4S: "enable patient-directed data sharing for research"), but not for use in patient care coordination. S4S may support data aggregation, but not a supplemental store of content that is not supported by EHRs. S4S also does not appear to have any patient/caregiver-facing app to visualize their data, only to collect and share for research.
