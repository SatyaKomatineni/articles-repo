# <!-- ************************************************************ -->
# Provider Operations in Healthcare IT
# <!-- ************************************************************ -->

**Note:** This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

10/26/2025/Satya Komatineni

This article provides an overview of *provider operations* in Healthcare IT — what they encompass, the technology layers involved, leading software vendors in the space, and how provider portals are managed to support patient engagement.

---

**Contents**

* Introduction
* Core Functions of Provider Operations
* Technology Infrastructure
* Leading Software Companies
* Patient Portals and Provider Management
* Handling Authorizations and Claims Submissions
* The Role of FHIR in Provider Operations
* Patient Authorization in FHIR
* The Practical Challenge of Multi-Provider FHIR Access
* Patient Access to Consolidated Medical Records
* Small Practices vs. Large Hospital Networks
* Joining Healthcare Networks as a Provider
* Acronyms and Abbreviations
* Summary
* References

---

# <!-- ************************************************************ -->
# Introduction
# <!-- ************************************************************ -->

In Healthcare IT, **provider operations** refer to the systems, workflows, and digital tools that enable healthcare providers—such as hospitals, clinics, and physician groups—to deliver patient care efficiently. These operations bridge the clinical and administrative worlds, ensuring that healthcare delivery remains effective, compliant, and financially sustainable.

---

# <!-- ************************************************************ -->
# Core Functions of Provider Operations
# <!-- ************************************************************ -->

Provider operations encompass the full lifecycle of clinical and administrative workflows that occur within a healthcare organization. The major components include:

* **Patient Access and Scheduling**: Appointment booking, registration, and insurance verification.
* **Clinical Documentation**: Electronic charting, orders, test results, and care notes recorded in Electronic Health Records (EHRs).
* **Revenue Cycle Management (RCM)**: Billing, claims submission, coding, and payment reconciliation.
* **Provider Credentialing and Compliance**: Verification of physician licenses, certifications, and adherence to HIPAA and CMS standards.
* **Care Coordination**: Data sharing and communication across multidisciplinary teams and care settings.

The overarching goal is to streamline provider workflows so clinicians can focus more on patient care rather than administrative burden.

---

# <!-- ************************************************************ -->
# Technology Infrastructure
# <!-- ************************************************************ -->

Provider operations rely heavily on digital systems integrated through standardized healthcare data protocols such as **HL7** and **FHIR**. Common components include:

* **Electronic Health Records (EHR) / Electronic Medical Records (EMR)**
  Manage clinical data and patient histories.

* **Practice Management Systems (PMS)**
  Handle scheduling, insurance verification, and administrative processes.

* **Health Information Exchanges (HIEs)**
  Facilitate secure sharing of patient data across organizations.

* **Analytics and Population Health Platforms**
  Enable insights into patient outcomes, performance, and risk management.

* **Interoperability Frameworks and APIs**
  Connect systems within and across provider networks, ensuring data continuity.

---

# <!-- ************************************************************ -->
# Leading Software Companies
# <!-- ************************************************************ -->

A few key players dominate provider operations technology through comprehensive platforms and integrations:

* **Epic Systems** – Industry leader offering EHR, patient portals (MyChart), scheduling, billing, and analytics in a single ecosystem.
* **Oracle Health (formerly Cerner)** – Provides EHRs, interoperability tools, and data-driven analytics platforms.
* **MEDITECH** – Focused on community hospitals and integrated care environments.
* **Athenahealth** – Cloud-based EHR, billing, and patient engagement platform widely used by physician practices.
* **NextGen Healthcare** – Specializes in ambulatory EHR and practice management software.
* **eClinicalWorks** – Offers EHR, telehealth, and patient engagement tools with cloud deployment.
* **Allscripts (now Altera Digital Health)** – Provides EHR and interoperability solutions across care settings.

These vendors collectively form the backbone of provider-side operations, supporting clinical documentation, workflow automation, and revenue processes.

---

# <!-- ************************************************************ -->
# Patient Portals and Provider Management
# <!-- ************************************************************ -->

**Patient portals** serve as the **digital front doors** for healthcare systems, giving patients access to their medical records, lab results, prescriptions, and scheduling features. These portals are typically managed **by the provider organizations themselves** but **powered by the same EHR vendors** that manage the internal clinical systems.

For example:

