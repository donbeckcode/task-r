# TaskR

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://semver.org)
[![Status](https://img.shields.io/badge/status-MVP-green.svg)](https://github.com/yourusername/task-r)

## 📝 Description

TaskR is an advanced task management application that enables users to efficiently plan, track, and accomplish their personal goals. The MVP version focuses on delivering solid foundations with key CRUD functionality, an intuitive interface, and reliable data synchronization for individual users.

### Key Features

- 🔐 Secure authentication system
- ✅ Task management (Create, Read, Update, Delete)
- 🎯 Priority and status tracking
- 🎨 Color coding for tasks
- ⏰ Deadline management
- 🔍 Advanced filtering and search
- 🌓 Dark/Light mode support

## 🛠 Tech Stack

### Frontend

- [React 19](https://react.dev) - Interactive UI Components
- [TypeScript 5](https://www.typescriptlang.org) - Type Safety
- [Tailwind CSS](https://tailwindcss.com) - Utility-first CSS Framework
- [Shadcn/ui](https://ui.shadcn.com) - Accessible UI Components
- [React Query](https://tanstack.com/query) - Data Fetching and State Management

### Backend

- [Supabase](https://supabase.com) - Backend as a Service
  - PostgreSQL Database
  - Authentication
  - Real-time Subscriptions
  - Row Level Security

### AI Integration (post-mvp)

- [OpenRouter.ai](https://openrouter.ai) - AI Model Access
  - Multiple AI Models Support
  - Cost Control Features

### DevOps

- [GitHub Actions](https://github.com/features/actions) - CI/CD
- [Docker](https://www.docker.com) - Containerization

## 🚀 Getting Started

### Prerequisites

- Node.js (v18 or higher)
- npm or yarn
- Git
- Docker (optional)

### Installation

1. Clone the repository:

```bash
git clone https://github.com/yourusername/task-r.git
cd task-r
```

2. Install dependencies:

```bash
npm install
```

3. Set up environment variables:

```bash
cp .env.example .env
```

Edit `.env` with your configuration:

- `SUPABASE_URL`
- `SUPABASE_ANON_KEY`
- `OPENROUTER_API_KEY`

4. Start the development server:

```bash
npm run dev
```

The application will be available at `http://localhost:3000`

## 📜 Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run test` - Run tests
- `npm run lint` - Run linter
- `npm run format` - Format code

## 🎯 Project Scope

### MVP Features

- User authentication (signup, login, password reset)
- Task management (CRUD operations)
- Task status and priority system
- Color coding for tasks
- Deadline management
- Filtering and search functionality
- Dark/Light mode
- Responsive design

### Future Plans

- Drag and drop functionality
- Notifications system
- Task sharing and collaboration
- PWA support
- Advanced analytics
- Mobile app

## 📊 Project Status

The project is currently in MVP (Minimum Viable Product) stage. We are actively working on:

- Core functionality implementation
- Performance optimization
- Security enhancements
- User experience improvements

### Known Issues

- See [Issues](https://github.com/yourusername/task-r/issues) for current bugs and feature requests

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
