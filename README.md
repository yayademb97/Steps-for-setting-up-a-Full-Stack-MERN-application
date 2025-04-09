MERN Project Setup

---

Here are the different steps to help you set up your MERN application.

- First, you should create a folder named after your project or assignment by opening your Git Bash terminal and entering the following commands::

  ```
  mkdir my-project-name or mkdir my-assignment-name
  ```

* Second, navigate to the newly created folder using the Git Bash command.

  ```
  cd my-project-name or cd my-assignment-name
  ```
* Third, split your terminal into two sections: one for the backend (server-side) and the other for the frontend (client-side).

### A) Server side (Backend): Specifically, on the terminal allocated for the backend (server-side)

1. **Create the Server Folder:**

* Create a new project directory:

  ```
  mkdir server
  ```

2. **Navigate to the `server` folder:**

   ```
   cd server
   ```
3. **Initialize a Node.js Project:**

   Initialize your Node.js project by running the following command:

   ```
   npm init -y
   ```

   After running this command, a `package.json` file will be automatically generated.
   Don't forget to add `"type": "module" ` for enabling the use ` import` and ` export` of and  in your `package.json` file.
4. **Install Required Dependencies**:

   Install the necessary packages for the backend, including Express, Mongoose, CORS, and dotenv:

```
npm install express mongoose cors dotenv
```

   After running this command, a `package-lock.json`  file and a `node_modules`  folder will be automatically generated.
   Don't forget to add `"type": "module" `, add this for abling the nodemon to run your server `dev`: `npx nodemon` for enabling the use ` import` and ` export` of and  in your `package.json` file.
   
   //* Correct file for `package.json` 
   ```
     {
       "name": "server",
       "version": "1.0.0",
       "description": "",
       "main": "server.js",
       "scripts": {
         "test": "echo \"Error: no test specified\" && exit 1",
         "start": "node server.js",
         "dev": "npx nodemon"
      },
       "keywords": [],
       "author": "",
       "license": "ISC",
       "type": "module",
      "dependencies": {
        "cors": "^2.8.5",
        "dotenv": "^16.4.7",
        "express": "^4.21.2",
        "mongoose": "^8.13.0",
        "nodemon": "^3.1.9"
     }
   }

   ```

5. **Create A Modularization Structure:**
   Create the following folders inside the server folder manually or Git Bash commands :

   ```
   mkdir config controllers models routes
   ```
6. **Create The followings files:**

   Create the files  and with Git Bash commands:

   ```
   1. touch .env 
   2. touch .gitignore
   3. touch server.js
   ```
7. **Set Up for** `.env` **file**:

   ```
   * Define the PORT for the server:
   PORT = 8000
   * PASTE THE MONGODB CONNECTION STRING LINK HERE
   MONGO_URI = <your-mongodb-connection-string>  //* Paste here your mongo connection string from YOUR MONGO DB ATLAS ACCOUNT
   * Specify The DB NAME
   DB = database_name

   ```
8. **Set Up for `.gitignore` file:**

   ```
   node_modules
   .gitignore
   ```
9. **DB Connect with your server application:**
   Inside your `config` folder, create a new file called  `mongoose.config.js` and paste the following code:

   ```
   //* Import the `connect` method from mongoose library to establish a connection with MongoDB
   import { connect } from 'mongoose';
   //* Import the dotenv package to load environment variables from the .env file
   import dotenv from 'dotenv';

   //* Load environment variables
   dotenv.config();

   //* Extract MongoDB URI and database name from environment variables
   const MONGO_URI = process.env.MONGO_URI; // MongoDB URI
   const DB = process.env.DB; // Database name

   /**
    * Establish a connection to MongoDB database
    * This function connects to the the datyabase specified by the MONGO_URI and DB environment variable
    * 
    **/ 

   async function dbConnect() {
       try {
           await connect(MONGO_URI, {
               dbName: DB,
           });
           console.log(`Pinged your deployment. You successfully connected to MongoDB! and your DB is : ${DB}`);
       } catch (error) {
           console.log(error);
           throw error;
       }
   }
   export default dbConnect;
   ```

   10. Set Up for `models` folder:
       Create a new file in your `models` folder, naming the file according to your context, project, or assignment.

```
Example: product.model.js, car.model.js, user.model.js and task.model.js, etc...
```

Here is an example of models:

