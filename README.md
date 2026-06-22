# 🗳️ PulseVote

PulseVote is a full-stack polling platform that enables users to create polls, cast votes, and visualize results in real time.

Built with Next.js, TypeScript, Prisma, PostgreSQL, Redux Toolkit, and Server-Sent Events (SSE), PulseVote demonstrates modern full-stack application architecture, real-time data synchronization, scalable state management, and interactive data visualization.

🌐 **Live Demo:** https://pulsevoteapp.vercel.app/

---

## Overview

Polling platforms are often limited to collecting responses without providing engaging, real-time feedback to participants.

PulseVote was designed to create a seamless voting experience where users can:

- Create custom polls instantly
- Vote without account registration
- See results update in real time
- Analyze voting trends through interactive charts
- Access polls across desktop and mobile devices

The application combines modern frontend engineering with a robust backend architecture to deliver a responsive and interactive user experience.

---

## Key Features

### 📝 Poll Creation

Users can create polls by providing:

- Poll title
- Optional description
- Multiple voting options
- Creator display name

Polls are immediately persisted to the database and become available to all users.

---

### 🗳️ Guest-Based Voting System

PulseVote removes the friction of mandatory account creation.

Instead of traditional authentication:

- A unique guest ID is generated and stored in localStorage
- Returning visitors retain the same identity
- Users can participate instantly
- Vote tracking prevents duplicate voting

This approach provides a streamlined user experience while maintaining poll integrity.

---

### ⚡ Real-Time Results

PulseVote uses **Server-Sent Events (SSE)** to deliver live vote updates.

When a vote is submitted:

1. Vote data is stored in PostgreSQL
2. Vote counts are updated through Prisma
3. The SSE service detects changes
4. Connected clients receive update events
5. Redux refreshes application state
6. Charts re-render automatically

Results update without requiring page refreshes.

---

### 📊 Interactive Data Visualization

Poll results are displayed through dynamic charts powered by Chart.js.

Features include:

- Live vote counts
- Percentage breakdowns
- Responsive visualizations
- Automatic updates during active voting

---

### 📱 Responsive Design

PulseVote is designed with a mobile-first approach and supports:

- Mobile devices
- Tablets
- Desktop screens

The interface adapts seamlessly across screen sizes.

---

### 🔄 Centralized State Management

Redux Toolkit manages:

- Poll collections
- Poll details
- Live voting results
- Loading states
- Error handling

This creates a predictable and scalable state architecture.

---

## Screenshots

### Home Page

![Homepage](./screenshots/homepage.png)

### Poll Creation

![Poll Creation](./screenshots/create-poll.png)

### Voting Experience

![Voting](./screenshots/vote.png)

### Live Results

![Results](./screenshots/results.png)

---

## Technical Highlights

### Real-Time Architecture with Server-Sent Events

Unlike traditional polling applications that require manual refreshes, PulseVote implements a real-time update system using Server-Sent Events (SSE).

#### Why SSE?

For this use case, communication is primarily one-directional:

```text
Server → Client
```

SSE provides:

- Lower overhead than WebSockets
- Automatic reconnection support
- Simpler implementation
- Native browser support
- Persistent server-to-client updates

---

### Hybrid Rendering with Next.js App Router

PulseVote leverages both Server Components and Client Components.

#### Server Components

Used for:

- Initial data fetching
- SEO-friendly rendering
- Database-backed content delivery

Examples:

```text
app/page.tsx
app/poll/[id]/page.tsx
```

#### Client Components

Used for:

- Voting interactions
- Form submissions
- Real-time subscriptions
- Redux integration

Examples:

```text
PollVoteClient
PollCreateForm
PollResultsChart
```

This architecture balances performance and interactivity.

---

### Database Design

The application uses PostgreSQL with Prisma ORM.

#### Poll

```text
Poll
├── Options
└── Votes
```

#### Key Design Decisions

##### Vote Counter Optimization

Each option stores a vote counter:

```prisma
votes Int @default(0)
```

This avoids expensive aggregate queries whenever charts are rendered.

##### Vote Audit Trail

Votes are stored separately from poll options.

Benefits include:

- Historical vote tracking
- Analytics opportunities
- Future reporting features
- Better data integrity

---

### Guest Identity System

Instead of implementing a full authentication system, PulseVote uses lightweight guest identities.

```text
guest_a1b2c3d4e5
```

