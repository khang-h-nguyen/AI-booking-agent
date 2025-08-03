# 💬 MedMe Virtual Assistant (Voice & Chat)

This is a voice-ready medical assistant built with **Retell AI**, designed for **MedMe Pharmacies**. It handles bookings, rescheduling, and cancellations over chat but can be quickly re-adapted to handle phone calls. The assistant integrates with **Cal** for scheduling and **Make** for backend workflows like appointment updates.

---

## 1. Architecture & Key Components

- **Agent Type**: Single Prompt Voice Agent via Retell AI
- **Channels**: Chat Widget (primary), can be adapted to Phone
- **Intent Routing**: Booking / Rescheduling / Cancellation
- **External APIs**:
  - `check_availability_cal`
  - `book_appointment_cal`
  - Cal's Reschedule and Cancel API
- **Automation via Make**:
  - Trigger flows for rescheduling, cancellations, and logging
- **Fallbacks**:
  - Uses `Cal` dashboard or Google Calendar as data source for non-call sessions

---

## 2. Tools & Why They Were Chosen

| Tool          | Use Case                         | Reason |
|---------------|----------------------------------|--------|
| **Retell AI** | Voice agent + chat widget        | Recommended, real-time audio/chat integration |
| **Cal**       | Booking calendar                 | Native Retell function support(check_availability_cal, book_appointment_cal) |
| **Make.com**  | Automation (reschedule, cancel flows)| Flexible, event-based automation |
| **Google Calendar/Cal Bookings** | Data review for chat sessions | Retell doesn't capture `{{phone}}` in chat, only in audio |

---

## 3. Error Handling Approach

| Scenario | Response |
|----------|----------|
| Time unavailable | Suggest next 3 available slots using `check_availability_cal` |
| Missing phone (in chat) | Prompt user: “What’s the best number to reach you at?” → store as `phone` |
| User wants to reschedule/cancel a booking that is not theirs | Prompt user: "Are you sure you didn't book under a different number?" |

---

## 4. "If I Had More Time..."

- Handle cases where a time slot can have more than 1 booking.
- Tweak the prompt or the built-in Cal-integrated functions because sometimes it would need another response from the user before calling the functions.
- Handle cases where the user needs to be redirected to a real person via different phone number/email if the AI cannot solve what the user wants/system is down.
- Build a better pipeline to import data straight into google sheets after the chat ends.

## 5. To Test
- Clone this repository
   ```bash
   git clone https://github.com/khang-h-nguyen/AI-booking-agent.git
   cd AI-booking-agent
   
- Start a local server
  If you have Python installed:
  ```bash
  python3 -m http.server 8080

- To chat with your local chat widget, go to http://localhost:8080
  
