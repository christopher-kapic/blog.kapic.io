---
title: "Authentication with React and Firebase"
date: 2021-05-29T11:21
thumb: "react_firebase.png"
tags: 
    - React
    - Firebase
    - Authentication
---

Authentication is something that most public applications will need, and it is a pain to configure, so I figured documenting how I do authentication with React and Firebase will be helpful for others (and for myself when I inevitably forget a step). I will update this blog as my process for authentication changes. Also, my initial strategy is derived from [h3webdevtut's tutorial](https://youtu.be/cFgoSrOui2M).

This tutorial assumes you already have created a Firebase project, a web app for the project, and you have a React project set up.

For a quick copy/paste, scroll to the bottom of this page.

## 1. Install `firebase`

    npm install firebase

## 2. Add your firebaseConfig

Create a file called `/src/fire.js`.

```js
// firebaseConfig at /src/fire.js
import firebase from 'firebase'

const firebaseConfig = {
    apiKey: 'yourAPIKey',
    authDomain: 'yourAuthDomain',
    databaseURL: 'yourDatabaseURL',
    projectId: 'yourProjectId',
    storageBucket: 'yourStorageBucket',
    messagingSenderId: 'yourMessagingSenderId',
    appId: 'yourAppId'
}

const fire = firebase.initializeApp(firebaseConfig);

export default fire;
```

Optional (will update later): You can put your firebaseConfig variables in a `.env` file.

## 3. Enable Authentication method in the Firebase Console

In the Firebase console, enable the email and password sign-in method.

## 4. Imports, Create state variables

In `/src/App.js` (or whatever file your app is in), import `useState` and `useEffect`:

    import React, { useState, useEffect } from 'react'
    import fire from './fire'

Add states for keeping track of the user:

```js
const App () => {
    const [user, setUser] = useState('');
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [emailError, setEmailError] = useState('');
    const [passwordError, setPasswordError] = useState('');
    const [hasAccount, setHasAccount] = useState(false);

    ...
}
```

## 5. Create functions to handle events

Define two helper functions to clear inputs and errors:

`clearInputs()`:
```js
const clearInputs = () => {
    setEmail('');
    setPassword('');
}
```
`clearErrors()`:
```js
const clearErrors = () => {
    setEmailError('');
    setPasswordError('');
}
```

`handleLogin()` for when a user logs in:
```js
const handleLogin = () => {
    clearErrors();
    fire
        .auth()
        .signInWithEmailAndPassword(email, password)
        .catch(err => {
            // user firebase err code to determine kind of error
            // and handle it accordingly
            switch(err.code) {
                case "auth/invalid-email":
                case "auth/user-disabled":
                case "auth/user-not-found":
                    setEmailError(err.message);
                    break;
                case "auth/wrong-password":
                    setPasswordError(err.message);
                    break;
            }
        })
}
```

`handleSignup()` for when a user signs up:
```js
const handleSignup = () => {
    clearErrors();
    fire
        .auth()
        .createUserWithEmailAndPassword(email, password)
        .catch(err => {
            // user firebase err code to determine kind of error
            // and handle it accordingly
            switch(err.code) {
                case "auth/email-already-in-use":
                case "auth/invalid-email":
                    setEmailError(err.message);
                    break;
                case "auth/weak-password":
                    setPasswordError(err.message);
                    break;
            }
        })
}
```

`handleLogout()` for when a user logs out:
```js
const handleLogout = () => {
    fire.auth().signOut();
}
```

`authListener()` for updating user state:
```js
const authListener = () => {
    fire.auth().onAuthStateChanged(user => {
        if (user) {
            clearInputs();
            setUser(user);
        } else {
            setUser('');
        }
    })
}
```
Also, call `authListener()` with a `useEffect()`:
```js
useEffect(() => {
    authListener();
}, [])
```

At this point, `/src/App.js` should look something like this:
```js
import React, { useState, useEffect } from 'react'
import fire from './fire'

const App () => {
    const [user, setUser] = useState('');
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [emailError, setEmailError] = useState('');
    const [passwordError, setPasswordError] = useState('');
    const [hasAccount, setHasAccount] = useState(false);

    const clearInputs = () => {
        setEmail('');
        setPassword('');
    }

    const clearErrors = () => {
        setEmailError('');
        setPasswordError('');
    }

    const handleLogin = () => {
        clearErrors();
        fire
            .auth()
            .signInWithEmailAndPassword(email, password)
            .catch(err => {
                // use firebase err code to determine kind of error
                // and handle it accordingly
                switch(err.code) {
                    case "auth/invalid-email":
                    case "auth/user-disabled":
                    case "auth/user-not-found":
                        setEmailError(err.message);
                        break;
                    case "auth/wrong-password":
                        setPasswordError(err.message);
                        break;
                }
            })
    }

    const handleSignup = () => {
        clearErrors();
        fire
            .auth()
            .createUserWithEmailAndPassword(email, password)
            .catch(err => {
                // use firebase err code to determine kind of error
                // and handle it accordingly
                switch(err.code) {
                    case "auth/email-already-in-use":
                    case "auth/invalid-email":
                        setEmailError(err.message);
                        break;
                    case "auth/weak-password":
                        setPasswordError(err.message);
                        break;
                }
            })
    }

    const handleLogout = () => {
        fire.auth().signOut();
    }

    const authListener = () => {
        fire.auth().onAuthStateChanged(user => {
            if (user) {
                clearInputs();
                setUser(user);
            } else {
                setUser('');
            }
        })
    }

    useEffect(() => {
        authListener();
    }, [])

    ...
}
```

## 6. Create Login component

Create a component for the login page. For example, `/src/components/Login.js`.

```js
import React from 'react'

const Login = (props) => {
    const {
        email,
        setEmail,
        password,
        setPassword,
        handleLogin,
        handleSignup,
        hasAccount,
        setHasAccount,
        emailError,
        passwordError
    } = props;

    return (
        <section>
            <div>
                <label>Username</label>
                <input type="text" autoFocus required value={email} onChange={e => setEmail(e.target.value)}/>
                <p>{emailError}</p>
                <label>Password</label>
                <input type="text" required value={password} onChange={e => setPassword(e.target.value)}/>
                <p>{passwordError}</p>
                <div>
                    { hasAccount ? (
                        <>
                        <button onClick={() => handleLogin()}>Sign In</button>
                        <p>Don't have an account? <span onClick={setHasAccount(!hasAccount)}>Sign up</span></p>
                        </>
                    ) : (
                        <>
                        <button onClick={() => handleSignup()}>Sign Up</button>
                        <p>Already have an account? <span onClick={setHasAccount(!hasAccount)}>Sign in</span></p>
                        </>
                    )}
                </div>
            </div>
        </section>
    )
}

export default Login;
```

The above component is the barebones code you need to handle user signups and logins. You will have to add your own styling.

## 7. Add the Login component to your App

In `/src/App.js`, import the Login component:
    import Login from './components/Login'

And in the function for the App component, return the Login component conditionally on user:

```js
const App = () => {
    ...

    return (
        {hasAccount ? (
            <Login
            email={email}
            setEmail={setEmail}
            password={password}
            setPassword={setPassword}
            handleLogin={handleLogin}
            handleSignup={handleSignup}
            hasAccount={hasAccount}
            setHasAccount={setHasAccount}
            emailError={emailError}
            passwordError={passwordError}
            />
        ) : (

        )}
    )
}
```

## 8. Things to note

This tutorial does not (yet) include how to store user ID's in a database, nor does it show how to handle welcome and forgot password emails. I plan on adding those in the future, as I have time and understand Firebase authentication better.

If you notice an error in this blog post, [email me](mailto:christopherkapic@gmail.com).

## All code
For a quick copy/paste.

`/src/App.js`

```js
import React, { useState, useEffect } from 'react'
import fire from './fire'

const App () => {
    const [user, setUser] = useState('');
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [emailError, setEmailError] = useState('');
    const [passwordError, setPasswordError] = useState('');
    const [hasAccount, setHasAccount] = useState(false);

    const clearInputs = () => {
        setEmail('');
        setPassword('');
    }

    const clearErrors = () => {
        setEmailError('');
        setPasswordError('');
    }

    const handleLogin = () => {
        clearErrors();
        fire
            .auth()
            .signInWithEmailAndPassword(email, password)
            .catch(err => {
                // use firebase err code to determine kind of error
                // and handle it accordingly
                switch(err.code) {
                    case "auth/invalid-email":
                    case "auth/user-disabled":
                    case "auth/user-not-found":
                        setEmailError(err.message);
                        break;
                    case "auth/wrong-password":
                        setPasswordError(err.message);
                        break;
                }
            })
    }

    const handleSignup = () => {
        clearErrors();
        fire
            .auth()
            .createUserWithEmailAndPassword(email, password)
            .catch(err => {
                // use firebase err code to determine kind of error
                // and handle it accordingly
                switch(err.code) {
                    case "auth/email-already-in-use":
                    case "auth/invalid-email":
                        setEmailError(err.message);
                        break;
                    case "auth/weak-password":
                        setPasswordError(err.message);
                        break;
                }
            })
    }

    const handleLogout = () => {
        fire.auth().signOut();
    }

    const authListener() => {
        fire.auth().onAuthStateChanged(user => {
            if (user) {
                clearInputs();
                setUser(user);
            } else {
                setUser('');
            }
        })
    }

    useEffect(() => {
        authListener();
    }, [])

    return (
        {hasAccount ? (
            <Login
            email={email}
            setEmail={setEmail}
            password={password}
            setPassword={setPassword}
            handleLogin={handleLogin}
            handleSignup={handleSignup}
            hasAccount={hasAccount}
            setHasAccount={setHasAccount}
            emailError={emailError}
            passwordError={passwordError}
            />
        ) : (

        )}
    )
}
```

`/src/components/Login.js`

```js
import React from 'react'

const Login = (props) => {
    const {
        email,
        setEmail,
        password,
        setPassword,
        handleLogin,
        handleSignup,
        hasAccount,
        setHasAccount,
        emailError,
        passwordError
    } = props;

    return (
        <section>
            <div>
                <label>Username</label>
                <input type="text" autoFocus required value={email} onChange={e => setEmail(e.target.value)}/>
                <p>{emailError}</p>
                <label>Password</label>
                <input type="text" required value={password} onChange={e => setPassword(e.target.value)}/>
                <p>{passwordError}</p>
                <div>
                    { hasAccount ? (
                        <>
                        <button onClick={() => handleLogin()}>Sign In</button>
                        <p>Don't have an account? <span onClick={setHasAccount(!hasAccount)}>Sign up</span></p>
                        </>
                    ) : (
                        <>
                        <button onClick={() => handleSignup()}>Sign Up</button>
                        <p>Already have an account? <span onClick={setHasAccount(!hasAccount)}>Sign in</span></p>
                        </>
                    )}
                </div>
            </div>
        </section>
    )
}

export default Login;
```