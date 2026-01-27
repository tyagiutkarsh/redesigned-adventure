Below is a **UI-oriented JSON structure** for a **day-by-day travel itinerary**.
It is designed to be directly consumable by a frontend to render a timeline-style daily plan.

* Timeline is flat and ordered → easy vertical rendering
* `type` and `category` drive icons, colors, and filters
* Summary supports collapsed view
* Stay and logistics stay predictable in layout
* Optional fields allow progressive enhancement

Assumptions made:

* The itinerary is already generated from user inputs.
* Each day is independent and renderable as a vertical timeline.
* Actions are ordered and time-bound but optional.
* UI needs summary + drill-down.

---

## Top-level itinerary object

```json
{
  "itinerary_id": "uuid",
  "trip_meta": {
    "destination": "string",
    "start_date": "YYYY-MM-DD",
    "end_date": "YYYY-MM-DD",
    "total_days": 7,
    "people_count": 2,
    "travel_style": ["relaxed", "sightseeing"],
    "budget_range": "mid",
    "preferences": {
      "activities": ["nature", "local food"],
      "pace": "moderate",
      "mobility_constraints": null
    }
  },
  "days": []
}
```

---

## Day-level structure (core UI unit)

Each day is meant to render as a **card or section**.

```json
{
  "day_number": 1,
  "date": "YYYY-MM-DD",
  "city": "string",
  "summary": {
    "headline": "Arrival and local exploration",
    "mood": "easy",
    "primary_theme": "arrival"
  },
  "timeline": [],
  "stay": {},
  "logistics": {},
  "notes": "string"
}
```

---

## Timeline items (ordered actions)

This is the **most important UI-driving structure**.

```json
{
  "id": "uuid",
  "type": "visit",
  "title": "Visit Colosseum",
  "description": "Guided tour of the Colosseum and Roman Forum",
  "start_time": "10:00",
  "end_time": "12:30",
  "location": {
    "name": "Colosseum",
    "lat": 41.8902,
    "lng": 12.4922
  },
  "category": "sightseeing",
  "booking": {
    "required": true,
    "status": "booked",
    "reference_id": "ABC123"
  },
  "media": {
    "icon": "sightseeing",
    "image_url": null
  },
  "ui_flags": {
    "highlight": true,
    "skippable": false
  }
}
```

### Allowed `type` values

Used for icons, colors, and grouping in UI.

```json
"arrival" | "transfer" | "visit" | "activity" | "meal" | "free_time" | "check_in" | "check_out"
```

---

## Stay / accommodation block

Rendered once per day (usually night).

```json
{
  "name": "Hotel Roma Central",
  "type": "hotel",
  "check_in": "15:00",
  "check_out": "11:00",
  "address": "Via Example, Rome",
  "geo": {
    "lat": 41.9028,
    "lng": 12.4964
  },
  "booking": {
    "status": "booked",
    "provider": "Booking.com",
    "reference_id": "HOTEL456"
  }
}
```

---

## Daily logistics (collapsed UI section)

```json
{
  "transport_used": ["walk", "metro"],
  "total_travel_time_minutes": 90,
  "estimated_cost": {
    "currency": "EUR",
    "amount": 75
  }
}
```

---

## Full example (single day)

```json
{
  "day_number": 2,
  "date": "2026-04-12",
  "city": "Rome",
  "summary": {
    "headline": "Ancient Rome highlights",
    "mood": "busy",
    "primary_theme": "history"
  },
  "timeline": [
    {
      "id": "1",
      "type": "meal",
      "title": "Breakfast near hotel",
      "start_time": "08:30",
      "end_time": "09:15",
      "category": "food"
    },
    {
      "id": "2",
      "type": "visit",
      "title": "Colosseum & Roman Forum",
      "start_time": "10:00",
      "end_time": "13:00",
      "category": "sightseeing"
    },
    {
      "id": "3",
      "type": "free_time",
      "title": "Explore Trastevere",
      "start_time": "16:00",
      "end_time": "18:00",
      "category": "leisure"
    }
  ],
  "stay": {
    "name": "Hotel Roma Central",
    "type": "hotel"
  },
  "logistics": {
    "transport_used": ["walk", "metro"]
  },
  "notes": "Wear comfortable shoes."
}
```