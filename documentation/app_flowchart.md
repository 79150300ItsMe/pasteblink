flowchart TD
  start[User visits app] --> signIn[Sign-in page]
  start --> signUp[Sign-up page]
  signIn --> authApi[Auth API route]
  signUp --> authApi
  authApi --> decision{Credentials valid}
  decision -->|Yes| db[PostgreSQL via Drizzle ORM]
  decision -->|No| signIn
  db --> sessionOk[Session created]
  sessionOk --> dashboard[Authenticated Dashboard]
  dashboard --> sidebar[Sidebar navigation]
  dashboard --> widgets[Custom widgets area]
  dashboard --> theme[Theme toggle]
  dashboard --> components[UI components]