# 📋 UI Implementation Document: Hello Push Notification SDK Dashboard

### 📌 User Story

As discussed, we will support **both in-app pop-up notifications** and **FCM notifications** in the Hello Push-Notification SDK. \
The UI team is responsible for implementing the necessary sections in the web dashboard to:

* Integrate Firebase
* Create FCM templates
* Update the launch campaign panel
* Allow project selection at runtime
#### 🔗 API Collection & Links:

* [HTML Template Push-Notification APIs collection](https://www.postman.com/aman12034876/html-template-push-notification-sdk/collection/t72jg36/html-template-push-notification-sdk?action=share&creator=14559709)
* [API Doc](https://docs.google.com/document/d/1W6i2dg06QkkRNS_DesQYDFk7xBhZCsKJ3udF2ADTaWA/edit?pli=1&tab=t.0#heading=h.v15f9xjhboch)
* [ClickUp (Mobile Board): Push-Notification SDK](https://app.clickup.com/t/86cxkbx9b)

---

## 🧩 Module Breakdown

---

### 1. 🔐 Firebase Project Integration Panel

#### ✅ Features:

* Upload Firebase Admin SDK JSON for each project
* Validate & Save credentials

#### 📄 Required Fields:

* `Project Name`
* `Project ID`
* `Firebase Admin JSON` (Upload field)

---

### 2. 📝 Template Management Panel (Updated)

#### ✅ Features:

* Create/manage both:

  * **FCM template along with the existing Template**


#### 📄 Fields for FCM Template (when popup + fcm is selected):

* Notification Title
* Notification Body
* Image URL / Media

---

### 3. 🚀 Launch Campaign Panel (Updated)

#### ✅ Features:

* Select Project at runtime
* Choose Notification Type:

  * In-App
  * FCM
  * Both
* Select Template
* Define Target (User ID / Segment / All)
* Launch Campaign

#### 📄 Required Fields:

* Project ID (dropdown)
* Notification Type (radio button / toggle)
* Template ID (filtered based on project & type)
* Audience (User Tokens / All)

#### 🔗 API Endpoint:

* `POST /send-notification`

---

## 📎 References

* OneSignal FCM Setup: [docs](https://documentation.onesignal.com/docs/android-firebase-credentials)
* React Native SDK Setup: [docs](https://documentation.onesignal.com/docs/react-native-sdk-setup)

