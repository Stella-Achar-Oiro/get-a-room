# Get a Room - Security & Privacy Framework

**Designer:** Stella Oiro  
**Client:** Grace Akinyi, Get a Room Coworking Space  
**Date:** July 16, 2025  
**Version:** 1.0

---

## Executive Summary

This security framework establishes comprehensive protection standards for Get a Room's digital booking platform, ensuring compliance with Kenya Data Protection Act 2019, GDPR requirements, and M-Pesa security protocols. The framework prioritizes customer trust while enabling seamless booking experiences.

### Key Security Objectives
- **Data Protection:** Safeguard customer personal and financial information
- **Payment Security:** Secure M-Pesa transaction processing and storage
- **Legal Compliance:** Meet Kenyan and international privacy regulations
- **Business Continuity:** Prevent security incidents that could disrupt operations
- **Customer Trust:** Build confidence in online payment systems

---

## 1. Kenya Data Protection Act Compliance

### 1.1 Data Controller Responsibilities

**Get a Room as Data Controller:**
- Register with Office of the Data Protection Commissioner (ODPC)
- Maintain data processing register
- Conduct Data Protection Impact Assessments (DPIA)
- Appoint Data Protection Officer if processing large volumes
- Implement Privacy by Design principles

### 1.2 Data Subject Rights

| Right | Implementation | Technical Requirement |
|-------|---------------|----------------------|
| **Access** | User dashboard showing all personal data | Data export functionality |
| **Rectification** | Profile editing with audit trail | Version control for user data |
| **Erasure** | Account deletion with data purging | Hard delete with verification |
| **Portability** | Data export in JSON/CSV format | Standardized export API |
| **Objection** | Marketing opt-out mechanisms | Granular consent management |
| **Restriction** | Temporary data processing suspension | Flag-based processing controls |

### 1.3 Lawful Basis for Processing

```
Data Category: Personal Information
- Lawful Basis: Contract performance (booking services)
- Retention: Active account + 7 years
- Purpose: Service delivery, customer support

Data Category: Payment Information  
- Lawful Basis: Contract performance + Legal obligation
- Retention: 7 years (tax compliance)
- Purpose: Payment processing, fraud prevention

Data Category: Marketing Preferences
- Lawful Basis: Consent
- Retention: Until consent withdrawn
- Purpose: Marketing communications

Data Category: Usage Analytics
- Lawful Basis: Legitimate interest
- Retention: 2 years maximum
- Purpose: Service improvement, security
```

### 1.4 Cross-Border Data Transfers

**Requirements for International Users:**
- Adequacy decision verification (EU/UK users)
- Standard Contractual Clauses (SCCs) implementation
- Transfer impact assessment documentation
- User notification of international transfers

---

## 2. M-Pesa Security Protocols

### 2.1 Safaricom API Security Standards

**Authentication & Authorization:**
```
API Security Layer 1: OAuth 2.0
- Client credentials grant type
- Token expiration: 1 hour maximum
- Scope-based access control
- Rate limiting: 100 requests/minute

API Security Layer 2: Request Signing
- SHA-256 HMAC message authentication
- Timestamp validation (±5 minutes)
- Nonce implementation (prevent replay)
- Base64 encoding for all signatures

API Security Layer 3: TLS Encryption
- TLS 1.3 minimum requirement
- Certificate pinning implementation
- Perfect Forward Secrecy (PFS)
- Strong cipher suite enforcement
```

### 2.2 M-Pesa Data Handling

**Sensitive Data Protection:**
- **Phone Numbers:** Encrypted at rest using AES-256
- **Transaction IDs:** Hashed using SHA-256 + salt
- **Payment Amounts:** Encrypted with format-preserving encryption
- **STK Push Passwords:** Generated dynamically, never stored

**Transaction Security Flow:**
```
1. User initiates payment
   ↓
2. Generate unique transaction reference
   ↓  
3. Create STK push request with encrypted payload
   ↓
4. Send to M-Pesa API over TLS 1.3
   ↓
5. Store transaction status (pending/complete/failed)
   ↓
6. Implement callback URL security validation
   ↓
7. Update booking status atomically
   ↓
8. Send confirmation via encrypted SMS
```

