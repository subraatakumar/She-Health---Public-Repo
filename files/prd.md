# Product Requirements Document (PRD) - She Health

## Overview
**She Health** is a React Native mobile application designed to help women track their menstrual cycle with privacy and ease. The app enables users to log period-related details and export data for medical consultations. All data is stored locally on the user's device to ensure privacy.

## Features
### 1. **Period Tracking**
- Users can log their period start and end dates.
- Track daily flow intensity (e.g., light, medium, heavy).
- Add optional notes for symptoms (e.g., cramps, mood swings, headaches).
- Edit or delete entries as needed.

### 2. **Data Export & Sharing**
- Users can generate a report of their period data for:
  - Last 3 months
  - Last 6 months
  - Last 1 year
- Exported data will be in a structured format (e.g., PDF or CSV).
- Users can share this report via email, messaging apps, or cloud storage services.

### 3. **Local Data Storage**
- All user data is stored locally on the device.
- No backend server is required, ensuring complete privacy.
- Data remains accessible even without an internet connection.

## Future Enhancements (Not in Initial Release)
- **Ovulation and Fertility Tracking**: Predict fertile windows and ovulation days.
- **Reminders & Notifications**: Custom alerts for period start, ovulation, and medication reminders.
- **Symptom Analysis**: Insights into recurring symptoms and trends.
- **Health Insights Dashboard**: Visual representation of cycle patterns over time.
- **Doctor Consultation Integration**: Securely connect with healthcare professionals.

## Tech Stack
- **Framework**: React Native
- **State Management**: React Context API or Redux
- **Local Storage**: AsyncStorage or SQLite
- **File Export**: PDFKit or react-native-fs for generating reports
- **UI Library**: React Native Paper or NativeBase

## Target Audience
- Women of all age groups who want to track their menstrual cycle.
- Users who prefer a privacy-focused period tracking app.
- Individuals looking for a simple and intuitive period log with data export functionality.

## Release Plan
### **MVP (Minimum Viable Product) Scope**
- Period tracking (start & end date, flow level, notes)
- Data export (3, 6, 12 months)
- Local storage implementation

### **Future Versions**
- Additional tracking features (symptoms, ovulation, etc.)
- Reminders and notifications
- Doctor consultation integration

## Success Metrics
- Number of active users tracking periods regularly.
- Number of data exports performed.
- User feedback and ratings.
- App retention rate over 3-6 months.

## Privacy & Security Considerations
- No user data is stored on servers.
- Users have full control over their data.
- Data export requires explicit user action.

## Conclusion
She Health aims to provide a secure and user-friendly way for women to track their menstrual health without privacy concerns. The app focuses on simplicity, ease of use, and complete local data storage, ensuring users maintain full control over their health records.
