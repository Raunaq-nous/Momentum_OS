# Momentum OS: Project Guide

This guide explains the Momentum OS project in plain English. It is written for a manager or new collaborator and uses only what the project's own documents and file notes say. Where those documents disagree or say nothing, this guide says so, and section 8 collects those gaps.

---

## 1. What this is and who it is for

Momentum OS is a personal productivity app that runs in a web browser. The owner's tagline is "Build Systems, Not Motivation." The idea is that one app holds your tasks, habits, goals, study life, career search, focus time and notes, so that you rely on routines instead of willpower.

The material points to students and early-career people as the main audience. The app tracks courses, assignments, grade averages, internship applications, hackathons (timed team coding competitions), research papers and certifications. It also lets each user pick a "persona" (student, professional or builder) that changes the wording on screen.

The app has a playful, game-like feel. It has levels, experience points, badges, a virtual cat and a hand-drawn "desk" home screen. The README describes it as a "$20/mo Premium SaaS Grade" product. Nothing else in the material says whether it is sold, who pays for it, or how many people use it.

---

## 2. The big picture: the main parts

The project has about nine main parts. For each one, the folders are given as they appear in the project. Everything lives under a folder called `src` unless noted.

### 2.1 The app shell and screen controller
**What it does:** This is the frame around everything. It sets up fonts, page titles, the light or dark theme, and a safety net that shows a friendly message if a screen crashes. A main controller decides what to show: the login screen, the intro scene, the desk rooms, or the "classic" sidebar layout.
**Where it lives:** `src/app` (files `layout.tsx`, `page.tsx`, `providers.tsx`), plus shared pieces in `src/components/common` (the top bar, the left sidebar, the error safety net, notifications, the keyboard search box, keyboard shortcuts).

### 2.2 Sign-in
**What it does:** Handles login, sign-up and sign-out. Users can enter details, choose a persona, or continue as a guest. If there is no cloud login available, the app falls back to a guest account. A loading screen with an animated logo shows while the workspace is prepared.
**Where it lives:** `src/components/auth` for the screens, `src/store/useAuthStore.ts` for the sign-in logic.

### 2.3 The "desk" home experience
**What it does:** This is the newest, most distinctive part. The home screen is drawn as a cozy hand-drawn desk. It shows window plants, corkboard notes, progress gauges, a calendar, a cat in a bed, a to-do pad, a focus clock and a radio. Before that, a short intro scene shows a cat asleep on the keyboard, and the user moves it away to start writing the day's tasks. Each area of life (tasks, habits, notes, goals) opens as a "room" that can be shown as a simple tactile sheet or as a full, more detailed view. Pop-up dialogs on the desk handle adding tasks and habits, plant status, deadlines, the AI chat called "Sparky," mail, search and sign-out.
**Where it lives:** `src/components/dashboard/desk/`.

### 2.4 The classic dashboards
**What it does:** An older, more conventional layout with a left sidebar. Its home screen is "Mission Control," which combines momentum stats, deadlines, interviews, today's tasks, a focus timer and project health. There is also an alternative "premium" dashboard, a circular "Life Score" gauge, and a "solar system" map where each life area appears as an orbiting planet colored by how healthy that area is. A phone-friendly list version of that map exists too.
**Where it lives:** `src/components/dashboard` (outside the `desk` subfolder).

### 2.5 Everyday productivity tools
**What it does:** These are the day-to-day screens.
- **Tasks:** several ways to view tasks (list, board, timeline, calendar, table), filters, search, sorting, subtasks, and actions on many tasks at once. The board lets you drag tasks between status columns. Folder: `src/components/tasks`.
- **Habits:** create, edit, pause, archive and delete habits, with streaks, a year-long activity grid and charts. Folder: `src/components/habits`.
- **Calendar:** a daily timeline from 7am to 9pm, an agenda list of upcoming items, and an end-of-day review where the user rates focus, writes a reflection and moves unfinished tasks to the next day. Folder: `src/components/calendar`.
- **Focus:** a countdown timer with sprint and break modes, a background sound mixer, and a "focus garden" that shows one tree per completed session. Folder: `src/components/focus`.
- **Notes and journal:** notes with titles, tags, markdown text (a simple way of writing formatted text), voice dictation and a link diagram; a journal with dated entries and a mood for each. Folders: `src/components/notes`, `src/components/journal`.
- **Systems (routines):** daily routines with tick-off steps and tips for reducing friction. Folder: `src/components/systems`.

