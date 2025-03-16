## 🚀 Implementing Version Checking and Update Handling in a React Native App  

Keeping your app up to date is critical for ensuring users get the latest features, performance improvements, and bug fixes. In this article, I’ll walk you through how I implemented **version checking and update handling** in my React Native app, **She Health**. This feature ensures that users are always notified about new updates and are redirected to the App Store or Play Store for the latest version. I'll cover everything — the **challenges**, the **solution**, and why this is important for a great user experience.  

---

## 🎯 **Goal**  
The goal was to implement a feature that:  
✅ Checks the app version at startup against the latest version available from the backend.  
✅ If the versions don't match, show an **update prompt** to the user.  
✅ Redirect the user to the appropriate store (App Store/Play Store) to update the app.  
✅ Offer an "Update Later" button only for patch updates (minor version increases).  
✅ Include a **"View Release Notes"** option so the user can see what’s new.  
✅ Ensure that the update alert keeps showing until the app is updated — even if the user navigates away after pressing "Update Now."  

---

## 🏗️ **Project Setup**  
### **Backend API Response**  
The backend exposes an endpoint that provides the latest version details:  
➡️ **Endpoint:** `https://backend.shehealth.subraatakumar.com/api/v1/getversiondetails`  

Example response from the backend:  
```json
{
  "version": "1.0.0001",
  "releaseNotesLink": "https://shehealth.subraatakumar.com/release-notes",
  "playStoreLink": "https://play.google.com/store/apps/details?id=com.subrata.shehealth&hl=en",
  "appStoreLink": "https://apps.apple.com/in/app/"
}
```

### **Current Version in App**  
The app stores the current version in a `configs.versionDetails` object:  
```ts
import { configs } from '@utils/configdata';

configs.versionDetails = {
  version: '1.0.0000',
  releaseNotesLink: 'https://shehealth.subraatakumar.com/release-notes',
  playStoreLink: 'https://play.google.com/store/apps/details?id=com.subrata.shehealth&hl=en',
  appStoreLink: 'https://apps.apple.com/in/app/'
};
```

---

## 💡 **Challenge**  
1. **Handling dismissed alerts:**  
   - Initially, the alert would disappear after the user clicked on "Update Now."  
   - If the user navigated back to the app without updating, no further prompt would appear.  

2. **Managing different update types:**  
   - Minor updates (patch versions) should allow the user to "Update Later."  
   - Major updates should force the user to update immediately.  

3. **Providing detailed release notes:**  
   - Users need to see what they’re getting in the new update before committing to it.  

---

## ✅ **Solution**  
I created a `VersionChecker` component that:  
✔️ Fetches the latest version details from the backend using `axios`.  
✔️ Compares the current version with the latest version.  
✔️ Differentiates between patch and major updates using semantic versioning.  
✔️ Shows an alert with **Update Now**, **Update Later** (if it’s a patch), and **View Release Notes** options.  
✔️ Keeps the alert state alive using `AppState` so that it reappears if the user navigates away without updating.  

---

### 🔥 **Code Implementation**  
Here’s the final implementation of the `VersionChecker` component:  

```tsx
import React, { useEffect, useState } from 'react';
import { Alert, Platform, Linking, AppState } from 'react-native';
import axios from 'axios';
import { configs } from '@utils/configdata';

const VersionChecker = () => {
  const [latestVersion, setLatestVersion] = useState<string | null>(null);
  const [isModalVisible, setIsModalVisible] = useState(false);

  useEffect(() => {
    checkVersion();

    // ✅ Re-check version when app comes to foreground
    const subscription = AppState.addEventListener('change', state => {
      if (state === 'active') {
        checkVersion();
      }
    });

    return () => subscription.remove();
  }, []);

  const checkVersion = async () => {
    try {
      const { data } = await axios.get('https://backend.shehealth.subraatakumar.com/api/v1/getversiondetails');

      const currentVersion = configs.versionDetails.version;

      if (currentVersion !== data.version) {
        setLatestVersion(data.version);

        if (!isModalVisible) {
          const isPatchUpdate = isPatchVersion(currentVersion, data.version);
          showUpdateModal(data, isPatchUpdate);
        }
      }
    } catch (error) {
      console.error('Failed to fetch version details:', error);
    }
  };

  const isPatchVersion = (currentVersion: string, latestVersion: string) => {
    const [cMajor, cMinor, cPatch] = currentVersion.split('.').map(Number);
    const [lMajor, lMinor, lPatch] = latestVersion.split('.').map(Number);

    return cMajor === lMajor && cMinor === lMinor && lPatch > cPatch;
  };

  const showUpdateModal = (data: any, isPatchUpdate: boolean) => {
    setIsModalVisible(true);

    Alert.alert(
      'Update Available',
      `A new version (${data.version}) is available. Please update to continue using the app.`,
      [
        {
          text: 'View Release Notes',
          onPress: () => Linking.openURL(data.releaseNotesLink),
        },
        ...(isPatchUpdate
          ? [
              {
                text: 'Update Later',
                onPress: () => setIsModalVisible(false),
                style: 'cancel',
              },
            ]
          : []),
        {
          text: 'Update Now',
          onPress: () => {
            const url = Platform.OS === 'ios' ? data.appStoreLink : data.playStoreLink;
            Linking.openURL(url);
            setTimeout(() => {
              setIsModalVisible(false);
            }, 1000);
          },
        },
      ]
    );
  };

  return null;
};

export default VersionChecker;
```

---

### 🛠️ **How It Works:**  
1. **Fetching Version Details** – The app fetches the latest version from the backend using `axios`.  
2. **Comparison Logic** – The `isPatchVersion` function checks if it's a major or minor update.  
3. **Re-Appear After Dismissal** – `AppState` keeps checking the version each time the app comes back to the foreground.  
4. **Multiple Button Handling** –  
   - "Update Now" → Redirects to the App Store/Play Store.  
   - "Update Later" → Available only for patch updates.  
   - "View Release Notes" → Opens the release notes link.  

---

## 🌟 **Why This Matters**  
✅ **User Experience:**  
- A smooth update experience builds user trust.  
- Providing detailed release notes enhances transparency and engagement.  

✅ **Performance and Security:**  
- Encouraging updates ensures users have the latest bug fixes and security patches.  

✅ **User Retention:**  
- A seamless update process reduces app crashes and improves user satisfaction.  

---

## 🚀 **Final Thoughts**  
This implementation makes sure that:  
✔️ The app stays updated without annoying the user.  
✔️ Users can make an informed decision before updating.  
✔️ The app gracefully handles different update types.  

👉 If you’re working on a React Native app, adding a version checker like this is highly recommended! 😎  

---

## 🔗 **Check Out the Latest Release Notes Here:**  
➡️ [https://shehealth.subraatakumar.com/release-notes](https://shehealth.subraatakumar.com/release-notes)  

---

If you found this helpful, drop a like or share it with someone working on React Native apps! 🙌
