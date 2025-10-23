# App Flow Document for Pasteblink

## Onboarding and Sign-In/Sign-Up

When a new user first arrives at the Pasteblink application by opening the root URL in their browser, the system checks whether they already have an active session. If they do not, the application automatically directs them to the sign-in page. On this page, the user sees two clear options: to enter existing credentials or to create a new account. Choosing to sign up takes the user to a form that asks for an email address and a password. After filling out these fields and submitting the form, the application sends their information to the authentication API, which securely registers the user in the PostgreSQL database and establishes a session. If the registration succeeds, the user is redirected to the main dashboard.

If the user already has an account, they return to the sign-in form, enter their email and password, and submit. The credentials are validated by the same API route. On successful login, the server creates a session cookie, and the user is immediately taken to the dashboard. If the user cannot remember their password, they can click the “Forgot Password” link, which opens a simple form to request a password reset. They enter their email address, and the system sends a reset link to that address. Following the link brings them to a page where they can enter a new password. Once they submit the new password, they can use it to sign in normally.

Signing out is available at any time from the dashboard header. When the user clicks the sign-out button, the application clears the session on the server side and returns the user to the sign-in page.

## Main Dashboard or Home Page

After logging in, the user lands on the dashboard, which serves as the central hub of the application. At the top of the page, a header displays the application logo, a theme toggle button, and a sign-out button. The left side of the screen features a vertical navigation sidebar that remains visible on every authenticated page. This sidebar lists links such as “Dashboard” and “Settings,” allowing the user to move between major sections. In the main content area, the dashboard shows a set of cards with example data. These cards are placeholders for dynamic content and can be extended or replaced with custom widgets in the future.

The theme toggle button in the header allows the user to switch between light mode and dark mode instantly. The choice persists across sessions thanks to a theme management system built into the application.

## Detailed Feature Flows and Page Transitions

When the user clicks on the “Dashboard” link in the sidebar, the application performs a quick check to ensure the user’s session is still valid. If so, it displays the dashboard content. If the session has expired or the user is not authenticated, the application redirects to the sign-in page. On the dashboard, each card or widget can be designed to fetch data from a server component or an API route. Currently, these cards use static data, but they can be wired in to the Drizzle ORM to load real records from the PostgreSQL database.

Navigating to the “Settings” link in the sidebar loads a settings page where the user can update their profile information, change their password, or modify notification preferences. This page uses server components to fetch the latest account details when it loads. When the user makes changes and submits the settings form, the application calls a dedicated API route. The server updates the database and then redirects the user back to the settings page, displaying a confirmation message.

For any sensitive form, such as profile editing or password updates, the application sends the data through TypeScript-typed API routes. These routes validate the input, apply the changes to the database using Drizzle ORM, and return a success or error response. After successful updates, the user sees an inline confirmation message and can continue navigating through the app by using the sidebar links or header controls.

## Settings and Account Management

The settings page is accessible from anywhere in the app by clicking the “Settings” link in the sidebar. When the settings page first appears, it shows the user’s current email address and other profile details. Below these fields, a section for changing the password asks for the current password, a new password, and confirmation of the new password. The user can also find a toggle for email notifications about account activity. Once the user fills out any section of the form and clicks “Save,” the application sends the data to the appropriate API route. If the update succeeds, the page reloads with a message indicating the changes were applied. If there is an error, such as a wrong current password, an error message appears inline next to the relevant field.

After managing settings, the user can simply click on “Dashboard” or any other link in the sidebar to return to the main flow.

## Error States and Alternate Paths

If the user enters incorrect credentials during sign-in or sign-up, the application displays a clear message above the form indicating that the email or password is invalid. The input fields remain populated so the user can correct the mistake without retyping everything. In case of network issues, such as loss of connectivity, the application shows a banner at the top of the page warning the user that they are offline. Any form submissions attempted while offline are prevented, and an alert reminds the user to check their internet connection.

When the user attempts to visit a protected route like the dashboard or settings page without being authenticated, the application automatically redirects them to the sign-in page. If an unexpected server error occurs—such as a database error—the application navigates to a generic error page that explains something went wrong and offers a button to return to the dashboard or sign-in page, depending on whether the user still has a valid session.

## Conclusion and Overall App Journey

From the moment a new user lands on Pasteblink, they are guided through sign-up, sign-in, and password recovery in a clear and simple manner. Once authenticated, they arrive at a consistent dashboard with navigation controls, theme switching, and an entry point to settings. Each interaction, from updating profile information to logging out, follows a straightforward page transition backed by typed API routes and secure session handling. Error messages are displayed inline or via banners to keep the user informed. Ultimately, the flow ensures that any user can quickly sign up, explore the dashboard, personalize their account, and return to their work smoothly, making Pasteblink a solid foundation for building richer web experiences.