### 2.6 Study and career tracking
**What it does:**
- **Academic:** courses, assignments with live countdowns, progress bars, submission controls, a target grade average (CGPA) set on a 10-point scale, and a study-activity heatmap. Folder: `src/components/academic`.
- **Career:** an internship pipeline board where applications are dragged between stages, a hackathon list, research papers, certifications, and a combined timeline of assignment, hackathon and internship deadlines in date order. Folder: `src/components/career`.

### 2.7 Goals, achievements and project health
**What it does:**
- **Goals:** goals across time spans from daily to life-long, with milestones, rewards, progress charts, and weekly, monthly or yearly reflection notes. Folder: `src/components/goals`.
- **Achievements:** level, experience points, seasonal challenges and unlockable badges, with celebration effects. Folder: `src/components/achievements`. A small habits card shows similar game elements. Folder: `src/components/habits`.
- **Project health:** each project's risk level, speed of work, completion progress and a "burndown" chart (a line showing how much work remains over time). Folder: `src/components/projects`.

### 2.8 Analytics and settings
**What it does:**
- **Analytics:** productivity numbers, charts, switchable time ranges, a yearly activity heatmap, and period summaries that include a burnout-risk indicator. Folder: `src/components/analytics`.
- **Settings:** choose a persona, switch modules on or off, pick theme, sound and notifications, and back up or clear data. Folder: `src/components/settings`.

### 2.9 The AI helpers
**What it does:** Several widgets, forms and background services use an AI service. They are described in section 5.
**Where it lives:** `src/components/ai`, `src/utils/aiAssistantEngine.ts`, and two server endpoints under `src/app/api`.

### 2.10 The data, storage and shared plumbing
**What it does:** This is the invisible layer that holds the app's information and keeps it saved. It also holds small reusable interface pieces (buttons, cards, inputs, progress bars, the round "add" button and the quick-add pop-up) and helper code for scores, grades, sounds and default values.
**Where it lives:** `src/store` (the central data holders), `src/lib` (the local database and the online connection), `src/types` (the definitions of what each kind of data looks like), `src/utils` (helper calculations and sound), `src/components/ui` (reusable interface pieces).

### 2.11 Supporting files outside the app
- `public`: the web app manifest, which controls how the app looks when installed on a phone or desktop home screen.
- `scratch`: helper and test scripts for developers (section 7).
- Root files: settings for the build tools and code checker, plus project documents such as `README.md`, `AGENTS.md`, `PROGRESS_LOG.md`, `FINAL_DELIVERY_REPORT.md` and `AUTONOMOUS_PLAN.json`.

---

## 3. How the pieces work together: one typical journey

Here is a walk through a new user's first session, based on the file notes. Some steps are inferred from what the files say they do, so treat the order as a reasonable picture and not a verified trace.

1. **The user opens the app in a browser.** The root layout sets up fonts, the theme and the crash safety net. The main controller (`page.tsx`) checks whether the user is signed in.

2. **The login gate.** A new user sees the sign-in screen. They can create an account, log in, choose a persona (student, professional or builder), or enter as a guest. If the online login service is not available, the app creates a guest account instead.

3. **A loading screen.** An animated logo appears while the workspace is prepared. The app loads whatever was previously saved on the user's own device.

4. **The intro scene.** The opening screen shows a cat sleeping on a keyboard. The user moves the cat away, and that starts the act of writing today's tasks.

5. **The desk home.** The user lands on the hand-drawn desk. Plants in the window, progress gauges, the to-do pad, the calendar and the focus clock give a quick view of how things are going. The mood and message of the friendly mascot card change with the user's "momentum score" and streak, according to the file notes.

