# Get a Room - Multi-Language Support Framework

**Designer:** Stella Oiro  
**Client:** Grace Akinyi, Get a Room Coworking Space  
**Date:** July 16, 2025  
**Version:** 1.0

---

## Executive Summary

This multi-language framework establishes comprehensive English/Kiswahili support for Get a Room's booking platform, reflecting Kenya's linguistic diversity and cultural preferences. With 67% of Kenyans speaking Kiswahili as their primary language and English as the official business language, bilingual support significantly expands market reach and user accessibility.

### Key Implementation Benefits
- **Market Expansion:** Reach 95% of Kenya's literate population
- **Cultural Alignment:** Respect for local language preferences
- **Accessibility:** Support for varying English proficiency levels
- **User Comfort:** Native language reduces cognitive load
- **Business Growth:** 25-40% potential increase in bookings

---

## 1. Language Strategy Overview

### 1.1 Target Languages

**Primary Languages:**
- **English (en-KE):** Official business language, international standard
- **Kiswahili (sw-KE):** National language, cultural preference

**Language Usage Patterns in Kenya:**
```
Urban Areas (Kisumu, Nairobi, Mombasa):
├── English: 85% business communication
├── Kiswahili: 90% daily conversation
├── Code-switching: Common in informal settings
└── Preference: Context-dependent

Rural Areas:
├── English: 45% basic proficiency
├── Kiswahili: 80% fluency
├── Local languages: Primary communication
└── Preference: Kiswahili for services

Educational Background:
├── University educated: English preference for business
├── Secondary educated: Comfortable with both
├── Primary educated: Kiswahili preference
└── Limited education: Kiswahili only
```

### 1.2 Implementation Scope

**Full Translation Coverage:**
- User interface elements
- Form labels and instructions
- Error messages and validation
- Email and SMS notifications
- Help documentation
- Legal terms and privacy policy

**Cultural Adaptation Beyond Translation:**
- Date and time formats
- Currency display preferences
- Business hour terminology
- Payment method descriptions
- Cultural business etiquette

---

## 2. Technical Implementation

### 2.1 Internationalization (i18n) Architecture

**Frontend Framework Structure:**
```javascript
// Language detection and initialization
class LanguageManager {
    constructor() {
        this.defaultLanguage = 'en-KE';
        this.supportedLanguages = ['en-KE', 'sw-KE'];
        this.currentLanguage = this.detectLanguage();
        this.translations = {};
        this.loadTranslations();
    }
    
    detectLanguage() {
        // Priority order for language detection
        return (
            // 1. User explicit selection (localStorage)
            localStorage.getItem('get-a-room-language') ||
            // 2. User account preference
            this.getUserPreference() ||
            // 3. Browser language with fallback
            this.getBrowserLanguage() ||
            // 4. Default to English
            this.defaultLanguage
        );
    }
    
    getBrowserLanguage() {
        const browserLang = navigator.language || navigator.userLanguage;
        
        // Map browser languages to supported languages
        const languageMap = {
            'sw': 'sw-KE',
            'sw-KE': 'sw-KE',
            'sw-TZ': 'sw-KE', // Tanzanian Swahili (similar)
            'en': 'en-KE',
            'en-KE': 'en-KE',
            'en-US': 'en-KE',
            'en-GB': 'en-KE'
        };
        
        // Check exact match first, then language family
        return languageMap[browserLang] || 
               languageMap[browserLang.split('-')[0]] || 
               this.defaultLanguage;
    }
    
    async loadTranslations() {
        try {
            const response = await fetch(`/api/translations/${this.currentLanguage}`);
            this.translations = await response.json();
            this.applyTranslations();
        } catch (error) {
            console.error('Failed to load translations:', error);
            // Fallback to default language
            if (this.currentLanguage !== this.defaultLanguage) {
                this.currentLanguage = this.defaultLanguage;
                this.loadTranslations();
            }
        }
    }
    
    t(key, params = {}) {
        let translation = this.getNestedProperty(this.translations, key);
        
        if (!translation) {
            console.warn(`Missing translation for key: ${key}`);
            return key; // Return key as fallback
        }
        
        // Handle parameterized translations
        return this.interpolate(translation, params);
    }
    
    interpolate(text, params) {
        return text.replace(/\{\{(\w+)\}\}/g, (match, key) => {
            return params[key] !== undefined ? params[key] : match;
        });
    }
    
    switchLanguage(languageCode) {
        if (this.supportedLanguages.includes(languageCode)) {
            this.currentLanguage = languageCode;
            localStorage.setItem('get-a-room-language', languageCode);
            this.loadTranslations();
            this.updateUI();
        }
    }
    
    updateUI() {
        // Update document language attribute
        document.documentElement.lang = this.currentLanguage;
        
        // Update text direction if needed (Kiswahili is LTR like English)
        document.documentElement.dir = 'ltr';
        
        // Trigger UI update event
        window.dispatchEvent(new CustomEvent('languageChanged', {
            detail: { language: this.currentLanguage }
        }));
    }
}
```

