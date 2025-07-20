# Wireframe Documentation - Get a Room UX/UI Project
**Project:** Get a Room Online Booking System  
**Designer:** Stella Oiro  
**Wireframe Version:** 1.0  
**Documentation Date:** July 2025

---

## Overview

This document provides comprehensive documentation for all wireframe prototypes created for the Get a Room booking system. All wireframes are interactive HTML prototypes optimized for mobile-first usage and built with Kenya's market specifics in mind.

### Wireframe Architecture
- **Framework:** Custom responsive CSS grid system (wireframe-base.css)
- **Interaction:** Vanilla JavaScript for prototype functionality
- **Optimization:** Mobile-first design (320px to 1200px)
- **Cultural Context:** Kenya business practices, M-Pesa integration, SMS confirmations

---

## Wireframe 1: Daily Booking System
**File:** `02-wireframes/html-wireframes/daily-booking/index.html`  
**Primary User:** James Ochieng (Freelance Designer)  
**Use Case:** Quick daily workspace booking for immediate needs

### User Flow Overview
1. **Space Selection** → 2. **Time Selection** → 3. **Contact Details** → 4. **Payment & Confirmation**

### Detailed Flow Documentation

#### Step 1: Space Selection
**Purpose:** Allow users to choose appropriate workspace type based on needs and budget

