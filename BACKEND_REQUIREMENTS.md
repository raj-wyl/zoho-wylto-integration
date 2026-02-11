# Backend Requirements for Zoho CRM Widget Integration

**Date:** February 2026  
**Integration Type:** Zoho CRM Widget (DoubleTick Model)  
**Status:** Ready for Implementation

---

## Overview

We are building a Zoho CRM widget integration where customers upload a widget ZIP file in Zoho, connect with their Wylto API token once, and then see Wylto chat for each Lead/Contact's phone number directly inside Zoho CRM.

**The widget is already built and ready.** We need the backend team to implement two API endpoints/behaviors to complete the integration.

---

## Requirement 1: Validate-Token API

### Purpose
The widget calls this API when a user clicks "Connect" after entering their Wylto API token. The backend must validate the token and return success/failure so the widget knows whether to save it or show an error.

### Endpoint Details

**URL:**  
```
POST https://server.wylto.com/api/v1/zoho/validate-token
```

**Request:**
- **Method:** `POST`
- **Content-Type:** `application/json`
- **Body:**
  ```json
  {
    "token": "USER_API_TOKEN_STRING"
  }
  ```
- **Token Format:** Long alphanumeric string that users copy from wylto.com → Settings → API Tokens
  - Example format: `Dp7T2tAovWSHdtxbPdvqtSiTXCrCxWEO99NWaVFjZLN3k61eZ2du0IqJBqroSRTBIDfDabA4xr0OkHPx8L6EVDG-sg2w4XT1`
  - No specific prefix required (e.g., no need for `wyl_` prefix)
  - Validate against your existing API token database/store

**Success Response (Token is Valid):**
- **HTTP Status:** `200 OK`
- **Body:** 
  ```json
  {
    "valid": true
  }
  ```
  OR simply `{}` (empty object)
- **Note:** Any HTTP 2xx status code will be treated as success by the widget

**Error Response (Token is Invalid):**
- **HTTP Status:** `4xx` (e.g., `401 Unauthorized` or `400 Bad Request`)
- **Body:**
  ```json
  {
    "valid": false,
    "message": "Invalid token"
  }
  ```
- **Note:** The `message` field is optional but recommended. If provided, the widget will display this message to the user.

### CORS Configuration

**Required:** Allow requests from Zoho CRM domains.

**Allowed Origins:**
- `https://crm.zoho.com`
- `https://crm.zoho.in`
- `https://*.zoho.com` (or specific Zoho domains your customers use)

**Why:** The widget runs inside Zoho CRM's iframe, so requests come from Zoho's domain. Without CORS, the browser will block the API call.

---

## Requirement 2: Inbox URL with Token Support

### Purpose
After the token is validated and saved, the widget loads the Wylto inbox in an iframe with the token and phone number. The inbox must validate the token, authenticate the user, and automatically open the chat for that phone number.

### URL Pattern

```
https://inbox.wylto.com?token={TOKEN}&open_chat={PHONE}
```

### Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `token` | Wylto API token (same token user entered in widget, already validated via Requirement 1) | `Dp7T2tAovWSHdtxbPdvqtSiTXCrCxWEO99NWaVFjZLN3k61eZ2du0IqJBqroSRTBIDfDabA4xr0OkHPx8L6EVDG-sg2w4XT1` |
| `open_chat` | Phone number from Zoho Lead/Contact (digits only, normalized) | `919876543210` |

### Required Behavior

When this URL is loaded (in an iframe inside Zoho CRM):

1. **Read token from URL:** Extract the `token` parameter from the query string
2. **Validate token:** Check the token against your API token store (same validation as Requirement 1)
3. **If valid:**
   - Authenticate the user associated with this token
   - Automatically open/display the chat for the phone number specified in `open_chat`
   - Show the inbox interface normally
4. **If invalid/missing:**
   - Show an error message to the user (e.g., "Invalid token" or "Please connect your token in the widget")
   - Do not show the inbox interface

### Iframe/CORS Configuration

**Required:** Allow the inbox to be embedded in an iframe from Zoho CRM domains.

**Allowed Framing:**
- Allow embedding from: `https://crm.zoho.com`, `https://crm.zoho.in`, and related Zoho domains
- **Do NOT** set `X-Frame-Options: DENY` or CSP headers that block Zoho domains
- The inbox must be accessible inside Zoho's iframe

**Why:** The widget loads the inbox in an iframe inside Zoho CRM. If the inbox blocks iframe embedding, users won't see the chat.

---

## Complete Integration Flow

Here's how everything works together:

1. **User uploads widget ZIP** in Zoho CRM → Widget appears on Lead/Contact records
2. **User opens a Lead** → Sees "Connect Wylto" form in the widget
3. **User enters Wylto API token** → Clicks "Connect" button
4. **Widget calls Requirement 1** (`POST /api/v1/zoho/validate-token`) with the token
5. **Backend validates token** → Returns success (200) or error (4xx)
6. **If success:**
   - Widget saves token in Zoho (using Zoho's storage API)
   - Widget gets the Lead's phone number from Zoho (via Zoho SDK)
   - Widget builds URL: `https://inbox.wylto.com?token={TOKEN}&open_chat={PHONE}`
   - Widget loads Requirement 2 (inbox URL) in an iframe
7. **Backend validates token again** (from URL parameter) → Authenticates user → Opens chat for that phone
8. **User sees Wylto chat** inside Zoho CRM widget

---

## Summary Checklist

| Item | Endpoint/URL | Status |
|------|--------------|--------|
| **Validate-Token API** | `POST /api/v1/zoho/validate-token` | ⏳ To be implemented |
| **CORS for API** | Allow Zoho CRM origins | ⏳ To be configured |
| **Inbox URL params** | Accept `?token=...&open_chat=...` | ⏳ To be implemented |
| **Inbox iframe support** | Allow embedding from Zoho | ⏳ To be configured |
| **Token validation logic** | Check against API token store | ⏳ To be implemented |

---

## Testing

Once implemented, test with:

1. **Validate-Token API:**
   - Valid token → Should return `200` + `{ "valid": true }`
   - Invalid token → Should return `4xx` + `{ "valid": false, "message": "..." }`
   - Test from browser console with CORS enabled

2. **Inbox URL:**
   - Valid token + phone → Should authenticate and open chat
   - Invalid token → Should show error message
   - Test in iframe from Zoho CRM domain

---

## Questions or Clarifications?

If you need any clarification on:
- Token format or validation logic
- CORS configuration
- Error handling
- Testing approach

Please reach out to the integration team.

---

**Widget Status:** ✅ Complete and ready  
**Backend Status:** ⏳ Pending implementation