* **Epic’s MyChart**, **Cerner’s HealtheLife**, **Athenahealth’s Patient Portal**, and **eClinicalWorks’ Patient Portal** are among the most widely deployed.
* Providers maintain control over authentication, branding, and integration with appointment systems and billing.
* These portals also connect to national health record networks via FHIR-based APIs, enabling patients to consolidate data from multiple providers.

However, there are several **competing and independent patient portal vendors** that integrate across multiple EHR systems. Notable examples include:

* **FollowMyHealth** (originally by Allscripts) – A cross-platform patient engagement solution used by hospitals that run various EHRs.
* **MEDITECH Patient and Consumer Health Portal** – Designed for MEDITECH’s ecosystem but also supports interoperability through APIs.
* **NextGen Patient Portal** – Focused on ambulatory and specialty care practices.
* **Luma Health** – Provides messaging, scheduling, and engagement tools that can complement or replace native EHR portals.
* **Well Health** – Specializes in patient communication and outreach, integrating with multiple EHR systems.

Increasingly, provider IT teams and digital health departments are managing these portals in partnership with EHR vendors and third-party engagement platforms, ensuring compliance with **HIPAA** and the **21st Century Cures Act**’s interoperability and patient-access mandates.

---

# <!-- ************************************************************ -->
# Handling Authorizations and Claims Submissions
# <!-- ************************************************************ -->

Both cloud-based and enterprise EHR systems handle **insurance authorizations** and **claims submissions** as part of their revenue cycle management (RCM) workflows. The scope and automation level, however, differ depending on the vendor and organization size.

**1. Authorizations and Eligibility Checks**

* Most modern systems integrate with payer networks to verify insurance eligibility in real time.
* Cloud-based systems such as **Athenahealth** and **eClinicalWorks** provide built-in prior authorization modules that automatically request approval from payers based on clinical documentation.
* Enterprise systems like **Epic Resolute** and **Cerner Millennium** embed authorization tracking and alerts within the clinical workflow, ensuring claims compliance before submission.

**2. Claims Creation and Submission**

* All EHR and practice management systems generate claims using standardized formats (e.g., **ANSI X12 837**).
* Smaller cloud platforms often connect directly to clearinghouses (e.g., Availity, Change Healthcare) to transmit and reconcile claims.
* Larger hospital systems typically integrate RCM modules directly with payers and clearinghouses, automating batching, rejection handling, and payment posting.

**3. Denial Management and Analytics**

* Vendors like **Epic**, **Oracle Health**, and **NextGen** provide analytics dashboards to monitor claim denials, payment cycles, and payer performance.
* Cloud-based EHRs emphasize simplicity and transparency, allowing small practices to track claim status through vendor-hosted dashboards.

In short, while **cloud-based systems** prioritize ease of use and vendor-managed automation, **enterprise EHRs** integrate authorizations and claims processes deeply into clinical and financial systems for large-scale efficiency and compliance.

---

# <!-- ************************************************************ -->
# The Role of FHIR in Provider Operations
# <!-- ************************************************************ -->

**FHIR (Fast Healthcare Interoperability Resources)** is a modern data exchange standard developed by HL7 that underpins many interoperability efforts across healthcare IT systems. It enables different software platforms—such as EHRs, patient portals, and analytics applications—to share healthcare data securely and efficiently.

**1. Purpose and Design**
FHIR uses web-based technologies such as RESTful APIs and JSON or XML formats to simplify data exchange. It defines modular data elements, known as *resources*, representing key healthcare concepts like patients, medications, lab results, and encounters.

**2. How FHIR is Used in Provider Operations**

* **Data Interoperability**: Enables seamless sharing of patient data between providers, payers, and third-party applications.
* **Patient Access**: Powers patient-facing apps and portals by allowing individuals to download and view their medical data across multiple providers.
* **Care Coordination**: Supports integration between hospitals, clinics, and specialty practices, enabling real-time updates to care plans.
* **Public Health and Analytics**: Facilitates secure data submission to registries and population health systems.

**3. Vendor Implementation Examples**

* **Epic App Orchard** and **Oracle Health APIs** provide FHIR-based access for developers and partner organizations.
* **Athenahealth** and **eClinicalWorks** expose FHIR endpoints for patient data retrieval and integration with external digital health tools.
* National frameworks such as **CommonWell** and **Carequality** use FHIR to standardize data exchange across healthcare networks.

FHIR continues to evolve as the foundation for healthcare interoperability under mandates from the **21st Century Cures Act**, making it essential for both cloud-based and enterprise provider systems.

