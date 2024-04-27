class UserCard extends HTMLElement {
    constructor() {
        super(); // Always call super first in the constructor of a custom element
        this.attachShadow({mode: 'open'}); // Encapsulates the styles and markup
        
        // Create elements inside the shadow root
        const wrapper = document.createElement('div');
        wrapper.setAttribute('class', 'user-card');

        const username = document.createElement('h2');
        username.textContent = this.getAttribute('name'); // Get the 'name' attribute from the element

        const info = document.createElement('p');
        info.textContent = `Email: ${this.getAttribute('email')}`; // Get the 'email' attribute from the element

        // Apply external styles
        const style = document.createElement('style');
        style.textContent = `
            .user-card {
                font-family: Arial, sans-serif;
                background: #f4f4f4;
                border: 1px solid #ddd;
                padding: 10px;
                margin-bottom: 10px;
            }
            h2 {
                color: #333;
            }
        `;

        // Attach the created elements to the shadow DOM
        this.shadowRoot.append(style, wrapper);
        wrapper.append(username, info);
    }
}

// Define the new element
customElements.define('user-card', UserCard);


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom Element Example</title>
    <script src="user-card.js" defer></script> <!-- Assuming the JavaScript is saved as user-card.js -->
</head>
<body>
    <h1>Welcome to Our Application</h1>
    <user-card name="John Doe" email="john@example.com"></user-card> <!-- Using the custom element -->
    <user-card name="Jane Smith" email="jane@example.com"></user-card> <!-- Using another instance of the custom element -->
</body>
</html>
