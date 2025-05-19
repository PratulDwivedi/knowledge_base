# 🧠 Multi-Tenant SaaS Application

## 🎯 Objective
Build a **scalable, multi-tenant SaaS application** name would be *Appu* with a modern, enterprise-grade UI and modular backend architecture.

---

## 🧩 Tech Stack

- **Frontend:** React + TypeScript
- **UI Framework:** Material UI (or similar modern UI library)
- **Backend:** API-first architecture with environment-based config
- **Environments:** `local`, `staging`, `production`
- **Auth:** Token-based authentication (Bearer Token)
- **Architecture Goals:** Clean, scalable, maintainable

---

## ✅ Tasks & Structure

### 🔧 Task 1: Application Configuration (`app.config.ts`)
- Create a global configuration file based on `APP_ENV`.
- Must export environment-aware constants.

```ts
APP_NAME = "Tech Techi"
APP_VERSION = "0.0.1"
APP_BASE_URL = "https://app.assetinfinity.ai"
API_BASE_URL = "https://api.assetinfinity.ai"
```

- Config values must be accessible throughout the app.

---

### 📁 Task 2: Project Structure & Naming Standards

```
src/
├── pages/                  # All page components
├── services/               # API logic per page/module
├── models/                 # TypeScript interfaces
├── components/
│   ├── common/             # Shared components
│   └── [PageName]/         # Page-specific components
├── helpers/
│   └── ApiHelper.ts        # Central API utility
└── config/
    └── app.config.ts       # Environment-based config
```

#### Pages
- Naming: `PageNamePage.tsx` (e.g., `LoginPage.tsx`, `DashboardPage.tsx`)

#### Services
- Naming: `PageNameService.ts`
- Use dynamic `API_BASE_URL` from config

#### Models
- Place all interfaces in `src/models/`
- Organize per module for reusability

#### Components
- Page-specific → `src/components/PageName/`
- Shared/common → `src/components/common/` or `shared/`

---

### 🌐 Task 3: API Endpoint & Helper

#### `ApiHelper.ts`
- Central utility for `GET`, `POST`, `PUT`, `DELETE` methods
- Auto-include Bearer token if authenticated
- Graceful fallback for unauthenticated requests

#### API Endpoint Constants
- Group by module
- Declare in `ApiHelper.ts` as constants

---

### 🧱 Task 4: Layouts

#### `PublicLayoutPage.tsx`
- Handles public routes (e.g., Login, Register, 404)
- Displays `APP_NAME` at the top
- Clean, minimal layout

#### `ProtectedLayoutPage.tsx`
- Wraps all private/authenticated routes (e.g., Dashboard)
- Features:
  - Collapsible sidebar with **n-level treeview**
    - Folder icons for groups
    - Page icons for leaf nodes
  - `APP_NAME` displayed at **top-right** inside sidebar
  - Main content adjusts with sidebar state

---

## 🌟 Optional Enhancements

- ✅ Multi-Tenant Theming: Allow branding overrides per tenant
- ✅ Role-Based Access: Show/hide pages in sidebar tree based on role
- ✅ Theming: Use MUI theme provider for light/dark mode, tenant brand
- ✅ Error Boundaries: Wrap the app in a global error handler

# ⚙️ AI App Builder Prompt: DynamicPage Enhancements & Backend Functions

## 🎯 Objective
Update the `DynamicPage` component to render controls dynamically based on metadata, enforce visibility modes, and wire up backend functions for tenants, files, and chat management.

---

## 🧱 Frontend: DynamicPage Component Updates

### ✅ 1. Remove Unnecessary UI
- **Remove** the placeholder text and leading icon from the Page Section Control.

---

### 🧪 2. Dynamic Control Rendering

Render input controls based on the following `control_type_id`. The control metadata includes:

```json
[
  {"control_type_id": 1, "control_type_name": "Alpha Numeric", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 2, "control_type_name": "Alpha Only", "json_data_type": "string", "db_data_type": "date"},
  {"control_type_id": 3, "control_type_name": "Email", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 4, "control_type_name": "Date", "json_data_type": "string", "db_data_type": "date"},
  {"control_type_id": 5, "control_type_name": "Date & Time", "json_data_type": "string", "db_data_type": "datetime"},
  {"control_type_id": 6, "control_type_name": "Password", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 7, "control_type_name": "Phone Number", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 8, "control_type_name": "Integer", "json_data_type": "number", "db_data_type": "integer"},
  {"control_type_id": 9, "control_type_name": "Decimal", "json_data_type": "number", "db_data_type": "decimal"},
  {"control_type_id": 10, "control_type_name": "Checkbox", "json_data_type": "boolean", "db_data_type": "boolean"},
  {"control_type_id": 11, "control_type_name": "Dropdown Multiselect", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 12, "control_type_name": "Dropdown", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 13, "control_type_name": "Text Area", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 14, "control_type_name": "File Upload", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 15, "control_type_name": "Hyperlink", "json_data_type": "string", "db_data_type": "text"},
  {"control_type_id": 16, "control_type_name": "Submit", "json_data_type": "string", "db_data_type": "text"}
]
```

Ensure each control renders the appropriate UI element (e.g., input, select, checkbox, textarea, file upload).

---

### 👁 3. Display Modes

Each control must support one of the following `display_mode` values:

- `require`: Standard input (mandatory)
- `visible`: Standard input (optional)
- `none`: Do **not** render (hidden)
- `readonly`: Show **label + value** using a custom display component (no input element)

> ⚠️ Note: Do **not** use disabled input fields for `readonly`, as they may truncate text or be non-scrollable.
