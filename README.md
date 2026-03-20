#  Problem Statement

Gig delivery workers face income loss (20–30%) due to external disruptions like weather, pollution, and curfews, with no financial protection available.

The goal is to build an AI-powered parametric insurance platform that provides automated income protection, using weekly pricing, real-time triggers, and fraud detection, ensuring quick and reliable payouts for lost earnings.

---

##  Problems Identified

1. Lack of income protection for gig workers during disruptions  
2. Frequent income loss (20–30%) due to external factors like weather, pollution, and curfews  
3. No existing insurance focused on income loss (only health/vehicle coverage exists)  
4. Unpredictable working conditions affecting daily earnings  
5. Absence of automated claim and payout systems  
6. High risk of financial instability for delivery partners  
7. No real-time monitoring of disruption events  
8. Possibility of fraudulent claims without proper detection systems  
9. Mismatch between traditional insurance models and weekly earning cycle of gig workers


##  Stakeholders

1. Delivery Partners (Gig Workers)  
   Face high income volatility and lack of social security due to unpredictable daily earnings.

2. E-commerce & Q-commerce Platforms (e.g., Amazon, Flipkart, Zepto)  
   Depend on a stable workforce for last-mile delivery; high partner turnover leads to increased recruitment costs and delivery delays.

3. Gig Platforms (e.g., Swiggy, Zomato, Uber)  
   Experience rider churn and reduced service reliability when workers shift to more stable jobs.

4. Traditional Insurance Providers  
   Struggle to offer micro-premiums or real-time coverage aligned with the dynamic risk faced by delivery partners.

5. End Consumers  
   Affected by surge pricing and inconsistent delivery timelines when delivery partner availability is unstable.

6. Government & Regulatory Bodies  
   Face challenges in supporting a large workforce without formal employment benefits or financial protection.

7. Financial Institutions  
   Often deny loans or credit due to the lack of stable and provable income among gig workers.


## User Persona

### Persona: E-commerce Delivery Partner

- Name: MURUGAN R  
- Age: 29 
- Location: SATHYAMANGALAM   
- Occupation: Full-time E-commerce Delivery Executive  
- Platform: Amazon / Flipkart  
- Monthly Income: ₹23,000 – ₹27,000  
- Work Type: Daily deliveries with flexible hours  

### Work Pattern
- Works 6–7 days a week  
- Earnings depend on number of deliveries completed  
- Paid on a weekly or task-based model  

### Pain Points
- Income drops during heavy rain, extreme heat, or curfews  
- No earnings when unable to work due to external disruptions  
- No insurance coverage for income loss  
- Financial stress due to unpredictable income  

### Needs
- Stable and predictable income  
- Protection against income loss  
- Quick and easy claim process  
- Instant payout during disruptions  

### Goals
- Maximize daily earnings  
- Ensure financial stability  
- Reduce risk during uncontrollable situations  

### Tech Behavior
- Uses smartphone daily for delivery apps  
- Comfortable with basic mobile applications  
- Prefers simple and quick interfaces

##  End-to-End Workflow

### 1. User Onboarding
- User downloads and opens the mobile application  
- Registers using mobile number / email  
- Completes profile (location, work type, platform)  

### 2. Policy Selection
- User selects income protection plan  
- Chooses weekly coverage amount  
- System shows estimated premium  

### 3. Premium Calculation
- System analyzes:
  - Location-based risk (weather, pollution, etc.)  
  - Historical disruption data  
- AI calculates weekly premium dynamically  

### 4. Policy Activation
- User pays weekly premium  
- Policy gets activated for the selected duration  

### 5. Real-Time Monitoring
- System continuously monitors:
  - Weather conditions  
  - Environmental data  
  - External disruptions  

### 6. Trigger Detection
- If any parameter crosses threshold (e.g., heavy rain):  
  - System identifies disruption  
  - Matches with user’s coverage  

### 7. Automatic Claim Initiation
- Claim is triggered automatically  
- No manual request required from user  

### 8. Fraud Detection
- System verifies:
  - User location (GPS validation)  
  - Activity status  
  - Duplicate claims  
- Suspicious claims are flagged  

### 9. Claim Approval & Payout
- Valid claims are approved instantly  
- Payment is processed via gateway  
- User receives compensation directly  

### 10. User Notification & Dashboard
- User gets notification of claim and payout  
- Dashboard shows:
  - Active policy  
  - Earnings protected  
  - Claim history

##  Premium Plan

The platform offers simple weekly insurance plans designed for delivery partners. These plans are affordable and provide income protection during unexpected disruptions.

### Base Plans

- Basic Plan  
  ₹20/week → Covers up to ₹500  

- Standard Plan  
  ₹40/week → Covers up to ₹1000  

- Premium Plan  
  ₹60/week → Covers up to ₹1500  

### How It Works

The premium is calculated weekly based on the selected plan and the risk level in the user’s area.

For example:
- If the weather conditions are normal → lower premium  
- If heavy rain or extreme heat is expected → slightly higher premium  
This ensures fair pricing based on real conditions.

### Triggers for Claim
The system automatically triggers claims when certain conditions are met:
- Heavy rainfall  
- High temperature  
- Pollution levels  
- Curfew or restricted movement  
No manual claim process is required.