**4. Practical Adoption Levels**
In practical terms, FHIR adoption across the U.S. healthcare ecosystem is still **uneven**. Most major EHR vendors—Epic, Oracle Health, Athenahealth, and MEDITECH—now provide **FHIR APIs** for regulatory compliance, but real-world integration remains partial:

* Large hospital systems use FHIR mainly for **regulatory data access** and **patient-facing apps** rather than full clinical data exchange.
* Smaller practices often have FHIR endpoints available through their EHR vendor, but they rarely build or integrate third-party applications that utilize them.
* Many payer-provider integrations still rely on legacy HL7 or proprietary APIs, limiting end-to-end interoperability.

Overall, while FHIR is **technically available**, its **operational use** is still emerging. True cross-platform interoperability and clinical workflow integration are progressing gradually as health systems modernize their data strategies and comply with new CMS and ONC interoperability rules.

---

# <!-- ************************************************************ -->
# Patient Authorization in FHIR
# <!-- ************************************************************ -->

**Short answer:** For **read-only patient access**, authorization is usually **straightforward** thanks to **SMART on FHIR** (OAuth 2.0 + OpenID Connect). For **write access** or complex data flows (e.g., prior auth, payer APIs), it becomes **moderate to hard**, depending on vendor support and organizational policies.

**1. The Model (SMART on FHIR)**

* **Standards**: OAuth 2.0 + OpenID Connect (OIDC) with **SMART on FHIR** profiles.
* **Scopes**: Apps request permissions like `patient/*.read`, `patient/MedicationRequest.read`, `offline_access` (for refresh tokens).
* **Launch Modes**:

  * *EHR Launch*: App starts inside the EHR with context (patient, encounter).
  * *Standalone Launch*: App starts outside the EHR (e.g., a mobile app) and obtains patient consent.
* **Tokens**: Authorization Code Flow with **PKCE** is typical for public/mobile apps.

**2. Patient Experience**

* Patient selects their health system → authenticates on the provider portal (IdP) → reviews requested scopes → **consents** → the app gets an access token to call FHIR endpoints.
* Identity proofing levels vary (some organizations meet **NIST IAL2** for higher assurance, especially for proxy access and minors).

**3. Backend/System Integrations**

* **SMART Backend Services** ("system-to-system") use JWT client credentials for server apps (no end-user present). Common for registries, analytics, and some payer exchanges.
* Often constrained to specific resources and audited tightly.

**4. Where It Gets Hard**

* **Scope variability**: Not all vendors support the same scopes/resources; some are read-only.
* **Write operations**: POST/PUT/PATCH may be disabled or require extra review.
* **Rate limits & quotas**: Throttle bulk or high-frequency calls.
* **Multi-org reality**: Each provider may require separate **app registration** and security review; TEFCA/RS remain in progress for harmonization.
* **Patient matching**: Linking the right chart across multiple EHRs is still non-trivial.
* **Proxies & minors**: Complex consent rules and legal constraints differ by state and organization.

**5. Best-Practice Checklist**

* Use **SMART on FHIR** with Authorization Code + **PKCE** and **OIDC**.
* Request **least-privilege scopes**; prefer `patient/*.read` over broad system scopes.
* Support **refresh tokens** (`offline_access`) for background sync with short-lived access tokens.
* Implement **token introspection**, **revocation**, and **auditing** (audit events for PHI access).
* Handle **error codes** and **retry/backoff** for EHR rate limits.
* For system-to-system, use **SMART Backend Services** with signed JWTs and narrow scopes.

**6. Practical Difficulty Scale**

* **Easy**: Patient-facing, read-only app pulling USCDI data from a single EHR using SMART on FHIR.
* **Medium**: Multi-provider aggregation across several EHRs; handling different scopes and IdPs.
* **Hard**: Write-back to the chart, ePA/prior authorization automations (Da Vinci **PAS**, **CRD**, **DTR**), or payer-provider exchanges (**PDex**); requires custom integrations and governance.

---

# <!-- ************************************************************ -->
# The Practical Challenge of Multi-Provider FHIR Access
# <!-- ************************************************************ -->

For patients visiting multiple providers, **gathering records via FHIR-based approvals** can be surprisingly **time-consuming** and **fragmented** despite the technical promise of interoperability.

**1. Fragmented Authorization Process**
Each provider’s EHR (Epic, Oracle, Athenahealth, etc.) often requires **separate authentication and consent**. Patients must repeat the SMART on FHIR authorization flow for every health system, logging in through its portal and approving access for each new app connection.

