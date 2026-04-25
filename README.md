# 📚 Student Task Tracker

A beautiful, modern, and fully responsive task management application built with Svelte. Stay organized, focused, and on top of your assignments with this sleek productivity tool.

![Student Task Tracker](https://img.shields.io/badge/Svelte-5.55.4-orange)
![Vite](https://img.shields.io/badge/Vite-8.0.10-646CFF)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E)
![Responsive](https://img.shields.io/badge/Responsive-Yes-4CAF50)

## ✨ Features

### 🎯 Core Functionality
- ✅ **Add Tasks** - Create new tasks with smart validation
- ✅ **Toggle Completion** - Click to mark tasks as complete/incomplete
- ✅ **Delete Tasks** - Remove unwanted tasks instantly
- ✅ **Persistent Storage** - Tasks automatically save to localStorage
- ✅ **Duplicate Prevention** - Smart validation prevents duplicate tasks

### 🎨 Modern Design
- 🌈 **Beautiful Gradients** - Stunning purple-blue gradient backgrounds
- ✨ **Glassmorphism** - Modern glass-like effects with backdrop blur
- 🎭 **Dark/Light Theme** - Seamless theme switching with smooth transitions
- 📱 **Fully Responsive** - Perfect on desktop, tablet, and mobile
- 🎬 **Smooth Animations** - Scale, slide, and fade transitions throughout

### 📊 Advanced Features
- 📈 **Progress Tracking** - Visual progress bar showing completion percentage
- 📊 **Statistics Dashboard** - Total, completed, and pending task counters
- 🔔 **Toast Notifications** - Success, error, warning, and info messages
- 🧹 **Bulk Actions** - Clear all completed tasks at once
- ⌨️ **Keyboard Navigation** - Full keyboard accessibility support

## 🚀 Live Demo

Experience the app live: [Student Task Tracker Demo](https://your-github-username.github.io/student-task-tracker)

## 🛠️ Tech Stack

- **Frontend Framework:** [Svelte 5](https://svelte.dev/) - The magical disappearing UI framework
- **Build Tool:** [Vite](https://vitejs.dev/) - Lightning-fast build tool
- **Styling:** Pure CSS with CSS Variables - No external CSS frameworks
- **Icons:** Unicode Emoji - Lightweight and accessible
- **Storage:** Browser localStorage - Client-side persistence
- **Deployment:** GitHub Pages - Free and easy hosting

## 📦 Installation

### Prerequisites
- Node.js (version 16 or higher)
- npm or yarn package manager

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/student-task-tracker.git
   cd student-task-tracker
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start the development server**
   ```bash
   npm run dev
   ```

4. **Open your browser**
   Navigate to `http://localhost:5173` (or the port shown in your terminal)

### Build for Production

```bash
# Create production build
npm run build

# Preview production build
npm run preview
```

## 🎮 Usage

### Adding Tasks
1. Type your task in the input field
2. Press Enter or click the "➕ Add Task" button
3. Your task will appear in the list with a smooth animation

### Managing Tasks
- **Complete a task:** Click anywhere on the task item
- **Delete a task:** Click the trash icon (🗑️) on the right
- **View progress:** Check the progress bar and statistics at the top

### Theme Switching
- Click the moon/sun icon in the top-right corner
- Seamlessly switch between light and dark themes

### Keyboard Shortcuts
- **Enter:** Add a new task (when input is focused)
- **Tab:** Navigate through interactive elements
- **Space/Enter:** Toggle task completion (when task is focused)

## 📁 Project Structure

```
student-task-tracker/
├── public/
│   └── vite.svg
├── src/
│   ├── App.svelte          # Main application component
│   ├── app.css            # Global styles and CSS variables
│   ├── main.js            # Application entry point
│   └── assets/            # Static assets
├── package.json           # Dependencies and scripts
├── vite.config.js         # Vite configuration
├── jsconfig.json          # JavaScript configuration
├── svelte.config.js       # Svelte configuration
└── README.md             # This file
```

## 🎨 Design Philosophy

### Color Palette
- **Primary:** Purple-blue gradients (`#667eea` to `#764ba2`)
- **Secondary:** Pink-purple gradients (`#f093fb` to `#f5576c`)
- **Success:** Blue-green gradients (`#4facfe` to `#00f2fe`)
- **Warning:** Orange-yellow gradients (`#fa709a` to `#fee140`)
- **Error:** Red gradients (`#ff6b6b` to `#ee5a24`)

### Typography
- **Font Family:** Inter (system font stack fallback)
- **Weights:** 400, 500, 600, 700, 800
- **Sizes:** Responsive with `clamp()` for optimal readability

### Animations
- **Duration:** 200-400ms for smooth interactions
- **Easing:** CSS `ease` for natural motion
- **Types:** Scale, slide, fade, and transform effects

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Make your changes**
4. **Test thoroughly**
   ```bash
   npm run build
   npm run preview
   ```
5. **Commit your changes**
   ```bash
   git commit -m 'Add amazing feature'
   ```
6. **Push to the branch**
   ```bash
   git push origin feature/amazing-feature
   ```
7. **Open a Pull Request**

### Development Guidelines
- Follow the existing code style
- Add comments for complex logic
- Test on multiple devices and browsers
- Ensure accessibility compliance
- Keep the bundle size optimized

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Svelte Team** - For creating an amazing framework
- **Vite Team** - For the incredible build tool
- **Inter Font** - For the beautiful typography
- **Open Source Community** - For inspiration and tools

## 📞 Support

If you find this project helpful, please:
- ⭐ **Star the repository** on GitHub
- 🐛 **Report bugs** via GitHub Issues
- 💡 **Suggest features** for future improvements
- 📢 **Share** with your friends and colleagues

---

**Made with ❤️ and Svelte by : Abdullahi Abdiweli Adam**

*Stay organized, stay productive!* 🚀