**Design Decisions:**
- **Grid layout** for easy mobile browsing of 6 different workspace options
- **Clear pricing display** upfront to build trust (James's key requirement)
- **Visual indicators** for availability status (available, unavailable, limited)
- **Descriptive workspace types** with amenities listed

**Available Options:**
- Hot Desk - Window Side (KES 800/day)
- Hot Desk - Central (KES 700/day) 
- Quiet Zone Desk (KES 900/day)
- Lounge Area (KES 600/day - shown as unavailable)
- Standing Desk (KES 750/day)
- Private Booth (KES 1,000/day)

**Interactive Elements:**
- Clickable workspace cards with hover states
- Selected state visual feedback
- Unavailable spaces clearly marked and non-clickable
- Error message display if no selection made

**Cultural Considerations:**
- Pricing in Kenyan Shillings (KES)
- Professional workspace descriptions
- Clear value proposition for each price point

#### Step 2: Time Selection
**Purpose:** Enable flexible time booking matching diverse user schedules

**Design Decisions:**
- **Date picker** with minimum date set to today
- **Duration dropdown** with common options (half-day discount, full-day, custom)
- **Time slot grid** appearing based on duration selection
- **Custom time picker** for specific hour requirements

**Time Options:**
- Half Day (4 hours) - 50% discount to incentivize usage
- Full Day (8+ hours) - standard pricing
- Custom Hours - specific start and end time selection

**Available Time Slots:**
- 6 AM - 10 AM (early bird option)
- 8 AM - 12 PM (morning focused)
- 10 AM - 2 PM (mid-morning to afternoon)
- 12 PM - 4 PM (afternoon focused)
- 2 PM - 6 PM (shown as unavailable for testing)
- 4 PM - 8 PM (late afternoon)
- 6 PM - 10 PM (evening option)

**Interactive Elements:**
- Dynamic time slot display based on duration selection
- Selected time slot highlighting
- Unavailable slots clearly marked
- Custom time dropdowns with hour-by-hour options

#### Step 3: Contact Details & Add-ons
**Purpose:** Capture necessary customer information and offer additional services

**Contact Information Required:**
- Full Name (required for booking confirmation)
- Phone Number (required for SMS confirmations)
- Email Address (required for backup confirmations)

**Optional Add-on Services:**
- Power Bank Rental (+KES 100)
- Printing Access (+KES 50)
- Unlimited Coffee (+KES 150)
- Personal Locker (+KES 50)

**Special Requirements:**
- Open text field for accessibility needs or special requests

**Design Decisions:**
- **Mobile-optimized form fields** with appropriate input types
- **Add-on grid layout** for easy selection
- **Price transparency** with costs clearly displayed
- **Optional vs. required field distinction**

#### Step 4: Payment & Confirmation
**Purpose:** Complete transaction with trusted payment method and provide confirmation

**Booking Summary Display:**
- Selected workspace and pricing
- Date, time, and duration
- Add-on services and costs
- Total amount calculation

**Payment Method Options:**
1. **M-Pesa** (primary option, most prominent)
2. **Credit/Debit Card** (secondary option)
3. **Bank Transfer** (for larger amounts)

**M-Pesa Integration Simulation:**
- Phone number input field
- M-Pesa prompt simulation message
- Payment processing loading state
- Confirmation with transaction reference

**Terms and Conditions:**
- Checkbox for agreement to terms
- Linked terms of service and cancellation policy
- Clear cancellation policy display (2-hour advance notice)

**Confirmation System:**
- Booking reference number generation
- SMS confirmation simulation
- Email confirmation mention
- Next steps and contact information

### Technical Implementation Notes

**Responsive Behavior:**
- 4-column grid on desktop, 2-column on tablet, 1-column on mobile
- Touch-friendly button sizing (minimum 44px)
- Optimized text sizing for mobile readability

**Form Validation:**
- Real-time validation feedback
- Error message display with clear resolution steps
- Required field indicators
- Mobile-appropriate keyboard types

**Loading States:**
- Progressive form revealing based on completion
- Loading animations for payment processing
- Success state animations and confirmations

**Accessibility Features:**
- Semantic HTML structure
- ARIA labels for screen readers
- Keyboard navigation support
- High contrast color ratios

---

## Wireframe 2: Weekly Booking System
**File:** `02-wireframes/html-wireframes/weekly-booking/index.html`  
**Primary User:** Sarah Wanjiku (Team Lead)  
**Use Case:** Team workspace booking with advance planning

### User Flow Overview
1. **Week Selection** → 2. **Workspace Type** → 3. **Recurring Options** → 4. **Team Details** → 5. **Confirmation**

### Detailed Flow Documentation

#### Calendar Interface
**Purpose:** Visual week selection for team planning coordination

**Design Decisions:**
- **Calendar grid layout** with Monday-Friday business week focus
- **Week-based selection** rather than individual days
- **Availability indicators** showing booking density
- **Month navigation** for advance planning

**Calendar Features:**
- Current week highlighting
- Availability status per day (available, busy, full)
- Booking count display
- Weekend graying out (business focus)

**Interactive Elements:**
- Click any day to select entire week
- Month navigation arrows
- Week range display and confirmation
- Availability legend

#### Workspace Categories
**Purpose:** Organized workspace selection for different team sizes and needs

**Categories:**
1. **Hot Desks** - Flexible workspace for individuals
   - Premium Hot Desk (KES 3,500/week)
   - Standard Hot Desk (KES 2,800/week)

2. **Dedicated Desks** - Assigned workspace with storage
   - Corner Dedicated Desk (KES 5,500/week)
   - Standard Dedicated Desk (KES 4,800/week)
   - Window Dedicated Desk (KES 6,000/week - shown unavailable)

3. **Team Spaces** - Collaborative areas for groups
   - 4-Person Team Pod (KES 8,500/week)
   - Team Corner 3 desks (KES 6,800/week)

**Design Features:**
- Category-based organization for easy navigation
- Savings calculations vs. daily rates
- Unavailable options clearly marked
- Feature descriptions for each workspace type

#### Recurring Booking Options
**Purpose:** Provide cost savings and convenience for regular team needs

**Options:**
1. **Single Week** - One-time booking (no additional discount)
2. **4 Weeks (Monthly)** - 10% discount on total booking
3. **12 Weeks (Quarterly)** - 20% discount + priority support

**Benefits Display:**
- Clear discount percentages
- Priority support mentions
- Long-term relationship building
- Cost comparison calculations

#### Team Information Collection
**Purpose:** Gather necessary details for team coordination and communication

**Required Information:**
- Team lead contact information
- Company/organization name
- Number of team members
- Preferred working hours
- Special requirements

**Additional Services:**
- Mail handling service (+KES 200/week)
- Meeting room allocation (2 hrs/week +KES 500)
- Printing credits (100 pages +KES 300)
- Unlimited coffee/tea (+KES 400)
- Personal storage locker (+KES 150)
- Access to networking events (free)

**Communication Preferences:**
- Working hours selection
- Team notification preferences
- Special accessibility requirements

### Technical Implementation Notes

**Calendar Functionality:**
- Dynamic month generation
- Week calculation and display
- Availability data simulation
- Mobile swipe navigation support

**Pricing Calculations:**
- Dynamic discount applications
- Multi-week total calculations
- Add-on service cost additions
- Real-time price updates

**Team Management:**
- Multiple contact information handling
- Team size accommodations
- Notification system planning

---

## Wireframe 3: Monthly Membership System
**File:** `02-wireframes/html-wireframes/monthly-booking/index.html`  
**Primary User:** David Kimani (Remote Developer)  
**Use Case:** Long-term workspace commitment with premium features

### User Flow Overview
1. **Membership Tier Selection** → 2. **Workspace Customization** → 3. **Contract Terms** → 4. **Payment Setup** → 5. **Application Submission**

### Detailed Flow Documentation

#### Membership Tier Comparison
**Purpose:** Clear value proposition for different commitment levels

**Membership Tiers:**
1. **Essential Member** (KES 12,000/month)
   - Dedicated desk assignment
   - Personal storage drawer
   - High-speed WiFi access
   - Business address service
   - Basic printing (50 pages)
   - Meeting room (2 hrs/month)

2. **Professional Member** (KES 18,000/month) - Most Popular
   - Premium desk location
   - Large storage cabinet
   - Priority WiFi access
   - Business address + mail handling
   - Printing credits (200 pages)
   - Meeting room (8 hrs/month)
   - Priority support
   - Guest day passes (2/month)

3. **Executive Member** (KES 25,000/month)
   - Corner office space
   - Personal filing cabinet
   - Dedicated WiFi line
   - Full business services
   - Unlimited printing
   - Meeting room (20 hrs/month)
   - Concierge support
   - Guest day passes (5/month)
   - Event space access

**Design Features:**
- Side-by-side comparison layout
- "Most Popular" badge on recommended tier
- Feature checkmarks vs. limitations
- Clear pricing and savings display

#### Workspace Customization
**Purpose:** Allow personalization of workspace assignment and contract terms

**Workspace Selection:**
- Visual grid of available workspaces (A1, A2, B1, etc.)
- Location preferences (window side, corner, central)
- Premium location surcharges
- Availability status for each spot

**Contract Duration Options:**
- 1 Month (flexible, no discount)
- 3 Months (5% discount)
- 6 Months (10% discount)
- 12 Months (15% discount)

**Add-on Services:**
- Extended Hours Access (+KES 2,000)
- Virtual Assistant 10hrs/month (+KES 5,000)
- Guest Workspace (+KES 3,000)
- Marketing Package (+KES 1,500)
- Phone Booth Access (+KES 1,000)
- Premium Refreshments (+KES 800)

#### Payment Plan Configuration
**Purpose:** Flexible payment options to accommodate different financial preferences

**Payment Plans:**
1. **Monthly Payment** - Maximum flexibility
2. **Quarterly Payment** - 5% discount, pay every 3 months
3. **Annual Payment** - 10% discount, guaranteed rate lock

**Payment Method Setup:**
- M-Pesa recurring payment authorization
- Backup payment method selection
- Security deposit explanation (KES 5,000 refundable)

#### Application Processing
**Purpose:** Collect necessary information for membership approval and contract generation

**Required Information:**
- Full legal name and ID/passport number
- Company/business details
- Industry/profession selection
- Business address for mail service

**M-Pesa Recurring Setup:**
- Primary M-Pesa phone number
- Automatic deduction authorization
- Backup payment method configuration

**Contract Terms:**
- Membership rights and responsibilities
- Cancellation policy (30 days notice)
- Usage guidelines and professional conduct
- Payment terms and late fee policies

### Technical Implementation Notes

**Membership Comparison:**
- Dynamic feature highlighting
- Price calculation with discounts
- Responsive comparison table
- Mobile-friendly tier selection

**Contract Management:**
- PDF contract generation simulation
- Digital signature preparation
- Automated billing setup
- Membership card/access preparation

---

## Wireframe 4: Admin Dashboard
**File:** `02-wireframes/html-wireframes/admin-dashboard/index.html`  
**Primary User:** Grace Akinyi (Business Owner)  
**Use Case:** Complete operational management and business oversight

### Dashboard Overview
**Purpose:** Centralized command center for all Get a Room operations

### Main Dashboard Components

#### Header Statistics
**Real-time Business Metrics:**
- Total Occupied: 47/50 spots
- Daily Spots: 12/15 occupied
- Monthly Members: 35/35 (full capacity)
- Today's Revenue: KES 125,400

**Quick Actions:**
- Quick Booking (for walk-ins)
- Mark Maintenance (space management)
- Send Reminders (payment follow-up)
- Export Report (daily/weekly analytics)

#### Live Calendar View
**Purpose:** Visual overview of booking density and availability

**Calendar Features:**
- Week view with booking density indicators
- Color-coded availability (green=available, yellow=busy, red=full)
- Booking count per day
- Navigation between weeks/months
- Today highlighting

**Interaction Capabilities:**
- Click days to see detailed bookings
- Quick booking creation
- Availability modification
- Maintenance scheduling

#### Real-time Occupancy Grid
**Purpose:** Instant overview of all 50 workspace spots

**Grid Layout:**
- 50 individual workspace indicators
- Color coding: Red (occupied), Yellow (reserved), Green (available), Gray (maintenance)
- Hover tooltips showing customer names and booking details
- Quick access to customer information

**Management Features:**
- Click spaces to see customer details
- Mark spaces for maintenance
- Quick customer communication
- Booking modification access

#### Recent Activity Feed
**Purpose:** Stay informed about all business activities

**Activity Types:**
- New bookings (with customer and space details)
- Payment received (with amount and method)
- Cancellations (with refund status)
- Maintenance completed
- Customer inquiries

**Activity Details:**
- Timestamp for each activity
- Customer information
- Action taken or required
- Priority indicators

#### Payment Management
**Purpose:** Complete financial oversight and reconciliation

**Payment Overview:**
- This Month: KES 342,000
- Today: KES 125,400
- Overdue: KES 15,200 (3 customers)

**Overdue Management:**
- Customer list with overdue amounts
- Days overdue tracking
- Automated reminder system
- Payment method retry options

**M-Pesa Integration:**
- M-Pesa transaction history
- Automatic reconciliation
- Payment verification
- Refund processing

#### Quick Booking Tool
**Purpose:** Efficient booking creation for walk-in customers

**Booking Form:**
- Customer name and phone
- Space type selection
- Date and duration
- Payment method
- Instant confirmation generation

**Features:**
- Auto-fill from customer history
- Real-time availability checking
- Immediate confirmation SMS
- Receipt generation

#### Communication Center
**Purpose:** Customer relationship management and communication

**Features:**
- Bulk SMS capabilities
- Payment reminder automation
- Booking confirmation sending
- Customer feedback collection

**Message Templates:**
- Welcome messages for new customers
- Payment reminders
- Booking confirmations
- Promotional messages

### Technical Implementation Notes

**Real-time Updates:**
- Live occupancy status
- Payment notification system
- Automatic activity feed updates
- Real-time availability synchronization

**Mobile Admin Interface:**
- Touch-friendly controls for mobile management
- Critical functions accessible on mobile
- Simplified mobile dashboard view
- Quick action buttons for common tasks

**Data Management:**
- Customer history tracking
- Revenue analytics
- Usage pattern analysis
- Business intelligence features

---

## Cross-Wireframe Technical Specifications

### Responsive Framework Details
**CSS Grid System:**
- 12-column flexible grid
- Breakpoints: 320px, 576px, 768px, 992px, 1200px
- Mobile-first media queries
- Flexible spacing system (0.25rem to 3rem scale)

### Interaction Patterns
**Common Elements:**
- Loading states for all form submissions
- Error message display patterns
- Success confirmation animations
- Progress indicators for multi-step flows

### Cultural Implementation
**Kenya-Specific Features:**
- KES currency formatting throughout
- M-Pesa payment flow integration
- SMS confirmation system
- Business hours display (6 AM - 10 PM)
- Professional Kenyan imagery placeholders

### Accessibility Implementation
**Standards Compliance:**
- WCAG 2.1 AA compliance
- Semantic HTML structure
- Keyboard navigation support
- Screen reader optimization
- High contrast color ratios (4.5:1 minimum)

### Performance Optimization
**Mobile-First Performance:**
- Optimized for 3G connection speeds
- Progressive enhancement
- Compressed image placeholders
- Minimal JavaScript for core functionality
- Cached content for repeat users

---

## Testing and Validation Notes

### Wireframe Testing Protocol
**User Testing Scenarios:**
- Task completion testing for each user persona
- Mobile device testing on actual Android devices
- Payment flow simulation testing
- Admin workflow efficiency testing

**Technical Validation:**
- Cross-browser compatibility (Chrome Mobile priority)
- Device compatibility (Android focus)
- Network condition testing (3G simulation)
- Accessibility tool validation

### Iteration Planning
**Based on User Feedback:**
- Form field optimization
- Navigation flow improvements
- Cultural sensitivity adjustments
- Performance enhancement priorities

**Grace's Business Feedback:**
- Admin workflow optimization
- Revenue tracking enhancement
- Customer communication improvements
- Operational efficiency features

---

## Implementation Handoff Notes

### Development Priorities
**Phase 1 (MVP):**
1. Daily booking system with M-Pesa integration
2. Basic admin dashboard with occupancy management
3. SMS confirmation system
4. Mobile-responsive implementation

**Phase 2 (Enhanced Features):**
1. Weekly booking system with team features
2. Advanced admin analytics
3. Customer management system
4. Monthly membership portal

**Phase 3 (Advanced Features):**
1. Advanced reporting and analytics
2. Automated communication systems
3. Business intelligence features
4. Multi-location support preparation

### Technical Requirements
**Payment Integration:**
- M-Pesa API implementation
- SMS gateway integration
- Email confirmation system
- Payment reconciliation automation

**Backend Requirements:**
- Real-time availability management
- Customer data management
- Booking history tracking
- Revenue analytics calculation

**Frontend Requirements:**
- Progressive web app capabilities
- Offline functionality for basic features
- Push notification system
- Advanced mobile optimizations

---

**Documentation prepared by:** Stella Oiro  
**Wireframe version:** 1.0  
**Ready for:** User testing and development handoff  
**Last updated:** July 2025