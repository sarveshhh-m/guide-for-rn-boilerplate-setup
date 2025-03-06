# 🔐 API & Networking in React Native Boilerplate

Managing API calls efficiently is crucial in a React Native app. Below are different approaches for handling networking, state management for API data, error handling, authentication, and caching.

---

## **🛡️ HTTP Clients for API Requests**  

### **1. Fetch API (Built-in)**
**Best for:** Small projects with minimal API interactions.

#### ✅ **Pros:**  
- No extra dependencies (built into JavaScript).  
- Simple to use for basic API requests.  

#### ❌ **Cons:**  
- No built-in request/response transformation.  
- No automatic retries.  
- Must manually handle timeouts, caching, and errors.  

---

### **2. Axios (Most Popular)**
**Best for:** Apps needing structured API handling with authentication, retries, and error handling.

#### ✅ **Pros:**  
- Supports **interceptors** for global request/response handling.  
- Automatic JSON parsing.  
- Timeout handling.  
- Can easily cancel requests (AbortController).  
- Works well with **JWT authentication** (attaching tokens in headers).  

#### ❌ **Cons:**  
- Slightly larger bundle size.  
- Not built into React Native (requires installation).  

**Installation:**  
```sh
npm install axios
```

**Example Usage:**  
```js
import axios from "axios";

const api = axios.create({
  baseURL: "https://api.example.com",
  timeout: 5000,
  headers: { "Content-Type": "application/json" },
});

export default api;
```

---

### **3. TanStack Query (React Query) – Best for Server State**
**Best for:** Apps relying heavily on API data that need caching & syncing.

#### ✅ **Pros:**  
- **Automatic caching, background fetching, and retries**.  
- Handles **pagination, optimistic updates, and revalidation**.  
- No need for **manual loading/error state management**.  
- Reduces Redux boilerplate for API requests.  

#### ❌ **Cons:**  
- Not for **local state management** (best used with Zustand or Redux).  
- Requires **manual cache invalidation** in some cases.  

**Installation:**  
```sh
npm install @tanstack/react-query
```

**Example Usage:**  
```js
import { useQuery } from "@tanstack/react-query";
import api from "./api";

const fetchUsers = async () => {
  const { data } = await api.get("/users");
  return data;
};

const { data, isLoading, error } = useQuery(["users"], fetchUsers);
```

---

### **4. SWR (Alternative to TanStack Query)**
**Best for:** Fast response with background updates.

#### ✅ **Pros:**  
- Uses **stale-while-revalidate** strategy.  
- Auto re-fetching and caching.  
- Simple API (no complex setup).  

#### ❌ **Cons:**  
- Not as feature-rich as TanStack Query.  
- No built-in support for **pagination or optimistic updates**.  

**Installation:**  
```sh
npm install swr
```

**Example Usage:**  
```js
import useSWR from "swr";
import api from "./api";

const fetcher = (url) => api.get(url).then(res => res.data);
const { data, error } = useSWR("/users", fetcher);
```

---

## **🛡️ Authentication & Token Handling**

### **Handling JWT Tokens**

**Attach tokens to requests using Axios interceptors:**  
```js
api.interceptors.request.use((config) => {
  const token = "your-auth-token"; // Retrieve from SecureStore or AsyncStorage
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

- **Refresh token logic** (automatic re-authentication when token expires).  

---

## **📊 Comparison Table**

| Library  | Best For | Caching | Auto Retry | Pagination | Easy to Use |
|----------|---------|---------|------------|------------|-------------|
| **Fetch API** | Simple requests | ❌ No | ❌ No | ❌ No | ✅ Yes |
| **Axios** | Advanced API handling | ❌ No | 🔹 Manual | ❌ No | ✅ Yes |
| **TanStack Query** | Server state management | ✅ Yes | ✅ Yes | ✅ Yes | 🔹 Moderate |
| **SWR** | Fast revalidation | ✅ Yes | ✅ Yes | ❌ No | ✅ Yes |

---

## **🛠 What Should You Include in Your Boilerplate?**

✅ **Axios** for flexible API calls with interceptors.  
✅ **TanStack Query** if your app relies heavily on API data.  
✅ **JWT authentication & refresh token handling**.  
✅ **Error handling, retries, and caching strategies**.  