Each visitor receives a unique identifier stored locally.

Benefits:

- Zero onboarding friction
- Instant participation
- Persistent user identification
- Simpler architecture

---

## Architecture

```text
                 Next.js App Router
                          │
                          ▼
                Server Components
                          │
                          ▼
                    API Routes
                          │
                          ▼
                     Prisma ORM
                          │
                          ▼
                    PostgreSQL
                          │
                          ▼
               Server-Sent Events
                          │
                          ▼
                Redux Toolkit Store
                          │
                          ▼
                     React UI
```

---

## Technology Stack

| Layer | Technology |
|---------|---------|
| Framework | Next.js 16 |
| Language | TypeScript |
| Styling | Tailwind CSS |
| State Management | Redux Toolkit |
| Database | PostgreSQL |
| ORM | Prisma |
| API Layer | Next.js Route Handlers |
| Real-Time Updates | Server-Sent Events (SSE) |
| HTTP Client | Axios |
| Charts | Chart.js |
| Deployment | Vercel |

---

## Project Structure

```text
app/
├── api/
│   └── polls/
│       ├── route.ts
│       └── [id]/
│           ├── route.ts
│           ├── vote/
│           ├── close/
│           └── stream/
│
├── components/
│   └── polls/
│       ├── PollCreateForm.tsx
│       ├── PollVoteClient.tsx
│       ├── PollCard.tsx
│       └── PollResultsChart.tsx
│
├── lib/
│   ├── prisma.ts
│   ├── axiosInstance.ts
│   ├── api/
│   └── utils/
│       └── guest.ts
│
├── redux/
│   ├── store.ts
│   ├── slices/
│   │   ├── pollsSlice.ts
│   │   └── votesSlice.ts
│   └── Providers.tsx
│
├── page.tsx
├── poll/
│   └── [id]/
│       └── page.tsx
└── layout.tsx
```

---

## Challenges Solved

### Building Real-Time Polling Without WebSockets

Real-time updates were required, but implementing a full WebSocket infrastructure would add unnecessary complexity.

**Solution**

- Server-Sent Events (SSE)
- Streaming responses
- Automatic client synchronization
- Lightweight implementation

---

### Managing Shared Application State

Polls, votes, and live results must remain synchronized across multiple views.

**Solution**

- Redux Toolkit
- Async thunks
- Centralized state management
- Predictable state updates

---

### Preventing Duplicate Voting

Without user accounts, duplicate voting needed to be handled differently.

**Solution**

- Guest identifiers stored in localStorage
- Vote tracking per poll
- Client-side safeguards

---

### Optimizing Vote Retrieval

Repeated aggregation queries can become expensive as poll participation grows.

**Solution**

- Dedicated vote counters
- Optimized Prisma queries
- Reduced database load

---

## AI-Assisted Development

This project was developed using modern AI-assisted engineering workflows.

### Tools Used

- GitHub Copilot
- Claude

### How AI Was Used

- Accelerating component development
- Generating boilerplate code
- Debugging implementation issues
- Improving documentation
- Exploring architectural approaches

All final implementation decisions, architecture design, database modeling, and feature development were completed manually.

---

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL
- npm

---

### Installation

```bash
git clone https://github.com/Kehinde13/pulsevote.git

cd pulsevote

npm install
```

---

### Environment Variables

Create a `.env` file:

```env
DATABASE_URL=your_postgresql_connection_string
```

---

### Run Development Server

```bash
npm run dev
```

Open:

```text
http://localhost:3000
```

---

### Prisma Commands

Generate Prisma Client:

```bash
npx prisma generate
```

Run Migrations:

```bash
npx prisma migrate dev
```

Open Prisma Studio:

```bash
npx prisma studio
```

---

## Future Enhancements

- User authentication
- Poll ownership dashboard
- Anonymous vs authenticated voting
- Poll scheduling and expiration
- Advanced analytics
- Poll templates
- Team collaboration
- Shareable insights dashboard
- Exportable poll reports

---

## Why This Project Matters

PulseVote demonstrates modern full-stack engineering principles through:

- Next.js App Router architecture
- Server and Client Component separation
- Real-time application design
- Relational database modeling
- State management with Redux Toolkit
- Data visualization
- Scalable API development

The project showcases the ability to design and build interactive, data-driven web applications using modern frontend and backend technologies.
