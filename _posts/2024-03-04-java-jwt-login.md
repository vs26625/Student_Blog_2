---
title: Java JWT Login
layout: base
description: A login screen that interacts with Java and obtains a JWT  
permalink: /java/login
---

## Login Page (Java)

<!-- Login Form -->
<form action="javascript:login_user()">
    <p><label>
        User ID:
        <input type="text" name="uid" id="uid" required>
    </label></p>
    <p><label>
        Password:
        <input type="password" name="password" id="password" required>
    </label></p>
    <p>
        <button>Login</button>
    </p>
    <p id="error-message" style="color: red;"></p>
</form>

<script>
    // URI identifies the resource
    let URI = '';
    if (location.hostname === "localhost") {  // location.hostname is built-in JavaScript property
        URI = "http://localhost:8085";
    } else if (location.hostname === "127.0.0.1") {
            URI = "http://127.0.0.1:8085";
    } else {
            URI = "https://spring.nighthawkcodingsociety.com";
    }
    // URL identifies the web address login
    const URL = URI + '/authenticate';
    // The redirect constant identifies the web page to open on login success
    const redirect_prefix = "{{ site.baseurl }}"; // Use a Liquid tag to get the baseurl
    const redirect = redirect_prefix + '/java/database'; 


    function login_user(){
        // Set body to include login data
        const body = {
            email: document.getElementById("uid").value,
            password: document.getElementById("password").value,
        };

        // Set Headers to support cross-origin
        const options = {
            method: 'POST',
            mode: 'cors', // no-cors, *cors, same-origin
            cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
            credentials: 'include', // include, *same-origin, omit
            body: JSON.stringify(body),
            headers: {
                "content-type": "application/json",
            },
        };

        document.getElementById("error-message").textContent = "";

        // Fetch JWT
        fetch(URL, options)
        .then(response => {
            // trap error response from Web API
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                document.getElementById("error-message").textContent = errorMsg;
                return;
            }
            // Success!!!
            // Redirect to Database location
            window.location.href = redirect;
        })
        .catch(error => {
            // Handle network errors
            console.log('Possible CORS or service down error: ' + error);
            document.getElementById("error-message").textContent = 'Possible CORS or service down error: ' + error;
        });
    }


</script>
