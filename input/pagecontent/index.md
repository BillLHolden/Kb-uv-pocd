### Purpose
This Implementation Guide defines the use of FHIR resources to convey measurements and supporting data 
from acute care point-of-care medical devices (PoCD) intended for use by qualified professional to receiving systems for electronic medical records, 
clinical decision support, and medical data archiving for aggregate quality measurement and research purposes. 

It adds "deep metadata for device observations".  Key goals include supplementing the measurement values in an Observatiion resourcewith full provenance for traceability, and with fuller details of device architecture and dynamically changing attributes such as calibration history and battery state than is provided for in a FHIR Observation resource. 

The next release of this Guide intends to cover physiological and technical alerts from medical devices.  Another related implementation guide focuses on home-based personal health devices. 

### Scope and Boundaries
The scope of this Implementation Guide is a FHIR-based treatment of quantitative and qualitative observations communicated from point-of-care medical devices. Devices without communications capabilities are out of scope. Imaging devices are also out of scope. Personal devices intented for home health and fitness 
purposes to be used by non-professional users are excluded becasue they are covered by a related Implementation Guide. See below.

### <span style="color:blue"> Patient Monitoring Specifics Highlighed Blue Text</span>

<span style="color:blue">All Patiient Monitoring specific discussion and data will be presented in blue text.  This is done to delineate the unique patient monitoring information from the generic Point-Of-Care devices information.</span>

### <span style="color:blue"> Patient Monitoring Scope and Boundaries</span>

<span style="color:blue">Specifically, FHIR is a standard describing data formats and elements and an application programming interface for exchanging electronic health record data including Patient Monitoring data and alarming. FHIR, is a standard describing data formats and elements (known as "resources") and an application programming interface (API) for exchanging Electronic Health Records (EHR).</span>

<span style="color:blue">FHIR resources support message content equivalent to HL7 Version-2 (HL7 v2) which is the subject of this specification specific to Patient Care Devices.  It also supports multiple options for facilitating data exchange among systems.  The main difference between HL7 and FHIR lies in the fact that unlike HL7 v2, FHIR employs RESTful web services and open web technologies, such as JSON and RDF data formats.</span>

<span style="color:blue">FHIR is an excellent choice for patient monitoring data and alarm interfacing because it combines the features of HL7 with the latest web standards. Increased interoperability in healthcare systems is the major benefit i.e., new information can be exchanged more securely between phone apps, electronic health records, on healthcare system's servers, and many other places.</span>

<span style="color:blue">A FHIR Server is capable of processing, validating, and storing healthcare data in an industry-standard format that can be used for running search and other reporting capabilities.  FHIR is also designed to support transferring patient data to these server(s) at sites intending to be a central repository accessible through RESTful interfaces.</span>

<span style="color:blue">Although FHIR servers are commonly used when implementing FHIR, it is also possible per the FHIR specification to issue FHIR messaging as self-referential e.g., no server required.  Any references required e.g., to measuring device(s), for example, may be defined and referred to within each self-defining message. This allows for FHIR compliant messaging to be sourced without a FHIR server involved, a necessary requirement for some patient monitoring applications.</span>  

### Intended Readers
This Implementation Guide is intended for 
- clinical users of point-of-care device data, 
- implementers of other health information systems wishing to use this extended data,
- device and device gateway system developers. 

### Structure of this Guide
The "Getting Started" pages, which include a reference page on Abbreviations and Definitions, give a general introduction to the concepts and approach used in this Implementation Guide, relevant to all potential users of this Guide including end users of the data. There are also pages devoted to each of the two main categories of implementers needing developer-level information:

- information relevant to implementers of systems consuming data that has 
and "Profile" section details

- Fuller information for implementers of device and gateway systems who are setting 
up device models in such a way as to  accomodate the needs of data consumer applications.


### Relationship to Other Projects & Guides
This Implementation Guide covers material related to work in other projects and guides including the Personal Health Device Implementation Guide, and IHE Patient Care Device (PCD) profiles.

