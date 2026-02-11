# Wylto Zoho CRM Widget - User Setup Guide

**Version:** 1.0  
**Last Updated:** February 2026

---

## Overview

This guide will help you install and set up the Wylto WhatsApp Chat widget in your Zoho CRM account. Once set up, you'll be able to view and manage Wylto chats directly from Lead and Contact records in Zoho CRM.

**What you'll need:**
- Zoho CRM account with admin/setup permissions
- Wylto account with an API token (from wylto.com → Settings → API Tokens)
- The Wylto widget ZIP file (provided by Wylto support or download link)

---

## Step 1: Get Your Wylto API Token

1. Log in to your **Wylto account** at [wylto.com](https://wylto.com)
2. Go to **Settings** → **API Tokens**
3. Copy your **API token** (it's a long alphanumeric string)
   - Keep this token secure—you'll need it in Step 4
   - Example format: `Dp7T2tAovWSHdtxbPdvqtSiTXCrCxWEO99NWaVFjZLN3k61eZ2du0IqJBqroSRTBIDfDabA4xr0OkHPx8L6EVDG-sg2w4XT1`

---

## Step 2: Download the Widget ZIP File

1. Download the **Wylto widget ZIP file** from:
   - Support email/link provided by Wylto, OR
   - Wylto documentation/download page
2. Save the file to your computer (e.g., `wylto-widget.zip`)

---

## Step 3: Upload Widget in Zoho CRM

1. **Log in to Zoho CRM** with an account that has **admin** or **setup** permissions

2. Go to **Setup** (gear icon in top right) → **Developer Hub** → **Widgets**

3. Click **Create New Widget** (or **Create your First Widget**)

4. Fill in the widget details:
   - **Name:** Enter `Wylto WhatsApp Chat` (or any name you prefer)
   - **Description:** (Optional) e.g., "Wylto chat on Lead/Contact"
   - **Type:** Select **Related List**
   - **Hosting:** Select **Zoho**
   - **File Upload:** Click **Upload** and select the `wylto-widget.zip` file you downloaded
   - **Index Page:** Enter `/widget.html` (must start with `/`)
   - **Mobile Compatibility:** (Optional) Enable if you want mobile support

5. Click **Save**

---

## Step 4: Add Widget to Lead and Contact Layouts

### For Leads:

1. Go to **Setup** → **Customization** → **Modules and Fields** (or **Layouts**)
2. Click **Leads** → **Layouts**
3. Select the layout you want to modify (e.g., "Standard Layout")
4. Find the **Related List** section (or widget section)
5. Click **Add Related List** or drag **Wylto WhatsApp Chat** into the layout
6. Click **Save**

### For Contacts:

1. Repeat the same steps above, but select **Contacts** instead of **Leads**
2. Add **Wylto WhatsApp Chat** to the Contact layout
3. Click **Save**

---

## Step 5: Connect Your Wylto Token (First Time Setup)

1. **Open any Lead or Contact** record in Zoho CRM

2. Scroll down to find the **Wylto WhatsApp Chat** section (in Related List area)

3. Click on **Wylto WhatsApp Chat** in the left sidebar (Related List)

4. You'll see a **"Connect Wylto"** form:
   - Enter your **Wylto API token** (the one you copied in Step 1)
   - Click **Connect**

5. **Wait for validation:**
   - The widget will validate your token
   - If successful, you'll see: "Connected! You can now view chats on Lead/Contact records."
   - If there's an error, check your token and try again

6. **Done!** Your token is now saved. You only need to do this once per Zoho CRM organization.

---

## Step 6: Using the Widget

After connecting your token:

1. **Open any Lead or Contact** record that has a phone number

2. Click on **Wylto WhatsApp Chat** in the Related List

3. **The widget will automatically:**
   - Get the phone number from the Lead/Contact record
   - Load the Wylto chat for that phone number
   - Display the chat interface inside Zoho CRM

4. **If a record has no phone number:**
   - The widget will show: "This record has no phone number. Add a phone number to see the WhatsApp chat."
   - Add a phone number to the Lead/Contact, then refresh the widget

---

## Troubleshooting

### Widget doesn't appear on Lead/Contact records

- **Check:** Did you add the widget to the layout? (Step 4)
- **Solution:** Go back to Setup → Layouts and make sure "Wylto WhatsApp Chat" is added to the Related List section

### "Connect Wylto" form keeps appearing

- **Check:** Did you successfully connect your token? (Step 5)
- **Solution:** Make sure you clicked "Connect" and saw a success message. If you see an error, check your API token.

### Chat doesn't load / Shows error

- **Check:** Does the Lead/Contact have a phone number?
- **Solution:** Add a phone number (Phone or Mobile field) to the record and refresh

### "Invalid token" error

- **Check:** Is your API token correct?
- **Solution:** 
  1. Go to wylto.com → Settings → API Tokens
  2. Copy the token again
  3. In Zoho, click on "Wylto WhatsApp Chat" → Re-enter the token → Connect

### Widget shows blank/white screen

- **Check:** Is the Index Page set to `/widget.html`? (Step 3)
- **Solution:** Go to Setup → Widgets → Edit "Wylto WhatsApp Chat" → Check Index Page is `/widget.html` (with leading slash)

---

## Updating the Widget

If Wylto releases a new version of the widget:

1. Download the new `wylto-widget.zip` file
2. Go to **Setup** → **Widgets** → **Wylto WhatsApp Chat** → **Edit**
3. Click **Change** next to the file upload
4. Upload the new ZIP file
5. Click **Save**
6. Refresh your Zoho CRM page

**Note:** Your token connection will remain—you don't need to reconnect.

---

## Support

If you encounter issues:

1. **Check this guide** for common troubleshooting steps
2. **Contact Wylto support** with:
   - Your Zoho CRM organization ID
   - Screenshot of the error (if any)
   - Steps you've already tried

---

## Frequently Asked Questions

**Q: Do I need to connect the token on every Lead/Contact?**  
A: No, you only connect once per Zoho CRM organization. After that, the widget automatically uses your saved token.

**Q: Can I use different tokens for different Leads?**  
A: No, the widget uses one token per Zoho organization. All chats will be associated with the account that owns that token.

**Q: What if I want to change my token?**  
A: You'll need to contact Wylto support or your admin to update the token in the widget (this may require re-uploading the widget or a token refresh feature).

**Q: Does this work on mobile?**  
A: It depends on your Zoho CRM mobile app and widget settings. Enable "Mobile Compatibility" when creating the widget if you want to try mobile support.

**Q: Can I remove the widget later?**  
A: Yes, go to Setup → Widgets → Find "Wylto WhatsApp Chat" → Delete. Make sure to remove it from layouts first.

---

**Congratulations!** You've successfully set up the Wylto WhatsApp Chat widget in Zoho CRM. You can now view and manage Wylto chats directly from your Lead and Contact records.