**2. Data Fragmentation and Manual Linking**
Even after authorizing multiple providers, the data aggregated by patient-facing apps like Apple Health or CommonHealth may differ in depth and format. Some systems expose only **USCDI-mandated data** (problems, meds, allergies, immunizations), while others offer extended clinical notes and imaging. This results in **partial views** rather than full continuity of care.

**3. Identity and Matching Challenges**
FHIR endpoints identify patients by the provider’s internal patient ID, not by a universal identifier. This means patients must confirm their identity independently for each provider, making true automation difficult.

**4. Regulatory Improvements on the Horizon**
New frameworks under **TEFCA** aim to streamline consent and patient identity exchange, potentially enabling patients to authorize multi-provider data sharing through a single trusted app or network. However, widespread implementation is still underway.

In essence, while the FHIR standard simplifies data format and access, the **authorization experience remains decentralized**, requiring patients to manage multiple logins and consents to collect a complete health record.

---

# <!-- ************************************************************ -->
# Patient Access to Consolidated Medical Records
# <!-- ************************************************************ -->

Despite growing interoperability standards, there is **no single national repository** that stores all patient medical records in one place. However, several tools and initiatives now allow individuals to **aggregate their data across providers**:

**1. Health App Integrations**

* **Apple Health** and **CommonHealth** (for Android) enable patients to connect to multiple provider systems using FHIR-based APIs. Once linked, they can download clinical summaries, lab results, allergies, medications, and immunizations from various EHRs.
* These apps rely on patient authorization and FHIR endpoints exposed by EHR vendors such as Epic, Cerner, and Athenahealth.

**2. National Interoperability Networks**

* Frameworks like **CommonWell Health Alliance**, **Carequality**, and **TEFCA (Trusted Exchange Framework and Common Agreement)** are working toward a future where patients can retrieve standardized health data across all connected networks.

**3. Provider Portals and Health Record Aggregators**

* Patients can access records from each provider’s portal (e.g., MyChart, FollowMyHealth) and, in some cases, use health record aggregators that pull data from multiple sources into a unified dashboard.

**4. Limitations and Challenges**

* True nationwide record unification remains limited by differences in vendor implementation, data sharing policies, and patient consent mechanisms.
* Most aggregation tools provide **read-only access**, meaning patients can view but not edit or correct clinical data.

In practical terms, patients can increasingly **view and collect their data across systems**, but full interoperability and unified health records are still evolving.

---

# <!-- ************************************************************ -->
# Small Practices vs. Large Hospital Networks
# <!-- ************************************************************ -->

Provider operations differ significantly between **small provider practices** and **large hospital networks**, primarily in terms of system scale, complexity, and integration requirements.

**1. System Scope and Infrastructure**

* *Small Practices*: Typically use cloud-based, subscription EHR systems like Athenahealth, eClinicalWorks, or NextGen for ease of setup and lower IT overhead. Their systems are often all-in-one platforms combining EHR, billing, and scheduling.
* *Large Networks*: Deploy enterprise-grade EHRs such as Epic or Oracle Health that support multi-facility data exchange, specialty modules, and complex integrations with lab, imaging, and pharmacy systems.

**2. IT Management and Customization**

* *Small Practices*: Depend on vendor-hosted (SaaS) solutions with limited customization and outsourced IT management.
* *Large Networks*: Maintain in-house IT teams or managed service partners to handle custom configurations, data analytics, cybersecurity, and system interoperability.

**3. Cost and Implementation**

* *Small Practices*: Seek predictable subscription costs and quick onboarding. Their priority is usability and affordability.
* *Large Networks*: Invest millions in long-term EHR deployments, data centers, and staff training. Integration with clinical research systems and population health analytics is common.

**4. Interoperability and Data Exchange**

* *Small Practices*: Rely on vendor-provided FHIR APIs or regional Health Information Exchanges (HIEs) for data sharing.
* *Large Networks*: Participate in national networks like CommonWell and Carequality, managing complex patient data interoperability across states and systems.

**5. Patient Engagement**

* *Small Practices*: Use vendor-provided patient portals and communication tools with limited customization.
* *Large Networks*: Employ enterprise-scale engagement platforms like MyChart or FollowMyHealth with custom branding, integrated mobile apps, and multilingual access.

