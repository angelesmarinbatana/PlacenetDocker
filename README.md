# Replicating Placenet App via Docker

## Pre-requisites

- Download Docker for Desktop (v4.35.1 tested)
    - https://www.docker.com/products/docker-desktop/ 
    - Run the executable and open the app when done
    - Also install the [CORS browser extension for Chrome](https://chromewebstore.google.com/detail/cross-domain-cors/mjhpgnbimicffchbodmgfnemoghjakai) (bandaid for now).
 
## Clone repos

- Clone this repo via command line:
    - `git clone https://github.com/angelesmarin/PlacenetDocker.git`

- CD into the PlacenetDocker folder and clone the frontend and backend repos.
    - `git clone https://github.com/angelesmarin/Placenet-App-Frontend.git`
    - `git clone https://github.com/angelesmarin/Placenet-App-Backend.git`

## Set-up Environment

- Replace "localhost" in api.js in the frontend with your own IP.
    - Find this in your machine's network settings under your connected network.
    - [Link to file in the frontend repo]([Placenet-App-Frontend\API\api.js](https://github.com/angelesmarin/Placenet-App-Frontend/blob/development/API/api.js)).

- Create a file called ".env" in the backend directory with the following in it:
    - `DB_NAME=placenet`
    - `DB_USER=admin`
    - `DB_PASSWORD=root`
    - `DB_HOST=postgres`
    - `DB_PORT=5432`
    - `PORT=3000`

## Run 

- CD back to the docker directory and run:
    - `docker-compose up`

## Database Config

- When finished, navigate to http://localhost:5050 and sign in with `admin@admin.com` as username and `root` as password.

- Click "Add a new server" and name it "placenet".
    - Under the "Connection" tab, enter "postgres" as Host name/address.
    - Keep port as 5432.
    - Enter username as "admin".
    - Enter password as "root".
    - Click "save".

- Now navigate to Servers > placenet > Databases > placenet > Scehmas > public > Tables.
    - You should see four tables: users, properties, projects, and documents.
        - If not, right-click "Tables", select "Query Tool", paste the following SQL:
            - `CREATE TABLE users ( user_id SERIAL PRIMARY KEY, username VARCHAR(50) UNIQUE NOT NULL, password_hash VARCHAR(255) NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP );`
            - `CREATE TABLE properties ( property_id SERIAL PRIMARY KEY, user_id INT REFERENCES users(user_id) ON DELETE CASCADE, name VARCHAR(255) NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP );`
            - `CREATE TABLE projects ( project_id SERIAL PRIMARY KEY, property_id INT REFERENCES properties(property_id) ON DELETE CASCADE, user_id INT REFERENCES users(user_id) ON DELETE CASCADE, name VARCHAR(255) NOT NULL, description TEXT, start_date DATE, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP );`
            - `CREATE TABLE documents ( document_id SERIAL PRIMARY KEY, project_id INT REFERENCES projects(project_id) ON DELETE CASCADE, user_id INT REFERENCES users(user_id) ON DELETE CASCADE, file_name VARCHAR(255) NOT NULL, file_path VARCHAR(255) NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP );`
        - Then run the script or press f5.

- Right-click the "users" table and select "Query Tool".
    - Enter the following SQL and execute the script by clicking the play button or pressing "f5":
        - `INSERT INTO users(username,password_hash) VALUES ('lala','lala');`

## Testing and Troubleshooting

- Test the app by going to http://localhost:8081 and logging in with "lala" in both username and password.
    - If you run into issues or can't sign in with the app, check the following:
        - Ensure you have the CORS browser extension enabled.
        - Ensure the IP in api.js in the frontend is your own IP and is spelled correctly.
             - You can manually change this within the docker container without needing to do a full rebuild by clicking on the placenet-app container, going to the "Files" tab, and navigating to `usr/src/app/placenet-app/API/api.js`.
             - Right-click api.js, and select "Edit file". Change what you need to, and press "ctrl/cmd + S" to save. Then restart the container.
        - Double check any spelling in the backend .env file.
              - You can similarly edit the .env within docker without a full rebuild like above with the api script.
        - Restart the backend container in Docker, or the full container for extra measure.
        - If all the above is correct, cd back to the docker directory and run `docker-compose build --no-cache` and then `docker-compose up` to rebuild the containers.

- Then test adding a property and check the database in http://localhost:5050 to see if it has been added by right-clicking the "properties" table, clicking "view rows > all rows".