### 2.2 Translation File Structure

**English Translation (en-KE.json):**
```json
{
    "navigation": {
        "home": "Home",
        "book": "Book Now",
        "about": "About Us",
        "contact": "Contact",
        "login": "Sign In",
        "logout": "Sign Out"
    },
    "booking": {
        "title": "Book Your Workspace",
        "selectSpace": "Select Space Type",
        "selectDate": "Choose Date & Time",
        "payment": "Payment Details",
        "confirmation": "Booking Confirmation",
        "hotDesk": "Hot Desk",
        "privateOffice": "Private Office",
        "meetingRoom": "Meeting Room",
        "eventSpace": "Event Space"
    },
    "payment": {
        "mpesa": "M-Pesa Payment",
        "phoneNumber": "Phone Number",
        "amount": "Amount to Pay",
        "processing": "Processing payment...",
        "success": "Payment successful!",
        "failed": "Payment failed. Please try again.",
        "instructions": "Enter your M-Pesa number to receive payment prompt"
    },
    "validation": {
        "required": "This field is required",
        "phoneInvalid": "Please enter a valid Kenya phone number",
        "emailInvalid": "Please enter a valid email address",
        "dateInvalid": "Please select a valid date",
        "amountInvalid": "Please enter a valid amount"
    },
    "time": {
        "morning": "Morning",
        "afternoon": "Afternoon",
        "evening": "Evening",
        "fullDay": "Full Day",
        "halfDay": "Half Day",
        "hourly": "Per Hour"
    },
    "currency": {
        "format": "KES {{amount}}",
        "symbol": "KES"
    },
    "dates": {
        "today": "Today",
        "tomorrow": "Tomorrow",
        "thisWeek": "This Week",
        "nextWeek": "Next Week",
        "format": "DD/MM/YYYY",
        "timeFormat": "12-hour"
    }
}
```

**Kiswahili Translation (sw-KE.json):**
```json
{
    "navigation": {
        "home": "Nyumbani",
        "book": "Hifadhi Sasa",
        "about": "Kuhusu Sisi",
        "contact": "Mawasiliano",
        "login": "Ingia",
        "logout": "Toka"
    },
    "booking": {
        "title": "Hifadhi Nafasi Yako ya Kazi",
        "selectSpace": "Chagua Aina ya Nafasi",
        "selectDate": "Chagua Tarehe na Wakati",
        "payment": "Maelezo ya Malipo",
        "confirmation": "Uhakikisho wa Uhifadhi",
        "hotDesk": "Dawati la Kila Mtu",
        "privateOffice": "Ofisi Binafsi",
        "meetingRoom": "Chumba cha Mkutano",
        "eventSpace": "Nafasi ya Hafla"
    },
    "payment": {
        "mpesa": "Malipo ya M-Pesa",
        "phoneNumber": "Nambari ya Simu",
        "amount": "Kiasi cha Kulipa",
        "processing": "Inachakata malipo...",
        "success": "Malipo yamefanikiwa!",
        "failed": "Malipo yameshindwa. Tafadhali jaribu tena.",
        "instructions": "Ingiza nambari yako ya M-Pesa kupokea agizo la malipo"
    },
    "validation": {
        "required": "Uga huu unahitajika",
        "phoneInvalid": "Tafadhali ingiza nambari halali ya simu ya Kenya",
        "emailInvalid": "Tafadhali ingiza anwani halali ya barua pepe",
        "dateInvalid": "Tafadhali chagua tarehe halali",
        "amountInvalid": "Tafadhali ingiza kiasi halali"
    },
    "time": {
        "morning": "Asubuhi",
        "afternoon": "Mchana",
        "evening": "Jioni",
        "fullDay": "Siku Nzima",
        "halfDay": "Nusu Siku",
        "hourly": "Kwa Saa"
    },
    "currency": {
        "format": "KES {{amount}}",
        "symbol": "KES"
    },
    "dates": {
        "today": "Leo",
        "tomorrow": "Kesho",
        "thisWeek": "Wiki Hii",
        "nextWeek": "Wiki Ijayo",
        "format": "DD/MM/YYYY",
        "timeFormat": "24-hour"
    }
}
```

### 2.3 Dynamic Content Translation

**React/Vue.js Integration:**
```javascript
// Translation hook for React
function useTranslation() {
    const [language, setLanguage] = useState(languageManager.currentLanguage);
    const [translations, setTranslations] = useState(languageManager.translations);
    
    useEffect(() => {
        const handleLanguageChange = (event) => {
            setLanguage(event.detail.language);
            setTranslations(languageManager.translations);
        };
        
        window.addEventListener('languageChanged', handleLanguageChange);
        return () => window.removeEventListener('languageChanged', handleLanguageChange);
    }, []);
    
    const t = (key, params) => languageManager.t(key, params);
    
    return { t, language, setLanguage: languageManager.switchLanguage.bind(languageManager) };
}

// Usage in components
function BookingForm() {
    const { t } = useTranslation();
    
    return (
        <form>
            <h2>{t('booking.title')}</h2>
            <label htmlFor="phone">
                {t('payment.phoneNumber')} *
            </label>
            <input 
                id="phone"
                type="tel"
                placeholder={t('payment.phoneNumber')}
                aria-label={t('payment.phoneNumber')}
            />
            <button type="submit">
                {t('booking.confirm')}
            </button>
        </form>
    );
}
```

