# Backend 

This backend is built with **Flask** and is designed to:
- Serve a machine learning model for emotion prediction
- Allow **secure upload** of new models by an administrator only
- Authenticate the admin via **JWT-based login**
- Process and return predictions based on user psychological data

---

# Admin Login

Allows an administrator to log in and receive a **JWT token** that proves identity and gives upload rights.

- Credentials are checked (`username`, `password`)
- If valid, a **JWT** token is generated:
  - It contains the user identity and an expiration time (2 hours)
  - It is signed using **HS256 (HMAC with SHA-256)** and a **SECRET_KEY**
- If invalid, an error is returned.

### HS256
HS256 ensures that the token:
- Cannot be altered by clients (tamper-proof)
- Can be **verified** quickly without storing sessions

---

##  Upload Model 

Only users with a **valid JWT token** can upload models.

- Token is sent in header: `Authorization: Bearer <token>`
- Token is decoded using the same `SECRET_KEY`
- The token must match the admin’s username
- Only `.sav` files are accepted

### Next
- The model is saved in the `uploaded_models` folder
- It is loaded with `joblib.load()` and replaces the current model

---

##  Prediction Endpoint 

Accepts user data and returns:
- The predicted emotion
- Probabilities for each class (if supported by the model)

### DataFrame
We wrap input data in a `pandas.DataFrame` to:
- Preserve feature names
- Ensure model compatibility
- Handle preprocessing pipelines properly

### Response Type 
```json
{
  "predicted_emotion": "3",
  "probabilities": {
    "0": 0.01,
    "1": 0.02,
    ...
  }
}
