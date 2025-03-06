# 📦 Storage & Offline Support in React Native Boilerplate

Managing storage and offline support efficiently is crucial in a React Native app. Below are different options for local storage, secure storage, offline caching, and file handling.

---

## **🔑 Local Storage Options**

### **1️⃣ AsyncStorage (Built-in)**
✅ Pros:  
✔ Simple key-value storage.  
✔ Works out of the box in React Native.  

❌ Cons:  
✖ Not encrypted (not suitable for sensitive data).  
✖ Slower for large data.  

📌 **Installation:**  
```sh
npm install @react-native-async-storage/async-storage
```

📌 **Example Usage:**  
```js
import AsyncStorage from "@react-native-async-storage/async-storage";

const storeData = async (key, value) => {
  await AsyncStorage.setItem(key, JSON.stringify(value));
};

const getData = async (key) => {
  const value = await AsyncStorage.getItem(key);
  return value ? JSON.parse(value) : null;
};
```

**Best for:** Storing lightweight app settings, user preferences.

---

### **2️⃣ MMKV (Fast Alternative)**
✅ Pros:  
✔ **Much faster** than AsyncStorage.  
✔ Supports encryption.  
✔ Works with Redux/Zustand persistence.  

❌ Cons:  
✖ No built-in eviction policy (may need manual cleanup).  

📌 **Installation:**  
```sh
npm install react-native-mmkv
```

📌 **Example Usage:**  
```js
import { MMKV } from "react-native-mmkv";
const storage = new MMKV();

storage.set("user", JSON.stringify({ name: "John" }));
const user = JSON.parse(storage.getString("user"));
```

**Best for:** High-performance local storage, persisting Redux/Zustand state.

---

### **3️⃣ SQLite (For Large Structured Data)**
✅ Pros:  
✔ Handles large, structured datasets.  
✔ Works well for complex queries.  

❌ Cons:  
✖ Slower than MMKV for key-value storage.  
✖ Requires SQL knowledge.  

📌 **Installation:**  
```sh
npm install react-native-sqlite-storage
```

📌 **Example Usage:**  
```js
import SQLite from "react-native-sqlite-storage";
const db = SQLite.openDatabase({ name: "app.db", location: "default" });

db.transaction((tx) => {
  tx.executeSql("CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, name TEXT);");
  tx.executeSql("INSERT INTO users (name) VALUES (?);", ["John"]);
});
```

**Best for:** Apps needing relational database functionality.

---

## **🔐 Secure Storage Options**

### **1️⃣ react-native-keychain (For Secure Credentials)**
✅ Pros:  
✔ Encrypts sensitive data.  
✔ Supports biometrics & Face ID.  

📌 **Installation:**  
```sh
npm install react-native-keychain
```

📌 **Example Usage:**  
```js
import * as Keychain from "react-native-keychain";

await Keychain.setGenericPassword("username", "password");
const credentials = await Keychain.getGenericPassword();
```

**Best for:** Storing authentication credentials securely.

---

## **📡 Offline Caching (API & Data Persistence)**

### **1️⃣ TanStack Query with Persistence**
✅ Pros:  
✔ Caches API data for offline use.  
✔ Auto re-fetches when online.  

📌 **Installation:**  
```sh
npm install @tanstack/react-query @react-native-async-storage/async-storage
```

📌 **Example Usage:**  
```js
import { persistQueryClient } from "@tanstack/react-query-persist-client";
import AsyncStorage from "@react-native-async-storage/async-storage";

persistQueryClient({
  queryClient,
  persister: createAsyncStoragePersister({ storage: AsyncStorage }),
});
```

---

### **2️⃣ WatermelonDB (Sync Large Data Efficiently)**
✅ Pros:  
✔ Syncs with backend & supports offline mode.  
✔ Great for **large datasets** in apps.  

📌 **Installation:**  
```sh
npm install @nozbe/watermelondb
```

**Best for:** Apps needing offline data sync with databases.

---

## **📂 File Storage Options**

### **1️⃣ react-native-fs**
✅ Pros:  
✔ Read/write files locally.  
✔ Supports downloads/uploads.  

📌 **Installation:**  
```sh
npm install react-native-fs
```

📌 **Example Usage:**  
```js
import RNFS from "react-native-fs";

const path = RNFS.DocumentDirectoryPath + "/test.txt";
RNFS.writeFile(path, "Hello World!", "utf8");
```

**Best for:** Managing media files offline.

---

## **📊 Comparison Table**

| Library | Use Case | Secure | Offline Support | Performance |
|---------|---------|--------|----------------|-------------|
| **AsyncStorage** | Simple key-value storage | ❌ No | ✅ Yes | 🔵 Medium |
| **MMKV** | Fast key-value storage | ✅ Yes | ✅ Yes | 🟢 High |
| **SQLite** | Large structured data | ❌ No | ✅ Yes | 🔵 Medium |
| **Keychain** | Auth credentials | ✅ Yes | ❌ No | 🟢 High |
| **TanStack Query** | API caching | ❌ No | ✅ Yes | 🟢 High |
| **WatermelonDB** | Large offline data sync | ✅ Yes | ✅ Yes | 🟢 High |
| **react-native-fs** | File storage | ❌ No | ✅ Yes | 🔵 Medium |

---

## **🛠 What Should You Include in Your Boilerplate?**

✅ **MMKV** for high-performance key-value storage.  
✅ **Keychain** for authentication tokens.  
✅ **TanStack Query** for API response caching.  
✅ **SQLite or WatermelonDB** if handling large offline datasets.  
