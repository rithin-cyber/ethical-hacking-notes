# Date

2026-08-03

# Objective

Understand the authentication workflow and learn how browsers, servers, sessions, and cookies work during login.

# Login Flow

Explain the complete authentication workflow:

- User opens the login page.
- Browser sends a GET request to receive the login page.
- User enters username and password.
- Browser sends a POST request containing the credentials.
- Server verifies the credentials.
- If valid, the server creates a session.
- Server sends a Set-Cookie header with the session ID.
- Browser stores the cookie.
- Browser automatically sends the Cookie header with future requests.
- Server recognizes the user using the session ID.
- During logout, the session is destroyed or expires.

# Session Creation

After successful authentication, the server creates a unique session for the user.
The server returns a Set-Cookie header containing the session ID.
The browser stores this cookie automatically.
The session allows the server to recognize the authenticated user without requiring the user to log in again on every request.

# Cookie

A cookie stores the session identifier in the browser.
The browser automatically includes the Cookie header with every request to the same website.
The server uses the session ID inside the cookie to identify the logged-in user.
Cookies do not usually store the user's password.

# Logout

When the user logs out, the server destroys or invalidates the session.
The session cookie becomes invalid or expires.
Future requests using that session ID are no longer authenticated.

# Burp Observations

Today I intercepted a login request using Burp Suite.

Observations:

- Login request used the POST method.
- Login endpoint: /u/login?state=
- Username parameter was visible.
- Password parameter was visible.
- Content-Type: application/x-www-form-urlencoded
- I observed both Cookie and Set-Cookie headers.
- I learned that Set-Cookie is sent by the server after successful authentication, while Cookie is automatically sent by the browser in future requests.

# Browser Observations

I used Chrome DevTools (Network tab) to observe the login request.

I noticed:

- The login request used POST.
- The response returned HTTP 200 OK.
- TryHackMe was using Google One Tap authentication, so instead of a normal username/password request, I observed authentication tokens.
- This helped me understand that some websites use OAuth tokens instead of traditional credentials.

# Questions

- How does the server store session information internally?
- What happens if someone steals a valid session cookie?
- What is the difference between a session cookie and a persistent cookie?
- How does session expiration work?
- How does session regeneration help prevent session fixation attacks?

# What I Learned

Today I understood the complete authentication workflow instead of just memorizing it.

Key takeaways:

- Authentication verifies the user's identity.
- Login pages are usually requested using GET.
- Credentials are normally submitted using POST.
- The server creates a session after successful authentication.
- Set-Cookie tells the browser to store the session ID.
- The browser automatically sends the Cookie header with future requests.
- Logout destroys or invalidates the session.
- Before testing authentication, a penetration tester should first inspect the login request, parameters, HTTP method, session creation, and cookies to fully understand the workflow.