6. **Adding a task.** The user can use the round add button, the quick-add pop-up, the desk's task dialog, or the AI chat. In the quick-add pop-up or a task form, the user can describe the task in plain language and the AI fills in the form fields. The user reviews it and saves.

7. **Saving.** The new task goes into the central data store. The store writes it to the on-device database so it survives closing the browser, including offline. The change is also placed in a waiting line, called the sync queue, so it can be sent to the online database (Supabase) when a connection is available.

8. **Doing the work.** The user starts a focus timer from a task. A background soundscape can play. When a session finishes, a tree is added to the focus garden. Completing a task plays a sound and earns experience points.

9. **Habits and streaks.** The user logs a habit for the day, or skips it. Streak counts update, and the year-long grid shows the day as done. Milestones trigger celebration effects, and badges can unlock.

10. **Study and career upkeep.** The user adds an assignment and sees a countdown. They log an internship application and drag it across the pipeline stages. A deadline-risk card may warn about assignments or hackathons that look likely to be missed.

11. **End-of-day review.** In the calendar, the user rates their focus, writes a short reflection, and moves unfinished tasks to the next day.

12. **Looking back.** The analytics screen turns the activity into charts, a score and, if needed, a burnout warning. The user can export the data as a backup file or a spreadsheet from settings.

---

## 4. Where the data lives and how it gets in

### Where it lives
There are three places data can sit.

1. **In the running app (memory).** A central "store" called `useMomentumStore` holds the live data: tasks, habits, projects, assignments, internships, hackathons, risks, experience points, level, coins, the focus timer, streaks and settings. A second store, `useAuthStore`, holds who is signed in (name, role, email and a session token). The automatic code map shows `useMomentumStore` as the most connected piece of the whole project, so most screens depend on it.

2. **On the user's own device.** A tool called Dexie keeps a copy in the browser's built-in database (`src/lib/db.ts`). This is what makes the app work offline. The same file also manages the waiting line of changes to send online.

3. **Online, in Supabase.** Supabase is a hosted database and login service. The app connects to it from `src/lib/supabase.ts`. It holds user accounts and cloud copies of data. The file notes mention tables for items such as hackathons and internships, but do not give a full list.

### How data gets in
- **Typing into forms.** Pop-up forms exist for tasks, habits, courses, assignments, internships, hackathons, goals and notes.
- **Plain-language descriptions.** Several forms (tasks, goals, courses, internships, hackathons) offer an AI option that fills in the fields from a sentence.
- **Speaking.** A voice button records audio and sends it to a transcription service. The text then lands in a note or form.
- **Default starting values.** `src/utils/initialData.ts` holds the default starting values for data collections and settings. One test checks that a brand-new account shows empty academic statistics, not made-up numbers. Another script scans for leftover fake sample values.
- **Backup files.** The README says users can export and import a JSON backup (a standard text format for data) and export a CSV (a spreadsheet-friendly file).
- **A one-off copy script.** `scratch/migrate_local_data_to_supabase.ts` copies sample hackathon and internship records into the online database.

### What the data looks like
`src/types/index.ts` defines the shapes of the data. The README's diagram describes a user profile with a name, level, experience points and momentum score. Under it sit tasks (with status, priority, time estimate and due date), habits (with category, current and best streaks, and completion history), courses (containing assignments), internships, hackathons, goals (containing milestones), routines and projects (containing tasks). The file notes also mention notes, journal entries, calendar events, certifications, notifications and achievements.

---

## 5. The AI parts

The project uses an AI service from a company called Groq, through its software kit (`groq-sdk`). The file notes do not say which AI model is used.

**What the AI does:**

