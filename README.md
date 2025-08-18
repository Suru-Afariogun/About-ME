# Suru's Portfolio Website

A modern, interactive portfolio website built with the PERN stack (PostgreSQL, Express.js, React, Node.js) showcasing coding projects, artwork, games, and personal story.

## 🎯 Features

- **Landing Page**: Welcome message, personal summary, latest projects preview, contact info, and profile picture
- **Projects Page**: Complete showcase of coding projects with live links
- **Artwork Page**: Gallery of artistic works
- **Games Page**: Interactive games or links to playable games
- **Story Page**: Animated personal essay with contact information

## 🛠️ Tech Stack

- **Frontend**: React.js with Tailwind CSS
- **Backend**: Node.js with Express.js
- **Database**: PostgreSQL (hosted on Render)
- **Deployment**: Render (both backend and database)
- **Additional**: Tailwind animations, responsive design

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v16 or higher) - [Download here](https://nodejs.org/)
- **Git** - [Download here](https://git-scm.com/)
- **Code Editor** (VS Code recommended)
- **Render Account** - [Sign up here](https://render.com/) (for database and deployment)

## 🚀 Setup Instructions

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd Portfolio_website/Suru
```

### 2. Database Setup (Using Render)

#### Create PostgreSQL Database on Render

1. **Sign up/Login to Render**
   - Go to [render.com](https://render.com)
   - Create an account or sign in

2. **Create a New PostgreSQL Database**
   - Click "New +" button
   - Select "PostgreSQL"
   - Choose a name for your database (e.g., `portfolio-db`)
   - Select the free tier or your preferred plan
   - Click "Create Database"

3. **Get Database Connection Details**
   - Once created, go to your database dashboard
   - Copy the following connection details:
     - **External Database URL** (for connecting from your local development)
     - **Internal Database URL** (for connecting from Render services)
     - **Host**, **Port**, **Database**, **Username**, **Password**

4. **Save Connection Details**
   - Keep these details secure - you'll need them for your environment variables

### 3. Backend Setup

#### Navigate to backend directory
```bash
mkdir backend
cd backend
```

#### Initialize Node.js project
```bash
npm init -y
```

#### Install backend dependencies
```bash
npm install express cors dotenv bcryptjs jsonwebtoken
npm install pg sequelize
npm install --save-dev nodemon concurrently
```

#### Create environment file
```bash
touch .env
```

Add the following to your `.env` file (using your Render database details):
```env
PORT=5000
# Use the External Database URL from Render for local development
DATABASE_URL=postgresql://username:password@host:port/database
# Or use individual components:
DB_HOST=your_render_db_host
DB_PORT=5432
DB_NAME=your_render_db_name
DB_USER=your_render_db_user
DB_PASSWORD=your_render_db_password
JWT_SECRET=your_jwt_secret_here
NODE_ENV=development
```

### 4. Frontend Setup

#### Navigate back to root and create frontend
```bash
cd ..
npx create-react-app frontend
cd frontend
```

#### Install Tailwind CSS
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

#### Configure Tailwind CSS
Update `tailwind.config.js`:
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.5s ease-out',
        'bounce-in': 'bounceIn 0.6s ease-out',
      }
    },
  },
  plugins: [],
}
```

Add Tailwind directives to `src/index.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

#### Install additional frontend dependencies
```bash
npm install axios react-router-dom
npm install framer-motion react-icons
npm install react-intersection-observer
```

### 5. Project Structure Setup

Your project structure should look like this:

```
Portfolio_website/Suru/
├── README.md
├── backend/
│   ├── config/
│   │   └── database.js
│   ├── controllers/
│   │   ├── projectController.js
│   │   ├── artworkController.js
│   │   └── gameController.js
│   ├── models/
│   │   ├── Project.js
│   │   ├── Artwork.js
│   │   └── Game.js
│   ├── routes/
│   │   ├── projects.js
│   │   ├── artwork.js
│   │   └── games.js
│   ├── middleware/
│   │   └── auth.js
│   ├── .env
│   ├── package.json
│   └── server.js
└── frontend/
    ├── public/
    │   └── index.html
    ├── src/
    │   ├── components/
    │   │   ├── Header/
    │   │   ├── Footer/
    │   │   ├── ProjectCard/
    │   │   ├── ArtworkCard/
    │   │   └── GameCard/
    │   ├── pages/
    │   │   ├── Landing/
    │   │   ├── Projects/
    │   │   ├── Artwork/
    │   │   ├── Games/
    │   │   └── Story/
    │   ├── styles/
    │   ├── utils/
    │   ├── App.js
    │   └── index.js
    ├── package.json
    └── build/
```

### 6. Database Schema Setup (Using Render)

#### Connect to your Render Database

You can connect to your Render PostgreSQL database in several ways:

**Option 1: Using Render's Web Console**
1. Go to your database on Render dashboard
2. Click "Connect" → "External Connection"
3. Use the provided connection details with any PostgreSQL client

**Option 2: Using psql command line**
```bash
# Use the External Database URL from Render
psql "postgresql://username:password@host:port/database"
```

**Option 3: Create a migration script in your backend**
Create `backend/migrations/init.sql`:

```sql
-- Projects table
CREATE TABLE IF NOT EXISTS projects (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    technologies VARCHAR(500),
    github_url VARCHAR(255),
    live_url VARCHAR(255),
    image_url VARCHAR(255),
    featured BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Artwork table
CREATE TABLE IF NOT EXISTS artwork (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    medium VARCHAR(100),
    image_url VARCHAR(255) NOT NULL,
    created_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Games table
CREATE TABLE IF NOT EXISTS games (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    technologies VARCHAR(500),
    play_url VARCHAR(255),
    github_url VARCHAR(255),
    image_url VARCHAR(255),
    is_playable BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Then run the migration from your backend code or manually execute the SQL in Render's console.

### 7. Running the Application

#### Start the backend server
```bash
cd backend
npm run dev
```

The backend will run on `http://localhost:5000`

#### Start the frontend development server
```bash
cd frontend
npm start
```

The frontend will run on `http://localhost:3000`

### 8. Building for Production

#### Build the frontend
```bash
cd frontend
npm run build
```

#### Configure backend to serve static files
Add the following to your `backend/server.js`:

```javascript
// Serve static files from React build
app.use(express.static(path.join(__dirname, '../frontend/build')));

// Catch all handler for React Router
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, '../frontend/build', 'index.html'));
});
```

## 📱 Pages Overview

### Landing Page
- Hero section with profile picture
- Personal summary
- Latest projects carousel (3-4 featured projects)
- Contact information
- Navigation to other sections

### Projects Page
- Grid layout of all coding projects
- Filter by technology/category
- Links to GitHub repositories and live demos
- Project descriptions and technologies used

### Artwork Page
- Gallery layout for artistic works
- Lightbox for viewing full-size images
- Categories/tags for different art types
- Creation dates and medium information

### Games Page
- Showcase of developed games
- Embedded playable games or external links
- Game descriptions and technologies used
- Screenshots and gameplay videos

### Story Page
- Animated text box with personal essay
- Smooth scrolling and fade-in effects
- Contact form at the bottom
- Social media links

## 🎨 Styling Guidelines (Tailwind CSS)

- **Color Scheme**: Use Tailwind's color palette with custom theme extensions
- **Responsive Design**: Utilize Tailwind's responsive prefixes (sm:, md:, lg:, xl:, 2xl:)
- **Animations**: Use Tailwind's built-in animations and custom ones defined in config
- **Typography**: Leverage Tailwind's typography classes and add Google Fonts if needed
- **Components**: Create reusable component classes using @apply directive
- **Dark Mode**: Use Tailwind's dark mode classes (dark:)

### Example Tailwind Classes for Your Portfolio:
```css
/* Header/Navigation */
.header-gradient { @apply bg-gradient-to-r from-blue-600 to-purple-600; }

/* Cards */
.project-card { @apply bg-white dark:bg-gray-800 shadow-lg rounded-lg p-6 hover:shadow-xl transition-shadow; }

/* Animations */
.fade-in { @apply animate-fade-in; }
.slide-up { @apply animate-slide-up; }

/* Buttons */
.btn-primary { @apply bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded transition-colors; }
```

## 🔧 Environment Variables

Make sure to set up the following environment variables:

**Backend (.env):**
```env
PORT=5000
# Render Database Connection
DATABASE_URL=postgresql://username:password@host:port/database
# Or individual components from Render:
DB_HOST=your_render_db_host
DB_PORT=5432
DB_NAME=your_render_db_name
DB_USER=your_render_db_user
DB_PASSWORD=your_render_db_password
JWT_SECRET=your_jwt_secret
NODE_ENV=development
```

**Frontend (.env):**
```env
REACT_APP_API_URL=http://localhost:5000
```

## 📦 Deployment (Using Render)

### Backend Deployment

1. **Push your code to GitHub**
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

2. **Create a Web Service on Render**
   - Go to Render dashboard
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Configure the service:
     - **Name**: `portfolio-backend`
     - **Root Directory**: `backend`
     - **Environment**: `Node`
     - **Build Command**: `npm install`
     - **Start Command**: `npm start`

3. **Configure Environment Variables**
   - In your Render web service dashboard
   - Go to "Environment" tab
   - Add all your environment variables:
     ```
     DATABASE_URL=<your_render_database_internal_url>
     JWT_SECRET=<your_jwt_secret>
     NODE_ENV=production
     ```

### Frontend Deployment

1. **Create another Web Service for Frontend**
   - Click "New +" → "Web Service"
   - Connect the same GitHub repository
   - Configure:
     - **Name**: `portfolio-frontend`
     - **Root Directory**: `frontend`
     - **Environment**: `Node`
     - **Build Command**: `npm install && npm run build`
     - **Start Command**: `npx serve -s build -l 3000`

2. **Configure Frontend Environment Variables**
   ```
   REACT_APP_API_URL=https://your-backend-service-url.onrender.com
   ```

### Database is Already Set Up!
Since you created your PostgreSQL database in step 2, it's already running on Render and ready to use.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Contact

**Suru Afariogun**
- Email: suruafa@gmail.com
- LinkedIn: [Suru Afariogun](https://www.linkedin.com/in/suru-afariogun-978b00340)
- GitHub: [Suru-Afariogun](https://github.com/Suru-Afariogun)

---

Made with ❤️ by Suru Afariogun