# ZT Policy Profile: Golden State Water Treatment Facility
## 1. ZTA Component Definitions
## 2. Core Principle Application
## 3. Simplified Policy Table
## 4. Submission Details
### Policy Engine (PE)
The Policy Engine is the “brain” of the Zero Trust system. Its job is to look at security information (like who the user is, what device they are using, and where they are connecting from) and decide if access should be allowed or denied.
### Policy Administrator (PA)
The Policy Administrator takes the Policy Engine’s decision and puts it into action. If access is approved, it helps set up the connection or session so the user can access the resource.
### Policy Enforcement Point (PEP)
The Policy Enforcement Point is the “gatekeeper.” It is the part that actually enforces the decision by allowing approved traffic through or blocking denied traffic from reaching the protected resource.
### Chosen Principle: Verify Explicitly
“Verify Explicitly” means the system should check important security signals before giving access instead of automatically trusting the user.
At the Golden State Water Treatment Facility, the Policy Engine (PE) enforces this principle when someone tries to access the HR Employee PII Database. The PE checks the user’s identity, the security status of the device, and the network connection before making a decision. For example, if an HR employee signs in with the correct account, uses a secure company-managed device, and connects from an approved network or VPN, the PE can approve access. If one of those checks fails, the PE denies access.
This helps protect sensitive HR information like background check results and certification status by requiring verification before access is granted.
| Policy Requirement (Signal) | Condition to be Met by User | Action if Condition is Met |
|---|---|---|
| User Identity | User must sign in with a valid HR account and complete MFA (multi-factor authentication). | Grant Access |
| Device Posture | Device must be company-managed, encrypted, and updated with current security patches. | Grant Access |
| Network Context | User must connect from the facility network or an approved secure VPN connection. | Grant Access |
# Git Repository Metadata
Filename: ZT-Policy-Profile.md
Commit Message: Add ZT policy profile for Golden State Water Treatment Facility HR PII database - https://github.com/kalynS42/github-portfolio