---

## 3. Cultural Localization

### 3.1 Date and Time Formatting

**Cultural Preferences:**
```javascript
class DateTimeFormatter {
    constructor(language) {
        this.language = language;
        this.setupFormats();
    }
    
    setupFormats() {
        this.formats = {
            'en-KE': {
                dateFormat: 'DD/MM/YYYY',
                timeFormat: '12-hour',
                firstDayOfWeek: 1, // Monday
                businessHours: {
                    start: '6:00 AM',
                    end: '10:00 PM',
                    format: 'h:mm A'
                }
            },
            'sw-KE': {
                dateFormat: 'DD/MM/YYYY',
                timeFormat: '24-hour',
                firstDayOfWeek: 1, // Monday
                businessHours: {
                    start: '06:00',
                    end: '22:00',
                    format: 'HH:mm'
                }
            }
        };
    }
    
    formatDate(date, options = {}) {
        const format = this.formats[this.language];
        
        if (this.language === 'sw-KE') {
            return this.formatKiswahiliDate(date, format);
        } else {
            return this.formatEnglishDate(date, format);
        }
    }
    
    formatKiswahiliDate(date, format) {
        const months = [
            'Januari', 'Februari', 'Machi', 'Aprili', 'Mei', 'Juni',
            'Julai', 'Agosti', 'Septemba', 'Oktoba', 'Novemba', 'Desemba'
        ];
        
        const days = [
            'Jumapili', 'Jumatatu', 'Jumanne', 'Jumatano', 
            'Alhamisi', 'Ijumaa', 'Jumamosi'
        ];
        
        return {
            day: days[date.getDay()],
            month: months[date.getMonth()],
            formatted: `${days[date.getDay()]}, ${date.getDate()} ${months[date.getMonth()]} ${date.getFullYear()}`
        };
    }
    
    formatEnglishDate(date, format) {
        return new Intl.DateTimeFormat('en-KE', {
            weekday: 'long',
            year: 'numeric',
            month: 'long',
            day: 'numeric'
        }).format(date);
    }
    
    getBusinessHours() {
        const format = this.formats[this.language];
        
        if (this.language === 'sw-KE') {
            return {
                opening: 'Tunafungua saa 12:00 (6:00 asubuhi)',
                closing: 'Tunafunga saa 16:00 (10:00 jioni)',
                format: '24-hour'
            };
        } else {
            return {
                opening: 'We open at 6:00 AM',
                closing: 'We close at 10:00 PM',
                format: '12-hour'
            };
        }
    }
}
```

### 3.2 Currency and Number Formatting

**Kenyan Currency Standards:**
```javascript
class CurrencyFormatter {
    constructor(language) {
        this.language = language;
    }
    
    format(amount) {
        const formatted = new Intl.NumberFormat('en-KE', {
            style: 'currency',
            currency: 'KES',
            minimumFractionDigits: 0,
            maximumFractionDigits: 2
        }).format(amount);
        
        if (this.language === 'sw-KE') {
            return this.adaptToKiswahili(formatted, amount);
        }
        
        return formatted;
    }
    
    adaptToKiswahili(formatted, amount) {
        // Kiswahili currency expressions
        const translations = {
            'KES': 'Shilingi',
            'KSh': 'Shilingi'
        };
        
        // Convert to Kiswahili format
        return `Shilingi ${amount.toLocaleString('sw-KE')}`;
    }
    
    formatPaymentInstructions(amount) {
        if (this.language === 'sw-KE') {
            return `Lipa shilingi ${amount.toLocaleString()} kupitia M-Pesa`;
        } else {
            return `Pay KES ${amount.toLocaleString()} via M-Pesa`;
        }
    }
}
```

### 3.3 Business Context Adaptation