### 2.3 PCI DSS Considerations

**Scope Reduction Strategy:**
- No card data storage (M-Pesa only reduces PCI scope)
- Tokenization for any card backup options
- Network segmentation for payment processing
- Regular vulnerability assessments

---

## 3. User Authentication Standards

### 3.1 Multi-Factor Authentication Framework

**Primary Authentication:**
- Phone number verification (SMS OTP)
- 6-digit numeric code, 5-minute expiry
- Rate limiting: 3 attempts per 15 minutes
- Account lockout after 5 failed attempts

**Secondary Authentication (High-Value Transactions):**
- M-Pesa PIN confirmation
- Biometric verification (mobile apps)
- Device fingerprinting for recognition

### 3.2 Session Management

**Security Requirements:**
```javascript
// Session Configuration
sessionConfig = {
    duration: 30 * 60 * 1000,        // 30 minutes
    renewalThreshold: 5 * 60 * 1000,  // 5 minutes before expiry
    secureFlag: true,                 // HTTPS only
    httpOnlyFlag: true,               // Prevent XSS access
    sameSiteFlag: 'Strict',          // CSRF protection
    entropyBits: 128                 // Session ID entropy
}

// Auto-logout warnings
const warningTime = 25 * 60 * 1000;  // 25 minutes
const logoutTime = 30 * 60 * 1000;   // 30 minutes
```

### 3.3 Password Security (If Implemented)

**Password Requirements:**
- Minimum 12 characters
- Mix of uppercase, lowercase, numbers, symbols
- No common passwords (check against breach databases)
- bcrypt hashing with minimum 12 rounds
- Password reset via phone verification only

---

## 4. Data Encryption Standards

### 4.1 Encryption at Rest

**Database Encryption:**
```sql
-- Customer personal data
CREATE TABLE customers (
    id UUID PRIMARY KEY,
    phone_encrypted BYTEA,              -- AES-256-GCM
    name_encrypted BYTEA,               -- AES-256-GCM  
    email_encrypted BYTEA,              -- AES-256-GCM
    created_at TIMESTAMPTZ,
    encryption_key_id UUID REFERENCES encryption_keys(id)
);

-- Payment information (minimal storage)
CREATE TABLE transactions (
    id UUID PRIMARY KEY,
    reference_hash CHAR(64),            -- SHA-256 of M-Pesa reference
    amount_encrypted BYTEA,             -- Format-preserving encryption
    status VARCHAR(20),                 -- 'pending', 'complete', 'failed'
    created_at TIMESTAMPTZ,
    customer_id UUID REFERENCES customers(id)
);
```

**Key Management:**
- Hardware Security Module (HSM) for key storage
- Key rotation every 90 days
- Envelope encryption for database keys
- Separate keys per data type
- Key access logging and monitoring

### 4.2 Encryption in Transit

**API Communication:**
- TLS 1.3 minimum for all external APIs
- Certificate pinning for M-Pesa endpoints
- HSTS headers with max-age 31536000
- HTTP/2 for performance with security

**Internal Communication:**
- mTLS between microservices
- Service mesh security (Istio/Linkerd)
- Network policies for pod-to-pod communication
- VPN for admin access

---

## 5. Fraud Prevention Measures

### 5.1 Real-Time Fraud Detection

**Behavioral Analysis:**
```javascript
// Fraud detection rules
const fraudRules = {
    velocityCheck: {
        maxBookingsPerHour: 3,
        maxAmountPerDay: 10000,        // KES
        maxFailedPayments: 5
    },
    
    deviceFingerprinting: {
        trackDeviceId: true,
        flagNewDevices: true,
        maxDevicesPerUser: 3
    },
    
    geolocationCheck: {
        flagVPNusage: true,
        restrictCountries: ['high-risk-list'],
        maxDistanceMovement: 1000      // km per hour
    },
    
    timePatterns: {
        flagUnusualHours: true,        // 2 AM - 5 AM suspicious
        weekendActivityCheck: true,
        holidayPatternAnalysis: true
    }
};
```

### 5.2 M-Pesa Fraud Prevention

