# User Connection Workflow - Wylto Zoho Widget

**Visual step-by-step flow of how users connect and use the widget**

---

## Complete User Journey

```
┌─────────────────────────────────────────────────────────────────┐
│                    USER CONNECTION WORKFLOW                      │
└─────────────────────────────────────────────────────────────────┘

STEP 1: PREPARATION
───────────────────
User has:
  ✓ Zoho CRM account (admin access)
  ✓ Wylto account
  ✓ Widget ZIP file (wylto-widget.zip)
  ✓ Wylto API token (from wylto.com → Settings → API Tokens)

                    ↓

STEP 2: INSTALL WIDGET IN ZOHO
───────────────────────────────
1. Login to Zoho CRM
2. Go to: Setup → Developer Hub → Widgets
3. Click: "Create New Widget"
4. Fill form:
   • Name: "Wylto WhatsApp Chat"
   • Type: Related List
   • Hosting: Zoho
   • Upload: wylto-widget.zip
   • Index Page: /widget.html
5. Click: Save

                    ↓

STEP 3: ADD WIDGET TO LAYOUTS
──────────────────────────────
1. Go to: Setup → Customization → Layouts
2. Select: Leads → Layouts → [Your Layout]
3. Add: "Wylto WhatsApp Chat" to Related List section
4. Repeat for: Contacts (if needed)
5. Click: Save

                    ↓

STEP 4: FIRST-TIME CONNECTION
─────────────────────────────
1. Open any Lead or Contact record in Zoho CRM
   
                    ↓

2. In the left sidebar, find "Related List" section
   • Look for: "Wylto WhatsApp Chat"
   
                    ↓

3. Click on "Wylto WhatsApp Chat"
   
                    ↓

4. Widget loads → Shows "Connect Wylto" form
   ┌─────────────────────────────────────┐
   │      Connect Wylto                  │
   │                                     │
   │  Enter your Wylto API token         │
   │  (from wylto.com → Settings →       │
   │   API Tokens)                       │
   │                                     │
   │  [Token Input Field]                │
   │  Paste your API token from wylto.com│
   │                                     │
   │  [Connect Button]                   │
   └─────────────────────────────────────┘

                    ↓

5. User enters their Wylto API token
   • Copies token from wylto.com → Settings → API Tokens
   • Pastes into the token input field
   
                    ↓

6. User clicks "Connect" button
   
                    ↓

7. Widget calls backend API:
   POST https://server.wylto.com/api/v1/zoho/validate-token
   Body: { "token": "USER_TOKEN_HERE" }
   
                    ↓

8. Backend validates token
   ┌─────────────────────────────────────┐
   │  Backend checks:                     │
   │  • Token exists in database?         │
   │  • Token is valid/active?            │
   │  • Token belongs to valid account?   │
   └─────────────────────────────────────┘
   
                    ↓

9. Backend responds:
   ✓ SUCCESS: HTTP 200 + { "valid": true }
   ✗ ERROR: HTTP 4xx + { "valid": false, "message": "..." }
   
                    ↓

10. Widget shows result:
    ┌─────────────────────────────────────┐
    │  ✓ SUCCESS:                         │
    │  "Connected! You can now view       │
    │   chats on Lead/Contact records."   │
    │                                     │
    │  ✗ ERROR:                           │
    │  "Invalid token. Check your token   │
    │   and try again."                    │
    └─────────────────────────────────────┘

                    ↓

11. If SUCCESS:
    • Widget saves token in Zoho (using Zoho's storage API)
    • Token is stored per Zoho organization (once per org)
    • Widget switches to "chat screen"

                    ↓

STEP 5: USING THE WIDGET (After Connection)
───────────────────────────────────────────
1. User opens any Lead or Contact record
   
                    ↓

2. User clicks "Wylto WhatsApp Chat" in Related List
   
                    ↓

3. Widget checks for saved token
   ┌─────────────────────────────────────┐
   │  Widget reads:                       │
   │  • Saved token from Zoho storage     │
   │  • Current Lead/Contact record      │
   └─────────────────────────────────────┘
   
                    ↓

4. Widget gets phone number from record
   • Reads: Lead.Phone OR Lead.Mobile
   • Normalizes: Removes spaces, keeps digits only
   • Example: "+91 98765 43210" → "919876543210"
   
                    ↓

5. Widget builds inbox URL:
   https://inbox.wylto.com?token={SAVED_TOKEN}&open_chat={PHONE}
   
                    ↓

6. Widget loads inbox in iframe
   ┌─────────────────────────────────────┐
   │  [Wylto WhatsApp Chat Widget]       │
   │  ┌───────────────────────────────┐ │
   │  │                               │ │
   │  │   Wylto Inbox (iframe)        │ │
   │  │   • Validates token from URL  │ │
   │  │   • Authenticates user        │ │
   │  │   • Opens chat for phone      │ │
   │  │                               │ │
   │  └───────────────────────────────┘ │
   └─────────────────────────────────────┘
   
                    ↓

7. Backend (inbox) validates token from URL
   • Reads token from ?token= parameter
   • Validates against API token store
   • If valid: Authenticates user → Opens chat
   • If invalid: Shows error message
   
                    ↓

8. User sees Wylto chat inside Zoho CRM
   • Chat interface loads
   • Messages for that phone number display
   • User can send/receive messages
   • All within Zoho CRM interface

                    ↓

STEP 6: SUBSEQUENT USES
───────────────────────
• User opens different Lead/Contact
• Clicks "Wylto WhatsApp Chat"
• Widget automatically:
  ✓ Uses saved token (no need to reconnect)
  ✓ Gets phone from current record
  ✓ Loads chat for that phone
• No token entry needed again!

```

---

## Key Points

### One-Time Setup (Steps 1-4)
- User installs widget ZIP in Zoho (once)
- User adds widget to layouts (once)
- User connects token (once per Zoho organization)

### Daily Usage (Steps 5-6)
- User opens Lead/Contact
- Clicks widget
- Chat loads automatically (no token entry needed)

### Token Storage
- Token is saved **per Zoho organization** (not per user)
- All users in that Zoho org share the same token
- Token persists until user changes it (requires re-connection)

### Error Handling
- **Invalid token:** Widget shows error, user can retry
- **No phone number:** Widget shows message to add phone
- **Backend down:** Widget shows connection error

---

## Visual Timeline

```
Day 1: Setup
├─ Install widget ZIP in Zoho (5 min)
├─ Add to layouts (2 min)
└─ Connect token (1 min)
   Total: ~8 minutes

Day 2+: Daily Use
├─ Open Lead/Contact
└─ Click widget → Chat loads instantly
   Total: ~2 seconds per record
```

---

## User States

### State 1: Not Connected
- Widget shows "Connect Wylto" form
- User must enter token and click Connect

### State 2: Connected
- Widget shows chat interface
- Token is saved, no re-entry needed
- Chat loads automatically for each record

### State 3: Error
- Widget shows error message
- User can retry connection
- Common errors: Invalid token, No phone number, Backend unavailable

---

## Success Indicators

✅ **Widget installed:** Appears in Setup → Widgets list  
✅ **Widget added:** Shows in Lead/Contact Related List  
✅ **Token connected:** "Connect" form disappears, chat loads  
✅ **Chat working:** Inbox loads with messages for the phone number  

---

**End of Workflow**