**Cultural Business Terminology:**
```javascript
const businessTerms = {
    'en-KE': {
        businessHours: 'Business Hours',
        workingDays: 'Working Days',
        payment: 'Payment',
        booking: 'Booking',
        reservation: 'Reservation',
        availability: 'Availability',
        pricing: 'Pricing',
        membership: 'Membership',
        workspace: 'Workspace',
        facilities: 'Facilities'
    },
    'sw-KE': {
        businessHours: 'Masaa ya Kazi',
        workingDays: 'Siku za Kazi',
        payment: 'Malipo',
        booking: 'Uhifadhi',
        reservation: 'Uhifadhi',
        availability: 'Upatikanaji',
        pricing: 'Bei',
        membership: 'Uanachama',
        workspace: 'Nafasi ya Kazi',
        facilities: 'Vifaa'
    }
};

// Payment method descriptions
const paymentMethods = {
    'en-KE': {
        mpesa: {
            name: 'M-Pesa',
            description: 'Pay using your M-Pesa mobile money account',
            instructions: 'Enter your phone number to receive STK push prompt'
        },
        bank: {
            name: 'Bank Transfer',
            description: 'Direct bank transfer to our business account',
            instructions: 'Use the provided reference number for your transfer'
        },
        cash: {
            name: 'Cash Payment',
            description: 'Pay in cash at our location',
            instructions: 'Please bring exact amount or change will be provided'
        }
    },
    'sw-KE': {
        mpesa: {
            name: 'M-Pesa',
            description: 'Lipa kwa kutumia akaunti yako ya M-Pesa',
            instructions: 'Ingiza nambari yako ya simu kupokea agizo la malipo'
        },
        bank: {
            name: 'Uhamisho wa Benki',
            description: 'Hamisha pesa moja kwa moja kwenye akaunti yetu ya biashara',
            instructions: 'Tumia nambari ya marejeleo uliyopewa kwa uhamisho wako'
        },
        cash: {
            name: 'Malipo ya Taslimu',
            description: 'Lipa kwa taslimu mahali petu',
            instructions: 'Tafadhali lete kiasi kamili au chenji itatoa'
        }
    }
};
```

---

## 4. User Interface Implementation

### 4.1 Language Selector Component

**Language Toggle Interface:**
```html
<!-- Language selector component -->
<div class="language-selector">
    <button 
        class="language-toggle" 
        aria-label="Change language / Badilisha lugha"
        onclick="toggleLanguageMenu()"
    >
        <span class="current-language" id="currentLang">English</span>
        <span class="language-arrow">▼</span>
    </button>
    
    <div class="language-menu" id="languageMenu" hidden>
        <button 
            class="language-option" 
            onclick="changeLanguage('en-KE')"
            data-lang="en-KE"
        >
            🇰🇪 English (Kenya)
        </button>
        <button 
            class="language-option" 
            onclick="changeLanguage('sw-KE')"
            data-lang="sw-KE"
        >
            🇰🇪 Kiswahili (Kenya)
        </button>
    </div>
</div>

<style>
.language-selector {
    position: relative;
    display: inline-block;
}

.language-toggle {
    background: white;
    border: 2px solid #e1e5e9;
    border-radius: 8px;
    padding: 8px 12px;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 14px;
    transition: all 0.3s ease;
}

.language-toggle:hover {
    border-color: #2E86AB;
    background: #f8f9fa;
}

.language-menu {
    position: absolute;
    top: 100%;
    right: 0;
    background: white;
    border: 2px solid #e1e5e9;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    min-width: 200px;
    z-index: 1000;
}

.language-option {
    width: 100%;
    background: none;
    border: none;
    padding: 12px 16px;
    text-align: left;
    cursor: pointer;
    font-size: 14px;
    transition: background-color 0.2s ease;
}

.language-option:hover {
    background: #f8f9fa;
}

.language-option.active {
    background: #2E86AB;
    color: white;
}
</style>
```

### 4.2 Form Localization

**Bilingual Form Implementation:**
```html
<!-- Booking form with translations -->
<form class="booking-form" data-translate>
    <div class="form-section">
        <h2 data-i18n="booking.title">Book Your Workspace</h2>
        <p data-i18n="booking.subtitle">Choose your preferred space and time</p>
    </div>
    
    <div class="form-group">
        <label for="spaceType" data-i18n="booking.selectSpace">Select Space Type</label>
        <select id="spaceType" required>
            <option value="" data-i18n="common.selectOption">-- Select Option --</option>
            <option value="hot-desk" data-i18n="booking.hotDesk">Hot Desk</option>
            <option value="private-office" data-i18n="booking.privateOffice">Private Office</option>
            <option value="meeting-room" data-i18n="booking.meetingRoom">Meeting Room</option>
        </select>
        <div class="form-error" id="spaceTypeError" hidden>
            <span data-i18n="validation.required">This field is required</span>
        </div>
    </div>
    
    <div class="form-group">
        <label for="phoneNumber" data-i18n="payment.phoneNumber">Phone Number</label>
        <input 
            type="tel" 
            id="phoneNumber" 
            placeholder="254712345678"
            pattern="^254[0-9]{9}$"
            required
        >
        <div class="form-help">
            <span data-i18n="payment.phoneHelp">Enter your M-Pesa number starting with 254</span>
        </div>
        <div class="form-error" id="phoneError" hidden>
            <span data-i18n="validation.phoneInvalid">Please enter a valid Kenya phone number</span>
        </div>
    </div>
    
    <button type="submit" class="btn-primary">
        <span data-i18n="booking.confirm">Confirm Booking</span>
    </button>
</form>

<script>
// Auto-translate form on language change
window.addEventListener('languageChanged', () => {
    translateElements(document.querySelectorAll('[data-i18n]'));
});

function translateElements(elements) {
    elements.forEach(element => {
        const key = element.getAttribute('data-i18n');
        const translation = languageManager.t(key);
        
        if (element.tagName === 'INPUT' && element.type !== 'submit') {
            element.placeholder = translation;
        } else {
            element.textContent = translation;
        }
    });
}
</script>
```

