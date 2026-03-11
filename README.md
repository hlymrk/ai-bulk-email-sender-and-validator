<image alt="Logo" src="https://images.apifyusercontent.com/Fh5iNz5iTDDEdnQqWcJxte26jcT09IaClvWs1a22TXE/cb:1/aHR0cHM6Ly9hcGlmeS1pbWFnZS11cGxvYWRzLXByb2QuczMudXMtZWFzdC0xLmFtYXpvbmF3cy5jb20vV3VHSG9BR3lTUFlUY3kzTHotYWN0b3Itb0pSSHNYNk14RE5MbjNQT0ItRENZaFhiZlkydi1sb2dvLnBuZw.png" width="200" height="200"></image>

# **AI Bulk Email Sender & Validator**

Professional email automation with AI-powered composition, advanced validation, and multi-provider support.

Send personalized bulk emails at scale with AI technology and comprehensive email validation. This Apify Actor combines 200+ AI models, advanced email validation, and support for major email providers in one integrated solution.

## **Legal and Ethical Use**

**This Actor must be used only for legitimate business purposes and in compliance with applicable laws and platform policies.**

### **Important Guidelines**

1. **Permission Requirements**: Only send emails to recipients who have explicitly opted in and granted permission. Sending unsolicited emails may violate anti-spam laws such as the CAN-SPAM Act (USA), GDPR (Europe), CASL (Canada), and similar regulations in other jurisdictions.

2. **Unsubscribe Links**: We strongly recommend including unsubscribe links in all email campaigns. This:
    - Fulfills legal requirements under CAN-SPAM and similar regulations
    - Improves sender reputation and deliverability
    - Prevents complaints and blocks
    - Demonstrates compliance with email marketing best practices

3. **Sender Reputation**: Maintain your sender reputation by:
    - Using consistent sender addresses verified with your email provider
    - Monitoring bounce rates and unsubscribe rates
    - Respecting rate limits and provider guidelines
    - Avoiding triggering spam filters through best practices

4. **Data Protection**: Protect recipient data in accordance with:
    - GDPR (for EU residents)
    - CCPA (for California residents)
    - PIPEDA (for Canadian residents)
    - Applicable data protection laws in your jurisdiction

