---
title: How to deploy a NodeJS server with NGINX on a VPS
date: 2021-07-01T23:22
thumb: node_redis_deployment.png
tags: 
    - NodeJS
    - NGINX
    - VPS
    - Vultr
    - Redis
---

Well, I did it [once](/react_firebase_authentication/)—I can do it again.

```React
import React, { useState, useEffect } from 'react'
import { Link } from "gatsby"
import { StaticImage } from "gatsby-plugin-image"
import GoTrue from 'gotrue-js'
import TextField from '@material-ui/core/TextField';

import Layout from "../components/layout"
import Seo from "../components/seo"
import Button from '@material-ui/core/Button';

const auth = new GoTrue({
  APIUrl: 'https://kapic-nip.netlify.app/.netlify/identity',
  audience: '',
  setCookie: true,
});

const IndexPage = () => {
  const [email, setEmail] = useState('')
  const [err, setErr] = useState('')
  const [password, setPassword] = useState('')
  const [hasAccount, setHasAccount] = useState(false)
  const [signupResponse, setSignupResponse] = useState('')
  const [loginResponse, setLoginResponse] = useState('')
  const [userData, setUserData] = useState('')
  const [oldJWT, setOldJWT] = useState('')
  const [newJWT, setNewJWT] = useState('')
  const [loggedIn, setLoggedIn] = useState(false)
  const [confirmationHash, setConfirmationHash] = useState('')

  const handleSignup = () => {
    auth.signup(email, password)
    .then(res => setSignupResponse(res))
    .then(() => setLoggedIn(true))
    .catch(err => setErr(err.message))
  }

  const handleLogin = () => {
    auth.login(email, password, true)
    .then(res => setLoginResponse(res))
    .then(() => setLoggedIn(true))
    .catch(err => setErr(err.message))
  }

  useEffect(async () => {
    const user = auth.currentUser()
    if (window.location.hash) {
      console.log('hash check passed')
      setConfirmationHash(window.location.hash)
      auth.confirm(window.location.hash, true)
      .then((res) => console.log('response', res))
      .catch(err => console.log('error:', err.message))
    }
    if (user) {
      user.update(
        {
          email: 'christopherkapic2022@u.northwestern.edu', // update email
          password: 'test1234',                             // update password
          data: {
            full_name: 'Christopher Kapic'                  // update name (in Netlify)
            // additional field: 'value',
          }
        }
      )
      .then((user) => console.log('Updated user', user))
      setLoggedIn(true)
      // const old_jwt = await user.jwt(false)
      // const new_jwt = await user.jwt(true)
      setUserData(user)
    }
  }, [])

  return (
    <Layout>
      { !loggedIn ? (
        <>
      <div style={{ display: 'flex', flexDirection: 'column' }}>
        <TextField id="standard-basic" label="email" style={{ width: 300 }} autoFocus onChange={(e) => setEmail(e.target.value)}/>
        <TextField id="standard-basic" label="password" type="password" style={{ width: 300 }} onChange={(e) => setPassword(e.target.value)}/>
      </div>
      <div style={{ marginTop: 10, marginBottom: 10 }}>
        { hasAccount ? (
          <Button variant="contained" color="primary" onClick={() => handleLogin()}>Log in</Button>
        ) : (
          <Button variant="contained" color="primary" onClick={() => handleSignup()}>Sign up</Button>
        )}
      </div>
      <div>
        <p>
          { hasAccount ? (
            <p>Don't have an account? <span onClick={() => setHasAccount(!hasAccount) } style={{ cursor: 'pointer', color: 'blue' }}>Sign up.</span></p>
          ) : (
            <p>Already have an account? <span onClick={() => setHasAccount(!hasAccount) } style={{ cursor: 'pointer', color: 'blue'}}>Log in.</span></p>
          )}
        </p>
      </div>
      </>
      ) : (
        <>
        <h1>{userData.email}</h1>
        <Button variant="contained" color="secondary" style={{ marginBottom: 4 }} onClick={() => {
          const user = auth.currentUser()
          user.logout()
          setLoggedIn(false)
        }}>Log out</Button>
        </>
      )}
      <h1>Confirmation Hash:</h1>
      <pre>{ confirmationHash.replace('#confirmation_token=', '') }</pre>
      <h1>Signup response:</h1>
      <pre>{JSON.stringify(signupResponse, null, 2)}</pre>
      <h1>Login response:</h1>
      <pre>{JSON.stringify(loginResponse, null, 2)}</pre>
      <h1>User data (if logged in):</h1>
      <pre>{JSON.stringify(userData, null, 2)}</pre>
      <h1>Errors:</h1>
      <pre>{err}</pre>
      {/* <h1>Old JWT:</h1>
      <pre>{oldJWT}</pre>
      <h1>New JWT:</h1>
      <pre>{newJWT}</pre> */}
   </Layout>
  )
}

export default IndexPage
```