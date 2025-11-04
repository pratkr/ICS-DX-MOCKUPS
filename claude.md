# ARGO HT Dual-Mode ICS Analysis and DX Mockup Requirements

## Project Overview
Building concept mockups for Alamar Biosciences ARGO HT system's dual-mode ICS (Instrument Control Software) for IVD (In Vitro Diagnostic) use. The goal is to transition from the current RUO (Research Use Only) system to an integrated DX version.

## Current RUO ICS Analysis

### System Architecture
- **Instrument**: ARGO HT - Three-bay laboratory instrument for NULISA assays
- **Interface**: Touch-screen ICS with dark theme UI
- **External Dependencies**:
  - NRV (NULISA Results Viewer) for results viewing
  - ACC (Alamar Cloud Computing) for cloud-based results

### Key Workflows Identified

#### User Authentication & Roles
- **Supervisor Level**: Full access to all functions, run creation, system management
- **Operator Level**: Limited access, primarily run execution and monitoring
- Login workflow with role-based permissions

#### Run Creation Process
1. **Sample Information Setup**
   - Manual entry or Excel/CSV import
   - Specific formatting requirements for sample metadata
   - Support for both tube-based and plate-based samples

2. **Assay Configuration**
   - NULISAqpcr and NULISAseq assay types
   - Protocol selection and parameter configuration
   - Quality control settings

3. **Bay Assignment & Loading**
   - Three-bay system with independent operation
   - Visual bay status indicators
   - Step-by-step loading procedures with guided instructions

#### Run Execution & Monitoring
- Real-time progress tracking with visual indicators
- Bay-specific status monitoring
- Error handling and troubleshooting workflows
- Completion notifications and next steps

### Current UI Patterns

#### Visual Design
- **Theme**: Dark background with light text
- **Navigation**: Tab-based interface structure
- **Status Indicators**: Color-coded progress bars and status icons
- **Data Entry**: Form-based inputs with validation
- **File Import**: Excel/CSV upload with format validation

#### User Experience
- Touch-optimized interface for laboratory environment
- Clear workflow progression with step indicators
- Contextual help and guidance throughout processes
- Error messaging and recovery options

## DX ICS Requirements (Target State)

### Key Differences from RUO
1. **Visual Theme**: Lighter theme instead of dark
2. **Integrated Results**: Built-in results viewing and downloading (eliminate NRV dependency)
3. **Clinical Reports**: Integrated clinical report generation
4. **IVD Messaging**: Regulatory compliance messaging throughout interface
5. **Workflow Integration**: Seamless run-to-results experience

### Workflow Enhancements Needed
- **Results Integration**: Direct results viewing within ICS post-run completion
- **Report Generation**: Clinical report creation and export capabilities
- **Data Management**: Enhanced data storage and retrieval for diagnostic use
- **Audit Trail**: Comprehensive logging for regulatory compliance
- **User Management**: Enhanced role-based access for clinical environments

### Technical Considerations
- Maintain existing hardware compatibility (touch-screen interface)
- Preserve core instrument control functionality
- Ensure backward compatibility with existing assay protocols
- Support regulatory validation requirements for IVD use

## Next Steps for Mockup Creation
1. Design lighter theme visual system
2. Create integrated results viewing interface
3. Develop clinical report generation UI
4. Implement IVD regulatory messaging
5. Design enhanced user management system
6. Create audit trail and compliance features

## File References
- `ICS-current.pdf`: Current RUO interface screenshots
- `ICS-current-screens-on-prem.pdf`: Detailed user guide and workflows

---

## Development Progress Log

### User Management Security Implementation (2025-11-01)

#### Issue Identified
Original user creation workflow in `settings.html` allowed administrators to directly set user passwords, which is **not compliant** with IVD software security standards and FDA 21 CFR Part 11 requirements.

**Problems:**
- Admin knows user's password (security vulnerability)
- No individual accountability
- Violates non-repudiation principles
- Insufficient audit trail for regulatory compliance

#### Solution Implemented: Temporary Password with Forced Change (Option 1)

**Industry Standard Approach:**
1. Admin creates user account with temporary password
2. System enforces password change on first login
3. Temporary password expires after initial use
4. Clear audit trail for all password-related events

#### Changes Made to `settings.html`

**1. Add User Modal Updates:**
- ✅ Removed "Confirm Password" field (not needed for temporary passwords)
- ✅ Relabeled password field to "Temporary Password" with expiration messaging
- ✅ Added **Email Address** field for password reset notifications
- ✅ Added **"Generate" button** for auto-generating secure temporary passwords
- ✅ Added prominent **yellow security notice panel** explaining:
  - User must change password on first login
  - Temporary password expires after initial use
  - Password should be provided securely to user
  - Temporary password must meet standard requirements
- ✅ Kept password requirements reference panel

**2. JavaScript Functions Added/Updated:**

```javascript
generateTemporaryPassword()
```
- Generates cryptographically secure 12-character password
- Ensures all password requirements are met (upper, lower, number, symbol)
- Displays generated password to admin with security warnings
- Auto-shows password field when generated

```javascript
createNewUser()
```
- Added email validation (required field)
- Updated success message to display temporary password
- Sets user status to "Password Reset Required" with warning indicator
- Shows "Pending first login" in Last Login column
- Comprehensive security reminders in success alert

**3. Audit Log Enhancements:**
- ✅ Updated example entry: "Temporary password set for: lab_tech1 (expires on first login)"
- ✅ Added audit trail showing complete first-login workflow:
  - AS-75: Temporary password set
  - AS-20: First login with mandatory password change prompt
  - AS-75: Successful password change on first login
- ✅ Updated info panel to clarify temporary password tracking

**4. User Table Display:**
- New users show **warning status indicator** (yellow)
- Status displays "Password Reset Required" until first login
- Last Login shows "Pending first login" in red italic text
- After first successful login and password change:
  - Status updates to "Active" (green indicator)
  - Last Login shows actual login timestamp

#### Compliance Features

**FDA 21 CFR Part 11 Alignment:**
- ✅ Individual accountability (passwords only known to users)
- ✅ Non-repudiation (clear audit trail)
- ✅ Password complexity enforcement
- ✅ Comprehensive audit logging (AS-75 event codes)
- ✅ Temporary password expiration
- ✅ Forced password change workflow

**Security Best Practices:**
- ✅ Admin never knows user's permanent password
- ✅ Temporary passwords are single-use
- ✅ Email field for future password reset functionality
- ✅ Visual indicators for pending accounts
- ✅ Auto-generation of secure passwords
- ✅ Clear security messaging throughout UI

#### Files Modified
- `settings.html` (lines 1268-1625): Complete user management security overhaul

#### Next Steps for User Security
- [ ] Create first-login password change screen mockup
- [ ] Design password reset workflow (forgot password)
- [ ] Add password expiration policy (e.g., 90 days)
- [ ] Design password history enforcement (prevent reuse)
- [ ] Add account lockout after failed login attempts
- [ ] Create user session management interface