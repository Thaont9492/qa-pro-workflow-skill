# Test Cases: Create New User

## Test Case Priority Convention
- **P1** — Block release if fail (happy path + critical validation + permission)
- **P2** — Business logic, edge cases
- **P3** — Regression, cosmetic, future sprint

## Test Case Types
`Functional` | `Negative` | `Edge Case` | `Permission` | `Integration`

---

## P1 Test Cases

### TC001 - Happy Path Registration (Functional)
**Preconditions:** User is on registration page  
**Steps:**
1. Enter valid full name: "John Doe"
2. Enter valid email: "john.doe@example.com"
3. Enter valid password: "Password123!"
4. Enter matching confirm password: "Password123!"
5. Click "Create Account"  
**Expected Result:** Account created successfully, verification email sent, redirected to verification page  
**Type:** Functional  
**Status:** TODO

### TC002 - Email Validation (Functional)
**Preconditions:** User is on registration page  
**Steps:**
1. Enter valid full name
2. Enter invalid email: "invalid-email"
3. Enter valid password and confirm
4. Click "Create Account"  
**Expected Result:** Email validation error displayed, form not submitted  
**Type:** Functional  
**Status:** TODO

### TC003 - Password Strength (Functional)
**Preconditions:** User is on registration page  
**Steps:**
1. Enter valid full name and email
2. Enter weak password: "123"
3. Enter matching confirm password
4. Click "Create Account"  
**Expected Result:** Password strength error displayed, form not submitted  
**Type:** Functional  
**Status:** TODO

### TC004 - Password Confirmation Match (Functional)
**Preconditions:** User is on registration page  
**Steps:**
1. Enter valid full name and email
2. Enter valid password: "Password123!"
3. Enter non-matching confirm password: "Different123!"
4. Click "Create Account"  
**Expected Result:** Password confirmation error displayed, form not submitted  
**Type:** Functional  
**Status:** TODO

---

## P2 Test Cases

### TC005 - Duplicate Email (Negative)
**Preconditions:** Existing user with email "existing@example.com"  
**Steps:**
1. Attempt to register with existing email
2. Enter valid name, password, confirm password
3. Click "Create Account"  
**Expected Result:** Duplicate email error displayed, account not created  
**Type:** Negative  
**Status:** TODO

### TC006 - Empty Required Fields (Edge Case)
**Preconditions:** User is on registration page  
**Steps:**
1. Leave all fields empty
2. Click "Create Account"  
**Expected Result:** All required field errors displayed  
**Type:** Edge Case  
**Status:** TODO

### TC007 - Maximum Field Length (Edge Case)
**Preconditions:** User is on registration page  
**Steps:**
1. Enter name with 100+ characters
2. Enter valid email and password
3. Click "Create Account"  
**Expected Result:** Name length validation or successful creation if allowed  
**Type:** Edge Case  
**Status:** TODO

### TC008 - Special Characters in Name (Edge Case)
**Preconditions:** User is on registration page  
**Steps:**
1. Enter name with special characters: "José María O'Connor"
2. Enter valid email and password
3. Click "Create Account"  
**Expected Result:** Account created successfully or appropriate validation  
**Type:** Edge Case  
**Status:** TODO

### TC009 - Email Verification Link (Integration)
**Preconditions:** New account created  
**Steps:**
1. Check email inbox
2. Click verification link in email  
**Expected Result:** Account verified, user can login  
**Type:** Integration  
**Status:** TODO

---

## P3 Test Cases

### TC010 - Form Accessibility (Regression)
**Preconditions:** User is on registration page  
**Steps:**
1. Use keyboard navigation (Tab) through all fields
2. Use screen reader to check labels  
**Expected Result:** All elements accessible via keyboard and screen readers  
**Type:** Regression  
**Status:** TODO

### TC011 - Mobile Responsiveness (Regression)
**Preconditions:** User on mobile device  
**Steps:**
1. Access registration page on mobile
2. Fill and submit form  
**Expected Result:** Form displays and functions correctly on mobile  
**Type:** Regression  
**Status:** TODO

### TC012 - Browser Compatibility (Regression)
**Preconditions:** User on different browsers (Chrome, Firefox, Safari, Edge)  
**Steps:**
1. Complete registration on each browser  
**Expected Result:** Consistent behavior across browsers  
**Type:** Regression  
**Status:** TODO

### TC013 - Loading States (Cosmetic)
**Preconditions:** User is on registration page  
**Steps:**
1. Submit form
2. Observe submit button during processing  
**Expected Result:** Button shows loading state, disabled during submission  
**Type:** Regression  
**Status:** TODO

### TC014 - Error Message Styling (Cosmetic)
**Preconditions:** Invalid form submission  
**Steps:**
1. Submit invalid form
2. Observe error message appearance  
**Expected Result:** Error messages styled consistently and clearly visible  
**Type:** Regression  
**Status:** TODO