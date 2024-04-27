Parcel

// editUserParcel.js - This could be hosted separately and loaded as needed
export function bootstrap(props) {
    return Promise.resolve();
}

export function mount(props) {
    // Typically, render a React, Vue, Angular, etc. component into the DOM
    props.domElement.innerHTML = `<input type="text" value="${props.userData.name}" />`;
    return Promise.resolve();
}

export function unmount(props) {
    props.domElement.innerHTML = '';
    return Promise.resolve();
}



profile MF - parent

// Assuming this code is part of the user profile microfrontend

import { mountRootParcel } from 'single-spa';
let parcelInstance;

document.getElementById('editButton').addEventListener('click', function() {
    if (!parcelInstance) {
        // Dynamically import the parcel module
        System.import('url-to-editUserParcel.js').then(parcelModule => {
            const parcelProps = {
                domElement: document.getElementById('editContainer'),
                userData: { name: 'John Doe' }
            };

            // Mount the parcel using Single-spa's mountRootParcel
            parcelInstance = mountRootParcel(parcelModule, parcelProps);
        });
    }
});

document.getElementById('cancelEditButton').addEventListener('click', function() {
    if (parcelInstance) {
        parcelInstance.unmount().then(() => {
            parcelInstance = null;
        });
    }
});



// change the trigger from edit button to url sub routing


import { mountRootParcel } from 'single-spa';
import { routeChange } from './router'; // Hypothetical router utility

let parcelInstance;
const parcelProps = {
    domElement: document.getElementById('editContainer'),
    userData: { name: 'John Doe' }
};

function handleRouteChange() {
    const currentPath = window.location.pathname;
    // Check if the route corresponds to the parcel's path
    if (currentPath.endsWith('/parcel/address_parcel') && !parcelInstance) {
        // Dynamically import the parcel module when the route matches
        System.import('url-to-editUserParcel.js').then(parcelModule => {
            // Mount the parcel
            parcelInstance = mountRootParcel(parcelModule, parcelProps);
        });
    } else if (parcelInstance && !currentPath.endsWith('/parcel/address_parcel')) {
        // Unmount the parcel when navigating away
        parcelInstance.unmount().then(() => {
            parcelInstance = null;
        });
    }
}

// Subscribe to route changes
routeChange.subscribe(handleRouteChange);

// Initial check on page load
handleRouteChange();