**Transaction Validation:**
- Callback URL verification with HMAC
- Transaction amount double-verification
- Duplicate transaction detection
- Phone number consistency checking
- Customer identity verification for large amounts

**Risk Scoring Model:**
```
Low Risk (Score 0-30):
- Verified phone number
- Consistent device usage  
- Normal booking patterns
- Successful payment history

Medium Risk (Score 31-70):
- New user (< 30 days)
- Different device
- Unusual booking time
- First large payment

High Risk (Score 71-100):
- VPN/proxy usage detected
- Multiple failed payments
- Rapid successive bookings
- Phone number mismatch
```

### 5.3 Response Protocols

**Automated Responses:**
- Low Risk: Process normally
- Medium Risk: Additional verification (SMS OTP)
- High Risk: Manual review + admin approval

**Manual Review Process:**
- Customer service verification call
- Document verification for high amounts
- Grace's approval for suspicious patterns
- Blacklist management for confirmed fraud

---

## 6. Privacy Policy Framework

### 6.1 Privacy Notice Template

```markdown
## Privacy Notice - Get a Room Coworking Space

### Who We Are
Get a Room is a coworking space located in Kisumu, Kenya. 
Grace Akinyi is the data controller responsible for your personal information.

Contact Information:
- Email: privacy@getaroom.co.ke
- Phone: +254 XXX XXX XXX
- Address: [Physical Address], Kisumu, Kenya

### Information We Collect
**Booking Information:**
- Full name and phone number
- Payment information (M-Pesa transactions)
- Booking preferences and history
- Communication records

**Technical Information:**
- Device and browser information
- IP address and location data
- Usage patterns and analytics
- Security logs

### Why We Use Your Information
**Service Delivery:** Process bookings and payments
**Customer Support:** Resolve issues and answer questions  
**Legal Compliance:** Meet tax and regulatory requirements
**Security:** Prevent fraud and unauthorized access
**Improvements:** Enhance our services and user experience

### Your Rights Under Kenya Data Protection Act
- Access your personal information
- Correct inaccurate information
- Delete your account and data
- Object to processing for marketing
- Export your data
- Complain to ODPC

### Data Retention
- Active accounts: During service use + 7 years
- Payment records: 7 years (tax compliance)
- Marketing consent: Until withdrawn
- Security logs: 2 years maximum

### International Transfers
If you access our service from outside Kenya, your information 
may be transferred internationally. We ensure adequate protection 
through appropriate safeguards.

### Changes to This Notice
We will notify you of any material changes via SMS or email.
Last updated: [Date]
```

### 6.2 Consent Management

**Granular Consent Options:**
- Essential services (cannot be declined)
- Marketing communications (opt-in)
- Analytics and improvement (opt-in)
- Third-party integrations (opt-in)

**Consent Records:**
```sql
CREATE TABLE consent_records (
    id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(id),
    consent_type VARCHAR(50),           -- 'marketing', 'analytics', etc.
    granted BOOLEAN,
    timestamp TIMESTAMPTZ,
    ip_address INET,
    user_agent TEXT,
    method VARCHAR(20)                  -- 'web', 'sms', 'phone'
);
```

---

## 7. Incident Response Plan

### 7.1 Security Incident Classification

**Level 1 - Critical:**
- Data breach affecting >100 customers
- Payment system compromise
- Unauthorized admin access
- Customer financial loss

**Level 2 - High:**
- Data breach affecting <100 customers
- Service unavailability >4 hours
- Attempted system intrusion
- Compliance violation

**Level 3 - Medium:**
- Suspicious activity detection
- Minor data exposure
- Service degradation
- Failed security controls

### 7.2 Response Timeline

**Immediate Response (0-1 hour):**
- Contain the incident
- Assess scope and impact
- Notify Grace and technical team
- Preserve evidence

**Short-term Response (1-24 hours):**
- Implement fixes and patches
- Customer notification if required
- Regulatory notification (ODPC within 72 hours)
- Document incident details

**Long-term Response (1-30 days):**
- Root cause analysis
- Security improvements
- Customer support for affected users
- Legal compliance follow-up

### 7.3 Communication Templates

