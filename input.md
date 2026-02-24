# =========================================================
# Child Application Requirement Specification
# Claims Fraud Detection Service
# =========================================================

module:
name: claim-service

base_package: com.example.claimservice

description: >
Claim management service for handling insurance claims,
fraud detection, and investigator assignment.

# ---------------------------------------------------------
# ENTITY DEFINITIONS
# ---------------------------------------------------------

entities:
- name: Claim
  fields:
  id: Long
  claimNumber: String
  policyNumber: String
  claimType: String
  incidentDate: LocalDateTime
  incidentLocation: String
  description: String
  claimantName: String
  claimantEmail: String
  claimantPhone: String
  status: String
  fraudScore: Double
  fraudFlagged: Boolean

- name: Policy
  fields:
  id: Long
  policyNumber: String
  policyType: String
  startDate: LocalDate
  endDate: LocalDate
  sumInsured: Double
  status: String

- name: Investigator
  fields:
  id: Long
  name: String
  email: String
  assignedRegion: String
  active: Boolean

# ---------------------------------------------------------
# API ENDPOINTS
# ---------------------------------------------------------

endpoints:
- group: Claim
  method: POST
  path: /claims

- group: Claim
  method: GET
  path: /claims/{id}

- group: Claim
  method: GET
  path: /claims

- group: Claim
  method: PUT
  path: /claims/{id}

- group: Claim
  method: DELETE
  path: /claims/{id}

- group: Claim
  method: GET
  path: /claims/fraud/flagged

- group: Policy
  method: GET
  path: /policies/{policyNumber}

- group: Investigator
  method: GET
  path: /investigators

# ---------------------------------------------------------
# FEATURE FLAGS
# ---------------------------------------------------------

features:
usesDatabase: true
usesExternalApi: true
useLombok: true
fraudDetectionEnabled: true
alertNotificationsEnabled: true
