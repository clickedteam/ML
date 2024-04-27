export async function bootstrap(props) {
    // This function is for setting up the microfrontend environment,
    // It could be used to load data or configurations needed before mounting,
    // but for this example, there isn't anything specific to do.
    console.log('UserCard bootstrap');
}

export async function mount(props) {
    // This function is called to render the microfrontend to the DOM.
    console.log('UserCard mount');
    const { domElement, name, email } = props;

    const wrapper = document.createElement('div');
    wrapper.setAttribute('class', 'user-card');

    const username = document.createElement('h2');
    username.textContent = name; // Use 'name' from props

    const info = document.createElement('p');
    info.textContent = `Email: ${email}`; // Use 'email' from props

    // Styles for the component
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

    // Attach elements to the provided DOM element
    domElement.append(style, wrapper);
    wrapper.append(username, info);
    props.userCardWrapper = wrapper; // Store the wrapper for cleanup in unmount
}

export async function unmount(props) {
    // This function is called to clean up the DOM and remove listeners, etc.
    console.log('UserCard unmount');
    const { userCardWrapper, domElement } = props;
    domElement.removeChild(userCardWrapper);
}



//a seond js handling parcel load'


import { mountRootParcel } from 'single-spa';

const domElement = document.getElementById('user-card-container');
const parcelProps = {
    domElement: domElement,
    name: 'John Doe',
    email: 'john@example.com'
};

System.import('path/to/userCardParcel.js').then(parcelModule => {
    mountRootParcel(parcelModule, parcelProps);
});



//parcel defined in plain old js


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Parcel Example with Plain JavaScript</title>
</head>
<body>
    <div id="user-card-container"></div>

    <script>
        // Define the lifecycle methods directly
        function bootstrap() {
            console.log('UserCard bootstrap');
            return Promise.resolve();
        }

        function mount(props) {
            console.log('UserCard mount');
            const { domElement, name, email } = props;

            const wrapper = document.createElement('div');
            wrapper.setAttribute('class', 'user-card');

            const username = document.createElement('h2');
            username.textContent = name; // Use the name from props

            const info = document.createElement('p');
            info.textContent = `Email: ${email}`; // Use the email from props

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

            domElement.appendChild(style);
            domElement.appendChild(wrapper);
            wrapper.appendChild(username);
            wrapper.appendChild(info);

            return Promise.resolve();
        }

        function unmount(props) {
            console.log('UserCard unmount');
            const { domElement } = props;
            domElement.innerHTML = '';
            return Promise.resolve();
        }

        // Example usage:
        // Assuming single-spa or similar manager will call these methods,
        // otherwise, you would trigger these manually or with another script.
    </script>

    <script>
        // Simulating mounting and unmounting
        const userCardProps = {
            domElement: document.getElementById('user-card-container'),
            name: 'John Doe',
            email: 'john.doe@example.com'
        };

        mount(userCardProps).then(() => {
            console.log('Mounted successfully');
        });

        // Optionally, you can simulate unmounting later:
        setTimeout(() => {
            unmount(userCardProps).then(() => {
                console.log('Unmounted successfully');
            });
        }, 5000); // Unmount after 5 seconds
    </script>
</body>
</html>

