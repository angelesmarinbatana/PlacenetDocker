# Replicating Placenet App via Docker

## Pre-requisites

- Download Docker for Desktop (v4.35.1 tested)
    - https://www.docker.com/products/docker-desktop/ 
    - Run the executable and open the app when done

- Also download Expo Go onto your mobile device. The Web build will not work.
    - https://expo.dev/go
 
## Clone repos

- Clone this repo via command line:
    - `git clone https://github.com/angelesmarin/PlacenetDocker.git`

- CD into the PlacenetDocker folder and clone the frontend.
    - `git clone https://github.com/angelesmarin/Placenet-App-Frontend.git`

## Set-up Environment

- Obtain the firebaseConfig.js, create a folder called `config` in the `Placenet-App-Frontend` folder, then place the firebaseConfig.js in that folder.

## Run 

- CD back to the docker directory and run:
    - `docker-compose up`
          

## Testing and Troubleshooting

- Test the app by going to `exp://youriphere:8081` on your mobile device, this should redirect you to Expo Go. Ensure your mobile device is connected to the same network as the device the project is on.
- Use the Sign Up feature to create a new user of your own specifications.
    - If you run into issues or can't sign up/in with the app, check the following:
        - Ensure the firebaseConfig.js is correct.
             - You can manually change this within the docker container without needing to do a full rebuild by clicking on the placenet-app container, going to the "Files" tab, and navigating to `usr/src/app/placenet-app/config/firebaseConfig.js`.
             - Right-click firebaseConfig.js, and select "Edit file". Change what you need to, and press "ctrl/cmd + S" to save. Then restart the container.
        - If all the above is correct, cd back to the docker directory and run `docker-compose build --no-cache` and then `docker-compose up` to rebuild the container.

- Then test adding a property and check the database on the firebase console.