Overall, small practices prioritize **simplicity and cost-efficiency**, while large hospital systems emphasize **integration, scalability, and regulatory compliance** across diverse clinical and operational units.

---

# <!-- ************************************************************ -->
# Joining Healthcare Networks as a Provider
# <!-- ************************************************************ -->

Providers who wish to participate in a **healthcare payer network** (e.g., Medicare Advantage, Blue Cross Blue Shield, UnitedHealthcare) must go through a structured **credentialing and contracting process**. This ensures compliance, credential verification, and data interoperability between the provider and payer systems.

**1. Credentialing Process**

* Providers submit applications through the **Council for Affordable Quality Healthcare (CAQH)** ProView platform, a centralized credentialing database used by most payers.
* The process involves verifying professional licenses, certifications, malpractice history, hospital privileges, and insurance coverage.
* Payers validate this information against national databases like **NPDB (National Practitioner Data Bank)** and **NPI (National Provider Identifier)** registries.

**2. Contracting and Enrollment Systems**

* Once credentialed, providers sign participation agreements through payer-specific **Provider Enrollment Portals**.
* Major payers (e.g., **UnitedHealthcare**, **Aetna**, **Cigna**, **Anthem**) offer self-service portals where providers can apply, upload credentials, and manage contracts digitally.
* For government programs such as **Medicare** or **Medicaid**, providers must enroll through the **PECOS (Provider Enrollment, Chain, and Ownership System)** portal managed by CMS.

**3. Integration with Practice Management and EHR Systems**

* EHR and practice management vendors like **Athenahealth**, **NextGen**, and **Epic** often include built-in payer connectivity modules that streamline eligibility checks and claims submissions once credentialing is complete.
* Some organizations use **Provider Network Management (PNM)** tools (e.g., **Optum**, **Availity**, **Change Healthcare**) to maintain real-time provider data, credential status, and payer relationships.

**4. Ongoing Maintenance and Compliance**

* Providers must revalidate credentials periodically (usually every 2–3 years).
* Payers track compliance, network adequacy, and participation status through internal systems or delegated credentialing organizations.

In summary, joining a healthcare network requires coordination across **CAQH, PECOS**, payer enrollment portals, and practice management systems. These interconnected platforms ensure that providers meet regulatory and payer-specific requirements while enabling smooth data exchange for claims and authorizations.


# <!-- ************************************************************ -->
# Analytics and Population Health Platforms
# <!-- ************************************************************ -->

**Analytics and Population Health Platforms** are essential components of provider operations, transforming raw clinical and administrative data into actionable insights. These systems are designed to help healthcare organizations understand patterns in care delivery, manage risk, and improve outcomes across patient populations.

## 1. What Happens Here

* **Data Aggregation and Normalization** – Collects information from EHRs, claims, lab systems, and social determinants of health (SDOH) to create a unified patient and population view.
* **Performance Analytics** – Provides dashboards for quality measures, clinical outcomes, cost utilization, and readmission rates.
* **Risk Stratification** – Identifies patients at high risk for chronic diseases or hospital readmission using predictive modeling.
* **Care Gap Analysis** – Detects missing screenings, vaccines, or follow-ups to guide proactive outreach.
* **Population Health Management (PHM)** – Enables coordinated interventions for specific groups (e.g., diabetics, elderly, or post-surgery patients).

## 2. Why It’s Important

* **Supports Value-Based Care** – Helps organizations transition from fee-for-service to value-based payment models by tracking cost and outcomes.
* **Improves Preventive Care** – Early detection of at-risk patients enables timely interventions, reducing hospitalizations.
* **Regulatory Reporting** – Facilitates mandatory submissions to CMS and other agencies for quality programs (e.g., MIPS, ACO metrics).
* **Operational Efficiency** – Identifies workflow bottlenecks and resource allocation opportunities through data-driven insights.

## 3. Leading Examples

* **Epic Cogito** – Integrated analytics suite within Epic providing dashboards and predictive analytics.
* **Oracle Cerner HealtheIntent** – Cloud-based population health platform that aggregates data across systems.
* **Athenahealth Population Health** – Offers analytics tools for managing care coordination and performance in value-based programs.
* **Innovaccer** – Independent data platform that integrates disparate EHR and payer data for unified analytics.
* **Arcadia.io** – Population health and quality analytics for health systems and ACOs.

In essence, these platforms serve as the intelligence layer of provider operations—enabling organizations to monitor outcomes, meet regulatory requirements, and deliver proactive, personalized care at scale.

