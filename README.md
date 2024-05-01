
# Todo-List WebApp :

This is a daily to-do list application built with Vue.js on the frontend and Node.js, Express.js on the backend, using MongoDB for data storage. It allows users to manage their daily tasks using CRUD (Create, Read, Update, Delete) operations.

- **Create**: Users can add new tasks using a form. This typically includes entering a description for the task.
- **Read**: The app displays a list of all the created tasks. This list can be updated dynamically as new tasks are added or existing ones are modified.
- **Update**: Users can edit the details of existing tasks. This might involve changing the description or marking a task as completed.
- **Delete**: Users can remove unwanted tasks from the list.


## .env files

**MONGO_URI**

**SECRETKEY**

**ORIGIN**

## Documentation

To setup Todo-List App follow the steps :

    git clone https://github.com/SrijanVerma04/Task-Todo-Frontend.git

Change the current directory

    pwd
    cd todo-list

Install the node modules

    npm install

Compiles and hot-reloads for development

    npm run serve


Application will be live at [http://localhost:8080/](http://localhost:8080/)

To setup the backend follow this repository : [Backend-API](https://github.com/SrijanVerma04/Task-todo-backend)





## Author

[Srijanverma04](https://github.com/SrijanVerma04)
## Todo-List API Endpoints

Signup: POST /auth/signup\
Login: POST /auth/login\
Logout: GET /auth/logout\
Token Refresh: GET /auth/refresh

#### Lists
Get All Lists: GET /lists/:userId\
Create List: POST /lists\
Update List: PUT /lists/:listId\
Delete List: DELETE /lists/:listId

#### Tasks
Add Task: POST /tasks\
Delete Task: DELETE /tasks/:listId/:index\
Toggle Task Completion: PUT /tasks/toggle
