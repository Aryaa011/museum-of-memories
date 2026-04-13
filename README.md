# 🌿 The Archive That Grew — Museum of Memories
 
> **"First-person. The museum is alive."**

A fully walkable 3D museum built in a single HTML file. Walk through personal artifacts, leave your own memory on the wall, and discover a constellation room where every visitor becomes a star.

---

## ✦ Live Demo

https://museumofmemories.netlify.app

---

## 🏛️ What's Inside

```
Entrance Hall → Wing A → Wing B → ✦ Constellation Room
```

### Entrance Hall
Dark stone walls, warm lighting, and a plaque on the right wall. Walk forward through the open arch.

### Wing A — The Permanent Collection
Six personal artifacts hanging on lit gallery walls:
- **Reflections** — The Neighbourhood album cover
- **Storm on the Sea of Galilee** — Rembrandt, 1633
- **Kyoto** — Kiyomizu-Dera Temple at sunset
- **The Starry Night** — Van Gogh, 1889
- **Mona Lisa** — Da Vinci, 1503–1519
- **Sunflowers** — Van Gogh, 1888

Three 3D sculptures in the room: classical bust, moonlight orb, and a marble Venus torso.  
Click any frame → full story popup with personal context.

### Wing B — The Growing Wing
Ten empty frames waiting to be filled by visitors. Every person who walks in can leave one artifact — a photo or a piece of writing. It stays on the wall forever for every future visitor to see.

Three sculptures: abstract bronze column, Interstellar star map on easel, kinetic gold ring sculpture.

### ✦ Constellation Room
Every visitor contribution becomes a star. Hover any star to see what they preserved. Stars are connected by thin constellation lines. The music shifts to something cinematic and vast.

---

## 🎮 Controls

### Desktop
| Input | Action |
|-------|--------|
| `W A S D` / Arrow keys | Walk |
| Hold left mouse + drag | Look around (360°) |
| Click canvas | Lock mouse pointer (smoother look) |
| Click frame | Open artifact story |
| `ESC` | Release mouse / close popup |

### Mobile
| Input | Action |
|-------|--------|
| Left joystick (bottom-left) | Walk |
| Drag right side of screen | Look around |
| `[ Interact ]` button | Open frames / popups |
| Tap `✕` | Close popup |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Three.js r128 (CDN) | 3D scene, rooms, lighting |
| Supabase JS v2 (CDN) | Database + live contributions |
| Web Audio API | Room jazz, footsteps, star tones |
| HTML / CSS / Vanilla JS | UI, controls, mobile joystick |

**No build tools. No npm. Single HTML file.**

---

## ⚙️ Setup

### 1. Supabase — Run this SQL
Go to your Supabase project → SQL Editor → paste and run:

sql
-- Contributions table
CREATE TABLE contributions (
  id          UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  device_id   TEXT NOT NULL UNIQUE,
  type        TEXT NOT NULL CHECK (type IN ('photo', 'text')),
  content     TEXT NOT NULL,
  caption     TEXT,
  bg_color    TEXT DEFAULT '#1a1a2e',
  frame_slot  INT,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);

-- Room visits tracker
CREATE TABLE visited_rooms (
  id          UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  device_id   TEXT NOT NULL,
  room        TEXT NOT NULL,
  visited_at  TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(device_id, room)
);

-- RLS Policies
ALTER TABLE contributions ENABLE ROW LEVEL SECURITY;
ALTER TABLE visited_rooms ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Public read"        ON contributions FOR SELECT USING (true);
CREATE POLICY "Anyone can insert"  ON contributions FOR INSERT WITH CHECK (true);
CREATE POLICY "Anyone can delete"  ON contributions FOR DELETE USING (true);
CREATE POLICY "Public read rooms"  ON visited_rooms FOR SELECT USING (true);
CREATE POLICY "Anyone insert rooms" ON visited_rooms FOR INSERT WITH CHECK (true);
CREATE POLICY "Anyone delete rooms" ON visited_rooms FOR DELETE USING (true);


## 🎵 Audio

All audio is synthesised with Web Audio API — no external files.

| Room | Sound |
|------|-------|
| Entrance | Soft low hum |
| Wing A | Jazz chord progressions (Cmaj7 → Am7 → Fmaj7 → G7) with reverb and melody |
| Wing B | Cooler Bb jazz voicings with walking bass line |
| Constellation | Interstellar-style organ fifths with LFO vibrato and heartbeat pulse |
| All rooms | Footstep noise burst on each step |
| Stars | Each contribution star plays a unique musical note on hover |

Audio starts on first click (browser requirement).

---

## 📁 File Structure


archive-that-grew/
├── museum-day2.html    ← entire project (single file, 372kb)
└── README.md
```
