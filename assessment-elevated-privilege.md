# Assessment — Elevated Privilege: Multi-Stage HR Portal Compromise

---

## Question 1 — MCQ

**Which vulnerability exposed the QA credentials `test_user_88 / testpass123`?**

- A) SQL Injection
- B) Broken Access Control
- C) **Hardcoded Secrets in a client-side JavaScript file** ✅
- D) Cross-Site Scripting (XSS)

> **Answer:** C — Inspecting `static/js/main.js` reveals the credentials in comments. Client-side JS is always readable by any user.

---

## Question 2 — MCQ

**What modification to the JSON password reset payload bypasses OTP verification?**

- A) Changing `"username"` to `"root"`
- B) Changing `"otp"` to `"WRONG"`
- C) Removing the `"new_password"` field
- D) **Changing `"status": "pending"` to `"status": "verified"`** ✅

> **Answer:** D — The endpoint trusts the client-supplied `status` field. Setting it to `"verified"` bypasses OTP checking regardless of the submitted OTP value.

---

## Question 3 — MCQ

**Which parameter is changed to exploit the IDOR on the `/api/profile` endpoint?**

- A) The `headers` object
- B) **`body: JSON.stringify({ uid: 1 })` — changing `uid` from `2` (user) to `1` (admin)** ✅
- C) The HTTP method
- D) The endpoint path

> **Answer:** B — The `uid` parameter in the POST body is not validated against the authenticated user's ID, exposing any user's profile data by simply changing the value.

---

## Question 4 — Fill in the Blank

**What is the username of the admin account that is targeted for a password reset via the OTP bypass?**

**Answer:** `admin_hr_master`

> The OTP bypass is performed on the `admin_hr_master` account by sending `{"username": "admin_hr_master", "otp": "WRONG", "status": "verified", "new_password": "hacked123"}` to `/api/reset_password`. The client-controlled `status` field bypasses OTP verification.

---

## Question 5 — Fill in the Blank

**What is the filename of the JavaScript file that exposes the hardcoded QA credentials when inspected in browser DevTools?**

**Answer:** `main.js`

> The file at `static/js/main.js` contains the QA credentials `test_user_88 / testpass123` in comments at the top of the file, accessible to any user via the browser Sources tab or by visiting `/static/js/main.js` directly.

---

*Lab target:* `http://localhost:5006`
