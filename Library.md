## Libraries Roles

| Library | Purpose |
|--------|---------|
| **Flask** | Micro web framework used to create the API. It handles HTTP routes, requests, and responses. |
| **Flask-CORS** (`CORS`) | Allows your frontend (which might be served on a different origin/port) to communicate with the backend by enabling **Cross-Origin Resource Sharing**. |
| **joblib** | Loads and saves machine learning models in `.sav` format. It is optimized for large numpy arrays (used in scikit-learn). |
| **pandas** | Structures input data into a `DataFrame`, ensuring it's correctly formatted and compatible with the ML model. |
| **os** | Interacts with the operating system. Used here to handle paths and create folders if they don’t exist (`uploaded_models`). |
| **jwt (PyJWT)** | Handles creation and decoding of **JSON Web Tokens (JWT)**. Used to secure the admin login and authorize model uploads. |
| **datetime** | Sets expiration times for JWTs (e.g., 2 hours validity). Provides current UTC time and duration calculations. |

---


These libraries together form a lightweight but secure machine learning backend:
- `Flask` for API structure
- `joblib` to handle the model
- `pandas` to format data for prediction
- `jwt` and `datetime` for admin authentication
- `os` and `CORS` for file and web communication management

Each library is crucial to keep the project modular, readable, and safe.