### 4.3 Error Message Localization

**Contextual Error Messages:**
```javascript
class ValidationMessages {
    constructor(language) {
        this.language = language;
        this.messages = {
            'en-KE': {
                required: 'This field is required',
                phoneInvalid: 'Please enter a valid Kenya phone number (e.g., 254712345678)',
                emailInvalid: 'Please enter a valid email address',
                dateInvalid: 'Please select a future date',
                amountInvalid: 'Amount must be greater than KES 0',
                paymentFailed: 'Payment failed. Please check your M-Pesa balance and try again.',
                networkError: 'Connection error. Please check your internet and try again.',
                bookingUnavailable: 'This time slot is no longer available. Please select another time.'
            },
            'sw-KE': {
                required: 'Uga huu unahitajika',
                phoneInvalid: 'Tafadhali ingiza nambari halali ya simu ya Kenya (mfano: 254712345678)',
                emailInvalid: 'Tafadhali ingiza anwani halali ya barua pepe',
                dateInvalid: 'Tafadhali chagua tarehe ya baadaye',
                amountInvalid: 'Kiasi kinapaswa kuwa zaidi ya Shilingi 0',
                paymentFailed: 'Malipo yameshindwa. Tafadhali angalia mizani yako ya M-Pesa na ujaribu tena.',
                networkError: 'Hitilafu ya muunganisho. Tafadhali angalia mtandao wako na ujaribu tena.',
                bookingUnavailable: 'Wakati huu haupatikani tena. Tafadhali chagua wakati mwingine.'
            }
        };
    }
    
    get(key, params = {}) {
        const message = this.messages[this.language][key];
        return message ? this.interpolate(message, params) : key;
    }
    
    interpolate(message, params) {
        return message.replace(/\{\{(\w+)\}\}/g, (match, key) => {
            return params[key] !== undefined ? params[key] : match;
        });
    }
    
    showError(fieldId, errorKey, params = {}) {
        const errorElement = document.getElementById(`${fieldId}Error`);
        if (errorElement) {
            errorElement.textContent = this.get(errorKey, params);
            errorElement.removeAttribute('hidden');
            errorElement.setAttribute('role', 'alert');
        }
    }
}
```

---

## 5. SMS and Email Localization

### 5.1 SMS Template System

**Bilingual SMS Notifications:**
```javascript
class SMSTemplates {
    constructor(language) {
        this.language = language;
        this.templates = {
            'en-KE': {
                bookingConfirmation: 'Your booking at Get a Room is confirmed! Space: {{spaceType}}, Date: {{date}}, Amount: KES {{amount}}. Ref: {{reference}}',
                paymentReceived: 'Payment of KES {{amount}} received via M-Pesa. Your workspace is ready! Get a Room, Kisumu.',
                reminderDay: 'Reminder: Your booking at Get a Room is tomorrow {{date}} at {{time}}. Address: {{address}}',
                reminderHour: 'Your booking starts in 1 hour. Get a Room awaits you at {{address}}. See you soon!',
                cancellation: 'Your booking {{reference}} has been cancelled. Refund of KES {{amount}} will be processed within 24 hours.',
                modification: 'Booking updated! New details - Space: {{spaceType}}, Date: {{date}}, Time: {{time}}. Ref: {{reference}}'
            },
            'sw-KE': {
                bookingConfirmation: 'Uhifadhi wako wa Get a Room umethibitishwa! Nafasi: {{spaceType}}, Tarehe: {{date}}, Kiasi: Shilingi {{amount}}. Nambari: {{reference}}',
                paymentReceived: 'Malipo ya Shilingi {{amount}} yamepokewa kupitia M-Pesa. Nafasi yako ya kazi iko tayari! Get a Room, Kisumu.',
                reminderDay: 'Ukumbusho: Uhifadhi wako wa Get a Room ni kesho {{date}} saa {{time}}. Anwani: {{address}}',
                reminderHour: 'Uhifadhi wako unaanza baada ya saa 1. Get a Room inakungojea {{address}}. Tutaonana hivi karibuni!',
                cancellation: 'Uhifadhi wako {{reference}} umeghairiwa. Marejesho ya Shilingi {{amount}} yatachakatwa ndani ya masaa 24.',
                modification: 'Uhifadhi umesasishwa! Maelezo mapya - Nafasi: {{spaceType}}, Tarehe: {{date}}, Wakati: {{time}}. Nambari: {{reference}}'
            }
        };
    }
    
    generate(templateKey, params = {}) {
        const template = this.templates[this.language][templateKey];
        if (!template) {
            console.error(`SMS template not found: ${templateKey}`);
            return '';
        }
        
        return this.interpolate(template, this.formatParams(params));
    }
    
    formatParams(params) {
        const formatted = { ...params };
        
        // Format date according to language preference
        if (params.date) {
            const dateFormatter = new DateTimeFormatter(this.language);
            formatted.date = dateFormatter.formatDate(new Date(params.date));
        }
        
        // Format currency
        if (params.amount) {
            const currencyFormatter = new CurrencyFormatter(this.language);
            formatted.amount = currencyFormatter.format(params.amount);
        }
        
        return formatted;
    }
    
    interpolate(template, params) {
        return template.replace(/\{\{(\w+)\}\}/g, (match, key) => {
            return params[key] !== undefined ? params[key] : match;
        });
    }
}

// Usage example
const smsService = new SMSTemplates(userLanguage);
const confirmationSMS = smsService.generate('bookingConfirmation', {
    spaceType: 'Hot Desk',
    date: '2025-07-16',
    amount: 500,
    reference: 'GAR-2025-001'
});
```