- **Fills in forms.** In the task, goal, course, internship and hackathon forms, the user describes something in a sentence and the AI proposes the field values. A test script checks that the AI only fills in details the user actually stated and does not invent any.
- **Understands typed commands.** The side-panel assistant accepts natural-language commands or questions. When it drafts tasks, the user confirms them first.
- **Breaks big goals into steps.** A pop-up splits a goal into smaller tasks, and the user picks which ones to add.
- **Plans the day.** A dashboard card suggests a focus block and the top task to work on, with a button to start a timer.
- **Summarizes progress.** An insights card describes task completion, focus hours and upcoming deadline risks in simple terms.
- **Flags deadline risk.** A card lists assignments and hackathons predicted to be at risk. According to the file notes, the "prediction" part lives in the same helper file as other AI-style features, which mentions a local fallback. Whether the prediction uses the AI service or simple rules is not clear.
- **Sparky, the AI chat on the desk.** The notes describe it as an interactive dialog. Its personality and capabilities beyond chat are not described.
- **Voice to text.** A server endpoint takes an audio recording and returns text using Groq's speech service.

**How it is connected:** The app sends requests to a server endpoint under `src/app/api`, which forwards them to Groq and returns structured results. The code map names several AI-related pieces, such as `GroqAIProvider` and `DynamicAIProvider`, but the file notes do not explain how they differ.

**Safety net:** `aiAssistantEngine.ts` includes "a local fallback if the AI service fails," so some features should keep working with simpler built-in rules when the AI is unavailable.

Many test scripts in `scratch` call the AI service with sample sentences. These need a valid key in the project's environment settings.

---

## 6. Glossary

1. **App Router:** The Next.js way of turning folders and files into pages. The main code is in `src/app`.
2. **Backlinks graph:** A small diagram of notes as dots, where connected notes are linked. Clicking a dot selects the note.
3. **Burndown chart:** A line chart showing how much work is left in a project over time.
4. **CGPA:** Cumulative Grade Point Average, a combined grade score across courses. This app uses Indian grade points and a 10-point scale.
5. **Command palette:** A keyboard-driven search box that jumps to sections or finds items by name. The README names the `Cmd+K` shortcut.
6. **Dexie:** A tool for using the browser's built-in database more easily. It stores the offline copy of the data.
7. **Environment settings:** Private configuration values (such as database addresses and AI keys) kept outside the code. Scripts check these are present.
8. **Error boundary:** A safety wrapper that catches a crashing screen and shows a friendly message with a reload button.
9. **Groq:** The company whose AI and speech services the app calls.
10. **Heatmap:** A calendar-style grid where darker squares mean more activity on that day.
11. **IndexedDB:** The browser's built-in database for storing larger amounts of data on the user's device.
12. **Kanban board:** Columns representing stages, with cards you drag between them. Used for tasks and internships.
13. **Linting:** An automatic check that flags likely mistakes or style problems in code. The tool here is oxlint.
14. **Manifest (web app manifest):** A small settings file that controls how the app looks when installed on a home screen.
15. **Module:** One area of the app, such as tasks, habits or career. Settings let users enable or disable them.
16. **Momentum score:** A number summarizing how well the user is doing. The exact formula is in `analyticsHelpers.ts`, but the notes do not spell it out.
17. **Next.js:** The web framework the app is built with.
18. **Persona:** A user type (student, professional or builder) that changes the interface wording.
19. **PWA (Progressive Web App):** A website that can be installed like an app and work offline.
20. **React:** The library used to build the screens out of reusable pieces.
21. **Store (Zustand):** A central holder of the app's live data and the actions that change it. Zustand is the library used.
22. **Streak (and streak freeze):** A streak counts consecutive days a habit is done. A freeze token appears to protect it, though the exact rules are not given.
23. **Supabase:** The online service for accounts and cloud data storage.
24. **Sync queue:** A waiting line of changes made offline, to be sent to the online database later.
25. **TypeScript:** A stricter form of JavaScript that catches certain mistakes before the app runs.

---

## 7. Where to start reading, and what to be careful with