### Add-ons (Optional)
Users can choose extra protection:
- Pollution coverage → +₹10  
- Curfew coverage → +₹15  

### Benefits
- Simple weekly payment  
- No paperwork for claims  
- Quick payout during disruptions  
- Flexible plans based on user needs  

### FUTURE
In future, delivery platforms can also contribute to the premium to support their workers and improve retention.


## AI/ML Integration Plan

### 1. AI-Based Premium Calculation
The system uses AI/ML models to dynamically calculate weekly premiums based on risk factors.
#### Inputs:
- Historical weather data (rainfall, temperature)
- Location-based risk levels
- Past disruption frequency
- User activity patterns (working hours, delivery zones)
#### Approach:
- Use regression or predictive models to estimate risk score
- Classify users into low, medium, high-risk categories
#### Output:
- Dynamically adjusted weekly premium
- Personalized pricing for each user

### 2. Predictive Risk Modeling
AI predicts the likelihood of disruptions before they occur.
#### Inputs:
- Weather forecasts (API data)
- Seasonal trends (monsoon, heatwaves)
- Historical disruption patterns
#### Approach:
- Time-series forecasting or classification models
- Generate weekly risk predictions
#### Output:
- Early warning of high-risk periods
- Adjusted premiums and coverage recommendations

### 3. Automated Claim Triggering (Parametric AI Logic)
Claims are triggered automatically based on predefined conditions.
#### Inputs:
- Real-time weather/environmental data
- Predefined thresholds (e.g., rainfall > limit)
#### Approach:
- Rule-based + AI validation system
- Compare live data with trigger conditions
#### Output:
- Automatic claim initiation
- Zero manual intervention required

### 4. Intelligent Fraud Detection
AI ensures only valid claims are processed.
#### Inputs:
- User GPS/location data
- Activity logs
- Claim history
#### Approach:
- Anomaly detection models
- Pattern recognition to identify suspicious behavior
#### Fraud Checks:
- Location mismatch detection
- Duplicate claims detection
- Unusual claim frequency
#### Output:
- Flagged suspicious claims
- Only genuine claims are approved

### 5. User Behavior Analysis
AI analyzes user patterns to improve system efficiency.
#### Inputs:
- App usage data
- Claim patterns
- Working hours
#### Output:
- Personalized recommendations
- Better premium adjustment
- Improved user experience

##  Tech Stack

### 1. Frontend (User Interface)
- React Native  
- Provides a mobile-friendly interface for delivery partners  
- Enables real-time notifications and smooth user experience  

### 2. Backend (Server-Side Logic)
- Node.js / Java (Spring Boot)  
- Handles business logic, API requests, and system operations  
- Manages premium calculation, claims processing, and user data  

### 3. Database
- MongoDB / MySQL  
- Stores user profiles, policy details, claims history, and transaction data  

### 4. APIs & Integrations
- Weather API  
  - Provides real-time and forecast weather data for trigger detection  

- Location Services (GPS)  
  - Used for user verification and fraud detection  

- Payment Gateway (Razorpay / Stripe - Test Mode)  
  - Handles secure payout processing  

### 5. AI/ML Tools
- Python (Scikit-learn / TensorFlow)  
- Used for:
  - Risk prediction  
  - Dynamic premium calculation  
  - Fraud detection  

### 6. Development Tools
- GitHub  
  - Version control and collaboration  

- Postman  
  - API testing
    
- Firebase (Optional)  
  - Authentication and notifications  

### 7. Deployment
- Cloud Platforms (AWS / Firebase / Render)  
- Ensures scalability, reliability, and real-time access


## Adversarial Defense & Anti-Spoofing Strategy

To prevent misuse of the system through GPS spoofing and coordinated fraud, the platform uses multiple validation layers instead of relying only on location data.

---

### 1. Differentiation Strategy

The system differentiates between genuine users and fraudulent actors by analyzing behavior patterns rather than just location.

- Real users show consistent movement (delivery routes, stops, activity patterns)  
- Spoofed users often show unrealistic or static movement (no actual travel)  

AI models compare:
- Movement consistency  
- Speed and route patterns  
- Activity vs claimed disruption  

This helps identify whether the user is actually working in that location or faking presence.

---

### 2. Data Points Used for Fraud Detection

The system uses multiple data sources beyond GPS:

- Device motion data (accelerometer, step movement)  
- App activity logs (screen usage, session timing)  
- Network signals (IP address consistency, network type)  
- Delivery activity (orders completed, route history)  
- Time patterns (sudden claims from multiple users at same time)  
- Historical claim behavior  

By combining these signals, the system detects:
- Fake stationary users  
- Mass coordinated claims  
- Unusual claim spikes in specific regions  

---

### 3. Handling Flagged Claims (UX Balance)

The system ensures that genuine users are not unfairly penalized.

- Claims are not immediately rejected  
- Suspicious cases are marked as "Under Review"  
- Users are asked for lightweight verification (e.g., app activity confirmation)  

For genuine users:
- Claims are processed with minimal delay  

For suspicious cases:
- Additional validation is applied before payout  

This approach balances:
- Fraud prevention  
- User trust and fairness  

  