```
//* task.model.js:

import {model, Schema} from 'mongoose';

//* Define the Schema for a Task
const TaskSchema = new Schema(
    {
        //* Task Title
        title: {
            type: String, // * Data type for the task title
            //* Validation for task title
            required: [true, "{PATH} of Task is required"],
            minLength: [3, "{PATH} of Task must be at least 3 characters long!"],
            maxLength: [120, "{PATH} of Task must be at most 120 characters long!"],
        },
        //* Task Description
        description: {
            type: String, // * Data type for the task description
            //* Validation for task description
            required: [true, "{PATH} of Task is required"],
            minLength: [3, "{PATH} of Task must be at least 3 characters long!"],
            maxLength: [500, "{PATH} of Task must be at most 500 characters long!"],
        },
        //* Task Status
        status: {
            type: String, // * Data type for the task status
            enum: ['Not Started', 'In Progress', 'Completed'], // * Valid values for task status
            required: [true, "{PATH} of Task is required"],
            default: 'Not Started', // * Default value for task status
        },
        //* Due Date
        dueDate: {
            type: Date, // * Data type for the task due date
            //* Validation for task due date
            required: [true, "{PATH} of Task is required"],
            min: [Date.now(), "{PATH} must be a future date"],
        },
    },
    {
        timestamps: true, //* Automatically add createdAt and updatedAt fields
    }
)

//* Create and export the Task model using the defined schema
const Task = model('Task', TaskSchema);

//* Export the Task model
export default Task;
```

11. Set Up for `controllers` folder:
    Inside your controllers folder, create a new file , naming the file according to your context, project, or assignment.

    ```
    Example: product.controller.js, user.controller.js, car.controller.js, task.controller.js, etc...
    ```

**Note: It is important to remember that in your controller, you should define the required functionalities for your application, such as the essential CRUD operations (Create, Read One, Read All, Update, and Delete).**
Here is an example of controller:

```
//* Import the Task model
import Task from "../models/task.model.js";

//* CRUD
/** 
* Create a new task in the database
* @param {Object} req - The HTTP request object, containing task data in req.body.
* @param {Object} res - The HTTP response object, used to send a response to the client.
**/

```
    //* Import the Task model
    import Task from "../models/task.model.js";

    const taskController = {
    //? CRUD Features
    //! 1. Create a Task feature
    createTask: async (req, res) => {
        try {
            const newTask = await Task.create(req.body);

            //* Update User for updating a Task
            await User.findByIdAndUpdate(newTask.assignedTo, {
                $push: { tasks: newTask._id }
            });

            res.status(201).json({
                success: true,
                message: "✅ Task created successfully ✅",
                data: newTask
            });
        } catch (error) {
            console.log(error);
            res.status(400).json({
                success: false,
                message: "❌ Task creation failed ❌",
                error: error,
            });
        }
    },

    //! 2. Read All Tasks feature:
    getAllTasks: async (req, res) => {
        try {
            const allTasks = await Task.find().populate("assignedTo", "firstName");
            res.status(200).json({
                success: true,
                message: "✅ All tasks retrieved successfully ✅",
                allTaskData: allTasks
            });
        } catch (error) {
            console.log(error);
            res.status(400).json({
                success: false,
                message: "❌ All Tasks not found ❌",
                error: error,
            });
        }
    },

    //! 3. Read One Task feature
    getOneTask: async (req, res) => {
        try {
            const foundTask = await Task.findById(req.params.id);
            res.status(200).json({
                success: true,
                message: "✅ Task found successfully ✅",
                data: foundTask
            });
        } catch (error) {
            console.log(error);
            res.status(400).json({
                success: false,
                message: "❌ Task not found ❌",
                error: error,
            });
        }
    },

    //! 4. Update a Task feature
    updateOneTask: async (req, res) => {
        const options = {
            new: true, //* Return the updated document
            runValidators: true //* Ensure the update follows schema rules
        };
        try {
            const updatedTask = await Task.findByIdAndUpdate(req.params.id, req.body, options);
            if (!updatedTask) {
                return res.status(404).json({
                    success: false,
                    message: "❌ Task not found ❌"
                });
            }

            res.status(200).json({
                success: true,
                message: "✅ Task updated successfully ✅",
                data: updatedTask
            });
        } catch (error) {
            console.log(error);
            res.status(400).json({
                success: false,
                message: "❌ Task update failed ❌",
                error: error,
            });
        }
    },

    //* 5. Delete a Task feature
    deleteOneTask: async (req, res) => {
        try {
            const deletedTask = await Task.findByIdAndDelete(req.params.id);
            if (!deletedTask) {
                return res.status(404).json({
                    success: false,
                    message: "❌ Task not found ❌"
                });
            }
            res.status(200).json({
                success: true,
                message: "✅ Task deleted successfully ✅",
                data: deletedTask
            });
        } catch (error) {
            console.log(error);
            res.status(400).json({
                success: false,
                message: "❌ Task deletion failed ❌",
               error: error
            });
        }
    }
};

export default taskController;

