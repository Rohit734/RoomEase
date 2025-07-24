🛠️ RoomEase Project Plan (MVP Focus)

✅ PHASE 0: Setup & Bootstrapping
🧱 Goal:
Prepare your local and AI-compatible dev environment.
Tasks:
	•	Set up Python venv and install FastAPI + dependencies.
	•	Set up Flutter SDK and scaffold the mobile app.
	•	Create .env.example and add .env to .gitignore.
	•	Set up Postgres (via Docker).
	•	Initialize Alembic with a versions/ folder.

✅ PHASE 1: User Auth & Onboarding (Epic 1)
🧩 Backend
File
Responsibility
routers/auth.py
Login, Signup
schemas/auth.py
LoginRequest, SignupRequest, Token
crud/auth.py
Create/find user, hash/verify password
models/user.py
User ORM
core/security.py
JWT logic
	•	Implement /signup, /login with JWT
	•	Add role selection on signup
	•	Add unit tests for login/signup
🧩 Frontend (Flutter)
	•	screens/login_screen.dart, signup_screen.dart
	•	Add role toggle (Owner/Tenant)
	•	Save token locally after login
	•	Create services/auth_service.dart

✅ PHASE 2: Owner Property Management (Epic 2)
🧩 Backend
Domain
Files
buildings
routers/buildings.py, schemas/buildings.py, crud/building.py, models/building.py
rooms
routers/rooms.py, schemas/rooms.py, crud/room.py, models/room.py
	•	Create Building + Room models and routes
	•	Enforce free tier: 1 building, 2 rooms
	•	Implement image/video upload endpoint
	•	Add AI description via Gemini API
	•	Alembic migration: building, room tables
🧩 Frontend
	•	screens/owner_dashboard.dart, create_building.dart, add_room.dart
	•	Integrate AI-generated room description trigger
	•	Use image_picker for media upload

✅ PHASE 3: Tenant Room Discovery (Epic 3)
Backend
	•	rooms.py: Add search filters
	•	Geo queries for approximate location (Google Maps API)
	•	Create schemas/search_filters.py
Frontend
	•	screens/tenant_home.dart, room_card.dart
	•	Search filter UI
	•	Approximate map display using google_maps_flutter

✅ PHASE 4: Booking Request & Escrow (Epic 4)
Backend
Domain
Files
bookings
routers/bookings.py, schemas/bookings.py, crud/booking.py, models/booking.py
payments
routers/payments.py, schemas/payments.py, crud/payment.py, models/payment.py
	•	Booking request endpoint with 24h expiry
	•	Integrate payment gateway (Razorpay/Stripe)
	•	Implement webhook receiver
	•	Auto-update room status on payment webhook
	•	Conditional contact/location reveal logic
Frontend
	•	screens/booking_request.dart, payment_page.dart
	•	Handle status: Available, Pending, Booked
	•	Trigger chat and location visibility after payment success

✅ PHASE 5: Notifications & Chat (Epic 5)
Backend
	•	notifications.py with FCM webhook
	•	chat.py (optional: WebSocket or polling)
	•	Store messages with templates
Frontend
	•	screens/chat_screen.dart
	•	Predefined templates (e.g., “Hi, when can I visit?”)
	•	In-app and push notification display

✅ PHASE 6: Polishing & Testing
	•	End-to-end test flows for Owner and Tenant
	•	Add meaningful loading/error states
	•	Write README setup instructions
	•	Finalize MVP schema diagram in docs/

🧠 Bonus CLI Script (Optional Later)
Create a CLI scaffold command (bash or Python):
bash scripts/create_module.sh bookings
That generates:
	•	routers/bookings.py
	•	schemas/bookings.py
	•	models/booking.py
	•	crud/booking.py
	•	tests/bookings/test_bookings.py

🗂️ Suggested Location
Place this plan in:
docs/project-plan.md