#### Personal Health Device IG
The [Personal Health Device Implementation guide](http://hl7.org/fhir/uv/phd/) treats wellness and chronic disease management devices used mainly by nonprofessionals in home and exercise settings. 

This Point-of-Care Device Implementation Guide is focused on acute-care devices for professional use mainly in healthcare delivery facilities. 

The Personal Health Device and Point-of-Care Device guides both use information models and nomenclature from the IEEE 11073 Medical Device Communications series of standards and the guides for these two kinds of devices are being developed cooperatively in the Devices On FHIR project with a goal of consistency and ease of use by receiving systems.

#### IHE Patient Care Device (PCD) Profiles
The IHE PCD domain has developed profiles for conveying acute care device data with context using HL7 V2. The base information system and nomenclature are based on the IEEE 11073 Medical Device Communications standards, also used in this FHIR Implementation Guide. 

These profiles cover Device Enterprise Communications (DEC), 
reporting device observations to enterprise systems including near-real-time 
charting to electronic medical records, clinical decision support systems and patient data archive systems. 
That is similar to the scope of this FHIR Implementation Guide.

A planned future FHIR use case for the Devices on FHIR group is the near-real-time communication of physiological and technical alerts to clinicians, and internal device status and state transition information for systems designed to process such information. This is similar to IHE PCD Alert Communication Management (ACM) and Infusion Pump Event Communication (IPEC) profiles.

Other PCD profiles include Implantable Device Cardiac Observations (IDCO), Point-of-Care Infusion Verification (PIV) and Point-of-Care Identity Management (PCIM). Future versions of this Implementation Guide will extend scope to cover related functionality based on FHIR rather than HL7 V2.

#### Vital Signs Profile
The Observation resource is used to record data that is retrieved from a device. Some values that are 
formalized in this resource are required to conform to the 
[Vital Signs Profile](http://hl7.org/fhir/observation-vitalsigns.html),
for example, heart rate or respiratory rate. Many of these are often provided by a PoCD patient-connected device.  
It is possible that in rare cases using this Implementation Guide that a device or device gateway using only ISO/IEEE 11073 terminology codes may not have sufficient information to recognize a particular meaurement as within the the Vital Signs set and determine an equivalent LOINC code and UCUM unit code for the result.  Therefor, provisions must be made to correctly make the mapping for these terms from 11073 to LOINC UCUM, and to ensure that this is done at the earliest possible point.

This is an area of necessary coordination between this IG and the Vital Signs Profile.

#### <span style="color:blue">FHIR Server communications and FHIR messaging in the Patient Monitoring Context</span>

<span style="color:blue">FHIR servers are intended to facilitate FHIR communications at site(s).  These server(s) receive messaging from each device monitoring the patient(s) through the FHIR restful interface(s).   The FHIR server(s) can later be queried for patient data by any FHIR compliant device.</span>

<span style="color:blue">Patient resource, Location resource of the patient(s) and more importantly the specific device(s) used to take measurements are provided to the server through a RESTful interface which then tracks and responds with the specified uniqueID for each device.  That uniqueID is then used for each subsequent measurement to bind said measurement to the exact device(s) used, the patient and the location for that measurement.  This is critical as the messaging need not repeat resource definitions, once provided, and acknowledged by the server.  This would require a FHIR server associated to the site.  e.g., shared or cloud server(s).</span>

<span style="color:blue">FHIR messaging essentially defines each resource within the message itself and is self contained with all pertinent informatoin.  Thus, for a measurement of a patient the references to the Patient resource, Location resource, Device Definition resource, Device resources used, etc. all will be declared within each message and referenced directly within the message.  A FHIR server is not required utilizing this approach. </span>

<span style="color:blue">NOTE: FHIR servers by default handle detection of repeated equivalent uniqueIds presented by self-referential messaging sent on an interval basis.  Non FHIR server products receiving FHIR messaging must be capable of detecting these repeated uniqueIds similarly.</span> 

<span style="color:blue">FHIR messaging and server-based messaging will be employed by Philips patient monitoring systems.  This is critical to note.</span>

<span style="color:blue">Philips Patient Monitoring systems will not require a FHIR server to source FHIR messaging as configured.</span>

<span style="color:blue">Philips Patient Monitoring systems support of both FHIR messaging and FHIR  server-based communications allows for communications to Philips based cloud products and offerings and other Philips’ products directly equivalent to HL7 IHE capability.</span>

<span style="color:blue">FHIR messaging and FHIR server-based communications will be employed in the patient monitoring context.  This is critical to note.  FHIR messaging will not require a FHIR server to source FHIR compliant messaging. </span> 

<span style="color:blue">Should a FHIR server receive a FHIR messaging based message, it would simply process Patient, Location, Device, etc. resources each time for each message received.  Reason for this is patient monitoring context FHIR based messages will include all supporting information within the message as to patient, location, devices applied, and so forth.</span>

<span style="color:blue">This is extra processing for the FHIR server but would still correctly operate as the server would recognize the duplicates.  </span>

### Abbreviations & Definitions

See under "Getting Started" -> "Abbreviations and Definitions"

### Future Guide Revisions
Future capabilities are planned for this General PoCD IG including:
1. Waveform Support optimized for high data volumes
2. Device Events & Alerts including:
  * Basic PCD ACM / IEC 60601-1-8 reporting
  * Status notifications back to the source Device
  
  
  