```
    //* task.routes.js:

    //* Import the Router module from Express to create modular and mountable route handlers
    import { Router } from 'express';

    //* Import all features from the task controller to the routes
    import taskController from "../controllers/task.controller.js";

    //* Create a new instance from the Router class called `router`
    const router = Router();
    //? CRUD Features Routes
    //! 1. Create a Task feature Route
    router.route("/tasks/create")
            .post(taskController.createTask)
      //! 2. Read All Tasks feature Route
    //* a) Read All Tasks Route
    router.route("/tasks")
        .get(taskController.getAllTasks)

    //! 3. Read One Task feature Route
    router.route("/tasks/:id/details")
        .get(taskController.getOneTask)

     //! 4. Update One Task feature Route
     router.route("/tasks/:id/edit")
           .put(taskController.updateOneTask)

    //! 5. Delte One Task feature Route
    router.route("/tasks/:id/destroy")
        .delete(taskController.deleteOneTask)

    //* Export the router in the main file for the server application called 'server.js'
    export default router;

    //* Explanation:
    /tasks/create: Route for creating a new task.
    /tasks: Route for getting all tasks.
    /tasks/:id/details: Route for getting a specific task by ID.
    /tasks/:id/edit: Route for updating a specific task by ID.
    /tasks/:id/destroy: Route for deleting a specific task by ID.
    ```

    14. Set Up for `server.js` file:
        The `server.js` file sets up the Express server, connects to MongoDB, configures middleware, defines routes, and starts the server.

```
//* server.js
```
//* Import necessary modules
import express from 'express';
import cors from 'cors';
import dotenv from 'dotenv';


//* Import the router from the routes folder
import router from './routes/task.routes.js';
import dbConnect from './config/mongoose.config.js';


//* Create an instance of express application
const app = express();


//* Configure the middlewares
app.use(express.json(), cors());


//* Load the server port from environment variables
dotenv.config();


//* Retrives the server port from .env
const PORT = process.env.PORT;


//* Connect to the MONGODB database
dbConnect();

//* Add main routes for app running
app.use("/api", router);

//* Start the server
app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`); // Log server URL to the console
  });
  
```

15. **Running the server:**
    If you already installed `nodemon` globally on your desktop, youu can run directly with the Git Bash Command:
    ```
    nodemon server.js
    ```

**Summary: This is what the back-end structure of your MERN project will look like once you have followed the entire process correctly.**

```
my-project-name or my-assignment-name/
├── server/
│   ├── config/
│   │ ├── mongoose.config.js/
│   ├── controllers/
│   │ ├── task.controller.js/
│   ├── models/
│   │ ├── task.model.js/
│   ├── node_modules/
│   ├── routes/
│   │ ├── task.routes.js/
│   ├── .env/
│   ├── .gitignore/
│   ├── package-lock.json/
│   ├── package.json/
│   └── server.js
└── 
```

### B) Client side (Frontend): Specifically, on the terminal allocated for the frontend (client-side)

1. **Install React Application With Vite Tool:**
   Launch the react app with this follliwing command:

   ```
   npm create vite@latest client
   ```

   a.Select a Framework `React` :

   ```
   > React
   ```

   b. Select a variant:

```
JavaScript + SWC
```

    c. Accessing`client` folder:

```
cd client
```

    2. Install all Dependencies for the Frontend:

```
npm install axios react-router-dom bootstrap
```

3. Copy the cdn for bootstrap in `main.jsx` file:

   ```
   import 'bootstrap/dist/css/bootstrap.css';
   ```

   4. Wrap the app in `BrowserRouter`:

```
//* main.jsx:

import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.jsx'
import 'bootstrap/dist/css/bootstrap.css';  //* Paste here the bootstrap 
import { BrowserRouter } from 'react-router-dom';  //* Paste here

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>
);

```

5. Put the following folders in the   directory to the instructions of the assignment, project, or wireframe:

   ```
   • components
   • views
   ```

   6. Set up routing in `App.jsx` regarding the routing of your application:

```
//* App.jsx:
//* Example:

import Header from './components/Header'
import { Routes, Route } from 'react-router-dom'
import Home from './Views/Home'
import Create from './Views/Create'
import DetailTask from './Views/DetailTask'
import Update from './Views/Update'

function App() {
    return (
        <Routes>
            <Route path={"/api/tasks"} element={<Home />} />
            <Route path={"/api/tasks/create"} element={<Create />} />
            <Route path={"/api/tasks/:id"} element={<DetailTask />} />
            <Route path={"/api/tasks/edit/:id"} element={<Update />} />
        </Routes>
    );
}

export default App;
```

    Frontend Folder Structure:

```
my-project-name or my-assignment-name/
├── client/
│   ├── src/
│   │   ├── components/
│   │   ├── App.css
│   │   ├── Views/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── public/
│   ├── .gitignore
│   ├── eslintconfig.js
│   ├── package-lock.json
│   ├── package.json
│   ├── README.md
│   └── vite.config.js

```

    Combined Project Structure:

```
my-project-name or my-assignment-name/
├── server/
│   ├── config/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── .env
│   ├── .gitignore
│   ├── package.json
│   └── server.js
├── client/
│   ├── src/
│   │   ├── components/
│   │   ├── App.css
│   │   ├── Views/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── public/
│   ├── .gitignore
│   ├── eslintconfig.js
│   ├── package-lock.json
│   ├── package.json
│   ├── README.md
│   └── vite.config.js

```
