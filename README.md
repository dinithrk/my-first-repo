# ATLAS (Adaptive Time-Series Analytics & Logging System)
## End-User Operational Manual & Product Guide

**Platform Release:** ATLAS Enterprise Hydro Risk Platform  
**Target Environment:** Web Single Page Application (Desktop & Field Tablets)  
**Author:** Principal Product Management & Technical Documentation Group  
**Classification:** Operational End-User Guide  

---

## Table of Contents
1. [Platform Overview & Core Capabilities](#1-platform-overview--core-capabilities)
   - [1.1 Mission & Problem Domain](#11-mission--problem-domain)
   - [1.2 Key Modules & Feature Matrix](#12-key-modules--feature-matrix)
   - [1.3 End-to-End Operational Lifecycle](#13-end-to-end-operational-lifecycle)
2. [User Roles & Permission Matrix](#2-user-roles--permission-matrix)
   - [2.1 User Persona Definitions](#21-user-persona-definitions)
   - [2.2 Role-Based Access Control (RBAC) Matrix](#22-role-based-access-control-rbac-matrix)
3. [Getting Started & Account Setup](#3-getting-started--account-setup)
   - [3.1 Accessing the Application](#31-accessing-the-application)
   - [3.2 First-Time Platform Configuration Checklist](#32-first-time-platform-configuration-checklist)
   - [3.3 Authentication, Session Lifecycles & Security](#33-authentication-session-lifecycles--security)
4. [Step-by-Step Task Guides (Core Workflows)](#4-step-by-step-task-guides-core-workflows)
   - [Workflow 1: Account Access & Personal Profile Management](#workflow-1-account-access--personal-profile-management)
   - [Workflow 2: User Account Administration & Role Provisioning](#workflow-2-user-account-administration--role-provisioning)
   - [Workflow 3: Managing Geographical Monitoring Sites](#workflow-3-managing-geographical-monitoring-sites)
   - [Workflow 4: Configuring Sensor Types & Categories](#workflow-4-configuring-sensor-types--categories)
   - [Workflow 5: Provisioning Sensor Hardware & 4-Tier Alert Thresholds](#workflow-5-provisioning-sensor-hardware--4-tier-alert-thresholds)
   - [Workflow 6: Real-Time Geospatial (GIS) Telemetry Surveillance](#workflow-6-real-time-geospatial-gis-telemetry-surveillance)
   - [Workflow 7: Monitoring & Escalating Active Disaster Alerts](#workflow-7-monitoring--escalating-active-disaster-alerts)
   - [Workflow 8: Compiling & Exporting Regulatory Incident & Telemetry Reports](#workflow-8-compiling--exporting-regulatory-incident--telemetry-reports)
5. [Reporting, Dashboards & Data Exports](#5-reporting-dashboards--data-exports)
   - [5.1 Historical Telemetry Audits](#51-historical-telemetry-audits)
   - [5.2 Disaster Incident Documentation](#52-disaster-incident-documentation)
   - [5.3 Supported File Formats & Layouts](#53-supported-file-formats--layouts)
   - [5.4 Download History & One-Click Re-generation](#54-download-history--one-click-re-generation)
6. [Field Device & External System Integration Guide](#6-field-device--external-system-integration-guide)
   - [6.1 IoT Edge Telemetry Protocol (MQTT Ingestion)](#61-iot-edge-telemetry-protocol-mqtt-ingestion)
   - [6.2 External API Integration & REST Contracts](#62-external-api-integration--rest-contracts)
   - [6.3 Programmatic Authentication & Token Headers](#63-programmatic-authentication--token-headers)
7. [Error Handling, Troubleshooting & FAQ](#7-error-handling-troubleshooting--faq)
   - [7.1 Common User Errors & Solutions](#71-common-user-errors--solutions)
   - [7.2 System Thresholds, Timing & Latency Constraints](#72-system-thresholds-timing--latency-constraints)
   - [7.3 Escalation & Support Paths](#73-escalation--support-paths)

---

## 1. Platform Overview & Core Capabilities

### 1.1 Mission & Problem Domain
ATLAS (**Adaptive Time-Series Analytics and Logging System**) is an enterprise hydro-meteorological surveillance and disaster decision-support platform. Engineered to protect lives, agriculture, and municipal infrastructure across sensitive river basins (e.g., Kelani River, Mahaweli River, and coastal hydrological networks), ATLAS bridges the gap between raw field instrumentation and critical disaster response.

In conventional flood and drought monitoring, authorities struggle with fragmented telemetry loggers, delayed alert dispatches, manual spreadsheet compilations, and static maps that lack real-time context. ATLAS solves these operational bottlenecks by providing:
1. **Zero-Latency Ingestion:** Continuous capture of river water levels, precipitation volumes, flow velocities, and hardware battery health from remote solar/cellular edge stations.
2. **Automated Risk Escalation:** Continuous sliding-window evaluation of incoming telemetry against statutory 4-tier warning and critical limits, generating active alerts without human intervention.
3. **Geospatial Command Center:** An interactive GIS surveillance dashboard with animated risk-severity markers, directional zoom, and sensor telemetry sparklines.
4. **Instant Regulatory Reporting:** Immediate compilation of operational sensor logs and emergency alert incident histories into publication-grade Adobe PDF documents or raw CSV spreadsheets for government disaster management agencies.

```mermaid
graph LR
    subgraph DataCollection ["Field Instrumentation"]
        Sensors["Remote Sensor Units<br/>(Water Level, Flow, Rain)"]
    end

    subgraph CoreEngine ["ATLAS Intelligence Core"]
        Ingestion["Continuous Ingestion<br/>& Health Validation"]
        Evaluator["Dynamic 4-Tier<br/>Threshold Engine"]
        GISService["Geospatial Projection<br/>& Trend Analytics"]
    end

    subgraph UserSurfaces ["Operational Interfaces"]
        GIS["GIS Map Dashboard"]
        AlertFeed["Active Alerts Monitor"]
        Reports["Regulatory Document Export"]
        AdminUI["Metadata & User Admin"]
    end

    Sensors --> Ingestion
    Ingestion --> Evaluator
    Ingestion --> GISService
    Evaluator --> AlertFeed
    GISService --> GIS
    Evaluator --> Reports
    AdminUI --> Evaluator
```

---

### 1.2 Key Modules & Feature Matrix

| Module | Purpose | Primary Capabilities | Target Users |
|---|---|---|---|
| **GIS Monitor** (`/gis-dashboard`) | Real-time map-based situational awareness | Interactive map of Sri Lanka, pulsing severity pins, station search, battery monitor, 1H/24H/7D/30D telemetry graphs | Operators, Field Engineers, Directors |
| **Active Alerts** (`/alerts`) | Real-time threat detection & triage | Continuous live alert feed, 4-tier severity tags, breached threshold comparison, manual refresh | Disaster Response Officers, Emergency Teams |
| **Sensors Inventory** (`/metadata/sensors`) | Sensor catalog & boundary configuration | Hardware registration, GPS coordinate assignment, 4-tier threshold boundary setup (`Low Critical`, `Low Warning`, `High Warning`, `High Critical`) | System Administrators, Technical Operators |
| **Site Management** (`/metadata/sites`) | Geographical zoning & river basin topology | Registration of river basins, monitoring posts, and geographic locations | System Administrators, Technical Operators |
| **Sensor Types** (`/metadata/sensor-types`) | Parameter cataloging & measurement units | Definition of physical parameters (Water Level, Flow Rate, Rain Gauge) | System Administrators, Technical Operators |
| **System Reports** (`/reports`) | Official auditing & historical data export | On-demand compilation of Telemetry Logs and Incident Alerts into formal PDF or raw CSV formats, download history log | Auditors, Planners, Executive Leadership |
| **User Administration** (`/admin/users`) | Access governance & identity lifecycle | Account provisioning, role assignments (`ADMIN`, `OPERATOR`, `VIEWER`), profile modifications, account deactivation | System Administrators |
| **Personal Profile** (`/profile`) | Self-service credential & identity security | Updating personal contact info, password rotation with confirmation checks | All Authenticated Users |

---

### 1.3 End-to-End Operational Lifecycle

The ATLAS operational loop transforms field telemetry into coordinated civic action through six automated stages:

```mermaid
sequenceDiagram
    autonumber
    actor Field as Field Edge Sensor
    participant System as ATLAS Platform
    actor Operator as Disaster Response Officer
    actor Admin as System Administrator

    Note over Admin,System: Phase 1: Infrastructure Provisioning
    Admin->>System: Register Site (e.g., Kelani Basin)
    Admin->>System: Register Sensor with 4-Tier Thresholds (Normal: 2-6m, Warning: 6-8m, Critical: >8m)

    Note over Field,System: Phase 2: Autonomous Surveillance
    Field->>System: Streams river level (7.25m, Battery: 94%)
    System->>System: Evaluates reading against thresholds -> BREACH: HIGH_WARNING
    System->>System: Elevates Sensor state to High Warning on GIS Monitor

    Note over Operator,System: Phase 3: Response & Escalation
    Operator->>System: Inspects Active Alerts screen & GIS Map
    Operator->>System: Reviews 24h trend sparkline confirming rapid river rise
    Operator->>System: Dispatches regional evacuation advisory

    Note over Field,System: Phase 4: Resolution & Auditing
    Field->>System: Telemetry recedes to normal (4.10m)
    System->>System: Automatically resolves alert & updates status to RESOLVED
    Operator->>System: Exports Official Regulatory PDF Incident Report
```

---

## 2. User Roles & Permission Matrix

### 2.1 User Persona Definitions

#### 1. System Administrator (`ADMIN`)
* **Profile:** Central IT administrator, meteorological chief technical officer, or disaster management IT lead.
* **Operational Goal:** Maintain platform health, manage physical sensor inventory, configure river basin sites, provision staff accounts, and govern system security.
* **Key Tasks:** Creating and deactivating user accounts, assigning roles, updating sensor coordinates, defining safety threshold boundaries, and auditing system usage.

#### 2. Technical Operator (`OPERATOR`)
* **Profile:** Hydro-meteorological duty officer, irrigation engineer, or municipal disaster response specialist.
* **Operational Goal:** Maintain day-to-day monitoring infrastructure, register new field sensor units, update equipment placements, monitor active flooding/drought alerts, and produce post-incident audit documentation.
* **Key Tasks:** Provisioning new sensor nodes, updating threshold limits based on seasonal monsoon directives, tracking GIS maps during severe weather events, and generating periodic reports.

#### 3. Public Viewer / Auditor (`VIEWER`)
* **Profile:** External emergency responder, regional civic official, environmental auditor, or research partner.
* **Operational Goal:** Observe live hydrological conditions, inspect sensor battery health, review historical river trends, and download operational reports without risk of altering system parameters.
* **Key Tasks:** Reviewing GIS map telemetry, inspecting active alerts, downloading PDF/CSV historical summaries, and tracking personal profile settings.

#### 4. External Edge System / Data Ingestion Gateway (Machine Persona)
* **Profile:** Automated telemetry field logger, solar-powered ESP32 microcontroller, or regional meteorological data gateway.
* **Operational Goal:** Transmit continuous environmental telemetry packages reliably into the ATLAS ingestion pipeline.
* **Key Tasks:** Emitting JSON telemetry payloads over MQTT protocols adhering to standardized device schemas.

---

### 2.2 Role-Based Access Control (RBAC) Matrix

| Feature / Business Action | Administrator (`ADMIN`) | Operator (`OPERATOR`) | Viewer (`VIEWER`) | External Field Device |
|---|---|---|---|---|
| **View Dashboard & GIS Monitor** | Allowed | Allowed | Allowed | Denied |
| **Inspect Active Alerts Feed** | Allowed | Allowed | Allowed | Denied |
| **View Sensor Telemetry Sparklines** | Allowed | Allowed | Allowed | Denied |
| **Export Reports (PDF / CSV)** | Allowed | Allowed | Allowed | Denied |
| **Re-download Historical Reports** | Allowed | Allowed | Allowed | Denied |
| **View Sensor & Site Inventory** | Allowed | Allowed | Allowed | Denied |
| **Update Personal Profile & Password** | Allowed | Allowed | Allowed | Denied |
| **Create / Edit Monitoring Sites** | Allowed | Allowed | Denied | Denied |
| **Delete Monitoring Sites** | Allowed | Allowed | Denied | Denied |
| **Create / Edit Sensor Categories** | Allowed | Allowed | Denied | Denied |
| **Delete Sensor Categories** | Allowed | Allowed | Denied | Denied |
| **Register / Edit Sensors & Thresholds** | Allowed | Allowed | Denied | Denied |
| **Decommission / Delete Sensors** | Allowed | Allowed | Denied | Denied |
| **View User Management Panel** | Allowed | Denied | Denied | Denied |
| **Create New User Accounts** | Allowed | Denied | Denied | Denied |
| **Modify User Roles & Profiles** | Allowed | Denied | Denied | Denied |
| **Delete User Accounts** | Allowed | Denied | Denied | Denied |
| **Transmit Field Telemetry (MQTT)** | Denied | Denied | Denied | Allowed |

> [!NOTE]
> The Viewer role possesses unrestricted read-only visibility into operational metrics, geographical coordinates, and exportable reports. Write operations (creating, editing, or deleting metadata) are strictly blocked at the user interface and rejected with HTTP `403 Forbidden` at the service boundary.

---

## 3. Getting Started & Account Setup

### 3.1 Accessing the Application
ATLAS is accessible via any modern desktop or field tablet web browser (Google Chrome 110+, Mozilla Firefox 110+, Microsoft Edge 110+, Apple Safari 16+).

1. Open your web browser and navigate to the assigned portal URL:
   * **Local / Staging Environment:** `http://localhost:8090`
   * **Production Environment:** `https://atlas.disaster-management.gov` (or your agency domain)
2. If unauthenticated, the application automatically redirects you to the **Sign In** screen (`/login`).
3. Enter your assigned **Username** and **Password**, then click **Log in**.
4. Upon successful validation, the system loads the main **Overview Dashboard** (`/`).

---

### 3.2 First-Time Platform Configuration Checklist
Before field telemetry can be evaluated or displayed on the GIS map, an **Administrator** or **Operator** must execute the initial configuration sequence:

```
[ ] Step 1: Provision System Accounts
    Create Operator and Viewer accounts via the User Management portal (/admin/users).

[ ] Step 2: Establish River Basin Sites
    Define geographical jurisdictions and river basins in the Sites module (/metadata/sites).

[ ] Step 3: Establish Sensor Parameter Types
    Configure physical measurement units (e.g., Water Level in meters) in Sensor Types (/metadata/sensor-types).

[ ] Step 4: Register Field Hardware & 4-Tier Thresholds
    Catalog physical sensors with exact GPS coordinates and assign safety thresholds (/metadata/sensors).

[ ] Step 5: Verify Edge Telemetry Stream
    Confirm that field loggers transmit valid payloads and pins appear on the GIS Monitor (/gis-dashboard).
```

---

### 3.3 Authentication, Session Lifecycles & Security

ATLAS employs dual-token security architecture designed to safeguard emergency operations against unauthorized tampering while providing a seamless user experience during ongoing crisis monitoring.

```mermaid
stateDiagram-v2
    [*] --> Unauthenticated: Open Browser Portal
    Unauthenticated --> Authenticated: Enter Username + Password (POST /api/auth/login)
    
    state Authenticated {
        [*] --> ActiveSession: Store Access Token (15-min lifespan)
        ActiveSession --> BackgroundRefresh: Token Approaching Expiry
        BackgroundRefresh --> ActiveSession: Transparent Token Refresh (24-hr cycle)
    }

    ActiveSession --> LoggedOut: User Clicks "Logout"
    ActiveSession --> SessionExpired: 24h Inactivity / Cookie Revocation
    LoggedOut --> Unauthenticated: Tokens Revoked & Blacklisted
    SessionExpired --> Unauthenticated: Redirect to /login
```

#### Key Security Characteristics:
1. **Access Token Lifespan:** Valid for **15 minutes**. Automatically attached to every API request via the browser.
2. **Silent Background Token Refresh:** A secure, HTTP-only refresh token allows your browser session to renew expired access tokens automatically in the background without interrupting your work.
3. **Session Inactivity Limit:** After **24 hours** without activity, refresh tokens expire, prompting a return to the login screen.
4. **Immediate Distributed Revocation:** Clicking **Logout** immediately revokes the active token across all platform services, preventing replay attacks from shared terminal environments.
5. **Credential Protection:** All user passwords are encrypted using one-way BCrypt cryptographic hashing. Passwords are never stored or transmitted in plain text.

---

## 4. Step-by-Step Task Guides (Core Workflows)

### Workflow 1: Account Access & Personal Profile Management

#### Objective
Authenticate into the ATLAS platform, verify account credentials, adjust display preferences, update contact details, and rotate passwords periodically to maintain security compliance.

#### Prerequisites
* Valid user credentials issued by your System Administrator.

#### Execution Steps: Authenticating
1. Open `http://localhost:8090/login`.
2. In the **Username** field, enter your assigned username.
3. In the **Password** field, enter your password.
4. Click the primary blue **Log in** button.
   * *Expected Outcome:* The screen transitions to the main monitoring dashboard. The navigation sidebar appears on the left with your status set to **System Online**.

#### Execution Steps: Toggling Visual Theme
1. Locate the **Settings** cog icon at the top-right of the left sidebar.
2. Click the cog icon to open the settings dropdown menu.
3. Click **Light Mode** (or **Dark Mode**) to switch the interface theme.
   * *Expected Outcome:* The interface instantly updates its palette. Your preference is persisted in your browser for future sessions.

#### Execution Steps: Updating Profile Information
1. Open the **Settings** dropdown menu from the sidebar and select **Profile** (or navigate directly to `/profile`).
2. Under the **Personal Information** card, review your current details:
   * **First Name:** Enter your updated first name.
   * **Last Name:** Enter your updated last name.
   * **Username:** Update your username if required.
   * **Email:** Enter your valid institutional email address.
   * *Note:* The **Role** field is read-only and displays your assigned privilege tier.
3. Click the blue **Save Changes** button.
   * *Expected Outcome:* A green confirmation toast appears stating `"Profile updated successfully"`. The navigation bar reflects your updated information.

#### Execution Steps: Rotating Account Password
1. Scroll down to the **Change Password** card on the `/profile` page.
2. Fill out the three password verification inputs:
   * **Current Password:** Enter your existing account password.
   * **New Password:** Enter your new password (minimum **6 characters**).
   * **Confirm New Password:** Re-enter your new password to verify accuracy.
3. Click the blue **Update Password** button.
   * *Expected Outcome:* A confirmation toast appears stating `"Password changed successfully"`. The password input fields clear automatically.

```
+---------------------------------------------------------------------------------------+
|                                    MY PROFILE                                         |
+---------------------------------------------------------------------------------------+
| Personal Information                                                                  |
| [ First Name: Samantha ]             [ Last Name: Perera         ]                    |
| [ Username:   s_perera ]             [ Email:     s.perera@gov.lk ]                    |
| [ Role:       OPERATOR (Disabled) ]                                                   |
| [ Save Changes (Button) ]                                                             |
+---------------------------------------------------------------------------------------+
| Change Password                                                                       |
| [ Current Password: ********** ]                                                      |
| [ New Password:     ********** ]     [ Confirm Password: ********** ]                 |
| [ Update Password (Button) ]                                                          |
+---------------------------------------------------------------------------------------+
```

---

### Workflow 2: User Account Administration & Role Provisioning

#### Objective
Create, inspect, update, and deactivate platform user accounts to govern team access across operational monitoring centers.

#### Prerequisites
* Must be logged in as an account with the **Administrator (`ADMIN`)** role.

#### Execution Steps: Creating a New User Account
1. From the navigation sidebar, click **User Management** (accessible at `/admin/users`).
2. Click the blue **Add User** button (`+` icon) at the top right.
3. In the **Create New User** modal dialog, complete the required fields:
   * **Username:** Unique login handle (e.g., `j_fernando`).
   * **Password:** Temporary alphanumeric password.
   * **First Name:** Given name of the staff member.
   * **Last Name:** Surname of the staff member.
   * **Email:** Official organizational email address.
   * **Role:** Select the appropriate operational privilege:
     * `ADMIN`: Full administrative control.
     * `OPERATOR`: Sensor, site, and operational management.
     * `VIEWER`: Read-only surveillance and reporting.
4. Click the blue **Create** button.
   * *Expected Outcome:* A confirmation message `"User created successfully"` displays. The modal closes, and the new account appears in the user directory table.

#### Execution Steps: Modifying an Existing User Profile or Role
1. Locate the user in the table using the **Search username, name, email** search bar.
2. In the user row's **Action** column, click the **Edit** icon (pencil symbol).
3. The **Edit User** modal opens pre-filled with the user's current data.
   * *Note:* The password field is excluded from this modal to protect existing credentials. Users rotate their own passwords via their Profile portal.
4. Adjust the user's name, email, or select a new **Role** from the dropdown menu.
5. Click **Save**.
   * *Expected Outcome:* A green banner confirms `"User updated successfully"`. The role tag updates in the user table immediately.

#### Execution Steps: Deactivating a User Account
1. In the user directory table, locate the target user account.
2. In the **Action** column, click the red **Delete** icon (trash can symbol).
3. A confirmation popover appears: *"Are you sure to delete this user?"*
4. Click **Yes**.
   * *Expected Outcome:* A green banner displays `"User deleted successfully"`. The record disappears from the user directory.

> [!CAUTION]
> **Self-Deletion Protection:** Administrators cannot delete their own active account. Attempting to delete your own logged-in account displays an error message: `"You cannot delete your own account."`

---

### Workflow 3: Managing Geographical Monitoring Sites

#### Objective
Define river basins, flood gauge checkpoints, and geographical monitoring zones where sensor hardware is deployed.

#### Prerequisites
* Authenticated as an **Administrator (`ADMIN`)** or **Operator (`OPERATOR`)**.

#### Execution Steps: Adding a New Site
1. From the navigation sidebar, click **Sites** (`/metadata/sites`).
2. Click the primary blue **Add Site** button (`+` icon) in the header.
3. In the **Create Site** modal:
   * **Site Name:** Enter a recognizable station identifier (e.g., `Kelani River - Nagalagam Street Basin`).
   * **Location:** Enter the administrative district or province (e.g., `Colombo North, Western Province`).
4. Click the **Save Site** button.
   * *Expected Outcome:* The modal dismisses, the site table reloads, and the new site appears with an auto-assigned numeric Site ID.

#### Execution Steps: Viewing & Updating Sites
1. **Search:** Use the **Search by ID...** input to isolate a site by its numeric identifier.
2. **View:** Click the **View** icon (eye symbol) on any row to open the read-only site inspector modal.
3. **Edit:** Click the **Edit** icon (pencil symbol) on any row to open the editing dialog, adjust the name or location text, and click **Save Changes**.
4. **Delete:** Click the **Delete** icon (trash can symbol), then confirm the browser alert.

```
+---------------------------------------------------------------------------------------+
| MANAGE SITES                                              [ Search ID ] [+ Add Site ]  |
+---------+-----------------------------------+---------------------------+-------------+
| Site ID | Site Name                         | Location                  | Actions     |
+---------+-----------------------------------+---------------------------+-------------+
| 101     | Kelani River - Nagalagam Street   | Colombo, Western Province | [👁] [✏️] [🗑] |
| 102     | Mahaweli River - Peradeniya Basin | Kandy, Central Province   | [👁] [✏️] [🗑] |
| 103     | Kalu Ganga - Ratnapura Gauge      | Ratnapura, Sabaragamuwa   | [👁] [✏️] [🗑] |
+---------+-----------------------------------+---------------------------+-------------+
```

---

### Workflow 4: Configuring Sensor Types & Categories

#### Objective
Establish standardized environmental telemetry parameters, defining measurement units and hardware categories for physical sensor loggers.

#### Prerequisites
* Authenticated as an **Administrator (`ADMIN`)** or **Operator (`OPERATOR`)**.

#### Execution Steps: Registering a Sensor Type
1. From the navigation sidebar, click **Sensor Types** (`/metadata/sensor-types`).
2. Click the blue **Add Sensor Type** button.
3. In the **Sensor Type Details** dialog:
   * **Type Name:** Enter the physical metric name (e.g., `River Water Level`, `Precipitation Rate`, `Flow Velocity`, `Reservoir Capacity`).
4. Click **Save Sensor Type**.
   * *Expected Outcome:* The modal closes, and the newly defined metric category appears in the list ready to be linked to physical sensors.

---

### Workflow 5: Provisioning Sensor Hardware & 4-Tier Alert Thresholds

#### Objective
Register physical field sensors, map them to river basin sites, define exact GPS coordinates for the GIS map, and calibrate 4-tier alert threshold boundaries.

#### Prerequisites
* Authenticated as an **Administrator (`ADMIN`)** or **Operator (`OPERATOR`)**.
* At least one **Site** and one **Sensor Type** must be configured beforehand.

#### Understanding the 4-Tier Threshold Model
ATLAS implements a dual-sided threshold model that accounts for both upper limits (flooding and reservoir overtopping) and lower limits (drought conditions and minimum environmental river flow):

```
                        [ DANGER: HIGH CRITICAL ]  >= Threshold High Critical (e.g., 8.0m)
-------------------------------------------------
                        [ WARNING: HIGH WARNING ]  >= Threshold High Warning (e.g., 6.0m)
-------------------------------------------------
                        [ NORMAL OPERATING BAND ]  Safe River Flow (e.g., 2.0m to 6.0m)
-------------------------------------------------
                        [ WARNING: LOW WARNING ]   <= Threshold Low Warning (e.g., 2.0m)
-------------------------------------------------
                        [ DANGER: LOW CRITICAL ]   <= Threshold Low Critical (e.g., 1.0m)
```

#### Execution Steps: Registering a Sensor
1. From the navigation sidebar, select **Sensors** (`/metadata/sensors`).
2. Click the blue **Add Sensor** button (`+` icon).
3. Complete the **Sensor Registration Form**:
   * **Sensor ID:** Enter the hardware identifier programmed into the field logger (e.g., `KELANI_STN_01`).
   * **Site:** Select the host river basin site from the dropdown list.
   * **Sensor Type:** Select the measurement category (e.g., `River Water Level`).
   * **Latitude:** Enter the decimal GPS latitude (e.g., `6.9583` for Nagalagam Street).
   * **Longitude:** Enter the decimal GPS longitude (e.g., `79.8736`).
   * **Unit of Measure:** Enter the statutory measurement unit abbreviation (e.g., `m` for meters, `mm` for rainfall). Maximum 8 characters.
4. Calibrate the **Hydrological Threshold Bands**:
   * **Low Critical:** Threshold below which severe drought or sensor starvation occurs (e.g., `1.0`).
   * **Low Warning:** Threshold below which dry weather warning procedures initiate (e.g., `2.0`).
   * **High Warning:** Water level threshold triggering minor flood alerts (e.g., `6.0`).
   * **High Critical:** Water level threshold triggering major flood and evacuation alerts (e.g., `8.0`).
5. Click **Save Sensor**.
   * *Expected Outcome:* The sensor appears in the inventory table, and its GPS pin is immediately projected onto the **GIS Monitor**.

```
+---------------------------------------------------------------------------------------+
| SENSOR INVENTORY MODAL                                                                |
+---------------------------------------------------------------------------------------+
| Hardware Identifier: [ KELANI_STN_01                                                ] |
| River Basin Site:    [ Kelani River - Nagalagam Basin                            [v] ] |
| Measurement Type:    [ River Water Level                                         [v] ] |
| Unit of Measure:     [ m        ]                                                     |
| Latitude:            [ 6.9583   ]             Longitude:        [ 79.8736   ]         |
+---------------------------------------------------------------------------------------+
| Statutory Threshold Boundaries                                                        |
| Low Critical:        [ 1.0      ]             Low Warning:      [ 2.0      ]          |
| High Warning:        [ 6.0      ]             High Critical:    [ 8.0      ]          |
+---------------------------------------------------------------------------------------+
| [ Cancel ]                                                             [ Save Sensor ] |
+---------------------------------------------------------------------------------------+
```

---

### Workflow 6: Real-Time Geospatial (GIS) Telemetry Surveillance

#### Objective
Monitor nationwide hydrological conditions across an interactive map, inspect real-time sensor measurements, verify edge battery health, and analyze multi-day telemetry trends.

#### Prerequisites
* Authenticated with any role (`ADMIN`, `OPERATOR`, or `VIEWER`).

#### Execution Steps: Navigating the Map
1. In the navigation sidebar, click **GIS Monitor** (or navigate to `/gis-dashboard`).
2. Inspect the **Surveillance Map of Sri Lanka**:
   * The map centers automatically over Sri Lanka (`[7.2, 80.6]`) with all active sensors plotted as animated GPS pins.
   * **Marker Color Legend:**
     * 🟢 **Green (Normal):** Telemetry is within statutory safe limits.
     * 🟠 **Orange (High Warning):** Water level has exceeded minor flood thresholds.
     * 🔴 **Red (High Critical):** Major flood threshold breached; emergency risk active.
     * 🟡 **Yellow (Low Warning):** Low river flow / minor water deficit.
     * 🟣 **Purple (Low Critical):** Severe water starvation or extreme drought.
3. Click the **Reset View** crosshair icon at the top-left of the map to re-center the map at default zoom.

#### Execution Steps: Searching and Inspecting a Station
1. In the header search bar, enter a **Station ID** (e.g., `KELANI_STN_01`) or partial name, and press **Enter** or click the search icon.
2. The map executes a smooth aerial zoom animation (`flyTo`), centering directly over the selected sensor.
3. Click on the pulsing map marker to slide open the **Sensor Details Drawer** on the right side of the screen:
   * **Station Identification:** Displays Station Name, Station ID, and Sensor Type.
   * **Battery Level Card:** Shows the remaining battery percentage. A green battery icon indicates healthy status (>25%), while red indicates low battery requiring field replacement.
   * **Sensor Status Badge:** Highlights current status (`NORMAL`, `WARNING`, or `CRITICAL`).
   * **Telemetry Trend Sparkline:** Interactive chart displaying recent water level readings.
   * **Time Range Selector:** Click the segmented buttons (**1H**, **24H**, **7D**, or **30D**) to expand or narrow the historical window.
   * **Threshold Reference Lines:** Horizontal dashed lines show your High Critical, High Warning, Low Warning, and Low Critical limits relative to actual water readings.
   * **Sensor Attributes Summary:** Lists GPS coordinates, Unit of Measure, Site ID, and statutory thresholds.
4. To dismiss the drawer, click the **X** close button in the drawer header or click outside the drawer area.

```
+-------------------------------------------------------+-------------------------------+
| ATLAS Telemetry GIS Monitor    [ Search Station... ]  | Station: KELANI_STN_01    [X] |
+-------------------------------------------------------+ River Water Level             |
| [🎯]                                                  +-------------------------------+
|                                                       | Battery Level   Sensor Status |
|                 (Map of Sri Lanka)                    | [ 🔋 94% ]      [ CRITICAL ]  |
|                                                       +-------------------------------+
|         🔴 KELANI_STN_01                              | Telemetry Trend               |
|                                                       | [ 1H ] [ 24H* ] [ 7D ] [ 30D ]|
|                 🟢 MAHAWELI_02                        |                               |
|                                                       |  8m - - - - - - - (Critical)  |
|                                                       |  6m - - - - - - - (Warning)   |
|                                                       |  4m      /\                   |
|                        🟢 NILWALA_01                  |  2m ----/--\------------------|
|                                                       +-------------------------------+
|                                                       | Attributes:                   |
|                                                       | Latitude: 6.9583  Long: 79.87 |
+-------------------------------------------------------+-------------------------------+
```

---

### Workflow 7: Monitoring & Escalating Active Disaster Alerts

#### Objective
Track critical threshold breaches across the nationwide sensor network, triage emergency events by severity, and evaluate breached measurements against safety ceilings.

#### Prerequisites
* Authenticated with any role (`ADMIN`, `OPERATOR`, or `VIEWER`).

#### How the Alert Engine Works
The ATLAS Alerting Engine continuously evaluates incoming telemetry against configured sensor thresholds:
* **Evaluation Frequency:** Scheduled sliding-window evaluation runs automatically every **60 seconds**.
* **Sliding Window:** Analyzes readings received within the trailing **10-minute window**.
* **Automatic Escalation:** If a river rises from High Warning into High Critical, the existing alert record is dynamically elevated to `HIGH_CRITICAL`.
* **Automatic De-escalation & Healing:** If river levels recede back into the safe operating band, the system automatically marks the alert as `RESOLVED`.
* **Archival Retention:** Resolved alerts are automatically archived and removed from active displays after **24 hours**.

#### Execution Steps: Inspecting Active Alerts
1. In the navigation sidebar, click **Alerts** (`/alerts`).
2. Review the live alerts table. Each row provides:
   * **Severity:** Visual badge indicating urgency:
     * `High Critical` (Red shield icon with pulsing glow)
     * `High Warning` (Orange octagon icon)
     * `Low Warning` (Yellow triangle icon)
     * `Low Critical` (Purple circle icon)
   * **Alert ID:** System-generated tracking UUID (hover to inspect the complete 36-character string).
   * **Sensor ID:** Station reporting the breach.
   * **Measurement:** Actual recorded water level or environmental reading (color-coded to match severity).
   * **Threshold:** The specific statutory limit that was breached.
   * **Time:** Exact date and time the breach occurred.
3. Click the **Refresh** button in the page header to immediately pull the latest alert state from the monitoring engine.
4. If no thresholds are breached, the table displays a green confirmation message: `"No active alerts. System is normal."`

```
+----------------------------------------------------------------------------------------------------+
| ACTIVE ALERTS                                                                      [ 🔄 Refresh ]  |
+---------------+------------------+---------------+-------------+-----------+----------------------+
| Severity      | Alert ID         | Sensor ID     | Measurement | Threshold | Time                 |
+---------------+------------------+---------------+-------------+-----------+----------------------+
| 🛡️ High Crit  | e4b29f01...      | KELANI_STN_01 | 8.4 m       | 8.0 m     | 2026-09-24 14:15     |
| 🛑 High Warn  | c10a8d32...      | MAHAWELI_04   | 6.2 m       | 6.0 m     | 2026-09-24 14:10     |
| ⚠️ Low Warn   | 8fa12c99...      | KALU_GN_02    | 1.9 m       | 2.0 m     | 2026-09-24 13:50     |
+---------------+------------------+---------------+-------------+-----------+----------------------+
```

---

### Workflow 8: Compiling & Exporting Regulatory Incident & Telemetry Reports

#### Objective
Generate, download, and archive audit-ready regulatory reports detailing historical river telemetry or emergency alert incidents.

#### Prerequisites
* Authenticated with any role (`ADMIN`, `OPERATOR`, or `VIEWER`).

#### Execution Steps: Generating a Report
1. In the navigation sidebar, click **Reports** (`/reports`).
2. **Step 1: Select Report Category:**
   * Click the **Telemetry Log** card to compile water level readings, battery health percentages, and hardware performance metrics over time.
   * Click the **Incident Alerts** card to compile threshold breaches, warning severity tiers, and incident durations.
3. **Step 2: Define Timeframe Range:**
   * Click a quick preset button: **Last 24 Hours**, **Last 7 Days**, or **Last 30 Days**.
   * Alternatively, select **Custom Range** and use the **Start Date & Time** and **End Date & Time** calendar pickers to define a specific audit window.
4. **Step 3: Choose Download Format:**
   * Select from the dropdown menu:
     * **Adobe Portable Document Format (`.pdf`):** Formal, printable document complete with government header branding, summary tables, and page numbering.
     * **Comma-Separated Values (`.csv`):** Raw tabular data formatted for ingestion into external GIS software, Microsoft Excel, or statistical toolkits.
5. Click the primary blue **Generate & Download** button.
   * *Expected Outcome:* The button displays a compilation spinner with the text `"Compiling Report..."`. Once compilation completes, the file downloads automatically via your browser, a green success banner appears confirming the filename, and a record is added to your **Recent Downloads** history.

#### Execution Steps: One-Click Re-downloading from History
1. Scroll to the **Recent Downloads** panel on the right side of the `/reports` screen.
2. Review past report generation tasks (stores your last 20 downloaded reports).
3. Click the **Re-download** button next to any past report to retrieve the identical report instantly without re-entering parameters.
4. Click **Clear All** if you wish to reset your local download history log.

```
+-------------------------------------------------------+---------------------------------------+
| SYSTEM REPORTS                                        |                                       |
+-------------------------------------------------------+ Recent Downloads          [ Clear All]|
| 1. Select Report Category                             +---------------------------------------+
|  +------------------------+  +---------------------+  | Telemetry (PDF)             [SUCCESS] |
|  | [*] Telemetry Log      |  | [ ] Incident Alerts |  | Start: 2026-09-01 00:00               |
|  | Water levels & health  |  | Threshold breaches  |  | End:   2026-09-24 23:59               |
|  +------------------------+  +---------------------+  | [ ⬇️ Re-download ]                     |
|                                                       +---------------------------------------+
| 2. Define Timeframe Range                             | Alerts (CSV)                [SUCCESS] |
|  [ 24h ] [ 7d ] [ 30d* ] [ Custom ]                   | Start: 2026-09-17 00:00               |
|  Start: [ 2026-08-25 14:00 ]  End: [ 2026-09-24 14:00 ]| End:   2026-09-24 14:00               |
|                                                       | [ ⬇️ Re-download ]                     |
| 3. Choose Download Format                             |                                       |
|  [ 📄 Adobe Portable Document Format (.pdf)       [v] ]|                                       |
|                                                       |                                       |
| [ ⬇️ Generate & Download (Button) ]                    |                                       |
+-------------------------------------------------------+---------------------------------------+
```

---

## 5. Reporting, Dashboards & Data Exports

### 5.1 Historical Telemetry Audits
Telemetry audits provide a complete historical record of sensor readings, allowing hydrologists and civil defense analysts to model river behavior over time.
* **Fields Included:** Sensor Identifier, River Basin Site, Measurement Reading, Statutory Unit (`m`, `mm`), Battery State of Health (`0-100%`), and ISO-8601 Timestamp.
* **Primary Use Case:** Monsoonal flow analysis, sensor health diagnostics, post-flood inundation reviews.

### 5.2 Disaster Incident Documentation
Disaster incident reports provide official proof of safety ceiling breaches during severe weather events.
* **Fields Included:** Incident UUID, Sensor Identifier, Station Name, Severity Tier (`HIGH_CRITICAL`, `HIGH_WARNING`, `LOW_WARNING`, `LOW_CRITICAL`), Peak Recorded Measurement, Statutory Threshold Ceilings, Incident Onset Timestamp, and Resolution Timestamp.
* **Primary Use Case:** Submitting official event documentation to civil emergency departments, insurance loss assessors, and ministerial inquiry boards.

### 5.3 Supported File Formats & Layouts

| Format | File Extension | Document Features | Recommended Application |
|---|---|---|---|
| **Adobe PDF** | `.pdf` | Structured layout, official header, tabular formatting, threshold breach highlighting | Official ministerial reporting, archived emergency bulletins, printable shift handovers |
| **Comma-Separated Values** | `.csv` | UTF-8 encoded plain text, comma delimited, standardized ISO date headers | Analytical processing in Microsoft Excel, Google Sheets, QGIS, ArcGIS, Python/R modeling |

### 5.4 Download History & One-Click Re-generation
* The application maintains an in-browser audit log of your **last 20 report generation requests**.
* Each history entry captures the category, timeframe, format, timestamp, and status.
* When emergency responders must hand over duties across operational shifts, incoming officers can re-download identical reports instantly with a single click.

---

## 6. Field Device & External System Integration Guide

*(This section is provided for Technical Operators, Municipal IT Teams, and Field Engineers configuring remote telemetry loggers or integrating external emergency alert systems.)*

### 6.1 IoT Edge Telemetry Protocol (MQTT Ingestion)
Remote telemetry stations (ESP32 microcontrollers, Campbell Scientific loggers, cellular RTUs) transmit data into ATLAS via the MQTT protocol.

#### Connection Parameters:
* **Protocol:** MQTT v3.1.1 over TCP
* **Broker Endpoint:** Assigned regional gateway (Default: `broker.hivemq.com` or local gateway)
* **Standard TCP Port:** `1883`
* **Topic Pattern:** `thw/telemetry/<DEVICE_ID>` (e.g., `thw/telemetry/KELANI_STN_01`)
* **Quality of Service (QoS):** `QoS 1` (At least once delivery)

#### Required JSON Telemetry Schema:
```json
{
  "deviceId": "KELANI_STN_01",
  "timestamp": "2026-09-24T14:30:00.000Z",
  "value": 7.42,
  "sensorHealth": 96
}
```

#### Field Validation Requirements:
| Field Name | Type | Constraints | Description |
|---|---|---|---|
| `deviceId` | String | Required, Non-blank | Matches the registered `Sensor ID` in the sensor inventory |
| `timestamp` | String | Required, ISO-8601 UTC Instant | Timestamp when the measurement was taken |
| `value` | Number (Double) | Required, Numeric | Physical metric reading (e.g., water level in meters) |
| `sensorHealth` | Integer | Required, `0` to `100` | Battery state or hardware signal quality percentage |

> [!IMPORTANT]
> The ATLAS ingestion service buffers incoming MQTT streams in an in-memory queue and commits batch inserts every **8 seconds**. Readings appear in analytical dashboards shortly after transmission.

---

### 6.2 External API Integration & REST Contracts
Authorized external systems (e.g., national meteorological servers, municipal alert siren networks) can ingest live alert feeds and sensor metadata programmatically.

#### Base URLs:
* **Gateway API Root:** `http://<portal-host>:8090/`
* **Direct Metadata API:** `http://<portal-host>:8092/`
* **Direct Alerting API:** `http://<portal-host>:8093/`
* **Direct Reporting API:** `http://<portal-host>:8094/`
* **Direct User API:** `http://<portal-host>:8096/`
* **Direct GIS API:** `http://<portal-host>:8097/`

#### Key Public REST Endpoints:
```
GET  /disaster-management/alerts/active
     Retrieves current unhealed emergency alerts. No authentication required.

GET  /metadata-api/sensors
     Retrieves catalog of all registered sensors with GPS coordinates and thresholds.

GET  /disaster-management/telemetry/sensor/{sensorId}?range=24h
     Retrieves time-series data points for a specific sensor (Supported ranges: 1h, 24h, 7d, 30d).

POST /reports/alerts/download
     Compiles and streams an alerts report payload (PDF or CSV).

POST /reports/telemetry/download
     Compiles and streams a telemetry report payload (PDF or CSV).
```

---

### 6.3 Programmatic Authentication & Token Headers
For secured endpoints (such as mutating metadata or managing user accounts), external clients must authenticate via the User Service:

1. **Obtain Token:**
   ```bash
   curl -X POST http://<host>:8096/api/auth/login \
     -H "Content-Type: application/json" \
     -d '{"username": "operator_api", "password": "SecurePassword123"}'
   ```
2. **Response:**
   ```json
   {
     "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
     "refreshToken": "7c9e6679-7425-40de-944b-...",
     "username": "operator_api",
     "role": "OPERATOR"
   }
   ```
3. **Attach Header to Subsequent Requests:**
   ```http
   Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
   ```

---

## 7. Error Handling, Troubleshooting & FAQ

### 7.1 Common User Errors & Solutions

| What You See (Message / Status) | Probable Cause | How to Resolve |
|---|---|---|
| **"Invalid username or password."** | Incorrect username or password entered on the login page. | Double-check caps lock and spelling. Contact an Administrator to reset your password if forgotten. |
| **"This site can't be reached" / `ERR_CONNECTION_REFUSED`** | The backend microservice or central database is temporarily offline or undergoing maintenance. | Click the blue **Reload Page** button on the error screen. If the error persists after 2 minutes, notify IT infrastructure support. |
| **"You do not have permission to edit this user" (403 Forbidden)** | An Operator or Viewer attempted to modify administrative user settings or change account roles. | Role alterations require an Administrator account. Request an Admin to perform user permission changes. |
| **"You cannot delete your own account."** | An Administrator attempted to delete their own currently authenticated user account. | To deactivate an administrative account, have a second Administrator log in and delete the target account. |
| **"Start Date must be earlier than End Date."** | The custom date range filter in Reports has a start time set after the end time. | Adjust the **Start Date & Time** input to precede the **End Date & Time**. |
| **"Please specify both start and end date/time parameters."** | One or both calendar fields were left blank when initiating a custom report download. | Ensure both start and end calendar fields are populated before clicking **Generate & Download**. |
| **"No sensor found matching..."** | The Sensor ID or Station Name entered in the GIS search box does not exist in the inventory. | Verify the exact spelling or Sensor ID in the **Sensors Inventory** (`/metadata/sensors`) table. |
| **Pulsing Marker is Yellow or Grey on Map** | The sensor is transmitting readings, but they fall into low warning or uncalibrated limits. | Click the marker to open the drawer and inspect the battery health and live telemetry sparkline. |
| **Report Download Yields Empty File** | No sensor telemetry or alerts occurred within the selected timeframe. | Broaden the report timeframe (e.g., from `Last 24 Hours` to `Last 7 Days` or `Last 30 Days`). |

---

### 7.2 System Thresholds, Timing & Latency Constraints

To maintain accurate expectations during crisis operations, keep these built-in timing windows in mind:

1. **60-Second Alert Evaluation Interval:** The alert engine evaluates safety thresholds on a **1-minute cron cycle**. A sudden water level spike recorded by a sensor will generate an active alert on the `/alerts` screen within 60 seconds.
2. **8-Second Ingestion Batch Buffer:** Raw field readings transmitted over MQTT are buffered and written in batches every **8 seconds** to optimize database throughput during high-frequency storms.
3. **10-Minute Telemetry Sliding Window:** The alerting engine scans telemetry received in the preceding **10 minutes** to compute current risk states. If a sensor stops transmitting for more than 10 minutes, the alerting service logs a warning indicating no fresh readings are available.
4. **15-Minute Token Lifespan:** Your session access token refreshes automatically in the background. If you leave your browser open on a monitoring screen, it remains active indefinitely as long as your session is refreshed within 24 hours.
5. **24-Hour Resolved Alert Purge:** Resolved alerts remain visible in analytical query logs but are automatically purged from the active operational tables after **24 hours** to keep displays fast and responsive.

---

### 7.3 Escalation & Support Paths

If you experience unresolved platform issues or encounter physical sensor hardware failures in the field:

```
[ Tier 1: Station Operator Assistance ]
  - Assistance with password changes, report compilation, or threshold adjustments.
  - Contact: duty-officer@disaster-management.gov | Internal Ext: 4401

[ Tier 2: Field Instrumentation Engineering ]
  - Assistance with offline physical sensors, dead batteries, damaged solar panels, or MQTT telemetry logger drops.
  - Contact: hydro-field-engineers@disaster-management.gov | Mobile: +94 (11) 234-5678

[ Tier 3: IT Infrastructure & Database Operations ]
  - Assistance with platform outages, ERR_CONNECTION_REFUSED errors, or data persistence issues.
  - Contact: kernelx-support@eng.pdn.ac.lk | Emergency Hot-line: 117
```

---

*ATLAS Hydro Risk Management System — Developed by KernelX Systems Engineering Group for the Department of Computer Engineering, University of Peradeniya.*
