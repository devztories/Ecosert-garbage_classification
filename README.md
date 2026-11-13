# EcoSort – AI-Powered Waste Classification System

EcoSort identifies **12 types of waste** from a photo (upload or live camera) using your trained
**VGG16 Keras model**, then shows the category, confidence, top-3 predictions and
recycling / disposal guidance.

* Python 3.10+ · Django 5.2 · SQLite · TensorFlow/Keras
* Black / dark-green / neon-green responsive UI (mobile, tablet, desktop)
* User accounts, private prediction history, and a separate custom Admin Dashboard

## Quick start (Windows PowerShell)

```powershell
cd ecosort
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Then open <http://127.0.0.1:8000/>. **Before predicting, place your two model files in `ml_models/`**
(see `SETUP_GUIDE.md`).

## Pages

| Area | URL |
|---|---|
| Home, About, Features, Contact | `/`, `/about/`, `/features/`, `/contact/` |
| Waste categories (12 detail pages) | `/categories/`, `/categories/<slug>/` |
| Login / Register / Logout / Change password | `/login/`, `/register/`, `/logout/` (POST), `/password-change/` |
| Image upload / Live camera | `/upload/`, `/camera/` |
| User dashboard, history, result, profile | `/dashboard/`, `/history/`, `/history/<id>/`, `/profile/` |
| Admin dashboard, users, predictions | `/manage/`, `/manage/users/`, `/manage/predictions/` |
| Django back-office (superusers) | `/django-admin/` |

## Project layout

```
ecosort/
├── manage.py  requirements.txt  README.md  SETUP_GUIDE.md  .gitignore
├── config/                 # settings, urls, wsgi/asgi
├── core/                   # the EcoSort app
│   ├── ml.py               # model loading (once) + preprocessing + prediction
│   ├── models.py           # Profile, Prediction (+image), ClassificationResult, ContactMessage
│   ├── views.py forms.py urls.py waste_data.py tests.py
│   ├── templates/          # base, layouts, pages, admin pages, auth
│   ├── templatetags/       # Lucide-style icon tag
│   └── static/core/        # css, js, logo, 12 category illustrations
├── ml_models/              # <-- put ecosort_vgg16_final.keras and class_mapping.json here
├── media/                  # uploaded prediction images (created automatically)
└── templates/              # 403.html, 404.html
```

## Security notes

* Passwords are always hashed (PBKDF2). Normal users may use **4+ character** passwords (4-digit PINs
  or simple words). Staff/superusers keep Django's strict validators (8+ chars, not common, not numeric-only),
  including on `createsuperuser` and the in-app password-change page.
* `/manage/*` is restricted to staff/superusers; normal users get a 403 page.
* Users can only open or delete their own predictions.
* Before deploying, set `ECOSORT_SECRET_KEY`, `ECOSORT_DEBUG=0` and `ECOSORT_ALLOWED_HOSTS`.

## Tests

```powershell
python manage.py test core
```