5. **Platform Compliance**: This service runs on the Apify platform. All usage must comply with [Apify's Acceptable Use Policy](https://docs.apify.com/legal/acceptable-use-policy), which prohibits:
    - Spam and unsolicited bulk emails
    - Campaigns that violate applicable laws
    - Phishing, deception, or fraud
    - Harassment or abuse

Violations of these policies may result in account suspension and legal action. Use this Actor responsibly and ethically.

---

## **How It Works**

This Actor operates in a straightforward pipeline:

1. **Input Reception**: Receives your configuration and recipient list
2. **Email Validation**: Validates recipient addresses at the specified level (format, DNS, or SMTP)
3. **AI Composition** (Optional): Generates email content using AI if enabled
4. **Template Processing**: Substitutes recipient-specific variables into email content
5. **Batch Sending**: Sends emails via your chosen provider with rate limiting
6. **Result Logging**: Outputs comprehensive results for each email

The entire process respects rate limits, provider constraints, and validation caching for optimal performance.

---

## **Input Specification**

The Actor accepts a comprehensive JSON configuration object. All required fields must be provided.

### **Input Schema Overview**

```json
{
    "title": "AI Bulk Email Sender Input",
    "type": "object",
    "schemaVersion": 1,
    "properties": {
        "emailProvider": "string (required)",
        "providerConfig": "object (required)",
        "fromEmail": "string (required)",
        "recipients": "array (required)",
        "validateEmails": "boolean",
        "validationLevel": "string",
        "useAiComposer": "boolean",
        "subject": "string",
        "htmlBody": "string",
        "textBody": "string"
    }
}
```

### **Required Fields**

#### **emailProvider**

**Type**: String  
**Allowed Values**: `"smtp"`, `"sendgrid"`, `"resend"`, `"ses"`, `"mailgun"`  
**Default**: `"smtp"`

Specifies which email service provider to use for sending.

```json
{
    "emailProvider": "sendgrid"
}
```

#### **providerConfig**

**Type**: Object  
**Required**: Yes

Provider-specific configuration. Structure varies by provider.

**SMTP Configuration:**

```json
{
    "providerConfig": {
        "smtpHost": "smtp.gmail.com",
        "smtpPort": 587,
        "smtpUser": "your-email@gmail.com",
        "smtpPassword": "your-app-password"
    }
}
```

**SendGrid Configuration:**

```json
{
    "providerConfig": {
        "apiKey": "SG.your-sendgrid-api-key"
    }
}
```

**Resend Configuration:**

```json
{
    "providerConfig": {
        "apiKey": "re_your-resend-api-key"
    }
}
```

**AWS SES Configuration:**

```json
{
    "providerConfig": {
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "secretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
        "region": "us-east-1"
    }
}
```

**Mailgun Configuration:**

```json
{
    "providerConfig": {
        "apiKey": "key-1234567890abcdef",
        "domain": "mg.yourdomain.com"
    }
}
```

#### **fromEmail**

**Type**: String  
**Pattern**: Valid email address  
**Required**: Yes

The sender's email address. This address must be verified with your chosen email provider.

```json
{
    "fromEmail": "sender@example.com"
}
```

#### **fromName**

**Type**: String  
**Default**: Not set

The display name for the sender (optional).

```json
{
    "fromName": "John Doe"
}
```

#### **recipients**

**Type**: Array of Objects  
**Required**: Yes  
**Max Items**: 10,000

List of email recipients with optional personalization data.

```json
{
    "recipients": [
        {
            "email": "john@company.com",
            "name": "John Doe",
            "customFields": {
                "company": "Acme Corp",
                "position": "CEO",
                "product": "Enterprise Suite"
            }
        },
        {
            "email": "jane@startup.com",
            "name": "Jane Smith",
            "customFields": {
                "company": "TechStart Inc",
                "position": "Founder",
                "product": "Starter Plan"
            }
        }
    ]
}
```

Each recipient object contains:

- `email` (required): Valid email address
- `name` (optional): Recipient's name for personalization
- `customFields` (optional): Object with custom variables for template substitution

#### **validateEmails**

**Type**: Boolean  
**Default**: `true`

Whether to validate email addresses before sending. Highly recommended.

#### **validationLevel**

**Type**: String  
**Allowed Values**: `"format"`, `"dns"`, `"smtp"`  
**Default**: `"dns"`

Specifies email validation depth:

- `"format"`: Only checks RFC 5322 format (fastest, ~95% accuracy)
- `"dns"`: Validates format and checks DNS/MX records (recommended, ~99% accuracy)
- `"smtp"`: Performs SMTP connection check (most accurate but slow, may trigger spam filters)

```json
{
    "validationLevel": "dns"
}
```

#### **enableTemplateVariables**

**Type**: Boolean  
**Default**: `true`

Enables replacing `{{variable}}` placeholders with recipient data.

### **Optional AI Fields**

#### **useAiComposer**

**Type**: Boolean  
**Default**: `false`

Enable AI-powered email composition. When enabled, the Actor generates email content based on your prompt and context.

#### aiMode

**Type**: String  
**Allowed Values**: `"openrouter"`, `"byok"`  
**Default**: `"openrouter"`

Selects the AI integration mode:

- `"openrouter"`: Use OpenRouter (200+ models, zero setup)
- `"byok"`: Bring Your Own Key (direct API access)

#### **openrouterModel**

**Type**: String  
**Default**: `"openai/gpt-4o-mini"`

AI model to use when `aiMode` is `"openrouter"`. Recommended models:

- `"openai/gpt-4o-mini"` - Fast, affordable
- `"openai/gpt-4o"` - Better quality
- `"anthropic/claude-3-5-sonnet"` - Superior writing
- `"google/gemini-flash-1.5"` - Fast and capable

#### **aiPrompt**

**Type**: String

Instructions for the AI to generate email content.

```json
{
    "aiPrompt": "Write a professional follow-up email thanking them for attending our product demo. Mention key benefits and ask for their thoughts."
}
```

#### **aiContext**

**Type**: String

Background information for the AI about your company, product, or campaign context.

```json
{
    "aiContext": "We are a SaaS company offering project management tools. Our product helps teams collaborate better, manage projects efficiently, and improve productivity by 40%."
}
```

#### **personalizePerRecipient**

**Type**: Boolean  
**Default**: `false`

Generate unique email content for each recipient using their `customFields`. Increases AI costs but produces highly personalized emails.

### **Email Content Fields**

#### **subject**

**Type**: String

Email subject line. Supports template variables if `enableTemplateVariables` is `true`.

```json
{
    "subject": "Hello {{name}}, follow-up on your {{product}} interest"
}
```

#### **htmlBody**

**Type**: String

HTML email content. Supports template variables.

```json
{
    "htmlBody": "<h1>Hi {{name}},</h1><p>Thank you for your interest in {{product}} at {{company}}.</p>"
}
```

#### **textBody**

**Type**: String

Plain text email content as fallback. Supports template variables.

```json
{
    "textBody": "Hi {{name}},\n\nThank you for your interest in {{product}} at {{company}}."
}
```

### **Performance and Cache Fields**

#### **validationCacheEnabled**

**Type**: Boolean  
**Default**: `true`

Enable validation result caching. Dramatically speeds up repeated campaigns with overlapping recipients.

#### **validationCacheTTL**

**Type**: Integer  
**Default**: `24`  
**Range**: 1-168 hours

How long to cache validation results (in hours).

#### **batchSize**

**Type**: Integer  
**Default**: `10`  
**Range**: 1-100

Number of emails to process per batch before applying delay. Affects rate limiting and provider compliance.

#### **delayBetweenBatches**

**Type**: Integer  
**Default**: `1000`  
**Range**: 0-60000 milliseconds

Delay between batches in milliseconds. Prevents rate limiting and respects provider constraints.

#### **dryRun**

**Type**: Boolean  
**Default**: `false`

Test mode: validates and composes emails without actually sending. Useful for testing configurations.

### **Complete Input Example**

```json
{
    "emailProvider": "sendgrid",
    "providerConfig": {
        "apiKey": "SG.your-api-key"
    },
    "fromEmail": "campaigns@example.com",
    "fromName": "Our Marketing Team",
    "replyTo": "support@example.com",
    "recipients": [
        {
            "email": "john@company.com",
            "name": "John Doe",
            "customFields": {
                "company": "Acme Corp",
                "position": "CEO"
            }
        },
        {
            "email": "jane@startup.com",
            "name": "Jane Smith",
            "customFields": {
                "company": "TechStart Inc",
                "position": "Founder"
            }
        }
    ],
    "useAiComposer": true,
    "aiMode": "openrouter",
    "openrouterModel": "anthropic/claude-3-5-sonnet",
    "aiPrompt": "Write a professional follow-up thank you email after a product demo",
    "aiContext": "We are a SaaS company providing project management solutions. Our key benefits include team collaboration, timeline management, and productivity tracking.",
    "personalizePerRecipient": true,
    "validateEmails": true,
    "validationLevel": "dns",
    "enableTemplateVariables": true,
    "validationCacheEnabled": true,
    "validationCacheTTL": 24,
    "batchSize": 10,
    "delayBetweenBatches": 1000,
    "dryRun": false
}
```

---

## **Output Specification**

The Actor outputs results to a dataset with comprehensive information about each recipient and their email processing status.

### **Output Dataset Schema**

Each item in the output dataset represents the complete processing result for one recipient.

#### **Output Object Structure**

```json
{
    "email": "string",
    "name": "string | null",
    "validationStatus": "string",
    "deliveryStatus": "string",
    "validationResult": "object | null",
    "messageId": "string | null",
    "composedSubject": "string | null",
    "composedHtmlBody": "string | null",
    "composedTextBody": "string | null",
    "sentAt": "string (ISO 8601) | null",
    "suggestedCorrection": "string | null",
    "error": "string | null",
    "batchNumber": "integer"
}
```

### **Output Fields Explained**

#### **email**

**Type**: String

The recipient's email address.

#### **name**

**Type**: String or Null

The recipient's display name, if provided in input.

#### **validationStatus**

**Type**: String  
**Possible Values**:

- `"valid"` - Email passed all validation checks
- `"invalid_format"` - Email format is invalid
- `"typo_detected"` - Possible typo detected (e.g., `gmaill.com`)
- `"disposable"` - Temporary/disposable email service
- `"no_mx_records"` - Domain has no MX records
- `"mailbox_not_found"` - SMTP validation indicates mailbox doesn't exist
- `"greylisted"` - Temporarily unavailable (retry later)
- `"skipped"` - Validation was skipped (dry run or validateEmails=false)

#### **deliveryStatus**

**Type**: String  
**Possible Values**:

- `"sent"` - Email successfully sent via provider
- `"skipped"` - Email was not sent (validation failed or dry run)
- `"failed"` - Email sending failed
- `"queued"` - Email is queued for sending
- `"bounced"` - Email bounced (hard or soft)

#### **validationResult**

**Type**: Object or Null

Detailed validation information. Null if validation was not performed or failed.

```json
{
    "validationResult": {
        "status": "valid",
        "formatValid": true,
        "dnsValid": true,
        "smtpValid": true,
        "mxRecords": [
            "alt2.gmail-smtp-in.l.google.com",
            "alt4.gmail-smtp-in.l.google.com",
            "gmail-smtp-in.l.google.com",
            "alt1.gmail-smtp-in.l.google.com",
            "alt3.gmail-smtp-in.l.google.com"
        ],
        "details": {
            "regex": true,
            "typo": true,
            "disposable": true,
            "mx": true,
            "smtp": false
        }
    }
}
```

**Sub-fields:**

- `status`: Overall validation status
- `formatValid`: RFC 5322 format compliance
- `dnsValid`: DNS/MX record validation result
- `smtpValid`: SMTP connection result (if attempted)
- `mxRecords`: Array of MX records for the domain
- `details`: Granular validation details for each check

#### **messageId**

**Type**: String or Null

Unique message ID returned by the email provider. Used to track this specific email with the provider. Null if email was not sent.

#### **composedSubject**

**Type**: String or Null

The final email subject line used. For AI-composed emails, this is the AI-generated subject. For templates, this is after variable substitution. Null if email was not sent.

#### **composedHtmlBody**

**Type**: String or Null

The final HTML email body sent. Null if only text was sent or email was not sent.

#### **composedTextBody**

**Type**: String or Null

The final plain text email body sent. Null if only HTML was sent or email was not sent.

#### **sentAt**

**Type**: String (ISO 8601 timestamp) or Null

UTC timestamp when the email was successfully sent. Format: `"2026-03-02T10:30:00.000Z"`. Null if not sent.

#### **suggestedCorrection**

**Type**: String or Null

If a typo was detected in the email address, this field contains the suggested correction (e.g., `"johndoe@gmail.com"` if input was `"johndoe@gmaill.com"`). Null if no typo detected.

#### **error**

**Type**: String or Null

Error message if email processing or sending failed. Null if successful.

#### **batchNumber**

**Type**: Integer

Which batch this email belonged to (relevant for rate limiting and retry logic).

### **Sample Output Item**

```json
{
    "email": "john@company.com",
    "name": "John Doe",
    "validationStatus": "valid",
    "deliveryStatus": "sent",
    "validationResult": {
        "status": "valid",
        "formatValid": true,
        "dnsValid": true,
        "smtpValid": true,
        "mxRecords": [
            "alt2.gmail-smtp-in.l.google.com",
            "alt4.gmail-smtp-in.l.google.com",
            "gmail-smtp-in.l.google.com",
            "alt1.gmail-smtp-in.l.google.com",
            "alt3.gmail-smtp-in.l.google.com"
        ],
        "details": {
            "regex": true,
            "typo": true,
            "disposable": true,
            "mx": true,
            "smtp": false
        }
    },
    "messageId": "abc123def456@sendgrid.com",
    "composedSubject": "John, your personalized follow-up on our demo session",
    "composedHtmlBody": "<h1>Hi John,</h1><p>Thank you for attending our product demo. As CEO at Acme Corp, I thought you might be interested in...</p>",
    "composedTextBody": "Hi John,\n\nThank you for attending our product demo. As CEO at Acme Corp, I thought you might be interested in...",
    "sentAt": "2026-03-02T10:30:00.000Z",
    "suggestedCorrection": null,
    "error": null,
    "batchNumber": 1
}
```

### **Sample Failed Validation Output**

```json
{
    "email": "jane-typo@gmaill.com",
    "name": "Jane Smith",
    "validationStatus": "typo_detected",
    "deliveryStatus": "skipped",
    "validationResult": {
        "status": "typo_detected",
        "formatValid": true,
        "dnsValid": false,
        "smtpValid": false,
        "mxRecords": [],
        "details": {
            "regex": true,
            "typo": true,
            "disposable": false,
            "mx": false,
            "smtp": false
        }
    },
    "messageId": null,
    "composedSubject": null,
    "composedHtmlBody": null,
    "composedTextBody": null,
    "sentAt": null,
    "suggestedCorrection": "jane@gmail.com",
    "error": null,
    "batchNumber": 1
}
```

### **Sample Output Format**

```json
{
    "email": "invalid-email",
    "name": null,
    "validationStatus": "invalid_format",
    "deliveryStatus": "skipped",
    "validationResult": null,
    "messageId": null,
    "composedSubject": null,
    "composedHtmlBody": null,
    "composedTextBody": null,
    "sentAt": null,
    "suggestedCorrection": null,
    "error": "Invalid email format",
    "batchNumber": 1
}
```

### **Output Views Available**

The Actor provides multiple organized views of the results:

1. **Overview View**: All processed recipients with complete details
2. **Successful View**: Only successfully sent emails
3. **Failed & Invalid View**: Emails with validation errors or delivery failures

This allows you to quickly identify successes and issues in your campaign.

---

## **Core Features**

### **Email Providers**

Supports five industry-leading email providers:

| Provider     | Best For                         | Setup Time |
| ------------ | -------------------------------- | ---------- |
| **SMTP**     | Testing, existing email accounts | 2 minutes  |
| **SendGrid** | Marketing campaigns, reliability | 5 minutes  |
| **Resend**   | Developer-friendly projects      | 2 minutes  |
| **AWS SES**  | High volume, cost optimization   | 10 minutes |
| **Mailgun**  | Advanced features, API control   | 5 minutes  |

### **Email Validation**

**Advanced multi-level validation system:**

- **Format Validation**: RFC 5322 compliance, structure checking
- **DNS Validation**: MX record verification, domain validity
- **Typo Detection**: Catches common typos (gmaill → gmail)
- **Disposable Email Blocking**: Rejects 10,000+ temporary services
- **SMTP Verification**: Optional mailbox existence checks
- **Validation Caching**: 100x faster for repeated campaigns

### **AI Email Composition**

Generate professional emails using 200+ AI models:

- **OpenRouter Integration**: Access to models from OpenAI, Anthropic, Google, Meta, and more
- **Custom Prompting**: Describe what you want, AI generates content
- **Per-Recipient Personalization**: Unique emails for each recipient using their data
- **Context Awareness**: Provide business context for more relevant content

### **Template Variables**

Dynamic personalization with variable substitution:

```
Hello {{name}}, we noticed you're {{position}} at {{company}}.
```

Supports nested properties, default values, and custom fields.

### **Performance Optimization**

- **Validation Caching**: Cache validation results for 1-7 days
- **Batch Processing**: Configurable batch sizes for rate limiting
- **Asynchronous Processing**: Efficient concurrent operations
- **Provider Rate Limiting**: Automatic throttling per provider

---

## **Quick Start**

### **1. Choose Your Email Provider**

**SMTP (Universal):**

```json
{
    "emailProvider": "smtp",
    "providerConfig": {
        "smtpHost": "smtp.gmail.com",
        "smtpPort": 587,
        "smtpUser": "your-email@gmail.com",
        "smtpPassword": "your-app-password"
    }
}
```

**SendGrid:**

```json
{
    "emailProvider": "sendgrid",
    "providerConfig": {
        "apiKey": "SG.your-sendgrid-api-key"
    }
}
```

**Resend:**

```json
{
    "emailProvider": "resend",
    "providerConfig": {
        "apiKey": "re_your-resend-api-key"
    }
}
```

**AWS SES:**

```json
{
    "emailProvider": "ses",
    "providerConfig": {
        "accessKeyId": "AKIA...",
        "secretAccessKey": "wJalr...",
        "region": "us-east-1"
    }
}
```

**Mailgun:**

```json
{
    "emailProvider": "mailgun",
    "providerConfig": {
        "apiKey": "key-...",
        "domain": "mg.yourdomain.com"
    }
}
```

### **2. Configure Recipients**

```json
{
    "recipients": [
        {
            "email": "john@example.com",
            "name": "John Doe",
            "customFields": {
                "company": "Acme Corp",
                "position": "Manager"
            }
        }
    ]
}
```

### **3. Set Email Content (or Use AI)**

**Manual Content:**

```json
{
    "subject": "Hello {{name}}!",
    "htmlBody": "<h1>Hi {{name}},</h1><p>Welcome to our service.</p>"
}
```

**AI Composition:**

```json
{
    "useAiComposer": true,
    "aiMode": "openrouter",
    "openrouterModel": "anthropic/claude-3-5-sonnet",
    "aiPrompt": "Write a warm welcome email",
    "aiContext": "We're a SaaS platform for project management"
}
```

### **4. Configure Validation and Sending**

```json
{
    "validateEmails": true,
    "validationLevel": "dns",
    "enableTemplateVariables": true,
    "validationCacheEnabled": true,
    "batchSize": 10,
    "delayBetweenBatches": 1000,
    "dryRun": false
}
```

---

## **Template Variables Guide**

### **Basic Syntax**

```
Hi {{name}}, we have a special offer for {{company}}!
```

### **With Default Values**

```
Hello {{firstName|friend}}, welcome!
```

If `firstName` is missing, displays "friend" instead.

### **Nested Properties**

```
Your account at {{user.company}} is ready!
```

Accesses nested custom fields.

### **Available Variables**

- `email`: Recipient email address
- `name`: Recipient display name
- `customFields.*`: Any custom field provided in recipients array

---

## **Validation Levels Explained**

### **Format Only (Fast)**

- Checks email syntax
- Speed: Instant
- Accuracy: ~95%
- Use for: Quick validation, high volumes

### **DNS/MX Records (Recommended)**

- Validates format and domain MX records
- Speed: 1-2 seconds per email
- Accuracy: ~99%
- Use for: Production campaigns (recommended)

### **SMTP Verification (Most Accurate)**

- Connects to recipient server for full verification
- Speed: 5-10 seconds per email
- Accuracy: ~99.5%
- Warning: May trigger spam filters, slow for large lists
- Use for: Small campaigns where accuracy is critical

---

## **Pricing Model**

**Pay-per-use with transparent, predictable costs:**

| Event                | Price                         |
| -------------------- | ----------------------------- |
| Actor Initialization | $0.001                        |
| Email Validation     | $0.002 per validation         |
| Email Sent           | $0.006 per email              |
| AI Composition       | $0.010 per email (OpenRouter) |
| Template Processing  | $0.001 per email              |

---

## **Security and Compliance**

### **Data Protection**

- API keys stored securely with encryption
- SMTP passwords handled through secure input fields
- No credentials logged in output
- Credentials never persisted to datasets

### **Email Compliance**

- CAN-SPAM Act compliance (include unsubscribe links)
- GDPR compliance (recipient consent and data protection)
- CASL compliance (Canada)
- Respects provider rate limits and specifications

### **Provider Verification**

- All email addresses must be verified with your provider
- Sender domains should be authenticated (SPF, DKIM, DMARC)
- Reply-to addresses recommended for compliance

### **Unsubscribe Links (Recommended)**

To maintain sender reputation and comply with regulations, include unsubscribe links in your email templates:

```html
<footer>
    <p>
        <a href="{{unsubscribeLink}}">Unsubscribe from this list</a>
    </p>
</footer>
```

Or in plain text:

```
---
To unsubscribe, visit: {{unsubscribeLink}}
```

Many providers (SendGrid, Mailgun, etc.) automatically handle unsubscribe links. Check your provider's documentation for details.

---

## **Troubleshooting**

### **Validation Issues**

**"No MX records found"**

- Email domain is configured incorrectly
- Domain doesn't exist or isn't set up for email
- Check domain spelling

**"SMTP validation failed"**

- Server may have rejected the connection
- Some providers temporarily block SMTP verification requests
- Try DNS validation level instead

### **Sending Issues**

**"Authentication failed"**

- API key or SMTP credentials are incorrect
- Provider credentials have expired
- Check credentials in your provider's dashboard

**"Rate limit exceeded"**

- Decrease batch size or increase delay between batches
- Provider may have specific rate limits
- Retry after waiting period

**"Email address validation skipped"**

- validateEmails is set to false
- Validation level may not support required checks
- Enable validation if deliverability is critical

### **AI Composition Issues**

**"AI model not available"**

- Model may be temporarily unavailable
- Check OpenRouter or your provider's status
- Try a different model

**"Invalid API key"**

- OpenRouter or BYOK API key is incorrect
- Credentials may have expired
- Check your provider's account settings

---

## **Professional Best Practices**

### **Pre-Campaign Checklist**

- Verify all recipient email addresses
- Test with a small sample (dryRun mode recommendation)
- Verify sender email with provider
- Set up SPF, DKIM, and DMARC records
- Use DNS-level validation minimum
- Include unsubscribe links in templates
- Review AI-generated content before sending

### **Campaign Optimization**

- Use DNS validation for best speed/accuracy balance
- Enable validation caching for repeated campaigns
- Start with small batches to test deliverability
- Monitor bounce rates and adjust batch sizes
- Use AI personalization only when beneficial
- Avoid SMTP validation for large campaigns

### **Post-Campaign Analysis**

- Review validation status distribution
- Check delivery failure reasons
- Monitor unsubscribe rates
- Track bounce rates by domain
- Adjust future campaigns based on results

---

## **Resources**

### **Documentation**

- Complete feature documentation in `/docs` folder
- Provider-specific setup guides available
- API integration examples and code samples

### **References**

- [Apify Platform Documentation](https://docs.apify.com)
- [Apify Acceptable Use Policy](https://docs.apify.com/legal/acceptable-use-policy)
- [CAN-SPAM Act Rules](https://www.ftc.gov/business-guidance/resources/can-spam-act-compliance-guide-business)
- [GDPR Compliance Guide](https://gdpr-info.eu/)

### **Support**

- Apify Community and Support Channels
- Provider-specific support contacts
- Technical documentation and guides

---

## **Summary**

The [AI Bulk Email Sender & Validator](https://apify.com/hlymrk/ai-bulk-email-sender-and-validator) provides a comprehensive, cost-effective solution for professional email campaigns at scale. With advanced validation, AI-powered composition, and support for multiple providers, it enables teams to send personalized, compliant emails efficiently.

Key capabilities:

- Multi-provider email sending (SMTP, SendGrid, Resend, SES, Mailgun)
- Advanced email validation with typo detection
- AI-powered email composition using 200+ models
- Template variable substitution for personalization
- Validation caching for performance
- Comprehensive output with detailed results
- Pay-per-use pricing without monthly fees

Remember to use this service responsibly, follow all applicable email regulations, include unsubscribe links, and maintain good sending practices to protect your sender reputation.