### Where to start
1. **`AGENTS.md`.** It is the most current map of the project and lists the key files and the commands for checking changes.
2. **`README.md`.** It gives the feature list and diagrams, but see the cautions below, because it is partly out of date.
3. **`src/types/index.ts`.** It shows every kind of data in one place, so it helps you learn the vocabulary of the app.
4. **`src/store/useMomentumStore.ts`.** This is the heart of the app. Nearly everything reads from or writes to it.
5. **`src/app/page.tsx`.** It shows how the app decides which screen to display.
6. **`src/components/dashboard/desk/`.** This is the newest user-facing work. `FINAL_DELIVERY_REPORT.md` and `PROGRESS_LOG.md` explain how it was built and checked.
7. **`src/lib/db.ts` and `src/lib/supabase.ts`.** Read these to understand saving and syncing.

### The checks to run after changes
`AGENTS.md` lists four checks: a type check (`npx tsc --noEmit`), a lint check (`npx oxlint`), a scan for leftover hard-coded sample values (`node --experimental-strip-types scratch/audit_hardcoded_literals.ts`), and a build (`npm run build`).

### What to be careful with
- **Two layouts coexist.** The desk experience and the classic sidebar layout both exist, and the main controller switches between them. A change to one may not appear in the other.
- **Duplicate-looking screens.** There are two dashboards, two achievements screens and two academic screens (a "dashboard" and a "tracker"). The notes do not say which is current, so confirm before editing one.
- **The README is partly stale.** It says Next.js 15, while `AGENTS.md` and the dependency list say Next.js 16. It also describes an older design. Prefer `AGENTS.md` when they differ.
- **Scripts that touch the online database.** Several files in `scratch` read or write real Supabase data. One adds sample records and removes them, and another copies sample records in. Check which database the environment settings point to before running them.
- **AI scripts need keys and may incur cost.** Do not commit environment files.
- **Fake data.** The project has an explicit goal of showing no invented numbers for new accounts. Avoid adding hard-coded sample values.
- **Generated files.** `next-env.d.ts` should not be edited by hand. The lock files record exact package versions.
- **Two lock files.** Both `package-lock.json` and `yarn.lock` exist. Pick one package manager and stay with it.
- **Offline and online copies can disagree.** Because data is saved on the device first and synced later, changes to the sync code deserve extra care and testing.
- **The map may be stale.** The automatic code map was built from commit `2a2f2096`. Compare it with the current commit before relying on it.

---

## 8. Open questions

The material did not make these things clear:

1. **Who actually uses the app?** The README suggests a paid product at "$20/mo," but there is no mention of customers, pricing logic, a payment system or a user count.
2. **Which design is the real product?** The README describes a sidebar-based app, while `AGENTS.md` and the delivery report describe a desk-style redesign. It is not stated whether the classic view is meant to stay.
3. **Which Next.js version is correct?** The README says 15 and other files say 16.
4. **Which duplicate screens are current?** See the duplicate-looking screens in section 7.
5. **What is the full list of online database tables?** Only a few are named, and the real structure is not shown. The README's data diagram may not match what is actually stored online.
6. **How do the AI provider pieces differ?** Names such as `GroqAIProvider` and `DynamicAIProvider` appear, with no explanation. The AI model is not named either.
7. **Is deadline-risk prediction done by the AI or by simple rules?**
8. **Where exactly are the two server endpoints?** The notes list two files both called `route.ts` under `src/app/api` without their folder names.
9. **What is the file name of the online connection?** `AGENTS.md` mentions `supabaseClient.ts`, but the file notes list `supabase.ts`.
10. **Is the product deployed?** The README has a "1-Click Vercel Deployment" heading, but its contents are not included. No live address is given.
11. **What are the rules for coins, streak freezes and experience points?** They appear in the store and the header, but how they are earned and spent is not described.
12. **How is the momentum score calculated?** It appears in many places, but its formula is not described in the material.
13. **How do accounts and data separation work?** The material says sign-in exists and a guest fallback exists, but does not explain how one user's data is kept apart from another's online.
14. **What does "offline" really cover?** The README promises offline use, and the on-device database supports it, but the notes do not describe what works with no connection and what does not.
15. **Is there automated testing?** The `scratch` scripts check specific behaviors, but no formal test suite is described.
16. **Is the README complete?** The folder structure section is cut off in the material provided.