### 5.2 Email Template Localization

**HTML Email Templates:**
```html
<!-- English Email Template -->
<div class="email-template" data-lang="en-KE">
    <div class="email-header">
        <h1>Get a Room - Booking Confirmation</h1>
        <p>Thank you for choosing our coworking space!</p>
    </div>
    
    <div class="email-body">
        <h2>Booking Details</h2>
        <table class="booking-details">
            <tr>
                <td><strong>Space Type:</strong></td>
                <td>{{spaceType}}</td>
            </tr>
            <tr>
                <td><strong>Date & Time:</strong></td>
                <td>{{date}} at {{time}}</td>
            </tr>
            <tr>
                <td><strong>Duration:</strong></td>
                <td>{{duration}}</td>
            </tr>
            <tr>
                <td><strong>Amount Paid:</strong></td>
                <td>KES {{amount}}</td>
            </tr>
            <tr>
                <td><strong>Reference Number:</strong></td>
                <td>{{reference}}</td>
            </tr>
        </table>
        
        <div class="location-info">
            <h3>Location & Contact</h3>
            <p><strong>Address:</strong> Get a Room Coworking Space<br>
            [Street Address], Kisumu, Kenya</p>
            <p><strong>Phone:</strong> +254 XXX XXX XXX</p>
            <p><strong>Email:</strong> info@getaroom.co.ke</p>
        </div>
        
        <div class="important-notes">
            <h3>Important Information</h3>
            <ul>
                <li>Please arrive 15 minutes early for check-in</li>
                <li>Bring a valid ID for verification</li>
                <li>WiFi password will be provided at reception</li>
                <li>Cancellations must be made 24 hours in advance</li>
            </ul>
        </div>
    </div>
    
    <div class="email-footer">
        <p>Thank you for choosing Get a Room!</p>
        <p>For any questions, please contact us at info@getaroom.co.ke</p>
    </div>
</div>

<!-- Kiswahili Email Template -->
<div class="email-template" data-lang="sw-KE">
    <div class="email-header">
        <h1>Get a Room - Uhakikisho wa Uhifadhi</h1>
        <p>Asante kwa kuchagua nafasi yetu ya kazi pamoja!</p>
    </div>
    
    <div class="email-body">
        <h2>Maelezo ya Uhifadhi</h2>
        <table class="booking-details">
            <tr>
                <td><strong>Aina ya Nafasi:</strong></td>
                <td>{{spaceType}}</td>
            </tr>
            <tr>
                <td><strong>Tarehe na Wakati:</strong></td>
                <td>{{date}} saa {{time}}</td>
            </tr>
            <tr>
                <td><strong>Muda:</strong></td>
                <td>{{duration}}</td>
            </tr>
            <tr>
                <td><strong>Kiasi Kilicholipwa:</strong></td>
                <td>Shilingi {{amount}}</td>
            </tr>
            <tr>
                <td><strong>Nambari ya Marejeleo:</strong></td>
                <td>{{reference}}</td>
            </tr>
        </table>
        
        <div class="location-info">
            <h3>Mahali na Mawasiliano</h3>
            <p><strong>Anwani:</strong> Get a Room Nafasi ya Kazi Pamoja<br>
            [Anwani ya Mtaa], Kisumu, Kenya</p>
            <p><strong>Simu:</strong> +254 XXX XXX XXX</p>
            <p><strong>Barua Pepe:</strong> info@getaroom.co.ke</p>
        </div>
        
        <div class="important-notes">
            <h3>Taarifa Muhimu</h3>
            <ul>
                <li>Tafadhali fika dakika 15 mapema kwa usajili</li>
                <li>Leta kitambulisho halali kwa uthibitisho</li>
                <li>Nenosiri la WiFi litatolewa kwenye mapokezi</li>
                <li>Kughairi kunapaswa kufanywa masaa 24 mapema</li>
            </ul>
        </div>
    </div>
    
    <div class="email-footer">
        <p>Asante kwa kuchagua Get a Room!</p>
        <p>Kwa maswali yoyote, tafadhali wasiliana nasi kupitia info@getaroom.co.ke</p>
    </div>
</div>
```