**Customer Notification (Data Breach):**
```
Subject: Important Security Notice - Get a Room

Dear [Customer Name],

We are writing to inform you of a security incident that may have 
affected your personal information.

What Happened:
[Brief description of incident]

Information Involved:
[Specific data types affected]

What We're Doing:
- Secured the affected systems
- Working with security experts
- Reported to authorities
- Implementing additional protections

What You Should Do:
- Monitor your M-Pesa statements
- Change your password if you have one
- Contact us with any concerns

We sincerely apologize for this incident and any inconvenience caused.

Grace Akinyi
Get a Room Coworking Space
privacy@getaroom.co.ke
+254 XXX XXX XXX
```

---

## 8. Staff Training & Awareness

### 8.1 Security Training Program

**All Staff Training (Quarterly):**
- Data protection awareness
- Phishing identification
- Password security
- Physical security protocols
- Customer privacy rights

**Technical Staff Training (Monthly):**
- Secure coding practices
- Vulnerability management
- Incident response procedures
- Security testing methods
- Compliance requirements

### 8.2 Access Controls

**Role-Based Access:**
```
Grace (Owner):
- Full system access
- Customer data viewing
- Payment information access
- Security configuration

Front Desk Staff:
- Booking management
- Customer profile viewing (limited)
- No payment information access
- No system configuration

Technical Support:
- System logs and monitoring
- Configuration management
- No customer personal data
- Encrypted database access only

External Auditors:
- Read-only access to security logs
- Compliance documentation access
- No customer data access
- Time-limited access grants
```

---

## 9. Compliance Monitoring

### 9.1 Regular Assessments

**Monthly Reviews:**
- Access log analysis
- Failed login attempt patterns
- Payment anomaly detection
- Consent record auditing

**Quarterly Assessments:**
- Vulnerability scanning
- Penetration testing
- Compliance gap analysis
- Staff training effectiveness

**Annual Reviews:**
- Full security audit
- Data protection impact assessment update
- Business continuity testing
- Regulatory compliance certification

### 9.2 Key Performance Indicators

**Security KPIs:**
- Mean Time to Detect (MTTD): <15 minutes
- Mean Time to Respond (MTTR): <1 hour
- False positive rate: <5%
- Security training completion: 100%

**Privacy KPIs:**
- Data subject request response time: <30 days
- Consent withdrawal processing: <24 hours
- Data breach notification time: <72 hours
- Customer privacy complaint resolution: <7 days

---

## 10. Implementation Roadmap

### Phase 1: Foundation (Weeks 1-4)
- [ ] ODPC registration and compliance setup
- [ ] Basic encryption implementation
- [ ] M-Pesa security integration
- [ ] Privacy policy publication
- [ ] Staff training program launch

### Phase 2: Enhancement (Weeks 5-8)
- [ ] Advanced fraud detection implementation
- [ ] Multi-factor authentication rollout
- [ ] Incident response plan testing
- [ ] Consent management system
- [ ] Security monitoring tools deployment

### Phase 3: Optimization (Weeks 9-12)
- [ ] Full compliance audit
- [ ] Performance optimization
- [ ] Advanced analytics implementation
- [ ] Third-party security assessment
- [ ] Certification maintenance

---

## 11. Budget Considerations

### Implementation Costs (One-time)
- Legal compliance consultation: KES 300,000
- Security tool licenses: KES 150,000
- SSL certificates and security infrastructure: KES 100,000
- Staff training and certification: KES 200,000
- **Total Initial Investment:** KES 750,000

### Ongoing Costs (Annual)
- Security monitoring tools: KES 120,000
- Compliance audits: KES 180,000
- Staff training updates: KES 80,000
- Insurance and liability coverage: KES 150,000
- **Total Annual Cost:** KES 530,000

### Return on Investment
- Customer trust and retention: +25% booking rate
- Reduced fraud losses: Savings of KES 200,000/year
- Regulatory compliance: Avoid penalties up to KES 5,000,000
- Insurance premium reductions: 15% savings

---

*This security framework provides comprehensive protection while maintaining usability for Get a Room's customers. Regular updates ensure continued effectiveness against evolving threats.*

**Document Classification:** Confidential  
**Review Schedule:** Quarterly  
**Next Review Date:** October 16, 2025