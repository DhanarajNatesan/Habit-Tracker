# 🌱 HabitLeaf

**Simple, clean, and lightweight habit tracker**

A beautiful, mobile-friendly web application for tracking your daily habits. Built with vanilla HTML, CSS, and JavaScript, powered by Supabase for user authentication and data persistence.

![HabitLeaf Screenshot](https://via.placeholder.com/800x400/4ade80/ffffff?text=HabitLeaf+Habit+Tracker)

## ✨ Features

- 🔐 **User Authentication** - Secure login and registration with Supabase Auth
- 📱 **Mobile-First Design** - Responsive design that works perfectly on all devices
- 📊 **Multiple Views** - Weekly, Monthly, and Statistics views for each habit
- 🎯 **Interactive Tracking** - Click any day to mark habits as complete/incomplete
- 📈 **Progress Analytics** - View completion percentages and current streaks
- 💾 **Cloud Storage** - Your data is automatically saved and synced across devices
- 🚀 **No Installation Required** - Works directly in your web browser
- 🌙 **Clean UI** - Beautiful, minimalist interface with smooth animations

## 🚀 Quick Start

### Prerequisites

- A Supabase account (free at [supabase.com](https://supabase.com))
- A web browser
- Basic knowledge of HTML/CSS/JavaScript (for customization)

### Setup Instructions

1. **Clone or Download** this repository
2. **Set up Supabase Database** (see Database Setup section)
3. **Configure your Supabase credentials** in the HTML file
4. **Open the HTML file** in your web browser
5. **Register an account** and start tracking habits!

## 🗄️ Database Setup

### Step 1: Create Supabase Project

1. Go to [supabase.com](https://supabase.com) and create a new project
2. Wait for the project to be fully initialized
3. Get your project URL and anon key from Settings → API

### Step 2: Run Database Migration

Execute this SQL in your Supabase SQL Editor:

```sql
-- Create habits table
CREATE TABLE habits (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    weekly_data BOOLEAN[] DEFAULT ARRAY[false, false, false, false, false, false, false],
    monthly_data BOOLEAN[] DEFAULT ARRAY_FILL(false, ARRAY[31]),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable Row Level Security
ALTER TABLE habits ENABLE ROW LEVEL SECURITY;

-- Create policy to allow users to only see their own habits
CREATE POLICY "Users can only see their own habits" ON habits
    FOR ALL USING (auth.uid() = user_id);

-- Create updated_at trigger
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_habits_updated_at BEFORE UPDATE ON habits
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
```

### Step 3: Configure Authentication

1. Go to **Authentication** → **Settings** → **General Configuration**
2. **Turn OFF** "Confirm email" (recommended for easier testing)
3. Configure any other auth settings as needed

### Step 4: Update Configuration

Replace the Supabase configuration in the HTML file:

```javascript
// Replace these with your actual Supabase credentials
const SUPABASE_URL = 'your-project-url';
const SUPABASE_ANON_KEY = 'your-anon-key';
```

## 🎮 How to Use

### Getting Started
1. **Register** a new account or **login** with existing credentials
2. **Add your first habit** using the input field (e.g., "Drink 8 glasses of water")
3. **Click on days** in the calendar to mark habits as complete
4. **Switch between views** to see weekly, monthly, or statistics

### Views Explained

- **📅 Weekly View**: Track your habits for the current week (Sun-Sat)
- **🗓️ Monthly View**: See your progress for the entire month (31 days)
- **📊 Statistics View**: View completion percentages, streaks, and progress bars

### Tips for Success
- Start with 2-3 habits maximum
- Make habits specific and measurable
- Click the day when you complete the habit
- Check your statistics regularly for motivation

## 🛠️ Customization

### Changing Colors
Edit the CSS variables in the `:root` section:

```css
:root {
    --primary: #4ade80;        /* Main green color */
    --primary-dark: #22c55e;   /* Darker green */
    --background: #f0fdf4;     /* Light green background */
    --done: #4ade80;           /* Completed day color */
    --today: #fef3c7;          /* Today's highlight */
}
```

### Adding New Features
The code is modular and well-commented. Key functions:
- `addHabit(name)` - Add a new habit
- `toggleWeeklyDay(habitIndex, dayIndex)` - Toggle day completion
- `calculateWeeklyCompletion(habit)` - Calculate statistics
- `renderHabits()` - Re-render the habit list

## 📱 Mobile Optimization

HabitLeaf is built mobile-first with:
- Responsive grid layouts
- Touch-friendly buttons and click targets
- Optimized font sizes for mobile screens
- Smooth animations and transitions
- Minimal data usage

## 🔧 Technical Details

### Tech Stack
- **Frontend**: Vanilla HTML5, CSS3, JavaScript (ES6+)
- **Backend**: Supabase (PostgreSQL database)
- **Authentication**: Supabase Auth
- **Hosting**: Can be deployed anywhere (Netlify, Vercel, GitHub Pages, etc.)

### Browser Support
- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+
- Mobile browsers

### Performance
- Lightweight (~50KB total)
- No external dependencies except Supabase SDK
- Efficient data loading and caching
- Smooth 60fps animations

## 🚀 Deployment

### Option 1: Static Hosting (Recommended)
1. Upload the HTML file to any static hosting service
2. Update the Supabase configuration
3. Access via your domain

### Option 2: Local Development
1. Open the HTML file directly in your browser
2. Or use a local server: `python -m http.server 8000`

### Option 3: GitHub Pages
1. Push to a GitHub repository
2. Enable GitHub Pages in repository settings
3. Access via `username.github.io/repository-name`

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Ideas for Contributions
- Dark mode toggle
- Habit categories and icons
- Export data functionality
- Reminder notifications
- Social sharing features
- Habit streaks visualization
- Import/export habits

## 🐛 Troubleshooting

### Common Issues

**"Supabase not loaded" Error**
- Check your internet connection
- Verify Supabase CDN is accessible
- Try refreshing the page

**"Email not confirmed" Error**
- Disable email confirmation in Supabase settings
- Or check your email for confirmation link

**Can't click days**
- Ensure JavaScript is enabled
- Check browser console for errors
- Try refreshing the page

**Data not saving**
- Verify Supabase configuration
- Check database permissions
- Ensure you're logged in

### Getting Help
- Check the browser console for error messages
- Verify your Supabase setup matches the instructions
- Try testing with a fresh user account

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Supabase** for providing excellent backend-as-a-service
- **Tailwind CSS** for design inspiration
- **The habit tracking community** for feature ideas and feedback

## 📞 Support

- **Issues**: Please report bugs via GitHub Issues
- **Features**: Submit feature requests via GitHub Issues
- **Questions**: Check existing issues or create a new one

---

**Happy habit tracking! 🌱✨**

*Made with ❤️ for building better habits*