---

## 6. Search Engine Optimization (SEO)

### 6.1 Multilingual SEO Implementation

**Language-Specific Meta Tags:**
```html
<!-- English version -->
<html lang="en-KE">
<head>
    <title>Get a Room - Coworking Space Booking in Kisumu, Kenya</title>
    <meta name="description" content="Book affordable coworking spaces in Kisumu. Hot desks, private offices, meeting rooms. Pay with M-Pesa. Available 6 AM - 10 PM daily.">
    <meta name="keywords" content="coworking space Kisumu, office rental Kenya, M-Pesa booking, workspace Kisumu, hot desk Kenya">
    
    <!-- Hreflang for language targeting -->
    <link rel="alternate" hreflang="en-KE" href="https://getaroom.co.ke/en/">
    <link rel="alternate" hreflang="sw-KE" href="https://getaroom.co.ke/sw/">
    <link rel="alternate" hreflang="x-default" href="https://getaroom.co.ke/">
    
    <!-- Open Graph for social sharing -->
    <meta property="og:title" content="Get a Room - Coworking Space Kisumu">
    <meta property="og:description" content="Book your perfect workspace in Kisumu. Affordable rates, M-Pesa payments, professional environment.">
    <meta property="og:url" content="https://getaroom.co.ke/en/">
    <meta property="og:locale" content="en_KE">
    <meta property="og:locale:alternate" content="sw_KE">
</head>

<!-- Kiswahili version -->
<html lang="sw-KE">
<head>
    <title>Get a Room - Nafasi za Kazi Pamoja Kisumu, Kenya</title>
    <meta name="description" content="Hifadhi nafasi za kazi pamoja bei nafuu Kisumu. Madawati ya kila mtu, maofisi binafsi, vyumba vya mikutano. Lipa kwa M-Pesa. Tupo 6 AM - 10 PM kila siku.">
    <meta name="keywords" content="nafasi ya kazi pamoja Kisumu, kukodi ofisi Kenya, uhifadhi M-Pesa, nafasi ya kazi Kisumu, dawati la kila mtu Kenya">
    
    <link rel="alternate" hreflang="en-KE" href="https://getaroom.co.ke/en/">
    <link rel="alternate" hreflang="sw-KE" href="https://getaroom.co.ke/sw/">
    <link rel="alternate" hreflang="x-default" href="https://getaroom.co.ke/">
    
    <meta property="og:title" content="Get a Room - Nafasi za Kazi Pamoja Kisumu">
    <meta property="og:description" content="Hifadhi nafasi yako kamili ya kazi Kisumu. Bei nafuu, malipo ya M-Pesa, mazingira ya kitaaluma.">
    <meta property="og:url" content="https://getaroom.co.ke/sw/">
    <meta property="og:locale" content="sw_KE">
    <meta property="og:locale:alternate" content="en_KE">
</head>
```

### 6.2 URL Structure for Languages

**Language-Specific URL Strategy:**
```
Primary Domain: getaroom.co.ke

URL Structure:
├── getaroom.co.ke/              (Default - redirects to user preference)
├── getaroom.co.ke/en/           (English version)
│   ├── en/book/                 (Booking page)
│   ├── en/spaces/               (Space listings)
│   ├── en/pricing/              (Pricing information)
│   └── en/about/                (About us)
├── getaroom.co.ke/sw/           (Kiswahili version)
│   ├── sw/hifadhi/              (Booking page - "hifadhi")
│   ├── sw/nafasi/               (Space listings - "nafasi")
│   ├── sw/bei/                  (Pricing - "bei")
│   └── sw/kuhusu/               (About us - "kuhusu")
└── getaroom.co.ke/api/          (API endpoints - language neutral)
```

---

## 7. Testing and Quality Assurance

### 7.1 Translation Quality Testing

**Review Process:**
```javascript
class TranslationQA {
    constructor() {
        this.reviewChecklist = {
            linguisticAccuracy: [
                'Grammar and syntax correctness',
                'Spelling and punctuation',
                'Terminology consistency',
                'Cultural appropriateness',
                'Natural language flow'
            ],
            technicalValidation: [
                'Variable interpolation works',
                'Character encoding (UTF-8)',
                'Text length fits UI elements',
                'Special characters display correctly',
                'Pluralization rules applied'
            ],
            contextualReview: [
                'Business terminology accuracy',
                'Cultural sensitivity check',
                'Local business practice alignment',
                'Payment method descriptions',
                'Legal compliance (contracts, privacy)'
            ]
        };
    }
    
    validateTranslation(key, englishText, kiswahiliText) {
        const issues = [];
        
        // Check for missing interpolation variables
        const englishVars = this.extractVariables(englishText);
        const kiswahiliVars = this.extractVariables(kiswahiliText);
        
        if (englishVars.length !== kiswahiliVars.length) {
            issues.push(`Variable count mismatch: ${key}`);
        }
        
        // Check for excessive length difference (may indicate missing content)
        const lengthRatio = kiswahiliText.length / englishText.length;
        if (lengthRatio < 0.5 || lengthRatio > 2.0) {
            issues.push(`Unusual length difference: ${key} (ratio: ${lengthRatio})`);
        }
        
        return issues;
    }
    
    extractVariables(text) {
        const matches = text.match(/\{\{(\w+)\}\}/g);
        return matches ? matches.map(m => m.replace(/[{}]/g, '')) : [];
    }
    
    generateQAReport(translations) {
        const report = {
            totalKeys: 0,
            translatedKeys: 0,
            missingTranslations: [],
            potentialIssues: []
        };
        
        for (const [key, englishText] of Object.entries(translations.en)) {
            report.totalKeys++;
            
            const kiswahiliText = translations.sw[key];
            if (!kiswahiliText) {
                report.missingTranslations.push(key);
                continue;
            }
            
            report.translatedKeys++;
            
            const issues = this.validateTranslation(key, englishText, kiswahiliText);
            if (issues.length > 0) {
                report.potentialIssues.push({ key, issues });
            }
        }
        
        return report;
    }
}
```

