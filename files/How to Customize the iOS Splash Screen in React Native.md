# 🚀 How to Customize the iOS Splash Screen in React Native – 

When you open a React Native app on iOS, you’ve probably seen the default white splash screen with the app name and "Powered by React Native" at the bottom. While this is functional, it’s not very **professional** or **brand-focused**.

For an app like **She Health**, which focuses on delivering a seamless and polished user experience, we needed to make the splash screen more aligned with our brand identity. Instead of showing the default text, we wanted to display:

> **"Crafted with Care by Subrata"**

This article will walk you through how we customized the splash screen for iOS, ensuring that it looks professional, works well in both light and dark mode, and provides a consistent user experience. This guide will be helpful for freshers and those looking to enhance the first impression of their apps. 😎

---

## 🛠️ **Why is the Splash Screen Important?**

A splash screen is the **first thing users see** when they open your app. It sets the tone for the user experience and gives a sense of polish and professionalism. A well-designed splash screen can:\
✅ Reinforce brand identity\
✅ Make the app feel more premium\
✅ Provide a smooth transition into the main UI

---

## 🎯 **Goal**

✅ Remove the default React Native splash screen text.\
✅ Replace it with a custom tagline: **"Crafted with Care by Subrata."**\
✅ Ensure the splash screen works in both light and dark modes.\
✅ Prevent UI distortion on different screen sizes.

---

## 📲 **Step-by-Step Implementation**

### **1. Open the iOS Project in Xcode**

You can’t modify the splash screen directly from the React Native project — you need to open the iOS project in **Xcode**:

```bash
npx react-native run-ios --simulator="iPhone 14"
```

Then, in Xcode, navigate to:

```
ios ➔ YourAppName ➔ Base.lproj ➔ LaunchScreen.storyboard
```

---

### **2. Modify the Storyboard**

1. Open the `LaunchScreen.storyboard` file in **Xcode**.
2. Delete the default "Powered by React Native" label.
3. Drag a new **UILabel** onto the screen and set the text to:
   > **"Crafted with Care by Subrata"**
4. Adjust the font, color, and size to match your app's theme:
   - Font: `System Bold 18`
   - Color: Matching She Health's **primary color** (`#D75A7B`)
5. Use **Auto Layout** to center the label horizontally and vertically:
   - Align **center** vertically
   - Align **center** horizontally

---

### **3. Update Splash Screen Behavior in `AppDelegate.m`**

To make the splash screen dynamic and programmatically adjust its appearance, modify the `AppDelegate.m` file:

👉 Open `AppDelegate.m` in:

```
ios/YourAppName/AppDelegate.m
```

👉 Add the following code:

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  // Load the Launch Screen from Storyboard
  UIView* launchScreen = [[NSBundle mainBundle] loadNibNamed:@"LaunchScreen" owner:self options:nil][0];
  launchScreen.frame = self.window.bounds;

  // Add Custom Label
  UILabel *customLabel = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, self.window.bounds.size.width, 40)];
  customLabel.text = @"Crafted with Care by Subrata";
  customLabel.textAlignment = NSTextAlignmentCenter;
  customLabel.font = [UIFont boldSystemFontOfSize:18];
  customLabel.textColor = [UIColor colorWithRed:0.84 green:0.35 blue:0.48 alpha:1.0]; // She Health Primary Color

  [launchScreen addSubview:customLabel];
  customLabel.center = CGPointMake(self.window.bounds.size.width / 2, self.window.bounds.size.height / 2);

  self.window.rootViewController.view = launchScreen;
  [self.window makeKeyAndVisible];

  return YES;
}
```

---

### **4. Add Dark Mode Support**

To adjust the label color based on light or dark mode, modify the code like this:

```objc
if (@available(iOS 13.0, *)) {
    if (self.window.traitCollection.userInterfaceStyle == UIUserInterfaceStyleDark) {
        customLabel.textColor = [UIColor colorWithRed:0.72 green:0.12 blue:0.37 alpha:1.0]; // Dark mode color
    } else {
        customLabel.textColor = [UIColor colorWithRed:0.84 green:0.35 blue:0.48 alpha:1.0]; // Light mode color
    }
}
```

---

### **5. Clean and Rebuild the Project**

Finally, clean the project and rebuild it to see the changes in action:

```bash
cd ios && xcodebuild clean
npx react-native run-ios
```

---

## 🖌️ **Final Result**

✅ The splash screen now shows a clean, branded message:

> **"Crafted with Care by Subrata"**

✅ The text is center-aligned and consistent with She Health’s theme.\
✅ It works in both **light** and **dark** modes.\
✅ No distortion or overflow issues on different screen sizes.

---

## ✅ **Why This Matters**

1. **First impressions matter** – A professional splash screen sets the tone for the entire app experience.
2. **Reinforces brand identity** – Seeing the "Crafted with Care by Subrata" tagline establishes a connection with the brand.
3. **Smooth transition** – The splash screen leads into the main UI without any jarring visual experience.

---

## 🚀 **Key Takeaways**

✔️ Always customize the splash screen to reflect your brand identity.\
✔️ Use `LaunchScreen.storyboard` for design flexibility.\
✔️ Adjust for dark mode to maintain consistency across themes.\
✔️ Test on multiple screen sizes to avoid overflow or alignment issues.

---

## 🎯 **Next Steps**

✅ Test the splash screen on different iOS devices and orientations.\
✅ If you have a multi-platform app, consider adding a similar splash screen on **Android** for a consistent user experience.

---

## 🔥 **What’s Next?**

Stay tuned for more deep dives into the development of **She Health** — including UI improvements, state management, and performance optimizations! 😎

---

👉 **"Crafted with Care by Subrata"** – A small but impactful change that makes your app feel more polished and professional. 👏

---

**#ReactNative #iOSDevelopment #TechCraft #SheHealth #MobileAppDevelopment**

