<script>
  import { onMount } from 'svelte'
  import { fade, slide, scale } from 'svelte/transition'

  const STORAGE_KEY = 'student-task-tracker-tasks'
  const THEME_KEY = 'student-task-tracker-theme'

  let newTask = ''
  let tasks = []
  let theme = 'light'
  let showToast = false
  let toastMessage = ''
  let toastType = 'success'

  onMount(() => {
    const savedTasks = localStorage.getItem(STORAGE_KEY)
    const savedTheme = localStorage.getItem(THEME_KEY)

    if (savedTasks) {
      try {
        tasks = JSON.parse(savedTasks)
      } catch (e) {
        console.error('Failed to parse saved tasks:', e)
        tasks = []
      }
    }

    if (savedTheme === 'dark' || savedTheme === 'light') {
      theme = savedTheme
    }

    // Apply theme immediately
    updateTheme()
  })

  function updateTheme() {
    document.documentElement.dataset.theme = theme
  }

  function addTask() {
    const trimmed = newTask.trim()
    if (!trimmed) {
      showToastMessage('Please enter a task!', 'error')
      return
    }

    if (tasks.some(task => task.name.toLowerCase() === trimmed.toLowerCase())) {
      showToastMessage('Task already exists!', 'warning')
      return
    }

    const newTaskObj = {
      id: Date.now() + Math.random(),
      name: trimmed,
      completed: false,
      createdAt: new Date().toISOString()
    }

    tasks = [...tasks, newTaskObj]
    newTask = ''
    showToastMessage('✅ Task added successfully!', 'success')
  }

  function toggleTask(id) {
    tasks = tasks.map(task =>
      task.id === id ? { ...task, completed: !task.completed } : task
    )
  }

  function deleteTask(id) {
    tasks = tasks.filter(task => task.id !== id)
    showToastMessage('🗑️ Task deleted!', 'info')
  }

  function clearCompleted() {
    const completedCount = tasks.filter(task => task.completed).length
    if (completedCount === 0) {
      showToastMessage('No completed tasks to clear!', 'warning')
      return
    }

    tasks = tasks.filter(task => !task.completed)
    showToastMessage(`🧹 Cleared ${completedCount} completed task${completedCount > 1 ? 's' : ''}!`, 'success')
  }

  function showToastMessage(message, type = 'success') {
    toastMessage = message
    toastType = type
    showToast = true
    setTimeout(() => showToast = false, 3000)
  }

  function toggleTheme() {
    theme = theme === 'light' ? 'dark' : 'light'
    updateTheme()
  }

  // Reactive statements
  $: localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks))
  $: localStorage.setItem(THEME_KEY, theme)
  $: totalTasks = tasks.length
  $: completedTasks = tasks.filter(task => task.completed).length
  $: pendingTasks = totalTasks - completedTasks
  $: progressPercentage = totalTasks > 0 ? (completedTasks / totalTasks) * 100 : 0
</script>