### 7.2 User Acceptance Testing

**Bilingual Testing Protocol:**
```markdown
## Language Testing Checklist

### Pre-Testing Setup
- [ ] Test on multiple devices (Android, iOS, Desktop)
- [ ] Test on different browsers (Chrome, Firefox, Safari)
- [ ] Verify font support for Kiswahili characters
- [ ] Test with screen readers in both languages

### Functional Testing
- [ ] Language selector works correctly
- [ ] Language preference persists across sessions
- [ ] All UI elements translate properly
- [ ] Form validation messages appear in correct language
- [ ] Error messages are culturally appropriate
- [ ] Date/time formats match language conventions

### Content Testing
- [ ] Business terminology is accurate
- [ ] Payment instructions are clear in both languages
- [ ] Cultural references are appropriate
- [ ] Legal text complies with local requirements
- [ ] Contact information is correctly localized

### User Experience Testing
- [ ] Native speakers can complete booking flow easily
- [ ] Business context is clear to Kenyan users
- [ ] SMS and email communications are professional
- [ ] Customer support scenarios work in both languages
- [ ] Cultural sensitivity maintained throughout
```

---

## 8. Implementation Timeline

### Phase 1: Foundation (Weeks 1-2)
- [ ] **Technical Setup**
  - i18n framework implementation
  - Translation file structure
  - Language detection logic
  - Basic UI language switching

- [ ] **Core Content Translation**
  - Navigation and common elements
  - Booking flow translation
  - Form labels and validation messages
  - Basic error messages

### Phase 2: Content & Localization (Weeks 3-4)
- [ ] **Advanced Translation**
  - Complete UI translation
  - Email and SMS templates
  - Help documentation
  - Legal terms and privacy policy

- [ ] **Cultural Adaptation**
  - Date/time formatting
  - Currency display
  - Business hour terminology
  - Cultural context adaptation

### Phase 3: Testing & Optimization (Weeks 5-6)
- [ ] **Quality Assurance**
  - Native speaker review
  - Cultural sensitivity review
  - Technical validation
  - User acceptance testing

- [ ] **SEO & Marketing**
  - Multilingual SEO implementation
  - Social media content
  - Marketing material translation
  - Search engine optimization

---

## 9. Maintenance and Updates

### Ongoing Translation Management
- **Monthly Reviews:** Check for new content requiring translation
- **Quarterly Updates:** Review and update existing translations
- **Annual Assessment:** Comprehensive language strategy review
- **Feedback Integration:** Incorporate user feedback on translations

### Success Metrics
- **User Engagement:** Track language preference usage patterns
- **Conversion Rates:** Compare booking completion rates by language
- **Customer Satisfaction:** Monitor support feedback by language
- **Content Performance:** Analyze which translations drive better engagement

---

## 10. Budget and ROI

### Implementation Costs
- **Professional Translation:** KES 180,000 (one-time)
- **Cultural Consultant:** KES 120,000 (one-time)
- **Technical Implementation:** 40 hours development time
- **Testing and QA:** KES 80,000 (one-time)
- **Total Initial Investment:** KES 380,000

### Ongoing Costs
- **Translation Updates:** KES 15,000/month
- **Cultural Review:** KES 10,000/quarter
- **Technical Maintenance:** 5 hours/month
- **Total Annual Cost:** KES 220,000

### Expected ROI
- **Market Expansion:** +35% user base (Kiswahili speakers)
- **Increased Conversions:** +25% booking completion rate
- **Customer Satisfaction:** +30% positive feedback
- **Revenue Impact:** +KES 125,000/month additional revenue
- **Annual ROI:** 580%

---

*This multi-language framework ensures Get a Room serves Kenya's diverse linguistic community while maintaining cultural sensitivity and business professionalism. Regular updates and community feedback will ensure continued effectiveness and cultural relevance.*

**Document Classification:** Technical Specification  
**Review Schedule:** Quarterly  
**Next Review Date:** October 16, 2025