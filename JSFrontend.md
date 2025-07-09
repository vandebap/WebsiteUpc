# Frontend JavaScript 

This frontend JavaScript code manages the interaction between users and the backend of our emotion prediction platform : 
- User input and validation
- Admin login with JWT-based authentication
- Secure model upload for admins only
- Display of predictions and probabilities

---

##  UserInterface

### Features:
- Form to input psychological scores and demographic data
- Displays predicted emotion and class probabilities
- Simple interface switching between pages 

---

## Security 

We implemented several **layers of security and input validation** on the frontend to enhance robustness and prevent common attacks.

###  Admin  Security
- Admin submits username and password.
- Before submission:
  - Username is **limited in length** (`MAX_USERNAME_LENGTH`)
  - Password is also **limited** (`MAX_PASSWORD_LENGTH`)
- If valid, credentials are sent to the backend `/admin_login` endpoint.
- On success, the backend responds with a **JWT token**.
- This token is:
  - Stored in `sessionStorage`
  - Automatically included in the `Authorization` header of protected requests (`Bearer <token>`)

> ⚠️ No sensitive credentials are stored in localStorage. Tokens expire in 2 hours (controlled by the backend).

---

## User Input Validation

### Rules:
- **Name field** is limited in length (`MAX_INPUT_LENGTH`)
- **Age field** is restricted to a reasonable range (e.g., 1 to 130)
- All psychological scores are parsed and validated as integers
- If a field violates the rules:
  - The form is not submitted
  - A red warning is displayed dynamically using the `showWarning()` function


This protects the backend from malformed or oversized input and improves user experience by giving immediate feedback.

---

## Prediction 

1. When the user submits the form:
   - All inputs are validated (name, age, scores...)
   - Data is structured into a JSON object
2. This object is sent to the `/predict` backend endpoint
3. The backend returns:
   - Predicted emotion (e.g., "3" → "Happiness")
   - Probabilities for all emotion classes
4. The UI then renders the result using a dynamically generated HTML block

---

## Secure Admin Model Upload

Only a logged-in admin with a valid **JWT token** can upload a new model.

### How it works:
- The admin selects a `.sav` file
- A check ensures only `.sav` extensions are allowed
- The `Authorization: Bearer <token>` header is attached automatically
- The backend validates and replaces the current model

### Error Handling:
- Failed uploads show a user-friendly error message
- Invalid file types are rejected on the client-side

---

## Page Section 

The `showSection()` function toggles between user view and admin view by showing/hiding `<section>` elements.

---

## showWarning()

This helper function:
- Visually marks a field as invalid (red border)
- Displays a warning message below the field
- Clears old warnings each time the form is submitted

---

##  Dependencies

This frontend uses vanilla JavaScript only — no frameworks required. However, to hash passwords (if needed), we used:

- `CryptoJS.MD5()` (optional, if local hashing was previously used)

---

## Example: JSON sent to the backend

```json
{
  "age": 21,
  "sex": "Male",
  "gad7_anxiety": 5,
  "phq9depression": 3,
  "mdq_mania": 1,
  "isi_insomnia": 6,
  "p16_prodromal_count": 2,
  "p16_prodromal_severity": 3,
  "schizotypy_unusual_experiences": 4,
  "schizotypy_cognitive_disorganisation": 2,
  "schizotypy_introvertive_anhedonia": 1,
  "schizotypy_impulsive_nonconformity": 2,
  "emotion_true": "Happiness"
}