<main class="app-container">
  {#if showToast}
    <div class="toast toast-{toastType}" transition:slide={{ duration: 400 }}>
      <span class="toast-icon">
        {#if toastType === 'success'}✅{:else if toastType === 'error'}❌{:else if toastType === 'warning'}⚠️{:else}ℹ️{/if}
      </span>
      <span class="toast-message">{toastMessage}</span>
    </div>
  {/if}

  <div class="hero-section">
    <div class="hero-content">
      <div class="hero-text">
        <h1 class="main-title">
          <span class="gradient-text">Student Task Tracker</span>
        </h1>
        <p class="hero-subtitle">Stay organized, stay focused, stay ahead</p>
      </div>

      <div class="hero-stats">
        <div class="stat-card">
          <div class="stat-number">{totalTasks}</div>
          <div class="stat-label">Total Tasks</div>
        </div>
        <div class="stat-card">
          <div class="stat-number completed">{completedTasks}</div>
          <div class="stat-label">Completed</div>
        </div>
        <div class="stat-card">
          <div class="stat-number pending">{pendingTasks}</div>
          <div class="stat-label">Pending</div>
        </div>
      </div>

      {#if totalTasks > 0}
        <div class="progress-section">
          <div class="progress-bar">
            <div class="progress-fill" style="width: {progressPercentage}%"></div>
          </div>
          <div class="progress-text">{Math.round(progressPercentage)}% Complete</div>
        </div>
      {/if}
    </div>

    <button class="theme-toggle" on:click={toggleTheme} aria-label="Toggle theme">
      <span class="theme-icon">{theme === 'light' ? '🌙' : '☀️'}</span>
    </button>
  </div>

  <div class="main-content">
    <div class="task-input-section">
      <div class="input-container">
        <input
          type="text"
          bind:value={newTask}
          placeholder="What needs to be done today?"
          class="task-input"
          on:keydown={(e) => e.key === 'Enter' && addTask()}
        />
        <button
          class="add-button"
          on:click={addTask}
          disabled={!newTask.trim()}
        >
          <span class="add-icon">➕</span>
          <span class="add-text">Add Task</span>
        </button>
      </div>
    </div>

    <div class="tasks-section">
      {#if tasks.length === 0}
        <div class="empty-state" transition:fade={{ duration: 300 }}>
          <div class="empty-icon">🎯</div>
          <h3 class="empty-title">No tasks yet</h3>
          <p class="empty-description">Add your first task to get started on your journey!</p>
        </div>
      {:else}
        <div class="tasks-header">
          <h2 class="tasks-title">Your Tasks</h2>
          {#if completedTasks > 0}
            <button class="clear-button" on:click={clearCompleted}>
              <span class="clear-icon">🧹</span>
              Clear Completed
            </button>
          {/if}
        </div>

        <div class="tasks-list">
          {#each tasks as task (task.id)}
            <div
              class="task-item {task.completed ? 'completed' : ''}"
              transition:scale={{ duration: 200, start: 0.9 }}
            >
              <div
                class="task-content"
                role="button"
                tabindex="0"
                on:click={() => toggleTask(task.id)}
                on:keydown={(e) => e.key === 'Enter' && toggleTask(task.id)}
                aria-label="Toggle task completion"
              >
                <div class="task-checkbox">
                  {#if task.completed}
                    <span class="checkmark">✓</span>
                  {:else}
                    <span class="checkbox-empty"></span>
                  {/if}
                </div>

                <div class="task-details">
                  <div class="task-name {task.completed ? 'completed-text' : ''}">
                    {task.name}
                  </div>
                  <div class="task-meta">
                    <span class="task-status">
                      {task.completed ? '✅ Completed' : '⏳ In Progress'}
                    </span>
                    <span class="task-date">
                      {new Date(task.createdAt).toLocaleDateString()}
                    </span>
                  </div>
                </div>
              </div>

              <button
                class="delete-task-btn"
                on:click={() => deleteTask(task.id)}
                aria-label="Delete task"
              >
                <span class="delete-icon">🗑️</span>
              </button>
            </div>
          {/each}
        </div>
      {/if}
    </div>
  </div>
</main>

<style>
  :global(:root) {
    --primary-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    --secondary-gradient: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
    --success-gradient: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
    --warning-gradient: linear-gradient(135deg, #fa709a 0%, #fee140 100%);
    --error-gradient: linear-gradient(135deg, #ff6b6b 0%, #ee5a24 100%);

    --bg-primary: #ffffff;
    --bg-secondary: #f8fafc;
    --bg-tertiary: #f1f5f9;
    --text-primary: #1e293b;
    --text-secondary: #64748b;
    --text-muted: #94a3b8;
    --border-color: #e2e8f0;
    --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
    --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
    --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
    --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1);

    --radius-sm: 8px;
    --radius-md: 12px;
    --radius-lg: 16px;
    --radius-xl: 24px;

    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    line-height: 1.6;
    color: var(--text-primary);
  }

  :global([data-theme='dark']) {
    --bg-primary: #0f172a;
    --bg-secondary: #1e293b;
    --bg-tertiary: #334155;
    --text-primary: #f8fafc;
    --text-secondary: #cbd5e1;
    --text-muted: #94a3b8;
    --border-color: #334155;
    --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.3);
    --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.4);
    --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.5);
    --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.6);
  }

  :global(*) {
    box-sizing: border-box;
  }

  :global(body) {
    margin: 0;
    padding: 0;
    min-height: 100vh;
    background: var(--primary-gradient);
    color: var(--text-primary);
    overflow-x: hidden;
  }

  .app-container {
    min-height: 100vh;
    position: relative;
  }

  .hero-section {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    padding: 2rem 1rem;
    position: relative;
  }

  :global([data-theme='dark']) .hero-section {
    background: rgba(15, 23, 42, 0.8);
    border-bottom-color: rgba(255, 255, 255, 0.1);
  }

  .hero-content {
    max-width: 1200px;
    margin: 0 auto;
    text-align: center;
  }

  .main-title {
    font-size: clamp(2.5rem, 5vw, 4rem);
    font-weight: 800;
    margin: 0 0 0.5rem;
    letter-spacing: -0.02em;
  }

  .gradient-text {
    background: var(--secondary-gradient);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-subtitle {
    font-size: 1.25rem;
    color: var(--text-secondary);
    margin: 0 0 2rem;
    font-weight: 400;
  }

  .hero-stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    gap: 1rem;
    margin-bottom: 2rem;
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
  }

  .stat-card {
    background: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(10px);
    border-radius: var(--radius-lg);
    padding: 1.5rem 1rem;
    box-shadow: var(--shadow-md);
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  :global([data-theme='dark']) .stat-card {
    background: rgba(30, 41, 59, 0.8);
    border-color: rgba(255, 255, 255, 0.1);
  }

  .stat-number {
    font-size: 2.5rem;
    font-weight: 800;
    margin-bottom: 0.25rem;
    color: var(--text-primary);
  }

  .stat-number.completed {
    background: var(--success-gradient);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .stat-number.pending {
    background: var(--warning-gradient);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .stat-label {
    font-size: 0.875rem;
    color: var(--text-secondary);
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .progress-section {
    max-width: 400px;
    margin: 0 auto;
  }

  .progress-bar {
    width: 100%;
    height: 8px;
    background: rgba(255, 255, 255, 0.3);
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 0.5rem;
  }

  .progress-fill {
    height: 100%;
    background: var(--secondary-gradient);
    border-radius: 4px;
    transition: width 0.3s ease;
  }

  .progress-text {
    font-size: 0.875rem;
    color: var(--text-secondary);
    font-weight: 500;
  }

  .theme-toggle {
    position: absolute;
    top: 1.5rem;
    right: 1.5rem;
    background: rgba(255, 255, 255, 0.9);
    border: 2px solid rgba(255, 255, 255, 0.3);
    border-radius: 50%;
    width: 3rem;
    height: 3rem;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: var(--shadow-md);
    font-size: 1.25rem;
  }

  :global([data-theme='dark']) .theme-toggle {
    background: rgba(30, 41, 59, 0.9);
    border-color: rgba(255, 255, 255, 0.2);
  }

  .theme-toggle:hover {
    transform: scale(1.1);
    box-shadow: var(--shadow-lg);
  }

  .main-content {
    max-width: 800px;
    margin: 0 auto;
    padding: 2rem 1rem;
  }

  .task-input-section {
    margin-bottom: 2rem;
  }

  .input-container {
    display: flex;
    gap: 1rem;
    background: var(--bg-primary);
    border-radius: var(--radius-xl);
    padding: 0.5rem;
    box-shadow: var(--shadow-lg);
    border: 1px solid var(--border-color);
  }

  :global([data-theme='dark']) .input-container {
    background: var(--bg-secondary);
  }

  .task-input {
    flex: 1;
    border: none;
    background: transparent;
    padding: 1rem 1.5rem;
    font-size: 1.125rem;
    outline: none;
    color: var(--text-primary);
  }

  .task-input::placeholder {
    color: var(--text-muted);
  }

  .add-button {
    background: var(--primary-gradient);
    color: white;
    border: none;
    border-radius: var(--radius-lg);
    padding: 1rem 2rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 1rem;
  }

  .add-button:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: var(--shadow-xl);
  }

  .add-button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    transform: none;
  }

  .tasks-section {
    background: var(--bg-primary);
    border-radius: var(--radius-xl);
    padding: 2rem;
    box-shadow: var(--shadow-lg);
    border: 1px solid var(--border-color);
  }

  :global([data-theme='dark']) .tasks-section {
    background: var(--bg-secondary);
  }

  .empty-state {
    text-align: center;
    padding: 3rem 1rem;
  }

  .empty-icon {
    font-size: 4rem;
    margin-bottom: 1rem;
    opacity: 0.7;
  }

  .empty-title {
    font-size: 1.5rem;
    font-weight: 600;
    margin: 0 0 0.5rem;
    color: var(--text-primary);
  }

  .empty-description {
    color: var(--text-secondary);
    margin: 0;
    font-size: 1rem;
  }

  .tasks-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
  }

  .tasks-title {
    font-size: 1.5rem;
    font-weight: 700;
    margin: 0;
    color: var(--text-primary);
  }

  .clear-button {
    background: var(--error-gradient);
    color: white;
    border: none;
    border-radius: var(--radius-lg);
    padding: 0.75rem 1.5rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 0.875rem;
  }

  .clear-button:hover {
    transform: translateY(-1px);
    box-shadow: var(--shadow-md);
  }

  .tasks-list {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .task-item {
    background: var(--bg-secondary);
    border-radius: var(--radius-lg);
    padding: 1.5rem;
    display: flex;
    align-items: center;
    gap: 1rem;
    box-shadow: var(--shadow-sm);
    border: 1px solid var(--border-color);
    transition: all 0.3s ease;
  }

  :global([data-theme='dark']) .task-item {
    background: var(--bg-tertiary);
  }

  .task-item:hover {
    box-shadow: var(--shadow-md);
    transform: translateY(-1px);
  }

  .task-item.completed {
    background: rgba(34, 197, 94, 0.1);
    border-color: rgba(34, 197, 94, 0.3);
  }

  :global([data-theme='dark']) .task-item.completed {
    background: rgba(34, 197, 94, 0.2);
  }

  .task-content {
    flex: 1;
    display: flex;
    align-items: center;
    gap: 1rem;
    cursor: pointer;
  }

  .task-checkbox {
    width: 24px;
    height: 24px;
    border-radius: 50%;
    border: 2px solid var(--border-color);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
    flex-shrink: 0;
  }

  .task-item.completed .task-checkbox {
    background: var(--success-gradient);
    border-color: transparent;
  }

  .checkmark {
    color: white;
    font-weight: bold;
    font-size: 0.875rem;
  }

  .checkbox-empty {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: var(--text-muted);
    opacity: 0.3;
  }

  .task-details {
    flex: 1;
  }

  .task-name {
    font-size: 1.125rem;
    font-weight: 600;
    margin-bottom: 0.25rem;
    color: var(--text-primary);
    transition: all 0.3s ease;
  }

  .task-name.completed-text {
    text-decoration: line-through;
    opacity: 0.6;
  }

  .task-meta {
    display: flex;
    align-items: center;
    gap: 1rem;
    font-size: 0.875rem;
  }

  .task-status {
    color: var(--text-secondary);
    font-weight: 500;
  }

  .task-date {
    color: var(--text-muted);
  }

  .delete-task-btn {
    background: rgba(239, 68, 68, 0.1);
    border: 1px solid rgba(239, 68, 68, 0.2);
    border-radius: var(--radius-md);
    padding: 0.5rem;
    cursor: pointer;
    transition: all 0.3s ease;
    flex-shrink: 0;
  }

  .delete-task-btn:hover {
    background: rgba(239, 68, 68, 0.2);
    transform: scale(1.1);
  }

  .delete-icon {
    font-size: 1rem;
  }

  .toast {
    position: fixed;
    top: 1.5rem;
    right: 1.5rem;
    padding: 1rem 1.5rem;
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-xl);
    display: flex;
    align-items: center;
    gap: 0.75rem;
    font-weight: 500;
    z-index: 1000;
    max-width: 400px;
    backdrop-filter: blur(10px);
  }

  .toast-success {
    background: rgba(34, 197, 94, 0.95);
    color: white;
    border: 1px solid rgba(34, 197, 94, 0.3);
  }

  .toast-error {
    background: rgba(239, 68, 68, 0.95);
    color: white;
    border: 1px solid rgba(239, 68, 68, 0.3);
  }

  .toast-warning {
    background: rgba(245, 158, 11, 0.95);
    color: white;
    border: 1px solid rgba(245, 158, 11, 0.3);
  }

  .toast-info {
    background: rgba(59, 130, 246, 0.95);
    color: white;
    border: 1px solid rgba(59, 130, 246, 0.3);
  }

  .toast-icon {
    font-size: 1.25rem;
    flex-shrink: 0;
  }

  .toast-message {
    flex: 1;
    font-size: 0.875rem;
  }

  @media (max-width: 768px) {
    .hero-section {
      padding: 1.5rem 1rem;
    }

    .hero-stats {
      grid-template-columns: repeat(3, 1fr);
      gap: 0.75rem;
    }

    .stat-card {
      padding: 1rem 0.5rem;
    }

    .stat-number {
      font-size: 2rem;
    }

    .main-content {
      padding: 1rem;
    }

    .input-container {
      flex-direction: column;
      gap: 0.5rem;
    }

    .add-button {
      width: 100%;
      justify-content: center;
    }

    .tasks-header {
      flex-direction: column;
      gap: 1rem;
      align-items: stretch;
    }

    .clear-button {
      align-self: flex-start;
    }

    .task-content {
      gap: 0.75rem;
    }

    .task-meta {
      flex-direction: column;
      align-items: flex-start;
      gap: 0.25rem;
    }

    .toast {
      left: 1rem;
      right: 1rem;
      top: 1rem;
    }
  }
</style>