# <!-- ************************************************************ -->
# Acronyms and Abbreviations
# <!-- ************************************************************ -->

This section lists common acronyms used throughout the article with brief explanations:

* **API** – Application Programming Interface; software interface for data exchange between systems.
* **CAQH** – Council for Affordable Quality Healthcare; organization that provides credentialing platforms (ProView) for provider-payer enrollment.
* **CMS** – Centers for Medicare & Medicaid Services; U.S. federal agency overseeing healthcare programs and data standards.
* **EHR / EMR** – Electronic Health Record / Electronic Medical Record; digital versions of patient charts used for clinical care and data management.
* **FHIR** – Fast Healthcare Interoperability Resources; HL7-developed standard for exchanging healthcare data using modern web technologies.
* **HIE** – Health Information Exchange; platform enabling secure data sharing between healthcare organizations.
* **HIPAA** – Health Insurance Portability and Accountability Act; U.S. law governing privacy and security of health data.
* **HL7** – Health Level Seven International; organization that creates data exchange standards like FHIR.
* **NPI** – National Provider Identifier; unique identification number for healthcare providers in the U.S.
* **NPDB** – National Practitioner Data Bank; database that tracks malpractice and disciplinary records of healthcare providers.
* **OIDC** – OpenID Connect; an authentication layer built on OAuth 2.0 used in SMART on FHIR for secure login.
* **OAuth 2.0** – Open authorization standard used for secure, token-based access between apps and servers.
* **ONC** – Office of the National Coordinator for Health Information Technology; U.S. government agency promoting interoperability.
* **PAS / CRD / DTR** – Da Vinci Project standards for automating prior authorization workflows (Prior Authorization Support, Coverage Requirements Discovery, Documentation Templates and Rules).
* **PECOS** – Provider Enrollment, Chain, and Ownership System; CMS system for Medicare/Medicaid provider enrollment.
* **PHI** – Protected Health Information; any health data regulated under HIPAA privacy rules.
* **PKCE** – Proof Key for Code Exchange; OAuth 2.0 security mechanism for mobile/public apps.
* **PNM** – Provider Network Management; systems used by payers and health networks to manage provider data and contracts.
* **PMS** – Practice Management System; administrative software for appointments, billing, and scheduling.
* **RCM** – Revenue Cycle Management; process for handling patient billing, claims, and payments.
* **SMART on FHIR** – Substitutable Medical Apps and Reusable Technology; framework that extends FHIR with OAuth/OIDC for secure app integration.
* **TEFCA** – Trusted Exchange Framework and Common Agreement; national policy for health data exchange in the U.S.
* **USCDI** – United States Core Data for Interoperability; federally mandated data set defining minimum patient data for exchange.


# <!-- ************************************************************ -->
# Summary
# <!-- ************************************************************ -->

Provider operations in Healthcare IT encompass the end-to-end digital ecosystem that supports clinical care, operational efficiency, and patient engagement. From EHR systems and scheduling tools to patient portals and interoperability frameworks, these technologies collectively form the foundation of modern healthcare delivery.

The success of provider operations depends on integration across vendors, regulatory compliance, and the ability to adapt to evolving healthcare models such as telemedicine and value-based care.

---

# <!-- ************************************************************ -->
# References
# <!-- ************************************************************ -->

* [Epic Systems – Official Site](https://www.epic.com/)
  Overview of Epic’s EHR, MyChart patient portal, and interoperability capabilities.

* [Oracle Health (Cerner)](https://www.oracle.com/industries/healthcare/)
  Information on Oracle’s health IT offerings, including EHR and data analytics solutions.

* [Athenahealth](https://www.athenahealth.com/)
  Cloud-based provider operations and patient engagement solutions.

* [MEDITECH](https://ehr.meditech.com/)
  EHR systems for hospitals and community healthcare providers.

* [NextGen Healthcare](https://www.nextgen.com/)
  Ambulatory EHR and practice management solutions.

* [FollowMyHealth](https://www.followmyhealth.com/)
  Patient engagement platform that integrates with multiple EHR systems.

* [Luma Health](https://www.lumahealth.io/)
  Patient communication and engagement software used by provider networks.

* [Well Health](https://wellapp.com/)
  Platform for patient outreach, messaging, and interoperability with EHR systems.

* [Office of the National Coordinator for Health Information Technology (ONC)](https://www.healthit.gov/)
  Government resources on interoperability, FHIR APIs, and patient-access rules.
