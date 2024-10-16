# eCare Applications

## Security Considerations

**MyCarePlanner Application Access**
The MyCarePlanner web-based application implements the [HL7 SMART v1 standalone launch sequence](https://hl7.org/fhir/smart-app-launch/1.0.0/#standalone-launch-sequence) standard that includes use of OAuth2 for secure login and access to patient data. A patient or caregiver user must have an existing EHR portal account at each healthcare provider organization where they would like to use MyCarePlanner to access and display their data. No additional non-EHR user accounts are needed or supported.

**eCarePlanner Application Access**
The eCarePlanner web-based application implements the HL7 SMART v1 [EHR launch sequence](https://hl7.org/fhir/smart-app-launch/1.0.0/#ehr-launch-sequence) standard that includes use of OAuth2 for secure login and access to patient data. eCarePlanner is launched by a clinical user from within an EHR user’s desktop application using that practitioner’s current login credentials and the current patient being viewed in the EHR. The eCarePlanner application is launched with that patient’s data displayed, if the EHR system is configured to allow the practitioner to access that patient’s data.

**SMART App Registration with EHR Vendors**
Before a SMART app can run against an EHR, the app must be registered with that EHR’s authorization service. SMART does not specify a standards-based registration process, so in order to install MyCarePlanner and/or eCarePlanner, a developer must find the SMART app registration site that is hosted by each EHR vendor and create a registration for each app.

No matter how an app registers with an EHR’s authorization service, at registration time every SMART app must:
- Register zero or more fixed, fully-specified launch URL with the EHR’s authorization server
- Register one or more fixed, fully-specified redirect_uris with the EHR’s authorization server.

**Supplemental Data Store (SDS) Access**
The SDS implemented a customized design based on the open-source HAPI FHIR server to enable single sign-on for patients and clinical users at OHSU with their Epic system. A patient or caregiver user of MyCarePlanner must be logged in to OHSU’s Epic using their patient portal account, which will then authorize that user to read and write only their data in the SDS. Similarly, a clinical user must be logged in to Epic using their practitioner account, which will authorize their access to the SDS data. Contact OHSU for more information on this design and configuration.

**Supplemental Data Store (SDS) Data Repository**
The SDS is installed on-premises at OHSU with security provided by their IT infrastructure. OHSU was required to complete and gain approval for an Authority to Operate (ATO) with AHRQ in order to install the SDS for production use to store patient data, including secure transmission and storage of that data. Contact OHSU for details on the SDS security considerations for production use with patient data.
