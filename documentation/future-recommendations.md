# Recommendations for Future eCare Work
In September 2024, the NIDDK-AHRQ development has compiled a list of suggestions for future iterations of the applications that were either out of scope or could not be accomplished during this contract period due to the maturity of FHIR adoption and other policies.

#### Broader interoperability landscape

* Monitor developments on the FHIR roadmap for TEFCA which would potentially enable providers to perform real-time queries for patient data with patient consent using the provider’s established care relationships. TEFCA’s FHIR roadmap is still nascent.  
* Monitor HIE expansion of FHIR capabilities and specifically patient-access FHIR APIs. Currently, HIEs are connected to providers but they may evolve their product model in the future to include patient access if they hear sufficient demand from their customers.  
* There are organizations that have private personal health repository products (i.e., [MyLinks](https://www.mypatientlink.com/mylinks-health-application/)). However, the data stored are mostly summarized from CMS or other claims data rather than patient-contributed data. With the personal health repository model, the patients own their data and can grant consent for access by others.  
* SMART on FHIR is still the most scalable way to connect apps to products and data repository services that have an external FHIR API interface. Even if these products and services are built using data structures that are proprietary or non-FHIR, as long as they have done the mapping to FHIR and provided the FHIR API layer, SMART on FHIR apps can be connected to them without special effort.  
* Check in on projects with eCare application implementations after January 1, 2026 when certified EHRs are required to adopt USCDIv3 which unlocks many more care planning-related data elements for exchange–notably, ServiceRequest used for orders, referrals, and care planning activities.

#### Use of MCC Value Sets

* Some work is being tested through a related ASTP/ONC-funded project: Applications of Machine Learning to Health Data for Patient-Centered Covid-19 Research. Many MCC eCarePlan IG value sets for conditions and lab results are being used in this project to summarize real-world data from over 250,000 patients in Indiana, preparing those FHIR data resources as feature inputs to Machine Learning (ML) models. We recommend staying tuned on the findings from this project.  
* Value set definitions evolve over time as terminology systems from SNOMED, ICD-10 and LOINC are updated, corrected, and expanded with new content. Ongoing maintenance is needed to keep the MCC IG value sets current and relevant.  
* As new initiatives are started to build on the MCC IG and applications, new clinical domains will be added that will require additional value sets to be created, preferably in VSAC. Examples already identified include cancer treatment, behavioral health, and dementia. The HL7 MCC IG is published for trial use, but a repository is needed to keep track of these additional value sets for future IG updates and for sharing among IG adopters.

#### Leveraging CQL

* The HL7 standard Clinical Quality Language (CQL) supports a platform-independent representation of clinical knowledge and built-in support for using value sets to classify FHIR data. By using CQL with MCC value sets in the ecare application and in the ASTP/ONC Machine Learning project, a single CQL logic implementation is available for reuse in JavaScript web apps and in Java language tools for data analysis or clinical decision support (CDS).  
* Based on our two experiences with reusing MCC value sets via CQL, there is an opportunity to extract this logic from MCC eCare applications into an independent component that could support future research analytics or CDS projects.

#### Questionnaire scoring and interpretation

* Clinicians have expressed interest in seeing scores from PCOR questionnaire instruments instead of individual question answers, or preferably the guideline-based interpretation of that score, e.g., “mild anxiety” instead of “6”. However, this guideline-based scoring and interpretation logic is unique to each PCOR instrument. And for PROMIS, the use of their scoring logic requires a paid license even for research and open-access projects.  
* This scoring and interpretation logic should be developed and managed in a standalone module for reuse in applications like MyCarePlanner and eCarePlanner, and also for use in clinical decision support and quality measurement. It should be designed such that it’s straightforward to expand this module with additional questionnaire assessment instruments.

#### Patient-reported outcomes

* There is an opportunity to continue collaboration with NCQA to expand the use of Person-Centered Outcomes (PCO) and digital quality measures that document person-centered goals, record progress outcomes over the lifetime of each goal, and share those goals and outcomes between providers, patients, and caregivers. These goals and outcome measures can become an important addition for MCC care planning.  
* NCQA’s work includes Goal Attainment Scaling (GAS) as a means to define goals focused on what matters most to each person, and to track progress toward outcomes. GAS complements use of Patient Reported Outcome Measures (PROMs) and has been well-received in NCQA pilot testing. Additional work is needed to define an HL7 FHIR IG that enables interoperability of PCO goals, GAS assessment of outcomes, and their use in quality measures. Updates on the Person-Centered Outcomes FHIR IG HL7 project is available [here](https://confluence.hl7.org/pages/viewpage.action?pageId=281216429).

#### Shared care planning

* There are opportunities to improve shared care planning and shared decision-making between clinicians, patients, and caregivers by including NCQA’s approach to capturing person-centered goals, outcome assessments, and progress tracking using Goal Attainment Scaling (GAS). Enabling patients and caregivers to enter their goals in MyCarePlanner is a good start, but these goals should incorporate GAS-defined levels of attainment and associated outcome tracking.  
* These enhanced person-centered goals must be shared across all provider organizations where a patient receives care, which will enable more effective person-centered shared care planning for patients with MCC. The current SDS only allows these patient or caregiver-authored goals to be seen by clinicians in a single primary care organization that hosts the SDS repository.  
* Add support for FHIR Task creation and assignment to members of the care team. Task management is an integral part of the SDOH Clinical Care IG and other efforts related to shared care planning and coordination.

#### Code refactoring and migration

* MyCarePlanner and eCarePlanner could benefit from code refactoring. This is a process where you restructure the code without changing user-facing functionality to improve the code’s structure and ease of maintenance. Due to the budget and time constraints, the development team was not able to dedicate effort to refactoring code.  
* Consider migration of eCarePlanner to Javascript React so that both MyCarePlanner and eCarePlanner will use the same code language. This will make adoption and future development much more efficient and feasible.  
* Consider migrating MyCarePlanner to a native app for both iOS and Android instead of remaining as a web-based application. Reasons a native app may make more sense now include:  
  * Better user experience for logging into multiple providers. A native mobile app would allow for preserving a user’s EHR access tokens between uses of the app and retaining the aggregated FHIR data using encrypted storage on a user’s mobile device.  
  * A native mobile app would also provide an improved user experience and app responsiveness using native UI display components.  
  * Better performance for loading larger amounts of data and running CQL processes. MyCarePlanner is subject to limits in data processing imposed by the browser which result in browser pop ups asking the user to wait or exit due to the page taking a while to load. There are workarounds to trick the browser into thinking that the data being processed is less than what it is, but a native app would provide a much more stable and sustainable solution.  
* There are several alternative development frameworks for developing a single mobile app codebase that is cross-platform on iOS and Android, each with its strengths and limitations. These alternative development frameworks continue to evolve and shift in rankings of preference and future support by open-source communities. Given that the MyCarePlanner app is developed using web-based React JavaScript frameworks, the best choice may be React Native which is based on many of the same components, but significant effort is still required to migrate from web to mobile native. Other alternative cross-platform frameworks should also be considered.

#### Application UX features

* Users have requested more graphical depictions of lab results and vital signs to more easily understand trends and abnormal results. Additionally, certain observations may benefit from a more specialized chart (i.e., radial dial design for the eGFR). The choice of graphical display programmatic tools may be different in a web-based app versus a native mobile app.  
* Users have expressed interest in seeing upcoming appointments or last scheduled encounter with specific providers. FHIR Encounter resources are available from EHR FHIR endpoints, however, queries for Encounter were not included in the applications. Future iterations may choose to include these to inform and refine Care Team display in both apps e.g., by providing Encounter dates associated with a Practitioner and prioritizing for filtering care team members based on recency of patient encounters.  
* Ability to save user-added preferences to the application, for example, pinning goals, health concerns, medications, etc. that are high priority for the user to the top of an application or reordering the display of listed items. This functionality would be much more feasible in a native app.
